# cloud-loan-platform-skill

网商银行信贷对接微服务平台的 AI Agent 技能。基于 Spring Cloud 构建贷款业务系统。

## 安装

```bash
npx skills add guoluhua38-max/cloud-loan-platform-skill
```

## 技能能力

- 项目架构分析
- 代码解读与导航
- 开发指导（新增接口/MQ/DAO）
- 问题排查（错误码+排查路径）
- 代码生成（遵循项目代码风格）

## 目录结构

```
├── SKILL.md              # 主文档
├── rules/                # 开发规则
│   ├── architecture.md   # 架构概览
│   ├── protocol.md       # XML 报文协议
│   ├── gateway.md        # 网关开发
│   ├── service.md        # 业务服务开发
│   ├── mq.md             # RabbitMQ 消息
│   ├── error-codes.md    # 错误码体系
│   └── code-style.md     # 代码风格
├── examples/             # 示例
│   ├── add_loan_api.md   # 新增信贷接口
│   ├── add_mq_consumer.md # 新增 MQ 消费者
│   └── add_dao_entity.md # 新增数据表
└── docs/
    └── agent-playbook.md # Agent 执行手册
```

## 项目来源

基于 [iLove9ou/cloud](https://github.com/iLove9ou/cloud) 开源项目构建。

## License

MIT
