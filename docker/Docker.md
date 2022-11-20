# Docker

## 第一章 Docker 介绍 - 运维工具

### 一、Docker概述

java-->jar 运行开发环境 mysql5.7

jar--->生产环境 出错了！ mysql8

![image-20220801153532910](Docker.assets/image-20220801153532910.4ecac8d4.png)

Docker 是一个开源的**应用容器引擎**，基于 Go 语言开发。Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。

**Docker应用场景**

- Web 应用的自动化打包和发布
- 自动化测试和持续集成、发布
- 在服务型环境中部署和调整数据库或其他的后台应用

使用Docker可以实现开发人员的开发环境、测试人员的测试环境、运维人员的生产环境的一致性。

![image-20220801152413424](Docker.assets/image-20220801152413424.1a6ba897.png)

### 二、Docker组成部分

![image-20220801161305417](Docker.assets/image-20220801161305417.031d6da6.png)

| **名称**                        | **说明**                                                     |
| ------------------------------- | ------------------------------------------------------------ |
| Docker 镜像(Images)             | Docker 镜像是用于创建 Docker 容器的模板。 镜像是基于联合文件系统的一种层式结构，由一系列指令一步一步构建出来。 |
| Docker 容器(Container)          | 容器是独立运行的一个或一组应用。镜像相当于类，容器相当于类的实例 |
| Docker 客户端(Client)           | Docker 客户端通过命令行或者其他工具使用 Docker API 与 Docker 的守护进程通信 |
| Docker 主机(Host)               | 一个物理或者虚拟的机器用于执行 Docker 守护进程和容器。       |
| Docker守护进程                  | 是Docker服务器端进程，负责支撑Docker 容器的运行以及镜像的管理。 |
| Docker 仓库DockerHub (Registry) | Docker 仓库用来保存镜像，可以理解为代码控制中的代码仓库。 Docker Hub提供了庞大的镜像集合供使用。用户也可以将自己本地的镜像推送到Docker仓库供其他人下载。 |

### 三、导入元动力虚拟机镜像

将资源中的虚拟机进行安装（相关的docker容器安装已经安装完毕,注意号段改为200）

补充：虚拟机：复制了？选**移动**了？

同户名：root

密码：ydl666

ifconfig 显示ip

导入虚拟机：注意修改相关号段

![image-20220801165505855](Docker.assets/image-20220801165505855.fc56eea7.png)

#### 1、修改vmware vmnet8

子网IP:192.168.246.0

![image-20220801165549232](Docker.assets/image-20220801165549232.1af12984.png)

 DHCP设置：起始结束号段 246

![image-20220801165628810](Docker.assets/image-20220801165628810.a983ccd5.png)

 NAT设置：网关 192.168.246.2

![image-20220801165650845](Docker.assets/image-20220801165650845.a017c878.png)

#### 2、修改本机vmnet8 地址

 右键网络打开共享中心

 修改适配器设置

![img](Docker.assets/2020-04-05-11-32-12-image.f586abdb.png)

-  vnet8网卡 右键 属性
-  ipv4协议 修改
-  本机IP 192.168.246.1
-  网关 192.168.246.2
-  dns 192.168.246.2

![image-20220801170000634](Docker.assets/image-20220801170000634.a522f4d5.png)

#### 3、启动虚拟机

 root

 ydl666

#### 4、xshell crt finalshell连接到虚拟机

![image-20220801170133621](Docker.assets/image-20220801170133621.cfad42d2.png)

连接成功！

![image-20220801170236614](Docker.assets/image-20220801170236614.10369883.png)

### 四、Docker安装与启动

Docker可以运行在MAC、Windows、CentOS、DEBIAN、UBUNTU等操作系统上，提供社区版和企业版，本课程基于CentOS安装Docker。CentOS6对docker支持的不好，使用docker建议使用CentOS7。

以下是在Ubuntu中安装Docker的步骤：

**ubuntu软件默认安装位置**

```bash
1、下载的软件存放位置
/var/cache/apt/archives
2、安装后软件默认位置
/usr/share
3、可执行文件位置
/usr/bin
4、配置文件位置
/etc
5、lib文件位置
 /usr/lib
```

----------------



```bash
# step 1: 安装必要的一些系统工具
sudo apt-get update
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
# step 2: 安装GPG证书
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
# Step 3: 写入软件源信息
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
# Step 4: 更新并安装 Docker-CE
sudo apt-get -y update
sudo apt-get -y install docker-ce

# 4、 安装docker；出现输入的界面都按 y 
sudo yum install -y docker-ce
# 5、 查看docker版本 
docker -v
```



**设置ustc镜像**

ustc是老牌的linux镜像服务提供者了，还在遥远的ubuntu 5.04版本的时候就在用。ustc的docker镜像加速器速度很快。ustc docker mirror的优势之一就是不需要注册，是真正的公共服务。

1、 编辑文件/etc/docker/daemon.json

```bash
# 执行如下命令： 
mkdir /etc/docker 
vi /etc/docker/daemon.json (daemon.conf)
```



2、在文件中加入下面内容

```bash
#不能出现空格

{
"registry-mirrors":[
	"https://mirror.ccs.tencentyun.com"
]
}
```



**Docker启动与停止命令**

```text
# 启动docker服务： 
systemctl start docker 
# 停止docker服务： 
systemctl stop docker 
# 重启docker服务： 
systemctl restart docker 
# 查看docker服务状态： 
systemctl status docker 
# 设置开机启动docker服务： 
systemctl enable docker
```



![image-20220801172055300](Docker.assets/image-20220801172055300.88ac4313.png)

### 五、镜像相关命令

**镜像**：Docker镜像是由文件系统叠加而成（是一种文件的存储形式）；是docker中的核心概念，可以认为镜像就是对某些运行环境或者软件打的包，用户可以从docker仓库中下载基础镜像到本地，比如开发人员可以从docker仓库拉取（下载）一个只包含centos7系统的基础镜像，然后在这个镜像中安装jdk、mysql、Tomcat和自己开发的应用，最后将这些环境打成一个新的镜像。开发人员将这个新的镜像提交给测试人员进行测试，测试人员只需要在测试环境下运行这个镜像就可以了，这样就可以保证开发人员的环境和测试人员的环境完全一致。

![image-20220801154638733](Docker.assets/image-20220801154638733.b17c9634.png)

#### **1、查看本地镜像**

```text
# 查看镜像可以使用如下命令： 
docker images
```



![image-20220802004512134](Docker.assets/image-20220802004512134.e251414d.png)

REPOSITORY：镜像名称

TAG：镜像标签

IMAGE ID：镜像ID

CREATED：镜像的创建日期

SIZE：镜像大小

#### **2、搜索镜像**

```text
# 如果你需要从网络中查找需要的镜像，可以通过以下命令搜索 
docker search 镜像名称
```



![image-20220802004640991](Docker.assets/image-20220802004640991.11e1bf8d.png)

NAME：镜像名称

DESCRIPTION：镜像描述

STARS：用户评价，反应一个镜像的受欢迎程度

OFFICIAL：是否官方

AUTOMATED：自动构建，表示该镜像由Docker Hub自动构建流程创建的

#### **3、拉取镜像**

```text
# 拉取镜像就是从Docker仓库下载镜像到本地，镜像名称格式为 名称:版本号，如果版本号不指定则是最新的版本 命令如下： d
ocker pull 镜像名称 
# 如拉取centos 7； 
docker pull centos:7
```



![image-20220802005024899](Docker.assets/image-20220802005024899.7a43f24d.png)

#### **4、删除镜像**

```text
# 可以按照镜像id删除镜像，命令如下： 
docker rmi 镜像id
```



