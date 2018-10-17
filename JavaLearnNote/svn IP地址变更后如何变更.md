# svn IP地址变更后如何变更


> 参考链接
>
> svn IP地址变更后如何变更
> <https://www.cnblogs.com/westfly/p/3363236.html>
>



通过grep ip地址，发现svn中url地址信息是记录在.svn文件夹entries文件中的，第一种方案应该是遍历目录下的entries文件，将ip替换为新的ip即可。

可以发现这个用sed命令即可搞定。

方案二是利用svn的relocate命令。

如下

    svn switch --relocate "svn://192.168.15.9" "svn://192.168.29.19"

其中 第一个参数所旧IP，第二个参数为新的IP地址。

