title: "Docker学习系列（二）之在Mac OS上安装Docker"

date: 2015-11-16 22:54:14

tags: mac 安装 docker

categories: docker

---

由于笔者使用的是Mac OS，所以在这里介绍一下如何在Mac OS中搭建Docker 环境。 我们可以使用[Boot2Docker](https://github.com/boot2docker/boot2docker)工具快速上手Docker.它能够帮我们在Mac OS下轻松搭建整个Docker环境，其主要包括：

-	一个VirtualBox的虚拟机（也称Docker主机）
-	Docker运行环境
-	Docker镜像或者容器的管理工具。

为了使用Boot2Docker创建Docker主机，我们需要设置boot2docker

1.	初始化Docker主机

	```
	boot2docker init
	```

2.	配置本地系统与Docker主机之间的端口映射

	```
	vboxmanage modifyvm "boot2docker-vm" --natpf1 "docker, tcp, 127.0.0.1,2376,,2376"
	```

3.	启动Docker主机

	```
	boot2docker up
	```

4.	导出Docker运行时环境变量

	```
	export DOCKER_HOST=tcp://192.168.59.103:2376
    export DOCKER_CERT_PATH=/Users/fgcui/.boot2docker/certs/boot2docker-vm
    export DOCKER_TLS_VERIFY=1
	```

5.	验证Docker运行环境

	```
	docker images
	```

	如果显示当前可用的Docker镜像，就证明你的docker环境搭建成功啦~

	![docker-list.jpg](/img/docker-list.jpg)
