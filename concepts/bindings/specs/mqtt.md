# MQTT Binding Spec

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <name>
spec:
  type: bindings.mqtt
  metadata:
  - name: url
    value: mqtt[s]://[username][:password]@host.domain[:port]
  - name: topic
    value: topic1
```

`url` MQTT代理url.   
`topic` 监听的主题.  
