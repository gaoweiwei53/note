# Principle
1. 装系统使用 GPT + UEFI 方式，文件系统使用NTFS。装机时Bios禁用 secure boot 和 fast boot。打开quick boot，可加快每次开机速度。装机后可打开fast boot，但这只能加快win8和win10的开机速度。
2. `sudo passwd root`设置root密码
3. 若在图形界面下，在 *.bashrc* 里设置环境变量

# Linux Command
1. `find`:查找指定文件下和子文件夹下的文件
2.  `find / -name sth`: 查找文件
3.  `free -h`/`top`: 查看内存使用
4. swap分区的作用是，当系统物理内存吃紧时，Linux 会将内存中不常访问的数据保存到 swap 上，这样系统就有更多的物理内存为各个进程服务，而当系统需要访问 swap 上存储的内容时，再将 swap 上的数据加载到内
5. `lsof -i:端口号` 列出谁在使用这个端口，如果没有任何输出就没有开启？ 
6. `netstat -nl`： 只显示监听端口 `netstat -aptn`查看所有端口，`netstat -nupl`查看所有udp端口号, `netstat -ntpl`查看所有tcp端口
7. `top + 大M(shift m)` 按内存使用降序排列
8. `ps -eo pid,ppid,%mem,%cpu,cmd --sort=-%mem | head`, `ps -eo pid,ppid,%mem,%cpu,comm --sort=-%mem| head -2`, `-e`选择所有进程，`-o`自定义输出格式 
9. linux查看线程：`top -H`, `top -H -p <pid>`, `ps -T -p <pid>`
10. 查看目录空间占用大小：`du -sh`
11. 创建软连接：`ln -s 源文件 链接文件`
12. 免密登录：将A主机的id_rsa.pub文件里的内容拷贝到主机B的authorized_keys中:`ssh-keygen -t rsa`, `ssh-copy-id 用户名@主机B`
13. `telnet ipaddress port`当某个端口在被使用时，可连接成功
14. `ss -tnp`：列出tcp连接极其对应的进程
# 软件
- [JDK8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
- [JDK11](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html)
- [IntelliJ IDEA](https://www.jetbrains.com/idea/)
- [Anaconda](https://www.anaconda.com/products/individual)
- [Scala](https://scala-lang.org/download/)
- [Panda](https://www.panhdpe.xyz/download)
- [Apache Spark](https://mirrors.tuna.tsinghua.edu.cn/apache/spark/)
- [Apache Flink](https://mirrors.tuna.tsinghua.edu.cn/apache/flink/)
- [Apache Kafka](https://mirrors.tuna.tsinghua.edu.cn/apache/kafka/)
- [Apache Hadoop](https://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/core/)
- [MySQL](https://dev.mysql.com/downloads/mysql/)
- [坚果云](https://www.jianguoyun.com/s/downloads)
## Windows
- [Xodo](https://www.xodo.com/#)
- [百度网盘](https://pan.baidu.com/download/)
- [VLC](https://www.videolan.org/vlc/)
- [Bandizip](https://www.bandizip.com/)
- [Notion](https://www.notion.so/desktop)
