# spark dataframe中新增一列递增列

spark dataframe中新增一列递增列

> 参考资料
>
> spark dataframe :how to add a index Column：<https://stackoverflow.com/questions/43406887/spark-dataframe-how-to-add-a-index-column/43408058>
>
> How to select the first row of each group?:  <http://stackoverflow.com/questions/33878370/spark-dataframe-select-the-first-row-of-each-group>
>
> GENERATE UNIQUE IDS FOR EACH ROWS IN A SPARK DATAFRAME: <https://hadoopist.wordpress.com/2016/05/24/generate-unique-ids-for-each-rows-in-a-spark-dataframe/>

代码如下：

    
    import org.apache.log4j.{Level, Logger}
    import org.apache.spark.SparkConf
    import org.apache.spark.sql.SparkSession
    import org.apache.spark.sql.expressions.Window
    import org.apache.spark.sql.functions._
    
    /**
     * Created by sunlu on 17/11/3.
     * spark dataframe中新增一列递增列
     *
     * 参考链接
     * spark dataframe :how to add a index Column：https://stackoverflow.com/questions/43406887/spark-dataframe-how-to-add-a-index-column/43408058
     */
    object addIncreasedCol {
    
      def SetLogger = {
        Logger.getLogger("org").setLevel(Level.OFF)
        Logger.getLogger("com").setLevel(Level.OFF)
        System.setProperty("spark.ui.showConsoleProgress", "false")
        Logger.getRootLogger().setLevel(Level.OFF)
      }
    
      def main(args: Array[String]) {
        SetLogger
    
        val sparkConf = new SparkConf().setAppName(s"addIncreasedCol").setMaster("local[*]").set("spark.executor.memory", "2g")
        val spark = SparkSession.builder().config(sparkConf).getOrCreate()
        val sc = spark.sparkContext
        import spark.implicits._
    
        val df = sc.parallelize(Seq(("a","b","c"),("d","e","f"),("g","h","i"),
                                    ("a","b","d"),("a","i","c"),("g","y","r"))).toDF("col1","col2","col2")
        df.show(false)
        /*
    +----+----+----+
    |col1|col2|col2|
    +----+----+----+
    |a   |b   |c   |
    |d   |e   |f   |
    |g   |h   |i   |
    |a   |b   |d   |
    |a   |i   |c   |
    |g   |y   |r   |
    +----+----+----+
         */
    
    
    
        /*
        新增一列column index列
         */
        val w1 = Window.orderBy("col1")
        val df1 = df.withColumn("id", row_number().over(w1))
        df1.show(false)
    /*
    +----+----+----+---+
    |col1|col2|col2|id |
    +----+----+----+---+
    |a   |b   |c   |1  |
    |a   |b   |d   |2  |
    |a   |i   |c   |3  |
    |d   |e   |f   |4  |
    |g   |h   |i   |5  |
    |g   |y   |r   |6  |
    +----+----+----+---+
    
     */
    
        /*
        新增一列分组后排序列
         */
        val w2 = Window.partitionBy("col1").orderBy("col1")
        val df2 = df.withColumn("id", row_number().over(w2))
        df2.show(false)
        /*
    +----+----+----+---+
    |col1|col2|col2|id |
    +----+----+----+---+
    |g   |h   |i   |1  |
    |g   |y   |r   |2  |
    |d   |e   |f   |1  |
    |a   |b   |c   |1  |
    |a   |b   |d   |2  |
    |a   |i   |c   |3  |
    +----+----+----+---+
         */
    
        sc.stop()
        spark.stop()
      }
    }
