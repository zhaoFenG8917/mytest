# Docker 容器化编排

学习目标：

- 掌握Docker基础知识，能够理解Docker镜像与容器的概念

- 完成Docker安装与启动

- 掌握Docker镜像与容器相关命令

- 掌握Weblogic jenkins 等软件的常用应用的安装

- 掌握docker迁移与备份相关命令

- 能够运用Dockerfile编写创建容器的脚本

- 能够搭建与使用docker私有仓库


# 实战

实战目标：

- Docker 下载 weblogic12 镜像并启动

- Docker 下载 centos:7 镜像并制作 jdk1.6镜像

- 从jdk1.6镜像制作weblogic10.6.3镜像和容器

- 部署燕赵财险核心系统开发环境。

- 修改容器启动时执行脚本。

- 下载oracle database 11g镜像，通过镜像制作oracle database 11g 



# 1 Docker简介

## 1.1 什么是虚拟化

​	在计算机中，虚拟化（英语：Virtualization）是一种资源管理技术，是将计算机的各种实体资源，如服务器、网络、内存及存储等，予以抽象、转换后呈现出来，打破实体结构间的不可切割的障碍，使用户可以比原本的组态更好的方式来应用这些资源。这些资源的新虚拟部份是不受现有资源的架设方式，地域或物理组态所限制。一般所指的虚拟化资源包括计算能力和资料存储。

​	在实际的生产环境中，虚拟化技术主要用来解决高性能的物理硬件产能过剩和老的旧的硬件产能过低的重组重用，透明化底层物理硬件，从而最大化的利用物理硬件   对资源充分利用

​	虚拟化技术种类很多，例如：软件虚拟化、硬件虚拟化、内存虚拟化、网络虚拟化(vip)、桌面虚拟化、服务虚拟化、虚拟机等等。


## 1.2 什么是Docker

