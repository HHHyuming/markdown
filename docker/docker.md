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
-t 分配一个终端并绑定容器的标准输入上，在后台运行
-i 让容器的标准输入保持打开
```





### docker file 文件shell

```dockerfile
docker build .   # 其中 "." 标识当前目录下的dockerfile

1.FROM 指令告诉 Docker 使用哪个镜像作为基础
2.RUN开头的指令会在创建中运行，比如安装一个软件包，在这里使用 apt-get 来安装了一些软件
3.还可以利用 ADD 命令复制本地文件到镜像；
4.用 EXPOSE 命令来向外部开放端口；
5.用 CMD 命令来描述容器启动后运行的程序等




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

