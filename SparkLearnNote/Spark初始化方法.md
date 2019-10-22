# Spark初始化方法

1. 方法一
```
val conf = new SparkConf().setAppName(s"ldaModelMysql").setMaster("local[*]").set("spark.executor.memory", "2g")
val spark = SparkSession.builder().config(conf).getOrCreate()
val sc = spark.sparkContext
 ```   

2. 方法二
```
//bulid environment
val spark = SparkSession.builder.appName("CheckTrainData").master("local[*]").getOrCreate()
val sc = spark.sparkContext
import spark.implicits._
 ```

不打印日志方法
```
  // do not print log
  def SetLogger = {
    Logger.getLogger("org").setLevel(Level.OFF)
    Logger.getLogger("com").setLevel(Level.OFF)
    System.setProperty("spark.ui.showConsoleProgress", "false")
    Logger.getRootLogger().setLevel(Level.OFF)
  }
  ```