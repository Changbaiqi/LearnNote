# Spring Security框架

---

## 第一章了解Spring Security

Spring Security是基于Spring的安全框架。它提供全面的安全性解决方案，同时在Web请求和方法调用级处理身份确认和授权。在Spring Framework基础上，Spring Security充分利用了依赖注入（DI）和面向切面编程（AOP）功能，为应用系统提供声明式的安全访问控制功能，减少了为企业系统安全控制编写大量重复代码的工作。是一个轻量级的安全框架。他与SpringMVC有很好地集成。

### 1.1Spring Security 核心功能

（1）认证（你是谁，用户/设备/系统）

（2）验证（你能干什么，也叫权限控制/授权，允许执行的操作）

### 1.2Spring Security 原理

基于 Filter，Servlet，AOP实现身份认证和权限验证



## 第二章 实例驱动学习

### 导入依赖：

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
```

### 初始默认密码登录

在初次导入框架使用的时候，Security会默认生成一个密码：

![](图片文件\Security\Snipaste_2023-05-14_05-58-02.png)

这个密码在默认访问任意接口的时候都需要你进行登录操作，登录时账号就为：user，密码就是上面生成的随机密码。



### 修改默认密码

修改默认密码后就不会再随机生成密码了，并且也不会展示，修改密码需要在application配置文件里面设置：

```yaml
spring:
  security:
    user:
      name: cbq  #账号
      password: cbq  #密码
```



### 关闭验证

关闭验证只需要在SpringBoot启动类注解里面添加排除项即可：

```java
//排除Security的配置，让他不启用
@SpringBootApplication(exclude = {SecurityAutoConfiguration.class})
public class SecurityStudyApplication {

    public static void main(String[] args) {
        SpringApplication.run(SecurityStudyApplication.class, args);
    }

}

```

