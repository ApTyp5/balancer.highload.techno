# balancer.highload.techno

nginx.conf
-----


```
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 768;
        # multi_accept on;
}

http {
    upstream balance {
        server 35.228.60.241 fail_timeout=10s;
        server 35.228.93.31 fail_timeout=10s;
        server 35.228.123.165 fail_timeout=10s;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://balance;

            proxy_set_header Host $host;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Real-IP $remote_addr;

            proxy_next_upstream error timeout http_500;
            proxy_next_upstream_timeout 1s;
        }
    }
}
```
