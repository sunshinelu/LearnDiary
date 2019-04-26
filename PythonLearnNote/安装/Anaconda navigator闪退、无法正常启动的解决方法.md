# Anaconda navigator闪退、无法正常启动的解决方法

> 参考链接
>
> Anaconda navigator闪退、无法正常启动的解决方法
> <https://blog.csdn.net/qq_34419607/article/details/81269866>
>
> Mac版Anaconda-Navigator闪退
> <https://www.jianshu.com/p/f66a410bccac>
>
> [已解决]Mac下运行spyder提示"Python 意外退出"
> <https://www.cnblogs.com/hingman/p/9801024.html>
>
>




sudo conda update anaconda-navigator

pip uninstall pyqt5

pip install pyqt5==5.10

sudo anaconda-navigator --reset

sudo conda update anaconda-client

sudo conda update -f anaconda-client