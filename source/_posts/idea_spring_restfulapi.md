---
title: Intellij IDEA Community 上用 Spring 开发 RestfulAPi 风格的接口
year: 2016
month: 06
day: 12
---
#### 新建一个 maven 项目

![](/images/intellijideaspringrestfulapi-0.png)
点 **Next** 进入下一页
![](/images/intellijideaspringrestfulapi-1.png)
GroupId 和 ArtifactId 填完选 **Next** 。项目建成后目录结构如下。
![](/images/intellijideaspringrestfulapi-1@2x.png)
#### 配置 pom.xml
填入如下内容
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>HelloWorld</groupId>
    <artifactId>HelloWorld</artifactId>
    <version>1.0-SNAPSHOT</version>

    <packaging>war</packaging>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <java.version>1.7</java.version>
        <maven.compiler.plugin>2.3.2</maven.compiler.plugin>
        <spring.version>4.0.0.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jms</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-oxm</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>

        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.2</version>
            <type>jar</type>
        </dependency>

        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.2.2</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>${java.version}</source>
                    <target>${java.version}</target>
                </configuration>
            </plugin>
        </plugins>
        <finalName>Hello</finalName>
        <defaultGoal>install</defaultGoal>


    </build>

</project>
```

#### 新建 Controller

在 src/main/java/ 目录下新建类 ，比如是 One.java。
![](/images/intellijideaspringrestfulapi-3@2x.png)
给类 One 注解 @Controller ，然后输入下面的代码。
```java
@Controller
public class One {


    Logger logger = LoggerFactory.getLogger(One.class);


    @RequestMapping(value = "/one", method = RequestMethod.GET)

    public
    @ResponseBody
    String displayTemplateJSON() throws Exception {

        logger.info("displayTemplateJSON() method invoked.");
        Map<String, Object> dataStructure = new HashMap<>();
        Map<String, Object> data = new HashMap<>();
        Gson gson = new Gson();

// build a simple data structure
        dataStructure.put("dataStructure", getStructure());
        logger.info("displayTemplateJSON(), building data structure.");

// convert Map to JSON
        String json = gson.toJson(dataStructure, Map.class);
        logger.info("displayTemplateJSON(), converting data structure to JSON.");

        if (json != null) {
            logger.info("displayTemplateJSON(), returning data structure as JSON.");
            return json;
        }

        return "\"{ \"No data foundl.\"}\"";
    }

    private Map<String, Object> getStructure() {
        Map<String, Object> data = new HashMap<>();
        data.put("Task", "Learn Spring Rest Web Services.");
        data.put("Company", "Example Inc.");

        List<String> requestTypes = new ArrayList<>();
        requestTypes.add("RequestTypeOne");
        requestTypes.add("RequestTypeTwo");
        requestTypes.add("RequestTypeThree");
        requestTypes.add("RequestTypeFour");
        requestTypes.add("RequestTypeFive");
        requestTypes.add("RequestTypeSix");
        requestTypes.add("RequestTypeSeven");
        requestTypes.add("RequestTypeEight");
        data.put("RequestTypes", requestTypes);

        logger.info("displayTemplateJSON(), data structure is: {}", data);
        return data;
    }
}

```

为了确保 Spring 知道你的 MVC Controller  ，还需要新建 类 WebConfg ，加上注解  @Configuration、@EnableWebMvc 、 @ComponentScan，如下图。
![](/images/intellijideaspringrestfulapi-4@2x.png)
#### 添加日志输出工具
为了能输出日志，在 ** src/main/resources** 下新建文件 **log4j.properties**
![](/images/intellijideaspringrestfulapi-5@2x.png)
其中代码如下
```
# Root logger option
log4j.rootLogger=INFO, stdout

# Direct log messages to stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n

```
#### 配置 web.xml
还需要在  ** src/main/webapp/WEB-INF/** 下新建 **web.xml **配置文件，如下图
![](/images/intellijideaspringrestfulapi-6@2x.png)
其中代码如下
```xml
<?xml version="1.0" ?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">

    <display-name>Web Application</display-name>
    <servlet>
        <servlet-name>myServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextClass</param-name>
            <param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
        </init-param>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>com.frankfancode.helloworld.WebConfig</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>myServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>

```
#### 添加 tomcat 的支持
现在 Spring 项目搭建完成了。IDEA 的 社区版并不能像 收费版那样方便的运行这个项目，还需要手动配置一下。先在 **pom.xml** 中配置支持 tomcat ，如下图。
![](/images/intellijidea06.png)
代码如下
```xml
<plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.2</version>
                <configuration>
                    <path>/HelloWorld</path>
                </configuration>
            </plugin>
```

然后进行运行时的配置，先 点击 **Edit Configurations**，点击 **+** ，再点 **Maven**，在**Command Line** 中输入 **tomcat7:run**。配置如下图 ![](/images/QQ20160618-0.png)
#### 成功运行
回到主页面，点击 运行，然后看到 输出的日志，如下图
![](/images/intellijideaspringrestfulapi-8@2x.png)
点击其中的 网址，然后加上 One 中 @RequestMapping 注解的 **one**，注意这里大小写敏感。看到下图说明配置成功了。
![](/images/QQ20160618-3.png)





参考文章

1. https://dzone.com/articles/hello-world-restful-web
1. http://stackoverflow.com/a/25110179/3587658
