# docker influxdb 指令

```bash
# 运行influxdb
docker run -d -p 8086:8086 --name influxdb --volume /path:/var/lib/influxdb influxdb

# 进入容器
docker exec -it influxdb /bin/bash
```



## influxdb 1.x

### 常用命令

```bash
#查看版本
influxd version
```

### 用户管理

# 创建用户
CREATE USER "username" WITH PASSWORD 'password'
CREATE USER "username" WITH PASSWORD 'password' WITH ALL PRIVILEGES; # 创建root用户
# 查看用户
SHOW USERS # 可加也可不加;
# 删除用户
DROP USER "username"
# 使用账号密码登录（influx 默认不启动认证机制，需要修改配置文件来启动认证机制）
influx -host 'influxdb_host' -port '8086' -username 'user_name'
1
2
3
4
5
6
7
8
9
数据库、表
SHOW DATABASES
CREATE DATABASE test
DROP DATABASE test
USE test
SHOW MEASUREMENTS
DROP MEASUREMENT "measurementName"
# 这里没有创建表，插入数据时创建表名
1
2
3
4
5
6
7
插入数据
# 通过命令行
insert weather,altitude=1000,area=北 temperature=11,humidity=-4
insert your_measurement,tag_name=tag_value... column_name=column_value
# 通过http接口
curl -i -XPOST 'http://localhost:8086/write?db=testDB' --data-binary 'weather,altitude=1000,area=北 temperature=11,humidity=-4'
1
2
3
4
5
weather ： 表名
altitude=1000,area=北 ： tag
temperature=11,humidity=-4 ：field
查询数据
# 通过命令行
SELECT * FROM weather ORDER BY time DESC LIMIT 3
# 通过Http接口
curl -G 'http://localhost:8086/query?pretty=true' --data-urlencode "db=testDB" --data-urlencode "q=SELECT * FROM weather ORDER BY time DESC LIMIT 3"
1
2
3
4
连续查询
SHOW CONTINUOUS QUERIES
CREATE CONTINUOUS QUERY cq_30m ON testDB BEGIN SELECT mean(temperature) INTO weather30m FROM weather GROUP BY time(30m) END
DROP CONTINUOUS QUERY <cq_name> ON <database_name>
1
2
3
Continuous Queries连续查询，相当于实现了数据聚合。

cq_30m：连续查询的名字
testDB：具体的数据库名
mean(temperature): 算平均温度
weather： 当前表名
weather30m： 存新数据的表名
30m：时间间隔为30分钟
数据删除
SHOW RETENTION POLICIES ON "testDB"
CREATE RETENTION POLICY "rp_name" ON "db_name" DURATION 30d REPLICATION 1 DEFAULT # 创建新的Retention Policies
ALTER RETENTION POLICY "rp_name" ON db_name" DURATION 3w DEFAULT # 修改
DROP RETENTION POLICY "rp_name" ON "db_name" # 删除
1
2
3
4
间接实现数据删除，通过Retention Policies指定数据的保留时间，超过保留时间的就会被删除。

rp_name：策略名
db_name：具体的数据库名
30d：保存30天，30天之前的数据将被删除
它具有各种时间参数，比如：h（小时），w（星期）
REPLICATION 1：副本个数，这里填1就可以了
DEFAULT 设为默认的策略
查看measurement的数据格式
show tag keys on <database> from <measurement>
show tag values on <database> from <measurement> with KEY [ [<operator> "<tag_key>" | <regular_expression>] | [IN ("<tag_key1>","<tag_key2")]] [WHERE <tag_key> <operator> ['<tag_value>' | <regular_expression>]] [LIMIT_clause] [OFFSET_clause]
show field keys on <database> from <measurement>
show series on <database> from <measurement>
1
2
3
4

