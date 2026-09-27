命令解读

```shell
docker run -d --name mysql -p 3306:3306 -e TZ=Asia/shanghai -e MYSQL_ROOT_PASSWORD=123 mysql
```

* `docker run`: <mark>创建</mark>并运行一个容器，-d是让容器在后台运行
* `--name mysql`：给容器起名字，必须唯一
* `-p 3306:3306`：(<mark>第一个端口是宿主机的端口，第二个是容器内的端口</mark>)设置端口映射，docker内的网络是隔离的，将宿主机上的端口映射到docker内，已通过宿主机间接访问docker内软件
* `-e KEY=VALUE`：是设置环境变量
* `mysql`：指定运行的镜像名字

镜像名字命名规范

* 镜像名称一般分两部分组成：`[repository]:[tag]`
  * 其中`repository`就是镜像名
  * `tag`是镜像的版本
* 在没有指定`tag`时，默认是`latest`，代表最新版本的镜像

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
> docker安装命令中-v参数后面是绑定挂载，由 **用户**自己管理，直接映射主机上的任意路径，**灵活、直接、方便**。非常适合开发环境、实时修改配置文件或代码（热更新）。
> 
> Docker 数据卷（Volume）是 Docker 用来**持久化保存数据**的机制。核心问题是：**容器删了，里面的数据也没了**，而数据卷就是用来解决这个问题的。

#### 数据卷的三种类型

| 类型                        | 说明             | 例子                              |
| ------------------------- | -------------- | ------------------------------- |
| **命名卷（named volume）**     | Docker 管理，有名字  | `-v mydata:/var/lib/mysql`      |
| **匿名卷（anonymous volume）** | Docker 管理，随机名字 | `-v /var/lib/mysql`             |
| **绑定挂载（bind mount）**      | 直接映射宿主机目录      | `-v /host/path:/container/path` |

#### 命名卷（最常用）

**创建和使用**

```shell
# 方式一：运行时自动创建
docker run -d --name mysql \
  -v mysql-data:/var/lib/mysql \
  mysql

# 方式二：手动先创建
docker volume create mysql-data
docker run -d -v mysql-data:/var/lib/mysql mysql
```

**`-v mysql-data:/var/lib/mysql` 含义：**

```textile
-v 卷名:容器内路径
   ↑        ↑
   Docker管理  容器里要持久化的目录
```

> * 卷名不是以 `/` 开头 → Docker 当作**命名卷**，存到 `/var/lib/docker/volumes/` 下
> 
> * 容器内 `/var/lib/mysql` 的数据会写进这个卷

**用 --mount 写法（更推荐）**

```bash
docker run -d --name mysql \
  --mount type=volume,source=mysql-data,target=/var/lib/mysql \
  mysql
```

#### 绑定挂载（bind mount）

> 把**宿主机的目录**直接映射到容器里：

```bash
docker run -d \
  -v /home/user/app/config:/app/config \
  my-go-app
```

**含义：**

```textile
-v /宿主机/绝对路径:/容器/路径
```

> 特点：
> 
> * 宿主机路径**必须写绝对路径**
> 
> * 宿主机和容器**双向同步**，改哪边都生效
> 
> * 常用于：开发时挂源码、挂配置文件、挂日志目录

**用 `--mount` 写法：**

```bash
docker run -d \
  --mount type=bind,source=/home/user/app/config,target=/app/config \
  my-go-app
```

#### 常用命令

| 常见命令                    | 说明         |
| ----------------------- | ---------- |
| `docker volume create`  | 创建数据卷      |
| `docker volume ls`      | 查看所有数据卷    |
| `docker volume rm`      | 删除指定数据卷    |
| `docker volume inspect` | 查看某个数据卷的详情 |
| `docker volume prune`   | 清除数据卷      |

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

> 镜像就是包含了应用程序、程序运行的系统函数库、运行配置等文件的文件包。构建镜像的过程其实就是把上述文件打包的过程。

#### 镜像结构

* 层：添加安装包、依赖、配置等，每次操作都形成新的一层。
* 入口：是层的其中一个，镜像运行入口，一般是层序启动的脚本和参数
* 基础镜像：是层的其中一个，应用依赖的系统函数库、环境、配置、文件等。

#### Dockerfile

> Dockerfile就是一个文本文件，其中包含一个个的指令(Insturction)，用指令来说明要执行什么操作来构建镜像。将来Docker可以根据Dockerfile帮我们构建镜像。常见指令如下：

