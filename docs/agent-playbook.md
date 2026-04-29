# Agent 执行手册

## 接收任务后的标准流程

### Phase 1: 需求理解

1. 确认用户意图分类：
   - **架构分析**：需要解释项目结构、模块关系 → 加载 `rules/architecture.md`
   - **代码生成**：需要新增功能 → 加载对应 rules + examples
   - **问题排查**：遇到错误 → 加载 `rules/error-codes.md`

### Phase 2: 知识加载

根据意图按需加载规则文件：
- 所有任务必读: `rules/architecture.md`
- 涉及接口开发: +`rules/gateway.md` +`rules/service.md` +`rules/protocol.md`
- 涉及消息队列: +`rules/mq.md`
- 涉及问题排查: +`rules/error-codes.md`
- 生成代码时: +`rules/code-style.md`

### Phase 3: 执行

1. 生成代码时，严格遵循 `rules/code-style.md` 的命名规范
2. 网关接口必须加 @Sign 注解
3. 业务逻辑遵循 Controller → Service → Manager 链路
4. 对外通信必须使用 XML + RSA 格式

### Phase 4: 验收清单

- [ ] 代码命名符合 code-style 规范
- [ ] 网关接口有 @Sign 注解
- [ ] Feign Client 与 Service 方法名一致
- [ ] 响应报文通过 Manager/Hanlder 构建
- [ ] 错误处理使用 BizErrorCode 枚举
- [ ] 未修改 Skill 安装目录内文件
