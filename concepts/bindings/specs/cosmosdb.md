# Azure CosmosDB绑定规范

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <name>
spec:
  type: bindings.azure.cosmosdb
  metadata:
  - name: url
    value: https://******.documents.azure.com:443/
  - name: masterKey
    value: *****
  - name: database
    value: db
  - name: collection
    value: collection
  - name: partitionKey
    value: message
```

`url` CosmosDB连接地址.  
`masterKey` CosmosDB账户主Key.  
`database` CosmosDB数据库名称.  
`collection` 数据库中的集合名称.  
`partitionKey` 负载中有效分区名称.