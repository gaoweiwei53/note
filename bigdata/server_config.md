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
# 疑难杂症
1) [群起flume时java.lang.ClassNotFoundException: com.google.common.collect.Lists](https://stackoverflow.com/questions/64519857/why-flume-failed-to-run-with-the-startup-script)
2) [flume java.lang.OutOfMemoryError: Java heap space](https://blog.csdn.net/weixin_46122692/article/details/110200293)
3) [Failed to initialize Log on [channel=c1]](https://serverfault.com/questions/690588/flume-error-log-while-using-filechannel): 删除data和checkpoint文件夹
4) flume输出到kafka的每条消息前乱码？: `a1.channels.c1.parseAsFlumeEvent = false:`
