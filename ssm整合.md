# ssm整合

- jdk
- idea
- maven

spring和springmvc是天然集成，所以只需要解决mybatis和spring整合的问题，中间项目mybatis-spring项目进行整合。

重点整合mybatis和spring

- 由spring容器管理mybatis这个mapper
- 由spring利用声明式事务（AOP）

## 添加依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.cc</groupId>
  <artifactId>springmvc</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <name>springmvc Maven Webapp</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <!--版本制定-->
    <spring.version>5.0.8.RELEASE</spring.version>
    <jackson.version>2.9.3</jackson.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>

    <!--spring依赖start-->
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-beans -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-context-support -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context-support</artifactId>
      <version>${spring.version}</version>
    </dependency>


    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-web -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-aspects -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aspects</artifactId>
      <version>${spring.version}</version>
    </dependency>


    <!-- https://mvnrepository.com/artifact/org.springframework/spring-aop -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aop</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <!--利用它处理事务问题-->
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-tx -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>${spring.version}</version>
    </dependency>



    <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>

    <!-- https://mvnrepository.com/artifact/javax.servlet/jstl -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>


    <!--json-->
    <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>${jackson.version}</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>${jackson.version}</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-annotations -->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-annotations</artifactId>
      <version>${jackson.version}</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/net.sf.json-lib/json-lib -->
    <dependency>
      <groupId>net.sf.json-lib</groupId>
      <artifactId>json-lib</artifactId>
      <version>2.4</version>
      <classifier>jdk15</classifier>
    </dependency>

    <!--新添加处理json为javabean，start-->
    <!-- https://mvnrepository.com/artifact/org.codehaus.jackson/jackson-core-asl -->
    <dependency>
      <groupId>org.codehaus.jackson</groupId>
      <artifactId>jackson-core-asl</artifactId>
      <version>1.9.2</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.codehaus.jackson/jackson-mapper-asl -->
    <dependency>
      <groupId>org.codehaus.jackson</groupId>
      <artifactId>jackson-mapper-asl</artifactId>
      <version>1.9.2</version>
    </dependency>
    <!--新添加处理json为javabean，end-->

    <!--xml数据处理-->
    <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.dataformat/jackson-dataformat-xml -->
    <dependency>
      <groupId>com.fasterxml.jackson.dataformat</groupId>
      <artifactId>jackson-dataformat-xml</artifactId>
      <version>2.9.3</version>
    </dependency>

    <!--Apache文件上传组件，start-->
    <!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
    <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.3</version>
    </dependency>
    <!--文件上传，end-->

    <!--持久层依赖-->
    <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.4.1</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.15</version>
    </dependency>



    <!--日志，start-->
    <!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-api -->
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>1.7.12</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-log4j12 -->
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>1.7.12</version>
      <scope>test</scope>
    </dependency>

    <!-- https://mvnrepository.com/artifact/log4j/log4j -->
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>1.2.17</version>
    </dependency>
    <!--日志，end-->


    <!--数据的引入，池化技术-->
    <!-- https://mvnrepository.com/artifact/com.mchange/c3p0 -->
    <dependency>
      <groupId>com.mchange</groupId>
      <artifactId>c3p0</artifactId>
      <version>0.9.2.1</version>
    </dependency>

    <!--mybatis ，spring整合的依赖-->
    <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.0</version>
    </dependency>


    <!--servlet jsp jstL等依赖，start-->
    <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>

    <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->
    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>javax.servlet.jsp-api</artifactId>
      <version>2.3.1</version>
      <scope>provided</scope>
    </dependency>

    <!-- https://mvnrepository.com/artifact/javax.servlet.jsp.jstl/jstl -->
    <dependency>
      <groupId>javax.servlet.jsp.jstl</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>
    <!--servlet jsp jstL等依赖，end-->

    <!--处理时间日期格式-->
    <!-- https://mvnrepository.com/artifact/joda-time/joda-time -->
    <dependency>
      <groupId>joda-time</groupId>
      <artifactId>joda-time</artifactId>
      <version>2.9.9</version>
    </dependency>

    <!--mybatis分页依赖-->
    <!-- https://mvnrepository.com/artifact/com.github.pagehelper/pagehelper -->
    <dependency>
      <groupId>com.github.pagehelper</groupId>
      <artifactId>pagehelper</artifactId>
      <version>5.1.2</version>
    </dependency>

    <!--apache用于md5加密-->
    <!-- https://mvnrepository.com/artifact/commons-codec/commons-codec -->
    <dependency>
      <groupId>commons-codec</groupId>
      <artifactId>commons-codec</artifactId>
      <version>1.10</version>
    </dependency>


    <dependency>
          <groupId>org.slf4j</groupId>
          <artifactId>slf4j-nop</artifactId>
          <version>1.7.2</version>
    </dependency>
  </dependencies>

  <build>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <!-- clean lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#clean_Lifecycle -->
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- default lifecycle, jar packaging: see https://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_jar_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-jar-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
        <!-- site lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#site_Lifecycle -->
        <plugin>
          <artifactId>maven-site-plugin</artifactId>
          <version>3.7.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-project-info-reports-plugin</artifactId>
          <version>3.0.0</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>


