##### Dockerfile文件

````
# 构建步骤
1： 创建镜像文件 Dockerfile
2： docker build 构建成为一个镜像
3： docker run 运行镜像
4： docker push 发布镜像
````

##### 基础命令

````
1： 每一个的保留关键字必须是大写字母
2： 执行是从上到下的
3： #表示注释
4： 每一个指令都会创建一个新的镜像层，并提交！

FROM: # 基础镜像 
MAINTAINER:  # 创作者谁写的 
RUN： # 构建镜像需要执行的命令
ADD： # 添加文件，内容
WORKDIR: # 镜像的工作目录
VOLUME: # 挂载的目录
EXPOSE: # 暴露端口  替代-p命令
CMD:  # 指定容器启动的时候要执行的指令， 只有最后一个会生效， 可被替代
ENTRYPOINT: # 和cmd差不多， 区别： 可以追加命令
COPY: # 复制copy文件到镜像中
ENV： # 构建的时候设置的环境变量

````

##### 实战测试

````dockerfile
# 编写Dockerfile文件
FROM centos

MAINTAINER WEI.PENG

ENV MYPATH /usr/local

WORKDIR  ${MYPATH}

RUN yum -y install vim 
RUN yum -y install net-tools

EXPOSE 80
CMD echo ${MYPATH}
CMD echo "hello world!"
CMD /bin/bash
````



