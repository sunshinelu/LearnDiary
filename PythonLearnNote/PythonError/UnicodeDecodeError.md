# UnicodeDecodeError

解决字符编码问题

错误：

UnicodeDecodeError: 'ascii' codec can't decode byte 0xe5 in position 0: ordinal not in range(128)

参考链接：http://blog.sina.com.cn/s/blog_6c39196501013s5b.html

解决方法：

    import sys
    reload(sys)
    sys.setdefaultencoding('utf-8')