```



## web.xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1"
         metadata-complete="true">
```

### 注册spring的上下文文件

```xml
<!--制定程序上下文信息-->
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>classpath:/spring/applicationContext.xml</param-value>
</context-param>
```

### 字符编码以及全HTTP请求支持过滤器添加

```xml
<!--过滤器-->
<filter>
  <filter-name>characterEncodingFilter</filter-name>
  <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  <!--指定字符编码-->
  <init-param>
    <param-name>encoding</param-name>
    <param-value>UTF-8</param-value>
  </init-param>
  <init-param>
    <param-name>forceRequestEncoding</param-name>
    <param-value>true</param-value>
  </init-param>
  <init-param>
    <param-name>forceResponseEncoding</param-name>
    <param-value>true</param-value>
  </init-param>
</filter>
<filter-mapping>
  <filter-name>characterEncodingFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>

<!--注册一个支持所有http请求的过滤器-->
<!--表单提交里还要添加一个隐藏参数 hidden-->
<filter>
  <filter-name>hiddenHttpMethodFilter</filter-name>
  <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
  <filter-name>hiddenHttpMethodFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```

### DispatcherServlet注册

```xml
<!--注册一个前端控制器
DispatcherServlet
-->
<servlet>
  <!--名字：
  如果不修改Spring配置文件默认的位置，那么Spring会去web-inf找叫做Spring-servlet.xml的文件-->
  <servlet-name>springMVC</servlet-name>
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  <init-param>
    <!--上下文配置的位置的制定-->
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:springmvc.xml</param-value>
  </init-param>
  <load-on-startup>1</load-on-startup>
  <!--<multipart-config>-->
  <!--<max-file-size>2000000000</max-file-size>-->
  <!--</multipart-config>-->
</servlet>

<!--servlet映射配置-->
<servlet-mapping>
  <servlet-name>springMVC</servlet-name>
  <!---->
  <!--这里统一写 /-->
  <url-pattern>/</url-pattern>
</servlet-mapping>
```

### spring启动监听器配置

```xml
<listener>
  <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

## spring的核心配置文件编写

核心配置文件用于引入其他配置文件

### applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">


    <!--
    引入spring和其他整合的配置文件
    鲁棒性
    -->
    <import resource="classpath:spring/spring-*.xml"/>

</beans>
```

### spring-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--
    启动注解
    排除了service注解
    -->
    <context:component-scan base-package="com.cc">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Service"/>
    </context:component-scan>

    <!--配置视图解析器
    常用的内部资源视图解析器
    -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--1.前缀-->
        <property name="prefix" value="/jsp/"></property>

        <!--2.后缀-->
        <property name="suffix" value=".jsp"></property>

    </bean>

    <!--mvc驱动-->
    <mvc:annotation-driven>

    </mvc:annotation-driven>

    <!--文件上传解析器
    id必须为multipartResolver，原因是源代码写死了
    -->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <!--定义最大上传大小-->
        <property name="maxUploadSize" value="1024000000"></property>
        <!--指定上传的编码-->
        <property name="defaultEncoding" value="UTF-8"></property>
        <!--单个文件最大上传大小-->
        <property name="maxUploadSizePerFile" value="2000000000"></property>
    </bean>

    <!--静态资源处理,默认的servlet处理者-->
    <mvc:default-servlet-handler/>

