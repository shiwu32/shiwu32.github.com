本节话题

- 构建Envoy及相关的工具程序
- 基于容器镜运行Envoy实现
- 配置生成器脚本及配置模板生成配置文件
- 校验配置文件
- **Listener及配置示例** **#重点**
- **Cluster及配置示例** **#重点**
- TCP Proxy简单应用
- HTTP connection manager简单应用
- HTTP L7由配置
- Admin Interface

- Enovy部署类型回顾
  图

- 下载官方预制好的docker镜像

- Envoy的启动步骤概述

  图

  - 常用的构建方法（三种）
  - 提供Bootstrap配置文件
  - 基于Bootstrap配置文件启动Envoy实例

- 构建及安装Envoy概述
  图

- Building Envoy with Bazel
  图

- 配置生成器
  图

- 预制的Envoy镜像
  图

- Envoy的几个工具程序
  图

- 启动Envoy
  图

- Listener简易静态配置
  图

- 基于镜像启动第一个Envoy实例
  图

- 实验测试

```bash
操作
git clone https://github.com/envoyproxy/envoy.git
cd envoy/

docker pull envoyproxy/envoy-alpine:v1.11.1
mkdir envoy.echo
cd
vi envoy.yaml
static_resources:
  listener:
    - name: listener_0
    address:
      socket_address:
        address: 0.0.0.0
        porrt_value: 15001
    filter_chains:
    - filters:
      - name: envoy.echo
vi Dockerfile
FROM envoyproxy/envoy-alpine:v1.11.1
ADD envoy.yaml /etc/envoy/

docker build . -t envoy-echo:v0.1
docker container run --name echo --rm envoy-echo:v0.1


docker exec
  - netstat -tnl
nc 容器IP 15001
  - hi~
监控套接字
官方文档看详细配置
docker container run -v /root/envoy_vol:/etc/envoy/
```

007---2020-06-07

#

Listener

- envoy.echo



- ingress, service-to-service
- egress,service-to-service

基本的envoy配置模型

纯静态手动定义

## Cluster简易静态配置

图

