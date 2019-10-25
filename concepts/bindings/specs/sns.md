# AWS SNS Binding Spec

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <name>
spec:
  type: bindings.aws.sns
  metadata:
  - name: region
    value: us-west-2
  - name: accessKey
    value: *****************
  - name: secretKey
    value: *****************
  - name: topicArn
    value: mytopic
```

`region` AWS区域  
`accessKey` AWS 访问密钥  
`secretKey` AWS 机密密钥  
`topicArn` SNS主题名称