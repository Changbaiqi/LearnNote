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



### 使用内存中的用户信息

这里一定要注意！！！！

> 在**Spring Security 5.7.0-M2**，我们弃用了 `WebSecurityConfigurerAdapter` ，因为我们鼓励用户转向使用基于组件的安全配置。

如果是使用了5.7以下的那么就按照以下方法来！！！



使用：WebSecurityConfigurerAdapter控制安全管理的内容。

需要做的使用：继承WebSecurityConfigurerAdapter，重新方法。实现自定义的认证信息。

注解：

1、@Configuration：表示当前类是一个配置类（相当于是Spring的xml配置文件），在这个类方法的返回值是Java对象，这些对象放入到Spring容器中。

2、@EnableWebSecurity：表示启动Spring Security安全框架的功能。

3/@Bean：把方法返回值的对象，放入到Spring容器中。

这里注意Security5中密码必须要加密，不然报错。

> ```java
> java.lang.IllegalArgumentException: There is no PasswordEncoder mapped foor the id "null"
> ```
>
> 

![](图片文件\Security\Snipaste_2023-05-14_06-37-15.png)

![](图片文件\Security\Snipaste_2023-05-14_06-42-48.png)

如果使用了新版Security那么：

```java
@Configuration
public class SecurityConfiguration {
    @Bean
    public InMemoryUserDetailsManager userDetailsService() {
        UserDetails user = User.withDefaultPasswordEncoder()
            .username("user")
            .password("password")
            .roles("USER")
            .build();
        return new InMemoryUserDetailsManager(user);
    }
}

```

**注意**：在这些例子中，我们为了可读性使用了`User.withDefaultPasswordEncoder()`。这不适合生产项目，我们建议在生产项目中使用散列密码。请按照[参考文档](https://docs.spring.io/spring-security/reference/features/authentication/password-storage.html#authentication-password-storage-boot-cli)所说的用Spring Boot命令行工具来做。



### 基于角色Role的身份认证，同一个用户可以有不同的角色。同时可以开启对方法级别的认证。

以下是WebSecurity.java配置文件：

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;

@Configuration
@EnableWebSecurity
@EnableMethodSecurity(prePostEnabled = true) //这个作用是用于开启基于角色访问，不然的话无效
public class MyWebSecurityConfig {

    //在方法中配置 用户和密码的信息，作为登录的数据
    @Bean
    public InMemoryUserDetailsManager userDetailsManager(){

        UserDetails user1 = User.withDefaultPasswordEncoder()
                .username("cbq")
                .password("cbq")
                .roles("normal")
                .build();
        UserDetails user2 = User.withDefaultPasswordEncoder()
                .username("cbq1")
                .password("cbq1")
                .roles("normal")
                .build();
        UserDetails user3 = User.withDefaultPasswordEncoder()
                .username("cbq2")
                .password("cbq2")
                .roles("normal","admin")
                .build();

        return new InMemoryUserDetailsManager(user1,user2,user3);
    }


}
```

这里开了三个用户，一共两个角色，一个normal，一个admin。一个用户可以拥有多个角色。基于角色的访问需要配合@PreAuthorize注解使用，详细如下，这里@PreAuthorize注解标注了哪个接口只能哪几个角色能访问。

```java
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("hello")
public class HelloSecurityController {

    @RequestMapping("world")
    @PreAuthorize(value = "hasAnyRole('admin','normal')")
    public Object sayHello(){
        return "拥有admin和normal";
    }

    @RequestMapping("hhh")
    @PreAuthorize(value = "hasAnyRole('admin')")
    public Object hhh(){
        return "admin角色才可以访问";
    }

}
```





### 基于JDBC的用户认证

从数据库Mysql中获取用户的身份信息（用户名称，密码，角色）

在Spring Security框架对象用户信息的表示类是UserDetails.

UserDetails是一个接口，高度抽象的用户信息类(相当于项目中的User类)



User类：是UserDetails接口的实现类，构造方法有三个参数：

username，password，authorities



需要向Spring Security提供User对象，这个对象的数据来自数据库中的查询。

实现UserDetailsService接口，重写UserDetails loadUserByUsername(String var1)在方法中获取数据库中的用户信息，也就是执行数据库的查询，条件是用户名称。

### 密码加密存储

实际项目中我们不会把密码明文存储在数据库表中。

默认使用的PasswordEncoder要求数据库中的密码格式为：{id}password。它会根据id去判断密码的加密方式。但是我们一般不会采用这种方式。所以就需要替换PordEncoder。

我们一般使用SpringSecurity为我们提供的BCryptPasswordEncoder。

我们只需要使用把BCryptPasswordEncoder对象注入Spring容器中，SpringSecurity就会使用该PasswordEncoder来进行密码校验。

我们可以定义一个SpringSecurity的配置类，SpringSecurity要求这个配置类要继承WebSecurityConfigurerAdapter。



## 拦截相应没有访问权限访问页面的用户

```java
import jakarta.annotation.Resource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
@EnableMethodSecurity(prePostEnabled = true)
public class MyWebSecurityConfig {
    @Resource
    private UserDetailsService userDetailsService;

    @Bean
    SecurityFilterChain securityFilterChain(HttpSecurity httpSecurity) throws Exception {
        //配置web授权访问,"/login","/register","/upLogin"这些统统无需权限
        httpSecurity.authorizeHttpRequests().requestMatchers("/login","register","/upLogin").permitAll()
                //成功的链接,也就是后续访问的链接放在这个里,我这里是成功页需要USER权限才能访问
                .requestMatchers("/success").hasAnyRole("USER").and()
        /**
         * 自定义登陆页面是loginSelf.html页(这里看你模板引擎怎么配),表单提交的处理链接是"/upLogin"
         * 就是<form action="/upLogin"></form>这样,这个"/upLogin"不需要自己处理,就自己决定
         * usernameParameter和passwordParameter,就是表单提交所带的参数,
         * 这里我是"username"和"password";
         */
                .formLogin().loginPage("/loginSelf").loginProcessingUrl("upLogin")
                .usernameParameter("username").passwordParameter("password");
        return httpSecurity.build();
    }



}
```