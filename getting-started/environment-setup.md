# 环境设置

Dapr可以独立模式或Kubernetes模式运行，以独立方式运行可让你在本地开发环境进行开发然后部署到目标环境。例如，你可以以Dapr独立方式开发应用，然后部署它们到Kubernetes集群。

## 目录

 - [先决条件](#先决条件)
 - [安装Dapr命令行](#安装Dapr命令行)
 - [以独立模式安装Dapr](#以独立模式安装Dapr)
 - [在Kubernetes集群中安装Dapr](#在Kubernetes集群中安装Dapr)

## 先决条件

* 安装 [Docker](https://docs.docker.com/install/)

> 对于Windows用户，请确保 `Docker Desktop For Windows` 使用Linux容器。

## 安装Dapr命令行

### 使用脚本安装最新版本

**Windows**

安装最新windows Dapr命令行到 `c:\dapr`, 并将此路径添加到PATH环境变量。

```powershell
powershell -Command "iwr -useb https://raw.githubusercontent.com/dapr/cli/master/install/install.ps1 | iex"
```

**Linux**

安装最新版本linux Dapr命令行到 `/usr/local/bin`

```bash
wget -q https://raw.githubusercontent.com/dapr/cli/master/install/install.sh -O - | /bin/bash
```

**MacOS**

安装最新版本 darwin Dapr 命令行到 `/usr/local/bin`

```bash
curl -fsSL https://raw.githubusercontent.com/dapr/cli/master/install/install.sh | /bin/bash
```

### 从二进制发布版本

每一个Dapr命令行发布包含多种操作系统和体系架构版本。这些二进制版本可手动下载安装。

1. 下载 [Dapr 命令行](https://github.com/dapr/cli/releases)
2. 解压 (如 dapr_linux_amd64.tar.gz, dapr_windows_amd64.zip)
3. 移动其到目标位置.
   * Linux/MacOS路径为 - `/usr/local/bin`
   * 针对Windows，创建路径并添加到系统PATH环境变量。例如创建路径 `c:\dapr` ，然后添加此路径到环境path

## 以独立模式安装Dapr

### 使用命令行方式安装Dapr运行时

在命令提示行中运行 `dapr init` 命令来安装Dapr运行时

> 对于Linux用户, 如果你是使用sudo方式运行的docker命令，你需要使用 "**sudo dapr init**"

> 对于Windows用户, 请确保你是以管理员方式启动的命令行终端

> **注意:** 参考 [Dapr 命令行](https://github.com/dapr/cli) 了解更多命令行使用细节

```bash
$ dapr init
⌛  Making the jump to hyperspace...
Downloading binaries and setting up components
✅  Success! Dapr is up and running
```

要检查Dapr是否安装成功，从命令行运行 `docker ps` 命令，检查 `daprio/dapr:latest` 以及 `redis` 容器已正常运行。

### 安装特定的运行时版本

你可以安装或更新Dapr运行时到一个特定版本，通过使用 `dapr init --runtime-version`命令，你可以在 [Dapr 发布](https://github.com/dapr/dapr/releases)中找到版本列表。

```bash
# 按照 v0.1.0 运行时
$ dapr init --runtime-version 0.1.0

# 检查命令行和运行时的版本
$ dapr --version
cli version: v0.1.0
runtime version: v0.1.0
```

### 在独立模式中卸载Dapr运行时

释放Dapr容器.

```bash
$ dapr uninstall
```

## 在Kubernetes集群中安装Dapr

在Kubernetes中你可以通过命令行或Helm方式进行安装

### 设置集群

* [设置 Minikube 集群](./cluster/setup-minikube.md)
* [设置 Azure Kubernetes Service 集群](./cluster/setup-aks.md)

### 使用Dapr命令行方式

你可以通过命令行部署Dapr到Kubernetes

#### 在Kubernetes中安装Dapr Install Dapr to Kubernetes

```bash
$ dapr init --kubernetes
ℹ️  注意: 此种方式建议在测试的时候使用，对于生成环境，请使用Helm方式

⌛  Making the jump to hyperspace...
✅  Deploying the Dapr Operator to your cluster...
✅  Success! Dapr has been installed. To verify, run 'kubectl get pods -w' in your terminal
```

Dapr命令行安装Dapr到 `default` 命名空间

#### 在Kubernetes中卸载Dapr

```bash
$ dapr uninstall --kubernetes
```

### 使用Helm (高级)

你可以通过Helm chart部署Dapr到Kubernetes集群

#### 安装Dapr到Kubernetes

1. 确保Kubernetes集群中已经初始化了Helm

2. 添加Azure Container Registry Helm仓库

```bash
helm repo add dapr https://daprio.azurecr.io/helm/v1/repo
helm repo update
```

3. 安装Dapr chart 到集群的 `dapr-system` 命名空间

```bash
helm install dapr/dapr --name dapr --namespace dapr-system
```

#### 安装验证

一旦chart安装完成，确认`dapr-system` 命名空间下的dapr-operator, dapr-placement 以及 dapr-sidecar-injector Pod 运行正常

```bash
$ kubectl get pods -n dapr-system -w

NAME                                     READY     STATUS    RESTARTS   AGE
dapr-operator-7bd6cbf5bf-xglsr           1/1       Running   0          40s
dapr-placement-7f8f76778f-6vhl2          1/1       Running   0          40s
dapr-sidecar-injector-8555576b6f-29cqm   1/1       Running   0          40s
```

#### 从Kubernetes集群卸载Dapr

```bash
helm del --purge -n dapr
```

> **注意:** 参考 [此处](https://github.com/dapr/dapr/blob/master/charts/dapr/README.md) 了解 Dapr helm charts详情
