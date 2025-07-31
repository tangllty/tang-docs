# Spring Boot 集成

本节介绍如何将 Kite 集成到 Spring Boot 应用程序中。

> MySQL 示例

## 使用方法

> Maven 中央仓库: [kite-spring-boot-starter](https://central.sonatype.com/artifact/io.github.tangllty/kite-spring-boot-starter)

1. 向项目添加以下依赖：

* Maven

```xml
<dependency>
   <groupId>io.github.tangllty</groupId>
   <artifactId>kite-spring-boot-starter</artifactId>
   <version>${kite.version}</version>
</dependency>
```

* Gradle

```kts
implementation("io.github.tangllty:kite-spring-boot-starter:${kite.version}")
```

2. 在数据库中创建表

```sql
create table account (
  id          bigint not null auto_increment,
  username    varchar(32)     default '',
  password    varchar(32)     default '',
  balance     decimal(10,2)   default '0.00',
  create_time datetime        default null,
  update_time datetime        default null,
  primary key (`id`)
);

insert into account (username, password, create_time, balance) values
('admin', 'admin123', '2020-01-01 12:00:00', 1000.10),
('user', 'user123', '2024-05-02 8:30:00', 101.00),
('guest', 'guest123', '2022-03-03 15:00:00', 10.00),
('tang', 'tang123', '2019-06-01 21:30:30', 1.88),
('jeo', 'jeo123', '2024-07-01 5:59:59', 0.10);
```

3. 在 `application.yml` 文件中配置数据库连接信息

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/kite-test
    username: root
    password: password
```

4. 为 `account` 表创建模型类

:::tabs key:kite
== Java

```java
import com.tang.kite.annotation.id.Id;
import com.tang.kite.annotation.id.IdType;
import java.math.BigDecimal;
import java.time.LocalDateTime;

public class Account {

    @Id(type = IdType.AUTO)
    private Long id;
    private String username;
    private String password;
    private BigDecimal balance;
    private LocalDateTime createTime;
    private LocalDateTime updateTime;

    // Getters and Setters
}
```

== Kotlin

```kotlin
import com.tang.kite.annotation.id.Id
import com.tang.kite.annotation.id.IdType
import java.math.BigDecimal
import java.time.LocalDateTime

class Account (

    @Id(type = IdType.AUTO)
    var id: Long? = null,
    var username: String = "",
    var password: String = "",
    var balance: BigDecimal? = null,
    var createTime: LocalDateTime? = null,
    var updateTime: LocalDateTime? = null

)
```
:::

5. 扩展 `BaseMapper` 接口创建 Mapper 接口

:::tabs key:kite
== Java

```java
import com.tang.kite.mapper.BaseMapper;
import com.tang.kite.spring.annotation.Mapper;

@Mapper
public interface AccountMapper extends BaseMapper<Account> {
}
```

== Kotlin

```kotlin
import com.tang.kite.mapper.BaseMapper
import com.tang.kite.spring.annotation.Mapper

@Mapper
interface AccountMapper : BaseMapper<Account>
```
:::

6. 在 Spring Boot 应用类上添加 `@MapperScan` 注解

:::tabs key:kite
== Java

```java
import com.tang.kite.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@MapperScan("com.tang.demo.mapper")
@SpringBootApplication
public class KiteApplication {

    public static void main(String[] args) {
        SpringApplication.run(KiteApplication.class, args);
    }

}
```

== Kotlin

```kotlin
import com.tang.kite.spring.annotation.MapperScan
import org.springframework.boot.autoconfigure.SpringBootApplication
import org.springframework.boot.runApplication

@MapperScan(["com.tang.demo.mapper"])
@SpringBootApplication
class KiteApplication

fun main(args: Array<String>) {
	runApplication<KiteApplication>(*args)
}
```

:::


7. 测试 Mapper 接口

:::tabs key:kite
== Java

```java
import com.tang.demo.mapper.AccountMapper;
import com.tang.kite.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@MapperScan("com.tang.demo.mapper")
@SpringBootApplication
public class KiteApplication {

    public static void main(String[] args) {
        var context = SpringApplication.run(KiteApplication.class, args);
        var accountMapper = context.getBean(AccountMapper.class);
        var accounts = accountMapper.select();
        accounts.forEach(System.out::println);
    }

}
```
== Kotlin

```kotlin
import com.tang.demo.mapper.AccountMapper
import com.tang.kite.spring.annotation.MapperScan
import org.springframework.boot.autoconfigure.SpringBootApplication
import org.springframework.boot.runApplication

@MapperScan(["com.tang.demo.mapper"])
@SpringBootApplication
class KiteApplication

fun main(args: Array<String>) {
	val context = runApplication<KiteApplication>(*args)
	val accountMapper = context.getBean(AccountMapper::class.java)
	val accounts = accountMapper.select()
	accounts.forEach { println(it) }
}
```
:::
