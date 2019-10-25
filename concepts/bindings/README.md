# 绑定

使用绑定，可以让你的应用被外部系统的事件触发，或调用外部系统服务。绑定允许按需或事件驱动的计算场景，Dapr绑定从以下方面帮助开发者：

* 消除消息系统连接轮询的复杂性（如队列、消息总线等）。
* 专注于业务逻辑，而无需关注系统间交互的逻辑。
* 保持代码不受SDK或库的影响
* 处理重试与故障恢复
* 在运行时对绑定进行切换
* 允许可移植应用，它可根据特定环境通过配置设定绑定，而无需变更代码。

绑定时独立于Dapr运行时开发的。你可以从[此处](https://github.com/dapr/components-contrib/tree/master/bindings)查看分发绑定。 

## 支持的绑定及规范

每一个绑定都有自己唯一的属性，单击以下连接查看组件YAML

| 名称  | 输入绑定 | 输出绑定 | 状态
| ------------- | -------------- | -------------  | ------------- |
| [Kafka](./specs/kafka.md) | V | V | 试验 |
| [RabbitMQ](./specs/rabbitmq.md) | V  | V | 试验 |
| [AWS SQS](./specs/sqs.md) | V | V | 试验 |
| [AWS SNS](./specs/sns.md) |  | V | 试验 |
| [Azure EventHubs](./specs/eventhubs.md) | V | V | 试验 |
| [Azure CosmosDB](./specs/cosmosdb.md) | | V | 试验 |
| [GCP Storage Bucket](./specs/gcpbucket.md)  | | V | 试验 |
| [HTTP](./specs/http.md) |  | V | 试验 |
| [MQTT](./specs/mqtt.md) | V | V | 试验 |
| [Redis](./specs/redis.md) |  | V | 试验 |
| [AWS DynamoDB](./specs/dynamodb.md) | | V | 试验 |
| [AWS S3](./specs/s3.md) | | V | 试验 |
| [Azure Blob Storage](./specs/blobstorage.md) | | V | 试验 |
| [Azure Service Bus Queues](./specs/servicebusqueues.md) | V | V | 试验 |
| [GCP Cloud Pub/Sub](./specs/gcppubsub.md) | V | V | 试验 |
| [Kubernetes Events](./specs/kubernetes.md) | V |  | 试验 |

## 输入绑定

输入绑定允许你的应用启用触发器，当外部资源事件出发时，通知你的应用。通知请求中包含可选的负载及元数据。

为了从输入绑定中接收事件：

1. 定义组件的YAML，它描述了绑定的类型以及相关元数据（如连接信息等）。
2. 监听输入事件的HTTP终结点，或者通过gRPC协议库来获取输入事件。

阅读[如何...](../../howto)章节开始输入绑定。

## 输出绑定

输出绑定允许用户调用外部资源。
可选的负载及元数据可同时随调用请求发送。

为了调用一个输出绑定：

1. 定义一个组件YAML，它描述了绑定的类型及其元数据（如连接信息等）。
2. 使用HTTP终结点或gRPC方法来调用绑定。

 阅读[如何...](../../howto) 章节开始输出绑定。
