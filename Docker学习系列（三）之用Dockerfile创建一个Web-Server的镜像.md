title: "Docker学习系列(三)之用Dockerfile创建一个Web Server的镜像"

date: 2015-12-26 22:56:15

tags: Dockerfile 

categories: docker

---

> Dockerfile采用基于DSL语法的指令来构建一个Docker镜像，之后用docker build命令基于该Dockerfile种的指令构建一个新的镜像。

今天，我们用Dockerfile创建一个简单的Webserver的镜像。

## 准备

创建Dockerfile

```
mkdir web_server && cd web_server

touch Dockerfile
```

## 编辑Dockerfile
用你熟悉的编辑器编辑Dockerfile文件

```
#Version: 0.0.1
FROM ubuntu:14.04
MAINTAINER Fugang<fgcui@outlook.com>
# Update the image with the latest packages (recommended)
RUN apt-get update
# Install nginx package
RUN apt-get install -y nginx
# Craete index.html file and input some words into index.html
RUN echo "Hi, I am in your container" > /usr/share/nginx/html/index.html
#Set up port
EXPOSE 80
```

如上，Dockerfile由一系列指令和命令参数组成。每条指令，如`FROM`，都必须为*大写字母*,且后面要跟一个参数。Dockerfile中的指令会按顺序从上到下执行，所以应该根据需要合理调整指令顺序。

Docker大体上按照如下流程执行Dockerfile中的指令。

* Docker从基础镜像运行一个容器
* 执行一条指令，对容器做出修改
* 执行类似于docker commit的操作，提交一个新的镜像层
* Docker 在基于刚提交的镜像运行一个新的容器
* 执行Dockerfile中的下一条命令，知道所有的指令都执行完毕

**注意**

`EXPOSE`这条指令告诉Docker该容器内的应用程序将会使用容器的指定端口。但这并不意味着可以自动访问任意容器中服务的端口（这里是80）。出于安全原因，Docker并不会自动打开该端口，而是需要在使用docker run运行容器时指定需要打开哪些端口。

## 基于Dockerfile构建新的镜像

执行`docker build`命令时，Dockerfile中的所有指令都会被执行并且提交，并且在所有执行成功执行结束之后，返回一个新的镜像。

我们使用下面的命令基于刚刚的Dockerfile文件构建一个新的镜像

```
docker build -t="fgcui/webserver:v0.0.1" .
```
-t 参数为新镜像设置了仓库(Repository name (and optionally a tag) for the image)和名称。

最后的`.`告诉Docker到本地目录中找Dockerfile文件。当然也可以指定一个Github的源地址来指定Dockerfile的位置.

当Dockerfile中的每条指令都执行成功之后，会返回新镜像的ID.

![build_success.png](/img/build_success.png)

## 指令执行失败肿么办

在build的过程中，有可能会由于网络原因或者其他错误导致某些指令执行失败，当遇到这种情况时，我们该如何调试呢？

比如，我在Dockerfile中把软件包`nginx`写成`ngonx`。则执行指令时会报下面的异常。

![build_error.png](/img/build_error.png)

这时候我需要调试一下这次失败，可以用`docker run`命令来基于这次构建的到目前位置已经成功的最后一个创建的容器，它的ID为`734f43c65554`

执行下面的命令

```
docker run -t -i 734f43c65554 /bin/bash
```

这个时候可以在容器中再次运行

```
apt-get install -y nginx
```
执行正确了之后，一旦解决了这个问题，就可以退出当前的容器，使用正确地包名修改Dockerfile文件，之后在尝试进行构建。

**注意**

在docker 的构建镜像的过程中，它会将之前创建成功的镜像层看做缓存，比如刚刚的例子中，第一部到第三步之间，我们没有修改任何东西，因此Docker会将之前构建是创建的镜像当做缓存并看做新的开始点，从而节省大量的时间。如果你不想存在缓存，可以在执行docker build的时候加上 `--no-cache` 参数.

## 查看新镜像

我们使用`docker images`命令查看新创建的镜像

```
docker images fgcui/webserver
```

![docker_image.png](/img/docker_image.png)

如果想深入探究这个镜像是怎么来的，可以使用
```
docker history image_name
```
命令来查看更多细节。

## 从新镜像启动容器

我们可以根据新构建的镜像启动一个新的容器，来检查我们的构建工作是否一切正常。

```
docker run -d -P --name webserver fgcui/webserver:v0.0.1 nginx -g "daemon off;"
```
我们使用`docker run`命令启动了一个名为webserver的新容器。

![docker_run.png](/img/docker_run.png)

`-d`参数告诉Docker咦分离(detached)的方式在后台运行，这种方式非常时候运行类似Nginx守护进程这样的需要长时间运行的进程。

`nginx -g "daemon off;`这将以前台运行的方式启动Nginx，来我作为我们的Web server

`-p` 指定docker在运行时应该公开哪些网络端口给外部(宿主机)。
使用`docker ps -l`命令查看容器的端口分配情况。

![docker_port.png](/img/docker_port.png)

## 将镜像推送到Docker Hub上

通过`docker login`命令，输入用户名，密码之后,通过

```
docker push fgcui/webserver
```
将刚刚创建的镜像推送到自己的Docker Hub上。当然也可以绑定github和docker hub，当有新的代码push到github上时，会它会trigger DockerHub，自动的构建新的镜像。此部分信息请参考：
[Automated Builds on Docker Hub](https://docs.docker.com/docker-hub/builds/)

## 删除镜像

可以通过

```
docker rmi fgcui/webserver
```
将本地的Docker image删除.

如果想删除Docker Hub上的镜像，可以登录到Docker Hub，手动将其删除

