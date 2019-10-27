# 分布式跟踪

Dapr使用OpenTelemetry（之前成为OpenCensus）来实现分布式跟踪及指标收集。OpenTelemetry支持多种后端包括[Azure Monitor](https://azure.microsoft.com/en-us/services/monitor/), [Datadog](https://www.datadoghq.com), [Instana](https://www.instana.com), [Jaeger](https://www.jaegertracing.io/), [SignalFX](https://www.signalfx.com/), [Stackdriver](https://cloud.google.com/stackdriver), [Zipkin](https://zipkin.io) 等等。


![跟踪](../../images/tracing.png)

# 跟踪设计

Dapr通过Dapr sidecar注入HTTP/gRPC中间件，中间件拦截所有Dapr及应用流量并自动注入关联ID来跟踪分布式事务。这种设计有以下优点：

* 无需代码注入，所有的流量自动被跟踪（根据配置的跟踪级别）
* 跨微服务的一致的跟踪行为，跟踪是由托管的Dapr sidecar配置，所有它可以保持跨团队甚至跨语言的一致性。
* 可配置及可扩展的。通过OpenTelemetry, Dapr跟踪可配置使用流行的后端，包括自定义后端。

# 关联ID

对于HTTP请求, Dapr注入**X-Correlation-ID**请求头，对于gRPC调用，Dapr注入**X-Correlation-ID** 字段，在**header** 元数据中，当一个请求没有关联ID时，Dapr将自动生成一个，否则，它将继续在调用链中传递关联ID。

# 配置

Dapr跟踪通过配置文件配置（本地模式）或一个Kubernetes配置对象来配置（Kubernetes模式）。例如，定义Zipkin导出，可通过定义以下配置对象：

```yaml
apiVersion: dapr.io/v1alpha1
kind: Configuration
metadata:
  name: zipkin
spec:
  tracing:
    enabled: true
    exporterType: zipkin
    exporterAddress: "http://zipkin.default.svc.cluster.local:9411/api/v2/spans"
    expandParams: true
    includeBody: true
```

请阅读 [参考](#references) 节点，了解更多在本地环境或Kubernetes环境配置跟踪的信息。

# 参考
* [如何: 设置分布式跟踪](../../howto/diagnose-with-tracing/readme.md)

