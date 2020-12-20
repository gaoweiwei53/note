# 1 [Ubuntu 安装mysql](https://www.zhihu.com/search?type=content&q=mysql%20ubuntu)
### 方法1：从ubuntu仓库中安装
```bash
sudo apt update
sudo apt install mysql-server -y
```
### 方法2: 使用官方仓库安装 MySQL
```bash
curl -OL https://dev.mysql.com/get/mysql-apt-config_0.8.12-1_all.deb
sudo dpkg -i mysql-apt-config*
sudo apt update
sudo apt install mysql-server -y
```
### 验证是否正确安装成功
```bash
sudo systemctl status mysql.service # 直接使用mysql也行
```
### 启动mysql服务
```bash
sudo systemctl start mysql.service  # 直接使用mysql也行
```
### 配置/保护 MySQL(可选)
```bash
sudo mysql_secure_installation
```
### 连接操作
```bash
mysql -h host_name -u user -p
```
# 指令
|指令 |功能|
|----|----|
|||
