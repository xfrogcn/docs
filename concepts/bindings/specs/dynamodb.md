# AWS DynamoDB绑定规范

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <name>
spec:
  type: bindings.aws.dynamodb
  metadata:
  - name: region
    value: us-west-2
  - name: accessKey
    value: *****************
  - name: secretKey
    value: *****************
  - name: table
    value: items
```

`region` AWS区域.  
`accessKey` AWS访问密钥.  
`secretKey`  AWS机密密钥.  
`table` DynamoDB表名称.
