# RabbitMQ绑定规范

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <name>
spec:
  type: bindings.rabbitmq
  metadata:
  - name: queueName
    value: queue1
  - name: host
    value: amqp://guest:guest@localhost:5672
  - name: durable
    value: true
  - name: deleteWhenUnused
    value: false
```

`queueName` RabbitMQ队列名  
`host` RabbitMQ地址  
`durable` RabbitMQ持久化消息到存储中  
`deleteWhenUnused` 启用或禁止自动删除  