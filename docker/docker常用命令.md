## 如果遇到启动端口问题： 重启docker

``systemctl restart docker``



##### 查找镜像

```shell
docker search httpd
```



##### 载入镜像

```shell
docker pull training/webapp  # 载入镜像

docker images # 查看镜像
docker run -d -P training/webapp python app.py  # 后台启动镜像
docker run -it 容器id  /bin/bash  # 进行交互页面

docker exec -it /bin/bash # 进入一个激活的容器
```



##### 查看容器

````shell
docker ps # 运行中的容器
docker ps -aq # 查看所有id 配合docker remove使用批量删除

docker rmi  容器id # 删除容器
````



##### 查看日志

```shell
docker logs -f bf08b7f2cd89
```



##### 构建镜像

**docker build** ：

````shell
# 我们需要创建一个 Dockerfile 文件，其中包含一组指令来告诉 Docker 如何构建我们的镜像。
docker build -t runoob/centos:6.7 .

# 参数说明：
	# -t ：指定要创建的目标镜像名
	# . ：Dockerfile 文件所在目录，可以指定Dockerfile 的绝对路径
````

##### 设置镜像标签

```
docker tag 860c279d2fec runoob/centos:dev
```

##### 端口映射

```shell
docker run -d -p 5000:5000 training/webapp python app.py

# 两种方式的区别是:
	-P :是容器内部端口随机映射到主机的高端口。
	-p : 是容器内部端口绑定到指定的主机端口
```



