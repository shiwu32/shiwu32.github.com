# Envoy基础

- 什么是Envoy
- Envoy组件拓扑

![image-20200607164134929](C:\gitbooks\docs\devops\_media\istio\image-20200607164134929.png)

- Envoy xDS核心术语
  - xDS API常用术语
    - 集群（Cluster）: 集群是Envoy连接到的一组逻辑上相似的端点；在v2中，RDS通过路由指向集群，CDS提供集群配置，而Envoy过EDS发现集群成员，即端点；
    - 下游（Downstream）：下游主机连接到Envoy，发送请求并接收响应，它们是Envoy的客户端；
    - 上游（Upstream）：上游主机接收来自Envoy的连接和请求并返回响应，它们是Envoy代理的后端服务器；
    - 端点（Endpoit）：端点即上游主机，是一个或多个集群的成员，可通过EDS发现；
    - 侦听器（Listener）：侦听器是能够由下游客户端连接的命名网络位置，例如单口或unix域套接字等；
    - 位置（Locality）：上游端点运行的区域拓扑，包括地域、区域和子区域等；
    - 管理服务器（Management Server）：实现v2 API的服务器，它支持复制和分片，并且能够在不同的物理机器上实现针对不同xDS API的API服务；
    - 地域（Region）：区域所属地理位置；
    - 区域（Zone）：AWS中的可用区（AZ）或GCP中的区域等；
    - 子区域：Envoy实例或端点运行的区域内的位置，用于支持区域内的多个负载均衡目标；
    - xDS: CDS、EDS、HDS、LDS、RLS(Rate Limit)、RDS、SDS、VHDS和RTDS等API的统称；
- Envoy的部署类型
- Envoy核心配置组件
  - Listener
  - Filter
  - Cluster

![image-20200607164204668](C:\gitbooks\docs\devops\_media\istio\image-20200607164204668.png)

- Envoy线程模型和边接处理机制
  - Envoy线程模型
    - Envoy使用单进程/多线程的架构模型，一个主线程（Main thread）负责实现各类管理任务，而一些工作线程（Worker threads）则负责执行监听、过滤和转发等代理服务器的核心功能
      - 主线程：负责Envoy程序的启动和关闭、xDS API调用处理（包括DNS、健康状态检测和集群管理等）、运行时配置、统计数据刷新、管理接口维护和其它线程管理（集号和热重启等）等，相关的所有事件均以异步非阻塞模式完成；
      - 工作线程：默认情况下

- 应用服务与网络剥离
- 说明Envoy的部署类型
- Service Containers
- Ingress
- Front proxy 前端代理

- 反向代理
  - 监控套接字 Listener
  - LDS
- 后端发现机制
  - DNS
  - 一个域名对应多个A记录
  - EDS
- Cluster
  - CDS
- SDS 生成证书和私钥
  - 动态轮替证书
- 基本配置
  - Listener
  - Cluster
  - 启动时的第一步连接Cluster,生成
- 总结