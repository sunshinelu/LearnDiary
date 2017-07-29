# mysql-connector-python安装报错

> 参考链接：
>
> [Python] MySQLdb（即 MySQL-python 包）在 OS X 中安装指南:
<http://www.cnblogs.com/ifantastic/archive/2013/04/13/3017677.html>
>



Mac：10.11.6

Python：Python 2.7.13

进入`终端`输入以下命令：

    easy_install mysql-connector-python

出现以下错误

    Searching for mysql-connector-python
    Reading https://pypi.python.org/simple/mysql-connector-python/
    No local packages or download links found for mysql-connector-python
    error: Could not find suitable distribution for Requirement.parse('mysql-connector-python')
    sunludeMacBook-Pro:~ sunlu$ 
    
进入该网站<https://dev.mysql.com/downloads/connector/python/>，手动下载安装`mysql-connector-python-2.1.6-macos10.12.dmg`.

安装后


   
进入`/Library/Python/2.7/site-packages`目录输入python后再`import mysql.connector`导入成功。


    sunludeMacBook-Pro:~ sunlu$ cd /Library/Python/2.7/site-packages/
    sunludeMacBook-Pro:site-packages sunlu$ ls *mysql*
    _mysql_connector.so
    mysql_connector_python_cext-2.1.6-py2.7.egg-info
    
    mysql:
    __init__.py	connector
    sunludeMacBook-Pro:site-packages sunlu$ python
    Python 2.7.13 |Anaconda custom (x86_64)| (default, Dec 20 2016, 23:05:08) 
    [GCC 4.2.1 Compatible Apple LLVM 6.0 (clang-600.0.57)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    Anaconda is brought to you by Continuum Analytics.
    Please check out: http://continuum.io/thanks and https://anaconda.org
    >>> import mysql.connector
    >>> 

但是在其它目录下输入`import mysql.connector`则报错：


    sunludeMacBook-Pro:~ sunlu$ python
    Python 2.7.13 |Anaconda custom (x86_64)| (default, Dec 20 2016, 23:05:08) 
    [GCC 4.2.1 Compatible Apple LLVM 6.0 (clang-600.0.57)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    Anaconda is brought to you by Continuum Analytics.
    Please check out: http://continuum.io/thanks and https://anaconda.org
    >>> import mysql.connector
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    ImportError: No module named mysql.connector
    >>> 
 
 在anaconda中安装`mysql-connector-python`，
 安装过程如下：
 
 打开anaconda，点击`Environments`,
 
 ![picture1](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/Python/python.error.mysql.01.png)
 
 ![picture2](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/Python/python.error.mysql.02.png)
 
 安装完成后，检测`mysql-connector-python`是否安装成功。
 
    Last login: Sat Jul 29 14:28:06 on ttys000
    sunludeMacBook-Pro:~ sunlu$ python
    Python 2.7.13 |Anaconda custom (x86_64)| (default, Dec 20 2016, 23:05:08) 
    [GCC 4.2.1 Compatible Apple LLVM 6.0 (clang-600.0.57)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    Anaconda is brought to you by Continuum Analytics.
    Please check out: http://continuum.io/thanks 
    
     and https://anaconda.org 
    
    >>> import mysql.connector
    >>> 


使用如下命令安装`MySQL-python`
 
    pip install MySQL-python
   
进入python
    
    python
    
输入以下命令查看是否安装成功
    
    import MySQLdb as mdb