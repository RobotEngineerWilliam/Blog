---
title: Docker指南
date: 2024-07-29 14:18:16
aim: 为每个过往项目建立dockerfile
---

## 基本概念

- **Docker 把应用程序及其依赖，打包在 image 文件里面。** image 文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。实际开发中，一个 image 文件往往通过继承另一个 image 文件，加上一些个性化设置而生成。

- 站在 Docker 的角度，软件就是容器的组合：业务逻辑容器、数据库容器、储存容器、队列容器......Docker 使得软件可以拆分成若干个标准化容器，然后像搭积木一样组合起来。微服务（microservices）的思想：软件把任务外包出去，让各种外部服务完成这些任务，软件本身只是底层服务的调度中心和组装层。每个容器承载一个服务。一台计算机同时运行多个容器，从而就能很轻松地模拟出复杂的微服务架构。

## 常用操作

- 换源
  
  > 1、在/etc/docker/下修改（如有）或创建daemon.json文件
  > 
  > ```shell
  > gedit /etc/docker/daemon.json
  > ```
  > 
  > 2、把以下内容复制进去：
  > 
  > ```shell
  > {
  >     "registry-mirrors": [
  >         "https://registry.hub.docker.com",
  >     ]
  > }
  > ```
  > 
  > 3、重启docker
  > 
  > ```shell
  > systemctl restart docker
  > ```
  > 
  > 4、查看是否更换成功
  > 
  > ```shell
  > docker info
  > ```

- 容器控制
  
  ```shell
  docker image pull ***(image)
  docker container run ***(image:tag)
  e.g. docker container run -p 8000:3000 -it koa-demo:0.0.1 /bin/bash
  docker container ls -all
  docker container kill ***(containID)
  docker container rm ***(containID)
  ```
  
    备注：
  
  > -p参数：容器的 3000 端口映射到本机的 8000 端口。
  > -it参数：容器的 Shell 映射到当前的 Shell，然后你在本机窗口输入的命令，就会传入容器。
  > koa-demo:0.0.1：image 文件的名字（如果有标签，还需要提供标签，默认是 latest 标签）。
  > /bin/bash：容器启动以后，内部第一个执行的命令。这里是启动 Bash，保证用户可以使用 Shell。

- image制作
  
  - .dockerignore编写：排除不需要拷贝进image的项目文件
    
    ```
    .git
    node_modules
    npm-debug.log
    ```
  
  - Dockerfile编写
    
    ```
    FROM node:8.4：该 image 文件继承官方的 node image，冒号表示标签，这里标签是8.4，即8.4版本的 node。
    COPY . /app：将当前目录下的所有文件（除了.dockerignore排除的路径），都拷贝进入 image 文件的/app目录。
    WORKDIR /app：指定接下来的工作路径为/app。
    RUN npm install：在/app目录下，运行npm install命令安装依赖。注意，安装后所有的依赖，都将打包进入 image 文件。
    EXPOSE 3000：将容器 3000 端口暴露出来， 允许外部连接这个端口。
    ```
  
  - 编译
    
    ```shell
    docker image build -t (name):(tag) (Dockerfile path)
    e.g. docker image build -t koa-demo:0.0.1 .
    ```
