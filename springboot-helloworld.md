

# SpringBoot简介

## 1、SpringBoot简介

概括来说，SpringBoot自身的特点：

- 简化Spring应用开发的一个框架
- 整个Spring技术栈的一个大整合
- J2EE开发的一站式解决方案

## 2、微服务

最早在2014年由martin fowler提出微服务的概念。微服务指的一种架构风格（服务微化），一个应用应该是一组小型服务，并且服务之间可以通过HTTP的方式进行通信。

单体应用：All in one

微服务：每一个功能元素最终都是一个可独立替换和独立升级的软件单元。

微服务原文文档：<https://martinfowler.com/articles/microservices.html#MicroservicesAndSoa> 

## 3、环境准备

- JDK8
- Maven 3.6.0
- IDEA 2018
- SpringBoot 1.5.21.RELEASE

ps：在做笔记的时候SpringBoot已经更新到2.1.5版本，SpringBoot 2.x很多地方相较于1.5.21版本都做了修改，但是整个框架的思想结构并没有太多的改变。

# SpringBoot-HelloWorld

任务目标：浏览器发送hello请求，服务器接受请求并处理，然后响应出“Hello World”字符串。

## 创建工程

### 创建SpringBoot项目方法一

打开IDEA，选择`new Project`，选择`Spring Initializr`，如下：

<div align="center"><img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/springboot/springboot-01.png"/>

</div>

点击`Next`，然后如下图所示：

<div align="center"><img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/springboot/springboot-02.png">

</div>

然后继续Next，选中`Web`模块，并选择`SpringBoot`的版本为`1.5.21`。

<div align="center"><img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/springboot/springboot-03.png">

</div>

然后一路`Next`下去，第一个HelloWorld工程就创建好了。

### 创建SpringBoot项目方法二

由于第一种方式由SpringBoot官方网站`https://start.spring.io`引导创建项目经常会失去链接，所以很多时候会采用第二种方式进行创建SpringBoot项目。

首先创建Maven项目：

<div align="center"><img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/springboot/springboot-04.png"/>

</div>

<div align="center"><img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/springboot/springboot-05.png"></div>

然后点击`Next`并且点击`Finish`完成创建Maven项目即可。

然后在`pom.xml`文件中手动添加SpringBoot依赖：

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.21.RELEASE</version>
</parent>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

其中注意SpringBoot是作为父工程项目引入的，web模块是作为依赖引入的。

然后我们需要手动创建如下文件：
<div align="center"><img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/springboot/springboot-06.png"></div>

其中`SpringBoothelloWorldApplication.java`中代码如下：

```java
package cn.bestzuo.springboot.helloworld;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringbootHelloworldApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringbootHelloworldApplication.class, args);
    }
}
```

这个主类是启动SpringBoot的主程序类，也是SpringBoot应用启动的入口，当然这个类内部做了什么事我们先不管，我们继续编写Controller类来实现HelloWorld。

## 实现HelloWorld

创建类`HelloWorld`如下：

```java
package cn.bestzuo.springboot.helloworld.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {
    
    @ResponseBody
    @RequestMapping("/hello")
    public String hello(){
        return "Hello World";
    }
}
```

可以看到这跟SpringMVC基本是一模一样的，这是因为SpringBoot内部集成了SpringMVC的原因。

然后我们运行`SpringBootHelloWorldApplication.java`主类，我们可以看到在控制台打印了很多日志输出。

<div align="center"><img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/springboot/springboot-07.png">

</div>

可以看到在最后面提示Tomcat运行在8080端口，然后打开浏览器输出`localhost:8080/hello`，可以看到成功输出`Hello World`了！

<div align="center"><img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/springboot/springboot-08.png"></div>

可能你觉得奇怪，为什么根本就没有配置Tomcat，而控制台却显示了Tomcat信息呢？然后你马上应该想到了，这肯定是SpringBoot内部已经集成好了Tomcat，所以不再需要手动配置了。

那么你肯定想到了，如此高度集成Tomcat插件，那么如果想要修改Tomcat信息，比如端口号等等，该怎么去修改呢？这个问题其实不是问题，SpringBoot已经帮我们准备好了，上面创建的`application.yml`文件就是这个SpringBoot的全局配置文件，Tomcat相关配置信息完全可以在这个配置文件中解决。

# SpringBoot-HelloWorld探析

刚刚完成上面的HelloWorld实验，一方面会因为SpringBoot能够如此迅速搭建web项目而感到惊叹，另外一方面你肯定也很想知道这是怎么做到的。下面我们就来一一探析。

## pom文件

首先我们从pom文件开始

