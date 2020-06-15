 bugs:

### 1: mapper 找不到 ==> 不使用@Repository 注解,改使用 @Mapper注解

```java
required a bean of type 'com.example.datawash.dao.TestDaoWordbook' that could not be found.
```

解决方案:

​	使用 @Mapper 注解

2. 启动类加

3. ```java
   java.lang.annotation.AnnotationFormatError: Invalid default: public abstract java.lang.Class org.mybatis.spring.annotation.MapperScan.factoryBean()
   ```

   版本问题,

4. 缺少 spring.starter包

   ```java
    		<dependency>
               <groupId>org.mybatis.spring.boot</groupId>
               <artifactId>mybatis-spring-boot-starter</artifactId>
               <version>2.1.1</version>
           </dependency>
   ```

   



### 2: 报警告 如下 mysql 的 异常

```tex
Wed Dec 11 11:13:39 CST 2019 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't se
```

解决: dbproperties.xml 中

```xml
spring.datasource.url = jdbc:mysql://localhost:3306/data_wash?useUnicode=true&characterEncoding=utf-8&useSSL=false
```

### 3: arraylist.contains() 空针异常

解决: 逻辑有问题



### 4: 不能加载bean  问题 11 是另一种情况

```java
org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'testController':
```

解决: jeecg 配置类的问题, 它指定了包扫描 路径;

### 5: org.apache.shiro.authc.AuthenticationException: Token失效，请重新登录

解决: 

### 6: Oracle "ORA-00942: 表或视图不存在 "

解决: Oracle 是大小写敏感的，但是 Oracle 同样支持”” 语法，将表名或字段名加上”“后，Oracle不会将其转换成大写



### 7: mybatis的一个bug

```java
org.apache.ibatis.exceptions.TooManyResultsException: Expected one result (or null) to be returned by selectOne(), but found: 26
```

 解决 : 结果集为空, 数据库表里没有对应的字段;



### 8: 标识符无效(oracle)

解决: oracle 大小写敏感,mybatis 在 转换时,会把列名转成 大写,但是 oracle里 表的字段名 如果是小写的话,oracle就识别不了, 

### 9: navicat 能查询,plsql不能

解决 : 当oracle数据表字段为小写时,必须使用引号("")将SQL中的列名包裹才能正确执行SQL语句.

### 10 : 

```java
org.apache.ibatis.binding.BindingException: Invalid bound statement (not found): com.example.datawash.dao.WordbookMapper.examineWordbookAll
```

解决 : 绑定问题, mapper.xml 中 id对应 的 sql语句 在 dao层中 未找到映射,xml中 有id 的 sql语句,在 dao层中 必须有对应的映射

### 11 : @Mapper  @Repository 都加载不到 dao层 bean

解决 : 自定义一个 配置类 , 自己加载 bean

```java
@Configuration
@ComponentScan(value = "org.jeecg.modules.datawash", includeFilters = {
  @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = {Mapper.class})})
public class DaoConfiguration {

    @Bean
    public WordbookMapper invokeImpController() {


        return new WordbookMapper(){

            @Override
            public List<Wordbook> examineWordbookAll() {
                return null;
            }

            @Override
            public List<MatchingToWordbook> examineMatchingToWordbook() {
                return null;
            }

            @Override
            public int insertBankFlow(HashMap<String, Object> hashMap) {
                return 0;
            }

            @Override
            public String examineWordbookFieldNameByType(Integer type) {
                return null;
            }

            @Override
            public String examineWordbookFieldCodeByType(Integer type) {
                return null;
            }
        };
    }
}
```

 

### 12  : springboot 使用thymeleaf 动态页面
跳转静态页面需要经过controller层才能实现跳转 (不经过静态资源报错)
否则 报 No mapping for GET错误

```java
No mapping for GET /index.html  springboot
```

### 13 . controller 重名

```java
unmapped spring configuration files found
non-compatible bean definition of same name and class 
```



### 14.mysql 时区问题

```sql
set global time_zone = '+8:00';
set time_zone = '+8:00';
flush privileges;
```



### 15: mysql 

​		使用navicat 连接 mysql 时 报

```java
**caching_sha2_password**
```

​		解决:

```java
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
```



### 16: idea 加载不了 项目 

```java
Unable to import maven project:see logs
```

​		解决:

​		maven版本过高,高于 idea 能承受的版本 

### 17: log 报红

​		解决: 安装lombok 插件