</beans> 
```

### spring-mybatis.xml

整合ORM功能

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                        http://www.springframework.org/schema/beans/spring-beans-3.1.xsd
                        http://www.springframework.org/schema/context
                        http://www.springframework.org/schema/context/spring-context-3.1.xsd
                        http://www.springframework.org/schema/mvc
                        http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
    <!-- 自动扫描 -->
    <context:component-scan base-package="com.cc.mapper" >
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>


    <!-- 引入数据库相关的配置文件 -->
    <context:property-placeholder location="classpath:db.properties"/>
    <bean id="dataSource"
          class="com.mchange.v2.c3p0.ComboPooledDataSource">

        <property name="driverClass" value="${jdbc.driver}" />
        <property name="jdbcUrl" value="${jdbc.url}" />
        <property name="user" value="${jdbc.username}" />
        <property name="password" value="${jdbc.password}" />

        <!--c3p0连接池私有属性-->

        <!-- 连接池最大数量-->
        <property name="maxPoolSize" value="30"></property>
        <!-- 连接池最小数量 -->
        <property name="minPoolSize" value="10"></property>
        <!--关闭连接后不自动commit-->
        <property name="autoCommitOnClose" value="false"/>
        <!--获取连接超时时间-->
        <property name="checkoutTimeout" value="100000"/>
        <!--当获取连接失败重试次数-->
        <property name="acquireRetryAttempts" value="2"/>
    </bean>

    <!-- spring和MyBatis完美整合，不需要mybatis的配置映射文件 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <!--引入mappers文件-->
        <!-- 自动扫描mapping.xml文件 ,必须在com/cc/mapper/文件夹下 -->
        <property name="mapperLocations" value="classpath:com/cc/mapper/*.xml"></property>
        <property name="configuration">
            <!---->
            <bean class="org.apache.ibatis.session.Configuration">
                <!-- 可以加入驼峰命名，其他mybatis配置也就是mybatis.cfg.xml的配置文件会转移到这里来-->
                <property name="mapUnderscoreToCamelCase" value="true"/>
            </bean>
        </property>

        <!--插件配置-->
        <property name="plugins">
            <array>
                <!--分页插件的配置，拦截器实现分页功能-->
                <bean class="com.github.pagehelper.PageInterceptor">
                    <!--如果不理解，去掉下边的配置-->
                    <property name="properties">
                        <value>
                            helperDialog=mysql
                            reasonable=true
                            supportMethodArgument=true
                            params=count=countSql
                            autoRuntimeDialect=true
                        </value>
                    </property>
                </bean>
            </array>
        </property>
    </bean>

    <!-- 持久层借口 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.cc.mapper" />
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>
    </bean>

    <!-- (事务管理)transaction manager, use JtaTransactionManager for global tx -->
    <bean id="transactionManager"
          class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource" />
    </bean>

    <!--确定事务层的策略，transaction-manager指向上边的transactionManager -->
    <tx:advice transaction-manager="transactionManager" id="transactionAdvice">
        <!-- 事务管理的相关值以及它的传播性 -->
        <tx:attributes>
            <!--查询相关配置为只读，select开头或 get 或 query-->
            <tx:method name="select*" read-only="true"/>
            <tx:method name="get*" read-only="true"/>
            <tx:method name="query*" read-only="true"/>
            <tx:method name="delete*" propagation="REQUIRED" rollback-for="Exception"/>
            <tx:method name="update*" propagation="REQUIRED" rollback-for="Exception"/>
            <tx:method name="insert*" propagation="REQUIRED" rollback-for="Exception"/>
            <tx:method name="add*" propagation="REQUIRED" rollback-for="Exception"/>
        </tx:attributes>
    </tx:advice>

<!--使用aop对事务管理的范围进行切入  明确几个点：！对哪些地方需要进行事务的管理，execution的书写，明确边界
2.使用什么策略去管理策略 我们使用了tx：advice全部书写于其中，在我们的aop的advice当中只需要去引用这个事务管理者的建议即可-->
    <aop:config>
        <aop:pointcut id="txCut" expression="execution(* com.cc.service..*.*(..))"/>
        <aop:advisor advice-ref="transactionAdvice" pointcut-ref="txCut"/>
    </aop:config>


    <!--采用注解进行事务配置，请在Service方法的实现类上面加上@Transactional注解-->
    <tx:annotation-driven transaction-manager="transactionManager"/>
</beans>
```

### Spring-context.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                        http://www.springframework.org/schema/beans/spring-beans-3.1.xsd
                        http://www.springframework.org/schema/context
                        http://www.springframework.org/schema/context/spring-context-3.1.xsd
                        http://www.springframework.org/schema/mvc
                        http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

<!--做spring的基础配置-->
    <!--1。spring容器注册-->
    <context:annotation-config></context:annotation-config>
    <!--2。自动扫描配置-->
    <context:component-scan base-package="com.cc.service"/>


    <!--3。激活aop注解方式代理-->
    <aop:aspectj-autoproxy/>


    <!--消息格式转换-->
    <bean id="con" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
        <property name="registerDefaultFormatters" value="false"/>
    </bean>
</beans>
```

### log4j.properties

```properties
log4j.rootLogger = INFO,Console,File

### 输出信息到控制抬 ###
log4j.appender.Console = org.apache.log4j.ConsoleAppender
log4j.appender.Console.Target = System.out
log4j.appender.Console.layout = org.apache.log4j.PatternLayout
log4j.appender.Console.layout.ConversionPattern = [%c]-%m%n

log4j.logger.com.cc.mapper=DEBUG

### 输出DEBUG 级别以上的日志到=E://logs/error.log ###
log4j.appender.File = org.apache.log4j.DailyRollingFileAppender
log4j.appender.File.File = Users/ss/IdeaProjects/log.log
log4j.appender.File.MaxFileSize = 10MB


### 输出ERROR 级别以上的日志到=E://logs/error.log ###
log4j.appender.File.Threshold = ALL
log4j.appender.File.layout = org.apache.log4j.PatternLayout
log4j.appender.File.layout.ConversionPattern = [%p][%d{yyyy-MM-dd HH:mm:ss}][%c]%m%n
```

### db.properties

```properties
jdbc.url=jdbc:mysql://localhost:3306/ssm
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.username=root
jdbc.password=xxxx
```

