
# 安装Minikube集群

## 先决条件

- [Docker](https://docs.docker.com/install/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)

> 注意: 对于Windows，确保在BIOS中开启了虚拟化以及[安装了Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)

## 开启Minikube集群

1. (可选) 设置默认的虚拟驱动

```bash
minikube config set vm-driver [driver_name]
```

> 注意: 参考[驱动](https://minikube.sigs.k8s.io/docs/reference/drivers/) 了解详细可支持的驱动以及如何安装插件

2. 启动集群
通过`--kubernetes-version`参数指定1.13.x或新版本的Kubernetes

> **注意:** [1.16.x版本的Kubernetes无法使用低于2.15.0版本的Helm](https://github.com/helm/helm/issues/6374#issuecomment-537185486)

```bash
minikube start --cpus=4 --memory=4096 --kubernetes-version=1.14.6 --extra-config=apiserver.authorization-mode=RBAC
```

3. 开启仪表盘以及ingress附加组件

```bash
# Enable dashboard
minikube addons enable dashboard

# Enable ingress
minikube addons enable ingress
```

## (可选) 安装Helm并部署Tiller

1. [安装Helm客户端](https://helm.sh/docs/using_helm/#installing-the-helm-client)

2. 创建Tiller服务账号

```bash
kubectl create serviceaccount -n kube-system tiller
kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
```

3. 安装Tiller

```bash
helm init --service-account tiller --history-max 200
```

4. 确保Tiller部署完成并运行

```bash
kubectl get pods -n kube-system
```

### 故障排查

1. 如果Tiller没有正常运行，可通过查看`tiller-deploy`部署的日志来分析问题:

```bash
kubectl describe deployment tiller-deploy --namespace kube-system
```

2. `kubectl get svc`命令结果中没有显示负载均衡的外部IP

在Minikube中，`kubectl get svc`命令结果EXTERNAL-IP栏显示`<pending>`状态。遇到此问题，你可以运行`minikube service [service_name]`命令开启你的服务而无需外部IP地址


```
$ kubectl get svc
NAME                        TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)            AGE
...
calculator-front-end        LoadBalancer   10.103.98.37     <pending>     80:30534/TCP       25h
calculator-front-end-dapr   ClusterIP      10.107.128.226   <none>        80/TCP,50001/TCP   25h
...

$ minikube service calculator-front-end
|-----------|----------------------|-------------|---------------------------|
| NAMESPACE |         NAME         | TARGET PORT |            URL            |
|-----------|----------------------|-------------|---------------------------|
| default   | calculator-front-end |             | http://192.168.64.7:30534 |
|-----------|----------------------|-------------|---------------------------|
🎉  Opening kubernetes service  default/calculator-front-end in default browser...
```
