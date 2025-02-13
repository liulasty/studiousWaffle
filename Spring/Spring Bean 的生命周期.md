在Spring框架中，Bean的生命周期经历以下几个阶段：

1）**实例化：** Spring 根据配置文件或注解等方式创建 Bean 实例，并将其存储在容器中。

2）**属性赋值：** Spring 自动将Bean的属性值从配置文件或注解等方式中注入到 Bean 实例中。

3）**初始化：** Spring 调用 Bean 实例的 `init-method`方法，完成一些初始化的操作，例如建立数据库连接等。

4）**使用：** Bean 实例已经可以正常使用，供应用程序调用。

5）**销毁：** Spring 调用 Bean 实例的 `destroy-method` 方法，完成一些资源的释放和清理操作，例如关闭数据库连接等。

### Bean生命周期详解

1）解析类得到 BeanDefinition。

2）如果有多个构造方法，则推断构造方法。

3）确定好构造方法后，进行实例化得到一个对象。

4）对对象中加了 `@Autowired` 注解的属性进行属性填充。

5）回调 Aware 方法，如 BeanNameAware，BeanFactoryAware。

6）调用 BeanPostProcessor 的初始化前的方法。

7）调用初始化方法。

8）调用 BeanPostProcessor 的初始化后的方法，在这里进行 AOP。

9）如果当前创建的 Bean 是单例的，则会把 Bean 放入单例池里。

10）使用 Bean。

11）Spring 容器关闭时调用 DisposableBean 中的 `destroy()` 方法。

## Bean作用域

Spring 框架提供了以下几种常见的 Bean 作用域：

1）**Singleton（单例）：** Spring 默认的作用域。在 Singleton 作用域下，Spring 容器只会创建 Bean 的一个实例，并在整个应用程序的生命周期内都共享这个实例。

2）**Prototype（原型）：** 每次请求该 Bean 时，Spring 容器都会创建一个新的实例，并返回该实例。即每次调用 `getBean()` 方法时都会返回一个新的 Bean实例。适用于那些需要状态的 Bean，例如控制器对象、线程安全的对象等。

3）**Request（请求）：** 在 Web 应用中，每个HTTP请求都会创建一个新的 Bean 实例。该作用域仅在 Web 环境中有效，用于在每个 HTTP 请求处理期间创建新的Bean 实例。适用于那些需要在每个请求中保持状态的 Bean。

4）**Session（会话）：** 在 Web 应用中，每个用户会话都会创建一个新的 Bean 实例。该作用域仅在 Web 环境中有效，用于在每个用户会话期间创建新的 Bean 实例。适用于那些需要在整个会话期间保持状态的 Bean。

5）**Global Session（全局会话）：** 该作用域仅在基于 portlet 的 Web 应用中有效。在 portlet 环境中，每个全局会话都会创建一个新的 Bean 实例。