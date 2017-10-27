# Nginx的安装与静态网站部署

## Nginx简介

>&emsp;&emsp;Nginx是一款高性能的HTTP服务器/反向代理服务器及电子邮件服务器。
官方测试能够支撑5万并发访问，并且CPU、内存等资源消耗却非常低，运行非常稳定。

### Nginx应用场景
- HTTP服务器。Nginx可以做网页静态服务器。
- 虚拟主机。 可以实现在一台服务器虚拟出多个网站。
- 反向代理，负载均衡。当单台服务器不能满足用户的请求时，需要用多台服务器集群可以使用
nginx做反向代理。并且多台服务器可以平均分担负载，不会因为某台服务器负载过高宕机
而某台服务器闲置的情况。

### Nginx在Linux下的安装
环境：
- gcc-c++，pcre，pcre-devel，zlib，zlib-devel，openssl，openssl-devel

1、上传并解压缩
>`tar zxvf nginx-1.8.0.tar.gz`

2、进入nginx-1.8.0目录，使用configure命令创建MakeFile文件
```shell
./configure \
--prefix=/usr/local/nginx \
--pid-path=/var/run/nginx/nginx.pid \
--lock-path=/var/lock/nginx.lock \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_gzip_static_module \
--http-client-body-temp-path=/var/temp/nginx/client \
--http-proxy-temp-path=/var/temp/nginx/proxy \
--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
--http-scgi-temp-path=/var/temp/nginx/scgi

```

configure 参数详解：
```shell
./configure \
--prefix=/usr \ 指向安装目录
--sbin-path=/usr/sbin/nginx \ 指向（执行） 程序文件（nginx）
--conf-path=/etc/nginx/nginx.conf \ 指向配置文件
--error-log-path=/var/log/nginx/error.log \ 指向 log
--http-log-path=/var/log/nginx/access.log \ 指向 http-log
--pid-path=/var/run/nginx/nginx.pid \ 指向 pid
--lock-path=/var/lock/nginx.lock \ （安装文件锁定， 防止安装文件被别人利用， 或自己
误操作。 ）
--user=nginx \
--group=nginx \
--with-http_ssl_module \ 启用 ngx_http_ssl_module 支持（使支持 https 请求， 需已安装
openssl）
--with-http_flv_module \ 启用 ngx_http_flv_module 支持（提供寻求内存使用基于时间的
偏移量文件）
--with-http_stub_status_module \ 启用 ngx_http_stub_status_module 支持（获取 nginx 自上次启
动以来的工作状态）
--with-http_gzip_static_module \ 启用 ngx_http_gzip_static_module 支持（在线实时压缩输出数据
流）
--http-client-body-temp-path=/var/tmp/nginx/client/ \ 设定 http 客户端请求临时文件路径
--http-proxy-temp-path=/var/tmp/nginx/proxy/ \ 设定 http 代理临时文件路径
--http-fastcgi-temp-path=/var/tmp/nginx/fcgi/ \ 设定 http fastcgi 临时文件路径
--http-uwsgi-temp-path=/var/tmp/nginx/uwsgi \ 设定 http uwsgi 临时文件路径
--http-scgi-temp-path=/var/tmp/nginx/scgi \ 设定 http scgi 临时文件路径
--with-pcre 启用 pcre 库
```
3、编译
>`make -j 8 `
参数 `-j` 用于指定编译的线程的个数。

4、安装
>`make install`

5、创建启动参数需要的文件目录`/var/temp/nginx/client`
>`mkdir /var/temp/nginx/client -p`

**Nginx的相关命令**
- 启动nginx :`./nginx`
- 关闭nginx :`./nginx -s quit`
- 重启nginx :`./nginx -s reload`

## Nginx虚拟主机的配置
1、上传静态网站
>将前端静态页cart.html以及图片样式等资源，上传至 /usr/local/nginx/cart

2、修改Nginx的修改配置文件：/usr/local/nginx/conf/nginx.conf
```shell
server {
    listen 81; # 对外访问端口
    server_name localhost;

    location / {

        root cart;
        index cart.html;

        }
    error_page 500 502 503 504 /50x.html;
    location = /50x.html{
        root html;
        }
    }
```
3、域名绑定

3.1 修改nginx.conf
```shell
server {
    listen 81; # 对外访问端口
    server_name cart.pinyougou.com;

    location / {

        root cart;
        index cart.html;

        }
    error_page 500 502 503 504 /50x.html;
    location = /50x.html{
        root html;
        }
    }
```
3.2、修改本机hosts映射
```shell
127.0.0.1 cart.pinyougou.com
```

## Nginx反向代理与负载均衡








