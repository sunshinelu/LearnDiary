# PySpark读取HBase数据

1. PySpark基于SHC框架读取HBase数据并转成DataFrame
<https://blog.csdn.net/u014736152/article/details/89605015>


2. Spark2.1.0+入门：读写HBase数据(Python版)
<http://dblab.xmu.edu.cn/blog/1715-2/>


<https://mvnrepository.com/artifact/org.apache.spark/spark-examples_2.11/1.6.0-typesafe-001>



python中，用pyspark读取Hbase数据，并转换为dataframe格式
<https://blog.csdn.net/u011412768/article/details/93404921>

pyspark连接hbase的三种方式
<https://blog.csdn.net/sunseaxhj/article/details/91311019>

pyspark连hbase报org.apache.spark.examples.pythonconverters.ImmutableBytesWritableToStringConverter
<https://blog.csdn.net/qiang0066/article/details/79669947>

用Python操作HBase之HBase-Thrift
<https://blog.csdn.net/y472360651/article/details/79055875>


songjian28/pyspark-hbase
<https://github.com/songjian28/pyspark-hbase/blob/master/shbase/shb.py>

akanyaani/PySpark-Helpers
<https://github.com/akanyaani/PySpark-Helpers/blob/master/pySparkWithHbase.py>


lmlzk/pyspark_hbase
<https://github.com/lmlzk/pyspark_hbase/blob/master/pyspark_connect_hbase_test.py>

Pyspark实战（五）pyspark+happybase操作hbase
<https://blog.csdn.net/luoye4321/article/details/94413916>


pyspark连接hbase学习
<https://blog.csdn.net/deer_sheep/article/details/82838559>



Spark2.1.0+入门：读写HBase数据(Python版)
<http://dblab.xmu.edu.cn/blog/1715-2/>

使用pycharm访问远端hbase集群
<https://blog.csdn.net/u014407826/article/details/84939515>

Spark 读写 HBase 的两种方式（RDD、DataFrame）
<https://blog.csdn.net/yitengtongweishi/article/details/82556824>


Apache HBase ™ Reference Guide
<https://hbase.apache.org/book.html#_sparksql_dataframes>

Read from HBase with PySpark
<https://stackoverflow.com/questions/51306666/read-from-hbase-with-pyspark>

Spark DataFrame写入HBase的常用方式
<https://www.cnblogs.com/xing901022/p/8486290.html>




-----

bigdata7:

hbase shell

list

disable 't_student_sunlu'
drop 't_student_sunlu'

create 't_student_sunlu','info'

describe 't_student_sunlu'

//首先录入student表的第一个学生记录
put 't_student_sunlu','1','info:name','Xueqian'
put 't_student_sunlu','1','info:gender','F'
put 't_student_sunlu','1','info:age','23'
//然后录入student表的第二个学生记录
put 't_student_sunlu','2','info:name','Weiliang'
put 't_student_sunlu','2','info:gender','M'
put 't_student_sunlu','2','info:age','24'

//如果每次只查看一行，就用下面命令
get 't_student_sunlu','1'
//如果每次查看全部数据，就用下面命令
scan 't_student_sunlu'

count 't_student_sunlu'


bigdata7:

/usr/hdp/2.2.4.2-2/hbase/lib

cp hbase*.jar /root/lulu/jars/hbase/
cp guava-12.0.1.jar /root/lulu/jars/hbase/
cp htrace-core-3.0.4.jar /root/lulu/jars/hbase/
cp protobuf-java-2.5.0.jar /root/lulu/jars/hbase/



### 1. 方法一：使用happyhase

使用 happybase

安装 happybase：

    pip install happybase
    
