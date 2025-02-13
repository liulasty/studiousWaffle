下面是一个实现 `HandlerMethodReturnValueHandler` 接口的示例代码，用于处理 `@Controller` 中的方法返回值：

```java
import org.springframework.core.MethodParameter;
import org.springframework.web.context.request.NativeWebRequest;
import org.springframework.web.method.support.HandlerMethodReturnValueHandler;
import org.springframework.web.method.support.ModelAndViewContainer;

public class CustomReturnValueHandler implements HandlerMethodReturnValueHandler {

    @Override
    public boolean supportsReturnType(MethodParameter returnType) {
        // 判断是否支持特定的返回类型
        return returnType.getParameterType().equals(MyCustomType.class);
    }

    @Override
    public void handleReturnValue(
            Object returnValue,
            MethodParameter returnType,
            ModelAndViewContainer mavContainer,
            NativeWebRequest webRequest) throws Exception {

        if (returnValue instanceof MyCustomType) {
            MyCustomType myValue = (MyCustomType) returnValue;
            // 处理返回值，例如，设置到模型或生成响应
            mavContainer.addAttribute("customData", myValue.getData());
            mavContainer.setViewName("myCustomView");
            // 标记响应已处理
            mavContainer.setRequestHandled(true);
        }
    }
}
```

### 说明

1. **`supportsReturnType` 方法**：
   - 用于判断当前处理器是否支持特定的返回类型。在这个示例中，处理器只支持 `MyCustomType` 类型。

2. **`handleReturnValue` 方法**：
   - 处理方法的返回值，将其转换为视图或模型的内容。
   - 使用 `mavContainer` 设置模型数据或视图名称。
   - 调用 `setRequestHandled(true)` 标记请求已经处理，避免进一步处理。

### 注册自定义处理器

需要在 Spring 配置中注册自定义的返回值处理器：

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.method.support.HandlerMethodReturnValueHandler;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import java.util.List;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addReturnValueHandlers(List<HandlerMethodReturnValueHandler> handlers) {
        handlers.add(new CustomReturnValueHandler());
    }
}
```

### 使用方法

1. 创建 `MyCustomType` 类，代表自定义的返回类型。
2. 编写控制器方法返回 `MyCustomType` 实例。
3. 在 Spring 应用程序中配置 `WebConfig`，注册 `CustomReturnValueHandler`。

通过这种方式，可以灵活地处理控制器方法的返回值。