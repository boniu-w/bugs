 bugs:

### 1: mapper 找不到 ==> 不使用@Repository 注解,改使用 @Mapper注解

```java
required a bean of type 'com.example.datawash.dao.TestDaoWordbook' that could not be found.
```

解决方案:

​	使用 @Mapper 注解



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



#### 24. mybatis的一个异常

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

