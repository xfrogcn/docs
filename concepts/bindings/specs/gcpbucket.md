# GCP Storage绑定规范

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <name>
spec:
  type: bindings.gcp.storage
  metadata:
  - name: bucket
    value: mybucket
  - name: type
    value: service_account
  - name: project_id
    value: project_111
  - name: private_key_id
    value: *************
  - name: client_email
    value: name@domain.com
  - name: client_id
    value: '1111111111111111'
  - name: auth_uri
    value: https://accounts.google.com/o/oauth2/auth
  - name: token_uri
    value: https://oauth2.googleapis.com/token
  - name: auth_provider_x509_cert_url
    value: https://www.googleapis.com/oauth2/v1/certs
  - name: client_x509_cert_url
    value: https://www.googleapis.com/robot/v1/metadata/x509/<project-name>.iam.gserviceaccount.com
  - name: private_key
    value: PRIVATE KEY
```

`bucket` 桶名称.  
`type` GCP凭证类型.  
`project_id` GCP项目id.  
`private_key_id` GCP 私有密钥id.  
`client_email` GCP 客户端电邮.  
`client_id` GCP客户端id.  
`auth_uri` Google账户oauth终结点.  
`token_uri` Google账户令牌uri.  
`auth_provider_x509_cert_url` GCP 凭证证书url.  
`client_x509_cert_url` GCP凭证项目x509证书url.  
`private_key` GCP 凭证私有Key.  
