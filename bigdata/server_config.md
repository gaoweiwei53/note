# 1. ubuntu
设置root密码:
```bash
sudo passwd //按回车
```
## 虚拟机设置静态ip
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
