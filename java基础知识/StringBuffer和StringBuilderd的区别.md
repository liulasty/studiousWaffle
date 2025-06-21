`StringBuffer` 和 `StringBuilder` 都是 Java 中用于处理可变字符串的类，它们的核心区别在于 **线程安全性** 和 **性能**。以下是详细对比：

---

### 1. **线程安全性**
- **StringBuffer**  
  - **线程安全**，所有方法都用 `synchronized` 修饰（如 `append()`、`insert()` 等），适合多线程环境。  
  - 示例：  
    ```java
    StringBuffer buffer = new StringBuffer();
    buffer.append("thread-safe");
    ```

- **StringBuilder**  
  - **非线程安全**，没有 `synchronized` 修饰，性能更高，但只能在单线程环境下使用。  
  - 示例：  
    ```java
    StringBuilder builder = new StringBuilder();
    builder.append("faster");
    ```

---

### 2. **性能**
- **StringBuilder** 由于无需同步锁，操作速度比 `StringBuffer` **快约 10%~15%**（单线程场景）。  
- **StringBuffer** 的同步机制会带来额外开销，性能较低。

---

### 3. **共同点**
- 均继承自 `AbstractStringBuilder`，底层使用 **可变字符数组（`char[] value`）**。  
- 提供相似的 API（如 `append()`、`insert()`、`reverse()`）。  
- 与不可变的 `String` 不同，它们可以直接修改内容，避免频繁创建新对象。

---

### 4. **使用场景**
- **用 `StringBuilder`**：单线程环境（如方法内局部变量），追求更高性能。  
- **用 `StringBuffer`**：多线程环境（如全局变量），需保证线程安全。  

---

### 5. **代码示例对比**
```java
// StringBuilder（单线程）
StringBuilder sb = new StringBuilder();
sb.append("Hello").append(" World"); // 非同步，高效

// StringBuffer（多线程）
StringBuffer buffer = new StringBuffer();
buffer.append("Hello").append(" World"); // 同步，安全
```

---

### 总结表
| 特性                | `StringBuffer`         | `StringBuilder`        |
|---------------------|------------------------|------------------------|
| **线程安全**         | 是（`synchronized`）   | 否                     |
| **性能**            | 较低（同步开销）       | 更高（无同步）         |
| **适用场景**         | 多线程                 | 单线程                 |

根据需求选择：优先用 `StringBuilder`（多数单线程场景），仅在需要线程安全时选 `StringBuffer`。