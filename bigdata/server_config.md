# 1. ubuntu
设置root密码:
```bash
sudo passwd //按回车
```

20.4 server 设置静态ip：
1) vim编辑`sudo vim /etc/netplan/***.yaml`
2) 编辑
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
3) `sudo netplan apply`
