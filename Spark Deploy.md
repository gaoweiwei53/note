Q: 
1. 免密登录为什么要修改权限
2. Hadoop的 NameNode 和 DataNode是否必须要启动？


# Spark on Yarn 部署
## 1. 环境配置
### 1.1 安装JDK 
在 .profile文件中设置环境变量

    export JAVA_HOME=/jdk-path
    export PATH=$PATH:$JAVA_HOME/bin
    
 ### 1.2 安装Sacla
 设置环境变量
 ```bash
    export SCALA_HOME=/home/hadoop/scala-2.X
    export PATH=$PATH:$SCALA_HOME/bin
```

```bash
    $ source /etc/profile  
    $ scala -version        #打印出版本信息，则说明安装成功。
```
 
 
### 1.3 修改hosts文件
在每台主机上修改
```bash
sudo vim /etc/hosts
*.*.*.* Master
*.*.*.* Slave1
*.*.*.* Slave2
127.0.0.1 localhost
127.0.1.1 localhost
```
### 1.4 配置免密登录
在所有机器上都生成私钥和公钥

```bash
ssh-keygen -t rsa  
```
在NameNode和ResourceManager所在的节点进行如下操作：
```bash
ssh-copy-id slave1
ssh-copy-id slave2
```
在终端中输入登录命令，例如：_ssh hadoop@Slave1_ 如果直接登录成功而不需要登录密码，则表示设置正确；如果登录不成功，即仍然需要登录密码，则可能需要修改文件authorized_keys的权限。  
**注：.ssh 文件夹的权限必须为700，authorized_keys文件权限必须为600**  
使用如下命令改变文件夹权限：chmod 600 ~/.ssh/authorized_keys

## 2. 配置Hadoop
### 2.1 配置 Yarn环境变量

    export HADOOP_HOME=/
    export HADOOP_CONF_DIR=
    export YARN_HOME=
    export YARN_CONF_DIR= 

`source ./profile`
### 2.2 配置Hadoop
需要配置以下7个文件：
* hadoop-env.sh
* yarn-en.sh
* slaves
* core-site.xml
* maprd-site.xml
* yarn-site.xml

在`hadoop-env.sh`中配置JAVA_HOME
```ruby
    # The java implementation to use.
    export JAVA_HOME=/jdk-path
```
在`yarn-env.sh`中配置JAVA_HOME
```ruby
    # some Java parameters
    export JAVA_HOME=/jdk-path
```
在`slaves`中配置slave节点的ip或者host
```
    slave1_hostname
    slave2_hostname
```
修改`core-site.xml`
```xml
    <configuration>
        <!-- 指定 HDFS 中 NameNode 的地址 -->
        <property>
        <name>fs.defaultFS</name>
        <value>hdfs://master:9000/</value>
        </property>
        <!-- 指定 Hadoop 运行时产生文件的存储目录 -->
        <property>
        <name>hadoop.tmp.dir</name>
        <value>file:/home/hadoop/hadoop-2.7.2/tmp</value>
        </property>
    </configuration>
```
修改`hdfs-site.xml`
```xml
    <configuration>
        <property>
        <name>dfs.namenode.secondary.http-address</name>
        <value>slave2:9001</value>
        </property>
        <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:/home/hadoop/hadoop-2.7.2/dfs/name</value>
        </property>
        <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:/home/hadoop/hadoop-2.7.2/dfs/data</value>
        </property>
        <property>
        <name>dfs.replication</name>
        <value>3</value>
        </property>
    </configuration>
```
修改`mapred-site.xml`
```xml
    <configuration>
        <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
        </property>
    </configuration>
```
修改`yarn-site.xml`
```xml
    <configuration>
        <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
        </property>
        <!-- 指定YARN 的 ResourceManager 的地址 -->
        <property> 
        <name>yarn.resourcemanager.hostname</name> 
        <value>master</value>
        </property>
    </configuration>
```
将配置好的hadoop-2.7.2文件夹分发给所有slaves节点
```dart
    scp -r ~/hadoop-2.6.0 hadoop@slave1-ubuntu:~/
```
启动 Hadoop  
在 master节点上执行以下操作，就可以启动 hadoop 了。  
`cd ~/hadoop-2.7.2 #进入hadoop目录`
```bash
    bin/hadoop namenode -format    #格式化namenode
```
**注：若格式化之后重新修改了配置文件，重新格式化之前需要删除tmp，dfs，logs文件夹。**
```bash
    sbin/start-dfs.sh              #启动dfs 
    sbin/start-yarn.sh              #启动yarn
```
验证 Hadoop 是否安装成功，可以通过jps命令查看各个节点启动的进程是否正常。
在 master 上应该有以下几个进程：
* NameNode
* SecondaryNameNode
* DataNode
* NodeManager
* ResourceManager

在每个slave上应该有以下几个进程：
* NodeManager
* DataNode

在浏览器中输入  [http://fang-ubuntu:8088](https://link.jianshu.com/?t=http://fang-ubuntu:8088)  ，可以看到hadoop 的管理界面。
## 3. 配置 Spark
### 3.1  配置 spark-env.sh文件
```bash
    cd ~spark-1.6.1-bin-hadoop2.6/conf    #进入spark配置目录
    cp spark-env.sh.template spark-env.sh  #从配置模板复制
    vim spark-env.sh    #添加配置内容
    在spark-env.sh末尾添加以下内容（可以自行修改）：
    export SPARK_HOME=/home/hadoop/spark-1.6.1-bin-hadoop2.6
    export SCALA_HOME=/home/hadoop/scala-2.10.6
    export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_77
    export HADOOP_HOME=/home/hadoop/hadoop-2.7.2
    export PATH=$PATH:$JAVA_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$SCALA_HOME/bin
    export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
    export YARN_CONF_DIR=$YARN_HOME/etc/hadoop
    export SPARK_MASTER_IP=218.199.92.227
    SPARK_LOCAL_DIRS=/home/haodop/spark-1.6.1-bin-hadoop2.6
    SPARK_DRIVER_MEMORY=1G
    export SPARK_LIBARY_PATH=.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib:$HADOOP_HOME/lib/native
```
**注：在设置Worker进程的CPU个数和内存大小，要注意机器的实际硬件条件，如果配置的超过当前Worker节点的硬件条件，Worker进程会启动失败。**
修改`slaves`文件
```
    slave1
    slave1
```
配置好的spark-1.6.1-bin-hadoop2.6文件夹分发给所有slaves吧
```dart
    scp -r ~/spark-1.6.1-bin-hadoop2.6 hadoop@slave1:~/
```
启动Spark
```
    sbin/start-all.sh
```
验证 Spark 是否安装成功
主节点上启动了Master进程：
* NameNode
* SecondaryNameNode
* DataNode
* NodeManager
* ResourceManager
* Master

在 slave 上启动了Worker进程：
* NodeManager
* Worker
* DataNode

进入Spark的Web管理页面：http://master:8080

## 4 运行实例
以集群模式运行SparkPi实例程序(deploy-mode 设置为cluster)
```kotlin
./bin/spark-submit 
--class org.apache.spark.examples.SparkPi
 --master yarn --deploy-mode cluster 
--driver-memory 1G --executor-memory 1G 
--executor-cores 1 lib/spark-examples-1.6.1-hadoop2.6.0.jar 40
```

注意 Spark on YARN 支持两种运行模式，分别为yarn-cluster和yarn-client，yarn-cluster适用于生产环境；而yarn-client适用于交互和调试，因为能在客户端终端看到程序输出。客户端模式实例和上面集群模式运行过程类似。
