---
title: nginx配置详情
date: 2018-02-27 11:39:56
categories: "nginx" #文章分类目录
tags: nginx #文章标签 可以省略
---

![webpack](/images/nginx.jpg)
<!-- more -->

### Nginx简介
   Nginx是一款开源代码的高性能HTTP服务器和反向代理服务器，同时支持IMAP/POP3/SMTP代理服务
   #### Nginx工作原理
       Nginx由内核和模块组成，完成工作是通过查找配置文件将客户端请求映射到一个location block(location是用于URL匹配的命令)，location配置的命令会启动不同模块完成工作。
       Nginx模块分为核心模块，基础模块和第三方模块。
           核心模块：HTTP模块、EVENT模块(事件)、MAIL模块。
           基础模块：HTTP Access模块、HTTP FastCGI模块、HTTP Proxy模块、HTTP Rewrite模块。
           第三方模块：HTTP Upstream Request Hash模块、Notice模块、HTTP Access Key模块。
   
  #### 性能优势
       web服务器，处理静态文件、索引文件以及自动索引效率高。
       代理服务器，快速高效反向代理，提升网站性能。
       负载均衡器，内部支持Rails和PHP,也可支持HTTP代理服务器，对外进行服务。同时支持简单容错和利用算法进行负载均衡。
       性能方面，Nginx专门为性能设计，实现注重效率。采用Poll模型，可以支持更多的并发连接，并在大并发时占用很低内存。
       稳定性方面，采用分阶段资源分配技术，使CPU资源占用率低。
       高可用性方面，支持热备，启动迅速。
###  Nginx编译安装,配置文件详解
   #### Nginx安装
 安装部署nginx所用到的安装工具和相关库
默认安装的http_rewrite_module（使用正则对请求重写）需pcre库
默认安装的httP_gzip_module（Gzip压缩）需zlib库
安装http_ssl_module(HTTPS/SLL)需openssl库
```
yum -y install gcc pcre pcre-devel zlib zlib-devel openssl openssl-devel
```
下载nginx源码包，并解压
```
wget http://nginx.org/download/nginx-1.10.3.tar.gz
tar -zxvf nginx-1.10.3.tar.gz
cd nginx-1.10.3
```
设置参数 参数具体参考《Nginx编译参数》
```
./configure \
--prefix=/usr/local/nginx \
--with-http_ssl_module 
```
编译并安装 
```
make && make install
#### Nginx配置文件（/usr/local/nginx/conf/nginx.conf）
```
   
   配置文件主要由四部分组成：main(全区设置)，server(主机配置)，upstream(负载均衡服务器设置)，和location(URL匹配特定位置设置)。
 ##### 全局变量
#Nginx的worker进程运行用户以及用户组
#user  nobody nobody;
#Nginx开启的进程数
worker_processes  1;
#worker_processes auto;
#以下参数指定了哪个cpu分配给哪个进程，一般来说不用特殊指定。如果一定要设的话，用0和1指定分配方式.
#这样设就是给1-4个进程分配单独的核来运行，出现第5个进程是就是随机分配了。
eg:
#worker_processes 4     #4核CPU 
#worker_cpu_affinity 0001 0010 0100 1000

#定义全局错误日志定义类型，[debug|info|notice|warn|crit]
#error_log  logs/error.log  info;
#指定进程ID存储文件位置
#pid        logs/nginx.pid;
#一个nginx进程打开的最多文件描述符数目，理论值应该是最多打开文件数（ulimit -n）与nginx进程数相除，但是nginx分配请求并不是那么均匀，所以最好与ulimit -n的值保持一致。
#vim /etc/security/limits.conf
*                soft    nproc          65535
*                hard    nproc          65535
*                soft    nofile         65535
*                hard    nofile         65535
worker_rlimit_nofile 65535;
##### 事件配置
```
events {
    #use [ kqueue | rtsig | epoll | /dev/poll | select | poll ]; epoll模型是Linux 2.6以上版本内核中的高性能网络I/O模型，如果跑在FreeBSD上面，就用kqueue模型。
    use epoll;
    #每个进程可以处理的最大连接数，理论上每台nginx服务器的最大连接数为worker_processes*worker_connections。理论值：worker_rlimit_nofile/worker_processes
    #注意：最大客户数也由系统的可用socket连接数限制（~ 64K），所以设置不切实际的高没什么好处
    worker_connections  65535;    
    #worker工作方式：串行（一定程度降低负载，但服务器吞吐量大时，关闭使用并行方式）
    #multi_accept on; 
}
```
##### http参数
文件扩展名与文件类型映射表
```
#include mime.types;
```
默认文件类型
```
#default_type application/octet-stream;
```

