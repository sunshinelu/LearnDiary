# Mac下安装tensorflow

> Mac版本：10.11.6
>
> python版本：Python 2.7.13

    conda create -n tensorflow python=2.7
    source activate tensorflow
    pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/mac/tensorflow-0.8.0rc0-py2-none-any.whl
    source deactivate


升级anaconda

    conda --v
    conda update conda
    conda update anaconda
    conda --v

查看pip版本：
 

    pip -V

Mac中自带python版本为`2.7.10`，位于`/usr/bin/python`中，使用anaconda安装的python版本为`2.7.11`，位于`/Users/sunlu/anaconda/bin/python`中。


##  重新安装Tensorflow

> 时间：2017年07月29日

### 1. 卸载anaconda：

    sudo rm -rf anaconda
    
*注意*：使用`rm -rf anaconda`卸载时会卸载不干净。

### 2. 重新安装anaconda

去网站<https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/>中下载`Anaconda2-4.4.0-MacOSX-x86_64.pkg`。
双击`Anaconda2-4.4.0-MacOSX-x86_64.pkg`进行安装。

### 3. 升级anaconda

    conda --v
    conda update conda
    conda update anaconda

### 4. 安装Tensorflow


    anaconda search -t conda tensorflow
    anaconda show conda-forge/tensorflow
    conda install --channel https://conda.anaconda.org/conda-forge  tensorflow

> 参考链接：
<http://blog.csdn.net/goodshot/article/details/61926805>

### 5. 查看TensorFlow版本

    sunludeMacBook-Pro:~ sunlu$ python
    Python 2.7.13 |Anaconda custom (x86_64)| (default, Dec 20 2016, 23:05:08) 
    [GCC 4.2.1 Compatible Apple LLVM 6.0 (clang-600.0.57)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    Anaconda is brought to you by Continuum Analytics.
    Please check out: http://continuum.io/thanks 
    
     and https://anaconda.org 
    
    >>> import tensorflow as tf
    >>> tf.__version__
    '1.2.0'
    >>> 
    
*注意*：`tf.__version__`中`__`看着像是一个下划线，实际上是两个下划线。