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
docker run -it --name centos01 centos /bin/bash
docker run -it --name centos01 centos bash # 也行
exit #退出

# 列出运行的容器
docker ps
docker ps -a # 列出正在和运行过的容器

# 退出容器
exit # 停止并退出

# 删除容器
docker rm 容器id

# 启动和停止容器
docker start 容器id
docker restart 容器id
docker stop/kill 容器id


# 其他常用命令
docker logs # 查看日志

# 查看容器内部进程
docker top
# 查看镜像的元数据
docker inspect 镜像id 

# 进入当前正在运行的容器
docker attach 容器名称

# 执行容器里的命令
docker exec 容器名称 command 
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
## 镜像原理
Docker镜像都是只读的，当容器启动时，一个新的的可写层被加载到镜像的顶部。

## commit镜像
```bash
docker commit
docker commit -m="描述信息" -a="作者" 容器id， 目标镜像名: [Tag]

## 实例
docker commit -a="wei" -m="add something" 容器id tomcat02:01
```

## 容器数据卷
将应用和环境打包成一个镜像。如果数据都在容器中，那么容器删除，数据就会丢失！需求：数据可以持久化。

容器之间可以有一个数据共享的技术，Docker容器中产生的数据，同步到本地。将容器内的目录，挂载到linux上面。

### 使用数据卷
```bash
方式一：直接使用命令来挂载 -v
# 1. 指定本机和容器的具体路径
docker run -it -v 主机目录：容器目录

# 例子
docker run -it -v /home/test:/home centos /bin/bash

docker inspect 容器id # 查看mount信息

# msyql
docker pull mysql
docker run -d -p 3310:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:latest
docker rm -f mysql101

## 2. 匿名挂载
docker run -d -p --name nginx01 -v /etc/ngnix ngnix

## 3. 具名挂载 指定卷名(常用) 
docker run -d -p --name ngnix02 -v juming-ngnix:/etc/ngnix ngnix
# 查看所有volume情况
docker volume ls
# 查看某个volume
docker volume inspect volume名称

# 所有docker内的卷没有指定本机目录的情况下都是在/var/lib/docker/volumes/xxx/

# ro 只读，rw可读可写 对容器来说
docker run -d -p --name ngnix02 -v juming-ngnix:/etc/ngnix:ro ngnix
docker run -d -p --name ngnix02 -v juming-ngnix:/etc/ngnix:rw ngnix
```

# Dockerfile
Dockerfile是用来构建镜像的文件
```shell
FROM        # 基础镜像
MAINTAINER  # 镜像作者
RUN         # 镜像构建的时候需要的命令
ADD         # 添加内容，例如添加jdk压缩包，tomcat压缩包，hadoop压缩包，并且会自动解压
WORKDIR     # 镜像的工作目录, 指定进入镜像所在的目录
VOLUME      # 挂载的目录
EXPOSE      # 对外端口
CMD         # 指向容器启动时要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT  # 同CMD
ONBUILD     # 当构建一个被继承 Dockerfile 这个时候就会运行ONBUILD的指令
COPY        # 类似ADD，将文件拷贝到镜像中，拷贝README.md
ENV         # 构建的时候设置环境变量

# 实例
FROM centos
MAINTAINER alex<alexander90@qq.com>
ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yun -y install net-tools
EXPOSE 80
CMD echo $MYPATH
CMD /bin/bash
```
构建命令
```shell
docker build -f mydockerfile-centos -t mycentos:0.1 .

#查看镜像构建步骤
docker history 镜像id
```
## 数据卷容器
实现多个容器之间数据共享
```shell
docker run -it --name docker02 --volumes-from docker01 镜像名称
# 即使原始的数据卷删除了，共享数据依然存在
```

# 发布自己的镜像
1) dockerhub上注册自己的账号
2) docker命令登录
```shell
docker login --help

docker login -u username

# 提交
docker push 镜像名
# 备份
docker save
docker load
```

# Docker网络
evth-pair技术

--link可以使容器之间通过容器名连通，但是反向就不行

--link其实就是在hosts文件中设置了IP地址映射，不建议使用

`docke network ls`查看所有网络
## 网络模式
- bridge    桥接docker
- none      不配置网络
- host      和主机共享网络
- container 用的少
## 创建自定义网络
```shell
docker network create --driver bridge --subset 192.168.0.0/16 --gateway 192.168.0.1 mynet
docker network inspect mynet 

## 使用自定义网络
docker run -d -P --name tomcat-net-01 --net mynet tomcat
```
> 使用自定义网络可以通过docker名称互相通信！建议使用
## 网络连通
使用`docker network connect`命令
```shell
docker network connect mynet tomcat01
# 实际上就是将tomcat01放在了mynet网络下
docker network inspect mynet
```