![image-20220802005140488](Docker.assets/image-20220802005140488.4384e8ef.png)

1、 docker rmi $IMAGE_ID：删除指定镜像

2、 docker rmi `docker images -q`：删除所有镜像

### 六、容器相关命令

容器，也是docker中的核心概念，容器是由镜像运行产生的运行实例。镜像和容器的关系，就如同Java语言中类和对象的关系。

#### **1、查看容器**

```text
查看正在运行的容器使用命令：
docker ps
查看所有容器使用命令：
docker ps -a
```



![image-20220802005933035](Docker.assets/image-20220802005933035.dca1923f.png)

#### **2、创建并启动容器**

可以基于已有的镜像来创建和启动容器，创建与启动容器使用命令：docker run

**参数说明**：

- -i：表示运行容器
- -t：表示容器启动后会进入其命令行。加入这两个参数后，容器创建就能登录进去。即分配一个伪终端。
- -d：在run后面加上-d参数,则会创建一个守护式容器在后台运行（这样创建容器后不会自动登录容器，如果只加-i -t两个参数，创建后就会自动进去容器）。
- --name :为创建的容器命名。
- -v：表示目录映射关系（前者是宿主机目录，后者是映射到宿主机上的目录），可以使用多个－v做多个目录或文件映射。注意：最好做目录映射，在宿主机上做修改，然后共享到容器上。
- -p：表示端口映射，前者是宿主机端口，后者是容器内的映射端口。可以使用多个-p做多个端口映射

**1**）交互式容器

以**交互式**方式创建并启动容器，启动完成后，直接进入当前容器。使用exit命令退出容器。需要注意的是以此种方式启动容器，如果退出容器，则容器会进入**停止**状态。

```text
# 先拉取一个镜像；这一步不是每次启动容器都要做的，而是因为前面我们删除了镜像，无镜像可用所以才再拉取一个 
docker pull centos:7 
#创建并启动名称为 mycentos7 的交互式容器；下面指令中的镜像名称 centos:7 也可以使用镜像id 
docker run -it --name=ydlcentos1 centos:7 /bin/bash
```



![image-20220802010948873](Docker.assets/image-20220802010948873.0da1db5b.png)

![image-20220802011046622](Docker.assets/image-20220802011046622.c8b4b7a2.png)

**2**）守护式容器

创建一个守护式容器；如果对于一个需要长期运行的容器来说，我们可以创建一个守护式容器。命令如下（容器名称不能重复）：

```bash
#创建并启动守护式容器 
docker run -di --name=ydlcentos2 centos:7 
#登录进入容器命令为：docker exec -it container_name (或者 container_id) /bin/bash（exit退出 时，容器不会停止） 
docker exec -it ydlcentos2 /bin/bash
```



![image-20220802011229343](Docker.assets/image-20220802011229343.1e39ca1b.png)

![image-20220802011349832](Docker.assets/image-20220802011349832.2378bfe5.png)

#### **3、停止并启动容器**

```text
# 停止正在运行的容器：docker stop 容器名称或者ID 
docker stop ydlcentos2 
# 启动已运行过的容器：docker start 容器名称或者ID 
docker start ydlcentos2
```



![image-20220802011603272](Docker.assets/image-20220802011603272.13c2326b.png)

#### 4、**文件拷贝**

注意：容器在停止状态下也可以完成文件的拷贝

将linux宿主机中的文件拷贝到容器内可以使用命令：

```bash
# docker cp 需要拷贝的文件或目录 容器名称:容器目录 

# 创建一个文件abc.txt 
touch itlils.txt 

# 复制abc.txt到mycentos2的容器的 / 目录下 
docker cp itlils.txt ydlcentos2:/root

# 进入mycentos2容器 
docker exec -it ydlcentos2 /bin/bash 

# 查看容器 / 目录下文件 
ll
```



![image-20220802012018175](Docker.assets/image-20220802012018175.aef89a53.png)

将文件从容器内拷贝出来到linux宿主机使用命令：

```bash
# docker cp 容器名称:容器目录 需要拷贝的文件或目录

#进入容器后创建文件cba.txt 
touch itnanls.log 

# 退出容器 
exit 

# 在Linux宿主机器执行复制；将容器mycentos2的/cba.txt文件复制到宿主机器的/root目录下 
docker cp ydlcentos2:/root/itnanls.log /root
```



![image-20220802012150020](Docker.assets/image-20220802012150020.1c4576f5.png)

#### **5、目录挂载**

可以在创建容器的时候，将宿主机的目录与容器内的目录进行映射，这样我们就可以通过修改宿主机某个目录的文件从而去影响容器。

![image-20220802012336743](Docker.assets/image-20220802012336743.c39587ed.png)

创建容器时添加-v参数，后边为宿主机目录:容器目录，例如： docker run -di -v /root/binlog:/root/binlog --name=ydlcentos3 centos:7

```text
# 创建linux宿主机器要挂载的目录 
mkdir /root/binlog 

# 创建并启动容器ydlcentos3 ,并挂载linux中的/root/binlog 目录到容器的/root/binlog ；也就是在 linux中的/root/binlog 中操作相当于对容器相应目录操作 
docker run -di -v /root/binlog:/root/binlog --name=ydlcentos3 centos:7

# 在linux下创建文件 
touch /root/binlog/mysql.log 

# 进入容器 docker exec -it ydlcentos3 /bin/bash

# 在容器中查看目录中是否有对应文件def.txt 
ll /root/binlog
```



![image-20220802012804347](Docker.assets/image-20220802012804347.2f9cc43d.png)

![image-20220802012812843](Docker.assets/image-20220802012812843.ae0981a9.png)

注意：如果你共享的是多级的目录，可能会出现权限不足的提示。 这是因为CentOS7中的安全模块selinux把权限禁掉了，需要添加参数 --privileged=true 来解决挂载的目录没有权限的问题。

#### 6、**查看容器**ip

可以通过以下命令查看容器运行的各种数据 docker inspect 容器名称（容器ID）

```text
# 在linux宿主机下查看 mycentos3 的ip 
docker inspect ydlcentos3
```



容器之间在一个局域网内，linux宿主机器可以与容器进行通信；但是外部的物理机笔记本是不能与容器直接通信的，如果需要则需要通过宿主机器端口的代理。

![image-20220802013027238](Docker.assets/image-20220802013027238.fec911e8.png)

#### **7、删除容器**

删除指定的容器：docker rm 容器名称（容器ID） 删除所有容器：docker rm `docker ps -a -q`

```bash
#先暂停
docker stop ydlcentos3
# 删除容器 
docker rm ydlcentos3
```



如果容器是运行状态则删除失败，需要停止容器才能删除

![image-20220802013334950](Docker.assets/image-20220802013334950.24260740.png)

## 第二章 部署相关软件

### 一、MySQL容器部署

1.搜索mysql镜像

```bash
docker search mysql
```



![image-20220802013901040](Docker.assets/image-20220802013901040.1c677f13.png)

2.拉取mysql镜像

```bash
docker pull mysql:5.7
```



3.创建容器，设置端口映射、目录映射

```bash
# 在/root目录下创建mysql目录用于存储mysql数据信息
mkdir /root/mysql
cd /root/mysql
```



```bash
docker run -id \
-p 3306:3306 \
--name=qjj_mysql \
-v /root/mysql/conf:/etc/mysql/conf.d \
-v /root/mysql/logs:/logs \
-v /root/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=Qjj123456 \
mysql:8 --lower-case-table-names=1
```

