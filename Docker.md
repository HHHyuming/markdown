# Docker



## image 镜像

#### 拉取docker hub镜像

```
docker pull ubuntu:18.04

docker pull mysql
```

#### 查找镜像

```
docker search imageName
```

#### 列出本地镜像

```
docker images

docker image
-a 列出所有镜像
-q 显示镜像ID
```

#### 删除镜像

```
docker image rm [image]
docker rmi [image]
docker rmi -f [image]  # 强制删除，
```

#### 镜像打tar包

```
docker save image_name -o file_path
```

#### 加载镜像

```
docker load -i file_path
```

#### 发布一个镜像

```
docker push new_image_name
```

#### 根据Dockerfile 构建容器

```
docker build -t image_name dockerfile

--tag, -t：
镜像的名字及标签，通常 name:tag 或者 name 格式；可以在一次构建中为一个镜像设置多个标签。
-f：指定要使用的Dockerfile路径；
```





## 容器

#### 运行容器

```
docker run [image]
docker run -it [image] bash # 交互式

docker start Name/Id
docker stop Name/Id
docker restart Name/ID
docker kill Name/Id  # 强制杀死一个容器
docker rm -f ${docker ps -aq}  # 强制删除所有容器
docker rm Name/ID # 删除单个容器
```

#### 查看容器

```
docker ps
-a 查看所有容器
-q 显示容器ID
```

#### 查看容器日志

```
docker logs [Name/Id]

docker logs -f [Name/Id]  #实时查看

```

#### 查看容器进程

```
docker top Name/Id
```

#### 提交容器修改

```
docker commit -a "作者信息" -m "镜像描述" 容器ID 镜像name
```

#### 容器文件拷贝

```
 docker cp Name:/container_path to_path #将容器内文件复制到宿主机
 docker cp localpath ID:/container_path 
```

#### 进入容器

```
docker exec -it container_id bash

```

#### 检查容器状态

```
sudo docker inspect container_id
```







## 





