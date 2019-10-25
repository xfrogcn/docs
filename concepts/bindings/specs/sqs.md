# AWS SQS 绑定规范

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <name>
spec:
  type: bindings.aws.sqs
  metadata:
  - name: region
    value: us-west-2
  - name: accessKey
    value: *****************
  - name: secretKey
    value: *****************
  - name: queueName
    value: items
```

`region` AWS区域  
`accessKey` AWS 访问密钥  
`secretKey` AWS 机密密钥  
`queueName` SQS 队列名