##### 日志相关定义
```
#log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
#                  '$status $body_bytes_sent "$http_referer" '
#                  '"$http_user_agent" "$http_x_forwarded_for"';
#定义日志的格式。后面定义要输出的内容。
#1.$remote_addr 与$http_x_forwarded_for 用以记录客户端的ip地址；
#2.$remote_user ：用来记录客户端用户名称；
#3.$time_local ：用来记录访问时间与时区；
#4.$request  ：用来记录请求的url与http协议；
#5.$status ：用来记录请求状态； 
#6.$body_bytes_sent ：记录发送给客户端文件主体内容大小；
#7.$http_referer ：用来记录从那个页面链接访问过来的；
#8.$http_user_agent ：记录客户端浏览器的相关信息
#连接日志的路径，指定的日志格式放在最后。
#access_log  logs/access.log  main;
#只记录更为严重的错误日志，减少IO压力
error_log logs/error.log crit;
#关闭日志
#access_log  off;

#默认编码
#charset utf-8;
#服务器名字的hash表大小
server_names_hash_bucket_size 128;
#客户端请求单个文件的最大字节数
client_max_body_size 8m;
#指定来自客户端请求头的hearerbuffer大小
client_header_buffer_size 32k;
#指定客户端请求中较大的消息头的缓存最大数量和大小。
large_client_header_buffers 4 64k;
#开启高效传输模式。
sendfile        on;
#防止网络阻塞
tcp_nopush on;
tcp_nodelay on;

#客户端连接超时时间，单位是秒
keepalive_timeout 60;
#客户端请求头读取超时时间
client_header_timeout 10;
#设置客户端请求主体读取超时时间
client_body_timeout 10;
#响应客户端超时时间
send_timeout 10;
```
##### FastCGI相关参数
```
    fastcgi_connect_timeout 300;
    fastcgi_send_timeout 300;
    fastcgi_read_timeout 300;
    fastcgi_buffer_size 64k;
    fastcgi_buffers 4 64k;
    fastcgi_busy_buffers_size 128k;
    fastcgi_temp_file_write_size 128k;
```
##### gzip模块设置
```
    #开启gzip压缩输出
    gzip on; 
    #最小压缩文件大小
    gzip_min_length 1k; 
    #压缩缓冲区
    gzip_buffers 4 16k;
    #压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
    gzip_http_version 1.0;
    #压缩等级 1-9 等级越高，压缩效果越好，节约宽带，但CPU消耗大
    gzip_comp_level 2;
    #压缩类型，默认就已经包含text/html，所以下面就不用再写了，写上去也不会有问题，但是会有一个warn。
    gzip_types text/plain application/x-javascript text/css application/xml;
    #前端缓存服务器缓存经过压缩的页面
    
gzip_vary on;
```
### 虚拟主机基本设置
#### 虚拟主机定义
```
    server {
        #监听端口
        listen       80;
        #访问域名
        server_name  localhost;
        #编码格式，若网页格式与此不同，将被自动转码
        #charset koi8-r;
        #虚拟主机访问日志定义
        #access_log  logs/host.access.log  main;
        #对URL进行匹配
        location / {
            #访问路径，可相对也可绝对路径
            root   html;
            #首页文件。以下按顺序匹配
            index  index.html index.htm;
        }
```
#### 错误信息返回页面
```
        #error_page  404              /404.html;
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
```
#### 访问URL以
```
        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}
```
#### php脚本请求全部转发给FastCGI处理
```
        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}
```
#### 禁止访问
```
        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }
```
#### HTTPS虚拟主机定义
```
    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;
    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;
    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;
    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;
    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
```
#### Nignx状态监控
```
#Nginx运行状态，StubStatus模块获取Nginx自启动的工作状态（编译时要开启对应功能）
        #location /NginxStatus {
        #    #启用StubStatus的工作访问状态    
        #    stub_status    on;
        #    #指定StubStaus模块的访问日志文件
        #    access_log    logs/Nginxstatus.log;
        #    #Nginx认证机制（需Apache的htpasswd命令生成）
        #    #auth_basic    "NginxStatus";
        #    #用来认证的密码文件
        #    #auth_basic_user_file    ../htpasswd;    
        #}
访问：http://IP/NginxStatus(测试就不加密码验证相关) 
```
#### 反向代理

