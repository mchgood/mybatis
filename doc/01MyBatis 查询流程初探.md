## 快速开始

为了简化代码量，预计使用 mybatis-plus 来实现查询功能

### sql 初始文件

schema-h2.sql

```sql
DROP TABLE IF EXISTS `user`;

CREATE TABLE `user`
(
    id BIGINT NOT NULL COMMENT '主键ID',
    name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
    age INT NULL DEFAULT NULL COMMENT '年龄',
    email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
    PRIMARY KEY (id)
);
```

data-h2.sql

```sql
DELETE FROM `user`;

INSERT INTO `user` (id, name, age, email)
VALUES (1, 'Jone', 18, 'test1@baomidou.com'),
       (2, 'Jack', 20, 'test2@baomidou.com'),
       (3, 'Tom', 28, 'test3@baomidou.com'),
       (4, 'Sandy', 21, 'test4@baomidou.com'),
       (5, 'Billie', 24, 'test5@baomidou.com');
```

### 依赖

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-spring-boot3-starter</artifactId>
    <version>3.5.9</version>
</dependency>

<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <version>2.2.224</version>
</dependency>
```

### application.yaml

```yaml
# DataSource Config
spring:
  datasource:
    driver-class-name: org.h2.Driver
    username: root
    password: test
  sql:
    init:
      schema-locations: classpath:db/schema-h2.sql
      data-locations: classpath:db/data-h2.sql
```

### 单元测试代码

```java
@SpringBootTest
class MybatisPlusDemoApplicationTests {
    @Autowired
    private SqlSessionFactory sqlSessionFactory;

    @Test
    void contextLoads() {
        try (val sqlSession = sqlSessionFactory.openSession()) {
            val userMapper = sqlSession.getMapper(UserMapper.class);
            List<UserPo> userList = userMapper.selectList(null);
            Assert.isTrue(5 == userList.size(), "");
            userList.forEach(System.out::println);
        }
    }
}
```

## 代码流程梳理

