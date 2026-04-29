# RabbitMQ 消息机制与重试策略

## 核心类

| 类 | 职责 |
|---|------|
| Producer | 发送消息到 RabbitMQ |
| Consumer | 消费消息，带重试机制 |
| MQAccessBuilder | 构建连接、通道、声明队列/交换机 |
| MQConfig | 队列和交换机名称常量 |
| ThreadPoolConsumer | 线程池消费 (线程数 5, 间隔 0ms) |
| RetryCache | 消息重试缓存，失败等待 1 分钟重试 |
| MessageSender | 消息发送器 (支持异步确认) |
| MessageConsumer | 消息消费器 (支持自动 ACK) |
| MessageProcess | 消息处理接口 |
| DetailRes | 消费结果封装 |

## 消息流程

```
Producer → Exchange → Queue → Consumer → MessageProcess.process()
                                         ↓ 失败
                                    RetryCache → 延迟重试
```

## 配置常量 (Constants)

- THREAD_COUNT = 5
- INTERVAL_MILS = 0
- ONE_SECOND = 1000ms
- ONE_MINUTE = 60000ms
- RETRY_TIME_INTERVAL = 60000ms
- VALID_TIME = 60000ms

## 新增消费者步骤

1. 创建 MessageProcess 实现类
2. 配置队列/交换机 (MQConfig)
3. 使用 MQAccessBuilder 构建消费者
4. 用 ThreadPoolConsumer 包装启动
