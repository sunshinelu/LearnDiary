# Linux使用小技巧


### 1. 后台提交任务

后台提交任务，并讲日志保存到指定的文件中：

    nohup ./ylzx_xgwz.sh > xgwz.out 2>&1 &
    
通过上述命令，可将任务运行过程中生成的日志保存在xgwz.out中。

### 2. VI 批量删除指定字符串

vi批量删除指定字符串

    vi DenseMatrix_10W_1024_matrix_2.txt
    :1,$s/\[
    :1,$s/\]
   
因为"［"和"］"是特殊字符，所有需要添加反斜线"\"

    :n,$s/test
    //n可以取1，表示从第一行开始删除含有test 
    
参考链接<http://blog.csdn.net/cheng1988shu/article/details/8253963>


### 3. linux查看指定文件夹大小


    du -sh Projects/IngloryBDMinning/