#以下配置追加在HTTP的全局变量中
#nginx跟后端服务器连接超时时间(代理连接超时)
```
proxy_connect_timeout      5;
```
#后端服务器数据回传时间(代理发送超时)
```
proxy_send_timeout         5;
```
#连接成功后，后端服务器响应时间(代理接收超时)
```
proxy_read_timeout         60;
```
#设置代理服务器（nginx）保存用户头信息的缓冲区大小
```
proxy_buffer_size          16k;
#proxy_buffers缓冲区，网页平均在32k以下的话，这样设置
proxy_buffers              4 32k;
```
#高负荷下缓冲大小（proxy_buffers*2）
```
proxy_busy_buffers_size    64k;
```
#设定缓存文件夹大小，大于这个值，将从upstream服务器传
```
proxy_temp_file_write_size 64k;
```
#反向代理缓存目录
```
proxy_cache_path /data/proxy/cache levels=1:2 keys_zone=cache_one:500m inactive=1d max_size=1g;
#levels=1:2 设置目录深度，第一层目录是1个字符，第2层是2个字符
#keys_zone:设置web缓存名称和内存缓存空间大小
#inactive:自动清除缓存文件时间。
#max_size:硬盘空间最大可使用值。
#指定临时缓存文件的存储路径(路径需和上面路径在同一分区)
proxy_temp_path 
/data/proxy/temp
```
#### 服务配置
```
server {
    #侦听的80端口
    listen       80;
    server_name  localhost;
    location / {
        #反向代理缓存设置命令(proxy_cache zone|off,默认关闭所以要设置)
        proxy_cache cache_one;
        #对不同的状态码缓存不同时间
        proxy_cache_valid 200 304 12h;
        #设置以什么样参数获取缓存文件名
        proxy_cache_key $host$uri$is_args$args;
        #后7端的Web服务器可以通过X-Forwarded-For获取用户真实IP
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr; 
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
  
        #代理设置
        proxy_pass   http://IP; 
        #文件过期时间控制
        expires    1d;
    }
    #配置手动清楚缓存(实现此功能需第三方模块 ngx_cache_purge)
    #http://www.123.com/2017/0316/17.html访问
    #http://www.123.com/purge/2017/0316/17.html清楚URL缓存
    location ~ /purge(/.*) {
        allow    127.0.0.1;
        deny    all;
        proxy_cache_purge    cache_one    $host$1$is_args$args;
    }
    #设置扩展名以.jsp、.php、.jspx结尾的动态应用程序不做缓存
    location ~.*\.(jsp|php|jspx)?$ { 
        proxy_set_header Host $host; 
        proxy_set_header X-Real-IP $remote_addr; 
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
  
        proxy_pass http://http://IP;
    }
```
#### 负载均衡
##### 负载均衡服务器池
```
upstream my_server_pool {
    #调度算法
    #1.轮循（默认）（weight轮循权值）
    #2.ip_hash：根据每个请求访问IP的hash结果分配。（会话保持）
    #3.fair:根据后端服务器响应时间最短请求。（upstream_fair模块）
    #4.url_hash:根据访问的url的hash结果分配。（需hash软件包）
    #参数：
    #down：表示不参与负载均衡
    #backup:备份服务器
    #max_fails:允许最大请求错误次数
    #fail_timeout:请求失败后暂停服务时间。
    server 192.168.1.109:80 weight=1 max_fails=2 fail_timeout=30;
    server 192.168.1.108:80 weight=2 max_fails=2 fail_timeout=30;
}
```
##### 负载均衡调用
```
server {
    ...
    location / {
    proxy_pass http://my_server_pool;
    }
}
```
#### URL重写
#根据不同的浏览器URL重写
```
if($http_user_agent ~ Firefox){
rewrite ^(.*)$  /firefox/$1 break; 
}
if($http_user_agent ~ MSIE){
rewrite ^(.*)$  /msie/$1 break; 
}
```
#### 实现域名跳转
location / {
    rewrite ^/(.*)$ https://web8.example.com$1 permanent;
}
#### IP限制
 #限制IP访问