```xml
<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring‐boot‐starter‐parent</artifactId>
	<version>1.5.21.RELEASE</version>
</parent>

他的父项目是
<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring‐boot‐dependencies</artifactId>
	<version>1.5.21.RELEASE</version>
	<relativePath>../../spring‐boot‐dependencies</relativePath>
</parent>
他来真正管理Spring Boot应用里面的所有依赖版本；
```

对了，这个SpringBoot的版本号就相当于是SpringBoot的版本仲裁中心，内部解决了所有Spring可能用到的依赖造成的冲突，所以在SpringBoot项目中导入依赖默认是不需要写版本号的，当然了，没有在dependencies里面管理的依赖自然需要声明版本号。

然后我们再来看一看web启动模块：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

我们可以打开这个依赖包看一下：

<div align="center"><img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/springboot/springboot-09.png">

</div>

然后就发现了这个玩意的本质，原来在这个依赖包中，已经默认帮我们导入了Spring的web和webmvc模块，并且还有相关的jackson依赖以及日志包等等，甚至还有一个神奇的spring-boot-starter-tomcat启动器依赖，这就不难理解为什么上面我们只导入了一个依赖却能轻松的使用这么多组件。原来都是SpringBoot帮我们一站式解决了这些依赖。

而细心的人应该已经发现上面还有一个`Spring-boot-starter`，翻译过来就是SpringBoot启动器，这个依赖就是SpringBoot的核心依赖，Spring Boot将所有的功能场景都抽取出来，做成一个个的starters（启动器），只需要在项目里面引入这些starter相关场景的所有依赖都会导入进来。要用什么功能就导入什么场景的启动器 。

## 主程序类

```java
@SpringBootApplication
public class SpringbootHelloworldApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringbootHelloworldApplication.class, args);
    }
}
```

然后回到主程序类中，我们首先会关注@SpringBootApplication这个注解，这个注解的外在含义比较容易理解，就是Spring Boot应用标注在某个类上从而说明这个类是SpringBoot的主配置类，SpringBoot就可以运行这个类的main方法来启动SpringBoot应用。

