# postgresql 在MAC上安装及卸载

> 参考链接
>
> Completely uninstall PostgreSQL 9.0.4 from Mac OSX Lion?
> <https://stackoverflow.com/questions/8037729/completely-uninstall-postgresql-9-0-4-from-mac-osx-lion>
>
> mac下完全卸载postgresql的方法
> <https://blog.csdn.net/stk_tianwen/article/details/17757393>


## 1.在MAC上安装postgresql

下载安装postgresql
<https://www.postgresql.org/download/macosx/>

## 2.在MAC上卸载postgresql

1、如果是postgresql.app的形式，这个简单，跟其他app一样，删除app即可。

2、如果是使用installer图形界面方式安装的。则需要打开终端命令行。

3、执行

    open /Library/PostgreSQL/10/uninstall-postgresql.app
可能会提示你输入密码。

4、等待上一步执行完成后，删除postgresql文件夹       

    sudo rm -rf /Library/PostgreSQL
可能会提示你输入密码

5、删除配置文件

    sudo rm /etc/postgres-reg.ini
可能会提示你输入密码

6、在用户管理中删除postgresql的用户， 系统偏好设置－－》用户及用户组。

7、删除共享内存设置 （我没有做过特殊设置，所以我本机是没有这个文件的，如果有，可以删除。）

    sudo rm /etc/sysctl.conf