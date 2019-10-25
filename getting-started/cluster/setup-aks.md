
# 配置 Azure Kubernetes Service 集群

## 先决条件

- [Docker](https://docs.docker.com/install/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)

## 部署 Azure Kubernetes Service 集群

以下向导可帮助你安装一个Azure Kubernetes Service集群，如果你想了解详情，可参考[快速开始: 使用Azure命令行部署Azure Kubernetes Service (AKS) 集群](https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough)

1. 登陆到Azure
```bash
az login
```

2. 设置默认订阅
```bash
az account set -s [your_subscription_id]
```

3. 创建一个资源组

```bash
az group create --name [your_resource_group] --location [region]
```

4. 创建Azure Kubernetes Service集群
 使用`--kubernetes-version`参数指定1.13.x或新版本的Kubernetes 

> **注意:** [1.16.x版本的Kubernetes无法使用低于2.15.0版本的Helm](https://github.com/helm/helm/issues/6374#issuecomment-537185486)

```bash
az aks create --resource-group [your_resource_group] --name [your_aks_cluster_name] --node-count 2 --kubernetes-version 1.14.6 --enable-addons http_application_routing --enable-rbac --generate-ssh-keys
```

5. 获取Azure Kubernetes集群的访问凭证

```bash
az aks get-credentials -n [your_aks_cluster_name] -g [your_resource_group]
```

## (可选) 安装Helm以及部署Tiller

1. [安装Helm客户端](https://helm.sh/docs/using_helm/#installing-the-helm-client)

2. 创建Tiller服务账号
```bash
kubectl apply -f https://raw.githubusercontent.com/Azure/helm-charts/master/docs/prerequisities/helm-rbac-config.yaml
```

1. 运行以下命令安装Tiller到集群
```bash
helm init --service-account tiller --history-max 200
```

4. 确保Tiller以及部署和运行
```bash
kubectl get pods -n kube-system
```
