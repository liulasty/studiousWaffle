
HashMap 和 Hashtable 都是 Java 中用于存储键值对的集合类，但它们有几个重要区别：

## 主要区别

| 特性            | HashMap                     | Hashtable                  |
|----------------|----------------------------|---------------------------|
| **同步性**       | 非同步 (线程不安全)            | 同步 (线程安全)              |
| **null 值**     | 允许键和值为 null            | 不允许键或值为 null         |
| **性能**        | 更高 (因为非同步)              | 较低 (因为同步开销)           |
| **继承关系**     | 继承自 AbstractMap          | 继承自 Dictionary (已过时)   |
| **迭代器**      | 使用快速失败的 Iterator      | 使用 Enumeration           |
| **引入版本**     | Java 1.2                   | Java 1.0                  |

## 详细说明

1. **线程安全性**
   - Hashtable 是同步的，所有方法都用 synchronized 修饰
   - HashMap 是非同步的，需要外部同步才能用于多线程环境

2. **null 处理**
   - HashMap 允许一个 null 键和多个 null 值
   - Hashtable 不允许 null 键或值，会抛出 NullPointerException

3. **性能**
   - HashMap 通常比 Hashtable 快，因为没有同步开销
   - 在需要线程安全时，通常推荐使用 ConcurrentHashMap 而非 Hashtable

4. **迭代方式**
   - HashMap 的迭代器是快速失败的 (fail-fast)
   - Hashtable 使用较老的 Enumeration 接口

## 使用建议

- 单线程环境下使用 HashMap
- 多线程环境下考虑使用 ConcurrentHashMap 而非 Hashtable
- 需要保持与旧代码兼容时才使用 Hashtable

## 示例代码

```java
// HashMap 示例
HashMap<String, Integer> hashMap = new HashMap<>();
hashMap.put("key1", 1);
hashMap.put(null, 2);  // 允许
hashMap.put("key3", null);  // 允许

// Hashtable 示例
Hashtable<String, Integer> hashtable = new Hashtable<>();
hashtable.put("key1", 1);
// hashtable.put(null, 2);  // 抛出 NullPointerException
// hashtable.put("key3", null);  // 抛出 NullPointerException
```

在现代 Java 开发中，Hashtable 已经很少使用，通常被 ConcurrentHashMap 替代。