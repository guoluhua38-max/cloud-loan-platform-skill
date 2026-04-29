# 网关开发指南

## 新增接口步骤

1. 在 `gateway/controller/BankCreditLoanController.java` 添加接口方法
2. 加上 `@PostMapping` + `@Sign` 注解
3. 在 `gateway/client/CreditBankLoanClient.java` 添加 Feign 方法
4. 在 `service` 模块实现对应逻辑

## 代码模板

```java
// 1. Gateway Controller
@PostMapping(value = "/new_api", consumes = "application/xml", produces = MediaType.APPLICATION_XML_VALUE)
@Sign
public Document newApi(@RequestBody DocumentInput input) {
    logger.info("new_api request: " + new Gson().toJson(input));
    Document document = creditBankLoanClient.newApi(input);
    logger.info("new_api response: " + new Gson().toJson(document));
    return document;
}

// 2. Feign Client
@RequestMapping(value = "/newApi", method = RequestMethod.POST)
public Document newApi(DocumentInput document);
```

## 过滤器

- GatewayFilter: Servlet Filter (doFilter 空实现，待补充)
- ApiFilter: Zuul Filter (pre 类型，doFilter 空实现，待补充)

## MQ 配置

application.yml 中需补充 RabbitMQ 配置:
```yaml
spring:
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest
```