| Dockerfile 指令 | 说明                                         | 示例                             |
| ------------- | ------------------------------------------ | ------------------------------ |
| FROM          | 指定基础镜像，用于后续的指令构建。                          |                                |
| MAINTAINER    | 指定Dockerfile的作者/维护者。（已弃用，推荐使用LABEL指令）      |                                |
| LABEL         | 添加镜像的元数据，使用键值对的形式。                         |                                |
| RUN           | 在构建过程中在镜像中执行命令。                            |                                |
| CMD           | 指定容器创建时的默认命令。（可以被覆盖）                       |                                |
| ENTRYPOINT    | 设置容器创建时的主要命令。（不可被覆盖）                       |                                |
| EXPOSE        | 声明容器运行时监听的特定网络端口。                          |                                |
| ENV           | 在容器内部设置环境变量。                               |                                |
| ADD           | 将文件、目录或远程URL复制到镜像中。                        |                                |
| COPY          | 将文件或目录复制到镜像中。                              |                                |
| VOLUME        | 为容器创建挂载点或声明卷。                              | `VOLUME /data`  `/data`是容器内的目录 |
| WORKDIR       | 设置后续指令的工作目录。                               |                                |
| USER          | 指定后续指令的用户上下文。                              |                                |
| ARG           | 定义在构建过程中传递给构建器的变量，可使用 "docker build" 命令设置。 |                                |
| ONBUILD       | 当该镜像被用作另一个构建过程的基础时，添加触发器。                  |                                |
| STOPSIGNAL    | 设置发送给容器以退出的系统调用信号。                         |                                |
| HEALTHCHECK   | 定义周期性检查容器健康状态的命令。                          |                                |
| SHELL         | 覆盖Docker中默认的shell，用于RUN、CMD和ENTRYPOINT指令。  |                                |

**示例**

```dockerfile
# ---- 构建阶段 ----
# 基于镜像golang:1.24-alpine，给这个阶段命名builder
FROM golang:1.24-alpine AS builder    

# 指定工作目录，如果目录不存在则创建
WORKDIR /build

# 先复制依赖文件，利用缓存加速后续构建
COPY go.mod go.sum ./
# 执行命令：下载依赖
RUN go mod download

# 复制源码并编译
# 把宿主机当前目录的所有源码复制到 /build/
# 第一个 . 是宿主机当前目录，第二个 . 是容器内的 /build
COPY . .

# CGO_ENABLED=0 是关键，确保生成不依赖外部 C 库的静态二进制
# 执行编译命令，将当前目录编译成可执行文件放入/app
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 \
    go build -ldflags="-s -w" -o /app .

# ---- 运行阶段 ----
# 选择一：最精简（约 2-10 MB），但需手动处理证书等问题
# FROM scratch
# 选择二：推荐生产使用，自带 CA 证书和非 root 用户（约 2-5 MB）
# 这是是一个很精简的镜像
FROM gcr.io/distroless/static-nonroot

# 从builder阶段的/app目录拷贝下的可执行文件到当前的镜像/app，这个/app是可执行文件
# /               ← 根目录
# ├── app         ← 这就是那个可执行文件（不是一个目录！）
# ├── etc/
# ├── ...
COPY --from=builder /app /app

# 表示程序会监听容器内的8080端口
EXPOSE 8080
# 容器启动时执行的命令
ENTRYPOINT ["/app"]
```



#### Dockerfile构建

```shell
docker build -t myImage:1.0 .
```

* -t：是给镜像起名，格式依然是repository:tag的格式，不指定tag时，默认为latest

* .：指定Dockerfile所在目录，如果就在当前目录，则指定为"." 
  
  

### docker网络

> 默认情况下，所有容器都是以bridge方式连接到Docker的一个虚拟网桥上，容器与容器之间是可以通过ip联通的(不推荐，因为ip可能会变)，因为它们在同一个网段内。

> 可以定义网络(即自定义网段)，加入同一个自定义网络的容器可以<mark>通过容器名互相访问</mark>，Docker的网络操作命令：

| 命令                          | 说明           |
| --------------------------- | ------------ |
| `docker network create`     | 创建一个网络       |
| `docker network ls`         | 查看所有网络       |
| `docker network rm`         | 删除指定网络       |
| `docker network prune`      | 清除未使用的网络     |
| `docker network connect`    | 使指定容器加入某网络   |
| `docker network disconnect` | 使指定容器连接离开某网络 |
| `docker network inspect`    | 查看网络详细信息     |

### DockerCompose

> Docker Compose通过一个单独的docker-compose.yml模板文件(YAML格式)来定义一组相关联的应用容器，帮助实现多个相互关联的Docker容器的快速部署。
> 
> 一个`docker-compose.yml`(也可以命名为`compose.yml`)对应一个项目

**一个简单的docker-compose.yml模板**

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

**示例**

> 假设有一个 Go 程序，想连同 MySQL 一起跑起来：

**第一步：创建 `compose.yaml` 文件**

```yaml
services:
  # Go 应用服务
  app:
    build: .              # 使用当前目录的 Dockerfile 构建镜像
    ports:
      - "8080:8080"      # 宿主机端口:容器端口
    depends_on:
      - db                # 等 db 服务启动后再启动
    environment:
      # 通过环境变量告诉应用数据库地址（直接用服务名 db）
      DB_HOST: db
      DB_USER: user
      DB_PASSWORD: password

  # 数据库服务
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: myapp
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    volumes:
      - db_data:/var/lib/mysql   # 数据持久化

volumes:
  db_data:             # 声明命名卷,使用所有默认配置
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
