# Kafka绑定规范

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <name>
spec:
  type: bindings.kafka
  metadata:
  - name: topics # 可选. 由输入绑定使用
    value: topic1,topic2
  - name: brokers
    value: localhost:9092,localhost:9093
  - name: consumerGroup
    value: group1
  - name: publishTopic # 可选. 由输出绑定使用
    value: topic3
```

`topics` 用于输入绑定的主题列表，由逗号分隔。  
`brokers` 由逗号分隔的字符串，用于指定Kafka代理。  
`consumerGroup` 需要监听的消费者组。  
`publishTopic` 用于输出绑定发布的主题。  