### 18: idea链接mysql 连接不上, 

```java
Connection to sj_lstj@localhost failed.
[08001] Could not create connection to database server. Attempted reconnect 3 times. Giving up.

```

  	解决: 在url后加 

```java
?serverTimezone=gmt
```



#### 19: git 在commit 之后 没有添加注释 (没有 -m "") 报

```java
: src refspec master does not match any
```



### 20: 系统问题

```java
yarn : 无法加载文件 d:\node_global\yarn.ps1,因为在此系统上禁止运行脚本。有关
```

​	解决:

​	管理员 运行powshell

​	执行

```java
PS C:\Users\Administrator> set-ExecutionPolicy RemoteSigned

执行策略更改
执行策略可帮助你防止执行不信任的脚本。更改执行策略可能会产生安全风险，如 https:/go.microsoft.com/fwlink/?LinkID=135170
中的 about_Execution_Policies 帮助主题所述。是否要更改执行策略?
[Y] 是(Y)  [A] 全是(A)  [N] 否(N)  [L] 全否(L)  [S] 暂停(S)  [?] 帮助 (默认值为“N”): y

```

### 21: git 问题

clone时 仓库要密码

解决: 把密钥加到库里

### 22. 查看MySQL版本号

```sql
select version();	
```

#### 23. spring test 的一个异常

```java
Unable to find a @SpringBootConfiguration, you need to use @ContextConfiguration or @SpringBootTest(classes=...) with your test
```

注解这样写

```java
@SpringBootTest(classes = Application.class)
```

> import javafx.application.Application;																																							



#### 24. mybatis的一个异常,invalid comparison: java.sql.Timestamp and java.lang.String

```java
invalid comparison: java.sql.Timestamp and java.lang.String
```

 解决: 

mybatis 3.3.0中对于时间参数进行比较时的一个bug. 如果拿传入的时间类型参数与空字符串''进行对比判断则会引发异常. 所以在sql中去掉空串判断, 只保留非空判断就正常了																																							

```xml
<if test="changeDate != null and changeDate != '' ">
changedate = #{changeDate},
</if>
```



#### 25. 空指针异常

1. 是因为没有初始化变量;



#### 26. poi文件导出时候的bug

```java
Caused by: com.fasterxml.jackson.databind.exc.MismatchedInputException: Cannot 			    		deserialize instance of `java.util.ArrayList` out of START_OBJECT token

```

解决: 解决不了,前端的问题,

#### 27. poi设置颜色不起作用

解决:  

```java
    CellStyle cellStyle = workbook.createCellStyle();
    cellStyle.setFillForegroundColor(HSSFColor.GOLD.index);
    cellStyle.setFillPattern(CellStyle.SOLID_FOREGROUND);
```



#### 28. SpringBoot 前台页面get不到js,css文件

解决:

```java
@SpringBootApplication
public class TestApplication extends WebMvcConfigurationSupport {

    public static void main(String[] args) {
        SpringApplication.run(TestApplication.class, args);
    }

    /***************************************************
     * 这里配置静态资源文件的路径导包都是默认的直接导入就可以
     * @author: wg
     * @time: 2020/4/20 15:54
     ***************************************************/
    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/static/**")
                .addResourceLocations(ResourceUtils.CLASSPATH_URL_PREFIX + "/static/");
        super.addResourceHandlers(registry);
    }
}
```

**之后, 重启idea**



#### 29. Caused by: java.lang.IllegalArgumentException: invalid comparison: java.util.Date and java.lang.String

这是mybatis 的一个bug,原因是 日期 不能和 空串 比较

```xml
<if test="createTime !=  null">
    CREATE_TIME = #{createTime},
</if>
```



错误示例,看不同 之处

```xml
<if test="createTime != '' and createTime !=  null">
    CREATE_TIME = #{createTime},
</if>
```





#### 30. springboot 集成springboot data redis出错：

