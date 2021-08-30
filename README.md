# docker

## Docker 是什么

`docker` 是一个基于 `Go` 语言实现的开源容器项目，开源理解为一个轻量级的沙盒，每个容器内运行着一个应用，不同容器彼此隔离，也可以彼此通信。

## 为什么要用 Docker

1. Docker 容器虚拟化，可以通过容器打包应用，迁移方便
2. Docker 在开发和运维中的优势
   * 更快速的交付和部署
   * 更高效的资源利用
   * 更轻松的迁移扩展
   * 更简单的更新部署

Docker 相对于虚拟机来说有以下几点优势

1. 容器很快，基本开启关闭都是秒级
2. 容器对于系统资源要求需求很少
3. 通过类似于Git的设计理念来方便用户操作镜像
4. 通过 Dockerfile 来支持灵活的自动化创建和部署机制

## Docker 工作原理

1. Client-Server 结构

2. docker-Server 以守护进程的方式运行在主机上

3. 通过 Socket 通信

4. DockerServer接受DockerClient指令执行命令

   <img src="/Users/mac/xy/docker-demo/imgs/image-20210828102706216.png" alt="image-20210828102706216" style="zoom:50%;" />

## Docker 的核心概念

![image-20210828101558439](/Users/mac/xy/docker-demo/imgs/image-20210828101558439.png)

### 镜像 (Image)

镜像类似于虚拟机的镜像，一个只读的模板。是创建一个容器的基础，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数

### 容器 (container)

容器类似于一个沙箱，利用容器运行和隔离应用，容器从镜像创建的应用运行实例。容器间互相隔离，互不可见。

### 仓库 (Repository)

类似于代码仓库，用于存放镜像文件的场所

## Docker 命令

```sh
docker version # 显示 docker 版本信息
docker info # 显示 docker 系统信息，包括镜像，容器命令
docker xxx --help # 帮助命令
```

文档地址：https://docs.docker.com/reference/

### 镜像命令

#### pull (拉取镜像)

通过此命令拉取镜像到本地

```sh
docker pull node
```

![image-20210828105745931](/Users/mac/xy/docker-demo/imgs/image-20210828105745931.png)

| 命令选项       | 意义                           |
| -------------- | ------------------------------ |
| -a, --all-tags | 是否获取仓库中所有镜像，默认否 |
| -q, --quiet    | 放弃详细输出                   |

#### images（查看镜像）

查看本机已有的镜像信息

```sh
docker images | docker image ls
```

![image-20210828104059305](/Users/mac/xy/docker-demo/imgs/image-20210828104059305.png)

字段含义

| 字段         | 意义         |
| ------------ | ------------ |
| `repository` | 来自哪个仓库 |
| `tag`        | 镜像标签内容 |
| `image_id`   | 镜像ID       |
| `created`    | 创建时间     |
| `size`       | 镜像大小     |

`image` 子命令支持的选项

| 命令选项            | 意义                           |
| ------------------- | ------------------------------ |
| -a, --all=true      | 列出所有镜像，默认为 true      |
| --disests=true      | 列出镜像数字摘要值，默认为true |
| -f, --filter=[]     | 过滤列出的镜像                 |
| --format="TEMPLATE" | 控制输出格式                   |
| -q, --quiet=true    | 仅输出ID信息，默认为 false     |

#### search（搜索镜像）

搜索`docker`镜像

```sh
docker search mysql
```

![image-20210828104545754](/Users/mac/xy/docker-demo/imgs/image-20210828104545754.png)

`search` 子命令支持的选项

| 命令选项        | 意义                         |
| --------------- | ---------------------------- |
| -f, --filter=[] | 过滤搜索内容                 |
| --format string | 控制输出格式                 |
| --limit-int     | 限制输出结果个数，默认 25 个 |
| --no-trunc      | 不截断输出结果               |

#### rmi | image rm （删除镜像）

可以通过使用标签或者镜像id的方式来删除镜像

通过标签删除的操作会删除指定标签的镜像，而不会影响到同一个镜像id的其他镜像

通过镜像id删除的操作，会先尝试删除指向该镜像id的标签，然后删除镜像本身

默认是无法删除被容器依赖的镜像，如需删除，需要加上 -f | -force 选项强制删除

