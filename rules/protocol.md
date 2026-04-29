# XML 报文协议与签名规范

## 报文格式

- Content-Type: application/xml
- 字符集: UTF-8
- 签名: RSA

## 报文结构

```xml
<Document>
  <Request>  <!-- 或 <Response> -->
    <Head>
      <version>1.0.0</version>
      <signType>RSA</signType>
      <respTime>20180909130908</respTime>
      <inputCharset>UTF-8</inputCharset>
      <appId>GLBANK85</appId>
      <function>com.mybank.cooperation.apply.notify</function>
      <reqMsgId>20180908120909</reqMsgId>
      <reverse></reverse>
    </Head>
    <Body>
      <applyNo>2018090908124567890</applyNo>
      <requestId>3456723456783456782345678</requestId>
      <resultInfo>
        <resultCode>0000</resultCode>
        <resultMsg>处理成功</resultMsg>
        <retry>Y</retry>
      </resultInfo>
    </Body>
  </Request>
  <Signature>
    <signature>signature_code_here</signature>
  </Signature>
</Document>
```

## 四个接口

| 接口 | 方向 | 说明 |
|------|------|------|
| /apply_notify | 网商银行 → 网关 | 初审通知 |
| /approve_upload | 网关 → 网商银行 | 初审数据上传 |
| /approveack_notify | 网商银行 → 网关 | 终审通知 |
| /approveack_confirm | 网关 → 网商银行 | 终审确认 |

## 签名流程

1. 请求到达 gateway controller，被 @Sign 注解标记
2. SignAspect 拦截，打印方法名和参数
3. 调用 SignUtil.createSign() 生成签名 (⚠️ 目前返回空字符串，待实现)

## 签名算法 (需补充实现)

```java
// SignUtil.createSign() 应实现:
// 1. 将参数按 key 排序拼接: key1=value1&key2=value2...
// 2. 排除 sign 和 key 字段
// 3. 使用 RSA 私钥签名
```
