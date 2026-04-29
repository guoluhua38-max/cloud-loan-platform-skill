# 示例：新增数据表 + DAO + Mapper

## 需求

新增贷款记录表 `loan_record`。

## 步骤

### 1. 创建 Entity

```java
// service/src/main/java/com/cloud/service/model/entity/LoanRecordDO.java
public class LoanRecordDO {
    private Long id;
    private String applyNo;      // 申请编号
    private String userId;       // 用户ID
    private BigDecimal amount;   // 贷款金额
    private String status;       // 状态: PENDING/APPROVED/REJECTED/DISBURSED
    private Date createTime;
    private Date updateTime;
    // getter/setter...
}
```

### 2. 创建 DAO

```java
// service/src/main/java/com/cloud/service/dao/LoanRecordDao.java
@Mapper
public interface LoanRecordDao {
    LoanRecordDO selectByApplyNo(String applyNo);
    List<LoanRecordDO> selectByUserId(String userId);
    int insert(LoanRecordDO record);
    int updateStatus(@Param("applyNo") String applyNo, @Param("status") String status);
}
```

### 3. 创建 Mapper XML

```xml
<!-- service/src/main/resources/mapper/LoanRecordMapper.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cloud.service.dao.LoanRecordDao">
    <select id="selectByApplyNo" resultType="com.cloud.service.model.entity.LoanRecordDO">
        SELECT * FROM loan_record WHERE apply_no = #{applyNo}
    </select>
    <select id="selectByUserId" resultType="com.cloud.service.model.entity.LoanRecordDO">
        SELECT * FROM loan_record WHERE user_id = #{userId}
    </select>
    <insert id="insert" parameterType="com.cloud.service.model.entity.LoanRecordDO">
        INSERT INTO loan_record(apply_no, user_id, amount, status, create_time)
        VALUES(#{applyNo}, #{userId}, #{amount}, #{status}, #{createTime})
    </insert>
    <update id="updateStatus">
        UPDATE loan_record SET status = #{status}, update_time = NOW()
        WHERE apply_no = #{applyNo}
    </update>
</mapper>
```

### 4. 创建数据库表

```sql
CREATE TABLE loan_record (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    apply_no VARCHAR(64) NOT NULL UNIQUE,
    user_id VARCHAR(64) NOT NULL,
    amount DECIMAL(15,2) NOT NULL,
    status VARCHAR(32) NOT NULL DEFAULT 'PENDING',
    create_time DATETIME DEFAULT CURRENT_TIMESTAMP,
    update_time DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    INDEX idx_user_id (user_id),
    INDEX idx_status (status)
);
```
```