```
docker rmi -f $(docker images -f "dangling=true" -q)
```

![image-20210828110701695](/Users/mac/xy/docker-demo/imgs/image-20210828110701695.png)

### 

### 容器命令

#### run (创建容器)

新建容器并启动

```sh
docker run [可选参数] image
```

![image-20210828114439141](/Users/mac/xy/docker-demo/imgs/image-20210828114439141.png)

参数说明

| 命令选项      | 意义                                                         |
| ------------- | ------------------------------------------------------------ |
| --name='Name' | 容器名字                                                     |
| -d            | 后台方式运行                                                 |
| -it           | 交互式方式运行                                               |
| -p            | 指定容器端口 <br />-p ip:主机端口 : 容器端口<br />-p 主机端口 : 容器端口<br />-p 容器端口 |
| -P            | 随机指定端口                                                 |

#### ps（查看主机容器）

列出所有在运行的容器

```sh
docker ps -a
```

![image-20210828114409685](/Users/mac/xy/docker-demo/imgs/image-20210828114409685.png)

| 命令选项 | 意义                                    |
| -------- | --------------------------------------- |
| -a       | 列出当前正在运行的容器+历史运行过的容器 |
| -q       |                                         |
| -n=?     | 显示最近创建的容器                      |

#### rm（删除容器）

删除容器 -f 强制删除

```sh
docker rm -f test
docker rm -f $(docker ps --filter ancestor=centos -a -q)
docker ps --filter ancestor=centos -a -q | xargs docker rm -f
```

![image-20210828114220016](/Users/mac/xy/docker-demo/imgs/image-20210828114220016.png)

#### 启动停止容器操作

```sh
docker start # 启动
docker restart # 重启
docker stop # 停止当前容器
docker kill # 强制停止当前容器
```

#### logs（查看容器日志）

```sh
docker logs -f -t --tail 10 [容器id]
```

#### top（查看容器中进程信息）

```sh
docker top [容器id]
```

#### inspect（获取镜像/容器详细信息）

获取镜像的详细信息。可以通过 `-f {{".Id"}}` 的选项来获取到某个内容

```sh
docker inspect [容器id/镜像id]
```

#### exec （进入容器）

进入当前正在运行的容器，新的终端

```sh
docker exec -it [容器id] /bin/bash
```

#### attach （直接进入容器）

进入正在执行当前的终端

```sh
docker attach [容器id] 
```

#### cp（复制文件到主机）

```sh
docker cp [容器id:容器内路径] 目的主机路径
```

### 常用命令总结

![img](/Users/mac/xy/docker-demo/imgs/Docker-Command-Diagram.png)

```sh
attach # 当前 shell 下连接指定运行镜像
build # 通过 Dockerfile 定制镜像
commit # 提交当前容器为新的镜像
cp # 拷贝文件
create # 创建一个新的容器，但是不启动
diff # 查看容器变化
events # 从docker服务中获取容器实时事件
exec # 从运行中容器运行命令
export # 导出容器的内容流作为tar文件
history # 展示一个镜像的历史
images # 列出系统当前镜像
import # 从tar包中创建一个镜像
info # 系统相关信息
inspect # 容器相关信息
kill # 杀死指定容器
load 	# 从tar包中加载一个镜像
login # 登录注册
logout # 退出当前登录
logs # 输出当前容器日志信息
port # 查看容器映射端口
pause # 暂停容器
ps # 列出容器
pull # 从镜像服务器中拉取指定镜像
push # 推送镜像到指定服务器
restart # 重启运行的容器
rm # 移除容器
rmi # 移除镜像
run # 创建一个新的容器并运行命令
save # 保存镜像为一个tar包
search # 搜索镜像
start # 启动容器
stop # 停止容器
tag # 给镜像打标签
top # 查看容器内进程信息
unpause # 取消暂停容器
version # 查看docker版本号
wait # 截取容器停止时的退出状态值
```

### 练习：nginx服务

