# Azure EventHubs绑定规范

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <name>
spec:
  type: bindings.azure.eventhubs
  metadata:
  - name: connectionString
    value: https://<address>
  - name: consumerGroup  # 可选
    value: group1
  - name: messageAge
    value: 5s         # 可选. Golang duration规则
```

`connectionString` 连接字符串.  
`consumerGroup` 监听的EventHubs消费者组.  
`messageAge` 允许接收的消息，不可在据此时长之前
