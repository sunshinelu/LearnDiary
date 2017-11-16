# 两个standby resourcemanager

> 参考资料：
>
> resourcemanager启动报错，两个resourcemanager都报这个错,进程起来了，但是两个rm既不是standby也不是active
> <https://q.cnblogs.com/q/89541>


yarn.resourcemanager.recovery.enabled改为false
这个属性的作用是：当yarn中有任务在跑时，如果rm宕机，设置成ture，rm重启时会恢复原来没有跑完的application，
日志提示Application with id application_1482728774024_0001 is already present! Cannot add a duplicate!
application_1482728774024_0001不存在，无法恢复，不知道为什么这个application被删除了，但是还保存了它的状态。很诡异！

RM HA机制的一些研究
<http://jxy.me/2015/05/02/hadoop-resourcemanager-recovery/>

ResourceManager fails to transition to Active mode with "InvalidResourceRequestException"
<https://community.mapr.com/docs/DOC-1184>