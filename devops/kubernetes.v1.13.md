# 1 kubernetes基础

单体----分层----微服务

- 微服务
  - 服务注册，服务发现
  - 30~50 ，网状
  - 非静态，动态服务发现
  - 服务编排系统
  - 容器编排系统

容器编排

​	What is container orchestration?

Kubernetes集群节点

- Master
  - control plane
  - 管理层
- Node
  - worker
  - 真正执行任务的执行层

## 1.1 Kubernetes Master

需补画图

- API Server 接受客户端请求的操作入口
- scheduler watch API 资源的变动
- 两种状态
  - 保存在etcd中的用户期望的状态
  - 真正在集群运行的实际状态，controller要确认他是一致的

## 1.2 Kubernetes Node

- Kubernetes Node

  画图

  - kubelet
  - Docker

- PID,Mount,USER , UTS,Network, IPC

  - Pod 共享UTS,Network, IPC

## 1.3 Kubernetes Objects

Kubernetes Objects
画图

- 
- 对象式编程语言：
  - 以数据为中心，代码服务于数据
    - 数据：对象
    - 代码：方法
  - class: 类，
    - 属性， 方法
- k8s api : REST API (http/https)
  - resource -> object
  - method: GET,PUT,POST,DELETE,PATCH,...
- k8s cluster,容器编排系统
  - 核心任务：容器编排
  - 容器：应用程序
  - Pod Controller, Deployment
- 概念
  - **Pod**内的服务怎么被外界访问？
  - 崩了怎么办？
  - 控制器--资源类型--
  - 崩了更改新的Pod，IP发生改变
  - **service** service-ip,cluster-ip (DNS解析记录，iptables规则中)反代，拥有调度功能 ，客户端访问service
    - service是iptables规则，ipvs
  - pod-ip（虚拟IP)
  - **label,**label selector 标签选择器来确定哪些pod
  - **DNS**
    - 访问service名称，依赖DNS解析
  - node-ip(node节点网卡上)
  - nmt:
    - n的client:远程客户端
    - t的client:nginx
    - m的client:t上的程序
    - service---nginx(控制器) ---service ----tomcat(控制器) ---service----mysql(控制器)
      - 控制器就是运维的管理操作封装起来的定制的框架

## 1.4 Kubernetes Network

画图

- pod1 ---- service网络(中间调度查询的过程[想像成DNS服务])----pod2

# 2 部署k8s集群

## 2.1 部署要点

- 测试环境
  - 可以使用单Master节点，单etcd实例；
  - Node主机数量按需而定；
  - nfs或glusterfs等存储系统；
- 生产环境
  - 高可用etcd集群，建3、5或7个节点；
  - 高可用Master
    - kube-apiserver无状态，可多实例；
  - 多Node主机，数量越多，冗余能力越强；
  - ceph,

## 2.2 部署工具

- 常用的部署环境
  - IaaS公有云环境：AWS,GCE,Azure等
  - IaaS私有云或公有云环境：OpenStack和vsphere等；
  - Baremetal环境：物理服务器或独立的虚拟机等；
  - 云托管k8s
- 常用的部署工具
  - kubeadm
  - kops
  - kubespray
  - kontena pharos
  - ...
- 其它二次封装的常用发行版
  - Rancher
  - Tectonic
  - Openshift

## 2.3 部署方式

- 让基础程序运行
- 以容器运行
  - Master
    - kube-apiserver
    - kube-scheduler
    - kube-controller-manager
  - Node
    - kube-proxy
    - kubelet

## 2.4 kubeadm部署k8s测试集群

### 2.4.1 主机环境准备

1、测试环境说明

- 时间同步
- DNS解析，用hosts
- 关闭iptables或firewalld服务
- 禁用SElinux
- 禁用swap设备
- 若要使用ipvs模型的proxy，各节点还需要载入ipvs相关的各模块；

2、设定时间同步

