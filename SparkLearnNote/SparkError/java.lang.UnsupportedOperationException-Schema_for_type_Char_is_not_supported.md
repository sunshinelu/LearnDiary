# java.lang.UnsupportedOperationException: Schema for type Char is not supported

错误信息：

    Exception in thread "main" java.lang.UnsupportedOperationException: Schema for type Char is not supported
        at org.apache.spark.sql.catalyst.ScalaReflection$.schemaFor(ScalaReflection.scala:733)
        at org.apache.spark.sql.catalyst.ScalaReflection$.schemaFor(ScalaReflection.scala:671)
        at org.apache.spark.sql.functions$.udf(functions.scala:3072)
        at com.evayInfo.Inglory.TestCode.uuidGenerate$.main(uuidGenerate.scala:31)
        at com.evayInfo.Inglory.TestCode.uuidGenerate.main(uuidGenerate.scala)

代码为：
    
    package com.evayInfo.Inglory.TestCode
    
    import java.util.UUID
    
    import org.apache.spark.{SparkConf, SparkContext}
    import org.apache.spark.sql.SparkSession
    import org.apache.spark.sql.functions._
    /**
     * Created by sunlu on 17/7/27.
     * 使用rt.jar生成UUID
     */
    object uuidGenerate {
      def main(args: Array[String]) {
        val s1 = UUID.randomUUID().toString().toLowerCase()
        println(s1)
    
        val sparkConf = new SparkConf().setAppName("uuidGenerate").setMaster("local[*]")
        val spark = SparkSession.builder().config(sparkConf).getOrCreate()
        val sc = spark.sparkContext
        import spark.implicits._
        val rdd1 = sc.parallelize(Seq((1,"a"),(2,"b"),(3,"c")))
    
        val df1 = rdd1.toDF("col1", "col2")
        df1.show()
    
    
        val uuidUDF = udf((arg:String) => uuidFunc(arg))
        val df2 = df1.withColumn("col3",uuidUDF($"col1"))
        df2.show()
    
        val uuidUDF2 = udf(uuidFunc2())
        val df3 = df1.withColumn("col3",uuidUDF2())
        df3.show()
    
        sc.stop()
        spark.stop()
      }
      def uuidFunc(arg:String):String={
        val uuid = UUID.randomUUID().toString().toLowerCase()
        uuid
      }
    
      def uuidFunc2():String={
        val uuid = UUID.randomUUID().toString().toLowerCase().toString
        uuid
      }
    }


使用`uuidFunc`函数的时候有出输出结果，但是使用`uuidFunc2`函数的时候就报错`java.lang.UnsupportedOperationException: Schema for type Char is not supported`。

`uuidFunc`函数与`uuidFunc2`函数的区别在于，`uuidFunc`函数有变量输入，而`uuidFunc2`函数没有输入变量。


*发生这种错误的原因尚未查找，需要查看源码....*