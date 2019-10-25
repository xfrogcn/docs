# Kubernetes Events绑定规范

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <name>
spec:
  type: bindings.kubernetes
  metadata:
  - name: namespace
    value: default
```

`namespace` 从中读取事件的Kubernetes命名空间. 默认为 `default`.
