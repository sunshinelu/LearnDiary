# 使用`$`获取spark dataframe中的column name


在使用`$`获取spark dataframe中的column name时如果红色标红，则在初始化spark的代码后面添加如下代码：

    import spark.implicits._