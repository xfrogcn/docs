# 服务调用 

支持Dapr的应用可通过已知终结点使用http或gRPC消息通讯。

![服务调用图示](../../images/service-invocation.png)

1. 服务A发起一个到服务B的http/gRPC调用，调用将会到达本地的Dapr sidecar。
2. Dapr查找服务B的位置，并将消息发送到服务B的Dapr sidecar。
3. 服务B的sidecar将请求转发给服务B，服务B处理业务逻辑。
4. 服务B发送应答到服务A，应答将会达到服务B sidecar。
5. Dapr转发应答到服务A的sidecar。
6. 服务A受到应答。

作为上述所有应用程序的一个示例，假设我们有以下示例中描述的应用程序集合，其中python应用程序调用Node.js应用程序:https://github.com/dapr/samples/blob/master/2.hello-kubernetes/README.md

在这个场景中，python应用为服务A，Node.js应用为服务B。

下面在这个示例的上下文中再次描述了项目1-6:

1. 假设Node.js应用程序的Dapr应用程序id为“nodeapp”，如示例中所示。python应用调用Node.js应用的`neworder`方法，可通过post `http://localhost:3500/v1.0/invoke/nodeapp/method/neworder`, 它首先达到python应用的本地Dapr sidecar。
2. Dapr找到Node.js的位置，并转发请求到Node.js应用的sidecar。
3. Node.js应用的sidecar转发请求到Node.js应用，Node.js应用处理其业务逻辑，就像示例中描述的一样，它将记录消息请求并持久化订单ID到Redis（未展示在上述图示中）。

步骤4-5与上述列表一致。

更多信息请参考：

- [服务调用规范](../../reference/api/service_invocation.md)
- 有关服务调用的[如何...]()教程