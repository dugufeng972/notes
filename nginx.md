## Nginx常用的功能模块

> 1. 静态资源部署
> 2. Rewrite地址重写
> 3. 反向代理
> 4. 负载均衡
> 5. web缓存
> 6. 环境部署
> 7. 高可用的环境
> 8. 用户认证模块

## Nginx的核心组成

> 1. nginx二进制可执行文件
> 2. nginx.conf配置文件
> 3. error.log错误的日志记录
> 4. access.log访问日志记录

## Nginx常用命令

> 使用nginx操作命令前提条件：必须进入nginx的目录，/usr/local/nginx/sbin

1. 查看nginx的版本号：`./nginx -v`

2. 启动nginx：`./nginx`

3. 关闭nginx：`./nginx -s stop`

4. 重新加载nginx（让新配置生效）：`./nginx -s reload`

## Nginx配置文件

### 1. nginx配置文件地址

> xxx

### 2.nginx配置文件组成

#### 1. 全局块

> 从配置文件开始到events块之间的内容，主要会设置一些影响nginx服务器整体运行的配置指令。
> 主要包括：
> 
> - 配置运行nginx服务器的用户（组）
> 
> - 允许生成的worker process数
> 
> - 进程PID存放路径
> 
> - 日志存放路径和类型
> 
> - 以及配置文件的引入

```nginx
#全局块

# 运行用户
user nginx;

# 工作进程数（通常设为CPU核心数）
# worker_processes 值越大可以支持的并发处理量也越多
worker_processes auto;

# 错误日志
error_log /var/log/nginx/error.log warn;

# 进程ID文件
pid /var/run/nginx.pid;

# 工作进程最大打开文件数
worker_rlimit_nofile 65535;
```

#### 2. events块

> events块涉及的指令主要影响nginx服务器与用户的网络连接，
> 常用的设置包括：
> 
> - 是否开启对多work process下的网络连接进行序列化，
> 
> - 是否允许同时接收多个网络连接，
> 
> - 选取哪种事件驱动模型来处理连接请求，
> 
> - 每个work process可以同时支持的最大连接数等。

```nginx
events {
    # 每个工作进程最大连接数
    worker_connections 1024;

    # 是否同时接受多个连接
    multi_accept on;

    # 事件驱动模型（epoll在Linux上性能最好）
    use epoll;

    # 是否允许一个worker进程同时接受多个新连接
    accept_mutex on;
}
```

#### 3. http块

> 这部分是nginx服务配置中最频繁的部分，代理、缓存和日志定义等绝大多数功能和第三方模块的配置都在这里
> 需要注意的是：http块也可以包括 **http全局块**、**server块**

##### 1. http全局块

> http全局块配置的指令包括文件引入、MIME-TYPE定义、日志自定义、连接超时时间、单链接请求数上限等

```nginx
http {
    # 文件扩展名与类型映射表
    include mime.types;

    # 默认文件类型
    default_type application/octet-stream;

    # 日志格式
    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';

    # 访问日志
    access_log logs/access.log main;

    # 高效文件传输
    sendfile on;

    # 优化数据包发送
    tcp_nopush on;
    tcp_nodelay on;

    # 连接超时时间
    keepalive_timeout 65;

    # 客户端请求体大小限制
    client_max_body_size 10m;

    # 客户端请求头缓冲区大小
    client_header_buffer_size 32k;
    large_client_header_buffers 4 32k;

    # 开放Gzip压缩
    gzip on;
    gzip_min_length 1k;
    gzip_buffers 4 16k;
    gzip_http_version 1.1;
    gzip_comp_level 2;
    gzip_types text/plain text/css text/xml text/javascript 
               application/json application/javascript application/xml+rss 
               application/rss+xml application/atom+xml image/svg+xml;
    gzip_vary on;
    gzip_disable "MSIE [1-6]\.";
}
    
```

##### 2. server块

> 这块和虚拟主机有密切关系，虚拟主机从用户角度看，和一台独立的硬件主机是完全一样的，该技术的产生是为了节省互联网服务器硬件成本。
> 
> 每个http块可以包括多个server块，而每个server块就相当于一个虚拟主机。
> 
> 而每个server块也分为全局server块，以及可以同时包含多个location块。

```nginx
server {
    # 监听端口和IP
    listen 80;
    listen [::]:80;

    # 服务器名称（域名 或 ip地址，如：192.168.17.129）
    server_name example.com www.example.com;

    # 网站根目录
    root /var/www/example.com;

    # 默认首页文件
    index index.html index.htm index.php;

    # 字符集
    charset utf-8;

    # 访问日志
    access_log /var/log/nginx/example.com.access.log main;

    # 错误日志
    error_log /var/log/nginx/example.com.error.log warn;
}
```

##### 3. location块（url匹配）

```nginx
# location匹配
# 精确匹配
location = / {
    # 只匹配根路径
    return 200 "Welcome to root!";
}

# 前缀匹配
location /images/ {
    # 匹配以/images/开头的路径
    root /data;
}

# 正则匹配（区分大小写）
location ~ \.(php|jsp)$ {
    # 匹配以.php或.jsp结尾的路径
    fastcgi_pass 127.0.0.1:9000;
}

# 正则匹配（不区分大小写）
location ~* \.(jpg|jpeg|png|gif|ico|css|js)$ {
    # 匹配图片、CSS、JS文件
    expires 30d;
    add_header Cache-Control "public";
}

# 优先前缀匹配
location ^~ /static/ {
    # 以/static/开头，如果匹配成功，停止搜索
    root /data/static;
}

# 命名location（内部重定向）
location @error {
    internal;
    return 500 "Internal Server Error";
}
```

##### 4. location块内参数详解

```nginx
# xxxx
```

### Nginx配置实例：反向代理

```nginx
http {
    include mime.types;
    ....
    server {
        listen 80;
        server_name 192.168.17.129;
        location / {
            root html;
            proxy_pass http://127.0.0.1:8080;
            index index.html index.htm;
        }
    }
}
```

### Nginx配置实例：负载均衡

> 将请求平均分摊到被代理的服务器上

```nginx
http{
    ...
    upstream myserver {
        # 负载均衡算法
        # 默认：轮询（round-robin）
        # least_conn：最少连接
        # ip_hash：IP哈希（保持会话）
        ip_hash;
        
        # 被代理的服务器
        server 192.168.1.10:8080 weight=3;  # weight权重
        server 192.168.1.11:8081 weight=2;
        server 192.168.1.12:8082 backup;    # 备份服务器
    
        keepalive 32; # 保持连接数
    }
    
    server {
        listen 80;
        server_name 192.168.1.10;
    
        location / {
           # 这里把 ip 名改成 upstream 名
           proxy_pass http://myserver;
        }
    }
}
```
