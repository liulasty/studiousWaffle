使用 `com.alibaba.fastjson` 库时，可以利用其提供的多种方法来处理 JSON 数据。以下是一些常用的方法和示例，这些方法可以帮助你解析、生成和处理 JSON 对象和数组。

### 常用方法

1. **解析 JSON 字符串为 JSON 对象**：
   ```java
   String jsonString = "{\"name\":\"John\", \"age\":30}";
   JSONObject jsonObject = JSON.parseObject(jsonString);
   ```

2. **获取 JSON 对象的字段值**：
   ```java
   String name = jsonObject.getString("name"); // 获取字符串类型的字段
   int age = jsonObject.getIntValue("age"); // 获取整型字段
   ```

3. **检查字段是否存在**：
   ```java
   boolean hasName = jsonObject.containsKey("name");
   ```

4. **将 JSON 对象转换为 Java 对象**：
   ```java
   User user = JSON.parseObject(jsonString, User.class); // User 是一个 Java 类
   ```

5. **创建 JSON 对象**：
   ```java
   JSONObject newObject = new JSONObject();
   newObject.put("name", "Alice");
   newObject.put("age", 25);
   ```

6. **将 JSON 对象转换为 JSON 字符串**：
   ```java
   String jsonOutput = JSON.toJSONString(newObject);
   ```

7. **解析 JSON 数组**：
   ```java
   String jsonArrayString = "[{\"name\":\"John\"}, {\"name\":\"Alice\"}]";
   JSONArray jsonArray = JSON.parseArray(jsonArrayString);
   ```

8. **遍历 JSON 数组**：
   ```java
   for (int i = 0; i < jsonArray.size(); i++) {
       JSONObject item = jsonArray.getJSONObject(i);
       String name = item.getString("name");
       System.out.println(name);
   }
   ```

9. **将 Java 对象转换为 JSON 数组**：
   ```java
   List<User> userList = Arrays.asList(new User("John", 30), new User("Alice", 25));
   String jsonArrayOutput = JSON.toJSONString(userList);
   ```

### 示例代码

以下是一个完整的示例，展示了如何使用上述方法：

```java
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONArray;
import com.alibaba.fastjson.JSONObject;

import java.util.Arrays;
import java.util.List;

public class FastJsonExample {
    public static void main(String[] args) {
        // 1. 解析 JSON 字符串
        String jsonString = "{\"name\":\"John\", \"age\":30}";
        JSONObject jsonObject = JSON.parseObject(jsonString);
        
        // 2. 获取字段值
        String name = jsonObject.getString("name");
        int age = jsonObject.getIntValue("age");
        System.out.println("Name: " + name + ", Age: " + age);

        // 3. 创建 JSON 对象
        JSONObject newObject = new JSONObject();
        newObject.put("name", "Alice");
        newObject.put("age", 25);
        
        // 4. 转换为 JSON 字符串
        String jsonOutput = JSON.toJSONString(newObject);
        System.out.println("New JSON: " + jsonOutput);

        // 5. 解析 JSON 数组
        String jsonArrayString = "[{\"name\":\"John\"}, {\"name\":\"Alice\"}]";
        JSONArray jsonArray = JSON.parseArray(jsonArrayString);
        
        // 6. 遍历 JSON 数组
        for (int i = 0; i < jsonArray.size(); i++) {
            JSONObject item = jsonArray.getJSONObject(i);
            System.out.println("Item " + (i+1) + ": " + item.getString("name"));
        }

        // 7. 将 Java 对象转换为 JSON 字符串
        List<User> userList = Arrays.asList(new User("John", 30), new User("Alice", 25));
        String jsonArrayOutput = JSON.toJSONString(userList);
        System.out.println("User List JSON: " + jsonArrayOutput);
    }
    
    public static class User {
        private String name;
        private int age;

        public User(String name, int age) {
            this.name = name;
            this.age = age;
        }
        
        // Getter 和 Setter 方法省略
    }
}
```