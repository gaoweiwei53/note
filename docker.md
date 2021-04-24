## 1. 简介
## 镜像image
镜像就是一个模板，可以通过这个模板来创建容器服务。
## 容器container
Docker利用容器技术，独立运行一个或几个应用。通过镜像创建。
## 仓库repository
仓库就是存放镜像的地方。

## 2. 常用命令
### 帮助命令
```bash
docker version
docker info
docker command --help
```
### 镜像命令
```bash
docker images
docker search mysql
docker pull mysql # 下载镜像，默认下载最新版本
docker rmi # 删除镜像
docker rmi -f $(docker images -aq) # 删除全部的容器
```
### 容器命令
有了镜像才可以创建容器，下载一个centos镜像来测试学习`docker pull centos`
```bash
docker run [可选参数] image # 启动镜像
# 参数说明
--name="name" # 容器名字
-d            #后台运行
-it           # 使用交互方式运行
-p            # 指定容器端口
# 测试
docker run -it centos /bin/bash
exit #退出

# 列出运行的容器
docker ps
dcoker ps -a # 列出正在和运行过的容器

# 退出容器
exit # 停止并退出
Ctrl + P + Q # 容器不停止退出

# 删除容器
docker rm 容器id

# 启动和停止容器
docker start 容器id
docker restart 容器id
docker stop 容器id
docker kill 容器id

# 其他常用命令
docker logs # 查看日志

# 查看容器内部进程
docker top
# 查看镜像的元数据
docker inspect 镜像id 

# 进入当前正在运行的容器
## 方式1 进入容器后开启一个新的终端，可以在里面操作(常用)
docker exec -it 容器id /bin/bash
## 方式2 进入容器正在执行的终端
docker attach -it 容器id 

# 从容器中拷贝文件到主机上
docker cp 容器id：容器内路径 目的主机的路径
```
## 命令实例
```bash
docker pull ngnix
docker run -d --name ngnix01 -p 3344:80 ngnix
docker ps
curl localhost:3344
docker exec -it nginx01 /bin/bash
whereis ngnix
```