```sh
# 1. 搜索镜像 search
# 2. 下载镜像 pull
# 3. 创建运行 run
# 4. 测试
➜  docker-demo git:(main) ✗ docker search nginx # 1
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
nginx                             Official build of Nginx.                        15374     [OK]
jwilder/nginx-proxy               Automated Nginx reverse proxy for docker con…   2060                 [OK]
richarvey/nginx-php-fpm           Container running Nginx + PHP-FPM capable of…   816                  [OK]
jc21/nginx-proxy-manager          Docker container for managing Nginx proxy ho…   236
linuxserver/nginx                 An Nginx container, brought to you by LinuxS…   152
tiangolo/nginx-rtmp               Docker image with Nginx using the nginx-rtmp…   140                  [OK]
jlesage/nginx-proxy-manager       Docker container for Nginx Proxy Manager        132                  [OK]
alfg/nginx-rtmp                   NGINX, nginx-rtmp-module and FFmpeg from sou…   105                  [OK]
jasonrivers/nginx-rtmp            Docker images to host RTMP streams using NGI…   92                   [OK]
nginxdemos/hello                  NGINX webserver that serves a simple page co…   71                   [OK]
privatebin/nginx-fpm-alpine       PrivateBin running on an Nginx, php-fpm & Al…   56                   [OK]
nginx/nginx-ingress               NGINX and  NGINX Plus Ingress Controllers fo…   55
nginxinc/nginx-unprivileged       Unprivileged NGINX Dockerfiles                  46
staticfloat/nginx-certbot         Opinionated setup for automatic TLS certs lo…   24                   [OK]
nginx/nginx-prometheus-exporter   NGINX Prometheus Exporter for NGINX and NGIN…   19
schmunk42/nginx-redirect          A very simple container to redirect HTTP tra…   19                   [OK]
nginxproxy/nginx-proxy            Automated Nginx reverse proxy for docker con…   17
centos/nginx-112-centos7          Platform for running nginx 1.12 or building …   15
centos/nginx-18-centos7           Platform for running nginx 1.8 or building n…   13
raulr/nginx-wordpress             Nginx front-end for the official wordpress:f…   13                   [OK]
flashspys/nginx-static            Super Lightweight Nginx Image                   10                   [OK]
mailu/nginx                       Mailu nginx frontend                            9                    [OK]
sophos/nginx-vts-exporter         Simple server that scrapes Nginx vts stats a…   7                    [OK]
ansibleplaybookbundle/nginx-apb   An APB to deploy NGINX                          2                    [OK]
wodby/nginx                       Generic nginx                                   1                    [OK]
➜  docker-demo git:(main) ✗ docker pull nginx # 2
Using default tag: latest
latest: Pulling from library/nginx
Digest: sha256:4d4d96ac750af48c6a551d757c1cbfc071692309b491b70b2b8976e102dd3fef
Status: Image is up to date for nginx:latest
docker.io/library/nginx:latest
➜  docker-demo git:(main) ✗ docker run -it -d --name nginx01 -p 3322:80 nginx # 3
004e81383a3138e7dfe559a01a3b31461ea072681eb564134f3439e781deff3d
➜  docker-demo git:(main) ✗ curl localhost:3322 # 4
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

##容器数据卷

将容器内的目录挂载到系统目录，目的是为了支持持久化和同步操作

### 特性

* `数据卷` 可以在容器之间共享和重用
* 对 `数据卷` 的修改会立马生效
* 对 `数据卷` 的更新，不会影响镜像
* `数据卷` 默认会一直存在，即使容器被删除

### 使用

```sh
docker run -v [主机目录]:[容器目录]
```

```sh
➜  ~ mkdir html
➜  ~ cd html
➜  html cat > index.html << EOF
heredoc> hello docker
heredoc> EOF
➜  html cat index.html
hello docker
➜  docker-demo git:(main) ✗ docker run -it -d -p 3002:80 -v /Users/mac/html:/usr/share/nginx/html/ nginx
a75a4b9b392f9fe8bd60619dd119376407731677bcd5d39366fd2cc5b598f9c4
➜  docker-demo git:(main) ✗ docker exec -it a75a4b9b392f9fe8bd60619dd119376407731677bcd5d39366fd2cc5b598f9c4 /bin/bash
root@a75a4b9b392f:/# cd usr/share/nginx/html/
root@a75a4b9b392f:/usr/share/nginx/html# ls
index.html
root@a75a4b9b392f:/usr/share/nginx/html# cat index.html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
  <meta http-equiv="Content-Style-Type" content="text/css">
  <title></title>
  <meta name="Generator" content="Cocoa HTML Writer">
  <meta name="CocoaVersion" content="1894.2">
  <style type="text/css">
    p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 14.0px; font: 12.0px Times; color: #000000; -webkit-text-stroke: #000000}
    span.s1 {font-kerning: none}
  </style>
