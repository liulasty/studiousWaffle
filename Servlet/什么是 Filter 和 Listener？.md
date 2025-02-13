在 Java EE（Jakarta EE）开发中，`Filter` 和 `Listener` 是两个常用的概念，主要用于 Web 应用开发中的请求处理和事件监听。它们在 Servlet 技术中起着重要作用。

---

### **Filter（过滤器）**

#### **1. 定义**

- `Filter` 是 Servlet 规范中的一个组件，用于对进入 Servlet 的请求和响应进行预处理或后处理。
- 它可以拦截请求和响应，实现逻辑处理（如权限验证、日志记录、编码设置等）。

#### **2. 工作原理**

- `Filter` 位于客户端请求和目标资源（如 Servlet、JSP）之间。
- 每次请求到达目标资源前，都会经过过滤器链，过滤器可以对请求或响应进行修改。

#### **3. 典型用例**

- 验证用户权限。
- 实现跨域资源共享（CORS）。
- 记录请求和响应日志。
- 数据压缩或解压缩。
- 设置请求编码（如 UTF-8）。

#### **4. 使用示例**

1. **创建一个 Filter 类**：
    
    ```java
    import javax.servlet.*;
    import java.io.IOException;
    
    public class MyFilter implements Filter {
        @Override
        public void init(FilterConfig filterConfig) throws ServletException {
            System.out.println("Filter initialized");
        }
    
        @Override
        public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
                throws IOException, ServletException {
            System.out.println("Before processing request");
            chain.doFilter(request, response); // 继续调用下一个过滤器或目标资源
            System.out.println("After processing request");
        }
    
        @Override
        public void destroy() {
            System.out.println("Filter destroyed");
        }
    }
    ```
    
2. **注册过滤器**（在 `web.xml` 中）：
    
    ```xml
    <filter>
        <filter-name>MyFilter</filter-name>
        <filter-class>com.example.MyFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>MyFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    ```
    
    或使用注解（Servlet 3.0+）：
    
    ```java
    @WebFilter("/*")
    public class MyFilter implements Filter {
        // Implementation here
    }
    ```
    

---

### **Listener（监听器）**

#### **1. 定义**

- `Listener` 是一种特殊的 Java 类，用于监听特定的事件并对事件作出响应。
- 它可以监听 ServletContext、HttpSession 和 ServletRequest 的生命周期变化，也可以监听属性的添加、修改和删除。

#### **2. 类型**

- **ServletContextListener**：监听 Web 应用的启动和销毁事件。
- **HttpSessionListener**：监听会话（Session）的创建和销毁。
- **ServletRequestListener**：监听请求的创建和销毁。
- **属性监听器**：
    - **ServletContextAttributeListener**：监听应用上下文属性的变化。
    - **HttpSessionAttributeListener**：监听会话属性的变化。
    - **ServletRequestAttributeListener**：监听请求属性的变化。

#### **3. 典型用例**

- 初始化资源（如数据库连接池、读取配置文件）。
- 统计在线用户数量。
- 记录用户会话。
- 监听用户行为。

#### **4. 使用示例**

1. **创建一个 Listener 类**：
    
    - 示例：统计在线用户数量：
    
    ```java
    import javax.servlet.http.HttpSessionEvent;
    import javax.servlet.http.HttpSessionListener;
    
    public class OnlineUserListener implements HttpSessionListener {
        private static int onlineUsers = 0;
    
        @Override
        public void sessionCreated(HttpSessionEvent se) {
            onlineUsers++;
            System.out.println("Session created. Online users: " + onlineUsers);
        }
    
        @Override
        public void sessionDestroyed(HttpSessionEvent se) {
            onlineUsers--;
            System.out.println("Session destroyed. Online users: " + onlineUsers);
        }
    }
    ```
    
2. **注册监听器**（在 `web.xml` 中）：
    
    ```xml
    <listener>
        <listener-class>com.example.OnlineUserListener</listener-class>
    </listener>
    ```
    
    或使用注解：
    
    ```java
    @WebListener
    public class OnlineUserListener implements HttpSessionListener {
        // Implementation here
    }
    ```
    

---

### **Filter 与 Listener 的区别**

|**特性**|**Filter**|**Listener**|
|---|---|---|
|**作用**|用于拦截请求和响应，进行逻辑处理|用于监听特定事件并作出响应|
|**工作范围**|请求和响应流|生命周期事件或属性变化|
|**应用场景**|预处理/后处理请求，如权限检查、编码设置|资源初始化、统计在线人数、属性监听等|
|**触发时机**|每次请求触发|生命周期事件或属性变化触发|
|**实现接口**|`javax.servlet.Filter`|`javax.servlet.*Listener`|

---

如果需要进一步了解或扩展某一部分，欢迎继续提问！