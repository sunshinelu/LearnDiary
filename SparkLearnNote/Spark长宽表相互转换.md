# Spark长宽表相互转换

> 参考链接
> 
> Reshaping Data with Pivot in Apache Spark <https://databricks.com/blog/2016/02/09/reshaping-data-with-pivot-in-apache-spark.html>
>
> Transpose Dataframe using Spark Scala without using Pivot function <https://stackoverflow.com/questions/36215755/transpose-dataframe-using-spark-scala-without-using-pivot-function>
>
> Transpose column to row with Spark <https://stackoverflow.com/questions/37864222/transpose-column-to-row-with-spark>
>
> Pivoting Data in SparkSQL <https://svds.com/pivoting-data-in-sparksql/>
>
> Reshaping/Pivoting data in Spark RDD and/or Spark DataFrames <https://stackoverflow.com/questions/30260015/reshaping-pivoting-data-in-spark-rdd-and-or-spark-dataframes>

## 1. 宽表转成长表

将宽表转换成长表，类似R语言中reshape2包中melt函数

    
    import org.apache.log4j.{Level, Logger}
    import org.apache.spark.SparkConf
    import org.apache.spark.sql._
    import org.apache.spark.sql.functions._
    
    /**
     * Created by sunlu on 17/11/3.
     * 将长宽表转换成长表，类似R语言中reshape2包中melt函数
     * 参考链接：
     * Transpose column to row with Spark：https://stackoverflow.com/questions/37864222/transpose-column-to-row-with-spark
     */
    object meltDemo {
      def SetLogger = {
        Logger.getLogger("org").setLevel(Level.OFF)
        Logger.getLogger("com").setLevel(Level.OFF)
        System.setProperty("spark.ui.showConsoleProgress", "false")
        Logger.getRootLogger().setLevel(Level.OFF)
      }
    
      def main(args: Array[String]) {
        SetLogger
    
        val sparkConf = new SparkConf().setAppName(s"meltDemo").setMaster("local[*]").set("spark.executor.memory", "2g")
        val spark = SparkSession.builder().config(sparkConf).getOrCreate()
        val sc = spark.sparkContext
        import spark.implicits._
    
        val df = Seq((1, 0.0, 0.6,0.8,0.1), (1, 0.6, 0.7,0.6,0.4),(2, 0.1, 0.2,0.3,0.4), (3, 0.5, 0.2,0.6,0.4)).
          toDF("A", "col_1", "col_2","col_3","col_4")
        df.show(false)
        /*
    +---+-----+-----+-----+-----+
    |A  |col_1|col_2|col_3|col_4|
    +---+-----+-----+-----+-----+
    |1  |0.0  |0.6  |0.8  |0.1  |
    |1  |0.6  |0.7  |0.6  |0.4  |
    |2  |0.1  |0.2  |0.3  |0.4  |
    |3  |0.5  |0.2  |0.6  |0.4  |
    +---+-----+-----+-----+-----+
         */
    
    
        def meltFunc(df: DataFrame, by: Seq[String]): DataFrame = {
          val (cols, types) = df.dtypes.filter{ case (c, _) => !by.contains(c)}.unzip
          require(types.distinct.size == 1)
    
          val kvs = explode(array(
            cols.map(c => struct(lit(c).alias("key"), col(c).alias("val"))): _*
          ))
    
          val byExprs = by.map(col(_))
    
          df.select(byExprs :+ kvs.alias("_kvs"): _*)
            .select(byExprs ++ Seq($"_kvs.key", $"_kvs.val"): _*)
        }
    
        val df1 = meltFunc(df, Seq("A"))
        df1.show(false)
    /*
    +---+-----+---+
    |A  |key  |val|
    +---+-----+---+
    |1  |col_1|0.0|
    |1  |col_2|0.6|
    |1  |col_3|0.8|
    |1  |col_4|0.1|
    |1  |col_1|0.6|
    |1  |col_2|0.7|
    |1  |col_3|0.6|
    |1  |col_4|0.4|
    |2  |col_1|0.1|
    |2  |col_2|0.2|
    |2  |col_3|0.3|
    |2  |col_4|0.4|
    |3  |col_1|0.5|
    |3  |col_2|0.2|
    |3  |col_3|0.6|
    |3  |col_4|0.4|
    +---+-----+---+
     */
        sc.stop()
        spark.stop()
      }
    
    }
    
## 2.长表转成宽表

将长表转换成宽表，类似R语言中reshape2包中cast函数

    
    import org.apache.log4j.{Level, Logger}
    import org.apache.spark.SparkConf
    import org.apache.spark.sql.SparkSession
    
    /**
     * Created by sunlu on 17/11/3.
     * 将长表转换成宽表，类似R语言中reshape2包中cast函数
     * 参考链接：
     * http://kirillpavlov.com/blog/2016/02/21/top-5-features-released-in-spark-1.6/
     * https://databricks.com/blog/2016/02/09/reshaping-data-with-pivot-in-apache-spark.html
     * Pivoting Data in SparkSQL: https://svds.com/pivoting-data-in-sparksql/
     */
    object castDemo {
    
      def SetLogger = {
        Logger.getLogger("org").setLevel(Level.OFF)
        Logger.getLogger("com").setLevel(Level.OFF)
        System.setProperty("spark.ui.showConsoleProgress", "false")
        Logger.getRootLogger().setLevel(Level.OFF)
      }
    
      def main(args: Array[String]) {
    
        SetLogger
    
        val sparkConf = new SparkConf().setAppName(s"castDemo").setMaster("local[*]").set("spark.executor.memory", "2g")
        val spark = SparkSession.builder().config(sparkConf).getOrCreate()
        val sc = spark.sparkContext
        import spark.implicits._
    
        val df = sc.parallelize(Array(
          ("one", "A", 1), ("one", "B", 2), ("one", "C", 4),("two", "A", 5), ("two", "B", 6), ("two", "C", 7)
        )).toDF("key1", "key2", "value")
    
        df.show(false)
    /*
    +----+----+-----+
    |key1|key2|value|
    +----+----+-----+
    |one |A   |1    |
    |one |B   |2    |
    |one |C   |4    |
    |two |A   |5    |
    |two |B   |6    |
    |two |C   |7    |
    +----+----+-----+
     */
        df.groupBy("key1").pivot("key2").sum("value").show(false)
    /*
    +----+---+---+---+
    |key1|A  |B  |C  |
    +----+---+---+---+
    |two |5  |6  |7  |
    |one |1  |2  |4  |
    +----+---+---+---+
     */
    
        val groupedData = df.groupBy("key1")
    
        groupedData.pivot("key2").sum("value").show(false)
    /*
    +----+---+---+---+
    |key1|A  |B  |C  |
    +----+---+---+---+
    |two |5  |6  |7  |
    |one |1  |2  |4  |
    +----+---+---+---+
     */
      }
    }
