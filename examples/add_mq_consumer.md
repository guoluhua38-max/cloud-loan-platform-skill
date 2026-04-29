# 示例：新增 MQ 消费者

## 需求

新增一个消费信贷审批结果的队列监听器。

## 步骤

### 1. 定义队列和交换机

```java
// gateway/src/main/java/com/cloud/gateway/mq/MQConfig.java
public static final String LOAN_APPROVAL_QUEUE = "loan_approval_queue";
public static final String LOAN_APPROVAL_EXCHANGE = "loan_approval_exchange";
```

### 2. 创建 MessageProcess 实现

```java
// gateway/src/main/java/com/cloud/gateway/common/LoanApprovalProcess.java
@Component
public class LoanApprovalProcess implements MessageProcess {
    @Override
    public DetailRes process(String message) {
        try {
            // 解析审批结果
            JSONObject json = JSON.parseObject(message);
            String applyNo = json.getString("applyNo");
            String result = json.getString("result");
            
            // 处理业务逻辑...
            
            return new DetailRes(true, "处理成功: " + applyNo);
        } catch (Exception e) {
            return new DetailRes(false, "处理失败: " + e.getMessage());
        }
    }
}
```

### 3. 注册消费者

```java
// 在 GatewayApplication 启动后或 @PostConstruct 中
MQAccessBuilder builder = new MQAccessBuilder("localhost", 5672, "guest", "guest");
ThreadPoolConsumer consumer = new ThreadPoolConsumer(
    builder, THREAD_COUNT, INTERVAL_MILS,
    new LoanApprovalProcess(), LOAN_APPROVAL_QUEUE
);
consumer.start();
```
```