</head>
<body>
<p class="p1"><span class="s1">hello docker 1</span></p>
</body>
</html>
```

![image-20210828153421780](/Users/mac/xy/docker-demo/imgs/image-20210828153421780.png)

```sh
docker inspect
```

![image-20210828153539432](/Users/mac/xy/docker-demo/imgs/image-20210828153539432.png)

```sh
-v [容器内路径] # 匿名挂载
-v [卷名]:[容器内路径] # 具名挂载
-v [主机路径]:[容器内路径] # 指定路径挂载
-v [容器内路径]:ro/rw # 改变读写权限 ro readonly / rw readwrite
```

#### Dockerfile 创建

`Dockerfile` 用于构建镜像的构建文件，通过脚本命令一层一层的构建，最后输出镜像文件

```dockerfile
VOLUME ['volume01', 'volume02']
```

![image-20210828160501572](/Users/mac/xy/docker-demo/imgs/image-20210828160501572.png)

```sh
docker build -f ./dockerfile -t xiayucentos:1.0 .
```

![image-20210828160533792](/Users/mac/xy/docker-demo/imgs/image-20210828160533792.png)

![image-20210828160625886](/Users/mac/xy/docker-demo/imgs/image-20210828160625886.png)

## Docker 镜像构建

### commit 命令

```sh
docker commit # 提交容器成为一个新的镜像
docker commit -m='描述信息' -a='作者' 容器id 目标镜像名:[tag]
```

操作

```sh
➜  docker-demo git:(main) ✗ docker run -it -d -p 8909:8080 tomcat:9.0
a83ed3be7034192b05a4703a2d7872b6a5026852833f88cbdbeef3574edb178a
➜  docker-demo git:(main) ✗ docker exec -it a83ed3be7034192b05a4703a2d7872b6a5026852833f88cbdbeef3574edb178a /bin/bash
root@a83ed3be7034:/usr/local/tomcat# cp -r webapps.dist/* webapps
root@a83ed3be7034:/usr/local/tomcat# ls webapps
ROOT  docs  examples  host-manager  manager
root@a83ed3be7034:/usr/local/tomcat#
➜  docker-demo git:(main) ✗ docker commit -m='test-tomcat' -a='xiayuchen' a83ed3be7034192b05a4703a2d7872b6a5026852833f88cbdbeef3574edb178a tomcatxy:1.0
sha256:e5302947026599b77a2c4cf25ff93f5b0c13a9e2dcc12bc70745a2339faf3961
➜  docker-demo git:(main) ✗ docker images
REPOSITORY                        TAG                      IMAGE ID       CREATED         SIZE
tomcatxy                          1.0                      e53029470265   4 seconds ago   673MB
tomcat                            9.0                      266d1269bb29   9 days ago      668MB                                                                                                                                                                ➜  docker-demo git:(main) ✗ docker run -it -d -p 8910:8080 tomcatxy:1.0
d0cf049b6c1a986693ab0b3ff11585c4a18c6f2eada6242b6424af4b2306676
```

###Dockerfile

用来构建docker镜像的文件，命令参数脚本

#### 构建步骤

1. 编写dockerfile文件
2. docker build 构建镜像
3. docker run 运行镜像
4. docker push 发布镜像

#### 构建过程

1. 每个镜像关键字（指令）都是大写
2. 执行从上往下执行
3. \# 表示注释
4. 每条指令都会创建一个新的镜像层

#### 指令

```dockerfile
FROM 				# 基础镜像，一切从这里开始构建
MAINTAINER 	# 镜像是谁写的，姓名+邮箱
RUN 				# 镜像构建是需要运行的命令
ADD					# 添加东西，COPY 文件，会自动解压
WORKDIR			# 设置工作目录 =cd
VOLUME			# 设置卷，挂载目录
EXPOSE			# 指定对外端口
CMD					# 启动时候运行的命令，只有最后一个生效，可被替代
ENTRYPOINT	# 启动时候运行的命令，可以追加命令
ONBUILD			# 当构建一个被继承的时候运行
COPY				# 文件复制到镜像中
ENV					# 设置环境变量
```

```dockerfile
FROM centos

