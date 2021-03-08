# 1. ubuntu
## 问题
1) 没有datanode
删除data文件夹里的current文件，再`bin/hdfs namenode -format`
2) 注意删除//etc/hosts文件里第二行
设置root密码:
```bash
sudo passwd //按回车
```
3）lzo没成功
4) 移除openjdk
5) 移除组
6) 
## [虚拟机设置静态ip](https://blog.csdn.net/qq_40824474/article/details/82749007)
1) 编辑-虚拟网络编辑器-更改设置 设置子网ip, 子网掩码, 网关(地址可随意)
2) 控制面板-网络连接-VMnet8-属性 ipv4设置，设置的地址和上面的一样，DNS地址和网关地址一样
3) 20.4 server 设置静态ip：
    1) `sudo vim /etc/netplan/***.yaml`
       ```bash
       network:
           ethernets:
               enp0s3:
                   dhcp4: false
                   addresses: [192.168.1.202/24]
                   gateway4: 192.168.1.1
                   nameservers:
                     addresses: [8.8.8.8,8.8.4.4,192.168.1.1]
           version: 2
       ```
    2) `sudo netplan apply`
    3) `ip a` `ping www.baidu.com`测试
## 修改主机名
1) `sudo hostnamectl set-hostname newNameHere`
2) `sudo nano /etc/hosts` 
3) `hostnamectl`

## 关闭防火墙
关闭防火墙不是必须的，只是为了减轻不同节点一直需要通过防火墙通信而造成的负担，所以最好关闭。

ubuntu默认的防火防火墙叫做`ufw`,其默认是关闭的
- 查看状态 `sudo ufw status`
- 开启
   1) `sudo ufw allow ssh/tcp` 或 `sudo ufw allow 22`  只打开使用tcp/ip协议的22端口/打开SSH服务器的22端口
   2) `sudo ufw enable`
   3) `sudo ufw disable`
## 设置远程登录和免密登录
1) `ps -ef | grep ssh`查看是否安装openssh-server，若没安装执行2
2) `sudo apt install openssh-server`
3) `ssh-keygen -t rsa`
```bash
ssh-copy-id hadoop1
ssh-copy-id hadoop2
ssh-copy-id hadoop3
```
> 在hadoop1, hadoop2配置免密登录就行了
### root用户不能ssh登录问题
1) `sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config`
2) 或者 `vim /etc/ssh/sshd_config`
```shell
#PermitRootLogin prohibit-password
PermitRootLogin yes #加入这一句
```
3) `sudo service ssh restart`
## 将用户赋予root权限
1) `sudo visudo`
2) `alex    ALL=(ALL:ALL) NOPASSWD:ALL`

[用户、用户组管理](https://blog.csdn.net/freeking101/article/details/78201539) 不用在这里设置

## 建立文件夹
1) `mkdir /opt/software /opt/module`
2) `chown alex:alex /opt/module/ /opt/software/`

> 以上命令操作均在root用户下进行

## JDK配置
1) `sudo vim /etc/profile.d/my_env.sh`
```shell
#JAVA_HOME
export JAVA_HOME=/opt/module/jdk-11.0.10
export PATH=$PATH:$JAVA_HOME/bin
```
2) `source /etc/profile.d/my_env.sh`
3) `java -version`

## 分发脚本
1) 检查bin目录是否在环境变量中
2) `vim ~/bin/xsync`
```shell
#!/bin/bash
#1. 判断参数个数
if [ $# -lt 1 ]
then
  echo Not Enough Arguement!
  exit;
fi
#2. 遍历集群所有机器
for host in hadoop1 hadoop2 hadoop3
do
  echo ====================  $host  ====================
  #3. 遍历所有目录挨个发送
  for file in $@
  do
    #4 判断文件是否存在
    if [ -e $file ]
    then
      #5. 获取父目录
      pdir=$(cd -P $(dirname $file); pwd)
      #6. 获取当前文件的名称
      fname=$(basename $file)
      ssh $host "mkdir -p $pdir"
      rsync -av $pdir/$fname $host:$pdir
    else
      echo $file does not exists!
    fi
  done
done
```
3) `chmod +x xsync`
4) `xsync /opt/module/jdk-11.0.10/` 要设置免密登录
5) `sudo /home/alex/bin/xsync /etc/profile.d/my_env.sh`
6) 要在每台机器上执行`source /etc/profile.d/my_env.sh`

## 集群日志生成脚本
1) 需要在`~/.bashrc`文件里设置JDK环境变量，然后分发到每个机器上, 然后`source .bashrc`生效。因为`ssh *@* 'command'`的环境变量不一样
>  或者像这样`ssh hadoop2 "bash --login -c 'java -version'"`
2) `vim ~/bin/log.sh`
```shell
#!/bin/bash
for i in hadoop102 hadoop103; do
    echo "========== $i =========="
    ssh $i "cd /opt/module/applog/; java -jar gmall2020-mock-log-2020-05-10.jar >/dev/null 2>&1 &"
done 
```
3) `chmod +x log.sh`

hadoop3引入了磁盘间均衡
## Zookeeper
1) 新建zkData/myid, id必须是1~255
2) 修改zoo.cfg
3) `bin/zkCli.sh` 查看zookeeper文件系统  `quit`退出
4) `ls /kafka/brokers/ids` 查看节点情况
## Kafka
1) 修改server.properties
2) 启动时先启动zookeeper，关闭时先关闭kafka
## flume
1) `rm lib/guava-11.0.2.jar`, 保证`HADOOP_HOME`环境变量已配置
2) 修改`conf/flume-env.sh`
### flume消费kafka，把数据放到hdfs上
1) 如何让flume可以按日期将数据放在hdfs不同的文件夹里?  使用拦截器，根据数据的时间戳分类
2) 前面用的时memory channel，现在用的时file channel
> 问题 几种channel的适用场景？
> FileChannel也有内存队列，但是它存的不是event(memory channel)，而是文件里数据的索引
> 为了防止机器挂了而引起索引的丢失，需要定期将内存队列的数据作快照
> source采集数据，先将数据写到put list中，写完一批，提交put事务，放到channel中

