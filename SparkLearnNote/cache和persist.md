# cache和persist

> 参考链接：
>
> 每次进步一点点——spark中cache和persist的区别 <http://blog.csdn.net/houmou/article/details/52491419>
>
> Spark性能优化指南——基础篇 <https://tech.meituan.com/spark-tuning-basic.html>
>
>
>


## 1. Spark的持久化级别

- MEMORY_ONLY	

使用未序列化的Java对象格式，将数据保存在内存中。如果内存不够存放所有的数据，则数据可能就不会进行持久化。那么下次对这个RDD执行算子操作时，
那些没有被持久化的数据，需要从源头处重新计算一遍。这是默认的持久化策略，使用cache()方法时，实际就是使用的这种持久化策略。

- MEMORY_AND_DISK	

使用未序列化的Java对象格式，优先尝试将数据保存在内存中。如果内存不够存放所有的数据，会将数据写入磁盘文件中，下次对这个RDD执行算子时，
持久化在磁盘文件中的数据会被读取出来使用。

- MEMORY_ONLY_SER	

基本含义同MEMORY_ONLY。唯一的区别是，会将RDD中的数据进行序列化，RDD的每个partition会被序列化成一个字节数组。
这种方式更加节省内存，从而可以避免持久化的数据占用过多内存导致频繁GC。

- MEMORY_AND_DISK_SER	

基本含义同MEMORY_AND_DISK。唯一的区别是，会将RDD中的数据进行序列化，RDD的每个partition会被序列化成一个字节数组。这种方式更加节省内存，
从而可以避免持久化的数据占用过多内存导致频繁GC。

- DISK_ONLY	

使用未序列化的Java对象格式，将数据全部写入磁盘文件中。

- MEMORY_ONLY_2, MEMORY_AND_DISK_2, 等等.	

对于上述任意一种持久化策略，如果加上后缀_2，代表的是将每个持久化的数据，都复制一份副本，并将副本保存到其他节点上。
这种基于副本的持久化机制主要用于进行容错。假如某个节点挂掉，节点的内存或磁盘中的持久化数据丢失了，
那么后续对RDD计算时还可以使用该数据在其他节点上的副本。如果没有副本的话，就只能将这些数据从源头处重新计算一遍了。

## 2. 如何选择一种最合适的持久化策略

* 默认情况下，性能最高的当然是MEMORY_ONLY，但前提是你的内存必须足够足够大，可以绰绰有余地存放下整个RDD的所有数据。
因为不进行序列化与反序列化操作，就避免了这部分的性能开销；对这个RDD的后续算子操作，都是基于纯内存中的数据的操作，不需要从磁盘文件中读取数据，
性能也很高；而且不需要复制一份数据副本，并远程传送到其他节点上。但是这里必须要注意的是，在实际的生产环境中，
恐怕能够直接用这种策略的场景还是有限的，如果RDD中数据比较多时（比如几十亿），直接用这种持久化级别，会导致JVM的OOM内存溢出异常。

* 如果使用MEMORY_ONLY级别时发生了内存溢出，那么建议尝试使用MEMORY_ONLY_SER级别。该级别会将RDD数据序列化后再保存在内存中，
此时每个partition仅仅是一个字节数组而已，大大减少了对象数量，并降低了内存占用。这种级别比MEMORY_ONLY多出来的性能开销，
主要就是序列化与反序列化的开销。但是后续算子可以基于纯内存进行操作，因此性能总体还是比较高的。此外，可能发生的问题同上，
如果RDD中的数据量过多的话，还是可能会导致OOM内存溢出的异常。

* 如果纯内存的级别都无法使用，那么建议使用MEMORY_AND_DISK_SER策略，而不是MEMORY_AND_DISK策略。
因为既然到了这一步，就说明RDD的数据量很大，内存无法完全放下。序列化后的数据比较少，可以节省内存和磁盘的空间开销。
同时该策略会优先尽量尝试将数据缓存在内存中，内存缓存不下才会写入磁盘。

