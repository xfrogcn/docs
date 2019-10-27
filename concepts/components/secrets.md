# 机密

组件可通过`spec.metadata`节来引用机密。<br>
为了引用一个机密，你需要通过`auth.secretStore`字段设置保存机密的机密存储名称。<br><br>
当运行在Kubernetes中时，如果`auth.secretStore`是空的，将会假设使用Kubernetes机密存储。

## 示例

使用纯文本:

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: statestore
spec:
  type: state.redis
  metadata:
  - name: redisHost
    value: localhost:6379
  - name: redisPassword
    value: MyPassword
```

使用Kubernetes机密:

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: statestore
spec:
  type: state.redis
  metadata:
  - name: redisHost
    value: localhost:6379
  - name: redisPassword
    secretKeyRef:
    	name: redis-secret
        key:  redis-password
auth:
  secretStore: kubernetes
```

以上示例告诉Dapr使用`Kubernetes`机密存储，指定机密名称未`redis-secret`，并使用机密中的`redis-password`字段值作为组件中`redisPassword`字段的值。

### 创建一个机密并在组件中引用

以下示例演示如何创建一个由Kubernetes机密来存储的，供`事件总线绑定`使用的机密。

首先，创建一个Kubernetes机密：

```
kubectl create secret generic eventhubs-secret --from-literal=connectionString=*********
```

接下来, 在绑定中引用机密：

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: eventhubs
spec:
  type: bindings.azure.eventhubs
  metadata:
  - name: connectionString
    secretKeyRef:
      name: eventhubs-secret
      key: connectionString
```

最后，将组件应用到Kubernetes集群：

```
kubectl apply -f ./eventhubs.yaml
```

完成!
