#Getting started

Let's create a Spring MVC project, and begin to build the backend REST APIs. 

##Prerequisites

1. Java 8

	Oracle Java 8 is recommended. For Windows user, just go to [Oracle Java website](http://java.oracle.com) to download it and install into your system. Redhat has just released a OpenJDK 8 for Windows user at DevNation 2016, if you are stick on the OpenJDK, go to [Redhat Developers website](https://developers.redhat.com) and get it.
	
	Most of the Linux distributions includes the OpenJDK, install it via the Linux package manager.

	Optionally, you can set **JAVA\_HOME** environment variable and add *&lt;JDK installation dir>/bin* in your **PATH** environment variable.

2. Apache Maven
   
	Download the latest Apache Maven from [http://maven.apache.org](http://maven.apache.org), and uncompress it into your local system. 

	Optionally, you can set **M2\_HOME** environment varible, and also do not forget to append *&lt;Maven Installation dir>/bin* your **PATH** environment variable.  
	
	If you are a Gradle fan, you can use Gradle as build tool. Gradle could be an alternative of Apache Maven.

3. Spring ToolSuite

	STS is an Eclipse based IDE, and built-in supports for many Spring features, it is highly recommended for new Spring users.

	Go to Spring official site, download a copy of [Spring Tool Suite](https://spring.io/tools/sts). At the moment, the latest version is 3.8.
	
	Extract the files into your local disk.
	
	You can select your favorate IDEs, such as NetBeans, Intellij IDEA etc. All of these have good Maven and Gradle support.
	
##Create project skeleton

In these days, more and more poeples are using Spring Boot to get autoconfiguration support and quicker build lifecycle. 

For those new to Spring, the regular approache(none Sprinb Boot) is more easy to understand Spring essential configurations. 

Let's us create a Maven based Java EE 7 web application to introduce how to configure a Spring MVC web application in details, then switch to Spring Boot, thus you can compare these two approaches, and understand how Spring Boot simplifies configurations.

###Create a regular web project

1. Create a Maven web project.

	Use the official Java EE 7 web archetype to generate the project skeleton, replace the value of *archetypeId* and *package* to yours.

		mvn -DarchetypeGroupId=org.codehaus.mojo.archetypes 
		-DarchetypeArtifactId=webapp-javaee7 
		-DarchetypeVersion=1.1 
		-DgroupId=your_group_id 
		-DartifactId=angularjs-springmvc-sample 
		-Dversion=1.0.0-SNAPSHOT
		-Dpackage=com.hantsylabs.restsampels
		-Darchetype.interactive=false 
		--batch-mode  
		archetype:generate

2. Add Spring IO platform `platform-bom` to the *dependencyManagement*.

	    <dependencyManagement>
			<dependencies>
				<!-- Spring BOM -->
				<dependency>
					<groupId>io.spring.platform</groupId>
					<artifactId>platform-bom</artifactId>
					<version>2.0.6.RELEASE</version>
					<type>pom</type>
					<scope>import</scope>
				</dependency>
			</dependencies>
		</dependencyManagement>

	You can also use *platform-bom* as parent of this project.
	
	Read the Apache Maven docs to understand [Dependency Mechanism](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html).
	
3. Add essential dependencies for Spring MVC.

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-websocket</artifactId>
        </dependency>
        <!-- Spring security -->
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-core</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-web</artifactId>
        </dependency>

        <!-- Spring Data -->
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-commons</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-jpa</artifactId>
        </dependency>	
		
		<dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-entitymanager</artifactId>
        </dependency>
		
		<!--bean validation -->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-validator</artifactId>
        </dependency>

	Besides Spring core dependencies, Spring MVC, Spring Security , Hibernate, JPA/Spring Data JPA, Bean validation are also required.
	
	If you would like use `@Inject` instead of `@Autowire` in codes, add *inject* dependency.
	
        <dependency>
            <groupId>javax.inject</groupId>
            <artifactId>javax.inject</artifactId>
            <version>1</version>
        </dependency>	
	
4. Enable Spring MVC `DisptachServlet`.

	Declare a `AbstractAnnotationConfigDispatcherServletInitializer` bean.

		@Order(0)
		public class AppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

			@Override
			protected Class<?>[] getRootConfigClasses() {
				return new Class[] {
						AppConfig.class, //
						DataSourceConfig.class, //            
						JpaConfig.class, //
						DataJpaConfig.class,//
						SecurityConfig.class,//   
						Jackson2ObjectMapperConfig.class,//
						MessageSourceConfig.class
				};
			}

			@Override
			protected Class<?>[] getServletConfigClasses() {
				return new Class[] {
					WebConfig.class, //
					SwaggerConfig.class //
				};
			}

			@Override
			protected String[] getServletMappings() {
				return new String[] { "/" };
			}

			@Override
			protected Filter[] getServletFilters() {
				CharacterEncodingFilter encodingFilter = new CharacterEncodingFilter();
				encodingFilter.setEncoding("UTF-8");
				encodingFilter.setForceEncoding(true);

				return new Filter[] { encodingFilter };
			}

		}
	
	Serlvet 3.0 provides a new feature *ServletInitializer* to configure web applciation without *web.xml*. 
	
	Spring has its *WebApplicationInitializer* interface, there are a few classes implement this interface, `AbstractAnnotationConfigDispatcherServletInitializer` includes configuration of Spring Dispatch Servlet, and leave some room to customize `DispatchServlet`. 
	
	`getRootConfigClasses` specifies the configuration classes should be loaded for the Spring infrastrucuture.
	
	`getServletConfigClasses` specifies the configurations depend on Servlet specification, esp, web mvc related configurations.
	
	`getServletMappings` is the Spring `DispatchServlet` mapping URL pattern.
	
	`getServletFilters` are the filters will be applied on the Spring `DispatchServlet`.
	
	Spring MVC `DispatchServlet` is configured in the super classes, explore the details if you are interested in it.
	
4. Enable Spring Security

		@Order(1)
		public class SecurityInitializer extends AbstractSecurityWebApplicationInitializer {

		}	

	Similiar with `AbstractAnnotationConfigDispatcherServletInitializer`, it is a `WebApplicationInitializer` implementation, and aleady configured  Spring Security filter chain for you. 
	
5. Configuration classes.

    Therea are a few configuration classes already created for this sample application, as you saw in above `AppInitializer`.
   
    Every configuration should be annotated with `@Configuration`.
   
    It is recommended to slipt the configurations into different classes by categories. eg. In this sample the configuration classes includes:
   
    * `AppConfig` is the base configuration.
    * `DataSourceConfig` is the configuration for `DataSource`.
    * `JpaConfig` is the configuration for JPA and transacation.
    * `DataJpaConfig` is the configuration for Spring Data JPA extension support.
    * `SecurityConfig` is the Spring Security configuration.
    * `Jackson2ObjectMapperConfig` is the customized Jackson `ObjectMapper` bean.
    * `MessageSourceConfig` is the configuration for `MessageSource`.
    * `WebConfig` is the configuration for Spring MVC.
    * `SwaggerConfig` is the configuration for Swagger schema APIs.
   

    `@ComponentScan` defines the component scanning strategy for loading Spring `Component` at the application starts up. 
   
    The **excludeFilters** property define rules to exclude some components. `AppConfig` only scans none web beans, and `WebConfig` only scan web relates beans.
   
    **@PropertySource** configures the properties file will be loadded by Spring, later you can read the properties by `Environment`. Check the usage in `DataSourceConfig` class.
   
    Have a look at the content of `WebConfig` which is responsible for configuring Spring MVC in details, including resource handling, view, view resolvers etc.
   
	   public class WebConfig extends SpringDataWebConfiguration {
	   
	   }
	
	Generally, `WebMvcConfigurerAdapter` is use for customizing MVC details. `SpringDataWebConfiguration` is a subclass of `WebMvcConfigurerAdapter`, it is from Spring data project, and add pagination, sort, and domain object conversion support. Open the source code of `SpringDataWebConfiguration` and research yourself.
	
	`SecurityConfig` is the Spring security configuration details.
	
		@Configuration
		@EnableWebSecurity
		public class SecurityConfig extends WebSecurityConfigurerAdapter {
			@Override
			public void configure(WebSecurity web) throws Exception {
				web
					.ignoring()
					.antMatchers("/**/*.html", //
								 "/css/**", //
								 "/js/**", //
								 "/i18n/**",// 
								 "/libs/**",//
								 "/img/**", //
								 "/webjars/**",//
								 "/ico/**");
			}

			@Override
			protected void configure(HttpSecurity http) throws Exception {
					http   
						.authorizeRequests()   
						.antMatchers("/api/**")
						.authenticated()
					.and()
						.authorizeRequests()   
						.anyRequest()
						.permitAll()
						.and()
							.sessionManagement()
							.sessionCreationPolicy(SessionCreationPolicy.STATELESS)
						.and()
							.httpBasic()
						.and()
							.csrf()
							.disable();
			}

			@Override
			protected void configure(AuthenticationManagerBuilder auth)
					throws Exception {
				auth.inMemoryAuthentication()
							.passwordEncoder(passwordEncoder())
							.withUser("admin").password("test123").authorities("ROLE_ADMIN")
							.and()
								.withUser("test").password("test123").authorities("ROLE_USER");
			}


			@Bean
			@Override
			public AuthenticationManager authenticationManagerBean() throws Exception {
				return super.authenticationManagerBean();
			}

			@Bean
			@Override
			public UserDetailsService userDetailsServiceBean() throws Exception {
				return super.userDetailsServiceBean();
			}
		}
		
	`AuthenticationManagerBuilder` is the simplest entry to configure the essential security requirements. *InMemory* authentication is frequently used for demonstration. In a real world project, it is better to implement a `UserDetailsService` to load users from database. 
	
	Get the [codes](https://github.com/hantsy/angularjs-springmvc-sample) from my github account to explore all configuration classes.
	
###Create a web project with Spring Boot

[SPRING INITIALIZR](http://start.spring.io) provides a visual web ui to start a Spring Boot project. You can select which starter will be added in the project you will create.

Open https://start.spring.io, search *Web*, *Secrity*, *JPA*, *Validation* in the **Dependencies** input box, select the items in the dropdown menus.

![start.png](start.png)

Then press ALT+Enter or click **Generate** button to download the generated codes in zip archive.

Extract the files into your local system.

As a start point, it only includes a few files.

![start-file.png](start-file.png)

* The Maven project configuration *pom.xml*, and several maven wrapper files which is like Gradle wrapper and use to download a specific maven for this project.
* A Spring Boot specific `Application` class as the application entry.
* A dummy test for the `Application` class.

Open the *pom.xml*, it looks like:

	<?xml version="1.0" encoding="UTF-8"?>
	<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
		<modelVersion>4.0.0</modelVersion>

		<groupId>com.example</groupId>
		<artifactId>demo</artifactId>
		<version>0.0.1-SNAPSHOT</version>
		<packaging>jar</packaging>

		<name>demo</name>
		<description>Demo project for Spring Boot</description>

		<parent>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-parent</artifactId>
			<version>1.3.6.RELEASE</version>
			<relativePath/> <!-- lookup parent from repository -->
		</parent>

		<properties>
			<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
			<java.version>1.8</java.version>
		</properties>

		<dependencies>
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-data-jpa</artifactId>
			</dependency>
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-security</artifactId>
			</dependency>
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-validation</artifactId>
			</dependency>
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-web</artifactId>
			</dependency>
			
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-test</artifactId>
				<scope>test</scope>
			</dependency>
		</dependencies>
		
		<build>
			<plugins>
				<plugin>
					<groupId>org.springframework.boot</groupId>
					<artifactId>spring-boot-maven-plugin</artifactId>
				</plugin>
			</plugins>
		</build>
	</project>
	
* The package type is **jar**, which means it will include an embedded tomcat at build time. You can start the application via command line `java -jar app.jar`.
* The parent module is `spring-boot-starter-parent` which is a BOM(Bill of material) and includes the declaration of Spring Boot  dependencies. Just add the dependencies you want to use under the `dependencies` node.
* Every starter will handle transitive dependencies. Besides those starters we selected, it also includes a starter for test purpose which will add the popular test dependencies transitively, such as *hamcrest*, *assertj*, *mockito* etc.
* `spring-boot-maven-plugin` allow you run the project in the embedded Tomcat.

Another important file is the entry class of this sample application.

	@SpringBootApplication
	public class DemoApplication {

		public static void main(String[] args) {
			SpringApplication.run(DemoApplication.class, args);
		}
	}

`@SpringBootApplication` is a meta-annotation, which is a combined annotation with `@EnableAutoConfiguration`, `@ComponentScan` and `@SpringBootConfiguration`.

`@EnableAutoConfiguration` enables the autoconfiguration detection by default.
`@SpringBootConfiguration` is similiar with `@Configuration`, it indicates the appplication is a Spring Boot application, and only allow one `@SpringBootConfiguration` in an application.
`@ComponentScan` defines the scope to find components, by default if not specify the package, it will scan the annotated class as base package. So it is recommended to put the `Application` in the root package.


Besides these, nothing! Where are the configuration files?

Spring Boot internally used plenty of auto-configuration mechanism to simplfy the configuration progress for developers. For this project, it configured a simple BASIC authentication by default. If you add a H2 database or other JDBC drivers, it will configure a datasource and transacation manager automaticially. 

Till now, if you added some dependencies into `pom.xml`, you can start to code now. It is the quick way to prototype your application.

Although Spring Boot provides auto-configuration feature, but it does not prevent you to customize your configuration.

Create a *application.yml* in *src/main/resources* folder. It will override the default configuration. 

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

This is a classic application configuration file in *YAML* format.    

It is easy to understand.

**server.port** specifies the port number this application will serve at start up.

Under **spring** defines *DataSource*, *JPA*, *Spring Data JPA* etc.

**logging** configures logging level for packages.

**NOTE**: Spring Boot also supports *properties*, groovy format for application configuration.

In the [sample codes](https://github.com/hantsy/angularjs-springmvc-sample-boot), there are some custom configuration classes.

The following configuration extends the JPA config in application.yml.

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

Due to `DataSource` is defined in application configuration file, this configuration class is very simple, the main purpose is configuring the `AuditorAware` bean for Spring Data JPA auditing.

The security configuration defines a `WebSecurityConfigurerAdapter` bean.

	@Configuration
	public class SecurityConfig {
		
		@Bean
		public WebSecurityConfigurerAdapter webSecurityConfigure(){
			return new WebSecurityConfigurerAdapter() {
				
				@Override
				protected void configure(HttpSecurity http) throws Exception {
				// @formatter:off
					http
						.authorizeRequests()
						.antMatchers("/api/signup", "/api/users/username-check")
						.permitAll()
						.and()
							.authorizeRequests()
							.regexMatchers(HttpMethod.GET, "^/api/users/[\\d]*(\\/)?$").authenticated()
							.regexMatchers(HttpMethod.GET, "^/api/users(\\/)?(\\?.+)?$").hasRole("ADMIN")
							.regexMatchers(HttpMethod.DELETE, "^/api/users/[\\d]*(\\/)?$").hasRole("ADMIN")
							.regexMatchers(HttpMethod.POST, "^/api/users(\\/)?$").hasRole("ADMIN")
						.and()
							.authorizeRequests()
							.antMatchers("/api/**").authenticated()
						.and()
							.authorizeRequests()
							.anyRequest().permitAll()
						.and()
							.sessionManagement()
							.sessionCreationPolicy(SessionCreationPolicy.STATELESS)
						.and()
							.httpBasic()
						.and()
							.csrf()
							.disable();
				// @formatter:on
				}
			};
		}
	}

The `UserDetailsService` and passwordEncoder are also declared as Spring components, Spring Security 4.1 can recognise them.

`SwaggerConfig` is no difference with before version.

Get the [codes](https://github.com/hantsy/angularjs-springmvc-sample-boot) from my github account to view the details.
