# Scala List 和 Java List之间相互转换

> java集合和scala集合互转
>
> <https://www.cnblogs.com/lixiaolun/p/5542450.html>
> 


使用 scala.collection.JavaConverters 与Java集合交互。它有一系列的隐式转换，添加了asJava和asScala的转换方法。使用它们这些方法确保转换是显式的，有助于阅读：


    import scala.collection.JavaConverters._
     
    val list: java.util.List[Int] = Seq(1,2,3,4).asJava
    val buffer: scala.collection.mutable.Buffer[Int] = list.asScala