```
启动chrony服务
systemctl status chronyd
rm -f /etc/localtime && ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
建议配置本地的时间服务器，修改节点的/etc/chrony.conf配置文侦探，并将时间服务器指向相应的主机即可，配置格式如下：
server CHRONY-SERVER-NAME-OR-IP iburst
```

3、主机名称解析

```bash
本测试环境用hosts文件进行各节点名称解析，文件内容如下：
192.168.20.51 master.shiwu.run master
192.168.20.61 node01.shiwu.run node01
192.168.20.62 node02.shiwu.run node02
```

4、关闭iptables和firewalld

```
systemctl stop firewalld
systemctl disable firewalld
```

5、关闭禁用SELinux

```
sed -i '/^SELINUX=/c SELINUX=disabled' /etc/sysconfig/selinux
setenforce 0
```

6、禁用swap

```
swapoff -a
```

7、启用ipvs模块

```
[root@master modules]# cat /etc/sysconfig/modules/ipvs.modules
#!/bin/bash
ipvs_mods_dir="/usr/lib/modules/$(uname -r)/kernel/net/netfilter/ipvs"
for mod in $(ls $ipvs_mods_dir |grep -o "^[^.]*"); do
/sbin/modinfo -F filename $mod &> /dev/null
if [ $? -eq 0 ]; then
/sbin/modprobe $mod
fi
done
```

### 2.4.2 安装程序包

在部署kubernetes时，要求master node和worker node上的版本保持一致，否则会出现版本不匹配导致奇怪的问题出现。本文将介绍如何在CentOS系统上，使用yum安装指定版本的Kubernetes。

我们需要安装指定版本的kubernetes。那么如何做呢？在进行yum安装时，可以使用下列的格式来进行安装：yum install -y kubelet-<version> kubectl-<version> kubeadm-<version>

```
vi /usr/lib/systemd/system/docker.service
ExecStartPost=/usr/sbin/iptables -P FORWARD ACCEPT
systemctl daemon-reload
systemctl start docker
systemctl enable docker

# sysctl -a |grep bridge
vim /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
sysctl -p
需安装指定版本的docker18.09.2
```

2、配置kubernetes源

```
安装
yum install kubeadm kubelet kubectl
需安装指定版本1.13.3
rpm -ql kubelet
rpm -ql kubadm

开始初始化集群
vim /etc/sysconfig/kubelet
"--fail-swap-on=false"
```

kubeadm命令

kubeadm config print init-defaults 打印默认配置

```
版本要改1.13.2
Service网络
Pod网络（需使用网络插件）
flannel:10.244.0.0/16 (默认地址）
calico:192.168.0.0/16
```

初始化方式一：传递参数

```
kubeadm init --kubernetes-version="v1.13.3" --pod-network-cidr="10.244.0.0/16" --image-repository="" --dry-run
kubeadm init --kubernetes-version="v1.13.3" --pod-network-cidr="10.244.0.0/16" --image-repository="" --ignore--preflight-errors=Swap --dry-run # 试运行
kubeadm config images list # 列出需要的镜像
kubeadm config images pull # 提前pull镜像
kubeadm init --kubernetes-version="v1.13.3" --pod-network-cidr="10.244.0.0/16" --image-repository="" --ignore--preflight-errors=Swap
初始化成功后操作添加配置
获取flannel清单
kubectl get nodes
github.com/coreos
kubectl apply -f
kubectl get pods -n kube-system
```

报错处理：

01- 安装kubeadm时报错提示？

![image-20200607171942943](C:\gitbooks\docs\devops\_media\kubernetes.v1.13\image-20200607171942943.png)

3、node节点上操作

- 安装yum install kubeadm kubelet
- master 上将kubernetes.repo 和/etc/sysconfig/kubeletscp 在node节点
- docker load -i 本地镜像包

常见的问题的是缺少CNI。我第一次测试的时候，遗漏了CNI网桥，因为RKE没有部署它。但是这个问题十分容易解决。