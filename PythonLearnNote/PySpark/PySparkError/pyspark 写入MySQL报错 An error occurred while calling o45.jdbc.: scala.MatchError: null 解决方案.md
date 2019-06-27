# pyspark 写入MySQL报错 An error occurred while calling o45.jdbc.: scala.MatchError: null 解决方案

> 参考链接
>
>
> 
> pyspark 写入MySQL报错 An error occurred while calling o45.jdbc.: scala.MatchError: null 解决方案
> <https://blog.csdn.net/helloxiaozhe/article/details/81033767>

    # 本地测试  
    #  出错代码A
    df1.write.mode("append").jdbc(url="jdbc:mysql://localhost:3306/spark_db?user=root&password=yyz!123456", table="test_person", properties={"driver": 'com.mysql.jdbc.Driver'})

    # 正确代码B
    df1.write.jdbc(mode="overwrite", url="jdbc:mysql://localhost:3306/spark_db?user=root&password=yyz!123456", table="test_person", properties={"driver": 'com.mysql.jdbc.Driver'})
    