* 通常不建议使用DISK_ONLY和后缀为_2的级别：因为完全基于磁盘文件进行数据的读写，会导致性能急剧降低，有时还不如重新计算一次所有RDD。
后缀为_2的级别，必须将所有数据都复制一份副本，并发送到其他节点上，数据复制以及网络传输会导致较大的性能开销，
除非是要求作业的高可用性，否则不建议使用。

## 3. 源码剖析


### StorageLevel 类的源码

    object StorageLevel {
      val NONE = new StorageLevel(false, false, false, false)
      val DISK_ONLY = new StorageLevel(true, false, false, false)
      val DISK_ONLY_2 = new StorageLevel(true, false, false, false, 2)
      val MEMORY_ONLY = new StorageLevel(false, true, false, true)
      val MEMORY_ONLY_2 = new StorageLevel(false, true, false, true, 2)
      val MEMORY_ONLY_SER = new StorageLevel(false, true, false, false)
      val MEMORY_ONLY_SER_2 = new StorageLevel(false, true, false, false, 2)
      val MEMORY_AND_DISK = new StorageLevel(true, true, false, true)
      val MEMORY_AND_DISK_2 = new StorageLevel(true, true, false, true, 2)
      val MEMORY_AND_DISK_SER = new StorageLevel(true, true, false, false)
      val MEMORY_AND_DISK_SER_2 = new StorageLevel(true, true, false, false, 2)
      val OFF_HEAP = new StorageLevel(false, false, true, false)
      ......
    }
    
这里列出了12种缓存级别，每个缓存级别后面都跟了一个StorageLevel的构造函数，构造函数里面包含了4个或5个参数，如下：


    val MEMORY_ONLY = new StorageLevel(false, true, false, true)

查看其构造函数

    class StorageLevel private(
        private var _useDisk: Boolean,
        private var _useMemory: Boolean,
        private var _useOffHeap: Boolean,
        private var _deserialized: Boolean,
        private var _replication: Int = 1)
      extends Externalizable {
      ......
      def useDisk: Boolean = _useDisk
      def useMemory: Boolean = _useMemory
      def useOffHeap: Boolean = _useOffHeap
      def deserialized: Boolean = _deserialized
      def replication: Int = _replication
      ......
    } 
    
可以看到StorageLevel类的主构造器包含了5个参数：

- useDisk：使用硬盘（外存）

- useMemory：使用内存

- useOffHeap：使用堆外内存，这是Java虚拟机里面的概念，堆外内存意味着把内存对象分配在Java虚拟机的堆以外的内存，这些内存直接受操作系统管理（而不是虚拟机）。这样做的结果就是能保持一个较小的堆，以减少垃圾收集对应用的影响。

- deserialized：反序列化，其逆过程序列化（Serialization）是java提供的一种机制，将对象表示成一连串的字节；而反序列化就表示将字节恢复为对象的过程。序列化是对象永久化的一种机制，可以将对象及其属性保存起来，并能在反序列化后直接恢复这个对象

- replication：备份数（在多个节点上备份）

理解了这5个参数，StorageLevel 的12种缓存级别就不难理解了。

`val MEMORY_AND_DISK_SER_2 = new StorageLevel(true, true, false, false, 2)`
就表示使用这种缓存级别的RDD将存储在硬盘以及内存中，使用序列化（在硬盘中），并且在多个节点上备份2份（正常的RDD只有一份）

另外还注意到有一种特殊的缓存级别
    
    val OFF_HEAP = new StorageLevel(false, false, true, false)
  
使用了堆外内存，StorageLevel 类的源码中有一段代码可以看出这个的特殊性，它不能和其它几个参数共存。

    if (useOffHeap) {
      require(!useDisk, "Off-heap storage level does not support using disk")
      require(!useMemory, "Off-heap storage level does not support using heap memory")
      require(!deserialized, "Off-heap storage level does not support deserialized storage")
      require(replication == 1, "Off-heap storage level does not support multiple replication")
    }