## Sqoop
### 简介
Sqoop是用来在hadoop和关系数据库之间转移数据的工具。
### 安装
1) `cp sqoop-env-template.sh sqoop-env.sh`
2) `vim sqoop-env.sh`
```shell
export HADOOP_COMMON_HOME=/opt/module/hadoop-3.1.3
export HADOOP_MAPRED_HOME=/opt/module/hadoop-3.1.3
export HIVE_HOME=/opt/module/hive
export ZOOKEEPER_HOME=/opt/module/zookeeper-3.5.7
export ZOOCFGDIR=/opt/module/zookeeper-3.5.7/conf
```
3) 拷贝java-mysql连接jar包至sqoop的lib文件夹下
4) 测试：`bin/sqoop list-databases --connect jdbc:mysql://hadoop102:3306/ --username root --password 000000`
5) 从Mysql导数据至HDFS
```shell
bin/sqoop import \
--connect jdbc:mysql://hadoop102:3306/gmall \
--username root \
--password 000000 \
--table user_info \
--columns id,login_name \
--where "id >= 10 and id <= 30" \
--target-dir /test \
--delete-target-dir \
--num-mappers 1 \
--fields-terminated-by '\t' \
--split-by id
```
或者
```shell
bin/sqoop import \
--connect jdbc:mysql://hadoop102:3306/gmall \
--username root \
--password 000000 \
--query "select id, login_name from user_info where id >= 10 and id <= 30 and \$CONDITIONS" \
--target-dir /test \
--delete-target-dir \
--num-mappers 1 \
--fields-terminated-by '\t' \
--split-by id
```
Sqoop可以将数据从数据库导入HDFS, Hive, HBase, 但是只能将数据从HDFS到入数据库
# 疑难杂症
1) [群起flume时java.lang.ClassNotFoundException: com.google.common.collect.Lists](https://stackoverflow.com/questions/64519857/why-flume-failed-to-run-with-the-startup-script)
2) [flume java.lang.OutOfMemoryError: Java heap space](https://blog.csdn.net/weixin_46122692/article/details/110200293)
3) [Failed to initialize Log on [channel=c1]](https://serverfault.com/questions/690588/flume-error-log-while-using-filechannel): 删除data和checkpoint文件夹
4) flume输出到kafka的每条消息前乱码？: `a1.channels.c1.parseAsFlumeEvent = false:`
5) [flume输出到hdfs java.lang.NoClassDefFoundError: org/apache/hadoop/conf/Configuration](https://blog.csdn.net/windavi/article/details/106699432)
# 数仓理论
## 建仓步骤
### 1. OSD层
1) 保持数据原貌不做任何修改，起到备份数据的作用。比如日志数据，只建一个日志表，其中只有一列属性，就是每条日志类容。保持原始数据内容和结构不改变。
2) 数据采用压缩，减少磁盘存储空间（例如：原始数据100G，可以压缩到10G左右）
3) 创建分区表，防止后续的全表扫描
### 2. DWD层
1) 对于日志，要解析每条日志的字符串，将其转为字段
2) 对于业务数据，要进行维度建模，将关系模型转化为维度模型。也就是明确要建哪些事实表和维度表，哪个维度表和事实表是有关联的。明确事实表和维度表有哪些字段。
DWD层需构建维度模型，一般采用星型模型，呈现的状态一般为星座模型。

维度建模一般按照以下四个步骤：
选择业务过程→声明粒度→确认维度→确认事实
1) 选择业务过程  
在业务系统中，挑选我们感兴趣的业务线，比如下单业务，支付业务，退款业务，物流业务，一条业务线对应一张事实表。
如果是中小公司，尽量把所有业务过程都选择。
如果是大公司（1000多张表），选择和需求相关的业务线。

2) 声明粒度  
数据粒度指数据仓库的数据中保存数据的细化程度或综合程度的级别。
声明粒度意味着精确定义事实表中的一行数据表示什么，应该尽可能选择最小粒度，以此来应各种各样的需求。
典型的粒度声明如下：
订单事实表中一行数据表示的是一个订单中的一个商品项。
支付事实表中一行数据表示的是一个支付记录。
3) 确定维度  
维度的主要作用是描述业务是事实，主要表示的是“谁，何处，何时”等信息。
确定维度的原则是：后续需求中是否要分析相关维度的指标。例如，需要统计，什么时间下的订单多，哪个地区下的订单多，哪个用户下的订单多。需要确定的维度就包括：时间维度、地区维度、用户维度。
4) 确定事实  
此处的“事实”一词，指的是业务中的度量值（次数、个数、件数、金额，可以进行累加），例如订单金额、下单次数等。
在DWD层，以业务过程为建模驱动，基于每个具体业务过程的特点，构建最细粒度的明细层事实表。事实表可做适当的宽表化处理。

事实表和维度表的关联比较灵活，但是为了应对更复杂的业务需求，可以将能关联上的表尽量关联上。如何判断是否能够关联上呢？在业务表关系图中，只要两张表能通过中间表能够关联上，就说明能关联上。
