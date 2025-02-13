在MyBatis-Plus中处理枚举类，可以使用 `IEnum` 接口或 `EnumValue` 注解，这两种方式都可以将枚举值映射到数据库字段中。以下是详细的操作步骤：

### 方式一：实现 `IEnum` 接口

1. **定义枚举类并实现 `IEnum` 接口**：
   ```java
   import com.baomidou.mybatisplus.annotation.IEnum;

   public enum GenderEnum implements IEnum<Integer> {
       MALE(1, "男"),
       FEMALE(2, "女");

       private final int value;
       private final String desc;

       GenderEnum(int value, String desc) {
           this.value = value;
           this.desc = desc;
       }

       @Override
       public Integer getValue() {
           return this.value;
       }

       public String getDesc() {
           return this.desc;
       }
   }
   ```

2. **在实体类中使用枚举类型**：
   ```java
   import com.baomidou.mybatisplus.annotation.TableId;
   import com.baomidou.mybatisplus.annotation.TableName;

   @TableName("user")
   public class User {
       @TableId
       private Long id;
       private String name;
       private GenderEnum gender;
       
       // getter 和 setter 方法
   }
   ```

### 方式二：使用 `EnumValue` 注解

1. **定义枚举类并使用 `EnumValue` 注解**：
   ```java
   import com.baomidou.mybatisplus.annotation.EnumValue;

   public enum GenderEnum {
       MALE(1, "男"),
       FEMALE(2, "女");

       @EnumValue
       private final int value;
       private final String desc;

       GenderEnum(int value, String desc) {
           this.value = value;
           this.desc = desc;
       }

       public int getValue() {
           return value;
       }

       public String getDesc() {
           return desc;
       }
   }
   ```

2. **在实体类中使用枚举类型**：
   ```java
   import com.baomidou.mybatisplus.annotation.TableId;
   import com.baomidou.mybatisplus.annotation.TableName;

   @TableName("user")
   public class User {
       @TableId
       private Long id;
       private String name;
       private GenderEnum gender;

       // getter 和 setter 方法
   }
   ```

### 配置映射

确保你的 MyBatis 配置正确，并且你的 MyBatis-Plus 版本支持枚举映射。如果需要，添加如下配置：

```yaml
mybatis-plus:
  configuration:
    map-underscore-to-camel-case: true
    default-enum-type-handler: com.baomidou.mybatisplus.core.handlers.EnumTypeHandler
```

### 示例

以下是一个完整的示例，展示了如何将用户性别枚举类型存储在数据库中并通过 MyBatis-Plus 进行操作：

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class UserService {
    @Autowired
    private UserMapper userMapper;

    public void addUser(User user) {
        userMapper.insert(user);
    }

    public List<User> getAllUsers() {
        return userMapper.selectList(null);
    }
}
```

```java
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface UserMapper extends BaseMapper<User> {
}
```

通过上述方式，你可以方便地在 MyBatis-Plus 中处理枚举类并将其映射到数据库字段中。