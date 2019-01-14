title: 'Nginx TCP四层转发HTTPS '
author: star liu
tags:
  - N g i n x
categories:
  - 运维
  - Linux
date: 2019-01-14 16:42:00
---
奇怪的使用场景用到了nginx的四层代理转发

<!--more-->
## 配置nginx转发tcp
### 1.查看版本以及原始编译参数
```
nginx -V
```
```shell
nginx version: nginx/1.12.2
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-36) (GCC) 
built with OpenSSL 1.0.2k-fips  26 Jan 2017
TLS SNI support enabled
configure arguments: --prefix=/usr/share/nginx --sbin-path=/usr/sbin/nginx \
--modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf \
--error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log \
--http-client-body-temp-path=/var/lib/nginx/tmp/client_body \
--http-proxy-temp-path=/var/lib/nginx/tmp/proxy \
--http-fastcgi-temp-path=/var/lib/nginx/tmp/fastcgi \
--http-uwsgi-temp-path=/var/lib/nginx/tmp/uwsgi \
--http-scgi-temp-path=/var/lib/nginx/tmp/scgi --pid-path=/run/nginx.pid \
--lock-path=/run/lock/subsys/nginx --user=nginx --group=nginx --with-file-aio \
--with-http_auth_request_module --with-http_ssl_module --with-http_v2_module \
--with-http_realip_module --with-http_addition_module --with-http_xslt_module=dynamic \
--with-http_image_filter_module=dynamic --with-http_geoip_module=dynamic \
--with-http_sub_module --with-http_dav_module --with-http_flv_module \
--with-http_mp4_module --with-http_gunzip_module --with-http_gzip_static_module \
--with-http_random_index_module --with-http_secure_link_module \
--with-http_degradation_module --with-http_slice_module --with-http_stub_status_module \
--with-http_perl_module=dynamic --with-mail=dynamic --with-mail_ssl_module --with-pcre \
--with-pcre-jit --with-debug --with-stream=dynamic --with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -specs=/usr/lib/rpm/redhat/redhat-hardened-cc1 -m64 -mtune=generic' 
--with-ld-opt='-Wl,-z,relro -specs=/usr/lib/rpm/redhat/redhat-hardened-ld -Wl,-E' 
```
此处参考自己当前使用的nginx编译参数
### 2.编译，配置
在configure里增加如下参数启用模块
```shell
./configure --prefix=/usr/share/nginx --sbin-path=/usr/sbin/nginx \
--modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf \
--error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log \
--http-client-body-temp-path=/var/lib/nginx/tmp/client_body \
--http-proxy-temp-path=/var/lib/nginx/tmp/proxy \
--http-fastcgi-temp-path=/var/lib/nginx/tmp/fastcgi \
--http-uwsgi-temp-path=/var/lib/nginx/tmp/uwsgi \
--http-scgi-temp-path=/var/lib/nginx/tmp/scgi --pid-path=/run/nginx.pid \
--lock-path=/run/lock/subsys/nginx --user=nginx --group=nginx --with-file-aio \
--with-http_auth_request_module --with-http_ssl_module --with-http_v2_module \
--with-http_realip_module --with-http_addition_module --with-http_xslt_module=dynamic \
--with-http_image_filter_module=dynamic --with-http_geoip_module=dynamic \
--with-http_sub_module --with-http_dav_module --with-http_flv_module \
--with-http_mp4_module --with-http_gunzip_module --with-http_gzip_static_module \
--with-http_random_index_module --with-http_secure_link_module \
--with-http_degradation_module --with-http_slice_module --with-http_stub_status_module \
--with-http_perl_module=dynamic --with-mail=dynamic --with-mail_ssl_module --with-pcre \
--with-pcre-jit --with-stream --with-stream=dynamic --with-stream_ssl_preread_module \
--with-stream_ssl_module --with-debug --with-cc-opt='-O2 -g -pipe -Wall \
-Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 \
-grecord-gcc-switches -specs=/usr/lib/rpm/redhat/redhat-hardened-cc1 -m64 -mtune=generic' \
--with-ld-opt='-Wl,-z,relro -specs=/usr/lib/rpm/redhat/redhat-hardened-ld -Wl,-E' 
```
最主要的使用`--with-stream --with-stream_ssl_preread_module --with-stream_ssl_module` 这几个模块，有需要可以添加其它的模块；然后编译安装make && make install。
### 3.配置
具体配置命令参考pread module和stream module，启动的时候指定下面的配置文件
```nginx
user  root;
worker_processes  auto;
error_log  logs/error.log;
pid        logs/nginx.pid;
worker_rlimit_core   2G;
worker_rlimit_nofile 65535;
events {
    worker_connections  81920;
}
stream {
    log_format  main  '$remote_addr - [$time_local] $connection '
                      '$status $proxy_protocol_addr $server_addr ';
    access_log  logs/access.log  main;
    resolver 114.114.114.114;
    resolver_timeout 60s;
    variables_hash_bucket_size 512;
    server {
        listen       443;
        ssl_preread on;
        proxy_pass $ssl_preread_server_name:443;
        #大致看了一下源码，这里为什么需要配置端口也没有研究明白，求解释？
    }
}
```
此处我使用的的是另一套配置,可以时间根据域名转发https(这段配置丢到stream配置块内)
```nginx
map $ssl_preread_server_name $backend_pool {
    a.com    a;
    b.com    b;
}
upstream aaa{
    server 192.168.166.3:443;
}
upstream bbb{
    server 192.168.166.4:443;
}
server {
    listen 443;
    ssl_preread on;
    resolver 223.5.5.5; 
    proxy_pass $backend_pool;
    proxy_connect_timeout 15s;
    proxy_timeout 15s;
    proxy_next_upstream_timeout 15s;
    error_log /var/log/nginx/tcp_a.com.log warn;
}
```
这样就可以给`https://a.com` 和`https://b.com`两个域名做tcp层的代理了，其他域名如果也绑host过来就会被403掉。
这里其实是利用了NGINX的TCP转发做了SNI 反代，与普通的http/http的区别在于，TCP转发只是四层转发不需要证书
### 4.遇到的问题
- 编译错误1
```
./configure: error: the invalid value in --with-ld-opt="-Wl,-z,relro -specs=/usr/lib/rpm/redhat/redhat-hardened-ld -Wl,-E"
#解决方案
yum install redhat-rpm-config
```
- 缺少依赖库
```
#根据缺少内容进行安装
yum install perl-ExtUtils-Embed gd-devel gperftools-devel
yum install gcc 
yum install pcre-devel  openssl-devel
```