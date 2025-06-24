# **Spring Cloud 常见面试问题及答案**

Spring Cloud 是构建分布式系统的工具集，基于 Spring Boot 提供微服务架构的解决方案。以下是 Spring Cloud 的核心面试问题及详细解答：

---

## **1. Spring Cloud 核心组件**
### **Q 1: Spring Cloud 的核心组件有哪些？**
Spring Cloud 包含以下核心组件：
1. **服务注册与发现**  
   - **Eureka**（Netflix，已停止维护） / **Nacos**（阿里） / **Consul**（HashiCorp）  
2. **服务调用**  
   - **Ribbon**（客户端负载均衡） / **OpenFeign**（声明式 HTTP 客户端）  
3. **服务熔断与降级**  
   - **Hystrix**（Netflix，已停止维护） / **Resilience 4 j** / **Sentinel**（阿里）  
4. **API 网关**  
   - **Zuul**（Netflix，已停止维护） / **Spring Cloud Gateway**（官方推荐）  
5. **配置中心**  
   - **Spring Cloud Config**（Git 存储） / **Nacos Config**（动态配置）  
6. **分布式事务**  
   - **Seata**（阿里）  
7. **消息总线**  
   - **Spring Cloud Bus**（基于 RabbitMQ/Kafka）  
8. **链路追踪**  
   - **Sleuth + Zipkin**  

---

## **2. 服务注册与发现**
### **Q 2: Eureka 和 Nacos 的区别？**

| **对比项**       | **Eureka**                          | **Nacos**                          |
|------------------|-------------------------------------|------------------------------------|
| **服务发现**     | 仅支持服务注册与发现                | 支持服务注册、发现、动态配置        |
| **健康检查**     | 心跳检测（Client → Server）         | 支持 TCP/HTTP/MySQL 等多种检查方式 |
| **CAP 模型**     | AP（高可用）                        | AP + CP（可切换）                  |
| **配置管理**     | 不支持                              | 支持动态配置管理                   |
| **社区活跃度**   | 已停止维护                          | 阿里开源，持续更新                 |

**推荐**：新项目建议使用 **Nacos**（功能更全，支持动态配置）。

---

### **Q 3: Eureka 如何保证高可用？**
- **Eureka Server 集群**：多个 Eureka Server 互相注册（`eureka.client.service-url.defaultZone` 配置其他节点地址）。  
- **客户端缓存**：即使 Eureka Server 宕机，客户端仍能通过本地缓存获取服务列表。  
- **自我保护机制**：当网络分区发生时，Eureka Server 不会立即剔除失效的服务实例（避免误删）。  

---

## **3. 服务调用与负载均衡**
### **Q 4: Ribbon 和 OpenFeign 的区别？**

| **对比项**       | **Ribbon**                          | **OpenFeign**                      |
|------------------|-------------------------------------|------------------------------------|
| **定位**         | 客户端负载均衡                      | 声明式 HTTP 客户端（基于 Ribbon）  |
| **使用方式**     | 需手动调用 `RestTemplate`           | 接口注解（`@FeignClient`）         |
| **集成性**       | 需额外配置                          | 与 Spring 深度集成                 |
| **代码简洁性**   | 较低（需写 HTTP 调用代码）          | 高（类似 Spring MVC 接口）         |

**示例（OpenFeign）**：
```java
@FeignClient(name = "user-service")
public interface UserClient {
    @GetMapping("/user/{id}")
    User getUser(@PathVariable Long id);
}
```

---

### **Q 5: Ribbon 的负载均衡策略有哪些？**
- **轮询（RoundRobinRule）**：默认策略，按顺序分配请求。  
- **随机（RandomRule）**：随机选择服务器。  
- **权重（WeightedResponseTimeRule）**：根据响应时间动态调整权重。  
- **最小并发（BestAvailableRule）**：选择并发请求最少的服务器。  
- **区域感知（ZoneAvoidanceRule）**：优先选择同区域的服务器。  

**配置方式**：
```yaml
user-service:
  ribbon:
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule
```

---

## **4. 服务熔断与降级**
### **Q 6: Hystrix 和 Sentinel 的区别？**

| **对比项**       | **Hystrix**                        | **Sentinel**                      |
|------------------|-----------------------------------|-----------------------------------|
| **熔断策略**     | 基于线程池/信号量隔离              | 基于 QPS/响应时间/异常比例         |
| **动态规则**     | 不支持动态配置                     | 支持动态规则（通过 Nacos/Dashboard）|
| **流量控制**     | 不支持                             | 支持秒级 QPS 限流、熔断降级        |
| **社区支持**     | 已停止维护                         | 阿里开源，持续更新                |

**推荐**：新项目建议使用 **Sentinel**（功能更强大，支持动态规则）。

---

### **Q 7: 什么是服务雪崩？如何解决？**
- **雪崩效应**：一个服务故障导致依赖它的服务也崩溃（级联故障）。  
- **解决方案**：  
  1. **熔断（Circuit Breaker）**：快速失败（如 Hystrix/Sentinel）。  
  2. **降级（Fallback）**：返回默认值或缓存数据。  
  3. **限流（Rate Limiting）**：控制请求速率（如 Sentinel）。  
  4. **异步调用**：使用消息队列（如 RabbitMQ）解耦。  

