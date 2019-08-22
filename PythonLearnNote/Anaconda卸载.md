# Anaconda卸载

>参考链接：
<http://blog.csdn.net/yingyujianmo/article/details/50800568>


由于Anaconda的安装文件都包含在一个目录中，所以直接将该目录删除即可。删除整个Anaconda目录：

    rm -rf anaconda


> 参考链接：
>
> 从Mac中彻底删除Anaconda
> <https://www.jianshu.com/p/8747a347ea8b>
>
> macOS Anaconda 安装和卸载
> <https://www.jianshu.com/p/d250a4245d81>
>
>
>

### 从Mac中彻底删除Anaconda


删除Anaconda的配置，命令如下：

    # 安装anaconda-clean
    conda install anaconda-clean
    
    # 备份
    anaconda-clean --yes
    
    # 删除备份
    rm -rf /Users/sunlu/.anaconda_backup/2019-08-21T105058
    
    # 删除目录
    sudo rm -rf //anaconda3/
    
    # 删除环境本来
    vi ~/.bash_profile
    