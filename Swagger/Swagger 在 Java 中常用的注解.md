Swagger 是一个流行的用于 API 文档生成的工具，尤其在 Java 中使用 Spring Boot 时非常常见。以下是 Swagger 在 Java 中常用的一些注解及其详细解释和示例：

### 1. `@Api`
- **解释**：用于标记整个控制器类，提供对这个 API 的描述。
- **示例**：

```java
import io.swagger.annotations.Api;
import org.springframework.web.bind.annotation.RestController;

@Api(value = "User Management System", description = "Operations pertaining to user in User Management System")
@RestController
public class UserController {
    // Controller methods
}
```

### 2. `@ApiOperation`
- **解释**：用于标记一个具体的操作或 API 方法，提供方法级别的描述。
- **示例**：

```java
import io.swagger.annotations.ApiOperation;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@ApiOperation(value = "Get a user by ID", response = User.class)
@GetMapping("/user/{id}")
public User getUserById(@PathVariable Long id) {
    // Method implementation
    return new User(id, "John Doe");
}
```

### 3. `@ApiParam`
- **解释**：用于描述方法参数。
- **示例**：

```java
import io.swagger.annotations.ApiParam;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestParam;

@GetMapping("/user/{id}")
public User getUserById(
    @ApiParam(value = "ID of the user to fetch", required = true) @PathVariable Long id,
    @ApiParam(value = "Whether to include detailed information", defaultValue = "false") @RequestParam(required = false) boolean detailed) {
    // Method implementation
    return new User(id, "John Doe");
}
```

### 4. `@ApiModel`
- **解释**：用于描述模型类，即 API 返回的实体类。
- **示例**：

```java
import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;

@ApiModel(description = "Details about the user")
public class User {

    @ApiModelProperty(notes = "The unique ID of the user")
    private Long id;

    @ApiModelProperty(notes = "The name of the user")
    private String name;

    // Constructors, getters, and setters
    public User(Long id, String name) {
        this.id = id;
        this.name = name;
    }

    // Getters and setters
}
```

### 5. `@ApiResponse` 和 `@ApiResponses`
- **解释**：用于描述单个或多个 API 响应。
- **示例**：

```java
import io.swagger.annotations.ApiResponse;
import io.swagger.annotations.ApiResponses;

@ApiOperation(value = "Delete a user by ID")
@ApiResponses(value = {
    @ApiResponse(code = 200, message = "Successfully deleted user"),
    @ApiResponse(code = 404, message = "The user you were trying to delete is not found")
})
@DeleteMapping("/user/{id}")
public void deleteUser(@PathVariable Long id) {
    // Method implementation
}
```

### 6. `@ApiImplicitParam` 和 `@ApiImplicitParams`
- **解释**：用于描述隐式参数（如查询参数）。
- **示例**：

```java
import io.swagger.annotations.ApiImplicitParam;
import io.swagger.annotations.ApiImplicitParams;

@ApiOperation(value = "Search users")
@ApiImplicitParams({
    @ApiImplicitParam(name = "name", value = "Name of the user to search", required = false, dataType = "string", paramType = "query")
})
@GetMapping("/users/search")
public List<User> searchUsers(@RequestParam(required = false) String name) {
    // Method implementation
    return Arrays.asList(new User(1L, "John Doe"), new User(2L, "Jane Doe"));
}
```

通过这些注解，Swagger 可以自动生成详细的 API 文档，使开发者能够更容易地理解和使用 API。