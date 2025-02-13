在Spring Boot中，`Bean`是Spring框架管理的对象，通常用来实现应用的核心逻辑和服务。Spring Boot通过依赖注入（Dependency Injection, DI）和控制反转（Inversion of Control, IoC）来管理这些Bean的生命周期和依赖关系。

---

## **1. 什么是Bean**

- **Bean的定义：**  
    在Spring中，`Bean`是一个受Spring IoC容器管理的对象，通常是一个类的实例。它可以通过配置文件、注解或者Java代码来声明。
    
- **Bean的主要特点：**
    
    - 由Spring容器实例化、配置和管理。
    - Bean的生命周期由容器控制。
    - 通过依赖注入解决对象之间的耦合问题。

---

## **2. Bean的创建方式**

### 1. **通过注解创建**

Spring Boot常用的注解方式：

#### ① @Component注解

```java
@Component
public class MyService {
    public void doSomething() {
        System.out.println("Doing something...");
    }
}
```

- 标注在类上，表示该类是一个Bean。
- 容器会扫描并自动注册该类。

#### ② @Service、@Repository、@Controller注解

这些注解是`@Component`的派生注解，分别用于不同的层次：

- `@Service`：用于标注服务层组件。
- `@Repository`：用于标注数据访问层组件。
- `@Controller`：用于标注控制器层组件（通常配合`@RestController`使用）。

#### ③ @Configuration + @Bean注解

```java
@Configuration
public class AppConfig {
    @Bean
    public MyService myService() {
        return new MyService();
    }
}
```

- `@Configuration`表示该类是一个配置类。
- `@Bean`方法返回的对象会被容器管理。

---

### 2. **通过XML配置创建（较少使用）**

在Spring传统项目中，可以通过XML配置文件定义Bean：

```xml
<beans xmlns="http://www.springframework.org/schema/beans" ...>
    <bean id="myService" class="com.example.MyService" />
</beans>
```

---

## **3. Bean的注入方式**

### 1. **@Autowired注解**

- 用于自动注入Bean。
- 支持构造器注入、字段注入和方法注入。

#### 构造器注入

```java
@Service
public class MyController {
    private final MyService myService;

    @Autowired
    public MyController(MyService myService) {
        this.myService = myService;
    }
}
```

#### 字段注入

```java
@Service
public class MyController {
    @Autowired
    private MyService myService;
}
```

#### 方法注入

```java
@Service
public class MyController {
    private MyService myService;

    @Autowired
    public void setMyService(MyService myService) {
        this.myService = myService;
    }
}
```

### 2. **@Qualifier注解**

当有多个相同类型的Bean时，用`@Qualifier`指定具体的Bean：

```java
@Service
public class MyController {
    @Autowired
    @Qualifier("specificService")
    private MyService myService;
}
```

---

## **4. Bean的生命周期**

1. **实例化**：Spring容器创建Bean实例。
2. **属性赋值**：通过依赖注入为Bean设置属性。
3. **初始化**：调用`@PostConstruct`标注的方法，或者实现`InitializingBean`接口的`afterPropertiesSet`方法。
4. **使用**：Bean被应用程序使用。
5. **销毁**：容器关闭时调用`@PreDestroy`标注的方法，或者实现`DisposableBean`接口的`destroy`方法。

示例：

```java
@Component
public class MyService {
    @PostConstruct
    public void init() {
        System.out.println("Initializing Bean");
    }

    @PreDestroy
    public void destroy() {
        System.out.println("Destroying Bean");
    }
}
```

---

## **5. Bean的作用域**

Spring提供多种作用域：

- **singleton**（默认）：整个Spring容器中只有一个实例。
- **prototype**：每次获取时都会创建一个新实例。
- **request**：每个HTTP请求对应一个Bean实例（Web应用）。
- **session**：每个HTTP会话对应一个Bean实例（Web应用）。
- **application**：每个ServletContext对应一个Bean实例（Web应用）。

通过`@Scope`注解指定作用域：

```java
@Component
@Scope("prototype")
public class MyPrototypeBean {
}
```

---

## **6. 总结**

在Spring Boot中，Bean是核心概念，贯穿整个开发过程。主要通过注解（如`@Component`、`@Service`、`@Configuration`等）创建和管理Bean，并通过`@Autowired`实现依赖注入。了解Bean的生命周期和作用域，有助于构建高效、易维护的应用程序。