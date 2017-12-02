#概述

在这本小书中, 我将示范采用 Spring MVC 和 AngularJS 来实现一个 RESTful web 程序.

它将由一系列小 posts 文章按照时间的顺序组成,每一篇 post 文章都是专注于同一个主题的独立章节.

##假定

我将假设你是一名 Java 开发者，同时有一定的 Spring framework 框架的开发经验.

你将学习到基本的 Java 和 Java EE 知识，和主要的 Spring framework 框架的用法.

* 官方 [Oracle Java 教程](https://docs.oracle.com/javase/tutorial/) 以及 [Java EE 教程](https://docs.oracle.com/javaee/7/tutorial) 提供给 Java 新手们.

* 阅读 [Spring 官方指南](https://spring.io/guides) 开始使用 Spring framework.

接下来的文章, 不会完全的覆盖 Spring and Java EE 特性, 但是一些新特性将会被使用.

* Spring framework

	Spring framework 是这个示例程序的基础框架. 

	他将提供一个轻量级的 IOC 容器和一个简易的 POJO 基本编程模型, 以及包含许多的 Java EE 规范支持的粘合代码 ，以及流行的开源框架集成. 

	采用 Spring 的好处是, 它将使得 Java EE 环境不在使用容器变为可能, 很容易的进行 Java EE 测试.过去的几年中, Spring 被认为是 Java EE 环境的事实标准.


* Spring MVC

	Spring framework 最吸引人特性之一就是 Spring MVC framework, 它的功能类似老旧的 Struts framework, 它是一个符合 Servlet 规范的 web 框架, 实现了标准的 MVC(Model, View, Controller) 模式. 

	Spring MVC 提供的许多的 view 展示, for traditional web application or RESTful APIs. In this sample application, we only use Spring MVC as the REST API producer and exposes the APIs to client.

	For the traditional web development, check my samples hosted on [Spring4 sandbox](https://github.com/hantsy/spring4-sandbox).

* Spring Security

	在传统的 Java EE 应用程序中,JAAS 是负责认证和授权的规范.但是它非常依赖于一个特定的容器.尽管大多数容器都包含于用户管理的可视化 Web UI.但是如果你想用程序的方式来管理用户和角色.
	
	Spring Security 填补了这个领域,使安全控制变得简单,并提供了一个简单的编程模型,Spring Security 还兼容 JAAS 规范,并在运行时提供 JAAS 集成.

	Java EE 8 试图引入一个新的安全规范来解决这个问题.

* JPA

	JPA 基于 JDBC 规范提供了高级别的 ORM 抽象,并带来了 OOP 哲学于传统的 RDBMS 交互.Hibernate 和 EclipseLink 也支持 NoSQL.

* Hibernate

	在这个示例应用程序中, Hibernate 被用作与 JPA 提供者. 大多数时候, 我们试图避免使用提供商特定的 API,使代码可以通过 JPA 的提供来运行.

* Spring Data JPA

	Spring Data JPA 简化了在 Spring 中使用 JPA, 包括统一的 `Repository` 无需编码即可去进行简单的 CRUD, 简化安全的  Criteria Query 和 QueryDSL 集成, 一个简单的审计实现, 简单的查询结果分页支持, Java 8 Optional 和 DateTime 支持等等.

	想查看更多请跳转去 Spring Data 示例程序  [Spring4 sandbox](https://github.com/hantsy/spring4-sandbox).
	 
	
我们还使用一些第三方的实用程序, 例如 [Lombok project](https://projectlombok.org/) 来消除 POJOs 的 getters 和 setters. 

为了测试的目的, 将使用 Spring test/JUnit, Mockito, Rest Assured

##示例应用程序

为了演示如何构建 RESTful APIs, 我将实现一个简单的 Blog 系统来详细解释它.

想象一下有两个角色会使用这个博客系统.

* ROLE_ADMIN, 管理员.
* ROLE_USER, 普通用户.

普通用户可以执行最常见的任务.

1. 创建一个新的帖子.
2. 更新帖子.
3. 查看帖子的详细信息.
4. 按照关键字搜索帖子.
5. 删除帖子.
6. 评论帖子.

管理员应该拥有更高级的权限,例如, 他可以管理用户系统.

1. 创建一个新用户.
2. 更新 user 信息.
3. 删除用户.
4. 按关键字搜索用户.

##示例代码

完整的示例代码托管在我的 Github.com 账户上.

[https://github.com/hantsy/angularjs-springmvc-sample](https://github.com/hantsy/angularjs-springmvc-sample)

基于 Spring Boot 的版本提供了更多的功能来演示前沿的一些技术.

[https://github.com/hantsy/angularjs-springmvc-sample-boot](https://github.com/hantsy/angularjs-springmvc-sample-boot)

请阅读这些仓库中的 README.md 文件并在本地系统下运行它们.

##反馈

本书的来源在我的 github.com 账户上.

[https://github.com/hantsy/angularjs-springmvc-sample-gitbook](https://github.com/hantsy/angularjs-springmvc-sample-gitbook)

随心所欲的提供这个项目的反馈意见.