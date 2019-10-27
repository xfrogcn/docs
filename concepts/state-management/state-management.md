# 状态管理 

通过Dapr可非常简单地实现在你所选择的存储中保存键值对数据。

![状态管理](../../images/state_management.png)

## 状态管理API

Dapr通过简单的状态API引入可靠的状态管理。开发者可使用这些API接收、保存、以及删除状态键。

Dapr的数据存储时插件化的，Dapr，Dapr提供开箱即用的[Redis](https://redis.io
) 。它也允许你选择其他的数据存储，如[Azure CosmosDB](https://azure.microsoft.com/Databases/Cosmos_DB
), [AWS DynamoDB](https://aws.amazon.com/DynamoDB
), [GCP Cloud Spanner](https://cloud.google.com/spanner
) 以及 [Cassandra](http://cassandra.apache.org/).

查看Dapr API规范以便详细了解[状态管理API](../../reference/api/state.md))

> **注意:** Dapr将会在状态键的前面加上当前Dapr实例/sidecar的ID前缀,这允许多个Dapr实例共享相同的状态存储。

## 状态存储行为
Dapr允许开发者在状态操作请求上指定请求处理的一些元数据，例如并发需求、一致性需求以及重试策略等。

默认情况下，你的应用应该假设数据存储时**最终一致性**的，以及使用最后一次写入并发模式。另一方面，如果你在请求中附加了操作元数据，Dapr将这些元数据一同传递给状态存储，以期望状态存储能够按希望处理请求。

并不是所有的存储都是一样的，为了确保你应用的可移植性，你需要了解存储的能力并确保你的代码能够在适配不同的存储能力。

以下表格中汇总了数据存储所实现的能力。

存储 | 强一致性写 | 强一致性读 | 实体标签(ETag)|
----|----|----|----
Cosmos DB | 是 | 是 | 是
Redis | 是 | 是 | 是
Redis (集群化的)| 是 | 否 | 是

## 并发
Dapr通过ETags支持乐观的并发控制（OCC）。当收到一个状态请求时，Dapr总是附加**ETag**属性到状态请求。当用户代码尝试更新或删除一个状态时，它将通过与**If-Match**头指定的ETag预期匹配。写入操作只有在当提供的ETag与数据库中的ETag匹配时才会成功。

Dapr采用乐观并发控制（OCC），是因为一般情况下更新冲突较少,因为数据通常都被业务上下文自然分隔。但是，如果你的应用选择ETags，请求可能因为ETag不匹配而失败，这时推荐使用[重试策略](#Retry-Policies) 来处理使用Etags所产生的冲突。

如果你的应用在写入请求中忽略了ETag，Dapr将跳过ETag检查。这本质上是**最后写入覆盖**模式，而ETags为**首次写入**模式。

> **注意:** 针对无法提供原生支持ETag的存储，期望Dapr状态存储实现模拟Etag的实现，并遵循Dapr状态管理API规范。因为Dapr状态存储是在数据存储之上实现的，通过存储提供的并发控制机制来模拟ETag应该非常简单。

## 一致性
Dapr同时支持**强一致性**及**最终一致性**, 最终一致性为默认行为。

当强一致性被使用，Dapr等待所有的副本（或指定的法定数副本）确认。当使用最终一致性时，Dapr将在底层数据存储接收到请求时直接返回，即使此时只有一个副本。

## 重试策略
Dapr允许你在任何写请求中附加重试策略，一个重试策略包含 **retryInterval**、 **retryPattern** 以及 **retryThreshold**。Dapr在给定的时间间隔内不断重试请求，直到达到指定的阈值。你可以选择使用**线性(linear)**重试策略或**指数(exponential)** (补偿) 策略. 当**指数(exponential)**模式被使用, 重试间隔将在每次重试时倍数增加。

## 批量操作

Dapr支持两种类型的批量操作 - **bulk** 或者 **multi**，你可以将多个相同类型的操作分配到一个批次，Dapr将分拆多个请求到底层存储。换句话说，bulk操作不是事务性的，而另一方面，你可以将不同的类型分配到一个multi操作，此时将以原子事务性方式处理。

## 直接查询状态存储

Dapr存储或获取状态数据不会做任何转化，你可以通过底层数据存储直接查询或聚合状态，例如，查询分配给应用ID为"myApp"的所有状态键：

```bash
KEYS "myApp*"
```

> **注意:** 查看 [如何查询Redis存储](../../howto/query-state-store/query-redis-store.md) 以了解更多细节

> 
### 查询角色状态

如果数据存储支持SQL查询，你可以使用SQL查询角色状态，例如：

```sql
SELECT * FROM StateTable WHERE Id='<dapr-id>-<actor-type>-<actor-id>-<key>'
```

您还可以跨actor实例执行聚合查询，从而避免actor框架常见的基于回合的并发性限制, 例如，要计算所有温度计角色的平均温度，请使用：

```sql
SELECT AVG(value) FROM StateTable WHERE Id LIKE '<dapr-id>-<thermometer>-*-temperature'
```

> **注意:** 由于直接查询不是由Dapr运行时调用的，所以它不受Dapr并发控制限制。您看到的是提交数据的快照，这对于跨多个参与者的只读查询是可以接受的，但是写入应该通过参与者实例完成。

## 参考
* [规范: Dapr状态管理规范](../../reference/api/state.md)
* [规范: Dapr角色规范](../../reference/api/actors.md)
* [如何: 设置Azure Cosmos DB存储](../../howto/setup-state-store/setup-azure-cosmosdb.md)
* [如何: 查询Azure Cosmos DB存储](../../howto/query-state-store/query-cosmosdb-store.md) 
* [如何: 设置Redis存储](../../howto/setup-state-store/setup-redis.md)
* [如何: 查询Redis存储](../../howto/query-state-store/query-redis-store.md) 
