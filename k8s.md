# k8s

熟悉linux和docker

了解共享存储和jenkins

了解iptables和dns服务

## 应用场景

***微服务架构***

![image-20220210102110113](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141602707.png)

![image-20220210102158730](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141602449.png)

![image-20220210102503577](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141602277.png)

## 核心组件及架构

![image-20220210102732192](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141603323.png)

![image-20220210103311692](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141603286.png)

## 集群安装-centos 7

![image-20220210104040377](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141603829.png)

![image-20220210104624550](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141603032.png)

![image-20220210104729312](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141603962.png)

![image-20220210105103211](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141603750.png)

![image-20220210105901607](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141603453.png)

![image-20220210110511806](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141604602.png)

## pod

![image-20220210111109041](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141604230.png)

# ubuntu 安装k8s

3台ubuntu系统(版本都是18), 网络(互相之间可以ping通).

- 每台机器2GB及以上内容
- 每台机器至少2核cpu

三台机子的ip地址:

```dockerfile
# 主节点/控制节点 主机名: k8s-master（/etc/hostname）
192.168.1.90
# 从节点1 主机名: k8s-node1
192.168.1.91
# 从节点2 主机名: k8s-node2
192.168.1.92
```

***禁用swap***

```dockerfile
vim /etf/fstab
# 注释掉文件中包含swap的那一行（最后一行）
# /swapfile                                 # none            swap    sw              0       0

# 然后重启下
sudo shutdown -r now

# 验证
top
# KiB Swap 后均为 0 就说明关闭成功了。
```

***关闭防火墙***

```dockerfile
sudo systemctl disable ufw.service && sudo systemctl stop ufw.service
```

***docker***

```dockerfile
# step 2: 安装GPG证书
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
# Step 3: 写入软件源信息
sudo add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
# Step 4: 更新并安装Docker-CE
sudo apt-get -y update
sudo apt-get -y install docker-ce
# Step 5: 配置镜像源
修改 /etc/docker/daemon.json (如果该文件不存在，则创建)
{
	"registry-mirrors": ["https://hub-mirror.c.163.com"],
	"exec-opts": ["native.cgroupdriver=systemd"]   # 修改cgroup
}
# Step 6: 创建并修改完daemon.json文件后，需要让这个文件生效
a.修改完成后reload配置文件
sudo systemctl daemon-reload
 
b.重启docker服务
sudo systemctl restart docker.service
```

***安装`kubeadm` `kubelet`和`kubectl`***

```dockerfile
#方法一：
# 更新
sudo -E apt-get update -y && sudo -E apt-get install -y apt-transport-https ca-certificates curl
# 外网浏览器访问改地址，手动下载pt-key.gpg
https://packages.cloud.google.com/apt/doc/apt-key.gpg
# cp
sudo cp /home/yundong/Downloads/apt-key.gpg  /usr/share/keyrings/kubernetes-archive-keyring.gpg
# 执行
sudo -E kubectl apply -f kube-flannel.yml
# 配置国内镜像
echo “deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main” | sudo tee /etc/apt/sources.list.d/kubernetes.list
# 给阿里云源添加key
cat /usr/share/keyrings/kubernetes-archive-keyring.gpg |  sudo apt-key add -
# 下载
sudo -E apt-get update -y && sudo -E apt-get install -y kubelet kubeadm kubectl && sudo -E apt-mark hold kubelet kubeadm kubectl
# 查看状态
systemctl status kubelet

#方法二：
sudo apt-get update && sudo apt-get install -y ca-certificates curl software-properties-common apt-transport-https

curl -s https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | sudo apt-key add -

sudo tee /etc/apt/sources.list.d/kubernetes.list <<EOF 
deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
EOF

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

***通过`kubeadm`初始化集群***

```dockerfile
# master初始化
sudo -E kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=192.168.1.71 --image-repository registry.aliyuncs.com/google_containers
# Your Kubernetes master has initialized successfully! 初始化成功
# 请把最后那行以kubeadm join开头的命令复制下来，之后安装工作节点时要用到的，如果你不慎遗失了该命令，可以在master节点上使用kubeadm token create --print-join-command命令来重新生成一条。
# 根据提示执行
export KUBECONFIG=/etc/kubernetes/admin.conf
# 如果在初始化过程中出现了任何Error导致初始化终止了，使用kubeadm reset重置之后再重新进行初始化。

# node初始化
# kubeadm token create --print-join-command
kubeadm join xxxx

# master 查看
kubectl get nodes
```

![image-20220212145123156](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141604734.png)

***通过命令查看集群情况***

```shell
# 查看kubelet的情况：
systemctl status kubelet  		
# 用命令查看cluster的信息：
kubectl cluster-info  			
# 查看节点列表
kubectl get nodes    			
# 查看节点详细信息
kubectl get nodes -o wide  		
# 查看组件状态kube-scheduler和kube-controller-manager状态，正常状态为Healthy     
kubectl get cs   
# 还原
kubeadm reset
```

## pve kubeadm join指令

```zsh
kubeadm join 192.168.1.90:6443 --token efs82r.p1lgduwzplbauuxa \
    --discovery-token-ca-cert-hash sha256:aad3c993412b0100a2afa7de6abb8430f944c9af02f77ac0a966994ea2350483
```



## 问题

kubectl get node 状态为 NotReady

![image-20220325170917333](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141604440.png)



```shell
kubectl describe node xxxx
# 显示 container runtime network not ready: NetworkReady=false reason:NetworkPluginNotReady message:docker: network plugin is not ready: cni config uninitialized
# 添加 cni 插件
 kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```



