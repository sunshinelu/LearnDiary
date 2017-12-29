# maven 打包外部数据

> 参考链接：
>
> maven资源文件的相关配置
> <https://www.cnblogs.com/pixy/p/4798089.html>

    <resources>
            <!-- 将resources和data文件夹打包到jar包中 -->
            <!-- 参考链接：https://www.cnblogs.com/pixy/p/4798089.html-->
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>*.txt</include>
                    <include>*.ser</include>
                </includes>
                <excludes>
                    <exclude>*.xml</exclude>
                </excludes>
            </resource>
            <resource>
                <directory>data</directory>
            </resource>
        </resources>