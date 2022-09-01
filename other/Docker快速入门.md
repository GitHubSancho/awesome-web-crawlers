# Docker快速入门
## Docker简介和安装
### Docker概述
- Docker 是一个应用打包、分发、部署的工具
- 一个轻量的虚拟机，它只虚拟你软件需要的运行环境
### 主要功能
- 打包：把软件运行所需的依赖、第三方库、软件组合成一个安装包
- 分发：可以把打包好的“安装包”上传到一个镜像仓库，其他人可以非常方便的获取和安装
- 部署：用“安装包”就可以模拟出一致的运行环境（跨平台）
### 应用场景
- 应用分发、部署，方便传播给他人安装。特别是开源软件和提供私有部署的应用
- 快速安装测试/学习软件，用完就丢（类似小程序），不把时间浪费在安装软件上。例如 Redis / MongoDB / ElasticSearch / ELK
- 多个版本软件共存，不污染系统，例如 Python2、Python3，Redis4.0，Redis5.0
- Windows 上体验/学习各种 Linux 系统
### 重要概念
- 镜像：可以理解为软件安装包，可以方便的进行传播和安装。
- 容器：软件安装后的状态，每个软件运行环境都是独立的、隔离的，称之为容器。
### 安装
- 桌面版：https://www.docker.com/products/docker-desktop
- 服务器版：https://docs.docker.com/engine/install/#server
- 安装教程：https://www.runoob.com/docker/windows-docker-install.html
- 镜像仓库：https://hub.docker.com
```
镜像加速源
Docker 中国官方镜像	https://registry.docker-cn.com
DaoCloud 镜像站	http://f1361db2.m.daocloud.io
Azure 中国镜像	https://dockerhub.azk8s.cn
科大镜像站	https://docker.mirrors.ustc.edu.cn
阿里云	https://<your_code>.mirror.aliyuncs.com
七牛云	https://reg-mirror.qiniu.com
网易云	https://hub-mirror.c.163.com
腾讯云	https://mirror.ccs.tencentyun.com
```
## 常用命令
### daemon
- 启动docker：`systemctl start docker`
- 查看状态：`systemctl status docker`
- 停止docker：`systemctl stop docker`
- 重启docker：`systemctl restart docker`
- 开机启动docker：`systemctl enable docker`
### image
- 查看本地镜像：`docker images`
- 搜索镜像：`docker search [imageName]`
- 拉取镜像：`docker pull [imageName:tag]`
- 删除镜像：`docker rmi [imageID]`
### container
- 查看运行中的容器：`docker ps`
    - `-a`：查看所有容器
- 创建容器: `docker run -it --name [containerName] [imageName:tag] [可选初始化指令，如：/bin/bash]`
    - `-i`：保持运行
    - `-t`：分配终端
    - `-d`：后台运行
- 进入容器:`docker exec -it [containerName] [可选初始化指令，如：/bin/bash]`
- 退出容器：`exit`
- 启动容器：`docker start [containerName]`
- 停止容器：`docker stop [containerName]`
- 删除容器：`docker rm [containerName]`
- 查看容器信息：`docker inspect [containerName]`
## 数据卷
### 数据卷概念
- 数据卷是宿主机中的一个目录或文件
- 当容器目录和数据卷目录绑定后，双方的修改会立即同步
- 数据卷的作用：容器数据持久化；外部机器和容器间接通信；容器之间的数据交换
### 配置数据卷
- 创建启动容器时使用`-v`参数：`docker run [...] -v [宿主机目录（文件）:容器内目录（文件）]`
    - 目录必须是绝对路径
    - 如果目录不存在，自动创建
    - 可以挂载多个数据卷
### 数据卷容器
- 创建数据卷容器：`docker run [...] -v [宿主机目录（文件）]`
- 挂载数据卷容器：`docker run [...] --volumes-from [cotainnerName]`
## 应用部署
### mysql
- 搜索镜像：`docker search mysql`
- 拉取镜像：`docker pull mysql:5.6`
- 创建容器：
```
# 在/root目录下创建mysql目录用于存储mysql数据信息
mkdir ~/mysql
cd ~/mysql

docker run -id \
-p 3307:3306 \
--name [containerName] \
-v $PWD/conf:/etc/mysql/conf.d \
-v $PWD/logs:/logs \
-v $PWD/data:var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=[password]
```
### redis
- 搜索镜像：`docker search redis`
- 拉取镜像：`docker pull redis:5.0`
- 创建容器：`docker run -id --name [containerName] -p 6379:6379 redis:5.0`
- 访问验证：`.\redis-cli.exe -h [host] -p 6379`

