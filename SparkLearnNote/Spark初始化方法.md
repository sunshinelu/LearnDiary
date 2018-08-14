# Spark初始化方法

1. 方法一

    val conf = new SparkConf().setAppName(s"ldaModelMysql").setMaster("local[*]").set("spark.executor.memory", "2g")
    val spark = SparkSession.builder().config(conf).getOrCreate()
    val sc = spark.sparkContext
    

2. 方法二

    //bulid environment
    val spark = SparkSession.builder.appName("CheckTrainData").master("local[*]").getOrCreate()
    val sc = spark.sparkContext
    import spark.implicits._