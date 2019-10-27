# 发布/订阅消息

通过Dapr开发者可设计使用消息代理的发布/订阅模式应用，这样事件的消费者与生产者彼此解耦，发送与接收消息之间的通信通过命名空间隔离（通常为主题）。

这允许事件生产者发送消息到未运行的消费者，消费者通过订阅主题的方式来接收消息。

Dapr提供至少一次的消息传递保证，以及可与多个消息代理集成。
这些实现时插件化的，与Dapr运行时独立[components-contrib](https://github.com/dapr/components-contrib/tree/master/pubsub).

## 发布/订阅 API

有关发布/订阅的API，可在[规范](../../reference/api/pubsub.md)中查看

## 行为及保证

Dapr确保至少一次的消息传递。这意味着，当一个应用发布一条消息到主题时，可以假设任何订阅者都可以至少一次成功处理该消息, 也就是订阅者终结点返回200 HTTP应答，或者gRPC没有错误返回。

处理消费者组以及组内多个实例的责任都由Dapr来完成。

### 应用ID

Dapr具有`id`的概念，它在Kubernetes中通过`dapr.io/id`注释来表示，在Dapr命令行中通过`app-id`参数指定。Dapr中每个应用都必须具有一个ID。

当具有相同应用ID的多个实例订阅一个主题时，Dapr将确保传递的消息只会到其中的一个实例。如果两个具有不同ID的应用同时订阅一个主题，每一个应用中的至少一个实例将会收到消息。

## 云事件

Dapr遵循[云事件0.3规范](https://github.com/cloudevents/spec/tree/v0.3) ，将发送到主题的任何负载都由一个云事件邮封封装。

以下云事件规范中的字段已被Dapr实现：

* `id`
* `source`
* `specversion`
* `type`
* `datacontenttype` (可选)
