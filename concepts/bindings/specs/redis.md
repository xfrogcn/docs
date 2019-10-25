# Redis Binding Spec

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <name>
spec:
  type: bindings.redis
  metadata:
  - name: redisHost
    value: <address>:6379
  - name: redisPassword
    value: **************
```

`redisHost` Redis地址.  
`redisPassword` Redis密码.
