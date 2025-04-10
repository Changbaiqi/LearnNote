---
title: MyBatis逆向生成使用
date: 2025-04-09 00:57:20
author: 长白崎
categories:
  - ["Mybatis"]
tags:
  - "Mybatis"
---

# MyBatis逆向生成使用

---

## 一、导入Mybatis-generator逆向插件

* 在`pom.xml`文件中加入相应插件配置

```xml
<plugins>
           <!--      mybatis-generator 逆向工具插件   -->
            <!-- mybatis代码生成插件 -->
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.4.1</version>
                <configuration>
                    <!--配置文件的位置-->
                    <configurationFile>src/main/resources/generatorConfig.xml</configurationFile>
                    <verbose>true</verbose>
                    <!--允许覆盖-->
                    <overwrite>true</overwrite>
                </configuration>
                <executions>
                    <execution>
                        <id>Generate MyBatis Artifacts</id>
                        <goals>
                            <goal>generate</goal>
                        </goals>
                    </execution>
                </executions>
               <!--   插件的依赖   -->
                <dependencies>
                    <dependency>
                        <groupId>org.mybatis.generator</groupId>
                        <artifactId>mybatis-generator-core</artifactId>
                        <version>1.4.1</version>
                    </dependency>
                    <!--mysql驱动依赖-->
                    <dependency>
                        <groupId>mysql</groupId>
                        <artifactId>mysql-connector-java</artifactId>
                        <version>8.0.33</version>
                    </dependency>
                </dependencies>
            </plugin>
      </plugins>

```

这里需要注意一下`configurationFile`对应配置信息的内容，这里的路径对应之后需要你创建的配置文件路径。



## 二、创建MyBatis逆向配置文件

以下为配置文件模板

`generatorConfig.xml`：注意此配置文件所在地方为第一步中插件配置中指定的路径

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <!--mysql 连接数据库jar 这里选择自己本地位置;
    如果不知道maven本地仓库地址，可以使用EveryThing工具全局搜索mysql-connector-java，找到jar包位置；
    也可以手动下载一个jar放在指定位置，进行引用。
    -->
    <!--    这里如果没有插件依赖没有引入mysql驱动的话就加这个指定mysql驱动依赖位置-->
<!--    <classPathEntry location="D:\Maven\repository\com\mysql\mysql-connector-j\8.0.33\mysql-connector-j-8.0.33.jar"/>-->
    <!--
            targetRuntime有两个值：
                MyBatis3Simple：生成的是基础版，只有基本的增删改查。
                MyBatis3：生成的是增强版，除了基本的增删改查之外还有复杂的增删改查。
        -->
    <context id="testTables" targetRuntime="MyBatis3">
        <!--防止生成重复代码-->
        <plugin type="org.mybatis.generator.plugins.UnmergeableXmlMappersPlugin"/>
        <commentGenerator>
            <!--是否去掉生成日期-->
            <property name="suppressDate" value="true"/>
            <!-- 是否去除自动生成的注释,true：是,false:否 -->
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>

        <!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/smstwo"
                        userId="root"
                        password="ht520520">
        </jdbcConnection>

        <!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL 和
           NUMERIC 类型解析为java.math.BigDecimal -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>

        <!-- 指定javaBean生成的位置
            targetPackage：生成的类要放的包，真实的包受enableSubPackages属性控制；
            targetProject：目标项目，指定一个存在的目录下，生成的内容会放到指定目录中，如果目录不存在，MBG不会自动建目录
         -->
        <javaModelGenerator targetPackage="com.SMS.domain" targetProject="src/main/java">
            <!-- 在targetPackage的基础上，根据数据库的schema再生成一层package，最终生成的类放在这个package下，默认为false；如果多个数据库改为true分目录 -->
            <property name="enableSubPackages" value="false"/>
            <!-- 设置是否在getter方法中，对String类型字段调用trim()方法 -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

        <!--  指定mapper映射文件生成的位置,也就是mapper.xml
           targetPackage、targetProject同javaModelGenerator中作用一样-->
        <sqlMapGenerator targetPackage="com.SMS.dao" targetProject="src/main/java">
            <property name="enableSubPackages" value="false"/>
        </sqlMapGenerator>
        <!--也可设置到Resource里面-->
<!--        <sqlMapGenerator targetPackage="mybatis.mapper" targetProject="vhr-web\src\main\resources"/>-->

        <!-- 指定mapper接口生成的位置
         targetPackage、targetProject同javaModelGenerator中作用一样
         -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.SMS.dao"
                             targetProject="src/main/java">
            <property name="enableSubPackages" value="false"/>
        </javaClientGenerator>

        <!-- 指定数据库表
        domainObjectName：生成的domain类的名字,当表名和domain类的名字有差异时一定要设置，如果不设置，直接使用表名作为domain类的名字；
        可以设置为somepck.domainName，那么会自动把domainName类再放到somepck包里面；
        -->
        <table tableName="t_userlogin" domainObjectName="User"></table>
        <table tableName="t_class" domainObjectName="MyClass"></table>
        <table tableName="t_course" domainObjectName="Course"></table>
        <table tableName="t_dept" domainObjectName="Dept"></table>
        <table tableName="t_leave" domainObjectName="Leave"></table>
        <table tableName="t_student" domainObjectName="Student"></table>
        <table tableName="t_tearcher" domainObjectName="Tearcher"></table>
    </context>
</generatorConfiguration>

```

