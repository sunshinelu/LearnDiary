# Spark Dataset转RDD中的坑

## 1、dataset转成RDD数据缺失
将dataframe转成RDD时，直接`df1.rdd.map`就可以转换，然后在将dataset转成RDD的时候发现`ds1.rdd.map`后的数量与ds1的数量不一致。

解决方案：

将dataset转成dataFram后再转成RD，例如：

     val conf = HBaseConfiguration.create() //在HBaseConfiguration设置可以将扫描限制到部分列，以及限制扫描的时间范围
    
        /*
        如果outputTable表存在，则删除表；如果不存在则新建表。
         */
        val hAdmin = new HBaseAdmin(conf)
        if (hAdmin.tableExists(outputTable)) {
          hAdmin.disableTable(outputTable)
          hAdmin.deleteTable(outputTable)
        }
        //    val htd = new HTableDescriptor(outputTable)
        val htd = new HTableDescriptor(TableName.valueOf(outputTable))
        htd.addFamily(new HColumnDescriptor("info".getBytes()))
        hAdmin.createTable(htd)
    
        //指定输出格式和输出表名
        conf.set(TableOutputFormat.OUTPUT_TABLE, outputTable) //设置输出表名
    
        val jobConf = new Configuration(conf)
        jobConf.set("mapreduce.job.outputformat.class", classOf[TableOutputFormat[Text]].getName)
    

        userR_5.toDF().rdd.map{
          case Row(user2:String,itemString:String,recomValue:Double, rn:Int,title:String,manuallabel:String,time:String,systime:String) =>
            (user2, itemString,recomValue,rn,title, manuallabel, time,systime)}.
          map(x => {
            val userString = x._1.toString
            val itemString = x._2.toString
            //保留rating有效数字
            val rating = x._3.toString.toDouble
            val rating2 = f"$rating%1.5f".toString
            val rn = x._4.toString
            val title = if (null != x._5) x._5.toString else ""
            val manuallabel = if (null != x._6) x._6.toString else ""
            val time = if (null != x._7) x._7.toString else ""
            val sysTime = if (null != x._8) x._8.toString else ""
            (userString, itemString, rating2, rn, title, manuallabel, time, sysTime)
          }).filter(_._5.length >= 2).
          map { x => {
            val paste = x._1 + "::score=" + x._4.toString
            val key = Bytes.toBytes(paste)
            val put = new Put(key)
            put.add(Bytes.toBytes("info"), Bytes.toBytes("userID"), Bytes.toBytes(x._1.toString)) //标签的family:qualify,userID
            put.add(Bytes.toBytes("info"), Bytes.toBytes("id"), Bytes.toBytes(x._2.toString)) //id
            put.add(Bytes.toBytes("info"), Bytes.toBytes("rating"), Bytes.toBytes(x._3.toString)) //rating
            put.add(Bytes.toBytes("info"), Bytes.toBytes("rn"), Bytes.toBytes(x._4.toString)) //rn
            put.add(Bytes.toBytes("info"), Bytes.toBytes("title"), Bytes.toBytes(x._5.toString)) //title
            put.add(Bytes.toBytes("info"), Bytes.toBytes("manuallabel"), Bytes.toBytes(x._6.toString)) //manuallabel
            put.add(Bytes.toBytes("info"), Bytes.toBytes("mod"), Bytes.toBytes(x._7.toString)) //mod
            put.add(Bytes.toBytes("info"), Bytes.toBytes("sysTime"), Bytes.toBytes(x._8.toString)) //sysTime
    
            (new ImmutableBytesWritable, put)
          }
          }.saveAsNewAPIHadoopDataset(jobConf)