ENV MYPATH /usr/local

WORKDIR ${MYPATH}

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo ${MYPATH}
CMD echo '--end--'
CMD /bin/bash
```

![image-20210828202035487](/Users/mac/xy/docker-demo/imgs/image-20210828202035487.png)

##docker 网络

Docker 启动时，会自动在主机上创建一个 `docker0` 虚拟网桥，实际上是 Linux 的一个 bridge，会在挂载到它的网口之间进行转发。

当创建一个 Docker 容器的时候，同时会创建了一对 `veth pair` 接口

![image-20210828204848976](/Users/mac/xy/docker-demo/imgs/image-20210828204848976.png)

#### 例子解释

##### 创建一个容器

```sh
docker run -d -P --name tom1 tomcat:9.0
e9dd13ce24061846e7674580937433baabce61abd50f1fbbd717019ce5e03c17
```

##### 查看容器信息

```sh
docker inspect tom1
```

![image-20210829094655936](/Users/mac/xy/docker-demo/imgs/image-20210829094655936.png)

这个就是容器被分配的IP地址

主机是可以ping通的

![image-20210829094756047](/Users/mac/xy/docker-demo/imgs/image-20210829094756047.png)

容器内也是可以ping通

![image-20210829094904034](/Users/mac/xy/docker-demo/imgs/image-20210829094904034.png)

如果不进行配置，无法直接ping容器名

![image-20210829095435017](/Users/mac/xy/docker-demo/imgs/image-20210829095435017.png)

#### --link （连接两个容器）

再创建一个tom3容器，通过--link连接tom1

```sh
docker run -d -P --name tom3 --link tom1 tomcat:9.0
```

连接后这里会多一个Links绑定

![image-20210829095857525](/Users/mac/xy/docker-demo/imgs/image-20210829095857525.png)

tom3 容器内部的 hosts

![image-20210829100120176](/Users/mac/xy/docker-demo/imgs/image-20210829100120176.png)

此时tom3就可以ping通tom1了

![image-20210829100229101](/Users/mac/xy/docker-demo/imgs/image-20210829100229101.png)

#### 自定义网络

```sh
docker network create -d bridge my-net
```

##### 网络模式

* bridge：桥接模式
* none：不配置网络
* host：与宿主机共享网络
* container：容器网络

创建三个容器

![image-20210829105203128](/Users/mac/xy/docker-demo/imgs/image-20210829105203128.png)

查看容器信息

```sh
docker inspect tom1
```

![image-20210829105322102](/Users/mac/xy/docker-demo/imgs/image-20210829105322102.png)

查看连通情况

![image-20210829105351321](/Users/mac/xy/docker-demo/imgs/image-20210829105351321.png)

查看自定义网络配置

![image-20210829105537371](/Users/mac/xy/docker-demo/imgs/image-20210829105537371.png)

#### 网络连通

```sh
docker network connect [容器] [网络]
```

![image-20210829185940131](/Users/mac/xy/docker-demo/imgs/image-20210829185940131.png)

my-net 信息

![image-20210829190000691](/Users/mac/xy/docker-demo/imgs/image-20210829190000691.png)

## docker compose

`compose` 是通过 `yaml ` 配置文件定义运行多个容器，现对 `Docker` 容器集群的快速编排

###概念

服务Dockerfileapk add --no-cache gcc musl-dev linux-headers (`service`)：一个应用的容器，实际上可以包括若干运行相同镜像的容器实例。

项目 (`project`)：由一组关联的应用容器组成的一个完整业务单元，在 `docker-compose.yml` 文件中定义

### 步骤

1. dockerfile 保证项目可以在任何地方运行
2. 在 docker-compose.yaml 中定义组成您的应用程序的服务，以便它们可以在隔离的环境中一起运行
3. 运行docker compose up, docker compose命令会启动并运行整个应用程序。你也可以使用docker-compose二进制文件运行docker-compose up

#### yaml规则

```yaml
# 3层
version: '' # 版本
services: # 服务
	服务1: web
		# 服务配置
		images:
		build:
		networks:
# 其他配置
volumes:
networks:
configs:
```



