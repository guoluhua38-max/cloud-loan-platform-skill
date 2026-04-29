# 示例：新增信贷接口

## 需求

新增一个"放款通知"接口 `/lending_notify`，网商银行 → 网关 → 业务服务。

## 步骤

### 1. Gateway Controller

```java
// gateway/src/main/java/com/cloud/gateway/controller/BankCreditLoanController.java

@PostMapping(value = "/lending_notify", consumes = "application/xml", produces = MediaType.APPLICATION_XML_VALUE)
@Sign
public Document lending_notify(@RequestBody DocumentInput input) {
    logger.info("lending_notify request: " + new Gson().toJson(input));
    Document document = creditBankLoanClient.lendingNotify(input);
    logger.info("lending_notify response: " + new Gson().toJson(document));
    return document;
}
```

### 2. Feign Client

```java
// gateway/src/main/java/com/cloud/gateway/client/CreditBankLoanClient.java

@RequestMapping(value = "/lendingNotify", method = RequestMethod.POST)
public Document lendingNotify(DocumentInput document);
```

### 3. Service Controller

```java
// service/src/main/java/com/cloud/service/controller/CreditBankLoanController.java

@RequestMapping(value = "/lendingNotify")
public Document lendingNotify(@RequestBody DocumentInput document) {
    return creditBankLoanService.lendingNotify(document);
}
```

### 4. Service 接口 + 实现

```java
// Service 接口
Document lendingNotify(DocumentInput document);

// ServiceImpl
@Override
public Document lendingNotify(DocumentInput document) {
    // 解析请求，执行放款逻辑
    String appId = "app";
    String funcKey = "lendingNotify";
    String sign = "sign";
    ResultInfo info = new ResultInfo();
    info.setResultCode(BizErrorCode.SUCCESS.getCode());
    info.setResultMsg(BizErrorCode.SUCCESS.getMessage());
    return handler.getDocuemnt(appId, funcKey, sign, info);
}
```

### 5. 请求/响应模型 (可选)

如需扩展字段，创建：
- `model/request/MybankCreditLoanLendingNotifyRequest.java`
- `model/response/MybankCreditLoanLendingNotifyResponse.java`
- `model/domain/MybankCooperationLendingNotifyDomain.java`
```