```
location / {
    deny 192.168.0.2；
    allow 192.168.0.0/24;
    allow 192.168.1.1;
    deny all;
}
```
### Nginx相关命令
#启动nginx
nginx 
#关闭nginx
nginx -s stop
#平滑重启
kill -HUP `cat /usr/local/nginx/logs/nginx.pid`
### nginx.conf中文详解
```js
#定义Nginx运行的用户和用户组
user www www;

#nginx进程数，建议设置为等于CPU总核心数。
worker_processes 8;
 
#全局错误日志定义类型，[ debug | info | notice | warn | error | crit ]
error_log /usr/local/nginx/logs/error.log info;

#进程pid文件
pid /usr/local/nginx/logs/nginx.pid;

#指定进程可以打开的最大描述符：数目
#工作模式与连接数上限
#这个指令是指当一个nginx进程打开的最多文件描述符数目，理论值应该是最多打开文件数（ulimit -n）与nginx进程数相除，但是nginx分配请求并不是那么均匀，所以最好与ulimit -n 的值保持一致。
#现在在linux 2.6内核下开启文件打开数为65535，worker_rlimit_nofile就相应应该填写65535。
#这是因为nginx调度时分配请求到进程并不是那么的均衡，所以假如填写10240，总并发量达到3-4万时就有进程可能超过10240了，这时会返回502错误。
worker_rlimit_nofile 65535;


events
{
    #参考事件模型，use [ kqueue | rtsig | epoll | /dev/poll | select | poll ]; epoll模型
    #是Linux 2.6以上版本内核中的高性能网络I/O模型，linux建议epoll，如果跑在FreeBSD上面，就用kqueue模型。
    #补充说明：
    #与apache相类，nginx针对不同的操作系统，有不同的事件模型
    #A）标准事件模型
    #Select、poll属于标准事件模型，如果当前系统不存在更有效的方法，nginx会选择select或poll
    #B）高效事件模型
    #Kqueue：使用于FreeBSD 4.1+, OpenBSD 2.9+, NetBSD 2.0 和 MacOS X.使用双处理器的MacOS X系统使用kqueue可能会造成内核崩溃。
    #Epoll：使用于Linux内核2.6版本及以后的系统。
    #/dev/poll：使用于Solaris 7 11/99+，HP/UX 11.22+ (eventport)，IRIX 6.5.15+ 和 Tru64 UNIX 5.1A+。
    #Eventport：使用于Solaris 10。 为了防止出现内核崩溃的问题， 有必要安装安全补丁。
    use epoll;

    #单个进程最大连接数（最大连接数=连接数*进程数）
    #根据硬件调整，和前面工作进程配合起来用，尽量大，但是别把cpu跑到100%就行。每个进程允许的最多连接数，理论上每台nginx服务器的最大连接数为。
    worker_connections 65535;

    #keepalive超时时间。
    keepalive_timeout 60;

    #客户端请求头部的缓冲区大小。这个可以根据你的系统分页大小来设置，一般一个请求头的大小不会超过1k，不过由于一般系统分页都要大于1k，所以这里设置为分页大小。
    #分页大小可以用命令getconf PAGESIZE 取得。
    #[root@web001 ~]# getconf PAGESIZE
    #4096
    #但也有client_header_buffer_size超过4k的情况，但是client_header_buffer_size该值必须设置为“系统分页大小”的整倍数。
    client_header_buffer_size 4k;

    #这个将为打开文件指定缓存，默认是没有启用的，max指定缓存数量，建议和打开文件数一致，inactive是指经过多长时间文件没被请求后删除缓存。
    open_file_cache max=65535 inactive=60s;

    #这个是指多长时间检查一次缓存的有效信息。
    #语法:open_file_cache_valid time 默认值:open_file_cache_valid 60 使用字段:http, server, location 这个指令指定了何时需要检查open_file_cache中缓存项目的有效信息.
    open_file_cache_valid 80s;

    #open_file_cache指令中的inactive参数时间内文件的最少使用次数，如果超过这个数字，文件描述符一直是在缓存中打开的，如上例，如果有一个文件在inactive时间内一次没被使用，它将被移除。
    #语法:open_file_cache_min_uses number 默认值:open_file_cache_min_uses 1 使用字段:http, server, location  这个指令指定了在open_file_cache指令无效的参数中一定的时间范围内可以使用的最小文件数,如果使用更大的值,文件描述符在cache中总是打开状态.
    open_file_cache_min_uses 1;
    
    #语法:open_file_cache_errors on | off 默认值:open_file_cache_errors off 使用字段:http, server, location 这个指令指定是否在搜索一个文件是记录cache错误.
    open_file_cache_errors on;
}
 
 
 
#设定http服务器，利用它的反向代理功能提供负载均衡支持
http
{
    #文件扩展名与文件类型映射表
    include mime.types;

    #默认文件类型
    default_type application/octet-stream;

    #默认编码
    #charset utf-8;

    #服务器名字的hash表大小
    #保存服务器名字的hash表是由指令server_names_hash_max_size 和server_names_hash_bucket_size所控制的。参数hash bucket size总是等于hash表的大小，并且是一路处理器缓存大小的倍数。在减少了在内存中的存取次数后，使在处理器中加速查找hash表键值成为可能。如果hash bucket size等于一路处理器缓存的大小，那么在查找键的时候，最坏的情况下在内存中查找的次数为2。第一次是确定存储单元的地址，第二次是在存储单元中查找键 值。因此，如果Nginx给出需要增大hash max size 或 hash bucket size的提示，那么首要的是增大前一个参数的大小.
    server_names_hash_bucket_size 128;

    #客户端请求头部的缓冲区大小。这个可以根据你的系统分页大小来设置，一般一个请求的头部大小不会超过1k，不过由于一般系统分页都要大于1k，所以这里设置为分页大小。分页大小可以用命令getconf PAGESIZE取得。
    client_header_buffer_size 32k;

    #客户请求头缓冲大小。nginx默认会用client_header_buffer_size这个buffer来读取header值，如果header过大，它会使用large_client_header_buffers来读取。
    large_client_header_buffers 4 64k;

    #设定通过nginx上传文件的大小
    client_max_body_size 8m;

    #开启高效文件传输模式，sendfile指令指定nginx是否调用sendfile函数来输出文件，对于普通应用设为 on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。注意：如果图片显示不正常把这个改成off。
    #sendfile指令指定 nginx 是否调用sendfile 函数（zero copy 方式）来输出文件，对于普通应用，必须设为on。如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络IO处理速度，降低系统uptime。
    sendfile on;

    #开启目录列表访问，合适下载服务器，默认关闭。
    autoindex on;

    #此选项允许或禁止使用socke的TCP_CORK的选项，此选项仅在使用sendfile的时候使用
    tcp_nopush on;
     
    tcp_nodelay on;

    #长连接超时时间，单位是秒
    keepalive_timeout 120;

    #FastCGI相关参数是为了改善网站的性能：减少资源占用，提高访问速度。下面参数看字面意思都能理解。
    fastcgi_connect_timeout 300;
    fastcgi_send_timeout 300;
    fastcgi_read_timeout 300;
    fastcgi_buffer_size 64k;
    fastcgi_buffers 4 64k;
    fastcgi_busy_buffers_size 128k;
    fastcgi_temp_file_write_size 128k;

    #gzip模块设置
    gzip on; #开启gzip压缩输出
    gzip_min_length 1k;    #最小压缩文件大小
    gzip_buffers 4 16k;    #压缩缓冲区
    gzip_http_version 1.0;    #压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
    gzip_comp_level 2;    #压缩等级
    gzip_types text/plain application/x-javascript text/css application/xml;    #压缩类型，默认就已经包含textml，所以下面就不用再写了，写上去也不会有问题，但是会有一个warn。
    gzip_vary on;

    #开启限制IP连接数的时候需要使用
    #limit_zone crawler $binary_remote_addr 10m;



    #负载均衡配置
    upstream piao.jd.com {
     
        #upstream的负载均衡，weight是权重，可以根据机器配置定义权重。weigth参数表示权值，权值越高被分配到的几率越大。
        server 192.168.80.121:80 weight=3;
        server 192.168.80.122:80 weight=2;
        server 192.168.80.123:80 weight=3;

        #nginx的upstream目前支持4种方式的分配
        #1、轮询（默认）
        #每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。
        #2、weight
        #指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。
        #例如：
        #upstream bakend {
        #    server 192.168.0.14 weight=10;
        #    server 192.168.0.15 weight=10;
        #}
        #2、ip_hash
        #每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。
        #例如：
        #upstream bakend {
        #    ip_hash;
        #    server 192.168.0.14:88;
        #    server 192.168.0.15:80;
        #}
        #3、fair（第三方）
        #按后端服务器的响应时间来分配请求，响应时间短的优先分配。
        #upstream backend {
        #    server server1;
        #    server server2;
        #    fair;
        #}
        #4、url_hash（第三方）
        #按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，后端服务器为缓存时比较有效。
        #例：在upstream中加入hash语句，server语句中不能写入weight等其他的参数，hash_method是使用的hash算法
        #upstream backend {
        #    server squid1:3128;
        #    server squid2:3128;
        #    hash $request_uri;
        #    hash_method crc32;
        #}

        #tips:
        #upstream bakend{#定义负载均衡设备的Ip及设备状态}{
        #    ip_hash;
        #    server 127.0.0.1:9090 down;
        #    server 127.0.0.1:8080 weight=2;
        #    server 127.0.0.1:6060;
        #    server 127.0.0.1:7070 backup;
        #}
        #在需要使用负载均衡的server中增加 proxy_pass http://bakend/;

        #每个设备的状态设置为:
        #1.down表示单前的server暂时不参与负载
        #2.weight为weight越大，负载的权重就越大。
        #3.max_fails：允许请求失败的次数默认为1.当超过最大次数时，返回proxy_next_upstream模块定义的错误
        #4.fail_timeout:max_fails次失败后，暂停的时间。
        #5.backup： 其它所有的非backup机器down或者忙的时候，请求backup机器。所以这台机器压力会最轻。

        #nginx支持同时设置多组的负载均衡，用来给不用的server来使用。
        #client_body_in_file_only设置为On 可以讲client post过来的数据记录到文件中用来做debug
        #client_body_temp_path设置记录文件的目录 可以设置最多3层目录
        #location对URL进行匹配.可以进行重定向或者进行新的代理 负载均衡
    }
     
     
     
    #虚拟主机的配置
    server
    {
        #监听端口
        listen 80;

        #域名可以有多个，用空格隔开
        server_name www.jd.com jd.com;
        index index.html index.htm index.php;
        root /data/www/jd;

        #对******进行负载均衡
        location ~ .*.(php|php5)?$
        {
            fastcgi_pass 127.0.0.1:9000;
            fastcgi_index index.php;
            include fastcgi.conf;
        }
         
        #图片缓存时间设置
        location ~ .*.(gif|jpg|jpeg|png|bmp|swf)$
        {
            expires 10d;
        }
         
        #JS和CSS缓存时间设置
        location ~ .*.(js|css)?$
        {
            expires 1h;
        }
         
        #日志格式设定
        #$remote_addr与$http_x_forwarded_for用以记录客户端的ip地址；
        #$remote_user：用来记录客户端用户名称；
        #$time_local： 用来记录访问时间与时区；
        #$request： 用来记录请求的url与http协议；
        #$status： 用来记录请求状态；成功是200，
        #$body_bytes_sent ：记录发送给客户端文件主体内容大小；
        #$http_referer：用来记录从那个页面链接访问过来的；
        #$http_user_agent：记录客户浏览器的相关信息；
        #通常web服务器放在反向代理的后面，这样就不能获取到客户的IP地址了，通过$remote_add拿到的IP地址是反向代理服务器的iP地址。反向代理服务器在转发请求的http头信息中，可以增加x_forwarded_for信息，用以记录原有客户端的IP地址和原来客户端的请求的服务器地址。
        log_format access '$remote_addr - $remote_user [$time_local] "$request" '
        '$status $body_bytes_sent "$http_referer" '
        '"$http_user_agent" $http_x_forwarded_for';
         
        #定义本虚拟主机的访问日志
        access_log  /usr/local/nginx/logs/host.access.log  main;
        access_log  /usr/local/nginx/logs/host.access.404.log  log404;
         
        #对 "/" 启用反向代理
        location / {
            proxy_pass http://127.0.0.1:88;
            proxy_redirect off;
            proxy_set_header X-Real-IP $remote_addr;
             
            #后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
             
            #以下是一些反向代理的配置，可选。
            proxy_set_header Host $host;

            #允许客户端请求的最大单文件字节数
            client_max_body_size 10m;

            #缓冲区代理缓冲用户端请求的最大字节数，
            #如果把它设置为比较大的数值，例如256k，那么，无论使用firefox还是IE浏览器，来提交任意小于256k的图片，都很正常。如果注释该指令，使用默认的client_body_buffer_size设置，也就是操作系统页面大小的两倍，8k或者16k，问题就出现了。
            #无论使用firefox4.0还是IE8.0，提交一个比较大，200k左右的图片，都返回500 Internal Server Error错误
            client_body_buffer_size 128k;

            #表示使nginx阻止HTTP应答代码为400或者更高的应答。
            proxy_intercept_errors on;

            #后端服务器连接的超时时间_发起握手等候响应超时时间
            #nginx跟后端服务器连接超时时间(代理连接超时)
            proxy_connect_timeout 90;

            #后端服务器数据回传时间(代理发送超时)
            #后端服务器数据回传时间_就是在规定时间之内后端服务器必须传完所有的数据
            proxy_send_timeout 90;

            #连接成功后，后端服务器响应时间(代理接收超时)
            #连接成功后_等候后端服务器响应时间_其实已经进入后端的排队之中等候处理（也可以说是后端服务器处理请求的时间）
            proxy_read_timeout 90;

            #设置代理服务器（nginx）保存用户头信息的缓冲区大小
            #设置从被代理服务器读取的第一部分应答的缓冲区大小，通常情况下这部分应答中包含一个小的应答头，默认情况下这个值的大小为指令proxy_buffers中指定的一个缓冲区的大小，不过可以将其设置为更小
            proxy_buffer_size 4k;

            #proxy_buffers缓冲区，网页平均在32k以下的设置
            #设置用于读取应答（来自被代理服务器）的缓冲区数目和大小，默认情况也为分页大小，根据操作系统的不同可能是4k或者8k
            proxy_buffers 4 32k;

            #高负荷下缓冲大小（proxy_buffers*2）
            proxy_busy_buffers_size 64k;

            #设置在写入proxy_temp_path时数据的大小，预防一个工作进程在传递文件时阻塞太长
            #设定缓存文件夹大小，大于这个值，将从upstream服务器传
            proxy_temp_file_write_size 64k;
        }
         
         
        #设定查看Nginx状态的地址
        location /NginxStatus {
            stub_status on;
            access_log on;
            auth_basic "NginxStatus";
            auth_basic_user_file confpasswd;
            #htpasswd文件的内容可以用apache提供的htpasswd工具来产生。
        }
         
        #本地动静分离反向代理配置
        #所有jsp的页面均交由tomcat或resin处理
        location ~ .(jsp|jspx|do)?$ {
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://127.0.0.1:8080;
        }
         
        #所有静态文件由nginx直接读取不经过tomcat或resin
        location ~ .*.(htm|html|gif|jpg|jpeg|png|bmp|swf|ioc|rar|zip|txt|flv|mid|doc|ppt|
        pdf|xls|mp3|wma)$
        {
            expires 15d; 
        }
         
        location ~ .*.(js|css)?$
        {
            expires 1h;
        }
    }
}
```
### 参考
http://blog.csdn.net/hzsunshine/article/details/63687054
https://www.cnblogs.com/hunttown/p/5759959.html