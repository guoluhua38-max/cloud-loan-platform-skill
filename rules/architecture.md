# 架构概览与模块职责

## 基本信息

- **项目地址**：https://github.com/iLove9ou/cloud
- **语言**：Java 1.8
- **框架**：Spring Boot 2.0.3 + Spring Cloud Finchley.RC2
- **构建**：Maven 多模块

## 三模块架构

```
cloud/
├── discovery/  → Eureka 注册中心 (端口 8083)
├── gateway/    → API 网关 (端口 8090)
└── service/    → 业务服务 (端口 8088)
```

### discovery (服务注册中心)

- 纯 Eureka Server，端口 8083
- gateway 和 service 启动时向其注册
- 配置: `eureka.client.registerWithEureka=false, fetchRegistry=false`

### gateway (API 网关)

**职责**：接收外部请求，签名验证，Feign 转发到 service

| 包 | 职责 |
|---|------|
| controller/ | 4 个信贷 REST 接口 (XML 格式) |
| client/ | Feign 客户端，调用 service 层 |
| filter/ | Zuul 过滤器 + Servlet Filter (目前空实现) |
| aspect/ | @Sign 注解 + AOP 签名验证切面 |
| manager/ | BankCreditBusinessManager，构建响应报文 |
| mq/ | RabbitMQ Producer/Consumer |
| common/ | MQ 基础设施 (MQAccessBuilder, RetryCache, ThreadPoolConsumer) |
| format/ | XML 报文模型 (Document/Head/Body/Signature/Request/Response) |
| request/ & vo/ | 请求/响应 VO |
| enums/ | CharsetEnum, ErrorCode, SignTypeEnum |
| constants/ | 系统版本、白名单、线程数、重试间隔 |
| utils/ | SignUtil, DateUtil, XmlUtil, CovertUtil |

### service (业务服务)

**职责**：核心业务逻辑处理 + 数据持久化

| 包 | 职责 |
|---|------|
| controller/ | CreditBankLoanController (4 个接口) |
| service/ | CreditBankLoanService 接口 + Impl (大量 TODO) |
| manager/ | BankCreditHandler，构建业务响应 |
| dao/ | UserDao, RoleDao (MyBatis Mapper) |
| model/entity/ | UserDO |
| model/domain/ | 4 个网商银行领域模型 |
| model/request/ & response/ | 请求/响应模型 |
| format/ | XML 报文模型 (与 gateway 同构) |
| enums/ | BizErrorCode (12 个错误码) |

## 通信链路

```
外部(网商银行) --XML/RSA--> gateway --Feign--> service --MyBatis--> MySQL
                              |
                          RabbitMQ (异步)
```

## 数据库

- 库名: ftdm
- 连接: jdbc:mysql://localhost:3306/ftdm
- 账号: root/123456 (仅开发环境)
- ORM: MyBatis + MyBatis Generator

## 已知问题

1. ServiceImpl 中大量 TODO，业务逻辑未实现
2. SignUtil.createSign() 返回空字符串
3. application.yml 未配置 RabbitMQ 连接
4. GatewayFilter/ApiFilter 空实现
