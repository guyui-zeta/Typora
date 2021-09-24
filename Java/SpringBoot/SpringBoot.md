---

---

# 一、SpringBoot入门

## 1.SpringBoot简介

SpringBoot 来简化Spring应用开发，约定大于配置，去繁从简，justrun就能创建一个独立的，产品级别的应用

**背景：**

J2EE笨重的开发、繁多的配置、低下的开发效率、复杂的部署流程、第三方技术集成难度大

**解决：**

“Spring全家桶”时代。

Spring Boot J2EE一站式解决方案

Spring Cloud 分布式整体解决方案

![image-20210903192951623](C:\Users\Zeta\AppData\Roaming\Typora\typora-user-images\image-20210903192951623.png)

**优点：**

​	–快速创建独立运行的Spring项目以及与主流框架集成

​	–使用嵌入式的Servlet容器，应用无需打成WAR包，直接打包成jar包，java -jar运行【ssm需要打包war包，再部署】

​	–starters【启动器】自动依赖与版本控制【一个功能一个starters】

​	–大量的自动配置，简化开发，也可修改默认值【用户只需要从一个微小的入口进入即可，sb会自动配置】

​	–无需配置XML，无代码生成，开箱即用【自动配置】

​	–准生产环境的运行时应用监控

​	–与云计算的天然集成



**缺点：**

入门容易，精通难

## 2.微服务简介

​	微服务是一种架构风格，一个应用应该是一组小型服务。每一个服务都运行在自己的进程中，通过HTTP的方式进行互通

### 2.1单体应用

​	传统的web应用架构，优点：开发简单，部署简单，扩展简单。缺点：牵一发而动全身【小小的修改，要重新运行】，现在需求多了都是大型应用，不能全部写在一个服务器里面

![image-20210903193122889](C:\Users\Zeta\AppData\Roaming\Typora\typora-user-images\image-20210903193122889.png)

### 2.2微服务

​	把每个功能元素放进一个独立的服务中，通过功能元素的动态组合，有需要的时候再进行复制，这样每一个服务都是可替换的、可升级的功能单元

![image-20210903193312812](C:\Users\Zeta\AppData\Roaming\Typora\typora-user-images\image-20210903193312812.png)

### 2.3微服务架构的样子

​	通过http的方式互联互调，进行轻量级通信，组成应用网。【分布式应用】每一个功能单元都是完整的功能单元

![image-20210903193342356](C:\Users\Zeta\AppData\Roaming\Typora\typora-user-images\image-20210903193342356.png)

### 2.4Your app搭建流程

​	1.SpirngBoot快速构建应用

​	2.SpringCloud实现网状分布式呼叫

​	3.SpringCloudDataFolw实现流式数据计算

![image-20210903193448681](C:\Users\Zeta\AppData\Roaming\Typora\typora-user-images\image-20210903193448681.png)

**环境约束：【统一环境】**

环境约束

–jdk1.8：Spring Boot 推荐jdk1.7及以上；java version "1.8.0_112"

–maven3.x：maven 3.3以上版本；Apache Maven 3.3.9

–IntelliJIDEA2017：IntelliJ IDEA 2017.2.2 x64、STS

–SpringBoot 1.5.9.RELEASE：1.5.9；

 

Maven设置

Idea设置

## 3.SpringBoot HelloWorld初试

**一个功能：**

浏览器发送hello请求，服务器接受请求并处理，响应Hello World字符串；

**步骤：**

​	1、创建一个maven工程；（jar）

​	2、导入spring boot相关的依赖

​	Spirng-boot-starter-web内置了Tomcat服务器的依赖

```java	
	<!--父项目-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.9.RELEASE</version>
    </parent>
    <!--启动器依赖-->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

```

​	3.编写一个主程序，启动springboot应用

```java
/**
 * @SpringBootApplication   来标注一个主程序类，说明这是一个SpringBoot应用
 */
@SpringBootApplication
public class SpringBoot02Application {
    public static void main(String[] args) {
        SpringApplication.run(SpringBoot02Application.class, args);
    }
}
```

​	4.编写相关的Controller、Service

```java
@Controller
public class helloController {
    @ResponseBody
    @RequestMapping("/hello")
    public String hello(){
        return "helloWorld!";
    }
}
```

​	5、运行主程序测试

​	6、简化部署

​		【以前的SSM需要把应用打包成war包，然后部署到Tomcat上运行，sb完全不用，**可以直接打成jar包，部署**】

```java
<!--打包成jar包-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

在maven中的package中运行打包

将这个应用打成jar包，直接使用java -jar的命令进行执行；【即时没有tomcat，也可以运行，因为web依赖自带了tomcat】

![image-20210903194221504](C:\Users\Zeta\AppData\Roaming\Typora\typora-user-images\image-20210903194221504.png)

## 4.HelloWorld探究

### 4.1 POM文件

```java
	<!--父项目-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.9.RELEASE</version>
    </parent>
    <!--启动器依赖-->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

### 4.1.1父项目

```java
 	<!--父项目-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.9.RELEASE</version>
    </parent>
```

↑他还有个父项目

```java
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-dependencies</artifactId>
		<version>1.5.9.RELEASE</version>
		<relativePath>../../spring-boot-dependencies</relativePath>
	</parent>
```

他来管理SpringBoot应用里面所有的依赖版本号：【SpringBoot的版本仲裁中心】

**以后我们导入依赖是不需要写版本的**

