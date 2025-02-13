Spring Boot 依赖于 Spring 的核心功能之一 —— **IoC (控制反转)** 和 **依赖注入 (DI)**。Spring Boot 将这些功能简化并实现自动配置，从而使开发者能够专注于业务逻辑的开发，而不是框架配置。

在 Spring 中，IoC 的核心思想是**控制反转**，即将对象的创建和管理交给 Spring 容器，而不是在应用程序中手动创建和管理这些对象。

### **IoC的基本概念**

**控制反转 (IoC)**，顾名思义，就是将控制权反转给容器或框架。在传统的应用程序中，通常是程序代码负责对象的创建、初始化和依赖关系的管理。而在 Spring IoC 中，控制权被交给了 Spring 容器，容器负责这些任务。开发者只需要定义好对象的需求和依赖，Spring IoC 容器会在应用启动时完成对象的创建、管理和注入。

**依赖注入 (DI)** 是 IoC 的一种实现方式，它通过将对象的依赖关系注入到目标对象中，避免了在目标对象内部创建依赖对象的代码。

### **Spring Boot 中的 IoC 容器**

在 Spring Boot 中，IoC 容器是基于 **Spring Context** 的。Spring Boot 自动为应用程序创建一个默认的 Spring 应用上下文，它会扫描项目中的所有组件，并根据配置管理 Bean 的生命周期。以下是 Spring Boot IoC 的关键组件和如何使用它们：

### **1. @SpringBootApplication**

`@SpringBootApplication` 注解是一个组合注解，通常用于 Spring Boot 应用的启动类，它包括了 `@Configuration`、`@EnableAutoConfiguration` 和 `@ComponentScan` 等注解。这些注解的作用如下：

- `@Configuration`：标识当前类为配置类，它会被 Spring IoC 容器作为配置文件来读取。
- `@EnableAutoConfiguration`：启用 Spring Boot 的自动配置功能，自动配置应用所需的各个组件。
- `@ComponentScan`：启用组件扫描机制，它告诉 Spring IoC 容器扫描当前包及其子包中的所有类，并将标有 `@Component`、`@Service`、`@Repository`、`@Controller` 等注解的类注册为 Bean。

### **2. Bean 的创建与管理**

在 Spring Boot 中，我们通过各种注解来定义 Bean，Spring 容器会根据这些注解自动创建对象，并管理它们的生命周期。

常用的注解包括：

- `@Component`：将类声明为一个 Spring 管理的组件。Spring 会将标注了该注解的类自动注册为 Bean。
- `@Service`：通常用于标注服务层的 Bean，功能与 `@Component` 相同，但语义上表示该类是一个业务服务类。
- `@Repository`：用于标注数据访问层的 Bean，功能与 `@Component` 相同，表示该类是一个数据访问对象（DAO）。
- `@Controller`：用于标注控制层的 Bean，功能与 `@Component` 相同，表示该类是一个 Spring MVC 控制器。
- `@RestController`：结合了 `@Controller` 和 `@ResponseBody` 注解，用于 RESTful Web 服务的控制器。

### **3. 依赖注入 (DI)**

依赖注入是 IoC 的实现机制，它通过将类的依赖对象自动注入到类中，减少了开发人员手动创建对象的麻烦。Spring Boot 支持多种方式的依赖注入：

#### **(1) 构造器注入**

构造器注入通过构造方法注入依赖的 Bean。Spring 会通过反射机制调用构造方法，并将需要的 Bean 自动注入。

```java
@Service
public class MyService {
    private final MyRepository myRepository;

    // 构造器注入
    @Autowired
    public MyService(MyRepository myRepository) {
        this.myRepository = myRepository;
    }
}
```

#### **(2) 字段注入**

字段注入是通过直接在字段上使用 `@Autowired` 注解来自动注入依赖。Spring 会自动为字段注入匹配的 Bean。

```java
@Service
public class MyService {
    @Autowired
    private MyRepository myRepository;
}
```

#### **(3) Setter 注入**

Setter 注入通过类的 setter 方法来注入依赖。Spring 会在 Bean 初始化时调用对应的 setter 方法。

```java
@Service
public class MyService {
    private MyRepository myRepository;

    @Autowired
    public void setMyRepository(MyRepository myRepository) {
        this.myRepository = myRepository;
    }
}
```

### **4. @Configuration 和 @Bean 注解**

虽然 Spring Boot 可以通过 `@Component` 注解自动扫描和管理 Bean，但有时我们需要自定义 Bean 的创建过程，通常这时会用到 `@Configuration` 和 `@Bean` 注解。

- **@Configuration**：定义一个配置类，告诉 Spring 该类包含一些 Bean 定义。
- **@Bean**：用于将方法返回的对象注册为 Spring 容器中的 Bean。

```java
@Configuration
public class MyConfig {
    @Bean
    public MyService myService() {
        return new MyService();
    }
}
```

### **5. Bean 生命周期管理**

Spring Boot 中，Bean 的生命周期是由 Spring 容器管理的。容器负责 Bean 的实例化、初始化、销毁等过程。Spring 提供了以下几种方式来干预 Bean 的生命周期：

- **@PostConstruct**：该注解用于方法上，表示该方法会在 Bean 被初始化后执行（即 Bean 完成所有依赖注入后执行）。
- **@PreDestroy**：该注解用于方法上，表示该方法会在 Bean 被销毁前执行（即 Spring 容器销毁 Bean 前执行）。

### **6. 自动装配**

Spring Boot 的 IoC 容器还具有自动装配的功能，自动装配使得开发人员无需显式地指定 Bean 的引用，Spring 会自动选择并注入 Bean。

#### **(1) @Autowired**

通过 `@Autowired` 注解，Spring 会根据类型自动匹配需要注入的 Bean。

```java
@Autowired
private MyService myService;
```

#### **(2) @Qualifier**

如果容器中有多个符合要求的 Bean，`@Qualifier` 注解可帮助指定要注入的 Bean。

```java
@Autowired
@Qualifier("myServiceImpl")
private MyService myService;
```

### **总结**

Spring Boot 中的 IoC（控制反转）机制通过管理 Bean 的生命周期和依赖注入（DI）来实现高效的对象创建与依赖关系管理。通过自动装配、组件扫描、构造器注入、字段注入等方式，Spring Boot 简化了配置工作，提升了开发效率。开发者只需要专注于业务逻辑的编写，而 Spring 容器会自动处理对象的创建、初始化和依赖管理，从而减少了代码的耦合度，提高了可维护性。