Spring 的动态代理是一种基于 Java 反射机制的技术，允许在运行时生成代理类，拦截对目标对象方法的调用。Spring 主要使用两种方式来实现动态代理：

1. **JDK 动态代理**：
    
    - 适用于接口类型的代理。
    - 它使用 `java.lang.reflect.Proxy` 类和 `InvocationHandler` 接口来实现。
    - 在运行时，Spring 会为实现了某个接口的类生成一个代理对象，代理对象会转发方法调用到目标对象。
    - 主要用于没有接口或者仅需要接口级别的代理的场景。
    
    **示例代码**：
    
    ```java
    public class MyService {
        public void doSomething() {
            System.out.println("Doing something...");
        }
    }
    
    public interface MyServiceInterface {
        void doSomething();
    }
    
    public class MyServiceProxy implements InvocationHandler {
        private Object target;
    
        public MyServiceProxy(Object target) {
            this.target = target;
        }
    
        @Override
        public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
            System.out.println("Before method call");
            Object result = method.invoke(target, args);
            System.out.println("After method call");
            return result;
        }
    }
    
    public class ProxyExample {
        public static void main(String[] args) {
            MyService myService = new MyService();
            MyServiceInterface proxy = (MyServiceInterface) Proxy.newProxyInstance(
                MyService.class.getClassLoader(),
                new Class[] { MyServiceInterface.class },
                new MyServiceProxy(myService)
            );
            proxy.doSomething();
        }
    }
    ```
    
2. **CGLIB 动态代理**：
    
    - 适用于没有接口的类。
    - 它通过继承目标类并重写方法来创建代理对象，而不需要目标类实现接口。
    - CGLIB 是一个第三方库，Spring 使用它来为没有实现接口的类生成代理。
    
    **示例代码**：
    
    ```java
    public class MyService {
        public void doSomething() {
            System.out.println("Doing something...");
        }
    }
    
    public class MyServiceInterceptor implements MethodInterceptor {
        @Override
        public Object invoke(MethodInvocation invocation) throws Throwable {
            System.out.println("Before method call");
            Object result = invocation.proceed();  // 调用目标方法
            System.out.println("After method call");
            return result;
        }
    }
    
    public class ProxyExample {
        public static void main(String[] args) {
            Enhancer enhancer = new Enhancer();
            enhancer.setSuperclass(MyService.class);
            enhancer.setCallback(new MyServiceInterceptor());
            MyService proxy = (MyService) enhancer.create();
            proxy.doSomething();
        }
    }
    ```
    

**Spring 中动态代理的应用场景**：

- **AOP（面向切面编程）**：Spring 使用动态代理来实现 AOP 功能，允许你在方法调用前后插入额外的行为（如日志记录、事务管理等）。
- **事务管理**：Spring 利用动态代理自动创建事务管理代理类，确保方法执行时能够自动提交或回滚事务。

Spring 默认使用 JDK 动态代理，如果目标类实现了接口，Spring 会使用 JDK 动态代理；如果目标类没有接口，则使用 CGLIB 动态代理。