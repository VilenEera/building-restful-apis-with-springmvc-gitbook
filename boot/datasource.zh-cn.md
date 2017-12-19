# 配置数据源

而不是在 Java 代码中配置 `DataSource`.

Spring Boot 提供了内置的 `application.properties`(或 YAML 格式的 `applicatoin.yml`) 来配置项目中特定的 `DataSource` 文件.

在 *src/main/resources* 中创建一个 *application.yml* 文件夹. 它将覆盖默认的配置. 

```yaml
server:
	port: 9000
	contextPath: 

spring: 
	profiles:
		active: dev
	devtools.restart.exclude: static/**,public/**
	datasource:
		dataSourceClassName: org.h2.jdbcx.JdbcDataSource
		url: jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1
		databaseName: 
		serverName: 
		username: sa
		password: 

	jpa:
		database-platform: org.hibernate.dialect.H2Dialect
		database: H2
		openInView: false
		show_sql: true
		generate-ddl: true
		hibernate:
			ddl-auto: 
			naming-strategy: org.hibernate.cfg.EJB3NamingStrategy
		properties:
			hibernate.cache.use_second_level_cache: true
			hibernate.cache.use_query_cache: false
			hibernate.generate_statistics: true
			hibernate.cache.region.factory_class: org.hibernate.cache.internal.NoCachingRegionFactory

	data:
		jpa.repositories.enabled: true 
		
	freemarker:
		check-template-location: false
		
	messages:
		basename: messages

logging:
	file: app.log
	level:
		root: INFO
		org.springframework.web: INFO
		com.hantsylabs.restexample.springmvc: DEBUG
```
			
这是一个经典的 *YAML* 格式化的配置文件 .    

可以被轻松理解.

**server.port** 这个程序将在启动时将在指定的该端口号提供服务.

在 **spring** 之下定义 *DataSource*, *JPA*, *Spring Data JPA* 等等.

**logging** 为所在的包配置 logging 等级.

**注意**: Spring Boot 也支持 *properties*, *groovy DSL* 的格式化配置信息.