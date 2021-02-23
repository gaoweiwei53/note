# 1. ubuntu
设置root密码:
```bash
sudo passwd //按回车
```
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
## 设置远程登录
1) `ps -ef | grep ssh`查看是否安装openssh-server，若没安装执行2
2) `sudo apt install openssh-server`
3) 
## 将用户赋予root权限
1) `sudo visudo`
2) `alex    ALL=(ALL:ALL) NOPASSWD:ALL`

[用户、用户组管理](https://blog.csdn.net/freeking101/article/details/78201539) 不用在这里设置

## 建立文件夹
1) `mkdir /opt/software /opt/module`
2) `chown alex:alex /opt/module/ /opt/software/`

> 以上命令操作均在root用户下进行