那么这个注解的内在含义呢？我们可以点开这个注解类一探究竟。

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = {
		@Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM,
				classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {

	@AliasFor(annotation = EnableAutoConfiguration.class, attribute = "exclude")
	Class<?>[] exclude() default {};

	@AliasFor(annotation = EnableAutoConfiguration.class, attribute = "excludeName")
	String[] excludeName() default {};

	@AliasFor(annotation = ComponentScan.class, attribute = "basePackages")
	String[] scanBasePackages() default {};

	@AliasFor(annotation = ComponentScan.class, attribute = "basePackageClasses")
	Class<?>[] scanBasePackageClasses() default {};
}
```

那么首先我们需要知道上面这几个注解的含义：

- `@SpringBootConfifiguration`：标注在某个类上，表示这是一个SpringBoot的配置类； 
- `@Configuration`：表示这是一个配置类，相当于配置文件；
- `@EnableAutoConfiguration`：开启自动配置功能，该会注解告诉SpringBoot开启自动配置功能，这样自动配置才能生效； 
- `@AutoConfigurationPackage`：自动配置包
- `@Import(AutoConfigurationPackages.Registrar.class)`： Spring的底层注解@Import，给容器中导入一个组件；导入的组件由 AutoConfigurationPackages.Registrar.class； 将主配置类（@SpringBootApplication标注的类）的所在包及下面所有子包里面的所有组件扫描到Spring容器； 
- `EnableAutoConfigurationImportSelector`：导入哪些组件的选择器； 将所有需要导入的组件以全类名的方式返回；这些组件就会被添加到容器中； 会给容器中导入非常多的自动配置类（xxxAutoConfifiguration）；就是给容器中导入这个场景需要的所有组件， 并配置好这些组件； 

那么同样你应该想到了，上面这些注解中，最关键的一个应该是`@EnableAutoConfiguration`注解，这个注解帮我们自动配置了很多东西，那么具体配置了什么呢？我们可以扒开源码一看究竟，我们打开依赖中的spring-boot-autoconfigure:1.5.21.RELEASE源码包，如下图所示：

<div align="center"><img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/springboot/springboot-10.png"></div>

然后打开`META-INF/spring.factories`文件，这个文件中内容非常多：

```properties
# Initializers
org.springframework.context.ApplicationContextInitializer=\
org.springframework.boot.autoconfigure.SharedMetadataReaderFactoryContextInitializer,\
org.springframework.boot.autoconfigure.logging.AutoConfigurationReportLoggingInitializer

# Application Listeners
org.springframework.context.ApplicationListener=\
org.springframework.boot.autoconfigure.BackgroundPreinitializer

# Auto Configuration Import Listeners
org.springframework.boot.autoconfigure.AutoConfigurationImportListener=\
org.springframework.boot.autoconfigure.condition.ConditionEvaluationReportAutoConfigurationImportListener

# Auto Configuration Import Filters
org.springframework.boot.autoconfigure.AutoConfigurationImportFilter=\
org.springframework.boot.autoconfigure.condition.OnClassCondition

# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
org.springframework.boot.autoconfigure.cassandra.CassandraAutoConfiguration,\
org.springframework.boot.autoconfigure.cloud.CloudAutoConfiguration,\
org.springframework.boot.autoconfigure.context.ConfigurationPropertiesAutoConfiguration,\
org.springframework.boot.autoconfigure.context.MessageSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.context.PropertyPlaceholderAutoConfiguration,\
org.springframework.boot.autoconfigure.couchbase.CouchbaseAutoConfiguration,\
org.springframework.boot.autoconfigure.dao.PersistenceExceptionTranslationAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.jpa.JpaRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.ldap.LdapDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.ldap.LdapRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.neo4j.Neo4jDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.neo4j.Neo4jRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.solr.SolrRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.rest.RepositoryRestMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.data.web.SpringDataWebAutoConfiguration,\
org.springframework.boot.autoconfigure.elasticsearch.jest.JestAutoConfiguration,\
org.springframework.boot.autoconfigure.freemarker.FreeMarkerAutoConfiguration,\
org.springframework.boot.autoconfigure.gson.GsonAutoConfiguration,\
org.springframework.boot.autoconfigure.h2.H2ConsoleAutoConfiguration,\
org.springframework.boot.autoconfigure.hateoas.HypermediaAutoConfiguration,\
org.springframework.boot.autoconfigure.hazelcast.HazelcastAutoConfiguration,\
org.springframework.boot.autoconfigure.hazelcast.HazelcastJpaDependencyAutoConfiguration,\
org.springframework.boot.autoconfigure.info.ProjectInfoAutoConfiguration,\
org.springframework.boot.autoconfigure.integration.IntegrationAutoConfiguration,\
org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.JdbcTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.JndiDataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.XADataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.DataSourceTransactionManagerAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.JmsAutoConfiguration,\
org.springframework.boot.autoconfigure.jmx.JmxAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.JndiConnectionFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.activemq.ActiveMQAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.artemis.ArtemisAutoConfiguration,\
org.springframework.boot.autoconfigure.flyway.FlywayAutoConfiguration,\
org.springframework.boot.autoconfigure.groovy.template.GroovyTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.jersey.JerseyAutoConfiguration,\
org.springframework.boot.autoconfigure.jooq.JooqAutoConfiguration,\
org.springframework.boot.autoconfigure.kafka.KafkaAutoConfiguration,\
org.springframework.boot.autoconfigure.ldap.embedded.EmbeddedLdapAutoConfiguration,\
org.springframework.boot.autoconfigure.ldap.LdapAutoConfiguration,\
org.springframework.boot.autoconfigure.liquibase.LiquibaseAutoConfiguration,\
org.springframework.boot.autoconfigure.mail.MailSenderAutoConfiguration,\
org.springframework.boot.autoconfigure.mail.MailSenderValidatorAutoConfiguration,\
org.springframework.boot.autoconfigure.mobile.DeviceResolverAutoConfiguration,\
org.springframework.boot.autoconfigure.mobile.DeviceDelegatingViewResolverAutoConfiguration,\
org.springframework.boot.autoconfigure.mobile.SitePreferenceAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.embedded.EmbeddedMongoAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.MongoAutoConfiguration,\
org.springframework.boot.autoconfigure.mustache.MustacheAutoConfiguration,\
org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration,\
org.springframework.boot.autoconfigure.reactor.ReactorAutoConfiguration,\
org.springframework.boot.autoconfigure.security.SecurityAutoConfiguration,\
org.springframework.boot.autoconfigure.security.SecurityFilterAutoConfiguration,\
org.springframework.boot.autoconfigure.security.FallbackWebSecurityAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.OAuth2AutoConfiguration,\
org.springframework.boot.autoconfigure.sendgrid.SendGridAutoConfiguration,\
org.springframework.boot.autoconfigure.session.SessionAutoConfiguration,\
org.springframework.boot.autoconfigure.social.SocialWebAutoConfiguration,\
org.springframework.boot.autoconfigure.social.FacebookAutoConfiguration,\
org.springframework.boot.autoconfigure.social.LinkedInAutoConfiguration,\
org.springframework.boot.autoconfigure.social.TwitterAutoConfiguration,\
org.springframework.boot.autoconfigure.solr.SolrAutoConfiguration,\
org.springframework.boot.autoconfigure.thymeleaf.ThymeleafAutoConfiguration,\
org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration,\
org.springframework.boot.autoconfigure.transaction.jta.JtaAutoConfiguration,\
org.springframework.boot.autoconfigure.validation.ValidationAutoConfiguration,\
org.springframework.boot.autoconfigure.web.DispatcherServletAutoConfiguration,\
org.springframework.boot.autoconfigure.web.EmbeddedServletContainerAutoConfiguration,\
org.springframework.boot.autoconfigure.web.ErrorMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.web.HttpEncodingAutoConfiguration,\
org.springframework.boot.autoconfigure.web.HttpMessageConvertersAutoConfiguration,\
org.springframework.boot.autoconfigure.web.MultipartAutoConfiguration,\
org.springframework.boot.autoconfigure.web.ServerPropertiesAutoConfiguration,\
org.springframework.boot.autoconfigure.web.WebClientAutoConfiguration,\
org.springframework.boot.autoconfigure.web.WebMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.WebSocketAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.WebSocketMessagingAutoConfiguration,\
org.springframework.boot.autoconfigure.webservices.WebServicesAutoConfiguration

# Failure analyzers
org.springframework.boot.diagnostics.FailureAnalyzer=\
org.springframework.boot.autoconfigure.diagnostics.analyzer.NoSuchBeanDefinitionFailureAnalyzer,\
org.springframework.boot.autoconfigure.jdbc.DataSourceBeanCreationFailureAnalyzer,\
org.springframework.boot.autoconfigure.jdbc.HikariDriverConfigurationFailureAnalyzer

# Template availability providers
org.springframework.boot.autoconfigure.template.TemplateAvailabilityProvider=\
org.springframework.boot.autoconfigure.freemarker.FreeMarkerTemplateAvailabilityProvider,\
org.springframework.boot.autoconfigure.mustache.MustacheTemplateAvailabilityProvider,\
org.springframework.boot.autoconfigure.groovy.template.GroovyTemplateAvailabilityProvider,\
org.springframework.boot.autoconfigure.thymeleaf.ThymeleafTemplateAvailabilityProvider,\
org.springframework.boot.autoconfigure.web.JspTemplateAvailabilityProvider
```

Spring Boot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值，将这些值作为自动配置类导入到容器中，自动配置类就生效，帮我们进行自动配置工作；以前我们需要自己配置的东西，自动配置类都帮我们配置好，这就是SpringBoot为什么会如此方便的原因。 

J2EE的整体整合解决方案和自动配置都在spring-boot-autoconfifigure-1.5.21.RELEASE.jar； 

# application.yml配置文件

SpringBoot使用一个全局的配置文件，配置文件名是固定的； 

- application.properties 
- application.yml 

配置文件的作用：修改SpringBoot自动配置的默认值；SpringBoot在底层都给我们自动配置好； 

YAML（YAML Ain't Markup Language） 

YAML A Markup Language：是一个标记语言 

YAML isn't Markup Language：不是一个标记语言； 

标记语言： 

以前的配置文件；大多都使用的是 xxxx.xml文件； 

YAML：以数据为中心，比json、xml等更适合做配置文件； 

YAML：配置例子 

```yml
server:
	port: 8081
```

XML：

```xml
<server>
	<port>8081</port>
</server>
```

关于yml语法这里就不再多说，可以自行参考yml语法官方文档。

## profile

在实际开发中，会有多种环境，比如实际生产环境、开发环境、测试环境，每种环境下的配置都不会完全相同，如果每次都要修改配置文件过于麻烦，因此SpringBoot引入了profile，可以通过切换profile来达到切换环境的目的。

我们在主配置文件编写的时候，文件名可以是`application-{profile}.properties/yml `

默认使用`application.properties`的配置；

yml文件同时也支持多文档块的写法：

```yml
server:
	port: 8081
	spring:
profiles:
	active: prod
‐‐‐
server:
	port: 8083
spring:
	profiles: dev
‐‐‐
server:
	port: 8084
spring:
	profiles: prod #指定属于哪个环境
```

那么如何切换呢？

- 在配置文件中指定 spring.profiles.active=dev 
- 命令行： java -jar spring-boot-02-confifig-0.0.1-SNAPSHOT.jar --spring.profifiles.active=dev； 可以直接在测试的时候，配置传入命令行参数 
- 虚拟机参数； -Dspring.profifiles.active=dev 

当然实际中用的最多的还是第一种方式，直接指定即可。

