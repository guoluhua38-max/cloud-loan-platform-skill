# 代码风格与命名规范

## 包结构

- controller: REST 接口，方法名与 URL 路径一致
- service: 业务接口 + Impl 实现
- manager: 业务处理器 (构建响应报文)
- dao: MyBatis Mapper 接口
- model/entity: 数据库实体 (后缀 DO)
- model/domain: 领域模型
- model/request: 请求模型
- model/response: 响应模型
- format: XML 报文模型
- enums: 枚举类
- constants: 常量类
- utils: 工具类
- common: 通用组件

## 命名约定

- Controller 方法名: 与 Feign Client 方法名一致
- Service 接口: 业务动词 + 名词 (如 applyNotify)
- ServiceImpl: 加 Impl 后缀
- DAO: 实体名 + Dao
- Entity: 实体名 + DO
- Request: MybankCreditLoan + 动作 + Request
- Response: MybankCreditLoan + 动作 + Response
- Domain: MybankCooperation + 动作 + Domain

## 注解使用

- @RestController (Controller)
- @Service (ServiceImpl)
- @Component (Manager, Handler)
- @Mapper (DAO)
- @Autowired (依赖注入)
- @Sign (需要签名验证的接口)
- @Aspect + @Before (签名切面)

## 依赖版本

所有版本由父 POM 统一管理，子模块不要单独指定 Spring Boot/Cloud 版本。
