- 项目中常用 **vim /etc/nginx/conf.d/pure.conf**

```shell
server {
  listen 80; # 
  #server_name www.threewhite.top 101.35.248.127;
  client_max_body_size   90m;
  #charset koi8-r;
  #access_log /var/log/nginx/log/host.access.log main;
  
  location / {
    root /home/pure/dist;
    index index.html index.htm;
    try_files $uri $uri/ /index.html;
  }
    location /admin/ {
       proxy_pass http://127.0.0.1:9898;
   }
   location /cover/ {
       root /home/qzadmin/images/;
   }
     location /swagger-ui.html {
        proxy_pass http://127.0.0.1:9898/swagger-ui.html;
    }
    location /doc.html {
        proxy_pass http://127.0.0.1:9898/doc.html;
    }
    location /webjars/ {
        proxy_pass http://127.0.0.1:9898/webjars/;
    }
    location /swagger-resources {
        proxy_pass http://127.0.0.1:9898/swagger-resources;
    }
    location /v2/api-docs {
        proxy_pass http://127.0.0.1:9898/v2/api-docs;
    }
}
```



- 适用于服务器没有外网ip,把内网及端口映射出来。**nginx.conf**

```powershell
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 768;
        # multi_accept on;
}
# 重点是这里,另外这里的node1-data-1，node1-data-2，node1-data-3在自己电脑hosts配置一下对应ip
stream {
    server {
             listen 2181;
             proxy_pass    node1-data-2:2181;
    }
    server {
        listen 9092;
        proxy_pass      node1-data-1:9092;
    }
    server { 
        listen 9093;
        proxy_pass      node2-data-2:9092;
    }
    server { 
        listen 9094;
        proxy_pass      node3-data-3:9092;
    }
}
http {
        ##
        # Basic Settings
        ##
        sendfile on;
        tcp_nopush on;
        types_hash_max_size 2048;
        # server_tokens off;
        # server_names_hash_bucket_size 64;
        # server_name_in_redirect off;
        include /etc/nginx/mime.types;
        default_type application/octet-stream;
        ##
        # SSL Settings
        ##
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE
        ssl_prefer_server_ciphers on;
        ##
        # Logging Settings
        ##
        access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;
        ##
        # Gzip Settings
        ##
        gzip on;
        # gzip_vary on;
        # gzip_proxied any;
        # gzip_comp_level 6;
        # gzip_buffers 16 8k;
        # gzip_http_version 1.1;
        # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
        ##
        # Virtual Host Configs
        ##
        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
}
```

- 上例子stream server也可以写成以下形式

```powershell
stream {
    upstream zk {
        server 172.21.190.149:2181;
    }
    server {
             listen 2181;
             proxy_pass    zk;
    }
}
```

- 这篇文章吸收了nginx通关

[Nginx 从入门到实践，万字详解！](https://juejin.cn/post/6844904144235413512)

