# Spring Security Oauth2



## 环境搭建

**1.父工程**

`pom.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://maven.apache.org/POM/4.0.0"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.9.RELEASE</version>
        <relativePath/>
    </parent>

    <groupId>com.lomofu</groupId>
    <artifactId>spring-oauth2-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>spring-oauth2-demo</name>
    <packaging>pom</packaging>

    <modules>
        <module>auth-server</module>
    </modules>

    <properties>
        <java.version>11</java.version>
        <maven.compiler.source>${java.version}</maven.compiler.source>
        <maven.compiler.target>${java.version}</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR10</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>com.lomofu</groupId>
                <artifactId>auth-server</artifactId>
                <version>0.0.1-SNAPSHOT</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
```



2.**认证服务器**（子工程）

`pom.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://maven.apache.org/POM/4.0.0"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>spring-oauth2-demo</artifactId>
        <groupId>com.lomofu</groupId>
        <version>0.0.1-SNAPSHOT</version>
        <relativePath>../pom.xml</relativePath>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <packaging>jar</packaging>

    <artifactId>auth-server</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-oauth2</artifactId>
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



**出错**

```bash
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'configurationPropertiesBeans' defined in class path resource [org/springframework/cloud/autoconfigure/ConfigurationPropertiesRebinderAutoConfiguration.class]: Post-processing of merged bean definition failed; nested exception is java.lang.IllegalStateException: Failed to introspect Class [org.springframework.cloud.context.properties.ConfigurationPropertiesBeans] from ClassLoader [jdk.internal.loader.ClassLoaders$AppClassLoader@73d16e93]
```

> 截止文章发表时，根据本人的测试，由于`springboot` 与 `springcloud` 版本有着严格的对照原则，所以当我第一次使用springboot 2.4.x版本与spring-cloud-starter-oauth2 搭配会出现启动如上图错误，经过谷歌后得出一下 `springboot` 与 `springcloud`版本对应关系。
>
> 
>
> 解决（本次搭建所用环境版本）：
>
> `springcloud`: **Hoxton.SR10**
>
> `springboot`:**2.3.9.RELEASE**
>
> | Release Train       | Boot Version                     |
> | ------------------- | -------------------------------- |
> | 2020.0.x aka Ilford | 2.4.x                            |
> | Hoxton              | 2.2.x, 2.3.x (Starting with SR5) |
> | Greenwich           | 2.1.x                            |
> | Finchley            | 2.0.x                            |
> | Edgware             | 1.5.x                            |
> | Dalston             | 1.5.x                            |







## 基于内存

### 配置

1.认证服务器配置

`AuthorizationServerConfiguration.java`

```java
package com.lomofu.auth.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.oauth2.config.annotation.configurers.ClientDetailsServiceConfigurer;
import org.springframework.security.oauth2.config.annotation.web.configuration.AuthorizationServerConfigurerAdapter;

/**
 * @author lomofu
 * @desc
 * @create 02/Mar/2021 16:08
 */
@Configuration
public class AuthorizationServerConfiguration extends AuthorizationServerConfigurerAdapter {

  private final PasswordEncoder passwordEncoder;

  @Autowired
  public AuthorizationServerConfiguration(PasswordEncoder passwordEncoder) {
    this.passwordEncoder = passwordEncoder;
  }

  @Override
  public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
    withMemory(clients);
  }

  private void withMemory(ClientDetailsServiceConfigurer clients) throws Exception {
    clients
        .inMemory()
        .withClient("client")
        .secret(passwordEncoder.encode("secret"))
        .authorizedGrantTypes("authorization_code")
        .scopes("app")
        .redirectUris("https://www.lomofu.com");
  }
}
```

2.mock用户配置

`WebSecurityConfiguration.java`

```java
package com.lomofu.auth.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

/**
 * @author lomofu
 * @desc
 * @create 02/Mar/2021 16:17
 */
@Configuration
@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true, jsr250Enabled = true)
public class WebSecurityConfiguration extends WebSecurityConfigurerAdapter {

  @Bean
  public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
  }

  @Override
  protected void configure(AuthenticationManagerBuilder auth) throws Exception {
    withMemory(auth);
  }

  private void withMemory(AuthenticationManagerBuilder auth) throws Exception {
    auth.inMemoryAuthentication()
        .withUser("admin")
        .password(passwordEncoder().encode("admin"))
        .roles("admin");
  }
}
```

### 测试

1.浏览器访问以下地址

```url
http://localhost:8080/oauth/authorize?client_id=client&response_type=code
```

> localhost:8080 认证服务器的地址端口
>
> client_id 指定在 `AuthorizationServerConfiguration.java`配置的
>
> response_type code 授权码模式



**第一次访问**会调转至此登录界面

![image-20210302174145450](img/image-20210302174145450.png)



成功输入用户名和密码，即验证成功后会询问用户是否授权客户端

<img src="img/image-20210302174308699.png" alt="image-20210302174308699" style="zoom: 50%;" />



点击授权后，会跳转到之前指定的redirectUris,并返回一个**授权码(code=40TDzC)**

```
https://www.lomofu.com/?code=40TDzC
```



通过授权码向服务器申请令牌

1. CURL

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'grant_type=authorization_code&code=40TDzC' "http://client:secret@localhost:8080/oauth/token"
```

2. Postman

![image-20210302174819079](img/image-20210302174819079.png)

> Ps:
>
> 1.要使用**Post**请求
>
> 2.注意请求url的拼写 -> http://client_id(指定的client_id):secret(指定的密文)@认证服务器地址/oauth/token

 成功请求后即可看到返回结果中含有**access_token**

