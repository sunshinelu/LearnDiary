# Spark Dataframe数据标准化

> 参考链接：
> <https://stackoverflow.com/questions/33924842/minmax-normalization-in-scala>
>
>

对data frame中的某一列数据进行标准化处理

代码如下：


    
    import org.apache.log4j.{Level, Logger}
    import org.apache.spark.SparkConf
    import org.apache.spark.ml.feature.MinMaxScaler
    import org.apache.spark.ml.linalg.{DenseVector, Vectors}
    import org.apache.spark.sql.functions._
    import org.apache.spark.sql.{Row, SparkSession}
    
    /*
    均一化参考链接：https://stackoverflow.com/questions/33924842/minmax-normalization-in-scala
    appRecomV1
     */
    object NormalizedDemo1 {
      def SetLogger = {
        Logger.getLogger("org").setLevel(Level.OFF)
        Logger.getLogger("com").setLevel(Level.OFF)
        System.setProperty("spark.ui.showConsoleProgress", "false")
        Logger.getRootLogger().setLevel(Level.OFF)
      }
    
      def main(args: Array[String]): Unit = {
        SetLogger
    
        val SparkConf = new SparkConf().setAppName(s"NormalizedDemo1").setMaster("local[*]").set("spark.executor.memory", "2g")
        val spark = SparkSession.builder().config(SparkConf).getOrCreate()
        val sc = spark.sparkContext
        import spark.implicits._
    
        val df = sc.parallelize(Seq(
          (1L, 0.5), (2L, 10.2), (3L, 5.7), (4L, -11.0), (5L, 22.3)
        )).toDF("k", "v")
    
        /*
        方法1：
         */
        val (vMin, vMax) = df.agg(min($"v"), max($"v")).first match {
          case Row(x: Double, y: Double) => (x, y)
        }
    
        val scaledRange = lit(2) // Range of the scaled variable
        val scaledMin = lit(-1) // Min value of the scaled variable
        val vNormalized = ($"v" - vMin) / (vMax - vMin) // v normalized to (0, 1) range
    
        val vScaled = scaledRange * vNormalized + scaledMin
    
        df.withColumn("vScaled", vScaled).show
        /*
        +---+-----+--------------------+
    |  k|    v|             vScaled|
    +---+-----+--------------------+
    |  1|  0.5| -0.3093093093093092|
    |  2| 10.2| 0.27327327327327344|
    |  3|  5.7|0.003003003003003...|
    |  4|-11.0|                -1.0|
    |  5| 22.3|                 1.0|
    +---+-----+--------------------+
         */
    
        /*
        方法2:
         */
        val vectorizeCol = udf((v: Double) => Vectors.dense(Array(v)))
        val df2 = df.withColumn("vVec", vectorizeCol(df("v")))
        val scaler = new MinMaxScaler()
          .setInputCol("vVec")
          .setOutputCol("vScaled")
          .setMax(1)
          .setMin(-1)
    
        val df3 = scaler.fit(df2).transform(df2)
        df3.show()
        /*
    +---+-----+-------+--------------------+
    |  k|    v|   vVec|             vScaled|
    +---+-----+-------+--------------------+
    |  1|  0.5|  [0.5]|[-0.3093093093093...|
    |  2| 10.2| [10.2]|[0.27327327327327...|
    |  3|  5.7|  [5.7]|[0.00300300300300...|
    |  4|-11.0|[-11.0]|              [-1.0]|
    |  5| 22.3| [22.3]|               [1.0]|
    +---+-----+-------+--------------------+
         */
    
        df3.printSchema()
        /*
        root
     |-- k: long (nullable = true)
     |-- v: double (nullable = true)
     |-- vVec: vector (nullable = true)
     |-- vScaled: vector (nullable = true)
         */
        /*
        将vScaled列的feature转换为double类型
         */
    
        df3.select("vScaled").map { case Row(v: DenseVector) => v(0).toString.toDouble }.show(false)
        /*
        +---------------------+
        |value                |
        +---------------------+
        |-0.3093093093093092  |
        |0.27327327327327344  |
        |0.0030030030030030463|
        |-1.0                 |
        |1.0                  |
        +---------------------+
         */
    
        val reversVectorizeCol = udf { (v: DenseVector) => v(0).toString.toDouble }
        val df4 = df3.withColumn("vScaled2", reversVectorizeCol(df3("vScaled")))
        df4.show()
        /*
        +---+-----+-------+--------------------+--------------------+
        |  k|    v|   vVec|             vScaled|            vScaled2|
        +---+-----+-------+--------------------+--------------------+
        |  1|  0.5|  [0.5]|[-0.3093093093093...| -0.3093093093093092|
        |  2| 10.2| [10.2]|[0.27327327327327...| 0.27327327327327344|
        |  3|  5.7|  [5.7]|[0.00300300300300...|0.003003003003003...|
        |  4|-11.0|[-11.0]|              [-1.0]|                -1.0|
        |  5| 22.3| [22.3]|               [1.0]|                 1.0|
        +---+-----+-------+--------------------+--------------------+
         */
    
        sc.stop()
        spark.stop()
    
      }
    }
    