![image-20210903195006438](https://i.loli.net/2021/09/09/c24ILmoKD9AQyFs.png)

### 4.1.2导入的依赖

```java
	<!--启动器依赖-->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

**Spring-boot-starter-**web :

​	**Spring-boot-starter：**spring-boot场景启动器；帮我们导入了Web模块正常运行依赖的组件【Tomcat、Spring、SpirngMVC】

![image-20210903195203540](https://i.loli.net/2021/09/09/CcnBVeKf5rUSNWz.png)

​	SpringBoot将所有的功能场景都抽取出来，做成一个个的Starters（启动器），只需要在项目里面引入这些Starter相关场景的所有依赖都会导入进来。要用什么功能，就导入什么场景的启动器

### 4.2主程序类、主入口类

```java
/**
 * @SpringBootApplication   来标注一个主程序类，说明这是一个SpringBoot应用
 */
@SpringBootApplication
public class SpringBoot02Application {
    public static void main(String[] args) {
        SpringApplication.run(SpringBoot02Application.class, args);
    }
}
```

**@SpringBootApplication：**SpringBoot应用标注在某个类上，说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；

是一个组合注解

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
```

**1.@SpringBootConfiguration:**

​		Spring Boot的配置类；【SpirngBoot的配置类注解】标注在某个类上，表示这是一个Spring Boot的配置类；

​	**@Configuration**:

​		配置类上来标注这个注解；【Spring的配置类注解】配置类 ----- 配置文件；配置类也是容器中的一个组件；@Component

**2.@EnableAutoConfiguration：**

​		以前我们需要配置的东西，Spring Boot帮我们自动配置；@EnableAutoConfiguration告诉SpringBoot开启自动配置功能；这样自动配置才能生效；

​		也是一个组合注解，他有下面两个注解组合而成

```java
@AutoConfigurationPackage
@Import({EnableAutoConfigurationImportSelector.class})
public@interfaceEnableAutoConfiguration {

```

**@AutoConfigurationPackage：自动配置包**

**@Import({Registrar.class})：**

​	属于Spring的底层注解@Import，给容器中导入一个组件里倒入的组件由Registrar.class

**作用：**==将主配置类（@SpringBootApplication标注的类）的所在包及下面所有子包里面的所有组件扫描到Spring容器中【所以能扫描到Controller，ssm需要配置扫描包的路径】==

**【所以，Starter类必须在主包下，不然无法扫描到其他包的组件！！】**

将所有需要导入的组件以全类名的方式返回；这些组件就会被添加到容器中；

**作用：**会给容器中导入非常多的自动配置类，就是给容器中导入这个场景需要的所有的组件，并配置好这些组件。

![image-20210903203132762](C:\Users\Zeta\AppData\Roaming\Typora\typora-user-images\image-20210903203132762.png)

有了自动配置类，免去了我们手动编写配置和注入功能组件等的工作

**总结：**【自动配置类重要-以后需要补充理解】

​	SpringBoot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值，将这些值作为自动配置类导入到容器中，自动配置类就生效了，帮我们进行自动配置工作。【以前我们需要自己配置的东西，自动配置类都帮我们做了】

J2EE的整体整合解决方案和自动配置都在下面这个spring-boot-autoconfigure-1.5.9.RELEASE.jar包里面

![image-20210903203221393](C:\Users\Zeta\AppData\Roaming\Typora\typora-user-images\image-20210903203221393.png)

## 5.使用Spring Initializer快速创建SpringBoot项目

**1、IDEA：使用 Spring Initializer快速创建项目**

​	IDE都支持使用Spring的项目创建向导快速创建一个Spring Boot项目；选择我们需要的模块；向导会联网创建Spring Boot项目；默认生成的Spring Boot项目；

【会自动导入sb的一些相关的依赖：如单元测试】

![image-20210903203446635](C:\Users\Zeta\AppData\Roaming\Typora\typora-user-images\image-20210903203446635.png)

![image-20210903203455476](C:\Users\Zeta\AppData\Roaming\Typora\typora-user-images\image-20210903203455476.png)

**resources文件夹中目录结构**

​	static：保存所有的静态资源：js、css、images

​	templates：保存所有的末班页面（sb莫仍jar包使用嵌入式的Tomcat，默认不支持JSP页面）（可以使用末班引擎FreeMarker）

​	Application.properties：Springboot应用的配置文件



# 二、SpringBoot配置

## 1.配置文件

SpringBoot使用一个全局的配置文件，配置文件名是固定的；

![image-20210903203814622](C:\Users\Zeta\AppData\Roaming\Typora\typora-user-images\image-20210903203814622.png)

•application.properties

•application.yml

**配置文件的作用:**

​	修改SpringBoot自动配置的默认值；SpringBoot在底层都给我们配置好

### 1.1YAML

**YAML（YAML Ain't Markup Language）**

​	YAML A Markup Language：是一个标记语言

​	YAML isn't Markup Language：不是一个标记语言；

**标记语言：**

​	以前的配置文件；大多都使用的是 xxxx.xml文件；

​	YAML：**以数据为中心**，比json、xml等更适合做配置文件；

**YAML配置例子：**

```yaml
server:
  port: 8081
```

**properties配置例子:**

````properties
server.port=8081
````

**XML配置例子：**

```xml
<server>
	<port>8081</port>
<server>
```

## 2.YML语法

### 1.基本语法

k:(空格)v：表示一对键值对（空格必须有）；

以空格的缩进来控制层级关系；只要是左对齐的一列数据，都是同一个层级的

```yaml
server:
  port: 8081
```

### 2.值的写法

#### ①字面量：普通的值（数字，字符串，布尔）

k: v：字面直接来写；

​	字符串默认不用加上单引号或者双引号；

​	**""：双引号；**会转义字符串里面的特殊字符；特殊字符会作为本身想表示的意思

name: "zhangsan \n lisi"：输出；zhangsan 换行 lisi

​	**''：单引号；**不会转义特殊字符，特殊字符最终只是一个普通的字符串数据

name: ‘zhangsan \n lisi’：输出；zhangsan \n lisi

#### ②对象、Map（属性和值）（键值对）：

k: v：在下一行来写对象的属性和值的关系；注意缩进

​	对象还是k: v的方式

```yaml
friends:
    lastName: zhangsan
    age: 20
```

行内写法：

```yaml
friends: {lastName: zhangsan,age: 18}
```

#### ③数组（List、Set）：

用- 值表示数组中的一个元素

```yaml
pets:
    ‐ cat
    ‐ dog
    ‐ pig
```

行内写法：

```yaml
pets: [cat,dog,pig]
```

## 3.实际使用：

### 1.yml配置类属性

![image-20210903211600562](C:\Users\Zeta\AppData\Roaming\Typora\typora-user-images\image-20210903211600562.png)

### 2.将配置文件中配置的每一个属性的值，映射到这个组件中

```java
/**
 * 将配置文件中配置的每一个属性的值，映射到这个组件中
 *
 * @ConfigurationProperties：告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定
 *      prefix = "person":配置文件中哪个下面的所有属性进行一一映射
 *      只有这个组件是容器中的组件，才能容器提供的@ConfigurationProperties功能；
 */
@Component
@ConfigurationProperties(prefix = "person")
public class person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;
```

出现报错：需要导入依赖，配置文件处理器

![image-20210903211714220](C:\Users\Zeta\AppData\Roaming\Typora\typora-user-images\image-20210903211714220.png)

导入依赖：配置文件处理器

```xml
<dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-configuration-processor</artifactId>
        <optional>true</optional>
</dependency>
```

### 3.单元测试

```java
/**
 * sb单元测试
 *  可以在测试期间很方便的类似编码一样进行自动注入容器的功能
 */
@SpringBootTest
class SpringBoot02ApplicationTests {
    @Autowired
    person person;
    @Test
    void contextLoads() {
        System.out.println(person);
    }
}
```

## 4.配置文件值注入

```properties
#idea使用的时utf-8，而properties默认时GBK，要该file encoding
#配置person的值
person.age=18
person.last-name=张三
person.birth=2017/1/1
person.boss=false
person.maps.k1=v1
person.maps.k2=14
person.lists=a,b,c
person.dog.name=dog
person.dog.age=15
```
### 1.properties乱码问题

会出现乱码，因为idea使用的时uft-8编码，而properties默认是GBK，所以需要在setting里修改编译时转换为ASCII码

![image-20210909134847061](D:\Typora\Typora_Note\Java\SpringBoot\SpringBoot.assets\image-20210909134847061.png)

### 2.@Value获取值和@ConfigurationProperties获取值比较

还可以通过@Value("person.last-name")的注解方式赋值
注意：就不需要@ConfigurationProperties(prefix = "person")注解来扫描配置文件里的前缀了

```java
@Component
//@ConfigurationProperties(prefix = "person")
public class person {
    /**
     * 等同于
     * <bean class="person">
     *     <property name="lastName" value="?"></property>
     * </bean>
     */
    @Value("${person.last-name}")
    private String lastName;
    @Value("#{11*2}")
    private Integer age;
    @Value("true")
    private Boolean boss;
```
| 一个普通标题         | @ConfigurationProperties                                     | @Vlue                                       |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------- |
| 实现方式             | @ConfigurationProperties(prefix = "person")扫描配置  文件中的类前缀 | @Value("${person.last-name}")一个个指定注入 |
| 功能                 | 批量注入配置文件中的属性                                     | 一个个指定                                  |
| 松散绑定（松散语法） | 支持【lastName=last-name】                                   | 不支持                                      |
| SpEL【EL表达式】     | 不支持                                                       | 支持EL表达式【#{11*2}】                     |
| JSR303数据校验       | 支持【加上@Validated来校验一些数据的格式，如@Email来校验邮箱格式】 | 不支持                                      |

如果说，我们只是在某个业务逻辑中需要获取一下配置文件中的某项值，**使用@Value**；  
```java
@Controller
public class helloController {
    @Value("${person.last-name}")
    private String name;
    
    @ResponseBody
    @RequestMapping("/sayHello")
    public String hello(){
        return "helloWorld!" + name;
    }
}
```
如果说，我们专门编写了一个javaBean【实体类】来和配置文件进行映射，我们就直接**使用@ConfigurationProperties**；
```java
@Component
@ConfigurationProperties(prefix = "person")
public class person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;
```

### 3.配置文件注入值数据校验

必须要用@ConfigurationProperties才可以
```java
@Component
@ConfigurationProperties(prefix = "person")
@Validated
public class person {
//    @Value("${person.last-name}")
    @Email
    private String lastName;
```

### 4.@PropertySource&@ImportResource

**@PropertySource:** 加载指定的配置文件【不像@ConfigurationProperties()加载的是全局配置文件application.properties】
```java
@Component
@PropertySource(value = {"classpath:person.properties"})
@ConfigurationProperties(prefix = "person")
public class person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;
```
**@ImportResource:** 导入Spring的配置文件，让配置文件里面的内容生效  
先举例【利用spring的配置文件，添加组件进容器】  

**1.添加自定义的spring配置文件beans.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="helloService" class="com.zeta.springboot02.service.HelloService"></bean>
</beans>
```

**2.测试容器中是否包含该组件【结果是无】**
```java
/**
     * 测试一下，容器中是否包含helloService
     */
    @Test
    public void testHelloService(){
        boolean b = ioc.containsBean("helloService");
        System.out.println(b);
    }
```

**结论：**
SpringBoot里面没有Spring的配置文件，我们自己编写的配置文件，也不能自动识别；
想让Spring的配置文件生效，加载进来；需要 **@ImportResource** 标注在一个类上

## 5.SpringBoot推荐给容器中添加组件的方式：推荐使用全注解的方式

### 1.Spring配置文件.xml

| 区别           | 配置文件                                  | 配置类                                                       |
| -------------- | ----------------------------------------- | ------------------------------------------------------------ |
| 添加组件的方式 | 在配置文件汇中用<bean></bean>标签添加组件 | 在配置类中用@Bean来添加组件                                  |
| 实现           | <bean></bean>                             | @Bean【@Bean作用：将方法的返回值添加到容器中，容器中这个组件的默认Id就是方法名】 |

### 2.@Bean给容器中添加组件
```java
/**
 * @Configuration指明当前类是一个配置类；就是来替代之前的Spring配置文件
 * 在配置文件汇中用<bean></bean>标签添加组件
 * 在配置类中用@Bean来添加组件
 */
@Configuration
public class MyAppConfig {
    //@Bean作用：将方法的返回值添加到容器中，容器中这个组件的默认Id就是方法名
    @Bean
    public HelloService helloService(){
        System.out.println("给容器中添加组件了");
        return new HelloService();
    }
}
```
## 5.配置文件的占位符

### 1、随机数

```java
${random.value}、${random.int}、${random.long}
${random.int(10)}、${random.int[1024,65536]}
```
### 2、占位符获取之前配置的值，如果没有可以是用:指定默认值

```java
person.last‐name=张三${random.uuid}
person.age=${random.int}
person.birth=2017/12/15
person.boss=false
person.maps.k1=v1
person.maps.k2=14
person.lists=a,b,c
person.dog.name=${person.hello:hello}_dog
person.dog.age=15
```
## 6.Profile

【开发、测试、生产】
Profile是Spring对不同环境提供不同配置功能的支持，可以通过激活、指定参数等方式快速切换环境

### 1.多Profile文件

我们在主配置文件编写的时候，文件名可以是 applicaiton-{profile}.properties/yml，然后就可以动态切换。
默认使用的是application.properties的配置文件；

### 2.yml支持多文档快方式

在yml中使用---会分割成文档块
```java
server:
  port: 8081
spring:
  profiles:
    active: dev
---
server:
  port: 8083
spring:
  config:
    activate:
      on-profile: dev
---
server:
  port: 8084
spring:
  config:
    activate:
      on-profile: prod
```

### 3.激活指定的profile

#### 1.在配置文件中指定Spring.profiles.active=dev【properties文件中】

```properties
spring.profiles.active=dev
```

#### 2.命令行【在editConfiguration中配置】：

```java
--spring.profiles.actice=dev
```

![image-20210909135000740](D:\Typora\Typora_Note\Java\SpringBoot\SpringBoot.assets\image-20210909135000740.png)

同理，打包成jar包后在cmd中加上后缀命令行也可以  
java -jar spring-boot-02-config-0.0.1-SNAPSHOT.jar --spring.profiles.active=dev；

![image-20210909135110987](D:\Typora\Typora_Note\Java\SpringBoot\SpringBoot.assets\image-20210909135110987.png)

最好在测试的时候，配置传入命令参数

#### 3.虚拟机参数

-Dspring.profiles.active=dev

![image-20210909135134257](D:\Typora\Typora_Note\Java\SpringBoot\SpringBoot.assets\image-20210909135134257.png)

## 7.配置文件加载位置

SpringBoot启动会扫描以下位置的application.properties或者application.yml文件作为SpringBoot的默认配置文件

**–file:./config/**   //最优先  
**–file:./**  
**–classpath:/config/**  
**–classpath:/**  
优先级由高到底，高优先级的配置会覆盖低优先级的配置；高优先级配置内容会覆盖低优先级配置内容
SpringBoot会从这四个位置全部加载主配置文件；互补配置；  
【运维小技巧】
项目打包成功后，需要修改一些配置，只需要编写一些少量的配置，加载新改的配置就可以  
**具体步骤：**  
我们还可以通过spring.config.location来改变默认的配置文件位置
项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；指定配置文件和默认加载的这些配置文件共同起作用形成互补配置；  
java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar--spring.config.location=G:/application.properties

## 8.外部配置加载顺序

**SpringBoot也可以从以下位置加载配置；优先级从高到低；高优先级的配置覆盖低优先级的配置，所有的配置会形成互补配置**

1.命令行参数  
所有的配置都可以在命令行上进行指定  
java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --server.port=8087   --server.context-path=/abc  
**多个配置用空格分开； --配置项=值**
2.来自java:comp/env的JNDI属性  
3.Java系统属性（System.getProperties()）  
4.操作系统环境变量  
5.RandomValuePropertySource配置的random.*属性值  
==**由jar包外向jar包内进行寻找，jar包外的优先级大于jar包内的配置**==

==**优先加载带profile**==  
6.jar包外部的application-{profile}.properties或application.yml(带spring.profile)配置文件  
7.jar包内部的application-{profile}.properties或application.yml(带spring.profile)配置文件  
==**再来加载不带profile**==  
8.jar包外部的application.properties或application.yml(不带spring.profile)配置文件  
9.jar包内部的application.properties或application.yml(不带spring.profile)配置文件  
10.@Configuration注解类上的@PropertySource  
11.通过SpringApplication.setDefaultProperties指定的默认属性  
所有支持的配置加载来源；  

## 9.自动配置原理

### 自动配置定义

#### 手动配置：

当通过@Autowired或者@Resource注解，自动注入一个类实例之前，被注入进来的这个类实例需要被Spring容器接纳管理，否则注入失败。往往需要在xml中通过 ```<bean id=""></bean>```  或者在类定义上使用```@Component、@Configuration、@Controller、@Service```等注解，来实现其被Spring容器管理。

#### 自动配置：

而SpringBoot的自动配置功能，会对我们配置的一些类，自动注入到Spring容器中，特别对于依赖的jar包中的一些类，在工程中，直接@AutoWired和@Resource注解注入即可【使用@EnableAutoConfiguration注解，会启用自动配置】




配置文件能些什么？怎么写？参照官方文档  
https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#common-application-properties  

### 原理源码逻辑

==**starter启动器注解**==
```java
@SpringBootApplication
public class SpringBoot02ConfigAutoconfigApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBoot02ConfigAutoconfigApplication.class, args);
    }
}
```
==**@SpringBootApplication组合注解中重要的两个注解**==
```java
@SpringBootConfiguration
@EnableAutoConfiguration
```
==**@EnableAutoConfiguration中的选择器**==
```java
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
```
==**选择器AutoConfigurationImportSelector的getAutoConfigurationEntry方法**==
```java
protected AutoConfigurationImportSelector.AutoConfigurationEntry getAutoConfigurationEntry(AnnotationMetadata annotationMetadata) {
        if (!this.isEnabled(annotationMetadata)) {
            return EMPTY_ENTRY;
        } else {
            AnnotationAttributes attributes = this.getAttributes(annotationMetadata);
            //获取候选的配置
            List<String> configurations = this.getCandidateConfigurations(annotationMetadata, attributes);
            
            configurations = this.removeDuplicates(configurations);
            Set<String> exclusions = this.getExclusions(annotationMetadata, attributes);
            this.checkExcludedClasses(configurations, exclusions);
            configurations.removeAll(exclusions);
            configurations = this.getConfigurationClassFilter().filter(configurations);
            this.fireAutoConfigurationImportEvents(configurations, exclusions);
            return new AutoConfigurationImportSelector.AutoConfigurationEntry(configurations, exclusions);
        }
    }
