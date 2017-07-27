# assembly 打包


> 参考链接：
>
>使用 IntelliJ IDEA打包Spark应用程序:
 <http://blog.csdn.net/u011826404/article/details/53206369>
>
> Storm Spark Scala 混合代码快速编译打包jar方式，然后java风格使用（朋友咨询）:
<https://my.oschina.net/yilian/blog/670786>

## 1. 配置assembly插件

在pom文件中配置assembly打包插件 

     <plugin>
                <artifactId>maven-assembly-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <classifier>dist</classifier>
                    <appendAssemblyId>true</appendAssemblyId>
                    <descriptorRefs>
                        <descriptor>jar-with-dependencies</descriptor>
                    </descriptorRefs>
                </configuration>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>


## 2. 配置打包信息


点击`File` > `Project Structure`

![配置Artifacts](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/Spark/spark.assembly.01.png)

在项目结构界面中选择`Artifacts`,在右边操作界面选择绿色`+`号，选择添加JAR包的`From modules with dependencies`方式，


![配置Artifacts](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/Spark/spark.assembly.02.png)


在Main Class中点击`...`，选择任意一个Main方法。

![配置Artifacts](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/Spark/spark.assembly.03.png)



![配置Artifacts](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/Spark/spark.assembly.04.png)

修改JAR包名称

![配置Artifacts](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/Spark/spark.assembly.05.png)

## 3. assembly打包

点击`assembly:assembly`进行打包。

![配置Artifacts](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/Spark/spark.assembly.06.png)

## 4. 结果

最后在根目录下生成`target`文件夹，其中包含`SparkDiary-1.0-SNAPSHOT.jar`和`SparkDiary-1.0-SNAPSHOT-jar-with-dependencies.jar`。
`SparkDiary-1.0-SNAPSHOT.jar`为没有打入依赖的jar，`SparkDiary-1.0-SNAPSHOT-jar-with-dependencies.jar`为打入依赖的jar。

![配置Artifacts](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/Spark/spark.assembly.07.png)


## 错误信息：

### 1)点击`assembly:assembly`时报错：
    [ERROR] Failed to execute goal net.alchim31.maven:scala-maven-plugin:3.2.2:compile (scala-compile-first) on project SparkDiary: Execution scala-compile-first of goal net.alchim31.maven:scala-maven-plugin:3.2.2:compile failed. CompileFailed -> [Help 1]
    [ERROR] 
    [ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
    [ERROR] Re-run Maven using the -X switch to enable full debug logging.
    [ERROR] 
    [ERROR] For more information about the errors and possible solutions, please read the following articles:
    [ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/PluginExecutionException

**解决方法** ： 删除项目中的spring相关内容后，`assembly:assembly`运行成功。