title: "Docker学习系列（一）之初识Docker"

date: 2015-11-13 20:36:35

tags: docker

categories: docker
------------------

##Docker资源

[Docker官方主页](https://www.docker.com/)

[Docker Hub](https://hub.docker.com/)

[Docker官方博客](https://blog.docker.com/)

[Docker官方文档](https://docs.docker.com/)

[Docker快速入门指南](https://docs.docker.com/mac/started/)

##Docker简介

> Docker是一个能够把开发的应用程序自动部署到容器的开源引擎。

##Docker可以提供以下东西：

-	提供一个简单、轻量的建模方式
-	职责的逻辑分离：开发和运维的分离
-	快速、高效的开发生命周期
-	鼓励使用面向服务的架构：Docker推荐单个容器只运行一个应用程序或进程

##Docker组件

###Docker客户端和服务器

Docker客户端和服务器是一个C/S架构，客户端像服务器发出请求，服务器将完成所有工作，并返回结果。可以在同一台宿主机上运行Docker服务器和客户端，也可以从本地的Docker客户端连接到运行在另一台宿主机上的远程Docker守护进程。

###Docker镜像

Docker镜像是Docker生命周期中的“构建”部分。镜像是由一系列指令一步一步构建出来的。

###Registry

Docker用Registry来保存用户构建的镜像。`Registry`分为`public`和`private`两种。Docker公司运行的公共`Registry`叫做Docker Hub.

###Docker容器

Docker可以帮你构建和部署容器，你只需要把自己的应用程序或服务打包放进容器即可。容器是基于镜像启动起来的，容器中可以运行一个或多个进程。可以认为，镜像是Docker生命周期中的构建或打包阶段，而容器则是启动或执行阶段。总结起来，Docker容器就是：

-	一个镜像格式；
-	一系列标准的操作；
-	一个执行环境

![docker-architecture.jpg](/img/docker-architecture.jpg)

Docker的技术组件
----------------

Docker可以运行在任何安装了Linux内核的x64主机上。Linux内核的命名空间(namespace)，用于隔离文件系统、进程和网络。

-	文件系统隔离： 每个容器都有自己的root文件系统。
-	进程隔离： 每个容器都运行在自己的进程环境中。
-	网络隔离： 容器间的虚拟网络几口和IP地址都是分开的。
-	资源隔离和分组：使用cgroups(即control group，Linux的内核特性之一)将CPU和内存之类的资源独立分配给每个Docker容器
-	写时复制： 文件系统都是荣国写时复制创建的，这就意味着文件系统式分层的、快速的，而且占用的磁盘空间更小。
-	日志： 容器产生的STDOUT、STDERR和STDIN这些IO流都会被收集并记录日志，用来进行日志分析和故障排除。
-	交互式shell：用户可以创建一个伪tty终端，将其连接到STDIN，为容器提供一个交互式的shell。

###参考
[Understand the architecture](https://docs.docker.com/engine/introduction/understanding-docker/)
[为什么要使用Docker](http://dockerpool.com/static/books/docker_practice/introduction/why.html)