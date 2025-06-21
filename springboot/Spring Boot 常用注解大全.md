# Spring Boot 常用注解大全

## 一、核心注解

1. **@SpringBootApplication**
   - 组合注解，包含以下三个注解：
     - `@Configuration`：标识该类为配置类
     - `@EnableAutoConfiguration`：启用自动配置
     - `@ComponentScan`：组件扫描

2. **@RestController**
   - 组合了 `@Controller` 和 `@ResponseBody`，用于创建 RESTful控制器

3. **@RequestMapping**
   - 映射 HTTP 请求到特定处理方法
   - 衍生注解：
     - `@GetMapping`
     - `@PostMapping`
     - `@PutMapping`
     - `@DeleteMapping`
     - `@PatchMapping`

## 二、依赖注入相关

4. **@Autowired**
   - 自动装配依赖对象

5. **@Component**
   - 通用组件注解
   - 衍生注解：
     - `@Service`：服务层组件
     - `@Repository`：数据访问层组件
     - `@Controller`：控制器组件

6. **@Qualifier**
   - 当有多个相同类型的 bean 时，指定要注入的 bean

7. **@Primary**
   - 当有多个相同类型的 bean 时，优先使用被标记的 bean

8. **@Bean**
   - 在配置类中声明一个 bean

## 三、配置相关

9. **@Configuration**
   - 声明一个类为配置类

10. **@Value**
   - 注入属性值
   - 示例：`@Value("${property.name}")`

11. **@ConfigurationProperties**
   - 批量注入配置属性到对象
   - 示例：`@ConfigurationProperties(prefix="app")`

12. **@PropertySource**
   - 指定属性文件位置
   - 示例：`@PropertySource("classpath:app.properties")`

## 四、Web 相关

13. **@RequestParam**
   - 绑定请求参数到方法参数
   - 示例：`@RequestParam("name") String name`

14. **@PathVariable**
   - 绑定 URL 模板变量到方法参数
   - 示例：`@PathVariable("id") Long id`

15. **@RequestBody**
   - 绑定请求体到方法参数

16. **@ResponseBody**
   - 将方法返回值绑定到响应体

17. **@CookieValue**
   - 绑定 cookie 值到方法参数

18. **@RequestHeader**
   - 绑定请求头到方法参数

19. **@SessionAttribute**
   - 访问会话属性

20. **@ModelAttribute**
   - 绑定方法参数或方法返回值到模型

## 五、数据访问相关

21. **@Entity**
   - JPA 实体类注解

22. **@Table**
   - 指定实体对应的表名

23. **@Id**
   - 标识主键字段

24. **@GeneratedValue**
   - 指定主键生成策略

25. **@Column**
   - 指定字段与列的映射

26. **@Transactional**
   - 声明事务

27. **@Repository**
   - 数据访问层组件

## 六、测试相关

28. **@SpringBootTest**
   - Spring Boot 集成测试

29. **@Test**
   - JUnit 测试方法

30. **@MockBean**
   - 模拟 bean 用于测试

31. **@DataJpaTest**
   - JPA 仓库测试

32. **@WebMvcTest**
   - MVC 控制器测试

## 七、AOP 相关

33. **@Aspect**
   - 声明一个切面

34. **@Pointcut**
   - 定义切点表达式

35. **@Before**
   - 前置通知

36. **@After**
   - 后置通知

37. **@AfterReturning**
   - 返回后通知

38. **@AfterThrowing**
   - 异常通知

39. **@Around**
   - 环绕通知

## 八、定时任务

40. **@EnableScheduling**
   - 启用定时任务

41. **@Scheduled**
   - 声明定时任务方法
   - 示例：`@Scheduled(cron="0 * * * * ?")`

## 九、缓存相关

42. **@EnableCaching**
   - 启用缓存

43. **@Cacheable**
   - 缓存方法结果

44. **@CacheEvict**
   - 清除缓存

45. **@CachePut**
   - 更新缓存

## 十、其他常用注解

46. **@Valid**
   - 启用参数验证

47. **@Validated**
   - 分组验证

48. **@ExceptionHandler**
   - 异常处理方法

49. **@ControllerAdvice**
   - 全局控制器增强

50. **@CrossOrigin**
   - 跨域支持

51. **@Profile**
   - 指定环境激活的 bean

52. **@Conditional**
   - 条件化创建 bean

53. **@Async**
   - 异步方法

54. **@EnableAsync**
   - 启用异步支持

55. **@EnableTransactionManagement**
    - 启用事务管理

56. **@Lazy**
    - 延迟初始化

57. **@Scope**
    - 指定 bean 的作用域

58. **@Order**
    - 指定 bean 的加载顺序

59. **@Import**
    - 导入其他配置类

60. **@EnableConfigurationProperties**
    - 启用配置属性绑定

61. **@EnableDiscoveryClient**
    - 启用服务发现客户端

62. **@FeignClient**
    - 声明 Feign 客户端

63. **@EnableFeignClients**
    - 启用 Feign 客户端

64. **@EnableCircuitBreaker**
    - 启用断路器

65. **@HystrixCommand**
    - Hystrix 命令注解