**示例（Hystrix 降级）**：
```java
@HystrixCommand(fallbackMethod = "getUserFallback")
public User getUser(Long id) {
    // 调用远程服务
}

public User getUserFallback(Long id) {
    return new User("默认用户");
}
```

---

## **5. API 网关**
### **Q 8: Spring Cloud Gateway 和 Zuul 的区别？**

| **对比项**       | **Zuul 1. X**                      | **Spring Cloud Gateway**          |
|------------------|-----------------------------------|-----------------------------------|
| **性能**         | 基于 Servlet（阻塞 IO）           | 基于 Reactor（非阻塞，性能更高）   |
| **扩展性**       | 过滤器（Filter）机制              | 基于 WebFlux，支持自定义路由       |
| **社区支持**     | 已停止维护                        | 官方推荐，持续更新                |

**推荐**：新项目使用 **Spring Cloud Gateway**（性能更好，支持异步）。

---

### **Q 9: Spring Cloud Gateway 的核心概念？**
1. **路由（Route）**：定义请求转发规则（如 `/user/** → user-service`）。  
2. **断言（Predicate）**：匹配请求的条件（如 Path、Header、Method）。  
3. **过滤器（Filter）**：对请求/响应进行修改（如添加 Header、限流）。  

**示例（YAML 配置）**：
```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: user-service
          uri: lb://user-service
          predicates:
            - Path=/user/**
          filters:
            - AddRequestHeader=X-Request-ID, 123
```

---

## **6. 配置中心**
### **Q 10: Spring Cloud Config 和 Nacos Config 的区别？**

| **对比项**       | **Spring Cloud Config**           | **Nacos Config**                  |
|------------------|-----------------------------------|-----------------------------------|
| **存储方式**     | Git/文件系统                      | 内置数据库（支持动态刷新）         |
| **动态刷新**     | 需配合 Spring Cloud Bus           | 原生支持（`@RefreshScope`）        |
| **功能扩展**     | 仅配置管理                        | 集成服务发现、配置管理             |

**推荐**：使用 **Nacos Config**（动态刷新更便捷）。

---

## **7. 分布式事务**
### **Q 11: 什么是 CAP 理论？Spring Cloud 如何保证？**
- **CAP 理论**：分布式系统无法同时满足 **一致性（C）**、**可用性（A）** 和 **分区容错性（P）**。  
- **Spring Cloud 方案**：  
  - **AP 系统**（如 Eureka）：保证高可用，牺牲强一致性。  
  - **CP 系统**（如 ZooKeeper/Nacos CP 模式）：保证一致性，牺牲可用性。  

---

### **Q 12: Seata 的工作原理？**
Seata 采用 **AT 模式**（自动补偿事务）：
4. **TM（事务管理器）**：定义全局事务（`@GlobalTransactional`）。  
5. **RM（资源管理器）**：管理分支事务（如 MySQL 的 undo_log 表）。  
6. **TC（事务协调器）**：协调全局事务的提交/回滚。  

**流程**：
- **阶段一（提交）**：各分支事务执行 SQL，生成 undo_log。  
- **阶段二（提交/回滚）**：TC 根据全局事务状态决定提交或回滚（通过 undo_log 补偿）。  

---

## **8. 链路追踪**
### **Q 13: Sleuth + Zipkin 的作用？**
- **Sleuth**：生成唯一 Trace ID 和 Span ID，记录调用链路。  
- **Zipkin**：可视化追踪数据，分析延迟和依赖关系。  

**示例（集成 Zipkin）**：
```yaml
spring:
  zipkin:
    base-url: http://localhost:9411
  sleuth:
    sampler:
      probability: 1.0  # 100% 采样率
```

---

## **9. 高频面试题**
### **Q 14: 微服务架构的优缺点？**
**优点**：
- 松耦合，独立开发、部署、扩展。  
- 技术栈灵活（不同服务可用不同语言）。  

**缺点**：
- 运维复杂度高（需治理服务发现、熔断、链路追踪等）。  
- 分布式事务挑战。  

---

### **Q 15: 如何设计一个高可用的微服务系统？**
7. **服务注册与发现**（Nacos/Eureka 集群）。  
8. **负载均衡**（Ribbon/Gateway）。  
9. **熔断降级**（Sentinel/Hystrix）。  
10. **分布式配置**（Nacos Config）。  
11. **API 网关**（Spring Cloud Gateway）。  
12. **链路追踪**（Sleuth + Zipkin）。  
13. **监控告警**（Prometheus + Grafana）。  

---

## **总结**
Spring Cloud 面试重点：
14. **核心组件**（Eureka/Nacos、Ribbon/Feign、Hystrix/Sentinel、Gateway）。  
15. **服务治理**（注册发现、熔断降级、负载均衡）。  
16. **分布式问题**（CAP、事务、配置管理）。  
17. **实战经验**（如何设计高可用微服务架构）。  

**建议**：结合项目经验，说明如何解决具体问题（如“我们用 Sentinel 实现了熔断和限流”）。