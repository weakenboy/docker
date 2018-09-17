# Docker

## 1、Docker简介

### 1.1 使用Docker容器开发、测试、部署

### 1.2 创建隔离的运行环境

###1.3 搭建测试环境 

### 1.4 构建多用户的平台即服务（PaaS）基础设施

### 1.5 提供软件即服务（SaaS）应用程序

### 1.6 高性能、超大规模的宿主机部署

## 2、Docker的基本组成

###2.1 Docker Cient 客户端

###2.2 Docker Daemon 守护进程

C/S架构      本地/远程

###2.3 Docker Image 镜像

容器的基石

层叠的只读文件系统

联合加载（union mount）

项目的构件和打包阶段

###2.4 Docker Container 容器

通过镜像启动

项目的启动和执行阶段

写时复制

###2.5 Docker Registry 仓库

公有——DockerHub

私有——自行搭建

## 3、Boot2Docker——windows环境下Docker配置

## 4、容器

###4.1 容器的基本操作

```docker
docker run -i -t IMAGE /bin/bash
建立并启动容器
    -i——用于交互式容器
    -t——提供终端用于交互

docker ps -a -l
查看容器（没有参数的时候查看正在运行的容器）
    -a——查看所有容器
    -l——查看最新容器

docker inspact
查看容器的内容

docker run -name=新容器名 -i -t IMAGE /bin/bash
容器建立、运行并重命名

docker start -i
重新启动已关闭的容器
	-i——交互模式

docker rm
删除容器
```



### 4.2 守护式容器

```docker
运行交互式容器时
    Ctrl+P 
    Ctrl+Q 
可以使容器在后台运行

再次进入已经运行中的容器
	docker attach 容器名

启动守护式容器
	docker run -d 镜像名[COMMAND][ARG]

查看容器日志
docker logs [-f][-t][--tail]容器名
    -f 一直跟踪容器运行的结果
    -t 是否添加时间戳
    --tail 返回结尾处多少数量的日志

查看运行中容器的进程
	docker top 容器名

运行的容器中启动新的进程
	docker exec [-d][-i][-t] 容器名[COMMAND][ARG]

停止守护式容器
    docker stop 容器名
    docker kill 容器名

```

###4.3 容器的端口映射

```docker
run [-P][-p]
    -P 为容器暴露的所有端口进行映射
    示例：docker run -P -i -t ubuntu /bin/bash
    -p 指定一个端口进行映射
指定容器的端口：
	docker run -p 80 -i -t ubuntu /bin/bash
同时指定宿主机的端口和容器的端口：
	docker run -p 8080:80 -i -t ubuntu /bin/bash
指定ip和容器的端口：
	docker run -p 0.0.0.0:80 -i -t ubuntu /bin/bash
指定ip、宿主机端口、容器端口：
	docker run -p 0.0.0.0:8080:80 -i -t ubuntu /bin/bash
```

## 5、镜像

### 5.1 查看和删除镜像

```docker
列出镜像
docker images [OPTSIONS] [REPOSITORY]
    -a 显示所有镜像
    -f 过滤条件
    --no-trunc 不使用截断的形式显示数据
    -q 只显示镜像的唯一id

查看镜像
docker inspect [OPTIONS]CONTAINER|IMAGE[CONTAINER|IMAGE...]
	-f 格式化信息

删除镜像
docker rmi [OPTIONS] IMAGE [IMAGE...]
    -f 强制删除镜像
    --no-prune 会保留被删除的镜像的父镜像

```

### 5.2 获取与推动镜像

```docker
查找镜像
dockerhub查找
docker search [OPTIONS] TERM
    --automated 只会显示自动化构建的镜像
    --notrunc 不以截断的形式显示输出
    -s 显示结果的最低星级
    一页只会显示25个结果

拉取镜像
docker pull [OPTIONS] NAME[:TAG]
-a 将所有匹配tag的镜像拉取到本地

镜像加速
使用--registry-mirror选项
    1、修改：/etc/default/docker
    2、添加：DOCKER_OPTS="--registry-mirror=http://MIRROR-ADDR"
    https://www.daocloud.io 
	
推送镜像
	docker push

```

### 5.3 构建镜像

```docker
docker commit 通过容器构建
    docker commit [OPTIONS]CONTAINER [REPOSITORY[TAG]]
    -a,--author="" 指定作者
    -m,--message="" 记录镜像构建的信息
    -p,--pause=true 指示不暂停当前运行的容器

docker build 通过Dockerfile文件构建
	1、创建Dockerfile文件
	2、使用docker build 命令
		docker build [OPTIONS] PATH | URL |-
		--force-rm=false
		--no-cache=false
		--pull=flase
		-q,--quiet=false
		--rm=true
		-t,--tag=""
	
```

