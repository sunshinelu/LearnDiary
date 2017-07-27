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

