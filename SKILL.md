---
name: cloud-loan-platform
description: "网商银行信贷对接微服务平台开发辅助技能。基于 Spring Cloud 构建贷款业务系统，涵盖 Eureka 服务注册、Zuul+Feign 网关、MyBatis+MySQL 业务层，支持信贷初审/终审/放款/还款全流程。当用户需要理解、分析、修改、扩展该贷款平台，或需要生成 Controller/Service/DAO/MQ 代码时使用。"
license: MIT
tags: ["java", "spring-cloud", "microservices", "loan", "credit", "banking", "eureka", "zuul", "feign", "mybatis"]
---

# Cloud Loan Platform Skill

网商银行信贷对接微服务平台的 AI Agent 开发辅助技能。

Agent 执行手册: [docs/agent-playbook.md](docs/agent-playbook.md)

## 🚨 重要开发原则 (CRITICAL DEVELOPER RULES)

1. **代码生成位置**：生成的业务代码必须放在用户项目目录下，**禁止修改 Skill 安装目录内的文件**。
2. **网关-服务分离**：所有对外接口必须通过 gateway 模块暴露，service 模块只通过 Feign 被调用。
3. **报文格式**：对外通信统一使用 XML + RSA 签名，参考 `rules/protocol.md`。

## 📐 规则指南 (Rules)

按需加载具体规则文件：

- [rules/architecture.md](rules/architecture.md) - **必读** 架构概览与模块职责
- [rules/protocol.md](rules/protocol.md) - XML 报文协议与签名规范
- [rules/gateway.md](rules/gateway.md) - 网关开发：路由、过滤器、Feign 调用
- [rules/service.md](rules/service.md) - 业务服务开发：Controller → Service → DAO 全链路
- [rules/mq.md](rules/mq.md) - RabbitMQ 消息机制与重试策略
- [rules/error-codes.md](rules/error-codes.md) - 错误码体系与排查指南
- [rules/code-style.md](rules/code-style.md) - 代码风格与命名规范

## 🎯 Agent 快速路由

| 用户意图 | 参考规则 | 示例代码 |
|---------|---------|---------|
| 新增信贷接口 | `rules/gateway.md` + `rules/service.md` | [examples/add_loan_api.md](examples/add_loan_api.md) |
| 新增 MQ 消息 | `rules/mq.md` | [examples/add_mq_consumer.md](examples/add_mq_consumer.md) |
| 排查签名失败 | `rules/protocol.md` + `rules/error-codes.md` | - |
| 新增数据库表 | `rules/service.md` + `rules/code-style.md` | [examples/add_dao_entity.md](examples/add_dao_entity.md) |
| 升级 Spring Cloud | `rules/architecture.md` | - |

## 📖 经典示例 (Examples)

- [examples/add_loan_api.md](examples/add_loan_api.md) - 新增信贷接口完整流程
- [examples/add_mq_consumer.md](examples/add_mq_consumer.md) - 新增 MQ 消费者
- [examples/add_dao_entity.md](examples/add_dao_entity.md) - 新增数据表 + DAO + Mapper

## 🛠️ 项目快速诊断

```bash
# 检查服务是否注册成功
curl http://localhost:8083/eureka/apps

# 测试网关接口
curl -X POST http://localhost:8090/apply_notify \
  -H "Content-Type: application/xml" \
  -d '<DocumentInput>...</DocumentInput>'

# 检查业务服务健康
curl http://localhost:8088/actuator/health
```

## 🚀 快速开始

1. 阅读 [rules/architecture.md](rules/architecture.md) 了解项目结构
2. 根据需求选择对应的 rules 文件
3. 参照 examples 目录下的示例生成代码
4. 使用 error-codes.md 排查问题

## 📂 项目来源

基于 [iLove9ou/cloud](https://github.com/iLove9ou/cloud) 开源项目构建。
