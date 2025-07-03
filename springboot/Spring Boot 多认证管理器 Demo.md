# Spring Boot 多认证管理器 Demo 结构

下面是一个完整的 Spring Boot 多认证管理器 Demo 项目结构，包含所有必要的组件和配置。

## 项目结构

```
multi-auth-demo/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── multiauthdemo/
│   │   │               ├── config/
│   │   │               │   └── SecurityConfig.java
│   │   │               ├── controller/
│   │   │               │   └── AuthController.java
│   │   │               ├── security/
│   │   │               │   ├── provider/
│   │   │               │   │   └── SmsAuthenticationProvider.java
│   │   │               │   ├── filter/
│   │   │               │   │   └── SmsAuthenticationFilter.java
│   │   │               │   ├── token/
│   │   │               │   │   └── SmsAuthenticationToken.java
│   │   │               │   └── service/
│   │   │               │       ├── PasswordUserDetailsService.java
│   │   │               │       └── SmsUserDetailsService.java
│   │   │               └── MultiAuthDemoApplication.java
│   │   └── resources/
│   │       └── application.properties
│   └── test/
│       └── java/
└── pom.xml
```

## 核心代码实现

### 1. 主应用类

`MultiAuthDemoApplication.java`

```java
package com.example.multiauthdemo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MultiAuthDemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(MultiAuthDemoApplication.class, args);
    }
}
```

### 2. 安全配置类

`SecurityConfig.java`

```java
package com.example.multiauthdemo.config;

import com.example.multiauthdemo.security.filter.SmsAuthenticationFilter;
import com.example.multiauthdemo.security.provider.SmsAuthenticationProvider;
import com.example.multiauthdemo.security.service.PasswordUserDetailsService;
import com.example.multiauthdemo.security.service.SmsUserDetailsService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.annotation.Order;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.authentication.ProviderManager;
import org.springframework.security.authentication.dao.DaoAuthenticationProvider;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.crypto.factory.PasswordEncoderFactories;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

import static org.springframework.security.config.Customizer.withDefaults;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public AuthenticationManager passwordAuthenticationManager(
            PasswordUserDetailsService userDetailsService,
            PasswordEncoder passwordEncoder) {
        DaoAuthenticationProvider provider = new DaoAuthenticationProvider();
        provider.setUserDetailsService(userDetailsService);
        provider.setPasswordEncoder(passwordEncoder);
        return new ProviderManager(provider);
    }

    @Bean
    public AuthenticationManager smsAuthenticationManager(
            SmsAuthenticationProvider smsAuthenticationProvider) {
        return new ProviderManager(smsAuthenticationProvider);
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return PasswordEncoderFactories.createDelegatingPasswordEncoder();
    }

    @Bean
    public SmsAuthenticationProvider smsAuthenticationProvider(
            SmsUserDetailsService smsUserDetailsService) {
        return new SmsAuthenticationProvider(smsUserDetailsService);
    }

    @Bean
    public SmsAuthenticationFilter smsAuthenticationFilter(
            AuthenticationManager smsAuthenticationManager) {
        SmsAuthenticationFilter filter = new SmsAuthenticationFilter();
        filter.setAuthenticationManager(smsAuthenticationManager);
        return filter;
    }

    @Bean
    @Order(1)
    public SecurityFilterChain securityFilterChain(
            HttpSecurity http,
            AuthenticationManager passwordAuthenticationManager,
            SmsAuthenticationFilter smsAuthenticationFilter) throws Exception {

        // 密码认证路径
        http.securityMatcher("/api/password/**")
            .authorizeHttpRequests(auth -> auth.anyRequest().authenticated())
            .authenticationManager(passwordAuthenticationManager)
            .httpBasic(withDefaults());

        // 短信认证路径
        http.securityMatcher("/api/sms/**")
            .authorizeHttpRequests(auth -> auth.anyRequest().authenticated())
            .addFilterBefore(smsAuthenticationFilter, UsernamePasswordAuthenticationFilter.class);

        // 公共路径
        http.securityMatcher("/api/public/**")
            .authorizeHttpRequests(auth -> auth.anyRequest().permitAll());

        // 默认拒绝所有未匹配的请求
        http.authorizeHttpRequests(auth -> auth.anyRequest().denyAll())
            .csrf().disable();

        return http.build();
    }
}
```

### 3. 用户服务实现

`PasswordUserDetailsService.java`

```java
package com.example.multiauthdemo.security.service;

import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

import java.util.Collections;

@Service
public class PasswordUserDetailsService implements UserDetailsService {

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        if ("user".equals(username)) {
            return new User(username, "{noop}password", Collections.emptyList());
        }
        throw new UsernameNotFoundException("User not found");
    }
}
```

`SmsUserDetailsService.java`

```java
package com.example.multiauthdemo.security.service;

import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

import java.util.Collections;

@Service
public class SmsUserDetailsService implements UserDetailsService {

    @Override
    public UserDetails loadUserByUsername(String mobile) throws UsernameNotFoundException {
        if ("13800000000".equals(mobile)) {
            return new User(mobile, "{noop}", Collections.emptyList());
        }
        throw new UsernameNotFoundException("Mobile not found");
    }
}
```