```java
org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'org.springframework.data.redis.connection.RedisConnectionFactory' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.raiseNoMatchingBeanFound(DefaultListableBeanFactory.java:1493) ~[spring-beans-4.3.14.RELEASE.jar:4.3.14.RELEASE]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1104) ~[spring-beans-4.3.14.RELEASE.jar:4.3.14.RELEASE]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1066) ~[spring-beans-4.3.14.RELEASE.jar:4.3.14.RELEASE]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:585) ~[spring-beans-4.3.14.RELEASE.jar:4.3.14.RELEASE]
	at org.springframework.beans.factory.annotation.InjectionMetadata.inject(InjectionMetadata.java:88) ~[spring-beans-4.3.14.RELEASE.jar:4.3.14.RELEASE]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor.postProcessPropertyValues(AutowiredAnnotationBeanPostProcessor.java:366) ~[spring-beans-4.3.14.RELEASE.jar:4.3.14.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1264) ~[spring-beans-4.3.14.RELEASE.jar:4.3.14.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:553) ~[spring-beans-4.3.14.RELEASE.jar:4.3.14.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:483) ~[spring-beans-4.3.14.RELEASE.jar:4.3.14.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory$1.getObject(AbstractBeanFactory.java:306) ~[spring-beans-4.3.14.RELEASE.jar:4.3.14.RELEASE]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:230) ~[spring-beans-4.3.14.RELEASE.jar:4.3.14.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:302) ~[spring-beans-4.3.14.RELEASE.jar:4.3.14.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:197) ~[spring-beans-4.3.14.RELEASE.jar:4.3.14.RELEASE]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:761) ~[spring-beans-4.3.14.RELEASE.jar:4.3.14.RELEASE]
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:867) ~[spring-context-4.3.14.RELEASE.jar:4.3.14.RELEASE]
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:543) ~[spring-context-4.3.14.RELEASE.jar:4.3.14.RELEASE]
	at org.springframework.boot.context.embedded.EmbeddedWebApplicationContext.refresh(EmbeddedWebApplicationContext.java:122) ~[spring-boot-1.5.10.RELEASE.jar:1.5.10.RELEASE]
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:693) [spring-boot-1.5.10.RELEASE.jar:1.5.10.RELEASE]
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:360) [spring-boot-1.5.10.RELEASE.jar:1.5.10.RELEASE]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:303) [spring-boot-1.5.10.RELEASE.jar:1.5.10.RELEASE]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1118) [spring-boot-1.5.10.RELEASE.jar:1.5.10.RELEASE]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1107) [spring-boot-1.5.10.RELEASE.jar:1.5.10.RELEASE]
	at com.yingda.xsignal.auth.AuthApplication.main(AuthApplication.java:12) [classes/:na]
 
2018-05-30 17:25:00.027 ERROR 16084 --- [           main] o.s.b.d.LoggingFailureAnalysisReporter   : 
 
***************************
APPLICATION FAILED TO START
***************************
 
Description:
 
Field redisConnectionFactory in com.yingda.xsignal.auth.config.AuthorizationServerConfig required a bean of type 'org.springframework.data.redis.connection.RedisConnectionFactory' that could not be found.
 
 
Action:
 
Consider defining a bean of type 'org.springframework.data.redis.connection.RedisConnectionFactory' in your configuration.
```



原因：我们在pom.xml中引入了spring-boot-starter-data-redis却没有引入redis.client

增加redis client依赖即可



#### 31. 程序启动时,不连接数据库

```java
Description:
 
Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.
//无法配置数据库，没有指定url属性，并且无法配置embedded datasource
Reason: Failed to determine a suitable driver class
//原因：无法明确指定正确的驱动类（driver.class）
 
Action:
 
Consider the following:
    If you want an embedded database (H2, HSQL or Derby), please put it on the classpath.
    If you have database settings to be loaded from a particular profile you may need to activate it (no profiles are currently active).
 
//建议：
//如果如果需要加载嵌入式的数据库，请将他放入路径中
//如果有数据库设置需要从指定配置文件中加载，需要调用该配置文件（目前没有活动的配置文件）
 
Process finished with exit code 1
```



解决:

```java
程序入口处:
 
@SpringBootApplication
public class DemoApplication {
 
修改为:
@SpringBootApplication(exclude = DataSourceAutoConfiguration.class)
public class DemoApplication {
```



#### 32. springboot 项目 版本 修改后 ,spring-cloud 版本也要做修改,不然不匹配

版本对应关系,参照如下网址:

https://blog.csdn.net/qq_33407429/article/details/104494109





#### 33. .properties 与 .yml  文件的区别

 



#### 34. application.properties

springmvc mybatis 和 springboot mybatis 在配置文件中的写法不一样;

springmvc 是 :

```xml
mybatis.mapper-locations=classpath*:wgcloud/userlogin/mapper/xml/*.xml
```

springboot 是 :

```xml
mybatis.type-aliases-package=wgcloud.userlogin.mapper
```

