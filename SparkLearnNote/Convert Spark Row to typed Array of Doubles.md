# Convert Spark Row to typed Array of Doubles

改变array中的数据类型

> 参考链接
>
> Convert Spark Row to typed Array of Doubles
> <https://stackoverflow.com/questions/30354483/convert-spark-row-to-typed-array-of-doubles>
>

    SparkDataFrame.map{r => 
      val array = r.toSeq.toArray 
      val doubleArra = array.map(_.toDouble) 
    } // Fails with value toDouble is not a member of any

或者

    SparkDataFrame.map{r => 
          val array = r.toSeq.toArray 
          array.map(_.asInstanceOf[Double]) 
        } // Fails with java.lang.ClassCastException: java.lang.Integer cannot be cast to java.lang.Double
        

或者

    SparkDataFrame.map{r => 
      val doubleArray = for (i <- 5 to 1000){
          r.getInt(i).toDouble
      }.toArray
      Vectors.dense(doubleArray) 
     } 
 
或者

    SparkDataFrame.map{r => 
      doubleArray = Array(r.getInt(5).toDouble, r.getInt(6).toDouble) 
      Vectors.dense(doubleArray) } 