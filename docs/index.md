# 服务模版说明文档

## 服务说明
本示例以nginx为例子介绍如何利用[KubeBlocks](https://kubeblocks.io/)扩展服务并跟计算巢托管版集成，本示例对应的Git仓库地址：[nginx-kubeblocks-demo](https://github.com/aliyun-computenest/nginx-kubeblocks-demo)。

根据该服务模板构建的服务默认包含三种套餐：

| 套餐名  | vCPU与内存      | 
|--------|----------------|
| 低配版 | 1vCPU 1GiB |
| 基础版 | 2vCPU 2GiB  |
| 高配版 | 2vCPU 4GiB |


## 前提条件
1. 本示例需要提前准备ack集群，需要到[容器服务控制台](https://cs.console.aliyun.com/) 提前创建。
2. 安装KubeBlocks
```shell
# 参考 https://cn.kubeblocks.io/docs/preview/user-docs/installation/install-with-helm/install-kubeblocks-with-helm

kubectl create -f https://github.com/apecloud/kubeblocks/releases/download/v0.8.1/kubeblocks_crds.yaml

helm repo add kubeblocks https://apecloud.github.io/helm-charts

helm repo update

helm install kubeblocks kubeblocks/kubeblocks --namespace kb-system --create-namespace
```
3. 在helm路径执行
```shell
# 接入参考 https://cn.kubeblocks.io/docs/preview/developer-docs/integration/how-to-add-an-add-on
helm install nginx ./nginx
```

## 服务构建计费说明

测试本服务构建无需任何费用，创建服务实例涉及的费用参考服务实例计费说明。

## RAM账号所需权限

本服务需要对ECS、VPC等资源进行访问和创建操作，若您使用RAM用户创建服务实例，需要在创建服务实例前，对使用的RAM用户的账号添加相应资源的权限。添加RAM权限的详细操作，请参见[为RAM用户授权](https://help.aliyun.com/document_detail/121945.html)。所需权限如下表所示：

| 权限策略名称                          | 备注                     |
|---------------------------------|------------------------|
| AliyunCSFullAccess             | 管理容器服务服务（CS）的权限       |
| AliyunROSFullAccess             | 管理资源编排服务（ROS）的权限       |
| AliyunComputeNestUserFullAccess | 管理计算巢服务（ComputeNest）的用户侧权限 |
| AliyunComputeNestSupplierFullAccess | 管理计算巢服务（ComputeNest）的服务商侧权限 |

## 服务实例计费说明

测试本服务在计算巢上的费用主要涉及：

- 导入的ACK集群的费用
- 在ACK集群新建的磁盘、网络等费用


## 服务详细说明

使用 [ALIYUN::CS::ClusterApplication](https://rosnext.console.aliyun.com/resourceType/ALIYUN::CS::ClusterApplication?entityType=Resource) 资源将 YAML 配置部署到 Kubernetes 集群中，并且将每个租户映射到专属的命名空间（Namespace）。

本服务利用 KubeBlocks 提供的 Cluster 自定义资源定义（CRD）来创建nginx服务实例。关于 nginx 的具体配置信息，请参见 Helm 配置文件，位置在 helm/nginx 目录。