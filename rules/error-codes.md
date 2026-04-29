# 错误码体系与排查指南

## BizErrorCode 枚举

| 错误码 | 枚举名 | 含义 | 常见原因 |
|--------|--------|------|----------|
| 0000 | SUCCESS | 处理成功 | - |
| 0001 | UNDEFINE_MSG_SCOPE | 未定义的消息域 | function 字段不匹配 |
| 0002 | UNDEFINE_PARAMS | 必填域缺失 | 请求缺少必填字段 |
| 0003 | NOT_RECOGNIZE_KEY_ITEM | 无法识别关键域 | 关键字段格式错误 |
| 0004 | ITEM_FORMAT_ERROR | 参数格式不符合要求 | 字段值与规范不符 |
| 0007 | INVLID_SIGN | 签名无效 | RSA 签名验证失败 |
| 0008 | PRIVATE_KEY_NOT_EXSITS | 密钥不存在 | 未配置 RSA 密钥对 |
| 0011 | PARAMS_ERROR | 参数错误 | 业务参数校验失败 |
| 0021 | TRANS_NO_IS_EMPTY | 外部流水号为空 | 缺少 reqMsgId |
| 0114 | CALL_GATEWAY_HANDLE_ERROR | 网关处理返回失败 | Feign 调用 service 失败 |
| 9000 | UNKNOW_SYSTEM_ERROR | 未知系统异常 | 未捕获的运行时异常 |
| 9020 | RESPONSE_ORDER_ERROR | 报文时序错误 | 请求时间戳不合规 |

## 排查路径

### 签名失败 (0007)
1. 检查 SignAspect 是否正确拦截
2. 检查 SignUtil.createSign() 实现 (⚠️ 当前返回空串)
3. 确认 RSA 密钥对配置

### 网关调用失败 (0114)
1. 检查 service 是否注册到 Eureka
2. 检查 Feign 服务名 "service-test" 是否匹配
3. 检查 service 端口 8088 是否可达

### 系统异常 (9000)
1. 查看 gateway/service 日志
2. 检查数据库连接
3. 检查 MQ 连接
