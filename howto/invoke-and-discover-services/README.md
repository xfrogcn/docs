# 调用远程服务

在许多环境中存在多个服务之间相互调用的问题，开发者一般会发现以下问题：
In many environments with multiple services that need to communicate with each other, developers often ask themselves the following questions:

* 我如何发现并调用不同的服务？
* 我如何处理重试及瞬态错误？
* 我如何使用分布式跟踪来查看调用图谱？

Dapr允许开发人员通过提供一个终结点来克服这些挑战，该终结点作为反向代理和内置的服务发现，同时可利用内置的分布式跟踪和错误处理。

## 1. 选择你的服务ID

Dapr允许你为你的应用分配一个全局唯一的ID<br>这个ID封装了应用程序的状态，而不管该应用有多少个实例。

### 使用Dapr命令行设置应用ID

在独立模式下，通过`--app-id`参数来设置：

`dapr run --app-id cart --app-port 5000 python app.py`

### 在Kubernetes集群中设置应用ID

在Kubernetes中，可以通过`dapr.io/id`注释来设置应用ID：

<pre>
apiVersion: apps/v1
kind: Deployment
metadata:
  name: python-app
  labels:
    app: python-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: python-app
  template:
    metadata:
      labels:
        app: python-app
      annotations:
        dapr.io/enabled: "true"
        <b>dapr.io/id: "cart"</b>
        dapr.io/port: "5000"
...
</pre>

## 在代码中调用服务

Dapr使用sidecar、去中心化的架构，要使用Dapr调用一个应用，你可以使用集群/环境中任何Dapr实例的`invoke`终结点。

在sidecar编程模型中，一般推荐应用与它自己的Dapr实例交互。Dapr实例之间彼此发现并通讯。

*注意: 以下为Python购物车示例，它可以由其他任何编程语言实现。*

```python
from flask import Flask
app = Flask(__name__)

@app.route('/add', methods=['POST'])
def add():
    return "Added!"

if __name__ == '__main__':
    app.run()
```

Python通过`/add`终结点导出`add()`方法。

### 通过curl调用

```
curl http://localhost:3500/v1.0/invoke/cart/add -X POST
```

由于终结点是'POST'方法，所以需要在curl命令中加上`-X POST`参数

以下调用一个'GET'终结点：

```
curl http://localhost:3500/v1.0/invoke/cart/add
```

调用'DELETE'终结点:

```
curl http://localhost:3500/v1.0/invoke/cart/add -X DELETE
```

Dapr将通过HTTP应答提返回目标服务的应答。

## 概览

以上示例如何直接调用本地或Kubernetes中的不同服务。Dapr输出的指标及跟踪信息允许你可视化服务间的调用图谱，记录错误日志以及可选地记录消息体。

有关更多信息，请参考[此连接](../../best-practices/troubleshooting/tracing.md).
