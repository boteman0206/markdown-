#### 镜像

Docker 镜像是用于创建 Docker 容器的模板，比如 Ubuntu 系统。

#### 容器

docker利用容器技术，可以运行一个或者一组应用，是镜像运行时的实体。

#### 仓库

仓库就是存放镜像的地方



##### 使用命令：

````linux
docker  version
docker info 
docker --help 
````

##### 搜索镜像

`docker search mysql --filter=stars=3000` ： 查找镜像的标星数大于3000的

##### 下载镜像

`docker pull mysql`  : 默认下载最新版本， 可以指定版本 docker pull mysql:5.7

##### 删除镜像

`docker rmi -f 镜像id`

`docker rmi -f $(docker images -aq)` : 批量删除所有的镜像









