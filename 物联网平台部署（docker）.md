# 物联网平台部署（docker）

## 1 安装docker，docker-compose

## 2 docker登录到registry.cn-shenzhen.aliyuncs.com

```dockerfile
docker login registry.cn-shenzhen.aliyuncs.com
```

username：test@jetlinks

password：test123456

## 3 创建docker-compose.yml文件

内容为：

```dockerfile
version: '2'
services:
  redis:
    image: redis:5.0.4
    container_name: jetlinks-redis
    ports:
      - "6379:6379"
    volumes:
      - "./data/redis:/data"
    command: redis-server --appendonly yes --requirepass "JetLinks@redis"
    environment:
      - TZ=Asia/Shanghai
  elasticsearch:
    image: elasticsearch:6.8.6
    container_name: jetlinks-elasticsearch
    environment:
      ES_JAVA_OPTS: -Djava.net.preferIPv4Stack=true -Xms2g -Xmx2g
      transport.host: 0.0.0.0
      discovery.type: single-node
      bootstrap.memory_lock: "true"
      discovery.zen.minimum_master_nodes: 1
      discovery.zen.ping.unicast.hosts: elasticsearch
    volumes:
      - ./data/elasticsearch:/usr/share/elasticsearch/data
    ports:
      - "9200:9200"
      - "9300:9300"
  kibana:
    image: kibana:6.8.6
    container_name: jetlinks-kibana
    environment:
      ELASTICSEARCH_URL: http://elasticsearch:9200
    links:
      - elasticsearch:elasticsearch
    ports:
      - "5601:5601"
    depends_on:
      - elasticsearch
  postgres:
    image: postgres:11-alpine
    container_name: jetlinks-postgres
    ports:
      - "5432:5432"
    volumes:
      - "./data/postgres:/var/lib/postgresql/data"
    environment:
      POSTGRES_PASSWORD: jetlinks
      POSTGRES_DB: jetlinks
      TZ: Asia/Shanghai
  ui:
    image: registry.cn-shenzhen.aliyuncs.com/jetlinks/jetlinks-ui-antd:1.10.0
    container_name: jetlinks-pro-ui
    ports:
      - 9000:80
    environment:
      - "API_BASE_PATH=http://jetlinks:8844/" #API根路径
    volumes:
      - "./data/upload:/usr/share/nginx/html/upload"
    depends_on:
      - jetlinks
  big-screen:
    image: registry.cn-shenzhen.aliyuncs.com/jetlinks-pro/jetlinks-big-screen:1.0.0
    container_name: big-screen
    ports:
      - 9002:80
    environment:
      - "API_BASE_PATH=http://jetlinks:8844/" #API根路径
    depends_on:
      - jetlinks
  jetlinks:
    image: registry.cn-shenzhen.aliyuncs.com/jetlinks-pro/jetlinks-develop:1.10.0-SNAPSHOT
    container_name: jetlinks-pro
    ports:
      - 8844:8844 # API端口
      - 1883:1883 # MQTT端口
      - 11883:11883 # 通过openAPI使用mqtt订阅平台消息
      - 8100-8110:8100-8110 # 预留端口
      - 8200-8210:8200-8210/udp # udp端口
    volumes:
      - "./data/upload:/application/static/upload"  # 持久化上传的文件
      - "./data/protocols:/application/data/protocols"
      - "./data/dump:/application/data/dump"
    environment:
      - "TZ=Asia/Shanghai"
      - "JAVA_OPTS=-Duser.language=zh -Xms4G -Xmx4G -XX:+UseG1GC -Dmemory.watermark=0.12 -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=./data/dump/"
      - "elasticsearch.buffer.size=500"
      - "captcha.enabled=true"
      - "hsweb.file.upload.static-location=http://192.168.1.61:9000/upload"  #上传的静态文件访问根地址,为ui的地址.
      - "spring.r2dbc.url=r2dbc:postgresql://postgres:5432/jetlinks" #数据库连接地址
      - "spring.r2dbc.username=postgres"
      - "spring.r2dbc.password=jetlinks"
      - "spring.data.elasticsearch.client.reactive.endpoints=elasticsearch:9200"
      - "spring.redis.password=JetLinks@redis"
      - "spring.redis.host=redis"
      - "spring.redis.port=6379"
      - "logging.level.org.springframework=warn"
      - "logging.level.org.hswebframework=warn"
      - "logging.level.io.r2dbc=warn"
      - "logging.level.org.springframework.data=warn"
      - "logging.level.org.jetlinks=debug"
      - "logging.level.org.springframework.data.r2dbc.connectionfactory=warn"
      - "logging.level.org.jetlinks.pro.elastic.search.service.reactive=trace"
      - "api.urls.big-screen-path=http://192.168.1.61:9002/"
    links:
      - redis:redis
      - postgres:postgres
      - elasticsearch:elasticsearch
    depends_on:
      - elasticsearch
      - postgres
      - redis
```

## 4 执行docker-compose

在docker-compose.yml所在⽬录下执⾏docker-compose启动命令

```
docker-compose up -d
```

## 5 启动完成后访问系统

```
http://宿主机:9000
```

用户名：admin

密码：admin