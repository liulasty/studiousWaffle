### **HashSet 概述**

`HashSet` 是 Java 集合框架中最常用的类之一，位于 `java.util` 包下。它实现了 `Set` 接口，用于存储不重复的元素，并且不保证存储顺序。

---

### **`HashSet` 的特点**

1. **无序**：
    
    - 元素在集合中的顺序可能与插入顺序不同，因为其底层依赖于哈希表（`HashMap`）。
2. **唯一性**：
    
    - 每个元素必须是唯一的。通过调用对象的 `hashCode()` 和 `equals()` 方法来确保元素的唯一性。
3. **线程不安全**：
    
    - 默认情况下，`HashSet` 不是线程安全的。如果需要线程安全，可以使用 `Collections.synchronizedSet()` 或 `ConcurrentSkipListSet`。
4. **允许存储 `null` 值**：
    
    - `HashSet` 允许一个 `null` 值，但只能存储一个。

---

### **`HashSet` 的底层实现**

- `HashSet` 的底层是基于 `HashMap` 实现的：
    - 每个存入 `HashSet` 的元素实际上是作为 `HashMap` 的键值（`key`）存储，值（`value`）是一个固定的 `Object`（通常是 `PRESENT`，占位符）。

```java
// HashSet 底层使用 HashMap
private transient HashMap<E,Object> map;
private static final Object PRESENT = new Object();
```

- **添加元素的过程**：
    1. 调用元素的 `hashCode()` 方法计算哈希值，确定存储的桶（数组位置）。
    2. 如果桶中没有冲突，直接存储。
    3. 如果发生冲突，采用链表（或红黑树）解决冲突。
    4. 确保元素唯一性：通过调用元素的 `equals()` 方法判断是否重复。

---

### **`HashSet` 的常用方法**

|方法|描述|
|---|---|
|`add(E e)`|将指定元素添加到集合中（如果不存在）。|
|`remove(Object o)`|从集合中移除指定元素（如果存在）。|
|`contains(Object o)`|检查集合中是否存在指定元素。|
|`size()`|返回集合中的元素数量。|
|`isEmpty()`|检查集合是否为空。|
|`clear()`|清空集合中的所有元素。|
|`iterator()`|返回集合元素的迭代器。|

---

### **代码示例**

#### 1. 基本使用

```java
import java.util.HashSet;

public class HashSetExample {
    public static void main(String[] args) {
        HashSet<String> set = new HashSet<>();
        
        set.add("Apple");
        set.add("Banana");
        set.add("Cherry");
        set.add("Apple"); // 重复元素不会添加
        
        System.out.println("Set: " + set);
        System.out.println("Contains 'Apple': " + set.contains("Apple"));
        System.out.println("Size: " + set.size());
        
        set.remove("Banana");
        System.out.println("After removal: " + set);
    }
}
```

**输出**：

```
Set: [Apple, Banana, Cherry]
Contains 'Apple': true
Size: 3
After removal: [Apple, Cherry]
```

#### 2. 存储 `null` 值

```java
import java.util.HashSet;

public class HashSetNullExample {
    public static void main(String[] args) {
        HashSet<String> set = new HashSet<>();
        
        set.add(null);
        set.add("Apple");
        set.add(null); // 再次添加 null，不会重复
        
        System.out.println(set);
    }
}
```

**输出**：

```
[null, Apple]
```

---

### **`HashSet` 的优缺点**

#### **优点**：

1. 插入、删除、查找操作的时间复杂度平均为 O(1)。
2. 保证元素的唯一性。

#### **缺点**：

1. 无法保证元素顺序。
2. 对内存要求较高（由于哈希表需要额外的存储空间）。
3. 插入和查找的性能依赖于 `hashCode()` 和 `equals()` 的实现质量。

---

### **注意事项**

1. **`hashCode()` 和 `equals()` 的重要性**：
    
    - 如果自定义类的对象存入 `HashSet`，必须重写 `hashCode()` 和 `equals()` 方法，否则可能导致无法确保唯一性。
    
    ```java
    import java.util.HashSet;
    import java.util.Objects;
    
    class Person {
        String name;
        int age;
    
        public Person(String name, int age) {
            this.name = name;
            this.age = age;
        }
    
        @Override
        public int hashCode() {
            return Objects.hash(name, age);
        }
    
        @Override
        public boolean equals(Object obj) {
            if (this == obj) return true;
            if (obj == null || getClass() != obj.getClass()) return false;
            Person person = (Person) obj;
            return age == person.age && Objects.equals(name, person.name);
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            HashSet<Person> set = new HashSet<>();
            set.add(new Person("Alice", 25));
            set.add(new Person("Bob", 30));
            set.add(new Person("Alice", 25)); // 不会重复
            System.out.println("Set size: " + set.size());
        }
    }
    ```
    
2. **线程安全**：
    
    - 如果在多线程环境中使用 `HashSet`，需要手动同步：
        
        ```java
        Set<String> syncSet = Collections.synchronizedSet(new HashSet<>());
        ```
        

---

### **`HashSet` 的应用场景**

1. **快速查找唯一元素**：如检查用户名是否重复。
2. **去重**：过滤重复数据。
3. **集合运算**：如求两个集合的交集、并集和差集。

