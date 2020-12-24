## docker

### 镜像

```dockerfile
查看镜像
docker images 
获取镜像
docker pull image_name 
# 上传一个docker镜像
docker push image_name
# 导出镜像到本地文件
docker save -o ubuntu16.04.bak.tar ubuntu16.04 
# 载入镜像
docker load --input ubuntu16.04.bak.tar
docker load < ubuntu16.04.bak.tar
# 移出本地镜像
docker rmi ubuntu16.04
# 启动容器
docker run 
# 从本地文件系统导入docker
cat ubuntu16.04.bak.tar | docker import - ubuntu16.04

```



### 容器

```
启动容器有两种方式，一种是基于镜像新建一个容器并启动，另外一个是将在终止状态（stopped）的容器重新启动。

因为 Docker 的容器实在太轻量级了，很多时候用户都是随时删除和新创建容器。

```

主要命令 docker run

```
docker run -t -i ubuntu:16.04 /bin/bash
-t 分配一个终端并绑定容器的标准输入上，在后台运行
-i 让容器的标准输入保持打开
-d  让docker容器以daemon形式运行，即守护态(后台运行)

# 启动一个已经终止的容器
docker start
# 查看容器信息
docker ps
 -a 查看所有容器进程
# 查看容器日志 
docker logs 容器名称
-f 实时查看日志
# 停止容器
docker stop 容器ID
# 进入容器
docker attach 容器ID
# 导入容器
docker import
example : cat ubuntu.tar | docker import - test/ubuntu:v1.0
也可以通过网络路径
docker import http://example.com/exampleimage.tgz example/imagerepo
# 导出容器
docker export 7691a814370e > ubuntu.tar
# 删除容器
docker rm 容器名称/容器ID


```

### 数据卷

```dockerfile
数据卷是一个可供一个或多个容器使用的特殊目录，它绕过 UFS，可以提供很多有用的特性：

数据卷可以在容器之间共享和重用
对数据卷的修改会立马生效
对数据卷的更新，不会影响镜像
卷会一直存在，直到没有容器使用
*数据卷的使用，类似于 Linux 下对目录或文件进行 mount。


# 下面创建一个 web 容器，并加载一个数据卷到容器的 /webapp 目录。
$ sudo docker run -d -P --name web -v /webapp training/webapp python app.py
# 挂载一个本地主机文件作为数据卷 下面的命令加载主机的 /src/webapp 目录到容器的 /opt/webapp 目录
$ sudo docker run -d -P --name web -v /src/webapp:/opt/webapp training/webapp python app.py

# 数据卷容器
# 首先，创建一个命名的数据卷容器 dbdata：
$ sudo docker run -d -v /dbdata --name dbdata training/postgres echo Data-only container for postgres
# 然后，在其他容器中使用 --volumes-from 来挂载 dbdata 容器中的数据卷。
$ sudo docker run -d --volumes-from dbdata --name db1 training/postgres
$ sudo docker run -d --volumes-from dbdata --name db2 training/postgres
# 还可以使用多个 --volumes-from 参数来从多个容器挂载多个数据卷。 也可以从其他已经挂载了数据卷的容器来挂载数据卷。
$ sudo docker run -d --name db3 --volumes-from db1 training/postgres

```



### 网络

```dockerfile
docker run 
	#-P 当使用 -P 标记时，Docker 会随机映射一个 49000~49900 的端口到内部容器开放的网络端口。
	sudo docker run -d -P training/webapp python app.py
	
	# 接口地址映射  本地-port:容器-port
	 docker run -d -P training/webapp python app.py
	 docker run -d -p 127.0.0.1:5000:5000 training/webapp python app.py
	 
	 # -p 小p 指定多个端口的绑定
	 $ sudo docker run -d -p 5000:5000  -p 3000:80 training/webapp python app.py
   	# --name 标记容器自定义的名称
   	$ sudo docker run -d -P --name web training/webapp python app.py

```



### docker file 文件shell

```dockerfile
docker build .   # 其中 "." 标识当前目录下的dockerfile

1.FROM 指令告诉 Docker 使用哪个镜像作为基础
3. MAINTAINER 维护者信息
2.RUN 开头的指令会在创建中运行，比如安装一个软件包，在这里使用
4.用 EXPOSE 命令来向外部开放端口；  格式为 EXPOSE <port> [<port>...]。
告诉 Docker 服务端容器暴露的端口号，供互联系统使用。在启动容器时需要通过 -P，Docker 主机会自动分配一个端口转发到指定的端口。
5.用 CMD 命令来描述容器启动后运行的程序等 指定启动容器时执行的命令，每个 Dockerfile 只能有一条 CMD 命令。如果指定了多条命令，只有最后一条会被执行。
6. ENV 指定启动容器时执行的命令，每个 Dockerfile 只能有一条 CMD 命令。如果指定了多条命令，只有最后一条会被执行。
7.ADD
格式为 ADD <src> <dest>。
该命令将复制指定的 <src> 到容器中的 <dest>。 其中 <src> 可以是Dockerfile所在目录的一个相对路径；也可以是一个 URL；还可以是一个 tar 文件（自动解压为目录）。
8.格式为 COPY <src> <dest>。
复制本地主机的 <src>（为 Dockerfile 所在目录的相对路径）到容器中的 <dest>。
当使用本地目录为源目录时，推荐使用 COPY。

9.VOLUME
格式为 VOLUME ["/data"]。
创建一个可以从本地主机或其他容器挂载的挂载点，一般用来存放数据库和需要保持的数据等。

10.USER
格式为 USER daemon。
指定运行容器时的用户名或 UID，后续的 RUN 也会使用指定用户。

11.WORKDIR
格式为 WORKDIR /path/to/workdir。
为后续的 RUN、CMD、ENTRYPOINT 指令配置工作目录。
可以使用多个 WORKDIR 指令，后续命令如果参数是相对路径，则会基于之前命令指定的路径。例如


# example

# put my local web site in myApp folder to /var/www
FROM ubuntu:16.04
RUN apt-get install python3 mysql redis
RUN pip3 install 
ADD myApp /var/www
# expose httpd port
EXPOSE 80
# the command to run
CMD ["/usr/sbin/apachectl", "-D", "FOREGROUND"]
```

