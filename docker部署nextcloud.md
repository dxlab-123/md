# docker部署nextcloud

docker run -d --name mysql -e MYSQL_ROOT_PASSWORD="luwen\!985" -v /home/docker/mysql:/var/lib/mysql -p 3306:3306 mysql

docker exec -it 12e950e62886 /bin/bash

mysql -u root -p


CREATE DATABASE nextcloud;
CREATE USER 'ncadmin'@'%' IDENTIFIED BY 'ncadmin!985';
GRANT ALL ON nextcloud.* TO 'ncadmin'@'%';


docker network create --driver bridge nextcloud

docker run -d --restart=always --network nextcloud -v /home/docker/nextcloud:/var/www/html --link mysql:db --name nextcloud -p 9001:80 nextcloud


docker run -d --network nextcloud --name documentserver -v /home/docker/documentserver:/var/www/onlyoffice/documentserver -p 9002:80 --restart=always documentserver


docker run -d -p 80:80 --restart=always --name nginx -v /home/docker/nginx/conf:/etc/nginx/conf.d -v /home/docker/nginx/www:/usr/share/nginx/html -v /home/docker/nginx/logs:/var/log/nginx nginx


docker run -d -p 80:80 --restart=always --name nginx --network redmine -v /home/docker/nginx/conf:/etc/nginx/conf.d -v /home/docker/nginx/www:/usr/share/nginx/html -v /home/docker/nginx/logs:/var/log/nginx nginx

nextcloud.conf

server {
        listen 80;
        server_name nc.dxcms.com;

        location / {
                proxy_pass http://127.0.0.1:9001;
    
    	proxy_set_header Host $host;
    	proxy_set_header X-Real-IP $remote_addr;
    	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    	proxy_redirect off;
    
    	proxy_set_header Host $host;
    	proxy_set_header X-Real-IP $remote_addr;
    	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    	proxy_set_header X-Forwarded-Proto $scheme;
    	proxy_set_header X-Forwarded-Port $server_port;
        }
}

server {
        listen 80;
        server_name drawio.dxcms.com;

        location / {
                proxy_pass http://127.0.0.1:9101;
    	proxy_redirect off;
    	proxy_set_header Host $host;
    	proxy_set_header X-Real-IP $remote_addr;
    	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    	proxy_set_header X-Forwarded-Host $server_name;
    	proxy_redirect default;
        }
}

server {
        listen 9101;
        location / {
                root /usr/share/nginx/html/drawio;
                index index.html index.htm;
                charset utf-8;
        }
}

/usr/share/nginx/html/drawio
firewall-cmd --zone=public --add-port=9002/tcp
firewall-cmd --reload


docker run -d --restart=always --name redmine --network redmine -p 9001:3000 redmine