## 制作Docker镜像
### Docker镜像原理
- Docker镜像是由特殊的文件系统叠加而成
- 最底端是bootfs，并使用宿主机的bootfs
- 第二层是root文件系统rootfs，称为base image
- 然后网上可以叠加其它镜像文件
- 统一文件系统技术能够将不同的层整合成一个文件系统，为这些层提供一个统一的视角，这样隐藏了多层的存在，在用户的角度来看只存在一个文件系统
- 一个镜像可以放在另一个镜像的上边，位于下面的镜像称为父镜像，最底部的镜像称为基础镜像
- 当从一个镜像启动容器时，Docker会在最顶层加载一个读写文件系统作为容器
### Dockerfile概念
- 容器转为镜像(挂载目录不会复制)
    - `docker commit [容器id] [镜像名称:版本号]`
    - `docker save -o [压缩文件名称] [镜像名称:版本号]`
    - `docker load -i [压缩文件名称]`
- 常用关键字
    - `FROM`：指定父镜像
    - `MAINTAINER`：作者信息
    - `RUN`：执行命令，可接收列表
    - `CMD`：容器启动命令
    - `COPY`：复制文件
    - `ADD`：添加文件，不局限上下文，可来源远程服务
    - `VOLUME`：定义外部可挂载的数据卷
    - `EXPOSE`:暴露端口
    - `WORKDIR`:工作目录
- 构建镜像
    - `docker build -f [文件目录] -t [image:tag] .`
        - `-f`：指定文件的当前目录
        - `-t`：设置新镜像的名称和版本
## 服务编排
### 服务编排概念
- 微服务框架的应用系统中一般包含若干个微服务，每个微服务一般都会部署多个实例，如果每个微服务都要手动启停维护的工作量会增大
- Docker Compose是一个编排多容器分布式部署的工具，提供命令及管理容器化应用的完整开发周期，包括服务构建、启停
    1. 利用Dockerfile定义运行环境镜像
    2. 使用`docker-compose.yml`定义组成应用的各服务
    3. 运行`docker-compse up`启动应用
### Docker Compose使用
- 安装Dcoker Compose
    - 下载：`curl -L https://hub.0z.gs/docker/compose/releases/download/v2.10.2/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose`
    - 设置文件可执行权限：`chmod +x /usr/local/bin/docker-compose`
    - 查看版本信息：`docker-compose --version`
- 编排
    - 创建目录：`mkdir docker-compose && cd docker-compose/`
    - 写配置：`vim docker-compose.yml`
```
version:'3' #版本命名
service: #将启动的容器
    nginx: # 命名容器
        image: nginx
        ports: 
            - 80:80
        links: 
            - app
        volumes: 
            - ./nginx/conf.d:/etc/nginx/conf.d
    app: 
        image: app
        expose: 
            - "8080"
```
    - 创建映射目录：`mkdir -p ./nginx/conf.d && cd ./nginx/conf.d`
    - 写配置：`vim sancho.conf`
```
server {
    listen 80;
    access_log off;

    location / {
        proxy_pass http://app:8080;
    }
}
```
    - 回到docker-compose目录：`cd ../../`
    - 启动docker-compose：`docker-compose up`
## 私有仓库
### 搭建私有仓库
```
#1.拉取私有仓库镜像
docker pull registry
#2.启动私有仓库容器
docker run -id --name registry -p 5000:5000 registry
#3.http://浏览器输入私有仓库地址:5000/v2/_catalog
#4.修改deamon.json
vim /etc/docker/deamon.json
#添加以下key表示信任私有仓库地址
{"insecure-registries":["私有仓库地址:5000"]}
5.重启docker服务
systemctl restart docker
docker start registry
```
### 上传镜像到私有仓库
```
#1.标记镜像为私有仓库
docker tag centos:7 私有仓库地址:5000/centos:7
#2.上传标记镜像
docker push 私有仓库地址:5000/centos:7
```
### 从私有仓库拉取镜像
```
docker pull 服务器地址:5000/centos:7
```