```bash
docker run --name=mysql \
-p 3306:3306 \
-v /docker/mysql/logs:/var/log/mysql \
-v /docker/mysql/data:/var/lib/mysql \
-v /docker/mysql/conf/my.cnf:/etc/mysql/my.cnf \
-e MYSQL_ROOT_PASSWORD=Qjj123456@ \
-di mysql:8.0 --lower-case-table-names=1
```







```
docker run -id \
-p 3307:3306 \
--name=qjj_mysql_3306 \
-v /root/mysql_3306/conf:/etc/mysql/conf.d \
-v /root/mysql_3306/logs:/logs \
-v /root/mysql_3306/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=123456 \
mysql:8




 
修改：
docker run --name qjj_mysql --restart=always \
    -v /root/mysql/conf/my.cnf:/etc/mysql/my.cnf \
    -v /root/mysql/data:/var/lib/mysql \
    -p 3306:3306 \
    -e MYSQL_ROOT_PASSWORD="Qjj123456" \
    -e TZ=Asia/Shanghai \
    -d mysql:8 --lower-case-table-names=1
```



- 参数说明：
  - **-p 3307:3306**：将容器的 3306 端口映射到宿主机的 3307 端口。
  - **-v $PWD/conf:/etc/mysql/conf.d**：将主机当前目录下的 conf/my.cnf 挂载到容器的 /etc/mysql/my.cnf。配置目录
  - **-v $PWD/logs:/logs**：将主机当前目录下的 logs 目录挂载到容器的 /logs。日志目录
  - **-v $PWD/data:/var/lib/mysql** ：将主机当前目录下的data目录挂载到容器的 /var/lib/mysql 。数据目录
  - **-e MYSQL_ROOT_PASSWORD=123456：**初始化 root 用户的密码。

![image-20220802014335044](Docker.assets/image-20220802014335044.ef0c1212.png)

4.进入容器，操作mysql

```bash
docker exec -it qjj_mysql /bin/bash
```



![image-20220802014458508](Docker.assets/image-20220802014458508.11796e6e.png)

5.使用外部机器连接容器中的mysql

![image-20220802014616050](Docker.assets/image-20220802014616050.536c06a6.png)

### 二、Tomcat容器部署

1.搜索tomcat镜像

```bash
docker search tomcat
```



![image-20220802014907657](Docker.assets/image-20220802014907657.0e4f2da7.png)

2.拉取tomcat镜像

```bash
docker pull tomcat
```



![image-20220802014958141](Docker.assets/image-20220802014958141.e8fde620.png)

3.创建容器，设置端口映射、目录映射

```bash
# 在/root目录下创建tomcat目录用于存储tomcat数据信息
mkdir /root/tomcat
cd /root/tomcat
```



```bash
docker run -id --name=ydl_tomcat \
-p 8081:8080 \
-v /root/tomcat:/usr/local/tomcat/webapps \
tomcat 
```



- 参数说明：

  - **-p 8080:8080：**将容器的8080端口映射到主机的8080端口

    **-v $PWD:/usr/local/tomcat/webapps：**将主机中当前目录挂载到容器的webapps

1. 使用外部机器访问tomcat

   http://192.168.246.128:8081/

![image-20220802015156699](Docker.assets/image-20220802015156699.4d79e120.png)

**小结**：

> 上传项目文件可以使用容器的目录挂载功能，外部访问可以使用端口映射

### 三、Nginx容器部署

1.搜索nginx镜像

```bash
docker search nginx
```



![image-20220802015500236](Docker.assets/image-20220802015500236.e63a8b02.png)

2.拉取nginx镜像

```bash
docker pull nginx
```



![image-20220802015611869](Docker.assets/image-20220802015611869.a52d72b8.png)

3.创建容器，设置端口映射、目录映射

```bash
# 在/root目录下创建nginx目录用于存储nginx数据信息
mkdir /root/nginx
cd /root/nginx
mkdir conf
cd conf
# 在~/nginx/conf/下创建nginx.conf文件,粘贴下面内容
vim nginx.conf
```



```bash
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```



```bash
docker run -id --name=ydl_nginx \
-p 80:80 \
-v /root/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
-v /root/nginx/logs:/var/log/nginx \
-v /root/nginx/html:/usr/share/nginx/html \
nginx
```



- 参数说明：
  - **-p 80:80**：将容器的 80端口映射到宿主机的 80 端口。
  - **-v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf**：将主机当前目录下的 /conf/nginx.conf 挂载到容器的 :/etc/nginx/nginx.conf。配置目录
  - **-v $PWD/logs:/var/log/nginx**：将主机当前目录下的 logs 目录挂载到容器的/var/log/nginx。日志目录

4.使用外部机器访问nginx

http://192.168.246.128/

![image-20220802015832349](Docker.assets/image-20220802015832349.a5c12ff5.png)

**小结**：

> 如果被占用了80端口，那么在指定映射的时候可以改变宿主机的端口映射，在访问时也需要带上端口号。

### 四、Redis容器部署

1.搜索redis镜像

```bash
docker search redis
```



![image-20220802020222446](Docker.assets/image-20220802020222446.3ed0df45.png)

2.拉取redis镜像

```bash
docker pull redis:5.0
```



![image-20220802020238021](Docker.assets/image-20220802020238021.09b0568e.png)

3.创建容器，设置端口映射

```bash
 mkdir /docker/redis
 mkdir /docker/redis/data
 mkdir /docker/redis/conf

docker run -i \
-p 6379:6379 \
--name=redis \
-v /docker/redis/conf/redis.conf:/etc/redis/conf/redis.conf \
-v /docker/redis/data:/data \
-d redis:6 redis-server /etc/redis/conf/redis.conf --appendonly yes


docker run -id --name=qjj_redis -p 6379:6379 redis

```



4.使用外部机器连接redis

```bash
./redis-cli.exe -h 192.168.246.128 -p 6380
```



![image-20220802020431703](Docker.assets/image-20220802020431703.b165c222.png)

![image-20220802020540154](Docker.assets/image-20220802020540154.6a37b55e.png)

### 五、docker-compose简介&安装

#### **1、** **概念**

Compose项目是Docker官方的开源项目，负责实现对Docker容器集群的快速编排。它**是一个定义和运行多容器的**docker应用工具。使用compose，你能通过YMAL文件配置你自己的服务，然后通过一个命令，你能使用配置文件创建和运行所有的服务。

#### **2、** **组成**

Docker-Compose将所管理的容器分为三层，分别是工程（project），服务（service）以及容器（container）。Docker-Compose运行目录下的所有文件（docker-compose.yml，extends文件或环境变量文件等）组成一个工程，若无特殊指定工程名即为当前目录名。一个工程当中可包含多个服务，每个服务中定义了容器运行的镜像，参数，依赖。一个服务当中可包括多个容器实例。

- **服务（service）**：一个应用的容器，实际上可以包括若干运行相同镜像的容器实例。每个服务都有自己的名字、使用的镜像、挂载的数据卷、所属的网络、依赖哪些其他服务等等，即以容器为粒度，用户需要Compose所完成的任务。
- **项目（project）**：由一组关联的应用容器组成的一个完成业务单元，在docker-compose.yml中定义。即是Compose的一个配置文件可以解析为一个项目，Compose通过分析指定配置文件，得出配置文件所需完成的所有容器管理与部署操作。

Docker-Compose的工程配置文件默认为docker-compose.yml，可通过环境变量COMPOSE_FILE或-f参数自定义配置文件，其定义了多个有依赖关系的服务及每个服务运行的容器。

使用一个Dockerfifile模板文件，可以让用户很方便的定义一个单独的应用容器。在工作中，经常会碰到需要多个容器相互配合来完成某项任务的情况。例如：要部署一个Web项目，除了Web服务容器，往往还需要再加上后端的数据库服务容器，甚至还包括负载均衡容器等。