测试代码如下：

    
    import happybase
    from pyspark.sql import SparkSession
    
    spark = SparkSession\
            .builder\
            .appName("read HBase data")\
            .getOrCreate()
    
    sc = spark.sparkContext
    #
    # # host = "192.168.37.21,192.168.37.22,192.168.37.23"
    host = "192.168.37.22"
    table_name = 't_student_sunlu'
    
    connection = happybase.Connection(host=host)
    print(connection.tables())
    """
    [b'034acdfe-c660-47cc-9608-ae1371e4fc19_webpage', b'03e2c58e-c881-4a0f-adb0-395dae3efaf0_webpage_webpage', b'0f9ddc53-24ca-482f-bc7e-c44aad753013_webpage_webpage', b'22261602-b772-4fb1-8465-9b710ad37059_webpage_webpage', b'309de942-b5ce-433e-9a1e-59c7c5d59e2b_webpage_webpage', b'33763d5c-4fdf-4ce2-aedf-a36d0dc1a1ed_webpage_webpage', b'471a783f-8f54-49a2-95d7-3eab9a7369da_webpage', b'55243530-c6ee-43c6-8770-2d656c324c6c_webpage', b'5c1408eb-3d59-43e3-89c8-3042882f2168_webpage', b'5e563de2-ac88-492f-99c2-9c0bff80bbc6_webpage', b'60d2906c-45d1-4226-b62b-562f20d932e2_webpage', b'79b52678-84e7-4be3-93ec-0be0440880ba_webpage_webpage', b'7cde02c7-175e-4bdc-9a74-7f3b2410228d_webpage', b'8d9fd563-4446-4412-b16d-456cd78718c0_webpage', b'98d83af3-92ed-4a41-ab89-a615831138b8_webpage', b'9c033c7d-de7b-4de0-9722-a66e0b3d8412_webpage', b'B_DATA_E', b'B_DATA_I', b'CNKIF-new-2016_webpage', b'CNKIG-2016_webpage', b'CNKIH-2016_webpage', b'CNKII-2016_webpage', b'CNKIJ-2016_webpage', b'CNKIJ5_webpage', b'CNKI_Parse', b'CNKI_Parse2016-1', b'CNKI_parse0812', b'CNKI_parseD', b'CNKI_parseE', b'CNKI_parseF', b'CNKI_parseF_sw', b'CNKI_parseG', b'CNKI_parseH', b'CNKI_parseI', b'CNKI_parseJ', b'CNKIsuntest_webpage', b'C_DATA_E', b'C_DATA_I', b'EN-test', b'IEEE2005-localNew_webpage', b'IEEE2005_author_localNew_webpage', b'IEEE2005_keyword_webpage', b'IEEE2005_keywords_local_webpage', b'IEEE2006_author_local_webpage', b'IEEE2006_author_webpage', b'IEEE2006_keyword_webpage', b'IEEE2006_keywords_local_webpage', b'IEEE2007_author_local_webpage', b'IEEE2007_author_webpage', b'IEEE2007_keyword_webpage', b'IEEE2007_keywords_local_webpage', b'IEEE2007_webpage', b'IEEE2008-local_webpage', b'IEEE2008_author_webpage', b'IEEE2008_keyword_webpage', b'IEEE2008_keywords_local_webpage', b'IEEE2009-localNew_webpage', b'IEEE2009_author_webpage', b'IEEE2009_keyword_webpage', b'IEEE2009_keywords_local_webpage', b'IEEE2009_webpage', b'IEEE2010-local_webpage', b'IEEE2010_author_localNew_webpage', b'IEEE2010_author_webpage', b'IEEE2010_keyword_webpage', b'IEEE2010_keywords_local_webpage', b'IEEE2010_webpage', b'IEEE2011-local_webpage', b'IEEE2011_author_localNew_webpage', b'IEEE2011_author_webpage', b'IEEE2011_keyword_webpage', b'IEEE2011_keywords_local_webpage', b'IEEE2011_webpage', b'IEEE2012-localNew_webpage', b'IEEE201201_webpage', b'IEEE2012_author_localNew_webpage', b'IEEE2012_author_webpage', b'IEEE2012_keyword_webpage', b'IEEE2012_keywords_local_webpage', b'IEEE2012_webpage', b'IEEE2013-localNew_webpage', b'IEEE2013_author_localNew_webpage', b'IEEE2013_author_local_webpage', b'IEEE2013_author_webpage', b'IEEE2013_keyword_webpage', b'IEEE2013_keywords_local_webpage', b'IEEE2013_webpage', b'IEEE2014-localNew_webpage', b'IEEE201401_webpage', b'IEEE2014_author_local_webpage', b'IEEE2014_author_webpage', b'IEEE2014_keyword_webpage', b'IEEE2014_webpage', b'IEEE2015-local_webpage', b'IEEE201503_webpage', b'IEEE201504_webpage', b'IEEE201505_webpage', b'IEEE2015_author_webpage', b'IEEE2015_keyword_webpage', b'IEEE2015_webpage', b'IEEE5000_webpage', b'IEEE_Final', b'IEEE_Final_webpage', b'IEEE_Parse0817', b'IEEE_Parse2012', b'IEEE_Parse2015', b'IEEE_Parse201503', b'IEEE_Parsebdtest', b'IEEEparse_author_local', b'RCH_webpage', b'RhbaseTest', b'SERVER_METRICS', b'SYSTEM.CATALOG', b'SYSTEM.SEQUENCE', b'SYSTEM.STATS', b'TEST', b'TEST.PERSON', b'TEST.SCORE', b'TestTable', b'WEB_STAT', b'XQGZtest_webpage', b'XQHZtest_webpage', b'a0b6ce3b-6e15-4642-977b-148daec19e62_webpage', b'a9f18266-2807-4952-bd5f-2d2c5a0672bb_webpage', b'a_webpage', b'b6d672a5-fef4-40be-a881-550d519e9292_webpage', b'b6d672a5-fef4-40be-a881-550d519e9292_webpage_webpage', b'bb0ba425-e360-496a-a4ce-c85a591e5b69_webpage_webpage', b'beijing-gpg_webpage', b'beijinggs-gpg_webpage', b'bjepb1_webpage', b'bjepb2_webpage', b'bjepb_webpage', b'bubs', b'cjcp-znjx1_webpage', b'cjcp-znjx_webpage', b'cjcpznjx_webpage', b'cnki_author_lix', b'cnkilw_zzb', b'crawlIEEE2009_webpage', b'crawlIEEE2010_webpage', b'crawlIEEE2011_webpage', b'crawlIEEE2012_webpage', b'crawlIEEE2015_webpage', b'crawlertest', b'd6ca3ee3-6ad7-44e4-8675-666ed81f1eb9_webpage', b'data0', b'data1', b'dframe', b'docsimi_jaccard', b'docsimi_title', b'ds-gsqy_webpage', b'e69263de-a12a-4ff7-9344-73d7088aeb19_webpage', b'e78563bb-20dc-4f5d-bf40-9e6836c8242c_webpage', b'elstest_webpage', b'epub_parse', b'f179e0ca-b82a-485c-9530-07a94ca219a0_webpage', b'f755fcd6-e2fd-401f-9498-643de1c37df0_webpage', b'f755fcd6-e2fd-401f-9498-643de1c37df0_webpage_webpage', b'f809624f-2acb-45e7-9c70-938d9470678f_webpage', b'fetch-test_webpage', b'ff84f058-8b35-43b5-b9dd-f7470f27e083_webpage', b'ffd1be01-cf3d-426f-93d0-5609b7c23072_webpage', b'filed_test', b'fj-hdfs_webpage', b'fj-test-temp_webpage', b'fj-test-yangch_webpage', b'fj-test_webpage', b'fj_table', b'fjdown-test_webpage', b'fjtest_webpage', b'html_webpage', b'ieee-test', b'ieee-test_webpage', b'ieee2006_parse', b'ieee2007_parse', b'ieee2008_parse', b'ieee2009_parse', b'ieee2010_parse', b'ieee2011_parse', b'ieee2013_parse', b'ieee2014_parse', b'ieee2015_parse', b'ieee_keyword_parse2004', b'ieee_parse_local_all', b'ieeeauthor_local_parse', b'ieeeparse_all', b'ieeeparse_sw', b'ieeeparselocal_2006', b'ieeeparselocal_2007', b'ieeeparselocal_2008', b'ieeeparselocal_2009', b'ieeeparselocal_2010', b'ieeeparselocal_2011', b'ieeeparselocal_2012', b'ieeeparselocal_2013', b'ieeeparselocal_2014', b'ieeeparselocal_2015', b'image-source', b'image_webpage', b'jn324.txt_webpage', b'jn324_webpage', b'kjreward', b'kxy-tpxw_webpage', b'lin', b'lixtest', b'lm-test-yangch-total-2_webpage', b'lm-test-yangch-total_webpage', b'lm-test-yangch_webpage', b'lx', b'member', b'mof-yangch_webpage', b'mohrss_webpage', b'mytable', b'ns1:t1', b'numberOfRounds_webpage', b'nutch-test_webpage', b'nutch-ywk', b'ocal_webpage', b'pentaho_mappings', b'people1000_local_webpage', b'people1000_parse', b'phoenix', b'plan1000_deploy_webpage', b'qingyunzhi', b'qy-jgjc_webpage', b'qy-test_webpage', b'qy_webpage', b'rc_lw', b'rch-sy-20180930_webpage', b'rch-sy_webpage', b'rchrmw1_webpage', b'rchrmw_webpage', b'recommender_als', b'recommender_combined', b'recommender_content', b'recommender_item', b'recommender_temp', b'recommender_user', b'relation', b'relation_shuxi', b'rencai-test-yangch_webpage', b'science2004-local_webpage', b'science2005-local_webpage', b'science2006-local_webpage', b'science2007-local_webpage', b'science2008-local_webpage', b'science2009-local_webpage', b'science2010-local_webpage', b'science2011-local_webpage', b'science2012-local_webpage', b'science2013-local_webpage', b'science2014-local_webpage', b'scores', b'sdast-20181024_webpage', b'sdast-20181026-sy_webpage', b'sdast-20181026_webpage', b'sdast-webpage_webpage', b'sdeic_tp_webpage', b'sdeic_webpage', b'sdslkx-20181024_webpage', b'sdslkx_webpage', b'seed', b'shandong-ccgp_webpage', b'shw-data_webpage', b'shw-test-0417_webpage', b'shw-total_webpage', b'shwt-swb_webpage', b'shwt-test_webpage', b'shwt-tjj_webpage', b'shwt-total_webpage', b'shwt1203_webpage', b'sipo-patexplorer1_webpage', b'sipo-patexplorer4_webpage', b'sipo-test-temp_webpage', b'sipo-test_webpage', b'springer-yangchun-20190806_webpage', b'springer2000-local_webpage', b'springer2001-local_webpage', b'springer2002-local_webpage', b'springer2003-local_webpage', b'springer2004-local_webpage', b'springer2005-local_webpage', b'springer2006-local_webpage', b'springer2007-local_webpage', b'springer2008-local_webpage', b'springer2009-local_webpage', b'springer2010-local_webpage', b'springer2011-local_webpage', b'springer2012-local_webpage', b'springer2013-local_webpage', b'springer2014-local_webpage', b'springer2015-local_webpage', b'springer2016-local_webpage', b'springer:springer-yangchun-20190806_webpage', b'springer_parse', b'springer_parse1', b'springer_parse_01', b'springer_parse_02', b'student', b'sunhaoTestSpace:test', b'swtest-lanmu_webpage', b'swtest-sdetn_webpage', b'swtest-ywk324_webpage', b'swtest-ywk_webpage', b'swtest1_webpage', b'swtest2_webpage', b'swtest321_webpage', b'swtest322_webpage', b'swtest323_webpage', b'swtest3_webpage', b'swtest4_webpage', b'swtest522_1_webpage', b'swtest522_webpage', b'swtest5_webpage', b'swtest6_webpage', b'swtest712', b'swtest_jn10_webpage', b'swtest_jn11_webpage', b'swtest_jn12_webpage', b'swtest_jn13_webpage', b'swtest_jn14_webpage', b'swtest_jn15_webpage', b'swtest_jn16_webpage', b'swtest_jn17_webpage', b'swtest_jn18_webpage', b'swtest_jn1_webpage', b'swtest_jn20_webpage', b'swtest_jn21_webpage', b'swtest_jn22_webpage', b'swtest_jn23_webpage', b'swtest_jn24_webpage', b'swtest_jn25_webpage', b'swtest_jn26_webpage', b'swtest_jn27_webpage', b'swtest_jn28_webpage', b'swtest_jn29_webpage', b'swtest_jn2_webpage', b'swtest_jn30_webpage', b'swtest_jn31_webpage', b'swtest_jn320_webpage', b'swtest_jn32_webpage', b'swtest_jn33_webpage', b'swtest_jn34_webpage', b'swtest_jn35_webpage', b'swtest_jn36_webpage', b'swtest_jn37_webpage', b'swtest_jn38_webpage', b'swtest_jn3_webpage', b'swtest_jn4_webpage', b'swtest_jn5_webpage', b'swtest_jn6_webpage', b'swtest_jn7_webpage', b'swtest_jn8_webpage', b'swtest_jn9_webpage', b'swtest_jn_webpage', b'swtest_lanmu1_webpage', b'swtest_lanmu3_webpage', b'swtest_lanmu4_webpage', b'swtest_lanmu6_webpage', b'swtest_lanmu7_webpage', b'swtest_lanmu_webpage', b'swtest_lanmusingle_webpage', b'swtest_yilan_guizhou10_webpage', b'swtest_yilan_guizhou11_webpage', b'swtest_yilan_guizhou12_webpage', b'swtest_yilan_guizhou1_webpage', b'swtest_yilan_guizhou2_webpage', b'swtest_yilan_guizhou3_webpage', b'swtest_yilan_guizhou4_webpage', b'swtest_yilan_guizhou5_webpage', b'swtest_yilan_guizhou6_webpage', b'swtest_yilan_guizhou7_webpage', b'swtest_yilan_guizhou8_webpage', b'swtest_yilan_guizhou9_webpage', b'swtest_yilan_guizhou_webpage', b't1', b't3_test_xuexia', b't_RatingSys', b't_TDT_result', b't_TDT_test_ten', b't_check_time', b't_docSimsRecommend', b't_docsimi', b't_hbaseFilter', b't_hbaseSink', b't_keyWords', b't_logs_sun', b't_simiDocs', b't_student_sunlu', b't_userProfileV1', b't_userProfileV2', b't_wu_alstest', b't_wu_alstest2', b't_wu_bidding', b't_wu_bidding_ww', b't_wu_newtest2', b't_wu_test', b't_yeeso100', b't_yeeso_xgwords', b't_yilan_als', b't_ylzx_sun', b't_ywkUP', b'test001_webpage', b'testSpringer_webpage', b'test_webpage', b'testels_webpage', b'think1000_webpage', b'tianjin-yilan_webpage', b'tjj-test_webpage', b'tmall-test_webpage', b'tp_webpage', b'webpage', b'wu_recommend', b'wzb', b'wzb2', b'xcls-jgs-webpage_webpage', b'xcls-jgs_webpage', b'xcls_webpage', b'xqhz-testNew0904_webpage', b'xxx_webpage', b'yangch-test-keji_webpage', b'yc-test-interface', b'yeeso-test-fj_webpage', b'yeeso-test-tp_webpage', b'yeeso-test-ycxpath_webpage', b'yeeso-test-ycxpath_webpage_prep', b'yeeso-test-ywk_webpage', b'yeeso-test-ywk_webpage_webpage', b'yeeso-zsj_webpage', b'yeesoTemp_webpage', b'yeeso_all_webpage', b'yeeso_beijing_title_webpage', b'yeeso_beijing_webpage', b'yeeso_liaoning1_webpage', b'yeeso_liaoning_webpage', b'yeeso_shandong_webpage', b'yeeso_webpage', b'yilan-jn1_webpage', b'yilan-jn2_webpage', b'yilan-jn_webpage', b'yilan-rencai-total-temp_webpage', b'yilan-rencai_webpage', b'yilan-sunlu', b'yilan-test-yangch_webpage', b'yilan-total-analysis_webpage', b'yilan-total_webpage', b'yilan-tp_webpage', b'yilan-ywkTemp_webpage', b'yilan-ywk_webpage', b'yilan-zhaotoubiao_webpage', b'yilan-zhengwu-total-temp_webpage', b'yilan-zlfzb_webpage', b'yilan-zlzx-test_webpage', b'yilan-zlzx-total-temp-2_webpage', b'yilan-zlzx-total-temp-test_webpage', b'yilan-zlzx-total-temp_webpage', b'yilan-zlzx-total-temp_webpage_webpage', b'yilan-ztb-total-temp-2_webpage', b'yilan-ztb-total-temp_webpage', b'yilan-ztb-total_webpage', b'yilan-ztb-xtlr-temp_webpage', b'yilan_guizhou_webpage', b'yilan_job', b'yilan_shw_webpage', b'yilan_web_column', b'yilan_web_column_name', b'yilan_web_programa', b'yilan_ywkTemp_webpage', b'yilan_ywk_webpage', b'ylzx-keji-test-temp_bf_webpage', b'ylzx-keji-test-temp_webpage', b'ylzx-rencai-syb-temp_webpage', b'ylzx-rencai-test-temp_webpage', b'ylzx-wxgzh_webpage', b'ylzx-zhengwu-test-temp_webpage', b'ylzx-ztb-test-temp_webpage', b'ylzx_cnxh', b'ylzx_cnxh_combined', b'ylzx_logs_20180702', b'ylzx_xgwz', b'ylzx_xgwz_2', b'ylzx_xgwz_temp', b'yq_cj_2_webpage', b'yq_cj_webpage', b'yqcrawl_webpage', b'ywkNew_webpage', b'ywkTemp1_webpage', b'ywkTemp_webpage', b'ywk_jn_webpage', b'ywk_jnjxwTemp_webpage', b'zdhsNew_webpage', b'zdhs_webpage', b'zlzx-test-yangch24_webpage', b'zlzx-test-yangch_webpage', b'ztb-fj-lm_webpage', b'ztb-parse-date_webpage', b'ztb-test-temp_webpage', b'ztb-total-0323_webpage', b'ztb-total-0324_webpage', b'ztb-total_webpage', b'ztb-zcwz2_webpage', b'ztb-zcwz_webpage', b'ztb_seedDetails', b'ztbcrawl-62-64_webpage', b'ztbcrawl-64-66_webpage', b'ztbcrawl-66-68_webpage', b'ztbcrawl-68-70_webpage', b'ztbcrawl-70-72_webpage', b'zzb-ieee', b'zzz-test-user', b'zzz-ztb-qb', b'zzz-ztb-qb-2']
    """
    h_table = connection.table(table_name)
    hbase_rdd = sc.parallelize(h_table.scan())
    print(hbase_rdd.collect())
    """
    [(b'1', {b'info:age': b'23', b'info:gender': b'F', b'info:name': b'Xueqian'}), 
    (b'2', {b'info:age': b'24', b'info:gender': b'M', b'info:name': b'Weiliang'})]
    """
    
    
    new_student = sc.parallelize([("3","info:name","sunlu"),("3","info:age","30"),("3","info:gender","F")])
    for persion in new_student.collect():
        h_table.put(persion[0],{persion[1]:persion[2]})
    
    hbase_rdd2 = sc.parallelize(h_table.scan())
    print(hbase_rdd2.collect())
    """
    [(b'1', {b'info:age': b'23', b'info:gender': b'F', b'info:name': b'Xueqian'}), (b'2', {b'info:age': b'24', b'info:gender': b'M', b'info:name': b'Weiliang'}), (b'3', {b'info:age': b'30', b'info:gender': b'F', b'info:name': b'sunlu'})]
    
    """

注意：需检查`ThriftServer`是否开启。
