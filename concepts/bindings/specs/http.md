# HTTP绑定规范

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <name>
spec:
  type: bindings.http
  metadata:
  - name: url
    value: http://something.com
  - name: method
    value: GET
```

`url` 调用的HTTP url.  
`method` 调用请求方法.
