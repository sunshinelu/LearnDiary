# Linux使用小技巧

后台提交任务，并讲日志保存到指定的文件中：

    nohup ./ylzx_xgwz.sh > xgwz.out 2>&1 &
    
通过上述命令，可将任务运行过程中生成的日志保存在xgwz.out中。