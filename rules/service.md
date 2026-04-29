# 业务服务开发指南

## 新增业务接口完整链路

```
Controller → Service(接口) → ServiceImpl → Manager → Domain/Request/Response
    ↓
Dao → Mapper XML → MySQL
```

## 新增 DAO 步骤

1. 创建 Entity 类 (参考 UserDO)
2. 创建 Dao 接口 (加 @Mapper 注解)
3. 创建 Mapper XML (在 resources/mapper/ 下)
4. 在 application.yml 中确认 mapperLocations 配置

## 代码模板

```java
// Controller
@RestController
public class NewController {
    @Autowired
    private NewService newService;

    @RequestMapping(value = "/newEndpoint")
    public Document newEndpoint(@RequestBody DocumentInput document) {
        return newService.handle(document);
    }
}

// Service Impl
@Service
public class NewServiceImpl implements NewService {
    @Autowired
    private NewHandler handler;

    @Override
    public Document handle(DocumentInput document) {
        // 解析请求
        // 调用 DAO
        // 构建响应
        ResultInfo info = new ResultInfo();
        info.setResultCode(BizErrorCode.SUCCESS.getCode());
        info.setResultMsg(BizErrorCode.SUCCESS.getMessage());
        return handler.getDocument(appId, funcKey, sign, info);
    }
}
```

## 数据库

-库名: ftdm
- 当前表: users (id, name, mobile, email, address)
- MyBatis Generator 已配置，可自动生成 DAO/Mapper
