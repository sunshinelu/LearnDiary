
# HBase shell基本操作

清空表：
truncate 'docsimi_word2vec'

删除表：
disable 'docsimi_svd'
drop 'docsimi_svd'

根据rowkey查看某一条数据：
get 'yilan-total_webpage','e7685b22-451a-434e-8489-832561c770d9'

查看表结构
get 'yilan-total-analysis_webpage','e7685b22-451a-434e-8489-832561c770d9'