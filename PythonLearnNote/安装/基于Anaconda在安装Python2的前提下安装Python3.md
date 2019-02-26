# 基于Anaconda在安装Python2的前提下安装Python3.md

> 参考链接
>
> Mac OS下 Anaconda Python2 和 Python3 配置
> <https://blog.csdn.net/xavierri/article/details/78618052>
>
> MAC 如何在安装anaconda的同时，安装python3和python2
> <https://www.cnblogs.com/dapeng-bupt/p/9927489.html>
>



conda create --name python37 python=3.7

source activate python37

python --version


source deactivate python37




### 删除一个已有的环境
conda remove --name python36 --all
 
### 其他指令
conda info -e   #查看已有的环境
conda remove -n env_name --all  #删除环境
conda install -n py27 anaconda #在py27下安装科学计算的包，包很多，慎重选择
