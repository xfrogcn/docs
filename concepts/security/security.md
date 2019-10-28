# 安全

## Dapr与应用的通讯

Dapr sidecar通过**localhost**与应用绑定运行。Dapr假设它在与应用相同的安全域下运行，所以，在Dapr sidecar与应用之间的通讯没有认证，授权或者加密。

## Dapr与Dapr间的通讯

Dapr是作为应用内部组件通讯来设计的，Dapr假设这些应用组件都在相同的信任边界中，因此，Dapr默认不提供Dapr间的通讯的保护。

不过，在多租户环境下，Dapr sidecar之间的安全通讯就很需要了。Dapr路线图中准备提供TLS以及其它认证、授权、加密方法的支持。

另一种可选方案是使用服务网格技术，如[Istio]( https://istio.io/) 来提供应用组件之间的安全通讯，Dapr可与流行的服务网格一起很好地工作。

默认情况下，Dapr支持全来源的跨域资源共享（CORS），你可以配置Dapr运行时只允许特定的来源。

## 网络安全

你可以采用常见的网络安全技术，如网络安全组（NSGs），高安全区（DMZs）以及防火墙来为你的网络提供多层保护。

例如，除非配置为与外部绑定目标交互，否则Dapr sidecar不会打开因特网连接。大多数绑定只实现出站连接，可以将防火墙规则设计为只允许通过指定端口进行出站连接。

## 绑定安全

绑定目标的身份验证由绑定的配置文件配置,通常，你应该配置最小的访问权限，例如，如果你只需要从绑定目标读取，你应该配置绑定使用一个具有只读权限的账号。

## 状态存储安全

Dapr不会转换应用的状态数据，这意味着Dapr不会尝试加密/解密状态数据。不过，你的应用可以选择采取自己的加密/解密方法，这样状态数据对Dapr仍然是不透明的。

Dapr使用配置的认证方法来认证底层的状态存储, 许多状态存储实现使用官方客户端库，这些客户端库通常使用与服务器之间的安全通信通道。

## 管理安全

当在Kubernetes中部署，你可以使用标准的[Kubernetes RBAC(基于角色的访问控制)]( https://kubernetes.io/docs/reference/access-authn-authz/rbac/)来控制管理活动。

当部署在Azure Kubernetes Service (AKS)，你可以使用[Azure Active Directory (AD)服务原则]( https://docs.microsoft.com/en-us/azure/active-directory/develop/app-objects-and-service-principals) 来控制管理活动及资源管理。

## 组件机密

Dapr组件使用Dapr内建的机密管理能力来管理机密，请参考[机密主题](../components/secrets.md)了解更多信息。
