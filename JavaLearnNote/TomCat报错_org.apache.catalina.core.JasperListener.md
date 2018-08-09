# TomCat报错_org.apache.catalina.core.JasperListener

>  参考链接
> <https://wenchao.ren/archives/677>
>
>
>

部署java web项目时，报错：
Tomcat java.lang.ClassNotFoundException: org.apache.catalina.core.JasperListener

解决方案：
检查发现因为之前使用的tomcat7部署的, 现在升级到tomcat8. 但tomcat8的conf中的server.xml文件跟tomcat7中的略有不同。
这个问题的解决办法很简单，在tomcat的server.xml文件中注释掉下面这一行内容就好：

Listener className="org.apache.catalina.core.JasperListener"  
 