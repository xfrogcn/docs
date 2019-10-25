# Azure Blob Storage绑定规范

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <name>
spec:
  type: bindings.azure.blobstorage
  metadata:
  - name: storageAccount
    value: myStorageAccountName
  - name: storageAccessKey
    value: ***********
  - name: container
    value: container1
```

`storageAccount` Blob Storage账户名.  
`storageAccessKey` Blob Storage访问密钥.  
`container` 写入的Blob Storage容器名称.
