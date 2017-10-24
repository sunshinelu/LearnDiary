# Yarn基本操作

查看当前任务列表：

    yarn application -list

根据任务ID杀死某个任务：

    yarn application -kill application_1437456051228_1725

查看日志：

    yarn logs -applicationId application_1504508652498_3356

查看日志并将日志保存到文件：

    yarn logs -applicationId application_1504508652498_3356 > application_1504508652498_3356.log

查看日志状态：

    yarn applicaiton -status application_1504508652498_3356