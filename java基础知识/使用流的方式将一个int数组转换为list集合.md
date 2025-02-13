在 Java 中，可以使用 Java 8 引入的流（Stream）API 将一个 `int` 数组转换为 `List` 集合。以下是一个示例，演示如何使用流将 `int` 数组转换为 `List<Integer>`：

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class IntArrayToList {
    public static void main(String[] args) {
        int[] intArray = {1, 2, 3, 4, 5};

        // 将 int 数组转换为 List<Integer>
        List<Integer> integerList = Arrays.stream(intArray) // 创建 IntStream
                                          .boxed() // 将每个 int 值转换为 Integer
                                          .collect(Collectors.toList()); // 收集到 List 中

        // 输出结果
        System.out.println(integerList);
    }
}
```

### 详细解释

1. **`Arrays.stream(intArray)`**：
   - 使用 `Arrays.stream` 方法将 `int` 数组转换为 `IntStream`。
   
2. **`.boxed()`**：
   - 将 `IntStream` 转换为 `Stream<Integer>`，这是因为 `IntStream` 是用于基本类型 `int` 的专用流，而 `Stream<Integer>` 是用于包装类型 `Integer` 的流。

3. **`.collect(Collectors.toList())`**：
   - 使用 `Collectors.toList()` 将流中的元素收集到一个 `List` 中。

### 示例输出

运行上述代码，会输出以下结果：

```
[1, 2, 3, 4, 5]
```

这个方法简洁高效，利用了 Java 8 的流 API，适用于将任何 `int` 数组转换为 `List<Integer>`。