# Azure Service Bus Queues绑定规范

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <name>
spec:
  type: bindings.azure.servicebusqueues
  metadata:
  - name: connectionString
    value: sb://************
  - name: queueName
    value: queue1
```

`connectionString`Service Bus连接字符串.  
`queueName`  Service Bus队列名称.
