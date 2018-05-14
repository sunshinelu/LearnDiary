# jupyter中加上 sys.setdefaultencoding('utf-8') 后不能输出问题

> 参考链接
> 
> jupyter中加上 sys.setdefaultencoding('utf-8') 后就不能输出了
> <https://ask.csdn.net/questions/346937>
>
> Python IDLE reload(sys)后无法正常执行命令的原因
> <https://www.2cto.com/kf/201411/355112.html>

解决方法：
    
    import sys
    stdi, stdo, stde = sys.stdin, sys.stdout, sys.stderr
    reload(sys)
    sys.setdefaultencoding('utf-8')
    sys.stdin, sys.stdout, sys.stderr = stdi, stdo, stde
    

问题原因：

主要是reload(sys)的时候，sys.stdout 这个参数被重置为了ipython 的对象，导致无法输出。