```Dockerfile
#FirstDockerfile
FROM ubuntu:14.04
MAINTAINER authorname "email-address"
RUN apt-get update
RUN apt-get install -y nginx
EXPOSE 80
```

## 6、Docker守护进程的配置和操作

```docker
查看守护进程
ps -ef|grep docker
sudo status docker

使用service命令管理
sudo service docker start
sudo service docker stop
sudo service docker restart

docker的启动选项
docker -d [OPTIONS]
	-D,--debug=false 调试模式
	-e,--exec-driver="native" 运行时的驱动模式
	-g,--graph="/var/lib/docker"
	--icc=true
	-l,--log-level="info" 日志级别
	--lable=[]
	-p,-pidfile="/var/run/docker.pid" 进程id写入文件的地址
	Docker服务器连接相关
	-G,,--group="docker"
	-H,--host=[]
	--tls=false
	--tlscacert="/home/sven/.docker/ca.pem"
	--tlscert="/home/sven/.docker/cart.pem"
	--tlskey="/home/sven/.docker/key.pem"
	--tlsverify=false
	RemoteAPI相关
	--api-enable-cors=false
	存储相关
	-s,--storage-driver=""
	--selinux-enable=false
	--storage-opt=[]
	Registry相关
	--insecure-registry=[]
	--registry-mirror=[]
	网络设置相关
	-b,--bridge=""
	--bip=""
	--fixed-cidr=""
	--fixed-cidr-v6=""
	--dns=[]
	--dns-search=[]
	--ip=0.0.0.0
	--ip-forward=true
	--ip-masq=true
	--iptables=true
	--ipv6=false
	--mtu=0
```

docker启动配置文件

​	/etc/default/docker

## 7、Docker的远程访问

修改Docker守护进程启动选项

```docker
-H  tcp://host:port 通常会使用2375端口进行服务
    unix:///path/to/socket
    fd://* or fd://socketfd

export DOCKER_HOST="tcp://host:port" 环境变量
```

守护进程默认配置

```docker
-H unix:///var/run.docker.sock
```

使用守护进程默认配置文件修改时，tcp启动-H tcp://0.0.0.0:2375进行服务就是开启了远程服务，远程服务开启会与本机服务互斥

所以还要再添加-H unix:///var/run.docker.sock开启本机服务

## 8、Dockerfile指令

### FROM

```dockerfile
FROM <image>
FROM <image>:<tag>
```

导入已经存在的镜像

后续指令都会根据这个镜像来执行

必须是第一条非注释指令

### MAINTAINER

指定镜像作者信息，包含镜像所有者和联系信息

### RUN

指定当前镜像中运行的命令

```dockerfile
RUN <command> (shell模式)
RUN ["executable","param1","param2"](exec模式)
```

###EXPOSE

```dockerfile
EXPOSE <port>[<port>...]
```

指定运行该镜像的容器使用的端口

### CMD

```dockerfile
CMD ["executable","param1","param2"](exec模式)
CMD command param1 param2(shell模式)
CMD ["param1","param2"](作为ENTRYPOINT指令的默认参数)
```

### ENTRYPOINT

```dockerfile
ENTRYPOINT ["executable","param1","param2"](exec模式)
ENTRYPOINT command param1 param2(shell模式)
```

###ADD

```dockerfile
ADD <src>...<dest>
ADD ["<src>"..."<dest>"](适用于文件路径有空格的情况)
自带tar解压功能
```

###COPY

```dockerfile
COPY <src>...<dest>
COPY ["<src>"..."<dest>"](适用于文件路径有空格的情况)
单纯复制最好使用copy
```

### VOLUME  [data]

用于给容器添加卷

一个卷可以给一个或者多个容器提供一个共享数据、持久化的功能

###WORKDIR /path/to/workdir

给文件设置工作目录

### ENV

用于设置环境变量，和workdir类似

```dockerfile
ENV <key><value>
ENV <key>=<value>
```

### USER deamon

用于指定容器以什么用户运行

```dockerfile
USER user
USER uid
USER user:group
USER user:gid
USER uid:gid
USER uid:group
默认 root
```

### ONBUILD

作为镜像触发器

当一个镜像被其他镜像作为基础镜像时执行

## 9、容器间的链接

推荐使用

--link CONTAINER

## 10、Docker与外部网络链接

--ip_forword

--iptables

与linux内核集成的包过滤防火墙系统

## 11、Docker容器的数据管理

###11.1 Docker容器的数据卷



###11.2 Docker的数据卷容器



###11.3 Docker数据卷的备份和还原