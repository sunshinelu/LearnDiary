# assembly 打包


> 参考链接：
>
> <http://blog.csdn.net/u011826404/article/details/53206369>







首先配置打包信息，在项目结构界面中选择“Artifacts”,在右边操作界面选择绿色”+”号，选择添加JAR包的”From modules with dependencies”方式，出现如下界面，在该界面中选择主函数入口为kmeans，接着填写该JAR包名称和调整输出内容。由于运行环境已经有Scala相关类包，所以在这里去除这些包只保留项目的输出内容。




[ERROR] Failed to execute goal net.alchim31.maven:scala-maven-plugin:3.2.2:compile (scala-compile-first) on project SparkDiary: Execution scala-compile-first of goal net.alchim31.maven:scala-maven-plugin:3.2.2:compile failed. CompileFailed -> [Help 1]
[ERROR] 
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/PluginExecutionException
