# 配置 JPA

确认下列的依赖在你的项目 `pom.xml` 文件的 `dependency`区域被添加上.

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

在你的项目中它将管理 JPA, Spring Data JPA 以及 Hibernate.

我们在 `application.yml` 已经配置了 `DataSource`, 和 JPA 以及 Spring Data JPA.

你也可以使用 Java code 配置的方式添加一些额外的特性.

```java
@Configuration
@EnableTransactionManagement(mode = AdviceMode.ASPECTJ)
@EntityScan(basePackageClasses = {User.class, Jsr310JpaConverters.class})
@EnableJpaAuditing(auditorAwareRef = "auditor")
public class JpaConfig {

    @Bean
    public AuditorAware<User> auditor() {
        return () -> SecurityUtil.currentUser();
    }

}
```

上面的代码中我们添加了 `@EnableTransactionManagement(mode = AdviceMode.ASPECTJ)`,作用是添加了事务管理器, 在你的业务逻辑中使用 `AspectJ` 组织事务管理切面.

以及 `@EnableJpaAuditing(auditorAwareRef = "auditor")` Spring Data JPA 打开简单的审计功能. 它需要一个 `AuditorAware` bean.

`@EntityScan(basePackageClasses = {User.class, Jsr310JpaConverters.class})`增加了 JPA entity 扫描范围, Spring Data JPA 的 JPA 2.1 采用 `AttributeConvertor` 功能为 Java 8 提供 DateTime 支持.

 