# 开始

Dapr是一种可移植的，事件驱动的运行时，企业开发者通过它可快速地构建弹性的、有状态或无状态的微服务应用，这些应用可运行在云或边缘，并支持语言与框架的多样性。

## 核心概念

* **构建块** 用于实现分布式系统能力的组件集合。如发布/订阅，状态管理，资源绑定以及分布式跟踪等。

* **组件**  构建块API的封装实现。例如要实现状态构建块可能需要包括Redis，Azure Storage, Azure Cosmos DB, 以及 AWS DynamoDB。许多组件都是插件化的，所以它可以从一个实现切换到另一种。

要了解更新, 请参考 [Dapr 概念](../concepts/README.md).

## 设置开发环境

Dapr可在本地或Kubernetes上运行。我们推荐从本地安装Dapr开始来了解Daper的核心概念以及熟悉Dapr命令行。按照以下的说明进行操作 [配置本地Dapr](./environment-setup.md#prerequisites) or [在Kubernetes中](./environment-setup.md#installing-dapr-on-a-kubernetes-cluster).

## 接下来

1. 一旦安装好Dapr，继续[Hello World 示例](https://github.com/dapr/samples/tree/master/1.hello-world).
2. 探索更多的 [示例](https://github.com/dapr/samples) ，熟悉更高级的概念, 如服务调用，发布/订阅，以及状态管理。
3. 跟随 [如何...指南](../howto)理解Dapr是怎样解决特定问题的，例如创建一个[速率限制应用](../howto/control-concurrency).