#### **3、安装**

Compose目前已经完全支持Linux、Mac OS和Windows，在我们安装Compose之前，需要先安装Docker。下面我们以编译好的二进制包方式安装在Linux系统中。

```bash
curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose 

# 设置文件可执行权限 
chmod +x /usr/local/bin/docker-compose 

# 查看版本信息 
docker-compose -version
```



#### **4、卸载**

```bash
# 二进制包方式安装的，删除二进制文件即可 
rm /usr/local/bin/docker-compose
```



#### 5、**常用命令参考**

使用Compose前，可以通过执行 docker-compose --help|-h 来查看Compose基本命令用法。

也可以通过执行 docker-compose [COMMAND] --help 或者 docker-compose --help [COMMAND] 来查看某个具体的使用格式。

可以知道Compose命令的基本的使用格式为：

```text
docker-compose [-f 参数...] [options] [COMMAND] [ARGS...]
```



命令选项如下：

```text
-f，–file FILE指定使用的Compose模板文件，默认为docker-compose.yml，可以多次指定。 
-p，–project-name NAME指定项目名称，默认将使用所在目录名称作为项目名。 
-x-network-driver 使用Docker的可拔插网络后端特性（需要Docker 1.9 及以后版本） 
-x-network-driver DRIVER指定网络后端的驱动，默认为bridge（需要Docker 1.9 及以后版本） 
-verbose输出更多调试信息 
-v，-version打印版本并退出
```



**Docker Compose**常用命令列表如下：

| **命令** | **说明**                                                     |
| -------- | ------------------------------------------------------------ |
| build    | 构建项目中的服务容器                                         |
| help     | 获得一个命令的帮助                                           |
| kill     | 通过发送SIGKILL信号来强制停止服务容器                        |
| confifig | 验证和查看compose文件配置                                    |
| create   | 为服务创建容器。只是单纯的create，还需要使用start启动compose |
| down     | 停止并删除容器，网络，镜像和数据卷                           |
| exec     | 在运行的容器中执行一个命令                                   |
| logs     | 查看服务容器的输出                                           |
| pause    | 暂停一个服务容器                                             |
| port     | 打印某个容器端口所映射的公共端口                             |
| ps       | 列出项目中目前的所有容器                                     |
| pull     | 拉取服务依赖的镜像                                           |
| push     | 推送服务镜像                                                 |
| restart  | 重启项目中的服务                                             |
| rm       | 删除所有（停止状态的）服务容器                               |
| run      | 在指定服务上执行一个命令                                     |
| scale    | 设置指定服务运行的容器个数                                   |
| start    | 启动已经存在的服务容器                                       |
| stop     | 停止已经处于运行状态的容器，但不删除它                       |
| top      | 显示运行的进程                                               |
| unpause  | 恢复处于暂停状态中的服务                                     |
| up       | 自动完成包括构建镜像、创建服务、启动服务并关闭关联服务相关容器的一些列操作 |
| version  | 打印版本信息                                                 |

1.up

格式为：

```text
docker-compose up [options] [--scale SERVICE=NUM...] [SERVICE...] 
```



up命令十分强大，它尝试自动完成包括构建镜像，（重新）创建服务，启动服务，并关联服务相关容器的一些列操作。链接的服务都将会被自动启动，除非已经处于运行状态。

多数情况下我们可以直接通过该命令来启动一个项目。

选项包括：

```text
-d 在后台运行服务容器 
–no-color 不使用颜色来区分不同的服务的控制输出 
–no-deps 不启动服务所链接的容器 
–force-recreate 强制重新创建容器，不能与–no-recreate同时使用 
–no-recreate 如果容器已经存在，则不重新创建，不能与–force-recreate同时使用 
–no-build 不自动构建缺失的服务镜像 
–build 在启动容器前构建服务镜像 
–abort-on-container-exit 停止所有容器，如果任何一个容器被停止，不能与-d同时使用 
-t, --timeout TIMEOUT 停止容器时候的超时（默认为10秒） 
–remove-orphans 删除服务中没有在compose文件中定义的容器 
–scale SERVICE=NUM 设置服务运行容器的个数，将覆盖在compose中通过scale指定的参数
```



**2. ps**

格式为：

```text
docker-compose ps [options] [SERVICE...]
```



列出项目中目前的所有容器。

选项包括：

```text
-q 只打印容器的ID信息
```



**3. stop**

格式为：

```text
docker-compose stop [options] [SERVICE...]
```



停止已经处于运行状态的容器，但不删除它。

选项包括：

```text
-t, --timeout TIMEOUT 停止容器时候的超时（默认为10秒）
```



**4. down**

格式为：

```text
docker-compose down [options]
```



停止和删除容器、网络、卷、镜像，这些内容是通过docker-compose up命令创建的. 默认值删除 容器 网络，可以通过指定 rmi 、volumes参数删除镜像和卷。

选项包括：

```text
–rmi type 删除镜像，类型必须是: ‘all’: 删除compose文件中定义的所以镜像；‘local’: 删除镜像名为空的 镜像
-v, --volumes 删除已经在compose文件中定义的和匿名的附在容器上的数据卷 
–remove-orphans 删除服务中没有在compose中定义的容器
```



**5. restart**

格式为：

```text
docker-compose restart [options] [SERVICE...]
```



重启项目中的服务。

选项包括：

```text
-t, --timeout TIMEOUT 指定重启前停止容器的超时（默认为10秒）
```



**6. rm**

格式为：

```text
docker-compose rm [options] [SERVICE...] 1
```



删除所有（停止状态的）服务容器。

选项包括：

```text
–f, --force 强制直接删除，包括非停止状态的容器 
-v 删除容器所挂载的数据卷
```



**7. start**

格式为：

```text
docker-compose start [SERVICE...]
```



启动已经存在的服务容器。

**8. run**

格式为：

```text
docker-compose run [options] [-v VOLUME...] [-p PORT...] [-e KEY=VAL...] SERVICE [COMMAND] [ARGS...]
```



在指定服务上执行一个命令。

例如：

```text
docker-compose run ubuntu ping www.baidu.com
```



将会执行一个ubuntu容器，并执行ping www.baidu.com命令。

默认情况下，如果存在关联，则所有关联的服务将会自动被启动，除非这些服务已经在运行中。该命令类似于启动容

器后运行指定的命令，相关卷、链接等都会按照配置自动创建。有两个不同点：

1. 给定命令将会覆盖原有的自动运行命令
2. 不会自动创建端口，以避免冲突

如果不希望自动启动关联的容器，可以使用–no-deps选项，例如：

```text
docker-compose run --no-deps web
```



将不会启动web容器关联的其他容器

选项包括：

```text
-d 在后台运行服务容器 
–name NAME 为容器指定一个名字 
–entrypoint CMD 覆盖默认的容器启动指令 
-e KEY=VAL 设置环境变量值，可多次使用选项来设置多个环境变量
-u, --user="" 指定运行容器的用户名或者uid
–no-deps 不自动启动管理的服务容器 
–rm 运行命令后自动删除容器，d模式下将忽略 
-p, --publish=[] 映射容器端口到本地主机 
–service-ports 配置服务端口并映射到本地主机 
-v, --volume=[] 绑定一个数据卷，默认为空 
-T 不分配伪tty，意味着依赖tty的指令将无法运行 
-w, --workdir="" 为容器指定默认工作目录
```



**9. confifig**

格式为：

```text
docker-compose config [options]
```



验证并查看compose文件配置。

选项包括：

```text
–resolve-image-digests 将镜像标签标记为摘要 
-q, --quiet 只验证配置，不输出。 当配置正确时，不输出任何内容，当文件配置错误，输出错误信息 
–services 打印服务名，一行一个 
–volumes 打印数据卷名，一行一个
```



