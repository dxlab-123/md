# 部署看板分发系统

操作系统：ubuntu 20.04 64位

## 安装系统

配置网络（vmbr1）

vim /etc/netplan/01-network-manager-all.yaml

![image-20211103085228135](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141548352.png)

这里需要注意下“：”后面必须空一格才能生效！

配置完成后，输入命令：sudo netplan apply 

## 安装docker、docker-compose

```sh
# step 1: 安装必要的一些系统工具
sudo apt-get update
sudo apt-get upgrade 
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
# step 2: 安装GPG证书
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
# Step 3: 写入软件源信息
sudo add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
# Step 4: 更新并安装Docker-CE
sudo apt-get -y update & sudo apt-get -y install docker-ce
# Step 5: 配置镜像源
修改 /etc/docker/daemon.json (如果该文件不存在，则创建)
{
	"registry-mirrors": ["https://hub-mirror.c.163.com"]
}
# Step 6: 创建并修改完daemon.json文件后，需要让这个文件生效
a.修改完成后reload配置文件
sudo systemctl daemon-reload
 
b.重启docker服务
sudo systemctl restart docker.service
 
c.查看状态
sudo systemctl status docker -l
 
d.查看服务
sudo docker info
# Step 7: 安装docker-compose
sudo apt-get -y install docker-compose
```

## 部署系统

```sh
# 创建相应的文件夹
mkdir kanban-docker  kanban-reactor-ui-docker  kanban-vue-ui-docker 
# 上传压缩包，至对应的文件夹，用winSCP,host name：dxlab.cloud,port：10722
# 解压缩
unzip xxx.zip
# 镜像导入
docker load < xxx.tar
# 启动
docker-compose up -d
# elasticsearch需要赋权
chmod 777 -R data

```

