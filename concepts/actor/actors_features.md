# Dapr角色运行时

Dapr角色运行时提供以下功能：

## 角色状态管理

 角色可通过状态管理能力可靠地保存状态。

 你可以通过Http/gRPC终结点与Dapr交互来进行状态管理。

### 保存角色状态

你可以通过一个键来保存某个角色类型下特定ID角色的状态，通过以下方式：

```
POST/PUT http://localhost:3500/v1.0/actors/<actorType>/<actorId>/state/<key>
```

键对应的值通过请求体传递。

```
{
  "key": "value"
}
```

如果你想在单一事务中保存多个项，可以调用：

```
POST/PUT http://localhost:3500/v1.0/actors/<actorType>/<actorId>/state
```

### 获取角色状态

一旦你保存了角色的状态，你可以通过以下方式获取到：

```
GET http://localhost:3500/v1.0/actors/<actorType>/<actorId>/state/<key>
```

### 移除角色状态

你可以通过以下方式永久性移除角色状态：

```
DELETE http://localhost:3500/v1.0/actors/<actorType>/<actorId>/state/<key>
```

参考 [dapr规范](../../reference/api/actors.md)了解更多信息。

## 角色定时器及唤醒器

角色可以通过注册定时器或唤醒器的方式来实现计划执行。

### 角色定时器

你可以注册一个回调角色来实现基于计时器的执行。

Dapr角色运行时确保回调方法遵循基于回合的并发保证。这意味着在回调执行完成之前，该角色的其他方法或定时/唤醒器回调不会被执行。

在回调完成之后，计时器的下一个周期开始。这意味着计时器在回调执行时将会停止，而在回调完成后继续计时。

Dapr角色运行时在回调完成时保存角色状态的变更，如果在保存状态时发生错误，角色对象将会被注销然后创建新的角色实例。

所有定时器将会在角色被垃圾回收销毁后停止，在这之后不会调用计时器回调。Dapr角色运行时不会保留关于停用之前正在运行的计时器的任何信息。角色在将来被重新激活时，有角色来注册任何需要的定时器。

你可以通过以下接口为一个角色创建一个定时器：

```
POST,PUT http://localhost:3500/v1.0/actors/<actorType>/<actorId>/timers/<name>
```

你可以通过请求体指定定时器时间以及回调。

要移除角色定时器可以调用：

```
DELETE http://localhost:3500/v1.0/actors/<actorType>/<actorId>/timers/<name>
```

参考[dapr规范](../../reference/api/actors.md)了解更多信息。

### 角色唤醒器

唤醒器是角色在指定时间触发持久化回调的机制，在功能上与定时器类似，但是与定时器不同的是，唤醒器会在任何情况下被触发，直到角色显示地注销或删除。另外，唤醒器是跨角色失活及故障转移的，因为Dapr角色运行时将会使用Dapr角色状态提供器保存角色唤醒器信息。

你可以通过以下请求为角色创建一个持久化的唤醒器：

```
POST,PUT http://localhost:3500/v1.0/actors/<actorType>/<actorId>/reminders/<name>
```

你可以通过请求体提供唤醒器保留时间及其周期。

#### 获取角色唤醒器

可通过以下方式获取角色唤醒器：

```
GET http://localhost:3500/v1.0/actors/<actorType>/<actorId>/reminders/<name>
```

#### 移除角色唤醒器

你可以通过以下方式移除唤醒器：

```
DELETE http://localhost:3500/v1.0/actors/<actorType>/<actorId>/reminders/<name>
```

参考 [dapr规范](../../reference/api/actors.md)了解更多信息。








