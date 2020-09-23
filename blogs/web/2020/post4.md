---
title: nginx配置以及生成https证书
date: 2020-09-09
tags:
 - Nginx
 - Https
categories:
 -  Web
---

下载nginx包之后，执行安装步骤

```bash
./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
make
make install
```

通过certbot安装https证书

```bash
wget https://dl.eff.org/certbot-auto 
chmod a+x certbot-auto
```
编辑nginx的配置文件nginx.conf, 在默认的server中添加：

```bash
location ^~ /.well-known/acme-challenge/ {
   default_type "text/plain";
   root     /usr/local/nginx/html;
}

location = /.well-known/acme-challenge/ {
   return 404;
}
```
然后，验证配置文件并重新启动nginx。

生成证书,jscode.top，是本站网站，需要替换成自己的。

```bash
 letsencrypt certonly --webroot -w /usr/local/nginx/html -d jscode.top -d www.jscode.top
```

下面的文件是我自己的nginx配置文件，仅作参考

```bash
    #user  nobody;
    worker_processes  1;

    #error_log  logs/error.log;
    #error_log  logs/error.log  notice;
    #error_log  logs/error.log  info;

    #pid        logs/nginx.pid;


    events {
    worker_connections  1024;
    }


    http {
    include       mime.types;
    default_type  applic
    ation/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;


    server {
    listen       443 ssl;
        listen [::]:443 ssl ipv6only=on;
    server_name  jscode.top;
    #rewrite ^(.*) https://$server_name$1 permanent;
    ssl_certificate      /etc/letsencrypt/live/www.jscode.top/fullchain.pem;
    ssl_certificate_key  /etc/letsencrypt/live/www.jscode.top/privkey.pem;
    ssl_trusted_certificate /etc/letsencrypt/live/www.jscode.top/chain.pem;
    #ssl_session_cache    shared:SSL:1m;
    #ssl_session_timeout  5m;

    #ssl_ciphers  HIGH:!aNULL:!MD5;
    # ssl_prefer_server_ciphers  on;

    location / {
    proxy_set_header  Host $http_host;
    proxy_set_header  X-NginX-Proxy true;
    proxy_pass        http://127.0.0.1:3000/;
    proxy_redirect    off;
    }
    }
    server {
    listen 80;
    server_name jscode.top;
    rewrite ^(.*) https://$server_name$1 permanent;
    location ^~ /.well-known/acme-challenge/ {
    default_type "text/plain";
    root     /usr/local/nginx/html;
    }

    location = /.well-known/acme-challenge/ {
    return 404;
    }

    #rewrite ^(.*) https://$server_name$1 permanent;
    }
    }
```

certbot在安装时已经启动了自动续期命令，使用crontab -e的命令来可以查看自动任务，命令行：

```bash
    crontab -e
```