### 4. 短信认证组件

`SmsAuthenticationToken.java`

```java
package com.example.multiauthdemo.security.token;

import org.springframework.security.authentication.AbstractAuthenticationToken;
import org.springframework.security.core.GrantedAuthority;

import java.util.Collection;

public class SmsAuthenticationToken extends AbstractAuthenticationToken {

    private final Object principal;
    private Object credentials;

    public SmsAuthenticationToken(Object principal, Object credentials) {
        super(null);
        this.principal = principal;
        this.credentials = credentials;
        setAuthenticated(false);
    }

    public SmsAuthenticationToken(Object principal, Object credentials,
                                 Collection<? extends GrantedAuthority> authorities) {
        super(authorities);
        this.principal = principal;
        this.credentials = credentials;
        super.setAuthenticated(true);
    }

    @Override
    public Object getCredentials() {
        return credentials;
    }

    @Override
    public Object getPrincipal() {
        return principal;
    }
}
```

`SmsAuthenticationFilter.java`

```java
package com.example.multiauthdemo.security.filter;

import com.example.multiauthdemo.security.token.SmsAuthenticationToken;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.authentication.AbstractAuthenticationProcessingFilter;
import org.springframework.security.web.util.matcher.AntPathRequestMatcher;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class SmsAuthenticationFilter extends AbstractAuthenticationProcessingFilter {

    public SmsAuthenticationFilter() {
        super(new AntPathRequestMatcher("/api/sms/login", "POST"));
    }

    @Override
    public Authentication attemptAuthentication(HttpServletRequest request,
                                               HttpServletResponse response) throws AuthenticationException {
        String mobile = request.getParameter("mobile");
        String code = request.getParameter("code");

        SmsAuthenticationToken authRequest = new SmsAuthenticationToken(mobile, code);
        return this.getAuthenticationManager().authenticate(authRequest);
    }
}
```

`SmsAuthenticationProvider.java`

```java
package com.example.multiauthdemo.security.provider;

import com.example.multiauthdemo.security.service.SmsUserDetailsService;
import com.example.multiauthdemo.security.token.SmsAuthenticationToken;
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.authentication.BadCredentialsException;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.stereotype.Component;

@Component
public class SmsAuthenticationProvider implements AuthenticationProvider {

    private final SmsUserDetailsService userDetailsService;

    public SmsAuthenticationProvider(SmsUserDetailsService userDetailsService) {
        this.userDetailsService = userDetailsService;
    }

    @Override
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {
        String mobile = (String) authentication.getPrincipal();
        String code = (String) authentication.getCredentials();

        // 验证短信验证码逻辑 (实际项目中应从缓存或数据库验证)
        if (!"123456".equals(code)) {
            throw new BadCredentialsException("Invalid SMS code");
        }

        UserDetails userDetails = userDetailsService.loadUserByUsername(mobile);
        return new SmsAuthenticationToken(userDetails, code, userDetails.getAuthorities());
    }

    @Override
    public boolean supports(Class<?> authentication) {
        return SmsAuthenticationToken.class.isAssignableFrom(authentication);
    }
}
```

### 5. 控制器类

`AuthController.java`

```java
package com.example.multiauthdemo.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api")
public class AuthController {

    @GetMapping("/public/hello")
    public String publicHello() {
        return "Hello Public!";
    }

    @GetMapping("/password/hello")
    public String passwordHello() {
        return "Hello Password User!";
    }

    @GetMapping("/sms/hello")
    public String smsHello() {
        return "Hello SMS User!";
    }
}
```

### 6. 应用配置

`application.properties`

```properties
# 服务器端口
server.port=8080

# 日志配置
logging.level.org.springframework.security=DEBUG
```

### 7. Maven 依赖

`pom.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.3.7</version>
        <relativePath/>
    </parent>

    <groupId>com.example</groupId>
    <artifactId>multi-auth-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>multi-auth-demo</name>
    <description>Demo project for multiple authentication managers</description>

    <properties>
        <java.version>17</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-test</artifactId>
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

## 测试端点

1. **公共端点** (无需认证):
   
   ```
   GET http://localhost:8080/api/public/hello
   ```

2. **密码认证端点** (使用Basic认证):
   
   ```
   GET http://localhost:8080/api/password/hello
   Headers:
   Authorization: Basic dXNlcjpwYXNzd29yZA==
   ```

3. **短信认证端点** (需要先获取token):
   
   ```
   POST http://localhost:8080/api/sms/login
   Body:
   mobile=13800000000&code=123456
   ```
   
   然后使用返回的token访问:
   
   ```
   GET http://localhost:8080/api/sms/hello
   Headers:
   Authorization: Bearer <token>
   ```

这个Demo项目完整展示了如何在Spring Boot中配置多个认证管理器，并针对不同的认证路径应用不同的认证方式。
