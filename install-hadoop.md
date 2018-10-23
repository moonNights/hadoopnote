# hadoop 软件安装

## 依赖软件
   + JDK
   + ssh
## 软件安装-本地模式
   测试
   ```bash
	 $ mkdir input
	 $ cp etc/hadoop/*.xml input
	 $ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.1.jar grep input output 'dfs[a-z.]+'
	 $ cat output/*
   ```
## 软件安装-单节点模式

   配置文件处理,在以下配置文件中添加如下配置：
   1. etc/hadoop/core-site.xml
   ```xml
   	<configuration>
	    <property>
	        <name>fs.defaultFS</name>
                <value>hdfs://localhost:9000</value>
	    </property>
	</configuration>
   ```
   2. etc/hadoop/hdfs-site.xml
   ```xml
     <configuration>
         <property>
	      <name>dfs.replication</name>
              <value>1</value>
	 </property>
     </configuration>
   ```
   3. 在本地添加免密登陆证书
   ```bash
      $ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
      $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
      $ chmod 0600 ~/.ssh/authorized_keys
   ```
   4. 执行
   ```bash
   --不要多次初始化namenode
    $ bin/hdfs namenode -format
    $ sbin/start-dfs.sh
   ```
   5. 在浏览器中通过网页浏览
   ```
   在3.x以后浏览
   http://localhost:98070/
   在3.x以前访问
   http://localhost:50070/
   ```

   6. 执行测试
   ```bash
   $ bin/hdfs dfs -mkdir /user
   $ bin/hdfs dfs -mkdir /user/test
   $ bin/hdfs dfs -put etc/hadoop input
   -- 版本号对应自己安装版本
   $ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.1.jar grep input output 'dfs[a-z.]+'
   $ bin/hdfs dfs -get output output
   $ cat output/*
   ```