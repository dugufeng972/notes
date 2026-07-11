命令解读

```shell
docker run -d --name mysql -p 3306:3306 -e TZ=Asia/shanghai -e MYSQL_ROOT_PASSWORD=123 mysql
```

* docker run: ==创建==并运行一个容器，-d是让容器在后台运行
* --name mysql：给容器起名字，必须唯一
* -p 3306:3306：(==第一个端口是宿主机的端口，第二个是容器内的端口==)设置端口映射，docker内的网络是隔离的，将宿主机上的端口映射到docker内，已通过宿主机间接访问docker内软件
* -e KEY=VALUE：是设置环境变量
* mysql：指定运行的镜像名字

镜像名字命名规范

* 镜像名称一般分两部分组成：[repository]:[tag]
  * 其中repository就是镜像名
  * tag是镜像的版本
* 在没有指定tag时，默认是latest，代表最新版本的镜像

### 常见命令

可以用`docker 命令 --help`来查看具体命令用法

1. 从远程镜像仓库下载到本地镜像
   - `docker pull`
2. 查看本地镜像
   - `docker images`
3. 删除本地镜像
   - `docker rmi`
4. 自行定义镜像
   - `docker build`
5. 将自定义镜像进行打包
   - `docker save`
6. 将打包的自定义镜像加载到本地镜像
   - `docker load`
7. 将本地镜像推送到远程镜像仓库
   - `docker push`
8. 创建并运行容器
   - `docker run`
9. 停止运行容器
   - `docker stop`
10. 重新运行容器(不会重复创建容器)
    - `docker start`
11. 查看运行中的容器
    - `docker ps`
12. 查看本地容器
    - `docker container ls` 
13. 删除容器
    - `docker rm`
14. 查看容器日志
    - `docker logs`
15. 在容器内部执行命令
    - `docker exec`

### 命令别名

具体查看命令linux别名

### 数据卷

> 数据卷(volume)是一个虚拟目录，是容器内目录与宿主机目录之间映射的桥梁。也就是docker内的文件在宿主机中(linux在/var/lib/docker/volumes/html/_data下)可以访问到并修改，但是并不推荐这样做，数据卷是由docker引擎管理的，与主机文件系统隔离（宿主机可以访问修改）。
> 
> 优势在于：**安全、可移植、易于备份**。非常适合生产环境、数据库等需要长期保存且重要的数据
> 
> docker安装命令中-v参数后面是绑定挂载，由 **用户**自己管理，直接映射主机上的任意路径，**灵活、直接、方便**。非常适合开发环境、实时修改配置文件或代码（热更新）

| 常见命令                  | 说明         |
| --------------------- | ---------- |
| docker volume create  | 创建数据卷      |
| docker volume ls      | 查看所有数据卷    |
| docker volume rm      | 删除指定数据卷    |
| docker volume inspect | 查看某个数据卷的详情 |
| docker volume prune   | 清除数据卷      |

```shell
sudo docker run -d --name nginx -p 101:80 -v html:/usr/share/nginx/html nginx
```

<mark>注意数据卷挂载必须在容器创建时挂载</mark>
-v:表示挂载数据卷
html:/usr/share/nginx/html ：html指宿主机中被映射的数据卷，`/usr/share/nginx/html`指容器内要映射的文件夹，宿主机中的目录`/var/lib/docker/volumes/html/_data`与数据卷html连接，间接的将容器内`/usr/share/nginx/html`里面的内容映射到`/var/lib/docker/volumes/html/_data`中

#### 数据卷本地目录挂载

```shell
sudo docker run -d --name nginx -p 101:80 -v ./nginx:/usr/share/nginx/html nginx
#例二
docker run -d --name mysql -p 3306:3306 -e TZ=Asia/shanghai -e MYSQL_ROOT_PASSWORD=123 -v /root/mysql/data:/var/lib/mysql -v /root/mysql/init:/docker-entrypoint-initdb.d -v /root/mysql/conf:/etc/mysql/conf.d mysql
```

注意：

1. 使用-v **本地目录:容器目录** 可以完成本地目录挂载
2. 本地目录必须以"/"或"./"开头，如果直接以名称开头，会被识别为数据卷而非本地目录

### 自定义镜像

镜像就是包含了应用程序、程序运行的系统函数库、运行配置等文件的文件包。构建镜像的过程其实就是把上述文件打包的过程。

#### 镜像结构

