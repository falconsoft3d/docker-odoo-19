# docker-odoo-19
Cómo instalar odoo 19 con docker

# Clonar el repo
```
git clone https://github.com/falconsoft3d/docker-odoo-19.git
cd docker-odoo-19
```

# Levantar el docker
```
apt-get update && apt-get upgrade -y
apt install docker-compose
docker-compose up -d
```

# Levantar el docker
```
cd /root
docker-compose down
docker-compose up -d
```

# Si te da error filestore
```
mkdir -p filestore
sudo chown -R 101:101 filestore
sudo chmod -R 775 filestore
docker-compose down
docker-compose up -d
```

# Listar los dockers
```
docker ps
```

# error comun
```
docker-compose exec -u 0 odoo bash -lc "mkdir -p /var/lib/odoo/sessions && chown -R odoo:odoo /var/lib/odoo && chmod -R 700 /var/lib/odoo/sessions"
docker-compose restart odoo
```

# reiniciar el docker 
```
docker-compose restart odoo
docker-compose logs -f odoo
```

# ver los logs
```
docker-compose logs --tail=200 odoo
docker logs --tail=30 odoo19
docker-compose logs -f db
```


#ngix
```
sudo nano /etc/nginx/sites-available/default
```

```
upstream odoo{
        server localhost:8069;
}

server {
        server_name cascada-group.bim30.com;
        keepalive_timeout           600;
        client_header_timeout       600;
        client_max_body_size 1200M;
        client_body_timeout         600;
        proxy_connect_timeout       600;
        proxy_send_timeout          600;
        proxy_read_timeout          600;
        send_timeout                600;

        root /usr/share/nginx/html;
        index index.html index.htm;

        proxy_buffers 16 64k;
        proxy_buffer_size 128k;


	location /websocket {
   	  proxy_pass http://odoo;

    	  proxy_http_version 1.1;
    	  proxy_set_header Upgrade $http_upgrade;
    	  proxy_set_header Connection "upgrade";

    	  proxy_set_header Host $host;
    	  proxy_set_header X-Real-IP $remote_addr;
    	  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    	  proxy_set_header X-Forwarded-Proto https;

    	  proxy_read_timeout 720s;
    	  proxy_send_timeout 720s;
	}


	location / {
            proxy_pass  http://odoo;
            proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
            proxy_redirect off;
            proxy_set_header    Host            $host;
            proxy_set_header    X-Real-IP       $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    X-Forwarded-Proto https;
        }

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/cascada-group.bim30.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/cascada-group.bim30.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}


server {
    if ($host = cascada-group.bim30.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


        listen 80 default;
        server_name cascada-group.bim30.com;
    return 404; # managed by Certbot


}
```

```
sudo nginx -t