**10. kill**

格式为：

```text
docker-compose kill [options] [SERVICE...]
```



通过发送SIGKILL信号来强制停止服务容器。 支持通过-s参数来指定发送的信号，例如：通过如下指令发送SIGINT信号：

```text
docker-compose kill -s SIGINT
```



**11. create**

格式为：

```text
docker-compose create [options] [SERVICE...] 1
```



为服务创建容器.只是单纯的create，还需要使用start启动compose。

选项包括：

```text
–force-recreate 重新创建容器，即使它的配置和镜像没有改变，不兼容–no-recreate参数 
–no-recreate 如果容器已经存在，不需要重新创建. 不兼容–force-recreate参数 
–no-build 不创建镜像，即使缺失 
–build 创建容器前，生成镜像
```



**12. exec**

格式为：

```text
docker-compose exec [options] SERVICE COMMAND [ARGS...]
```



与 docker exec 命令功能相同，可以通过service name登陆到容器中。

选项包括：

```text
-d 分离模式，后台运行命令. 
–privileged 获取特权. 
–user USER 指定运行的用户. 
-T 禁用分配TTY. By default docker-compose exec分配 a TTY. 
–index=index 当一个服务拥有多个容器时，可通过该参数登陆到该服务下的任何服务，例如：docker-compose exec --index=1 web /bin/bash ，web服务中包含多个容器
```



### [#](https://www.ydlclass.com/doc21xnv/docker/#六、compose模版文件)六、Compose模版文件

模板文件是使用Compose的核心，涉及的指令关键字也比较多，大部分指令与 docker run 相关参数的含义都是类似的。默认的模板文件名称为docker-compose.yml，格式为YAML格式。

比如一个Compose模板文件：

```text
version: "2" 
	services: 
		web:
			images: nginx 
			ports: -"8080:80" 
			volumes: - /usr/local/abc:/usr/local/cba 
#volumes: 
#networks:
```



Docker Compose的模板文件主要分为3个区域，如下：

- services
  - 服务，在它下面可以定义应用需要的一些服务，每个服务都有自己的名字、使用的镜像、挂载的数据卷、所属的网络、依赖哪些其他服务等等。
- volumes
  - 数据卷，在它下面可以定义的数据卷（名字等等），然后挂载到不同的服务下去使用。
- networks
  - 应用的网络，在它下面可以定义应用的名字、使用的网络类型等等。

