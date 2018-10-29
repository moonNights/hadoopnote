# hadoop 使用yarn 作资源管理运行mapreduce 出现143异常
## 问题描述
    错误信息:Container [pid=8074,containerID=container_1540817814835_0003_01_000008] is running 530844160B beyond the 'VIRTUAL' memory limit. Current usage: 160.4 MB of 1 GB physical memory used; 2.6 GB of 2.1 GB virtual memory used. Killing container
    Container killed on request. Exit code is 143
    大概说的就是物理内存使用达到了一个限制需要kill对应的container

## 解决办法
   在网上找了 初步解决办法是给yarn 添加配置关闭yarn 启动任务检查是否超过最大值的比例,在yarn-site.xml 添加如下配置
   ```xml
   <property>
      <name>yarn.nodemanager.vmem-check-enabled</name>
      <value>false</value>
   </property>
   ```
   或者通过设置允许container 申请资源的大小限制或在计算机允许申请的虚拟内存占比。

  参考内容：
  1. https://stackoverflow.com/questions/43441437/container-is-running-beyond-virtual-memory-limits
  2. https://blog.csdn.net/mlljava1111/article/details/51867883
