# 1.weblog-web

建一个 `weblog-web` 模块，此模块是项目的入口

手动添加 `Spring Boot` 的启动类，类名为 `WeblogWebApplication` , 代码如下：

```java
package com.quanxiaoha.weblog.web;

import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
@Slf4j
public class WeblogWebApplicationTests {

    @Test
    public void test() {
        // 编写单元测试
    }
}
```

#  搭建 Spring Boot 多模块工程（通过 Maven Archetype）



犬小哈

2023-11-24约 2,809 字大约 10 分钟共 31 张图

1,052

友情提示

推荐您使用 Chrome 浏览器来阅读本实战专栏，其他浏览器可能存在兼容性问题。

本小节为**补充内容**，原因是部分球友反馈说，`IDEA` 新建项目时，如果通过 `Spring Initializr` 来创建 `Spring Boot` , 已经无法选择 `Java 8` 版本，通过上小节的教程，不知道该如何创建 `Spring Boot` 多模块工程。如下图所示：

![img](https://img.quanxiaoha.com/quanxiaoha/170081687458260)

本小节中，小哈就来演示一下另一种方式，通过 `Maven Archetype` 来创建 `Spring Boot` 多模块工程。

## IDEA 搭建 Spring Boot 多模块工程骨架

### 开始动手

首先选择一个位置，新建一个名为 `weblog` 的工程目录，用于统一存放后续要创建的后端项目、前端项目，方便管理：

![img](https://img.quanxiaoha.com/quanxiaoha/169105275897127)

### 创建父项目

打开 IDEA, 依次点击菜单 *File -> New -> Project*, 准备新建**父项目** :

![img](https://img.quanxiaoha.com/quanxiaoha/170081726847889)

- ①：选择 `Maven Archetype` 来创建项目；

- ②：填写工程名称；

- ③：工程路径；

- ④：选择 `JDK 1.8` 版本；

- ⑤： IDEA 需要知道 Maven Archetype Catalog 的位置，以便从中获取可用的 Archetype 列表。这个 Catalog 文件通常包含了 Maven 官方仓库或其他远程仓库中可用的 Archetype 信息。我这里选择的是

   

  ```
  Default Local
  ```

   

  , 也就是我本地安装的

   

  ```
  Maven
  ```

   

  路径：

  - ![img](https://img.quanxiaoha.com/quanxiaoha/170081715545737)

- ⑥：通过使用 Archetype，你可以基于已有的项目模板创建一个新项目，从而加快项目的启动和初始化过程，选择 `maven-archetype-site-simple`。

- ⑦：填写 Group 组织名称，通常为公司域名倒写，如 `com.quanxiaoha`；

- ⑧：项目的唯一标识符；

- ⑨：项目版本号，默认就行；

所有选项填写完成后，点击 *Create* 创建项目：

![img](https://img.quanxiaoha.com/quanxiaoha/170081751558564)

因为这是个父项目，专门负责统一管理子模块、依赖版本等，不包含功能代码，所以，创建完成后，在左边导航栏中，还需要将 `/src` 这个文件夹删除掉，最终目录结构如下：

![img](https://img.quanxiaoha.com/quanxiaoha/170081800656954)

接下来，整理一下父项目的 `pom.xml` 文件，内容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <!-- 将 Spring Boot 的版本号切换成 2.6 版本 -->
        <version>2.6.3</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <groupId>com.quanxiaoha</groupId>
    <artifactId>weblog-springboot</artifactId>
    <version>${revision}</version>
    <name>weblog-springboot</name>
    <!-- 项目描述 -->
    <description>前后端分离博客 Weblog By 犬小哈</description>

    <!-- 多模块项目父工程打包模式必须指定为 pom -->
    <packaging>pom</packaging>

    <!-- 子模块管理 -->
    <modules>
    </modules>


    <!-- 版本号统一管理 -->
    <properties>
        <!-- 项目版本号 -->
        <revision>0.0.1-SNAPSHOT</revision>
        <java.version>1.8</java.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        
        <!-- Maven 相关 -->
        <maven.compiler.source>${java.version}</maven.compiler.source>
        <maven.compiler.target>${java.version}</maven.compiler.target>
        
    </properties>

    <!-- 统一依赖管理 -->
    <dependencyManagement>

    </dependencyManagement>

    <build>
        <!-- 统一插件管理 -->
        <pluginManagement>

        </pluginManagement>
    </build>
    
    <!-- 使用阿里云的 Maven 仓库源，提升包下载速度 -->
    <repositories>
        <repository>
            <id>aliyunmaven</id>
            <name>aliyun</name>
            <url>https://maven.aliyun.com/repository/public</url>
        </repository>
    </repositories>
</project>
```

### 创建 web 访问模块（打包也在这个模块中进行）

接下来，我们开始创建父项目下面的子模块。在父项目上**右键**，添加模块 `Module`:

![img](https://img.quanxiaoha.com/quanxiaoha/169105542889388)

还是和上面创建父项目差不多的步骤，创建一个 `weblog-web` 模块，此模块是项目的入口，Maven 打包插件也是放在此模块下，另外，和博客前台页面展示相关的功能也统一放在该模块下：

![img](https://img.quanxiaoha.com/quanxiaoha/170081816576610)

> **TIP** : 唯一多的一个选项就是 *Parent* , 用于指定该模块的父项目。

点击 *Create* 创建子模块，创建完成后，打开父项目的 `pom.xml` 文件，就可以看到在 `<modules>` 子节点下，已经自动添加了 `weblog-web` 模块了。接着，我们还需要再添加一个 `spring-boot-maven-plugin` 插件，代码如下：

```
<!-- 子模块管理 -->
<modules>
    <!-- 入口模块 -->
    <module>weblog-web</module>
</modules>

<build>
        <!-- 统一插件管理 -->
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                    <configuration>
                        <excludes>
                            <exclude>
                                <groupId>org.projectlombok</groupId>
                                <artifactId>lombok</artifactId>
                            </exclude>
                        </excludes>
                    </configuration>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
```

添加完成后，开始编辑 `weblog-web` 模块中的 `pom.xml`，配置内容如下:

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!-- 指定父项目为 weblog-springboot -->
    <parent>
        <groupId>com.quanxiaoha</groupId>
        <artifactId>weblog-springboot</artifactId>
        <version>${revision}</version>
    </parent>

    <groupId>com.quanxiaoha</groupId>
    <artifactId>weblog-web</artifactId>
    <name>weblog-web</name>
    <description>weblog-web (入口项目，负责博客前台展示相关功能，打包也放在这个模块负责)</description>

    <dependencies>
        <!-- Spring Boot Web 依赖 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- 免写冗余的 Java 样板式代码 -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <!-- 单元测试 -->
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
```

#### 手动添加 Spring Boot 启动类

在里面手动添加单元测试类，类名为 `WeblogWebApplicationTests` , 代码如下

```java
ackage com.quanxiaoha.weblog.web;

import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
@Slf4j
public class WeblogWebApplicationTests {

    @Test
    public void test() {
        // 编写单元测试
    }
}
```

### 启动项目

![image-20231222114228196](https://bu.dusays.com/2023/12/22/658505a5bc7ea.png)

![image-20231222114201789](https://bu.dusays.com/2023/12/22/6585058b544c8.png)

# 2.weblog-module-admin

### 创建 Admin 管理后台功能模块

命名为 `weblog-module-admin` ，此模块用于统一放置和 `Admin` 管理后台相关的功能:

# 3.weblog-module-common

此模块专门用于存放一些通用的功能，如接口的日志切面、全局异常管理等等，后续小节会讲到，这里先把子模块建好。



# 打包测试看看

![image-20231222120039423](https://bu.dusays.com/2023/12/22/658509e9a98c7.png)

# 4.Spring Boot 项目多环境配置

- `application.properties`: 存放所有环境通用的配置。
- `application-dev.properties`: 存放开发环境的特殊配置。
- `application-test.properties`: 存放测试环境的特殊配置。
- `application-prod.properties`: 存放生产环境的特殊配置。

### 激活默认环境

编辑 `applicaiton.yml`, 添加通用配置：

```yaml
spring:
  profiles:
    # 默认激活 dev 环境
    active: dev
```

### 验证是否生效

![image-20231222125342514](https://bu.dusays.com/2023/12/22/658516585bcd0.png)

# 5.配置 Lombok

Lombok 是一个超酷的 Java 库，**它能让你避免编写那些冗余的 Java 样板式代码**，如对象中的 `get`、`set`、`toString` 等方法，解放你的双手

## Lombok 的用途

1. **简化 Getter 和 Setter 方法：** 在传统的 Java 开发中，你经常需要为每个类的属性手动编写 Getter 和 Setter 方法，但是有了 Lombok，你只需要在属性上加上 `@Getter` 和 `@Setter` 注解，Lombok 就会为你自动生成这些方法。
2. **自动生成构造函数：** 通过 `@NoArgsConstructor`、`@RequiredArgsConstructor` 或 `@AllArgsConstructor` 注解，你可以快速生成无参构造函数、带有必需参数的构造函数或者带有全部参数的构造函数。
3. **自动生成 equals 和 hashCode 方法：** 通过 `@EqualsAndHashCode` 注解，Lombok 会根据类的字段自动生成 `equals()` 和 `hashCode()` 方法，让你的类更易于比较和使用在集合中。
4. **日志记录更轻松：** 使用 `@Slf4j` 注解，你可以直接在类中使用 `log` 对象，而无需手动创建日志记录器。
5. **简化异常抛出：** 通过 `@SneakyThrows` 注解，你可以在方法中抛出受检异常，而无需显式地在方法上声明或捕获它们。
6. **数据类简化：** 使用 `@Data` 注解，Lombok 会为你自动生成所有常用方法，如 Getter、Setter、`toString()` 等，让你的数据类更加简洁。
7. **链式调用：** 使用 `@Builder` 注解，Lombok 可以帮你创建一个更优雅的构建器模式，让你的对象初始化更加流畅。

## 项目中添加 Lombok 依赖

想要在项目中使用 Lombok, 首先, 你需要在项目中添加 Lombok 依赖。前面小节中，小哈其实已经将 Lombok 依赖添加好了，这里再提一下。

针对通过 Maven 构建的项目来说，只需要在项目的 `pom.xml` 文件中添加以下依赖：

```
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.28</version> <!-- 注意查看最新的 Lombok 版本，并替换此处的版本号 -->
    <scope>provided</scope>
</dependency>
```

## 6.IDEA 配置 Lombok 插件

> TIP: IDEA 2020.3 及以上版本已经内置安装了 Lombok 插件，如果你的 IDEA 版本大于该版本，则不用管，否则需要按下面的步骤，来手动安装 Lombok 插件。

# 7.Spring Boot 自定义注解，实现 API 请求日志切面

### 自定义注解

在 `weblog-module-common` 通用模块下，新建一个名为 `aspect` 的包，用于放置切面相关的功能类，接着，在其中创建一个名为 `ApiOperationLog` 的自定义注解：



元注解说明：

- `@Retention(RetentionPolicy.RUNTIME)`： 这个元注解用于指定注解的保留策略，即注解在何时生效。`RetentionPolicy.RUNTIME` 表示该注解将在运行时保留，这意味着它可以通过反射在运行时被访问和解析。
- `@Target({ElementType.METHOD})`： 这个元注解用于指定注解的目标元素，即可以在哪些地方使用这个注解。`ElementType.METHOD` 表示该注解只能用于方法上。这意味着您只能在方法上使用这个特定的注解。
- `@Documented`： 这个元注解用于指定被注解的元素是否会出现在生成的Java文档中。如果一个注解使用了 `@Documented`，那么在生成文档时，被注解的元素及其注解信息会被包含在文档中。这可以帮助文档生成工具（如 JavaDoc）在生成文档时展示关于注解的信息。

### 日志切面

#### aspectj 注解说明

在配置 AOP 切面之前，我们需要了解下 `aspectj` 相关注解的作用：

- **@Aspect**：声明该类为一个切面类；
- **@Pointcut**：定义一个切点，后面跟随一个表达式，表达式可以定义为切某个注解，也可以切某个 package 下的方法；

切点定义好后，就是围绕这个切点做文章了：

- **@Before**: 在切点之前，织入相关代码；
- **@After**: 在切点之后，织入相关代码;
- **@AfterReturning**: 在切点返回内容后，织入相关代码，一般用于对返回值做些加工处理的场景；
- **@AfterThrowing**: 用来处理当织入的代码抛出异常后的逻辑处理;
- **@Around**: 环绕，可以在切入点前后织入代码，并且可以自由的控制何时执行切点；

#### 创建 JSON 工具类

.在定义日志切面之前，我们先来创建一个 JSON 工具类，这在日志切面中打印出入参为 JSON 字符串会用到。在 `weblog-module-common` 通用模块下，创建一个 `utils` 包，用于统一放置工具类相关，然后，新建一个名为 `JsonUtil` 的工具类， 代码如下：

#### 定义日志切面类

工具类搞定后，在 `aspect` 包下，新建切面类 `ApiOperationLogAspect` , 代码如下，附有详细注释：

### 添加包扫描

在启动类 `WeblogWebApplication` 中，手动添加包扫描 `@ComponentScan` , 指定扫描 `com.quanxiaoha.weblog` 包下面的所有类:

![1703235275335](https://bu.dusays.com/2023/12/22/65854ed3029fa.png)

# 通过 JSR 380 实现参数校验功能

### 引入依赖

首先，我们需要在 `weblog-web` 模块中的 `pom.xml` 文件添加参数校验依赖：

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

以下是 JSR 380 中提供的主要验证注解及其描述：

1. **@NotNull**: 验证对象值不应为 null。
2. **@AssertTrue**: 验证布尔值是否为 true。
3. **@AssertFalse**: 验证布尔值是否为 false。
4. **@Min(value)**: 验证数字是否不小于指定的最小值。
5. **@Max(value)**: 验证数字是否不大于指定的最大值。
6. **@DecimalMin(value)**: 验证数字值（可以是浮点数）是否不小于指定的最小值。
7. **@DecimalMax(value)**: 验证数字值（可以是浮点数）是否不大于指定的最大值。
8. **@Positive**: 验证数字值是否为正数。
9. **@PositiveOrZero**: 验证数字值是否为正数或零。
10. **@Negative**: 验证数字值是否为负数。
11. **@NegativeOrZero**: 验证数字值是否为负数或零。
12. **@Size(min, max)**: 验证元素（如字符串、集合或数组）的大小是否在给定的最小值和最大值之间。
13. **@Digits(integer, fraction)**: 验证数字是否在指定的位数范围内。例如，可以验证一个数字是否有两位整数和三位小数。
14. **@Past**: 验证日期或时间是否在当前时间之前。
15. **@PastOrPresent**: 验证日期或时间是否在当前时间或之前。
16. **@Future**: 验证日期或时间是否在当前时间之后。
17. **@FutureOrPresent**: 验证日期或时间是否在当前时间或之后。
18. **@Pattern(regexp)**: 验证字符串是否与给定的正则表达式匹配。
19. **@NotEmpty**: 验证元素（如字符串、集合、Map 或数组）不为 null，并且其大小/长度大于0。
20. **@NotBlank**: 验证字符串不为 null，且至少包含一个非空白字符。
21. **@Email**: 验证字符串是否符合有效的电子邮件格式。

除了上述的标准注解，JSR 380 也支持开发者定义和使用自己的自定义验证注解。此外，这个规范还提供了一系列的APIs和工具，用于执行验证和处理验证结果。大部分现代Java框架（如 Spring 和 Jakarta EE）都与 JSR 380 兼容，并支持其验证功能。

#  Spring Security 整合 JWT 实现表单加密

![1703396862808](https://bu.dusays.com/2023/12/24/6587c60910cf4.png)

## JWT 流程图

JWT 完整流程如下：

![img](https://img.quanxiaoha.com/quanxiaoha/169319074774424)

解释一下上面的流程图，主要包含以下几个步骤：

- 用户使用账号和密码发起 POST 请求，也就是上小节完成的登录认证接口；
- 服务器使用私钥创建一个 Token 令牌；
- 服务器返回这个 Token 令牌给浏览器；
- 浏览器将该 Token 令牌附带在请求头中，向服务器发送请求；
- 服务器验证该令牌；
- 若令牌正确，则返回响应的资源给浏览器。

![1703400414479](https://bu.dusays.com/2023/12/24/6587d3e2cf888.png)

# 8.Element Plus 手搭 Admin 管理后台骨架

基本来说，市面上比较主流的 `Admin` 管理后台的布局如下图所示，共分为 *5 个*区域：

- **左侧导航栏**：也是功能菜单栏，点击后，内容区域会展现不同的页面，如文章管理列表等；
- **顶部栏**：通常用于显示用户是否登录、以及其它一些功能，如全屏展示、白天黑夜效果、语言国际化等；
- **标签导航栏**：每次打开一个新的页面，标签导航栏内就会动态新增一个标签，点击标签可来回切换页面；
- **内容区域**：根据当前路由动态渲染不同的内容页；
- **Footer 页脚** ：展示一些页脚信息，如 `Copyright` 版权信息等。

![img](https://img.quanxiaoha.com/quanxiaoha/169407092957922)

![image-20231226111033909](https://bu.dusays.com/2023/12/26/658a442ce53d1.png)
