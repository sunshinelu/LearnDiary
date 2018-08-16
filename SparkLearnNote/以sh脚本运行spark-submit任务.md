# 以sh脚本运行spark-submit任务

    #!/bin/bash
    #name=$1
    
    set -x
    source /root/.bashrc
    #cd /root/software/spark-2.0.2/bin
    
    spark-submit \
    --class com.evayInfo.Inglory.Project.UserProfile.RetentionSave \
    --master yarn \
    --num-executors 2 \
    --executor-cores 2 \
    --executor-memory 2g \
    /root/lulu/Progect/Test/SparkDiary.jar
    

注意，

    set -x
    source /root/.bashrc
 
是必须的。