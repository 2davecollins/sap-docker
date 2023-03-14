# HTTPS using NGINX and lets encrypt

[medium article](https://pentacent.medium.com/nginx-and-lets-encrypt-with-docker-in-less-than-5-minutes-b4b8a60d3a71)
```
version: '3'
services:
    nginx:
        image: nginx:1.15-alpine
        ports:
            - "80:80"
            - "443:443"
        restart: always
        volumes:
            - ./data/certbot/conf:/etc/letsencrypt
            - ./data/certbot/www:/var/www/certbot
      
    certbot:
        image: certbot/certbot


```
## config file data/nginx/app.conf/
### replace example.com with your domain
```
server {
    listen 80;
    server_name example.org;
    location / {
        return 301 https://$host$request_uri;
    }    
}
server {
    listen 443 ssl;
    server_name example.org;
    
    location / {
        proxy_pass http://example.org;  #for demo purposes
    }
}
```