```
==**getCandidateConfigurations方法获取候选的配置**==
```java
    protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
        //通过loadFactoryNames 
        List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());
        Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
        return configurations;
    }
    

```
==**loadFactoryNames方法从类路径下得到资源,扫描"META-INF/spring.factories"**==
```java
 public static List<String> loadFactoryNames(Class<?> factoryType, @Nullable ClassLoader classLoader) {
        ClassLoader classLoaderToUse = classLoader;
        if (classLoader == null) {
            classLoaderToUse = SpringFactoriesLoader.class.getClassLoader();
        }

        String factoryTypeName = factoryType.getName();
        return (List)loadSpringFactories(classLoaderToUse).getOrDefault(factoryTypeName, Collections.emptyList());
    }
private static Map<String, List<String>> loadSpringFactories(ClassLoader classLoader) {
    Map<String, List<String>> result = (Map)cache.get(classLoader);
    if (result != null) {
        return result;
    } else {
        HashMap result = new HashMap();

        try {
            Enumeration urls = classLoader.getResources("META-INF/spring.factories");
            while(urls.hasMoreElements()) {
            //遍历url成一个properties
                URL url = (URL)urls.nextElement();
                UrlResource resource = new UrlResource(url);
                Properties properties = PropertiesLoaderUtils.loadProperties(resource);
                Iterator var6 = properties.entrySet().iterator();
```
### 1.自动配置原理

#### 1）SpringBoot启动的时候加载主配置类，开启了自动配置功能@EnableAutoConfiguration

#### 2）EnableAutoConfiguration作用：

-   利用AutoConfigurationImportSelector选择器来给Spring容器中导入一些组件  
-   可以查看选择器中的selectImports（）方法【sb2版本是getAutoConfigurationEntry】的内容,通过
   List<String> configurations = this.getCandidateConfigurations(annotationMetadata, attributes);
   来获取候选的配置
-   SpringFactoriesLoader.loadFactoryNames()  
    **扫描所有jar包类路径下的  "META-INF/spring.factories",把扫描到的这些文件的内容包装成properties对象，再从properties中获取到EnableAutoConfiguration.class类（类名）对应的值，然后把他们添加到容器当中**

==**总结：**==
**将类路径下META-INF/spring.factories中配置的所有EnableAutoConfiguration的值加入到了容器当中**
```xml
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
org.springframework.boot.autoconfigure.cassandra.CassandraAutoConfiguration,\
org.springframework.boot.autoconfigure.context.ConfigurationPropertiesAutoConfiguration,\
org.springframework.boot.autoconfigure.context.LifecycleAutoConfiguration,\
org.springframework.boot.autoconfigure.context.MessageSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.context.PropertyPlaceholderAutoConfiguration,\
org.springframework.boot.autoconfigure.couchbase.CouchbaseAutoConfiguration,\
org.springframework.boot.autoconfigure.dao.PersistenceExceptionTranslationAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ReactiveElasticsearchRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ReactiveElasticsearchRestClientAutoConfiguration,\
org.springframework.boot.autoconfigure.data.jdbc.JdbcRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.jpa.JpaRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.ldap.LdapRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.neo4j.Neo4jDataAutoConfiguration,\
```

==**每一个这样的xxxAutoConfiguration类都是容器中的一个组件，都加入到容器中；用他们来做自动配置**==

#### 3）每一个自动配置类进行自动配置功能

#### 4）以HttpEncodingConfiguration（HTTP编码自动配置）为例解释自动配置原理

```java
@Configuration(   //表示这是一个配置类，同配置文件，可以给容器中添加组件
    proxyBeanMethods = false
)
@EnableConfigurationProperties({ServerProperties.class})//启用指定类的ConfigurationProperties功能；将配置文件中的值和ServerProperties绑定起来
@ConditionalOnWebApplication(//Spring底层@Conditional注解，根据不同条件，如果满足指定条件，整个配置类的配置才会生效；判断当前注解是否为web应用
    type = Type.SERVLET
)
@ConditionalOnClass({CharacterEncodingFilter.class})//判断当前项目有没有这个类CharacterEncodingFilter；MVC中解决乱码的过滤器
@ConditionalOnProperty(//配置文件中是否存在某个配置，默认为true
    prefix = "server.servlet.encoding",
    value = {"enabled"},
    matchIfMissing = true
)
public class HttpEncodingAutoConfiguration {
    //已经和springboot的配置文件映射了
    private final Encoding properties;
    //只有一个有参构造器，会从容器中拿
    public HttpEncodingAutoConfiguration(ServerProperties properties) {
        this.properties = properties.getServlet().getEncoding();
    }
    @Bean//给容器中添加组件
    @ConditionalOnMissingBean
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
        filter.setEncoding(this.properties.getCharset().name());
        filter.setForceRequestEncoding(this.properties.shouldForce(org.springframework.boot.web.servlet.server.Encoding.Type.REQUEST));
        filter.setForceResponseEncoding(this.properties.shouldForce(org.springframework.boot.web.servlet.server.Encoding.Type.RESPONSE));
        return filter;
    }
```
==**总结：**==
根据当前不同的条件判断，决定这个配置类是否生效？
一旦这个配置类生效；这个配置类就会给容器中添加各种组件；这些组件的属性是从对应的properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的。

#### 5）所有在配置文件中能配置的属性都是在xxxproperties类中封装着

**@EnableConfigurationProperties({ServerProperties.class})注解**
```java
@ConfigurationProperties(//从配置文件中获取指定的值和bean的属性进行绑定
    prefix = "server",
    ignoreUnknownFields = true
)
public class ServerProperties {
```

### SpringBoot精髓

1）、SpringBoot启动会加载大量的自动配置类   
2）、我们看我们需要的功能有没有SpringBoot默认写好的自动配置类   
3）、我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件有，我们就不需要再来配置了）
4）、给容器中自动配置类添加组件的时候，会从properties类中获取某些属性，我们就可以在配置文件中指定这些属性的值了；就像下面这样的属性就可以在properties中配置了

```java
server.port=8080
//上面的server是通过自动配置加载进来，port是从properties类中获取的属性
```
xxxAutoConfiguration：自动配置类，给容器中添加组件  
xxxProperties：封装配置文件中相关属性；

### 细节@Conditional

#### 1.@Conditional派生注解

**作用：**
必须是@Conditional指定的条件成立，才给容器中添加组件，配置类里面的所有内容才生效

| @Conditional扩展注解            | 作用（判断是否满足当前指定条件）                 |
| ------------------------------- | ------------------------------------------------ |
| @ConditionalOnJava              | 系统的java版本是否符合要求                       |
| @ConditionalOnBean              | 容器中存在指定Bean；                             |
| @ConditionalOnMissingBean       | 容器中不存在指定Bean                             |
| @ConditionalOnExpression        | 满足SpEL表达式指定                               |
| @ConditionalOnClass             | 系统中有指定的类                                 |
| @ConditionalOnMissingClass      | 系统中没有指定的类                               |
| @ConditionalOnSingleCandidate   | 容器中只有一个指定的Bean，或者这个Bean是首选Bean |
| @ConditionalOnProperty          | 系统中指定的属性是否有指定的值                   |
| @ConditionalOnResource          | 类路径下是否存在指定资源文件                     |
| @ConditionalOnWebApplication    | 当前是web环境                                    |
| @ConditionalOnNotWebApplication | 当前不是web环境                                  |
| @ConditionalOnJndi              | JNDI存在指定项                                   |

自动配置类必须在一定的条件下才能生效
我们可以通过在properties中开启 debug=true属性；来让控制台打印自动配置报告，这样我么就可以很方便的知道哪些自动配置类生效

![image-20210909135341392](D:\Typora\Typora_Note\Java\SpringBoot\SpringBoot.assets\image-20210909135341392.png)

positive matches【自动配置类启用的】  
negative matches【没启用的】

# 三、日志
## 1.日志框架

**故事：**
小张；开发一个大型系统；  
1、System.out.println("")；将关键数据打印在控制台；去掉？写在一个文件？  
2、框架来记录系统的一些运行时信息；日志框架 ； zhanglogging.jar；  
3、高大上的几个功能？异步模式？自动归档？xxxx？ zhanglogging-good.jar？  
4、将以前框架卸下来？换上新的框架，重新修改之前相关的API；zhanglogging-prefect.jar；  
5、JDBC---数据库驱动；  
写了一个统一的接口层；日志门面（日志的一个抽象层）；logging-abstract.jar；  
给项目中导入具体的日志实现就行了；我们之前的日志框架都是实现的抽象层；  
**市面上的日志框架**；  
JUL、JCL、Jboss-logging、logback、log4j、log4j2、slf4j....

| 日志门面 （日志的抽象层）                                    | 日志实现                                      |
| ------------------------------------------------------------ | --------------------------------------------- |
| JCL（Jakarta Commons Logging） SLF4j（Simple Logging Facade for Java） jboss-logging | Log4j JUL（java.util.logging、Log4j2、Logback |

左边选一个门面(抽象层)、右边来选一个实现；
日志门面【能用的】：SLF4J
日志实现：Logback【和SLF4J出自于同一作者】

SpringBoot：底层是Spring框架，Spring框架默认是用JCL。Springboot使用SLF4J和logback；

## 2.SLF4j的使用

### 1.如何在系统中使用SLF4j

以后开发的时候，日志记录方法的调用：不应该直接调用日志的实现类，而是调用日志抽象层的方法，会自动调用实现层方法【多态】
给系统里面到导入slf4j和logback的时间jar

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```
图示：

![image-20210909135501830](D:\Typora\Typora_Note\Java\SpringBoot\SpringBoot.assets\image-20210909135501830.png)

### 2.遗留问题

a(slf4j+logback):Spring(commons-logging)、Hibernate(jobss-logging)、MyBatis、xxx
其他框架的不同日志，用slf4j提供的对应的jar包替代【适配层、包装层】

![image-20210909135552454](D:\Typora\Typora_Note\Java\SpringBoot\SpringBoot.assets\image-20210909135552454.png)

**总结：**   
如何让系统中所有的日志都统一到slf4j：  
1、将系统中其他日志框架先排除出去  
2、用中间包来替代原有的日志框架  
3、我们导入slf4j其他的实现  

## 3.SpringBoot日志关系

**SpringBoot最基本的依赖：SpringBoot-Starter**
```java
<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
      <version>2.5.4</version>
      <scope>compile</scope>
    </dependency>
```
**Springboot使用它来做日志功能**

```java
<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-logging</artifactId>
      <version>2.5.4</version>
      <scope>compile</scope>
    </dependency>
```

**SpringBoot底层依赖关系**

![image-20210909135625366](D:\Typora\Typora_Note\Java\SpringBoot\SpringBoot.assets\image-20210909135625366.png)

在底层偷梁换柱，实现日志的统一  

![image-20210909135640825](D:\Typora\Typora_Note\Java\SpringBoot\SpringBoot.assets\image-20210909135640825.png)

**总结：**

-   1）SpringBoot底层也是使用slf4j+logback的方式进行日志记录
-   2）SpringBoot也把其他的日志都替换成了slf4j
-   3）中间替换包，实现不同框架之间的日志统一
-   4）如果我们要引入其他框架，一定要把这个框架的默认日志移除掉【因为包名和类名都一样】

**1.5版本在引入spring 的时候就把日志给排除了**
SpringBoot能自动适配所有的日志，而且底层使用slf4j+logback的方式记录日志，引入其他框架的时候，只需要把这个框架的依赖的日志框架排除掉

![image-20210909135704062](D:\Typora\Typora_Note\Java\SpringBoot\SpringBoot.assets\image-20210909135704062.png)

新版本的转环包的包名已经不一样了，不需要排除

## 4.日志的使用

### 1.默认配置

SpringBoot默认帮我们配置好了日志；
**注意：**导包别导错了，导slf4j的而不是原先框架的

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
```

**SpringBoot默认是info级别的日志**，可以在properties中设置

```java
logging.level.com.zeta=trace
```

```java
@SpringBootTest
class SpringBoot03LoggingApplicationTests {
    //日志记录的记录器
    Logger logger = LoggerFactory.getLogger(getClass());
    @Test
    public void test(){
        //System.out.println();
        //由低到高：trace<debug<info<warn<error
        //可以调整输出的日志级别；日志就只会在这个级别和以后的高级别生效
        logger.trace("这是trace日志");
        logger.debug("这是dubug日志");
        //springBoot默认给我们使用的是info级别的日志,root级别
        logger.info("这是info日志");
        logger.warn("这是warn日志");
        logger.error("这是error日志");
    }
}
```

Spring修改日志的默认配置

```properties
logging.level.com.zeta=trace


#当前项目下生成SpringBoot.log，也可以指定完整的路径
#logging.file.name=springboot.log
#在当前磁盘的【根路径】下创建spring文件夹和里面的log文件；使用spring.log作为默认文件
logging.file.path=/spring/log

#日志输出格式：
#%d表示日期时间，
#%thread表示线程名，
#%-5level：级别从左显示5个字符宽度
#%logger{50} 表示logger名字最长50个字符，否则按照句点分割。
#%msg：日志消息，
#%n是换行符

#在控制台输出的日志的格式
logging.pattern.console=%d{yyyy-MM-dd} [%thread] %-5level %logger{50} - %msg%n
#指定文件中日志输出的格式
logging.pattern.file=%d{yyyy-MM-dd} [%thread] %-5level %logger{50} - %msg%n
```



| logging.file | logging.path | Example  | Description                        |
| ------------ | ------------ | -------- | ---------------------------------- |
| (none)       | (none)       |          | 只在控制台输出                     |
| 指定文件名   | (none)       | my.log   | 输出日志到my.log文件               |
| (none)       | 指定目录     | /var/log | 输出到指定目录的 spring.log 文件中 |

### 2.指定配置

给类路径下放上每个日志框架自己的配置文件即可；spirngBoot就不使用他默认配置了

![image-20210910085313665](D:/Typora/Typora_Note/Java/SpringBoot/SpringBoot.assets/image-20210910085313665.png)

**logback.xml：**会直接被日志框架识别

![image-20210910085645957](D:/Typora/Typora_Note/Java/SpringBoot/SpringBoot.assets/image-20210910085645957.png)

**logback-spring.xml：**日志框架就不直接加载日志的配置项，由springBoot加载【就可以用到sb一个高级特性：profile，不同的运行环境显示不同的日志】

![image-20210910090800227](D:/Typora/Typora_Note/Java/SpringBoot/SpringBoot.assets/image-20210910090800227.png)

```xml
<springProfile name="staging">
    <!‐‐ configuration to be enabled when the "staging" profile is active ‐‐>
    可以指定某段配置只在某个环境下生效
</springProfile>
```

```xml
<!--开发环境-->
<springProfile name="dev">
    <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} -----> [%thread] -----> %-5level %logger{50} - %msg%n</pattern>
</springProfile>
<!--非开发环境-->
<springProfile name="!dev">
    <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ====== [%thread] ======= %-5level %logger{50} - %msg%n</pattern>
</springProfile>
```

## 5.切换日志框架

可以按照slf4j的日志适配图，进行相关的切换；



# 四、Web开发

## **1.使用SpringBoot步骤:**

- 1)创建SpringBoot应用，选中我们需要的模块
- 2)SpringBoot已经默认将这些场景配置好了，只需要在配置文件中指定少量配置就可以运行起来
- 3)自己编写业务代码

**前提：理解自动配置原理**

​	这个场景SpringBoot帮我们配置了什么？能不能修改哪些配置？能不能扩展XXX？

```java
xxxxAutoConfiguration:帮我们给容器中自动配置组件
xxxxProperties:配置类来封装配置文件的内容；
```

## 2.SpringBoot对静态资源的映射规则 

**资源映射源码**

```java
public void addResourceHandlers(ResourceHandlerRegistry registry) {
            if (!this.resourceProperties.isAddMappings()) {
                logger.debug("Default resource handling disabled");
            } else {
                this.addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
                this.addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
                    registration.addResourceLocations(this.resourceProperties.getStaticLocations());
                    if (this.servletContext != null) {
                        ServletContextResource resource = new ServletContextResource(this.servletContext, "/");
                        registration.addResourceLocations(new Resource[]{resource});
                    }

                });
            }
        }

```

```java
//配置欢迎页映射
@Bean
public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext, FormattingConversionService mvcConversionService, ResourceUrlProvider mvcResourceUrlProvider) {
    WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(new TemplateAvailabilityProviders(applicationContext), applicationContext, this.getWelcomePage(), this.mvcProperties.getStaticPathPattern());
    welcomePageHandlerMapping.setInterceptors(this.getInterceptors(mvcConversionService, mvcResourceUrlProvider));
    welcomePageHandlerMapping.setCorsConfigurations(this.getCorsConfigurations());
    return welcomePageHandlerMapping;
}
```



- 1）对所有/webjars/**，都去classpath:/META-INF/resources/webjars/找资源【别人写好的静态资源】

  webjars：以jar包的形式导入静态资源

  https://www.webjars.org/

  ```xml
  <!--引入jquery-webjar-->在访问的时候只需要写webjars下面资源的名称即可
  <dependency>
      <groupId>org.webjars</groupId>
      <artifactId>jquery</artifactId>
      <version>3.6.0</version>
  </dependency>
  ```

  ![image-20210910100653504](D:/Typora/Typora_Note/Java/SpringBoot/SpringBoot.assets/image-20210910100653504.png)

  f访问路径：localhost:8080/webjars/jquery/3.6.0/jquery.js

  ![image-20210910100832228](D:/Typora/Typora_Note/Java/SpringBoot/SpringBoot.assets/image-20210910100832228.png)

- 2）“/**”访问当前项目的任何资源【静态资源的文件夹】

  ```java
  public static class Resources {
          private static final String[] CLASSPATH_RESOURCE_LOCATIONS = new String[]{"classpath:/META-INF/resources/", "classpath:/resources/", "classpath:/static/", "classpath:/pub
  ```

  会去这些路径下访问资源

  ```java
  "classpath:/META‐INF/resources/",
  "classpath:/resources/",
  "classpath:/static/",
  "classpath:/public/"
  "/"：当前项目的根路径
  ```

  ![image-20210910102401388](D:/Typora/Typora_Note/Java/SpringBoot/SpringBoot.assets/image-20210910102401388.png)

  localhost:8080/abc === 去静态资源文件夹里面找abc

- 3）欢迎页：静态资源文件夹下的所有idex.html页面；被“/**”映射

  localhost:8080/ 找index页面

- 4）所有的**/favicon.ico都是在静态资源文件下找

## 3.模板引擎

JSP、Velocity、Freemarker、Thymeleaf


![image-20210910103500244](D:/Typora/Typora_Note/Java/SpringBoot/SpringBoot.assets/image-20210910103500244.png)

SpringBoot推荐的Thymeleaf【**动态页面技术，可以代替jsp】**：语法更简单，功能更强大；

Thymeleaf其实就是显示前后端分离的，【前后端分离】前端可以单独展示，只是没有数据，连接上后端的接口后可以显示数据，即thymeleaf动态显示

### 1.引入引擎依赖

```
<!--引入thymeleaf引擎-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>

通过修改properties来切换thymeleaf版本
<properties>
	<thymeleaf.version>3.0.12.RELEASE</thymeleaf.version>
    <!--布局功能的支持程序 thymeleaf3主程序，适配layout2以上版本-->
    <thymeleaf-layout-dialect.version>2.1.1</thymeleaf-layout-dialect.version>
</properties>
```

### 2.Thymelead的语法

在sb的autoconfiguration的jar包中的

![image-20210914152713201](D:/Typora/Typora_Note/Java/SpringBoot/SpringBoot.assets/image-20210914152713201.png)

存在以下默认规则

```java
@ConfigurationProperties(
    prefix = "spring.thymeleaf"
)
public class ThymeleafProperties {
    private static final Charset DEFAULT_ENCODING;
    public static final String DEFAULT_PREFIX = "classpath:/templates/";
    public static final String DEFAULT_SUFFIX = ".html";
    private boolean checkTemplate = true;
    private boolean checkTemplateLocation = true;
    private String prefix = "classpath:/templates/";
    private String suffix = ".html";
    private String mode = "HTML";
    //只要我们把html页面放在classpath:/templates/下，thymeleaf就可以自动渲染
```

**总结：**只需要将html页面放在classpath:/templates/下，thymeleaf就可以自动渲染

### 3.使用步骤

1.导入thymeleaf的名称空间，来导入一些名词

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```

2.使用thymeleaf的语法

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>成功</h1>
    <!--th:text 将div里面的文本内容设置为。可以替换掉原先前端的数据-->
    <div th:text="${hello}">这是解释【前】</div>
</body>
</html>
```

### 4.语法规则

#### 1）th:text;  来改变当前元素里面的文本内容

​	th: + 任意html的属性：来替换原生属性的值

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>成功</h1>
    <!--th:text 将div里面的文本内容设置为。可以替换掉原先前端的数据-->
    <div id="div01" class="myDiv" th:id="${hello}" th:class="${hello}" th:text="${hello}">这是解释【前】</div>
</body>
</html>
```

静态页面打开

![image-20210914154803873](D:/Typora/Typora_Note/Java/SpringBoot/SpringBoot.assets/image-20210914154803873.png)

运行，替换后的结果

![image-20210914154737384](D:/Typora/Typora_Note/Java/SpringBoot/SpringBoot.assets/image-20210914154737384.png)

![image-20210914155141296](D:/Typora/Typora_Note/Java/SpringBoot/SpringBoot.assets/image-20210914155141296.png)

#### 2）表达式

官方文档中有

```properties
${...}：获取变量值；OGNL；
	1）、获取对象的属性、调用方法
	2）、使用内置的基本对象：
	3）、内置的一些工具对象：
*{...}：选择表达式：和${}在功能上是一样；
#{...}：获取国际化内容
@{...}：定义URL；
~{...}：片段引用表达式
```

#### 3）作用举例

在html上加上th:href

```html
<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
		<meta name="description" content="">
		<meta name="author" content="">
		<title>Signin Template for Bootstrap</title>
		<!-- Bootstrap core CSS -->
		<link href="asserts/css/bootstrap.min.css" th:href="@{/webjars/bootstrap/5.1.0/css/bootstrap.css}" rel="stylesheet">
		<!-- Custom styles for this template -->
		<link href="asserts/css/signin.css" th:href="@{/asserts/css/signin.css}" rel="stylesheet">
	</head>

```

当url的前缀改了

```properties
server.servlet.context-path=/crud
```

html上会自动加上前缀

![image-20210915130917602](D:/Typora/Typora_Note/Java/SpringBoot/SpringBoot.assets/image-20210915130917602.png)

## 4.SpringMVC自动配置

https://docs.spring.io/spring-boot/docs/1.5.10.RELEASE/reference/htmlsingle/#boot-features-developing-web-applications

SpringBoot自动配置好了SpringMVC

以下是SpringBoot对SpringMVC的默认配置：

### 1.Spring MVC auto-configuration

27.1.1 Spring MVC auto-configuration

Spring Boot provides auto-configuration for Spring MVC that works well with most applications.

The auto-configuration adds the following features on top of Spring’s defaults:

- Inclusion of `ContentNegotiatingViewResolver` and `BeanNameViewResolver` beans.
  - **自动配置了ViewResolver（视图解析器：根据方法的返回值得到视图对象（View），视图对象决定如何渲染（转发还是重定向））**
  - **ContentNegotiatingViewResolver：组合所有的视图解析器**
  - 如何定制：我们可以自己给容器中添加一个视图解析器；自动的将其组合起来
  
- Support for serving static resources, including support for WebJars (see below). 静态资源文件夹路径。webjars

- Automatic registration of `Converter`, `GenericConverter`, `Formatter` beans.

- Support for `HttpMessageConverters` (see below).

  自己给容器中添加HttpMessageConverter，只需要将自己的组件注册容器中
  （@Bean,@Component）

自动注册了Converter、Formatter

​	Converter：转换器，类型转换

​	Formatter：格式化；String==>Date

- Automatic registration of `MessageCodesResolver` (see below).
- Static `index.html` support. 静态首页访问
- Custom `Favicon` support (see below).
- Automatic use of a `ConfigurableWebBindingInitializer` bean (see below).

```java
初始化WebDataBinder；
请求数据=====JavaBean；
```

**org.springframework.boot.autoconfigure.web：web的所有自动场景；**

If you want to keep Spring Boot MVC features, and you just want to add additional [MVC configuration](https://docs.spring.io/spring/docs/4.3.14.RELEASE/spring-framework-reference/htmlsingle#mvc) (interceptors, formatters, view controllers etc.) you can add your own `@Configuration` class of type `WebMvcConfigurerAdapter`, but **without** `@EnableWebMvc`. If you wish to provide custom instances of `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter` or `ExceptionHandlerExceptionResolver` you can declare a `WebMvcRegistrationsAdapter` instance providing such components.

If you want to take complete control of Spring MVC, you can add your own `@Configuration` annotated with `@EnableWebMvc`.



### 2.扩展SpringMVC【重要】

以前mvc的配置文件的功能：

```xml
    <mvc:view-controller path="/hello" view-name="success"/>    视图映射：不通过controller，直接跳转页面
    <mvc:interceptors>	拦截器
        <mvc:interceptor>
            <mvc:mapping path="/hello"/>    拦截的路径
            <bean></bean>					拦截器类
        </mvc:interceptor>
    </mvc:interceptors>
```

==编写一个配置类（@Configuration），是WebMvcConfigurerAdapter类型【2.x中直接实现WebMvcConfigurer即可】；不能标注@EnableWebMvc【否则会全面接管MVC】==

既保留了所有的自动配置，也能用我们扩展的配置

```java
//使用WebMvcConfigurationAdapter（2.x直接实现接口，用了default）可以扩展SpringMVC的功能
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        //浏览器发送/zeta，会直接到success页面
        registry.addViewController("/zeta").setViewName("success");
    }
}
```

**原理：**
	1）、WebMvcAutoConfiguration是SpringMVC的自动配置类
	2）、在做其他自动配置时会导入；@Import(EnableWebMvcConfiguration.class)

​	3）、容器中所有的WebMvcConfigurer都会一起起作用；
​	4）、我们的配置类也会被调用；
**效果**：SpringMVC的自动配置和我们的扩展配置都会起作用；

### 3.全面接管SpringMVC

SpringBoot对SpringMVC的自动配置不需要了，所有都是我们自己配置；所有的SpringMVC的自动配置都失效了

我们需要在配置类中添加@EnableWebMvc即可

```java
//使用WebMvcConfigurationAdapter（2.x直接实现接口，用了default）可以扩展SpringMVC的功能
@EnableWebMvc
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        //浏览器发送/zeta，会直接到success页面
        registry.addViewController("/zeta").setViewName("success");
    }
}
```

**原理：**

​	1）@EnableWebMvc的核心，导入了WebMvcConfigurationSupport

```java
@Import(DelegatingWebMvcConfiguration.class)
public @interface EnableWebMvc {
```

​	2）

```java
@Configuration
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
```

​	3）

```java
@Configuration
@ConditionalOnWebApplication
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class,
WebMvcConfigurerAdapter.class })

//容器中没有这个组件的时候，这个自动配置类才生效
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
@AutoConfigureAfter({ DispatcherServletAutoConfiguration.class,
ValidationAutoConfiguration.class })
public class WebMvcAutoConfiguration {
```

​	4）、@EnableWebMvc将WebMvcConfigurationSupport组件导入进来；
​	5）、导入的WebMvcConfigurationSupport只是SpringMVC最基本的功能；

## 5.如何修改SpringBoot的默认配置

模式：

​	1）、SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（@Bean、@Component）。如果有就用用户配置的，如果没有，才自动配置；

​		如果有些组件可以有多个（ViewResolver视图解析器）将用户配置的和自己默认的组合起来；

​	2）、在SpringBoot中会有非常多的xxxConfigurer帮助我们进行扩展配置
​	3）、在SpringBoot中会有很多的xxxCustomizer帮助我们进行定制配置2）

## 6.RestfulCRUD实验

静态资源的位置

![image-20210915103836033](D:/Typora/Typora_Note/Java/SpringBoot/SpringBoot.assets/image-20210915103836033.png)

### **1）默认访问首页**

**①使用Controller实现**

```java
	@RequestMapping({"/","/index.html"})
    public String index(){
        return "index";
    }
```

**②使用配置类实现**

用匿名内部类实现一次接口

```java
	//所有的WebMvcConfigurationAdapter组件都会一起起作用
	@Bean//将组件注册在容器中
    public WebMvcConfigurer webMvcConfigurerAdapter(){
        WebMvcConfigurer adapter = new WebMvcConfigurer() {
            @Override
            public void addViewControllers(ViewControllerRegistry registry) {
                registry.addViewController("/").setViewName("index");
                registry.addViewController("/index.html").setViewName("index");
            }
        };
        return adapter;
    }
```

### 2）国际化

可以根据浏览器的语言信息，切换页面的语言

- **用SpringMVC需要的步骤**

  - 1.编写国际化配置文件
  - 2.使用ResourceBundleMessageSource管理国际化资源文件
  - 3.在页面使用fmt：message取出国际化内容

- **用SpringBoot的步骤**

  - 1.编写国际化配置文件，抽取页面需要显示的国际化消息

  ![image-20210915131914511](https://i.loli.net/2021/09/22/O4QA975UihZI6Sf.png)

  - 2.SpringBoot自动配置好了管理国际化资源文件的组件

    我们的国际化配置文件默认可以放在类路径下叫message.properties中，也可以自定义路径

    ```properties
    spring.messages.basename=i18n.login
    ```

  - 3.去页面获取国际化的值，#{}来取国际化信息

  ```java
  <body class="text-center">
  		<form class="form-signin" action="dashboard.html">
  			<img class="mb-4" th:src="@{/asserts/img/bootstrap-solid.svg}" src="asserts/img/bootstrap-solid.svg" alt="" width="72" height="72">
  			<h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}">Please sign in</h1>
  			<label class="sr-only" th:text="#{login.username}">Username</label>
  			<input type="text" class="form-control" placeholder="Username" th:text="#{login.username}" required="" autofocus="">
  			<label class="sr-only"th:text="#{login.password}">Password</label>
  			<input type="password" class="form-control" placeholder="Password" th:text="#{login.password}" required="">
  			<div class="checkbox mb-3">
  				<label>
            <input type="checkbox" value="remember-me"> [[#{login.remember}]]
          </label>
  			</div>
  			<button class="btn btn-lg btn-primary btn-block" type="submit" th:text="#{login.btn}">Sign in</button>
  			<p class="mt-5 mb-3 text-muted">© 2017-2018</p>
  			<a class="btn btn-sm">中文</a>
  			<a class="btn btn-sm">English</a>
  		</form>
  
  	</body>
  ```

  

**效果：**

​	根据浏览器语言设置的信息切换了国际化；【想要根据页面的中文/en来切换】

**国际化原理：**

​	重要对象：国际化Locale（区域信息对象）；LocaleResolver（获取区域信息对象）

```JAVA
		@Bean
        @ConditionalOnMissingBean(
            name = {"localeResolver"}
        )
        public LocaleResolver localeResolver() {
            if (this.webProperties.getLocaleResolver() == org.springframework.boot.autoconfigure.web.WebProperties.LocaleResolver.FIXED) {
                return new FixedLocaleResolver(this.webProperties.getLocale());
            } else if (this.mvcProperties.getLocaleResolver() == org.springframework.boot.autoconfigure.web.servlet.WebMvcProperties.LocaleResolver.FIXED) {
                return new FixedLocaleResolver(this.mvcProperties.getLocale());
            } else {
                AcceptHeaderLocaleResolver localeResolver = new AcceptHeaderLocaleResolver();
                Locale locale = this.webProperties.getLocale() != null ? this.webProperties.getLocale() : this.mvcProperties.getLocale();
                localeResolver.setDefaultLocale(locale);
                return localeResolver;
            }
        }
默认的就是根据请求头带来的区域信息获取Locale进行国际化
```

### 3) **如何实现切换CN/EN：**

**1.自定义LocalResolver**

```java
/**
 * 在连接上携带区域信息
 * @author Zeta
 * @version 1.0
 * @date 2021/9/15 18:14
 */
public class MyLocalResolver implements LocaleResolver {
    @Override
    public Locale resolveLocale(HttpServletRequest httpServletRequest) {
        String l = httpServletRequest.getParameter("l");
        //如果没有参数，就用系统默认的Local
        Locale locale = Locale.getDefault();
        if(!StringUtils.isEmpty(l)){
            //l=zh_CN，中间有下划线，需要分割开来
            String[] split =  l.split("_");
            //new一个locale，第一个参数是语言代码，第二个参数是国家代码
            locale = new Locale(split[0],split[1]);
        }
        return locale;
    }

    @Override
    public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {

    }
}
```

**2.添加到容器中：在配置类中添加组件**

@Configuration用于定义配置类，可替换xml配置文件，被注解的类内部包含有一个或多个被@Bean注解的方法，这些方法将会被AnnotationConfigApplicationContext或AnnotationConfigWebApplicationContext类进行扫描，并用于构建bean定义，初始化Spring容器。

```java
	//添加自己的区域信息解析器
    @Bean
    public LocaleResolver localeResolver(){
        return new MyLocalResolver();
    }
```

### 4)登陆

开发期间模板引擎页面修改以后，要实时生效

- 1.禁用模板引擎的缓存
- 2.页面修改后ctrl+F9，重新编译

```properties
#禁用thymeleaf的缓存，html修改后一刷新就会显示
#修改后，配合ctrl+F9重新编译即可
spring.thymeleaf.cache=false
```

登陆错误消息的显示

```html
失败了，一直在显示没有做判断
<!--判断-->
<p style="color: red" th:text="${msg}" th:if="${not #strings.isEmpty(msg)}"></p>
```

##### 注意：重复提交表单解决

当进入main页面时，按F5刷新 ，会出现以下提示

![image-20210922105050280](D:/Typora/Typora_Note/Java/SpringBoot/SpringBoot.assets/image-20210922105050280.png)

**解决方法：**

https://blog.csdn.net/qq_41649001/article/details/107491939

利用重定向，加一个视图映射

```java
//所有的WebMvcConfigurationAdapter组件都会一起起作用
    @Bean
    public WebMvcConfigurer webMvcConfigurerAdapter(){
        WebMvcConfigurer adapter = new WebMvcConfigurer() {
            @Override
            public void addViewControllers(ViewControllerRegistry registry) {
                registry.addViewController("/").setViewName("login");
                registry.addViewController("/login.html").setViewName("login");
                //新增的视图映射
                registry.addViewController("/main.html").setViewName("dashboard");
            }
        };
        return adapter;
    }
```

添加重定向

```java
if(StringUtils.hasLength(username) && "12345".equals(password)){
            //登陆成功,防止表单重复提交，可以重定向到主页
            return "redirect:/main.html ";
        }else{
```

### 5）拦截器进行登陆检查

- **配置拦截器**

```java
/**
 * 做登陆检查拦截
 * @author Zeta
 * @version 1.0
 * @date 2021/9/22 10:56
 */
public class LoginHandlerInterceptor implements HandlerInterceptor {
    //目标方法执行之前
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        Object user = request.getSession().getAttribute("loginUser");
        if(user == null){
            //未登录，返回登陆页面
            request.setAttribute("msg" , "没有权限，请先登录");
            request.getRequestDispatcher("/index.html").forward(request,response);
            return false;
        }else{
            //已登陆，放行
            return true;
        }
    }
```

- 在MVC配置类中配置拦截器，拦截的路径信息

```java
//所有的WebMvcConfigurationAdapter组件都会一起起作用
    @Bean
    public WebMvcConfigurer webMvcConfigurerAdapter(){
        //配置视图映射
        WebMvcConfigurer adapter = new WebMvcConfigurer() {
            @Override
            public void addViewControllers(ViewControllerRegistry registry) {
                registry.addViewController("/").setViewName("login");
                registry.addViewController("/index.html").setViewName("login");
                registry.addViewController("/main.html").setViewName("dashboard");
            }
            //配置拦截器
            @Override
            public void addInterceptors(InterceptorRegistry registry) {
                //静态资源：默认不需要排除，sb已经做好了静态资源的映射
                registry.addInterceptor(new LoginHandlerInterceptor()).addPathPatterns("/*")
                        .excludePathPatterns("/index.html","/","/user/login");
            }
        };

        return adapter;
    }
```

### 6)CRUD-员工列表

实验要求：

- **1)ResutfulCRUD：区别在于CRUD需要满足Rest风格**
- URI：/资源名称/资源标识

补充知识：URI和URL的区别：

​	URL是URI的子集，是URI的一种实现方式。

| URL（统一资源定位）标识资源的位置   | 是URI的一种，标识了Web资源，指定了操作或获取方式，同时包含访问机制和网络位置 | http://www.163.com/servlet/hi |
| ----------------------------------- | ------------------------------------------------------------ | ----------------------------- |
| URI（统一资源标识符）标识互联网资源 |                                                              | servlet/hi                    |
| URN                                 | 是URI的一种，用特定命名空间的名字标识资源。包括名字（给定的命名空间），但不包含访问方式。 |                               |

![image-20210922160343671](D:/Typora/Typora_Note/Java/SpringBoot/SpringBoot.assets/image-20210922160343671.png)

|      | 普通CRUD(URI来区分操作) | RestfulCRUD       |
| ---- | ----------------------- | ----------------- |
| 查询 | getEmp                  | emp---GET         |
| 添加 | addEmp?xxx              | emp---POST        |
| 修改 | updateEmp?id=xxx        | emp/{id}---PUT    |
| 删除 | deleteEmp?id=1          | emp/{id}---DELETE |



- **2)实现的请求架构**

|                                      | 请求URI | 请求方式 |
| ------------------------------------ | ------- | -------- |
| 查询所有员工                         | emps    | GET      |
| 查询某个员工（来到修改页面）         | emp/1   | GET      |
| 来到添加页面                         | emp     | GET      |
| 添加员工                             | emp     | POST     |
| 来到修改页面（查询员工进行信息回显） | emp/1   | GET      |
| 修改员工                             | emp     | PUT      |
| 删除员工                             | emp/1   | DELETE   |

- **3）员工列表**

thymeleaf公共页面元素抽取：会出现一些重复的页面代码【导航栏、头部信息】，只需要把代码放在了公共的页面，调用时引入即可

```html
1、抽取公共片段
<div th:fragment="copy">
&copy; 2011 The Good Thymes Virtual Grocery
</div>
2、引入公共片段
<div th:insert="~{footer :: copy}"></div>
~{templatename::selector}：模板名::选择器
~{templatename::fragmentname}:模板名::片段名

3、默认效果：
insert的公共片段在div标签中
如果使用th:insert等属性进行引入，可以不用写~{}：
行内写法可以加上：[[~{}]];[(~{})]；
```

**三种引入公共片段的th属性：**

- th:insert：将公共片段整个插入到声明引入的元素中
- th:replace：将声明引入的元素替换为公共片段
- th:include：将被引入的片段的内容包含进这个标签中



引入片段的时候传入参数【实现点哪个菜单，哪个亮的效果】

```html
<nav class="col‐md‐2 d‐none d‐md‐block bg‐light sidebar" id="sidebar">
<div class="sidebar‐sticky">
<ul class="nav flex‐column">
<li class="nav‐item">
<a class="nav‐link active"
th:class="${activeUri=='main.html'?'nav‐link active':'nav‐link'}"
href="#" th:href="@{/main.html}">
<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24"
viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke‐width="2" strokelinecap="
round" stroke‐linejoin="round" class="feather feather‐home">
<path d="M3 9l9‐7 9 7v11a2 2 0 0 1‐2 2H5a2 2 0 0 1‐2‐2z"></path>
<polyline points="9 22 9 12 15 12 15 22"></polyline>
</svg>
Dashboard <span class="sr‐only">(current)</span>
</a>
</li>
<!‐‐引入侧边栏;传入参数‐‐>
<div th:replace="commons/bar::#sidebar(activeUri='emps')"></div>
```



### 7)CRUD-员工添加

添加页面

```html
<form>
    <div class="form‐group">
        <label>LastName</label>
        <input type="text" class="form‐control" placeholder="zhangsan">
    </div>
    <div class="form‐group">
        <label>Email</label>
        <input type="email" class="form‐control" placeholder="zhangsan@atguigu.com">
    </div>
    <div class="form‐group">
        <label>Gender</label><br/>
        <div class="form‐check form‐check‐inline">
            <input class="form‐check‐input" type="radio" name="gender" value="1">
            <label class="form‐check‐label">男</label>
    	</div>
        <div class="form‐check form‐check‐inline">
            <input class="form‐check‐input" type="radio" name="gender" value="0">
            <label class="form‐check‐label">女</label>
        </div>
	</div>
	<div class="form‐group">
        <label>department</label>
        <select class="form‐control">
            <option>1</option>
            <option>2</option>
            <option>3</option>
            <option>4</option>
            <option>5</option>
        </select>
	</div>
    <div class="form‐group">
        <label>Birth</label>
        <input type="text" class="form‐control" placeholder="zhangsan">
    </div>
	<button type="submit" class="btn btn‐primary">添加</button>
</form>
```

注意：【常见问题】

1.提交的数据格式不对：生日和日期。日期的格式化：Spirng，需要格式校验和格式转换

2017-12-12；2017/12/12；2017.12.12；
日期的格式化；SpringMVC将页面提交的值需要转换为指定的类型;
2017-12-12---Date； 类型转换，格式化;
默认日期是按照/的方式；

```properties
spring.mvc.format.date=yyyy-MM-dd
日期格式化作用：
不按照这个格式输入数据，会报错
```

