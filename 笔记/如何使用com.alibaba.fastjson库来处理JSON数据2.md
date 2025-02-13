

```java
import com.alibaba.fastjson.JSON;  
import com.alibaba.fastjson.JSONArray;  
import com.alibaba.fastjson.JSONObject;  
  
import java.util.Arrays;  
import java.util.List;  
  
/**  
 * 测试  
 *  
 * @author lz * @date 2024/12/16 18:24:52 */public class test {  
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
  
        // 7. 将 Java 对象列表转换为 JSON 字符串  
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
  
        public String getName() {  
            return name;  
        }  
  
        public void setName(String name) {  
            this.name = name;  
        }  
  
        public int getAge() {  
            return age;  
        }  
  
        public void setAge(int age) {  
            this.age = age;  
        }  
    }  
}
```

在这个示例中，User类现在有了完整的getter和setter方法，这允许外部代码以更标准的方式访问和修改User对象的属性。

另外，值得注意的是，当使用`JSON.toJSONString()`方法将包含复杂对象（如自定义类实例）的列表转换为JSON字符串时，fastjson通常能够正确地处理这些对象，包括它们的属性和类型信息。然而，在处理更复杂的泛型类型时（例如，List<Map<String, User>>），可能需要使用`TypeReference`来确保泛型信息的正确转换。但在您提供的示例中，直接使用`JSON.toJSONString(userList)`就足够了。