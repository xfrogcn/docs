# Dapr概念

此目录包含多个Dapr概念，此文档的目的是扩展你对[Dapr 规范](../reference/api/README.md)的认识。

## 核心概念

* [**绑定**](./bindings/README.md)

  绑定提供外部云/内部服务或系统间的双向链接。Dapr允许你使用标准Dapr绑定API调用外部服务，并且可让其他已连接服务通过事件方式触发你的应用。

* [**构建块**](./architecture/building_blocks.md)

  一个构建块是由一个或多个Dapr组件构成的单一目的的API接口。Dapr由一系列构建块构成，同时可扩展添加新的构建块。

* **组件**
  
  Dapr采用模块化设计，它的功能由一系列*组件*来实现，如[发布/订阅](./publish-subscribe-messaging/README.md) 以及 [机密](./components/secrets.md)。许多组件都支持插件模式，所以你可以使用你的自定义实现来替换默认实现。

* [**分布式跟踪**](./distributed-tracing/README.md)

  分布式跟踪收集和聚合事务中的跟踪事件，它允许你跨多个服务跟踪整个调用链。Dapr与[OpenTelemetry](https://opentelemetry.io/)集成，提供分布式跟踪及指标收集。

* [**发布/订阅消息**](./publish-subscribe-messaging/README.md)
  
  订阅/发布是一种低耦合的消息模式，发送器（或发布者）发布一条消息到主题，然后由订阅者订阅。Dapr原生支持发布/订阅模式。

* [**机密**](./components/secrets.md)

  Dapr中，机密表示未授权用户无法获取的私密信息。Dapr提供简单的机密API以及与Azure Key Vault或Kubernetes机密存储集成来存储机密。

* [**状态**](./state-management/state-management.md)

  应用状态是指应用程序希望在单个会话之外需要保存的任何状态。Dapr允许使用插件化的键值对状态API的状态存储。

## 角色

* [概览](./actor/actor_overview.md)
* [功能](./actor/actors_features.md)

## 扩展性

* [组件分发](https://github.com/dapr/components-contrib)
