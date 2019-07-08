# Mac使用本机windows虚拟机上的网络


> 参考链接
>
> Mac如何直连东区服务器
> <http://10.20.5.63/post/threaddetail?threadId=99>
>
>
>


所需安装软件：

CCProxy代理理服务器软件(windows版) 

Proxifier代理理服务器器(Mac版) 

去百度云盘`Mac链接win网络`中下载所需软件，并安装。其中安装`Proxifier代理理服务器器(Mac版) `时需要输入`key`文档中的秘钥。


## 1. windows虚机安装CCProxy及相关配置说明

(1) 安装CCProxy

在CCProxy主界面上，单击设置->高级->⽹网络，在弹出的高级对话框中取消选中“禁⽌局域网外部用户”，再单击“确定”按钮。

![CountBased01](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/Mac/Mac.Mac使用本机windows虚拟机上的网络_win_01.png)

(2) 获取IP


![CountBased01](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/Mac/Mac.Mac使用本机windows虚拟机上的网络_win_02.png)

IP地址为：172.16.6.128

## 2. Mac安装Proxifier及相关配置说明

(1) Mac安装Proxifier

安装`Proxifier代理理服务器器(Mac版) `时需要输入`key`文档中的秘钥。

(2)Proxifier配置Socks代理

Proxies配置Socks代理，输入windows代理服务器的地址和端⼝号，Protocol选择第一项。

![CountBased01](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/Mac/Mac.Mac使用本机windows虚拟机上的网络_mac_01.png)


(3)DNS设置

![CountBased01](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/Mac/Mac.Mac使用本机windows虚拟机上的网络_mac_02.png)


(4)Rules设置

![CountBased01](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/Mac/Mac.Mac使用本机windows虚拟机上的网络_mac_03.png)