| **指令**         | **功能**                                                     |
| ---------------- | ------------------------------------------------------------ |
| build            | 指定服务镜像Dockerfifile所在路径                             |
| cap_dd，cap_drop | 指定容器的内核能力（capacity）分配                           |
| command          | 覆盖容器启动后默认执行的命令                                 |
| cgroup_parent    | 指定父cgroup组，意味着将基础该组的资源限制                   |
| container_name   | 指定容器名称。默认将会使用项目名称服务名称序号这样的格式     |
| devices          | 指定设置映射关系                                             |
| dns              | 自定义DNS服务器。可以是一个值，也可以是一个列表              |
| dns_search       | 配置DNS搜索域。可以是一个值，也可以是一个列表                |
| dockerfifile     | 指定额外编译镜像的Dockerfifile文件，可以通过该指令来指定     |
| env_fifile       | 从文件中获取环境变量，可以为单独的文件路径或列表             |
| environment      | 设置环境变量，可以使用数组或字典两种格式                     |
| expose           | 暴露端口                                                     |
| external_links   | 链接到docker-compose.yml外部的容器，甚至可以是非Compose管理的外部容器 |
| extra_hosts      | 指定额外的host名称映射信息                                   |
| image            | 指定为镜像名称或镜像ID。如果镜像在本地不存在，Compose将会尝试拉取这个镜像 |
| labels           | 指定服务镜像Dockerfifile所在路径                             |
| links            | 链接到其他服务中的容器                                       |
| log_driver       | 指定日志驱动类型，类似于Docker中的–log-driver参数。目前支持三种日志驱动类型：log_driver:“json-fifile”、log_driver:“syslog”、log_driver:“none” |
| log_opt          | 日志驱动的相关参数                                           |
| net              | 设置网络模式。参数类似于docker clinet的–net参数一样          |
| pid              | 跟主机系统共享进程命名空间。打开该选项的容器之间，以及容器和宿主机系统之间可以通过进程ID来相互访问和操作 |
| ports            | 暴露端口信息                                                 |
| security_opt     | 指定容器模板标签（label）机制的默认属性（如用户、角色、类型、级别等） |
| ulimits          | 指定容器的ulimits限制值                                      |
| volumes          | 数据卷所挂载路径设置。可以设置宿主机路径（HOST:CONTAINER）或加上访问模式(HOST:CONTAINER:ro） |

### [#](https://www.ydlclass.com/doc21xnv/docker/#七、compose应用)七、Compose应用

**需求**：编写compose模版文件，实现同时启动tomcat、mysql和redis容器。

#### [#](https://www.ydlclass.com/doc21xnv/docker/#_1、-编写模版文件)**1、** **编写模版文件**

```bash
# 创建文件夹
mkdir -p /usr/local/mycompose 
#进入文件夹 
cd /usr/local/mycompose 
#创建 docker-compose.yml文件；内容如下 
vi docker-compose.yml
```



docker-compose.yml文件内容如下（文件内容请从 资料\配置文件\docker-compose.yml 复制）：

```text
version: '3'
services:
  redis1:
    image: redis
    ports:
      - "6379:6379"
    container_name: "redis1"
    networks: 
      - dev
  mysql1:
    image: centos/mysql-57-centos7
    environment:
      MYSQL_ROOT_PASSWORD: "root"
    ports: 
      - "3306:3306"
    container_name: "mysql1"
    networks: 
      - dev
  web1:
    image: tomcat
    ports: 
      - "9090:8080"
    container_name: "web1"
    networks: 
      - dev
      - pro
networks:
  dev:
    driver: bridge
  pro:
    driver: bridge
```



上面我们声明了3个服务；分别是：redis1、mysql1、web1；并且对3个服务都指定了对应的docker 镜像和端口。

#### [#](https://www.ydlclass.com/doc21xnv/docker/#_2、启动)**2**、**启动**

```text
#启动前最好把docker重启，不然原来的tomcat/mysql/redis容器也是启动状态的话，那么端口会冲突而启动失败 
systemctl restart docker 

cd /usr/local/mycompose 

docker-compose up 

# 如果后台启动则使用如下命令 
docker-compose up -d 

# 若要停止 
docker-compose stop
```



可以查看到命令中，创建了两个自定义的网络（mycompose_dev和mycompose_pro），然后创建容器，并「Attaching to …」，将网络应用到服务上。

可以查看一下具体的网络，使用docker network ls 如下：

![image-20220802030057950](Docker.assets/image-20220802030057950.73932fc5.png)

查看启动的容器：

![image-20220802030034771](Docker.assets/image-20220802030034771.cd359b2e.png)

#### [#](https://www.ydlclass.com/doc21xnv/docker/#_3、-测试)**3、** **测试**

在windows下访问启动的3个服务进行测试都可以；如下面访问9090的tomcat如下：

![image-20220802030132427](Docker.assets/image-20220802030132427.e2867ecf.png)

## [#](https://www.ydlclass.com/doc21xnv/docker/#第三章-迁移备份仓库)第三章 迁移备份仓库

![image-20220802035604213](Docker.assets/image-20220802035604213.a1ff4638.png)

### [#](https://www.ydlclass.com/doc21xnv/docker/#一、迁移与备份)一、迁移与备份

#### [#](https://www.ydlclass.com/doc21xnv/docker/#_1、-将docker容器保存为镜像)**1、** **将**Docker容器保存为镜像

使用docker commit命令可以将容器保存为镜像。

命令形式：docker commit 容器名称 镜像名称

```bash
# 保存nginx容器为镜像 
docker commit ydlcentos1 ydlcentos1
```



此镜像的内容就是当前容器的内容，接下来你可以用此镜像再次运行新的容器

#### [#](https://www.ydlclass.com/doc21xnv/docker/#_2、-镜像备份)**2、** **镜像备份**

使用docker save命令可以将已有镜像保存为tar 文件。

命令形式：docker save –o tar文件名 镜像名

```text
# 保存镜像为文件 
docker save -o ydlcentos1.tar ydlcentos1
```



![image-20220802032156887](Docker.assets/image-20220802032156887.a2e3f565.png)

#### [#](https://www.ydlclass.com/doc21xnv/docker/#_3、-镜像恢复与迁移)**3、** **镜像恢复与迁移**

使用docker load命令可以根据tar文件恢复为docker镜像。

命令形式：docker load -i tar文件名

```text
# 停止mynginx容器 
docker stop ydlcentos1 
# 删除mynginx容器 
docker rm ydlcentos1 
# 删除mynginx镜像 
docker rmi ydlcentos1 
# 加载恢复mynginx镜像 
docker load -i ydlcentos1.tar 
# 在镜像恢复之后，基于该镜像再次创建启动容器 
docker run -di --name=ydlcentos1 ydlcentos1
```



注意：在执行docker load命令恢复镜像时，需要先删除原镜像。

### [#](https://www.ydlclass.com/doc21xnv/docker/#二、-使用dockerfile创建镜像)二、 使用Dockerfile创建镜像（根据我们的流程 制作镜像）

#### [#](https://www.ydlclass.com/doc21xnv/docker/#_1、-什么是dockerfile文件)**1、** **什么是**Dockerfile文件

前面的课程中已经知道了，要获得镜像，可以从Docker仓库中进行下载。那如果我们想自己开发一个镜像，那该如何做呢？答案是：Dockerfifile

Dockerfifile其实就是一个文本文件，由一系列命令和参数构成，Docker可以读取Dockerfifile文件并根据Dockerfifile文件的描述来构建镜像。

Dockerfifile文件内容一般分为4部分：

- 基础镜像信息
- 维护者信息
- 镜像操作指令
- 容器启动时执行的指令

#### [#](https://www.ydlclass.com/doc21xnv/docker/#_2、dockerfifile常用命令)2、Dockerfifile常用命令

Dockerfile可以基于镜像制作镜像；`docker build -t='jdk1.8' .`

| 关键字      | 作用                     | 备注                                                         |
| ----------- | ------------------------ | ------------------------------------------------------------ |
| FROM        | 指定父镜像               | 指定dockerfile基于那个image构建                              |
| MAINTAINER  | 作者信息                 | 用来标明这个dockerfile谁写的                                 |
| LABEL       | 标签                     | 用来标明dockerfile的标签 可以使用Label代替Maintainer 最终都是在docker image基本信息中可以查看 |
| RUN         | 执行命令                 | 执行一段命令 默认是/bin/sh 格式: RUN command 或者 RUN ["command" , "param1","param2"] |
| CMD         | 容器启动命令             | 提供启动容器时候的默认命令 和ENTRYPOINT配合使用.格式 CMD command param1 param2 或者 CMD ["command" , "param1","param2"] |
| ENTRYPOINT  | 入口                     | 一般在制作一些执行就关闭的容器中会使用                       |
| COPY        | 复制文件                 | build的时候复制文件到image中                                 |
| ADD         | 添加文件                 | build的时候添加文件到image中 不仅仅局限于当前build上下文 可以来源于远程服务 |
| ENV         | 环境变量                 | 指定build时候的环境变量 可以在启动的容器的时候 通过-e覆盖 格式ENV name=value |
| ARG         | 构建参数                 | 构建参数 只在构建的时候使用的参数 如果有ENV 那么ENV的相同名字的值始终覆盖arg的参数 |
| VOLUME      | 定义外部可以挂载的数据卷 | 指定build的image那些目录可以启动的时候挂载到文件系统中 启动容器的时候使用 -v 绑定 格式 VOLUME ["目录"] |
| EXPOSE      | 暴露端口                 | 定义容器运行的时候监听的端口 启动容器的使用-p来绑定暴露端口 格式: EXPOSE 8080 或者 EXPOSE 8080/udp |
| WORKDIR     | 工作目录                 | 指定容器内部的工作目录 如果没有创建则自动创建 如果指定/ 使用的是绝对地址 如果不是/开头那么是在上一条workdir的路径的相对路径 |
| USER        | 指定执行用户             | 指定build或者启动的时候 用户 在RUN CMD ENTRYPONT执行的时候的用户 |
| HEALTHCHECK | 健康检查                 | 指定监测当前容器的健康监测的命令 基本上没用 因为很多时候 应用本身有健康监测机制 |
| ONBUILD     | 触发器                   | 当存在ONBUILD关键字的镜像作为基础镜像的时候 当执行FROM完成之后 会执行 ONBUILD的命令 但是不影响当前镜像 用处也不怎么大 |
| STOPSIGNAL  | 发送信号量到宿主机       | 该STOPSIGNAL指令设置将发送到容器的系统调用信号以退出。       |
| SHELL       | 指定执行脚本的shell      | 指定RUN CMD ENTRYPOINT 执行命令的时候 使用的shell            |

#### [#](https://www.ydlclass.com/doc21xnv/docker/#_3、-使用dockerfile创建镜像)**3、** **使用Dockerfile创建镜像**

```bash
# 1、创建目录 
mkdir –p /usr/local/dockerjdk8 
cd /usr/local/dockerjdk8 


rz -be  #上传
# 2、下载jdk-8u231-linux-x64.tar.gz并上传到服务器（虚拟机）中的/usr/local/dockerjdk8目录 

# 3、在/usr/local/dockerjdk8目录下创建Dockerfile文件，文件内容如下： 
vi Dockerfile 

FROM centos:7 
MAINTAINER ITLILS 
WORKDIR /usr 
RUN mkdir /usr/local/java 
ADD jdk-8u231-linux-x64.tar.gz /usr/local/java/ 
ENV JAVA_HOME /usr/local/java/jdk1.8.0_231 
ENV JRE_HOME $JAVA_HOME/jre 
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH 
ENV PATH $JAVA_HOME/bin:$PATH 

# 4、执行命令构建镜像；不要忘了后面的那个 . 
docker build -t='jdk1.8' . 

# 5、查看镜像是否建立完成 
docker images






FROM ubuntu
MAINTAINER QJJTest
WORKDIR /usr 
#java
RUN mkdir /usr/local/java 
ADD jdk-8u333-linux-x64.tar.gz /usr/local/java/ 
ENV JAVA_HOME /usr/local/java/jdk1.8.0_333 
ENV JRE_HOME $JAVA_HOME/jre 
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH 
ENV PATH $JAVA_HOME/bin:$PATH 

#加入项目
RUN mkdir /usr/bin/project
ADD spring-test-0.0.1-SNAPSHOT.jar /usr/bin/project/app.jar
EXPOSE 8888
ENTRYPOINT java -jar /usr/bin/project/app.jar

=========================






FROM jdk1.8
MAINTAINER QJJTest
WORKDIR /usr 

#加入项目
RUN mkdir /usr/bin/project
ADD spring-test-0.0.1-SNAPSHOT.jar /usr/bin/project/app.jar
EXPOSE 8888
EXNTRYPOINT ["java" ,""-jar" ,""/usr/bin/project/app.jar"]




# 启动镜像
docker run -id \
-p 8089:8888 \
--name=java_project_net \
--network mycompose_dev \
java_project

docker run -id \
-p 8088:8888 \
--name=project_test2_net \
--network mycompose_dev \
project_test2


```



![image-20220802034204602](Docker.assets/image-20220802034204602.52a6d1f9.png)

#### [#](https://www.ydlclass.com/doc21xnv/docker/#_4、-基于镜像创建容器)**4、** **基于镜像创建容器**

基于刚刚创建的镜像 jdk1.8 创建并启动容器进行测试；

```bash
# 创建并启动容器 
docker run -it --name=testjdk jdk1.8 /bin/bash 
# 在容器中测试jdk是否已经安装 
java -version
```



![image-20220802034313026](Docker.assets/image-20220802034313026.97b93b5b.png)

**作业：**

1. boot 一个controller 访问/test,返回“hello yuandongli!”
2. 打成jar包，使用Dockerfile，centos:7为底构建一个镜像，起起来，可以访问到8080/test,返回“hello yuandongli!”

### [#](https://www.ydlclass.com/doc21xnv/docker/#三、-私有仓库搭建与配置)三、 私有仓库搭建与配置

![image-20220802035500638](Docker.assets/image-20220802035500638.f74f89e7.png)

#### [#](https://www.ydlclass.com/doc21xnv/docker/#_1-私有仓库搭建与配置)**1.** **私有仓库搭建与配置**

Docker官方的Docker hub（https://hub.docker.com）是一个用于管理公共镜像的仓库，我们可以从上面拉取镜像到本地，也可以把我们自己的镜像推送上去。但是，有时候我们的服务器无法访问互联网，或者你不希望将自己的镜像放到公网当中，那么我们就需要搭建自己的私有仓库来存储和管理自己的镜像。

私有仓库搭建步骤：

```bash
# 1、拉取私有仓库镜像 
docker pull registry

# 2、启动私有仓库容器 
docker run -id --name=registry -p 5000:5000 registry

# 3、打开浏览器 输入地址http://私有仓库服务器ip:5000/v2/_catalog，看到{"repositories":[]} 表示私有仓库 搭建成功

# 4、修改daemon.json   
vim /etc/docker/daemon.json   

# 在上述文件中添加一个key，保存退出。此步用于让 docker 信任私有仓库地址；注意将私有仓库服务器ip修改为自己私有仓库服务器真实ip 

{
"registry-mirrors":["https://mirror.ccs.tencentyun.com"],
"insecure-registries":["124.220.78.31:5000"]
}


# 5、重启docker 服务 
systemctl restart docker
docker start registry
```



![image-20220802035716690](Docker.assets/image-20220802035716690.5eaa68fb.png)

![image-20220802035845322](Docker.assets/image-20220802035845322.f9eec603.png)

#### [#](https://www.ydlclass.com/doc21xnv/docker/#_2-将镜像上传至私有仓库)**2.** **将镜像上传至私有仓库**

```bash
# 1、标记镜像为私有仓库的镜像     
docker tag java_project 124.220.78.31:5000/java_project_registery
 
# 2、上传标记的镜像     
docker push 124.220.78.31:5000/java_project_registery
```



![image-20220802040142936](Docker.assets/image-20220802040142936.cd6ccb18.png)

#### [#](https://www.ydlclass.com/doc21xnv/docker/#_3-从私有仓库拉取镜像)**3.** **从私有仓库拉取镜像**

 **（1）** **私有仓库所在服务器拉取镜像**

若是在私有仓库所在的服务器上去拉取镜像；那么直接执行如下命令：

```bash
# 因为私有仓库所在的服务器上已经存在相关镜像；所以先删除；请指定镜像名，不是id 
docker rmi 192.168.246.128:5000/redis:5.0

#拉取镜像 
docker pull 192.168.246.128:5000/redis:5.0

#可以通过如下命令查看 docker 的信息；了解到私有仓库地址 
docker info
```



![image-20220802040432037](Docker.assets/image-20220802040432037.627459e6.png)

在仓库所在服务器上去拉取镜像是比较少有的操作；作为了解即可。

 **（2）** **其它服务器拉取私有仓库镜像**

大多数情况下，都是某台服务器部署了私有镜像仓库之后；到其它服务器上从私有仓库中拉取镜像，若要拉取私有仓库镜像需要去修改docker的配置文件，设置启动时候的仓库地址。

```bash
# 打开配置文件 
vi /usr/lib/systemd/system/docker.service 

# 在打开的上述文件中按照下面的图，添加如下的内容；注意修改下面内容中的ip地址 
--add-registry=124.220.78.31:5000 --insecure-registry=124.220.78.31:5000 \ 

# 修改完后需要重新加载docker配置文件并重启docker 
systemctl daemon-reload 
systemctl restart docker
```



在重启之后；那么则可以去拉取私有仓库中的镜像：

```bash
# 执行拉取镜像命令并查看 
docker pull 124.220.78.31:5000/java_project_registery

docker images
```



### 四、和虚拟机对比

![image-20200622200917932](Docker.assets/image-20200622200917932.0185b1c3.png)

![image-20200622200927992](Docker.assets/image-20200622200927992.2fb8689d.png)

![image-20220802041415505](Docker.assets/image-20220802041415505.8a5a6240.png)















# 命令总结



```bash
# step 1: 安装必要的一些系统工具
sudo apt-get update
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
# step 2: 安装GPG证书
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
# Step 3: 写入软件源信息
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
# Step 4: 更新并安装 Docker-CE
sudo apt-get -y update
sudo apt-get -y install docker-ce

# 4、 安装docker；出现输入的界面都按 y 
sudo apt-get install -y docker-ce
# 5、 查看docker版本 
docker -v
```



1、 编辑文件/etc/docker/daemon.json

```bash
# 执行如下命令： 
mkdir /etc/docker 
vi /etc/docker/daemon.json (daemon.conf)
```



2、在文件中加入下面内容

```bash
{
"registry-mirrors": [
	"https://docker.mirrors.ustc.edu.cn",
	"http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo",
	"https://mirror.ccs.tencentyun.com"
] 
}
```



**Docker启动与停止命令**

```bash
# 启动docker服务： 
systemctl start docker 
# 停止docker服务： 
systemctl stop docker 
# 重启docker服务： 
systemctl restart docker 
# 查看docker服务状态： 
systemctl status docker 
# 设置开机启动docker服务： 
systemctl enable docker
```



```bash
#创建并启动守护式容器 
docker run -di --name=ydlcentos2 centos:7 
#登录进入容器命令为：docker exec -it container_name (或者 container_id) /bin/bash（exit退出 时，容器不会停止） 
docker exec -it ydlcentos2 /bin/bash
```





## 使用

### docker-compose简介&安装 		(用来创建容器)

####  **概念**

Compose项目是Docker官方的开源项目，负责实现对Docker容器集群的快速编排。它**是一个定义和运行多容器的**docker应用工具。使用compose，你能通过YMAL文件配置你自己的服务，然后通过一个命令，你能使用配置文件创建和运行所有的服务。

```bash
curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose 

# 设置文件可执行权限 
chmod +x /usr/local/bin/docker-compose 

# 查看版本信息 
docker-compose -version
```

### Compose应用

**需求**：编写compose模版文件，实现同时启动tomcat、mysql和redis容器。

#### **编写模版文件**

```bash
# 创建文件夹
mkdir -p /usr/local/mycompose 
#进入文件夹 
cd /usr/local/mycompose 
#创建 docker-compose.yml文件；内容如下 
vi docker-compose.yml
```

docker-compose.yml文件内容如下：

```bash
version: '3'
services:
  redis1:
    image: redis
    ports:
      - "6379:6379"
    container_name: "redis1"
    networks: 
      - dev
  mysql1:
    image: centos/mysql-57-centos7
    environment:
      MYSQL_ROOT_PASSWORD: "root"
    ports: 
      - "3306:3306"
    container_name: "mysql1"
    networks: 
      - dev
  web1:
    image: tomcat
    ports: 
      - "9090:8080"
    container_name: "web1"
    networks: 
      - dev
      - pro
networks:
  dev:
    driver: bridge
  pro:
    driver: bridge
```



上面我们声明了3个服务；分别是：redis1、mysql1、web1；并且对3个服务都指定了对应的docker 镜像和端口。

#### **启动**

```bash
#启动前最好把docker重启，不然原来的tomcat/mysql/redis容器也是启动状态的话，那么端口会冲突而启动失败 
systemctl restart docker 

cd /usr/local/mycompose 

docker-compose up 

# 如果后台启动则使用如下命令 
docker-compose up -d 

# 若要停止 
docker-compose stop
```





### 使用Dockerfile创建镜像（根据我们的流程 制作镜像）

#### **1、** **什么是**Dockerfile文件

前面的课程中已经知道了，要获得镜像，可以从Docker仓库中进行下载。那如果我们想自己开发一个镜像，那该如何做呢？答案是：Dockerfifile

Dockerfifile其实就是一个文本文件，由一系列命令和参数构成，Docker可以读取Dockerfifile文件并根据Dockerfifile文件的描述来构建镜像。

Dockerfifile文件内容一般分为4部分：

- 基础镜像信息
- 维护者信息
- 镜像操作指令
- 容器启动时执行的指令

#### 2、Dockerfifile常用命令

Dockerfile可以基于镜像制作镜像；`docker build -t='jdk1.8' .`

| 关键字      | 作用                     | 备注                                                         |
| ----------- | ------------------------ | ------------------------------------------------------------ |
| FROM        | 指定父镜像               | 指定dockerfile基于那个image构建                              |
| MAINTAINER  | 作者信息                 | 用来标明这个dockerfile谁写的                                 |
| LABEL       | 标签                     | 用来标明dockerfile的标签 可以使用Label代替Maintainer 最终都是在docker image基本信息中可以查看 |
| RUN         | 执行命令                 | 执行一段命令 默认是/bin/sh 格式: RUN command 或者 RUN ["command" , "param1","param2"] |
| CMD         | 容器启动命令             | 提供启动容器时候的默认命令 和ENTRYPOINT配合使用.格式 CMD command param1 param2 或者 CMD ["command" , "param1","param2"] |
| ENTRYPOINT  | 入口                     | 一般在制作一些执行就关闭的容器中会使用                       |
| COPY        | 复制文件                 | build的时候复制文件到image中                                 |
| ADD         | 添加文件                 | build的时候添加文件到image中 不仅仅局限于当前build上下文 可以来源于远程服务 |
| ENV         | 环境变量                 | 指定build时候的环境变量 可以在启动的容器的时候 通过-e覆盖 格式ENV name=value |
| ARG         | 构建参数                 | 构建参数 只在构建的时候使用的参数 如果有ENV 那么ENV的相同名字的值始终覆盖arg的参数 |
| VOLUME      | 定义外部可以挂载的数据卷 | 指定build的image那些目录可以启动的时候挂载到文件系统中 启动容器的时候使用 -v 绑定 格式 VOLUME ["目录"] |
| EXPOSE      | 暴露端口                 | 定义容器运行的时候监听的端口 启动容器的使用-p来绑定暴露端口 格式: EXPOSE 8080 或者 EXPOSE 8080/udp |
| WORKDIR     | 工作目录                 | 指定容器内部的工作目录 如果没有创建则自动创建 如果指定/ 使用的是绝对地址 如果不是/开头那么是在上一条workdir的路径的相对路径 |
| USER        | 指定执行用户             | 指定build或者启动的时候 用户 在RUN CMD ENTRYPONT执行的时候的用户 |
| HEALTHCHECK | 健康检查                 | 指定监测当前容器的健康监测的命令 基本上没用 因为很多时候 应用本身有健康监测机制 |
| ONBUILD     | 触发器                   | 当存在ONBUILD关键字的镜像作为基础镜像的时候 当执行FROM完成之后 会执行 ONBUILD的命令 但是不影响当前镜像 用处也不怎么大 |
| STOPSIGNAL  | 发送信号量到宿主机       | 该STOPSIGNAL指令设置将发送到容器的系统调用信号以退出。       |
| SHELL       | 指定执行脚本的shell      | 指定RUN CMD ENTRYPOINT 执行命令的时候 使用的shell            |

#### **3、** **使用Dockerfile创建镜像**

```bash
# 1、创建目录 
mkdir –p /usr/local/dockerjdk8 
cd /usr/local/dockerjdk8 


rz -be  #上传
# 2、下载jdk-8u231-linux-x64.tar.gz并上传到服务器（虚拟机）中的/usr/local/dockerjdk8目录 

# 3、在/usr/local/dockerjdk8目录下创建Dockerfile文件，文件内容如下： 
vi Dockerfile 

FROM centos:7 
MAINTAINER ITLILS 
WORKDIR /usr 
RUN mkdir /usr/local/java 
ADD jdk-8u231-linux-x64.tar.gz /usr/local/java/ 
ENV JAVA_HOME /usr/local/java/jdk1.8.0_231 
ENV JRE_HOME $JAVA_HOME/jre 
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH 
ENV PATH $JAVA_HOME/bin:$PATH 

# 4、执行命令构建镜像；不要忘了后面的那个 . 
docker build -t='jdk1.8' . 

# 5、查看镜像是否建立完成 
docker images





#案例 使用ubuntu作为底层 创建java镜像 同时运行jar项目
FROM ubuntu
MAINTAINER QJJTest
WORKDIR /usr 
#java
RUN mkdir /usr/local/java 
ADD jdk-8u333-linux-x64.tar.gz /usr/local/java/ 
ENV JAVA_HOME /usr/local/java/jdk1.8.0_333 
ENV JRE_HOME $JAVA_HOME/jre 
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH 
ENV PATH $JAVA_HOME/bin:$PATH 
#加入项目
RUN mkdir /usr/bin/project
ADD spring-test-0.0.1-SNAPSHOT.jar /usr/bin/project/app.jar
EXPOSE 8888
ENTRYPOINT java -jar /usr/bin/project/app.jar

=========================

#以 Java为底包 创建 jar(我们自己项目) 镜像
FROM jdk1.8
MAINTAINER QJJTest
WORKDIR /usr 
#加入项目
RUN mkdir /usr/bin/project
ADD spring-test-0.0.1-SNAPSHOT.jar /usr/bin/project/app.jar
#暴露端口
EXPOSE 8888
#执行命令
ENTRYPOINT ["java" ,""-jar" ,""/usr/bin/project/app.jar"]




# 通过镜像文件 创建容器
docker run -id \
-p 8089:8888 \
--name=java_project_net \
--network mycompose_dev \
java_project

docker run -id \
-p 8088:8888 \
--name=project_test2_net \
--network mycompose_dev \
project_test2

```

#### 

#### **4、** **基于镜像创建容器**

基于刚刚创建的镜像 jdk1.8 创建并启动容器进行测试；

```bash
# 创建并启动容器 
docker run -it --name=testjdk jdk1.8 /bin/bash 
# 在容器中测试jdk是否已经安装 
java -version
```

