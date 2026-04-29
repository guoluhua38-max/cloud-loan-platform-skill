---
name: cloud-loan-platform
description: "网商银行信贷对接微服务平台开发辅助技能。基于 Spring Cloud 构建贷款业务系统，包含服务注册(Eureka)、API网关(Zuul+Feign)、业务服务(MyBatis+MySQL)三大模块，支持信贷初审/终审/放款/还款全流程。当用户需要理解、分析、修改、扩展该贷款平台项目时使用。"
license: MIT
tags: ["java", "spring-cloud", "microservices", "loan", "credit", "banking"]
---

# cloud-loan-platform

网商银行信贷对接微服务平台的开发辅助技能。基于 Spring Cloud 构建贷款业务系统，包含服务注册、API 网关、业务服务三大模块。

## 技能能力

1. **项目架构分析**：解读微服务模块划分、依赖关系、通信链路
2. **代码解读与导航**：快速定位关键类、方法、配置，解释业务逻辑
3. **开发指导**：指导如何新增接口、扩展模块、配置部署
4. **问题排查**：根据错误码、日志定位问题根因
5. **代码生成**：按照项目既有风格生成新的 Controller/Service/DAO/MQ 等代码

## 项目概览

### 基本信息

- **项目地址**：https://github.com/iLove9ou/cloud
- **语言**：Java 1.8
- **框架**：Spring Boot 2.0.3 + Spring Cloud Finchley.RC2
- **构建工具**：Maven
- **项目状态**：早期开发阶段，核心接口框架已搭建，业务逻辑多为 TODO

### 模块结构

```
cloud/
├── pom.xml                    # 父 POM，统一管理依赖版本
├── discovery/                 # 服务注册中心 (Eureka Server, 端口 8083)
├── gateway/                   # API 网关 (Zuul + Feign, 端口 8090)
│   ├── controller/            # 对外 REST 接口 (4个信贷接口)
│   ├── client/                # Feign 客户端调用 service 层
│   ├── filter/                # Zuul 过滤器 + Servlet Filter
│   ├── aspect/                # @Sign 签名验证切面
│   ├── manager/               # 响应报文构建 (BankCreditBusinessManager)
│   ├── mq/                    # RabbitMQ 生产者/消费者
│   ├── common/                # MQ 基础设施（重试、线程池消费等）
│   ├── format/                # XML 报文格式模型 (Document/Head/Body/Signature)
│   ├── request/ & vo/         # 请求/响应 VO
│   ├── enums/                 # 枚举（字符集、错误码、签名类型）
│   ├── constants/             # 常量
│   └── utils/                 # 工具类（签名、日期、XML转换）
└── service/                   # 业务服务层 (端口 8088, MySQL ftdm 库)
    ├── controller/            # REST 接口
    ├── service/               # 业务逻辑（ServiceImpl）
    ├── manager/               # 业务处理器 (BankCreditHandler)
    ├── dao/                   # MyBatis Mapper 接口
    ├── model/
    │   ├── entity/            # 数据库实体 (UserDO)
    │   ├── domain/            # 领域模型
    │   ├── request/           # 请求模型
    │   └── response/          # 响应模型
    ├── format/                # XML 报文格式模型（与 gateway 同构）
    ├── enums/                 # 枚举 + BizErrorCode 错误码
    ├── constants/             # 常量
    └── utils/                 # 工具类
```

### 核心业务流程

网商银行与银行机构之间的信贷业务对接，基于 XML 报文通信：

```
网商银行                    API 网关(gateway)              业务服务(service)
  │                              │                              │
  │── apply_notify ────────────>│── Feign ──> applyNotify ────>│
  │   (初审通知)                 │                              │
  │<── approve_upload ──────────│<── Feign ── approveUpload ───│
  │   (初审数据上传)              │                              │
  │── approveack_notify ───────>│── Feign ──> finalNotify ────>│
  │   (终审通知)                 │                              │
  │<── approveack_confirm ──────│<── Feign ── finalConfirm ────│
  │   (终审确认)                 │                              │
```

### 技术栈

| 组件 | 版本 | 用途 |
|------|------|------|
| Spring Boot | 2.0.3.RELEASE | 基础框架 |
| Spring Cloud | Finchley.RC2 | 微服务治理 |
| Eureka | - | 服务注册与发现 |
| Zuul | 1.4.4 | API 网关路由 |
| Feign | 1.4.4 | 声明式服务调用 |
| MyBatis | 1.3.2 | ORM 框架 |
| MySQL | 5.0.2 driver | 数据存储 (ftdm 库) |
| RabbitMQ | 2.0.2 | 消息队列 |

### 错误码体系 (BizErrorCode)

| 错误码 | 含义 |
|--------|------|
| 0000 | 处理成功 |
| 0001 | 未定义的消息域 |
| 0002 | 必填域缺失 |
| 0003 | 无法识别关键域 |
| 0007 | 签名无效 |
| 0011 | 参数错误 |
| 0114 | 网关处理返回失败 |
| 9000 | 未知系统异常 |
| 9020 | 报文时序错误 |

### 通信协议

- **报文格式**：XML（consumes/produces = application/xml）
- **签名方式**：RSA
- **字符集**：UTF-8
- **报文结构**：
  ```xml
  <Document>
    <Request/Response>
      <Head>version, signType, respTime, inputCharset, appId, function, reqMsgId, reverse</Head>
      <Body>applyNo, requestId, resultInfo(resultCode, resultMsg, retry)</Body>
    </Request/Response>
    <Signature>signature</Signature>
  </Document>
  ```

## 执行流程

当用户提出关于该项目的需求时，按以下步骤执行：

### 步骤 1：理解需求

- 架构分析 / 代码解读 → 直接回答
- 新增功能 / 修改代码 → 进入步骤 2
- 问题排查 → 进入步骤 3

### 步骤 2：开发指导或代码生成

1. **新增接口**：按照 BankCreditLoanController → Service → ServiceImpl → Manager → Domain/Request/Response
2. **新增 Feign 调用**：在 gateway/client 下新增 FeignClient 接口
3. **新增 MQ**：参考 mq/ 下的 Producer/Consumer 模式
4. **新增 DAO**：参考 UserDao，配合 MyBatis mapper XML
5. **配置修改**：注意 application.yml 中的端口和 Eureka 地址

### 步骤 3：问题排查

1. 根据错误码查阅 BizErrorCode 枚举
2. 检查签名验证流程（SignAspect → SignUtil）
3. 检查 Feign 调用链路是否正确（服务名 "service-test"）
4. 检查 MQ 配置和重试机制
5. 检查数据库连接和 MyBatis 映射

## 注意事项

1. **项目处于早期阶段**：ServiceImpl 中大量 TODO，核心业务逻辑尚未实现
2. **依赖版本较老**：Spring Boot 2.0.3 / Spring Cloud Finchley.RC2，升级需注意兼容性
3. **签名未完整实现**：SignUtil.createSign() 返回空字符串，RSA 签名逻辑待补充
4. **MQ 配置不完整**：application.yml 中未配置 RabbitMQ 连接信息
5. **网关过滤器空实现**：GatewayFilter 和 ApiFilter 的 doFilter 方法为空
6. **数据库弱密码**：默认 root/123456，仅开发环境使用
