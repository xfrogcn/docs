# Redis与Dapr

Dapr可通过两种方式来使用Redis:

1. 状态的持久化及恢复
2. 针对消息传递的一步发布/订阅模式

## 创建Redis存储

Dapr可以使用任何Redis实例 - 容器化的, 运行在本地开发机上的, 或者是由云服务托管的。如果你已经有了一个Redis存储，请移步到[配置](#configuration) 节点。

### 在Kubernetes集群中使用Helm创建Redis缓存

我们可以使用[Helm](https://helm.sh/)在Kubernetes集群中快速创建一个Reids实例。此方式需要[安装Helm](https://github.com/helm/helm#install)。

1. 在集群中安装Redis: `helm install stable/redis --name redis --set image.tag=5.0.5-debian-9-r104`。注意，我们通过镜像标签指定使用5以上的Redis版本，这是Dapr发布/订阅功能必须的。如果你只是使用Redis来作为状态存储（而不是发布/订阅），你可无需指定Redis镜像标签。
2. 运行 `kubectl get pods` 查看Redis是否已经运行。
3. 在[redis.yaml](#configuration) 文件中设置`redisHost`字段值为`redis-master:6379`。 例如:
    ```yaml
        metadata:
        - name: redisHost
          value: redis-master:6379
    ```
4. 接下来, 获取Redis的密码，依据系统不同，获取方式不同：
    - **Windows**: 运行 `kubectl get secret --namespace default redis -o jsonpath="{.data.redis-password}" > encoded.b64`, 获取到已编码的密码到encoded.b64文件中。 接下来, 运行 `certutil -decode encoded.b64 password.txt`, 解密密码到 `password.txt`文件中。最后，拷贝密码然后删除这两个文件。

    - **Linux/MacOS**: 运行 `kubectl get secret --namespace default redis -o jsonpath="{.data.redis-password}" | base64 --decode` 然后从输出中拷贝密码。

    在 [redis.yaml](#configuration) 文件中，设置`redisPassword`字段值为Redis密码。例如:
    ```yaml
        metadata:
        - name: redisPassword
          value: lhDOkwTlp0
    ```

### 创建由Azure托管的Redis缓存

**注意**: 此方法必须要有一个Azure订阅。

1. 打开 [此链接](https://ms.portal.azure.com/#create/Microsoft.Cache) 开始配置Redis缓存。如果需要请先登陆。
2. 填写必要的信息，并勾选**check the "Unblock port 6379" box**, 这允许你使用非SSL来持久化状态。
3. 点击"Create"开始部署Redis实列。
4. 一旦你的实例创建完成，你需要赋予你的访问密钥，导航到"Settings"下的"Access Keys"，拷贝你的密钥。
5. 运行 `kubectl get svc`，并拷贝redis-master的集群IP。
6. 最后, 你需要添加你的Key及Redis地址到`redis.yaml`文件，如果你只是运行一个示例，你可以添加到提供的`redis.yaml`文件。如果你是从头开始创建一个项目，你可参考[配置](#configuration)创建 `redis.yaml`。然后设置`redisHost`为 `[上一步骤获取到的集群IP]:6379` 以及 `redisPassword` 为第四步骤获取的访问密钥。**注意:** 在基于生产环境的应用，参考[机密管理](https://github.com/dapr/docs/blob/master/concepts/components/secrets.md)介绍来管理你的机密。

> **注意:** Dapr发布/订阅使用[Redis流](https://redis.io/topics/streams-intro)，此功能在Redis 5.0中引入, 目前Azure托管的Redis缓存尚不支持，因此，你只能使用Azure托管Redis缓存作为状态持久化存储。



### 创建Redis数据库的其他方式

- [AWS Redis](https://aws.amazon.com/redis/)
- [GCP Cloud MemoryStore](https://cloud.google.com/memorystore/)

## 配置

Dapr可使用Redis作为`statestore`组件 (针对状态持久化或恢复) 或 `messagebus` 组件 (针对发布/订阅)。 以下yaml文件演示如何定义它们。 **注意:** 以下yaml文件使用纯文本作为机密管理，在基于生产环境的应用中，请参考[机密管理](https://github.com/dapr/docs/blob/master/concepts/components/secrets.md) 说明来管理你的机密。

### 配置Redis作为状态持久化及恢复组件

创建一个名称为redis-state.yaml的文件, 粘贴以下内容:

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: statestore
spec:
  type: state.redis
  metadata:
  - name: redisHost
    value: <HOST>
  - name: redisPassword
    value: <PASSWORD>
```

### 配置Redis作为发布/订阅组件

创建一个名称为redis-pubsub.yaml文件, 粘贴以下内容:

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: messagebus
spec:
  type: pubsub.redis
  metadata:
  - name: redisHost
    value: <HOST>
  - name: redisPassword
    value: <PASSWORD>
```

## 应用配置

### Kubernetes

```
kubectl apply -f redis-state.yaml

kubectl apply -f redis-pubsub.yaml
```

### 独立方式

在默认情况下，Dapr命令行在你运行`dapr init`命令时会自动创建一个默认的Redis实例。不过，如果你想要配置不同的Redis实例，可在Dapr根目录下的components目录中，放入`redis.yaml`文件。
