# Nginx

看的这个教程：[NGINX Tutorial for Beginners (youtube.com)](https://www.youtube.com/watch?v=9t9Mp0BGnyI)，Nginx的发音是 engine X。

## nginx 模板

忘了的配置项直接问AI。

```conf
worker_processes 1;

events {
    worker_connections 1024;
}

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    upstream django {
        server 0.0.0.0:8000;
    }

    server {
        listen 80;
        server_name 0.0.0.0;
	    client_max_body_size 10M;
        # 前端静态文件服务
        location / {
            root /app/dist;
            try_files $uri $uri/ /index.html;
        }

        # Django 静态文件
        location /static/ {
            alias /app/dist/static/;
        }

        # Django 媒体文件
        location /media/ {
            alias /app/media/;
        }

        # 代理 Django 请求
        location /api/ {
            proxy_pass http://django;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'Origin, Content-Type, Accept, Authorization';
            add_header 'Access-Control-Allow-Credentials' 'true';
        }

        location /admin/ {
            proxy_pass http://django;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'Origin, Content-Type, Accept, Authorization';
            add_header 'Access-Control-Allow-Credentials' 'true';
        }
    }
}

```