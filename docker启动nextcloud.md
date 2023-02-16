# docker启动nextcloud

## 部署

vim docker-compose.yml

```yaml
version: '2'
services:
  db:
    image: mysql
    container_name: mysql
    restart: always
    volumes:
      - ./db/mysql:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=123456
      - MYSQL_PASSWORD=123456
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
    ports: 
      - 6379:6379
  app:
    image: nextcloud
    container_name: nextcloud
    ports:
      - 8888:80
    links:
      - db
    volumes:
      - ./db/nextcloud:/var/www/html
    restart: always
```

docker-compose up -d

网站：dxlab.xyz:8087

配置

![image-20220902132333813](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141601078.png)

登录：

dx_admin

admin_123456

## 优化

```yaml
version: '2'  
services:  
  db:  
    container_name: cloud_db  
    image: mysql:5.7
    volumes:  
      - /var/data/cloud/mysql:/var/lib/mysql
      - /etc/localtime:/etc/localtime 
    environment:  
      MYSQL_ROOT_PASSWORD: nextcloud
      MYSQL_DATABASE: nextcloud  
  nextcloud:  
    container_name: cloud_web  
    depends_on:  
      - db  
    image: nextcloud  
    volumes:  
      - /var/data/cloud/config:/var/www/html/config  
      - /var/data/cloud/data:/var/www/html/data  
      - /var/data/cloud/apps:/var/www/html/apps  
    ports:  
      - "8090:80"  
  onlyoffice:
    container_name: cloud_office
    image: onlyoffice/documentserver
    ports:
      - "9100:80"
networks:  
  default:  
    external:  
      name: nextcloud
```



onlyoffice

解压缩

本期视频主要讲如何完善Nextcloud中的Extract插件解压rar和7z等格式压缩文件的能力，但是还存在解压文件中文名乱码和外部存储解压文件Cross-device link的错误 进入Nextcloud容器的命令： sudo docker exec -it nextcloud /bin/bash 在Nextcloud容器内安装unrar的命令： apk add unrar 在Nextcloud容器内安装p7zip的命令： apk add p7zip 退出容器的命令： exit