​	Docker 是一个开源项目，诞生于 2013 年初，最初是 dotCloud 公司内部的一个业余项目。它基于 Google 公司推出的 Go 语言实现。 项目后来加入了 Linux 基金会，遵从了 Apache 2.0 协议，项目代码在 [GitHub](https://github.com/docker/docker) 上进行维护。

​	![](image/1-3.png)



​	Docker 自开源后受到广泛的关注和讨论，以至于 dotCloud 公司后来都改名为 Docker Inc。Redhat 已经在其 RHEL6.5 中集中支持 Docker；Google 也在其 PaaS 产品中广泛应用。

​	Docker 项目的目标是实现轻量级的操作系统虚拟化解决方案。 Docker 的基础是 Linux 容器（LXC）等技术。

​	在 LXC 的基础上 Docker 进行了进一步的封装，让用户不需要去关心容器的管理，使得操作更为简便。用户操作 Docker 的容器就像操作一个快速轻量级的虚拟机一样简单。

为什么选择Docker?

（1）上手快。

​	用户只需要几分钟，就可以把自己的程序“Docker化”。Docker依赖于“写时复制”（copy-on-write）模型，使修改应用程序也非常迅速，可以说达到“随心所致，代码即改”的境界。	

         随后，就可以创建容器来运行应用程序了。大多数Docker容器只需要不到1秒中即可启动。由于去除了管理程序的开销，Docker容器拥有很高的性能，同时同一台宿主机中也可以运行更多的容器，使用户尽可能的充分利用系统资源。

（2）职责的逻辑分类

​	使用Docker，开发人员只需要关心容器中运行的应用程序，而运维人员只需要关心如何管理容器。Docker设计的目的就是要加强开发人员写代码的开发环境与应用程序要部署的生产环境一致性。从而降低那种“开发时一切正常，肯定是运维的问题（测试环境都是正常的，上线后出了问题就归结为肯定是运维的问题）”

（3）快速高效的开发生命周期

​	Docker的目标之一就是缩短代码从开发、测试到部署、上线运行的周期，让你的应用程序具备可移植性，易于构建，并易于协作。（通俗一点说，Docker就像一个盒子，里面可以装很多物件，如果需要这些物件的可以直接将该大盒子拿走，而不需要从该盒子中一件件的取。）

（4）鼓励使用面向服务的架构

​	Docker还鼓励面向服务的体系结构和微服务架构。Docker推荐单个容器只运行一个应用程序或进程，这样就形成了一个分布式的应用程序模型，在这种模型下，应用程序或者服务都可以表示为一系列内部互联的容器，从而使分布式部署应用程序，扩展或调试应用程序都变得非常简单，同时也提高了程序的内省性。（当然，可以在一个容器中运行多个应用程序）

## 1.3 容器与虚拟机比较

​	下面的图片比较了 Docker 和传统虚拟化方式的不同之处，可见容器是在操作系统层面上实现虚拟化，直接复用本地主机的操作系统，而传统方式则是在硬件层面实现。

![](image/1-1.png)

与传统的虚拟机相比，Docker优势体现为启动速度快、占用体积小。

## 1.4 Docker 组件

### 1.4.1 Docker服务器与客户端

​	Docker是一个客户端-服务器（C/S）架构程序。Docker客户端只需要向Docker服务器或者守护进程发出请求，服务器或者守护进程将完成所有工作并返回结果。Docker提供了一个命令行工具Docker以及一整套RESTful API。你可以在同一台宿主机上运行Docker守护进程和客户端，也可以从本地的Docker客户端连接到运行在另一台宿主机上的远程Docker守护进程。

![](image/1-2.png)

### 1.4.2 Docker镜像与容器

​	镜像是构建Docker的基石。用户基于镜像来运行自己的容器。镜像也是Docker生命周期中的“构建”部分。镜像是基于联合文件系统的一种层式结构，由一系列指令一步一步构建出来。例如：

添加一个文件；

执行一个命令；

打开一个窗口。

也可以将镜像当作容器的“源代码”。镜像体积很小，非常“便携”，易于分享、存储和更新。

​	Docker可以帮助你构建和部署容器，你只需要把自己的应用程序或者服务打包放进容器即可。容器是基于镜像启动起来的，容器中可以运行一个或多个进程。我们可以认为，镜像是Docker生命周期中的构建或者打包阶段，而容器则是启动或者执行阶段。  容器基于镜像启动，一旦容器启动完成后，我们就可以登录到容器中安装自己需要的软件或者服务。

所以Docker容器就是：

​	一个镜像格式；

​	一些列标准操作；

​	一个执行环境。

​Docker借鉴了标准集装箱的概念。标准集装箱将货物运往世界各地，Docker将这个模型运用到自己的设计中，唯一不同的是：集装箱运输货物，而Docker运输软件。

和集装箱一样，Docker在执行上述操作时，并不关心容器中到底装了什么，它不管是web服务器，还是数据库，或者是应用程序服务器什么的。所有的容器都按照相同的方式将内容“装载”进去。

Docker也不关心你要把容器运到何方：我们可以在自己的笔记本中构建容器，上传到Registry，然后下载到一个物理的或者虚拟的服务器来测试，在把容器部署到具体的主机中。像标准集装箱一样，Docker容器方便替换，可以叠加，易于分发，并且尽量通用。

### 1.4.3 Registry（注册中心）

​	Docker用Registry来保存用户构建的镜像。Registry分为公共和私有两种。Docker公司运营公共的Registry叫做Docker Hub。用户可以在Docker Hub注册账号，分享并保存自己的镜像（说明：在Docker Hub下载镜像巨慢，可以自己构建私有的Registry）。

​	https://hub.docker.com/

# 2 Docker安装与启动

## 2.1 安装Docker 

​	Docker官方建议在Ubuntu中安装，因为Docker是基于Ubuntu发布的，而且一般Docker出现的问题Ubuntu是最先更新或者打补丁的。在很多版本的CentOS中是不支持更新最新的一些补丁包的。

​	由于我们学习的环境都使用的是CentOS，因此这里我们将Docker安装到CentOS上。注意：这里建议安装在CentOS7.x以上的版本，在CentOS6.x的版本中，安装前需要安装其他很多的环境而且Docker很多补丁不支持更新。

​	请直接挂载课程配套的Centos7.x镜像	

（1）yum 包更新到最新

```
sudo yum update
```

（2）安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的

```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

（3）设置yum源为阿里云

```
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

（4）安装docker

```
sudo yum install docker-ce
```

（5）安装后查看docker版本

```
docker -v
```

## 2.2 设置ustc的镜像 

ustc是老牌的linux镜像服务提供者了，还在遥远的ubuntu 5.04版本的时候就在用。ustc的docker镜像加速器速度很快。ustc docker mirror的优势之一就是不需要注册，是真正的公共服务。

[https://lug.ustc.edu.cn/wiki/mirrors/help/docker](https://lug.ustc.edu.cn/wiki/mirrors/help/docker)

编辑该文件：

```
vi /etc/docker/daemon.json  
```

在该文件中输入如下内容：

```
{
"registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}
```

## 2.3 Docker的启动与停止

**systemctl**命令是系统服务管理器指令

启动docker：

```
systemctl start docker
```

停止docker：

```
systemctl stop docker
```

重启docker：

```
systemctl restart docker
```

查看docker状态：

```
systemctl status docker
```

开机启动：

```
systemctl enable docker
```

查看docker概要信息

```
docker info
```

查看docker帮助文档

```
docker --help
```

# 3 常用命令

## 3.1 镜像相关命令

### 3.1.1 查看镜像

```
docker images
```

REPOSITORY：镜像名称

TAG：镜像标签

IMAGE ID：镜像ID

CREATED：镜像的创建日期（不是获取该镜像的日期）

SIZE：镜像大小

这些镜像都是存储在Docker宿主机的/var/lib/docker目录下

### 3.1.2 搜索镜像

如果你需要从网络中查找需要的镜像，可以通过以下命令搜索

```
docker search 镜像名称
```

NAME：仓库名称

DESCRIPTION：镜像描述

STARS：用户评价，反应一个镜像的受欢迎程度

OFFICIAL：是否官方

AUTOMATED：自动构建，表示该镜像由Docker Hub自动构建流程创建的

### 3.1.3 拉取镜像

拉取镜像就是从中央仓库中下载镜像到本地

```
docker pull 镜像名称
```

例如，我要下载centos7镜像

```
docker pull centos:7
```

### 3.1.4 删除镜像

按镜像ID删除镜像

```
docker rmi 镜像ID
```

删除所有镜像

```
docker rmi `docker images -q`
```

## 3.2 容器相关命令

### 3.2.1 查看容器

查看正在运行的容器

```
docker ps
```

查看所有容器

```
docker ps –a
```

查看最后一次运行的容器

```
docker ps –l
```

查看停止的容器

```
docker ps -f status=exited
```

### 3.2.2 创建与启动容器

创建容器常用的参数说明：

创建容器命令：docker run

 -i：表示运行容器

 -t：表示容器启动后会进入其命令行。加入这两个参数后，容器创建就能登录进去。即分配一个伪终端。

 --name :为创建的容器命名。

 -v：表示目录映射关系（前者是宿主机目录，后者是映射到宿主机上的目录），可以使用多个－v做多个目录或文件映射。注意：最好做目录映射，在宿主机上做修改，然后共享到容器上。

 -d：在run后面加上-d参数,则会创建一个守护式容器在后台运行（这样创建容器后不会自动登录容器，如果只加-i -t两个参数，创建后就会自动进去容器）。

 -p：表示端口映射，前者是宿主机端口，后者是容器内的映射端口。可以使用多个-p做多个端口映射

（1）交互式方式创建容器

```
docker run -it --name=容器名称 镜像名称:标签 /bin/bash
```

这时我们通过ps命令查看，发现可以看到启动的容器，状态为启动状态  

退出当前容器

```
exit
```

（2）守护式方式创建容器：

```
docker run -di --name=容器名称 镜像名称:标签
```

登录守护式容器方式：

```
docker exec -it 容器名称 (或者容器ID)  /bin/bash
```

### 3.2.3 停止与启动容器

停止容器：

```
docker stop 容器名称（或者容器ID）
```

启动容器：

```
docker start 容器名称（或者容器ID）
```

### 3.2.4 文件拷贝

如果我们需要将文件拷贝到容器内可以使用cp命令

```
docker cp 需要拷贝的文件或目录 容器名称:容器目录
```

也可以将文件从容器内拷贝出来

```
docker cp 容器名称:容器目录 需要拷贝的文件或目录
```

### 3.2.5 目录挂载

我们可以在创建容器的时候，将宿主机的目录与容器内的目录进行映射，这样我们就可以通过修改宿主机某个目录的文件从而去影响容器。
创建容器 添加-v参数 后边为   宿主机目录:容器目录，例如：

```
docker run -di -v /usr/local/myhtml:/usr/local/myhtml --name=mycentos3 centos:7
```

如果你共享的是多级的目录，可能会出现权限不足的提示。

这是因为CentOS7中的安全模块selinux把权限禁掉了，我们需要添加参数  --privileged=true  来解决挂载的目录没有权限的问题

### 3.2.6 查看容器IP地址

我们可以通过以下命令查看容器运行的各种数据

```
docker inspect 容器名称（容器ID） 
```

也可以直接执行下面的命令直接输出IP地址

```
docker inspect --format='{{.NetworkSettings.IPAddress}}' 容器名称（容器ID）
```

### 3.2.7 删除容器 

删除指定的容器：

```
docker rm 容器名称（容器ID）
```

# 4 应用部署

## 4.1 Weblogic部署

（1）拉取Weblogic镜像



 [https://www.docker.com/products/docker-hub](https://www.docker.com/products/docker-hub)
 ![](image/20220213185612.png)


``` 
docker pull ismaleiva90/weblogic12
```

（2）创建容器

```
docker run -d -p 49163:7001 -p 49164:7002 -p 49165:5556 ismaleiva90/weblogic12:latest
```

-d 表示后台运行

-p 代表端口映射，格式为  宿主机映射端口:容器运行端口


 (3) 容器提供的命令

```
http://localhost:49163/console
```

```
User: weblogic
```

```
Pass: welcome1
```

 ![](image/20220213185820.png)

 ![](image/weblogic1.png)

## 4.2 jenkins 部署

###Jenkins简介

  ![](image/20220213190953.png)

###Jenkins官网
 
  [https://www.jenkins.io/](https://www.jenkins.io/)

  ![](image/20220213191136.png)
  
  [GitHub](https://github.com/jenkinsci/jenkins)

  ![](image/20220213191352.png)

###Jenkins的Docker容器化安装

  [DockerHub](https://hub.docker.com/_/jenkins)

```
docker pull jenkins
```


  ![](image/20220213191545.png)

  ![](image/20220213191759.png)

###我们只需要知道怎么去使用他就可以了。
   
```
docker run -p 8080:8080 -p 50000:50000 jenkins
```

  ![](image/20220213191919.png)

jenkins 启动的时候会在控制台输出一段密码稍后使用

  ![](image/20220213192415.png)

首次登录的时候会需要输入密码，复制刚才控制台输出的密码即可

  ![](image/20220213192538.png)

Jenkins 解锁完成 接下来就可以配置插件了，至此，Jenkins安装完成，后续我会继续介绍Jenkins的功能，在此不再赘述。

  ![](image/20220213192939.png)

## 4.1 通过centos:7镜像制作weblogic10容器基于jdk1.6环境

### pull centos:7镜像

<font color='red' size='4' ><strong>注意:不要直接使用docker pull centos这样会下载lastest版本</strong></font>

  ![](image/20220213193855.png)

我们下载centos:7这个版本

```
docker pull centos:7
```

  ![](image/20220213195009.png)


### 编写 Dockerfile


	# CentOS with JDK 1.6
	# Author   fg
	
	# build a new image with basic  centos
	FROM centos:centos7
	# who is the author
	MAINTAINER fg
	
	# make a new directory to store the jdk files
	RUN mkdir /usr/local/java
	
	# copy the jdk  archive to the image,and it will automaticlly unzip the tar file
	ADD jdk1.6.0_45.tar.gz /usr/local/java/
	
	# make a symbol link 
	RUN ln -s /usr/local/java/jdk1.6.0_45 /usr/local/java/jdk
	
	# set environment variables
	ENV JAVA_HOME /usr/local/java/jdk
	ENV CLASSPATH .:${JAVA_HOME}/lib.tools.jar
	ENV PATH ${JAVA_HOME}/bin:$PATH

因为通过Dockerfile直接制作的的容器使用的是centos7镜像，我们需要做一些准备工作

将jdk1.6放入到宿主机/app/softwares/weblogic1036 文件夹下面
将上边的脚本也放入到此文件夹下面 文件名称为 Dockerfile

在此文件夹下面构建镜像

```
docker build -t="weblogic1036" .
```

 ![](image/20220213203507.png)

构建完成，没有报错

查看镜像

```
docker images
```

 ![](image/20220213204858.png)


启动镜像成为容器

```
docker run -d -i -t -v /app:/app -p 7001:7001 --name weblogic1036 --privileged=true  weblogic1036:latest /bin/bash
```

 ![](image/20220213205035.png)

-v 为文件挂载 前一个为宿主机，后一个为容器 挂载的目的就是要安装软件部署项目

--privileged=true 的意义在于要给容器能够操作挂载目录文件的权限

-p 为从从容器映射到宿主机的哪一个端口



接下来进入到容器当中

```
docker exec -it weblogic1036 /bin/bash
```

 ![](image/20220213205334.png)

容器中的java环境就安装好了

### 关于Dockerfile的学习

 [https://www.runoob.com/docker/docker-dockerfile.html](https://www.runoob.com/docker/docker-dockerfile.html)

 ![](image/20220213205614.png)
 ![](image/20220213205710.png)
 ![](image/20220213205947.png)
 ![](image/20220213210043.png)

 这里我只摘取一部分

### 从制作好的jdk镜像commit安装weblogic1036

首先保证宿主机/app/softwares/wls1036_generic.jar 文件存在

```
java -jar wls1036_generic.jar
```
 ![](image/20220213210925.png)
 Next
 ![](image/20220213211133.png)
 我们这里放入到/oracle/middleware 下面, 注意这里操作的是容器，不要与挂载的容器目录相同以免后续软件运行不正常。
 ![](image/20220213211522.png)
 选择1 next
 ![](image/20220213211644.png)
 选择3 next

 Weblogic 的 Register for Security Updates 默认为Yes 如果填Yes就需要填邮箱和密码之类的，很烦，直接NO。
 ![](image/20220213212714.png)
 在这里我们进行典型安装 
 
 1 回车
 ![](image/20220213213023.png)
 我们使用安装好的jdk1.6
 next 回车
 ![](image/20220213213522.png)
 next
 ![](image/20220213213727.png)
 next
 ![](image/20220213213910.png)
 next

 安装完成
 ![](image/20220213214025.png)
 
 容器中的weblogic10就安装好了
 ![](image/20220213214137.png)

####创建domain域

 进入common/bin 目录下
 ![](image/20220213214515.png)
 执行

``` 
 ./config.sh
```

 ![](image/20220213214618.png)
 
 1 回车
 ![](image/20220213215011.png)
 next
 ![](image/20220213215047.png)

 在这里名字要重新起一个
 base_domain_7001
 ![](image/20220213215140.png)
 1为选择 2为放弃

 我们这里选择1
 ![](image/20220213215309.png)
 
 domain域的路径就使用这个路径

 next
 ![](image/20220213215422.png)
 
 这里我们需要设置weblogic登录的密码
 
 用户名:weblogic
 密码：weblogic123
 确认密码：weblogic123
 ![](image/20220213215653.png)

 ![](image/0220213215824.png)

 ![](image/20220213215917.png)
 
 description 就不写了

 直接Next
 这里默认是开发环境
 Next
 ![](image/20220213220026.png)

 这里为默认安装好的jdk

 next
 ![](image/20220213220238.png)

 在这里选择第一个

 next
 ![](image/20220213220431.png)

 查看一下确保无误

 next
 ![](image/20220213220601.png)

 这样domain域就创建好了
 ![](image/20220213220654.png)

#### 启动weblogic

 直接看图吧
 ![](image/20220213220847.png)
 


 启动成功
 ![](image/20220213221012.png)

 部署燕赵核心系统开发环境

 配置好数据源部署好项目之后启动开发环境发现报错
 ![](image/20220213222326.png)

 原因在于当前的容器还不支持中文环境
 
```
docker ps
```

```
docker exec -it 42f4018ae023 /bin/bash
```

 ![](image/20220213222725.png)


```
locale
```

 ![](image/20220213223002.png)

 
```
locale -a
```

 ![](image/20220213223106.png)


安装中文环境
下载中文语言包

```
yum -y install kde-l10n-Chinese && yum -y reinstall glibc-common
```
 
 ![](image/20220213223334.png)

```
locale -a
```
 
 ![](image/20220213223518.png)

en_US.utf8 

修改~/.bashrc

	export LC_ALL=en_US.utf8 
	export LANG=en_US.UTF-8 

修改 /etc/locale.conf：

	LANG="en_US.UTF-8"


使修改生效
```
source ~/.bashrc
```


docker容器执行：

```
localedef -c -f UTF-8 -i en_US en_US.UTF-8
```

 ![](image/20220213224223.png)


还需要修改weblogic其中的一个脚本

	路径user_projects/domains/base_domain_7001/bin
	文件setDomainEnv.sh
	
	JAVA_OPTIONS="${JAVA_OPTIONS} ${JAVA_PROPERTIES} -Dwlw.iterativeDev=${iterativeDevFlag} -Dwlw.testConsole=${testConsoleFlag} -Dwlw.logErrorsToConsole=${logErrorsToConsoleFlag} -Dfile.encoding=UTF-8"



重启容器中的weblogic1036

   发现还是没有解决，目前没有很好的办法，那只能把“开发用”改成“dev”试试了

  ![](image/20220213230345.png)

  到宿主机的WEB-INF目录下
  
```
mv app-config-acegi-security_开发用.xml app-config-acegi-security_dev.xml
```

```
vim web.xml
```

	修改 
	app-config-acegi-security_开发用.xml app-config-acegi-security_dev.xml

进入到容器重启weblogic


```
 ./startWebLogic.sh 
```

内存溢出
需要设置堆栈

 ![](image/20220213230843.png) 


直接通过脚本进行后台启动

nohupstart.sh

	DATE=`date "+%Y%m%d%H%M"`
	export DATE
	DOMAIN_DIR="/oracle/middleware/user_projects/domains/base_domain_7001"
	# NOHUP_LOG="${DOMAIN_DIR}/servers/AdminServer/logs/base_domain7001.out"
	# 设置log输出至宿主机
	APPLOGS_DIR="/app/application/logs/base_domain7001.out"
	DOMAIN_NAME1="base_domain7001"
	
	USER_MEM_ARGS="-Xms2048m -Xmx2048m -Xss256k -XX:NewSize=1024m -XX:MaxNewSize=1024m -XX:PermSize=512m -XX:MaxPermSize=1024m -XX:+HeapDumpOnOutOfMemoryError -DdomainName=base_domain7001"
	export USER_MEM_ARGS
	
	# kill -9 `ps -ef |grep java  |grep ${DOMAIN_NAME1} | awk '{print $2}'`
    # 脚本直接把log挂载到宿主机
	nohup ${DOMAIN_DIR}/startWebLogic.sh 1>${APPLOGS_DIR} 2>&1 &
	
	sleep 5
	echo Please see ${APPLOGS_DIR}
	tail -f ${APPLOGS_DIR} 


把编写好的脚本上传至宿主机/app/application

  ![](image/20220213231753.png)


在容器内启动/app/application/nohupstart.sh

  ![](image/20220213232224.png)

启动完成，访问一下目标

核心环境能够正常打开
  ![](image/20220213232425.png)


将容器commit 称为一个镜像

```
docker commit weblogic1036 weblogic1036:latest
```

查看刚刚覆盖commit的镜像

```
docker images
```

 ![](image/20220213233030.png)

我们把容器down一下

```
docker stop weblogic1036
```

启动容器

```
docker start weblogic1036
```

发现容器虽然启动，但容器内部weblogic没有启动

```
docker exec -it weblogic1036 /bin/bash
```

```
Jps
```

 ![](image/20220213233722.png)

我们通过刚才commit的镜像重新启动一个容器并把脚本挂载上去

```
docker stop weblogic1036
```
  

这样启动就直接执行脚本并把日志输出到控制台
 
```
docker run -it -v /app:/app -p 7001:7001 --name weblogic1036new  --privileged=true weblogic1036 /bin/bash /app/application/nohupstart.sh
```

这样有个缺点就是 ctrl+c 就退出了。

我们加上 -d

```
docker run -d -it -v /app:/app -p 7001:7001 --name weblogic1036new  --privileged=true weblogic1036 /bin/bash /app/application/nohupstart.sh
```

再到宿主机里面 logs下面查看日志

```
tail -100f base_domain7001.out 
```
 ![](image/20220213235259.png)

核心环境正常打开
 ![](image/20220213232425.png)

然后我们就可以通过

	docker stop container
	docker start container

这两条命令关闭和启动服务，并且启动服务的时候自动执行脚本

 ![avatar](image/20220213235655.png)

 ![avatar](image/20220213235743.png)

 ![avatar](image/20220213235821.png)


# END