* 层：添加安装包、依赖、配置等，每次操作都形成新的一层。
* 入口：是层的其中一个，镜像运行入口，一般是层序启动的脚本和参数
* 基础镜像：是层的其中一个，应用依赖的系统函数库、环境、配置、文件等。

#### Dockerfile

Dockerfile就是一个文本文件，其中包含一个个的指令(Insturction)，用指令来说明要执行什么操作来构建镜像。将来Docker可以根据Dockerfile帮我们构建镜像。常见指令如下：
|指令|说明|示例|
|---------|------------------|----------------------|
|FORM|指定基础镜像|FORM centos:6|
|ENV|设置环境变量，可以在后面指令使用|ENV key value|
|COPY|拷贝本地文件到镜像的指定目录|COPY ./jrell.tar.gz /tmp|
|RUN|执行Linux的shell命令，一般是安装过程的命令|RUN tar -zxvf /tmp/jrell.tar.gz|
|EXPOSE|指定容器运行时监听的端口，是给镜像使用者看的|EXPOSE 8080|
|ENTRYOINT|镜像中应用的启动命令，容器运行时调用|ENTRYPOINT java -jar xx.jar|

#### Dockerfile构建

```shell
docker build -t myImage:1.0 .
```

* -t：是给镜像起名，格式依然是repository:tag的格式，不指定tag时，默认为latest

* .：指定Dockerfile所在目录，如果就在当前目录，则指定为"."
  
  ### docker网络
  
  默认情况下，所有容器都是以bridge方式连接到Docker的一个虚拟网桥上，容器与容器之间是可以通过ip联通的(不推荐，因为ip可能会变)，因为它们在同一个网段内。
  可以定义网络(即自定义网段)，加入自定义网络的容器可以==通过容器名互相访问==，Docker的网络操作命令：
  
  | 命令                        | 说明           |
  | ------------------------- | ------------ |
  | docker network create     | 创建一个网络       |
  | docker network ls         | 查看所有网络       |
  | docker network rm         | 删除指定网络       |
  | docker network prune      | 清除未使用的网络     |
  | docker network connect    | 使指定容器加入某网络   |
  | docker network disconnect | 使指定容器连接离开某网络 |
  | docker network inspect    | 查看网络详细信息     |

### DockerCompose

Docker Compose通过一个单独的docker-compose.yml模板文件(YAML格式)来定义一组相关联的应用容器，帮助实现多个相互关联的Docker容器的快速部署。

一个docker-compose.yml对应一个项目
一个简单的docker-compose.yml模板

```yaml
#项目
version: "3.8"
#服务
services: 
   containerA: 
      image: A
      container_name: A
      ports:
         - "11:11"
   containerB:
      image: B
      container_name: B
      ports:
         - "22:22"
   containerC:
      image: C
      container_name: C
      ports:
         - "33:33"
```

#### dockers run命令与dockercompose对比

```bash
docker run -d 
   --name mysql 
   -p 3306:3306 
   -e TZ=Asia/shanghai 
   -e MYSQL_ROOT_PASSWORD=123 
   -v /root/mysql/data:/var/lib/mysql 
   -v /root/mysql/init:/docker-entrypoint-initdb.d 
   -v /root/mysql/conf:/etc/mysql/conf.d 
   --network ddw
   mysql
```

```yaml
version: "3.8"
services: 
   mysql: 
      image: mysql
      container_name: mysql
      ports:
         - "3306:3306"
      environment: 
         TZ: Asia/shanghai
         MYSQL_ROOT_PASSWORD: 123
      volumes:
         - "/root/mysql/data:/var/lib/mysql"
         - "/root/mysql/init:/docker-entrypoint-initdb.d"
         - "/root/mysql/conf:/etc/mysql/conf.d"
      network:
         - ddw
```

#### 使用DockerCompose部署

docker compose的命令格式如下：

```bash
docker compose [options] [command]
```

| 类型       | 参数或指令   | 说明               |
| -------- | ------- | ---------------- |
| options  | -f      | 指定compose文件的路径   |
| options  | -p      | 指定project名称      |
| commands | up      | 创建并启动所有service容器 |
| commands | down    | 停止并移除所有容器、网络     |
| commands | ps      | 列出所有启动的容器        |
| commands | logs    | 查看指定容器的日志        |
| commands | stop    | 停止容器             |
| commands | start   | 启动容器             |
| commands | restart | 重启容器             |
| commands | top     | 查看运行的进程          |
| commands | exec    | 在指定的运行中容器中执行命令   |


