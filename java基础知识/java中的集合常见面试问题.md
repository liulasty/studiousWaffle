

---

### 1. **Java 集合框架的层次结构**

- **问题**：Java 集合框架的核心接口有哪些？它们的作用是什么？
- **回答**：
    - **核心接口**：
        - **Collection**：是所有集合接口的父接口，主要有 `List`、`Set`、`Queue` 三个子接口。
        - **Map**：存储键值对，`Map<K, V>` 是独立的接口，不是 `Collection` 的子接口。
    - **主要实现类**：
        - **List**：有序集合，允许重复元素（如 `ArrayList`、`LinkedList`）。
        - **Set**：无序集合，不允许重复元素（如 `HashSet`、`TreeSet`）。
        - **Queue**：先进先出的队列（如 `PriorityQueue`）。
        - **Map**：键值对存储（如 `HashMap`、`TreeMap`）。

---

### 2. **ArrayList 和 LinkedList 的区别**

- **问题**：`ArrayList` 和 `LinkedList` 的区别是什么？
- **回答**：
    - **底层实现**：
        - `ArrayList`：基于动态数组。
        - `LinkedList`：基于双向链表。
    - **访问速度**：
        - `ArrayList`：随机访问快，查询时间复杂度是 O(1)。
        - `LinkedList`：查询需要遍历链表，时间复杂度是 O(n)。
    - **插入和删除**：
        - `ArrayList`：在中间插入或删除元素需要移动数组，时间复杂度是 O(n)。
        - `LinkedList`：插入或删除只需调整链表指针，时间复杂度是 O(1)。
    - **内存消耗**：
        - `ArrayList`：只存储数据。
        - `LinkedList`：额外存储前后指针，内存开销更大。

---

### 3. **HashMap 的底层原理**

- **问题**：`HashMap` 的底层结构是什么？
- **回答**：
    - **底层结构**：数组 + 链表 + 红黑树。
    - **工作原理**：
        - 使用键的 `hashCode` 方法计算哈希值，然后用 `(n - 1) & hash` 计算数组索引。
        - 如果索引位置冲突，采用链表或红黑树存储冲突元素。
        - 当链表长度超过阈值（默认 8），会转为红黑树存储。
        - `HashMap` 默认容量是 16，负载因子是 0.75，达到阈值时扩容。

---

### 4. **HashMap 和 Hashtable 的区别**

- **问题**：`HashMap` 和 `Hashtable` 有什么区别？
- **回答**：
    - **线程安全**：
        - `HashMap`：非线程安全。
        - `Hashtable`：线程安全（通过 `synchronized` 实现）。
    - **性能**：
        - `HashMap` 性能更高，因其没有锁开销。
    - **null 值支持**：
        - `HashMap`：允许一个 `null` 键和多个 `null` 值。
        - `Hashtable`：不允许 `null` 键或值。
    - **迭代器**：
        - `HashMap` 使用 `Iterator`。
        - `Hashtable` 使用 `Enumeration`（较为过时）。

---

### 5. **ConcurrentHashMap 的特点**

- **问题**：`ConcurrentHashMap` 如何实现线程安全？
- **回答**：
    - `ConcurrentHashMap` 在 JDK 1.8 之后的底层实现是 **CAS + 锁分段机制**。
    - 使用了分段锁，每个分段操作独立的部分数据，避免全表锁，从而提升并发性能。
    - 使用 `CAS` 机制来确保原子性更新操作。

---

### 6. **TreeMap 和 HashMap 的区别**

- **问题**：`TreeMap` 和 `HashMap` 有什么区别？
- **回答**：
    - **排序**：
        - `HashMap`：无序存储。
        - `TreeMap`：基于红黑树实现，按照键的自然顺序或比较器排序。
    - **性能**：
        - `HashMap`：查询性能更高（O(1)）。
        - `TreeMap`：查询性能较低（O(log n)）。
    - **用例**：
        - `HashMap`：适合快速查找。
        - `TreeMap`：适合需要有序存储的场景。

---

### 7. **ArrayList 和 Vector 的区别**

- **问题**：`ArrayList` 和 `Vector` 有什么区别？
- **回答**：
    - **线程安全**：
        - `ArrayList`：非线程安全。
        - `Vector`：线程安全（方法通过 `synchronized` 修饰）。
    - **性能**：
        - `ArrayList` 性能较高。
        - `Vector` 性能较低（因其锁机制）。
    - **扩容机制**：
        - `ArrayList`：扩容为当前大小的 1.5 倍。
        - `Vector`：扩容为当前大小的 2 倍。

---

### 8. **fail-fast 和 fail-safe**

- **问题**：什么是 fail-fast 和 fail-safe？示例说明。
- **回答**：
    - **fail-fast**：
        - 直接操作原集合的迭代器，在遍历时如果检测到集合被修改，则抛出 `ConcurrentModificationException`。
        - 示例：`ArrayList` 的 `Iterator`。
    - **fail-safe**：
        - 遍历时使用集合的副本，不会抛异常。
        - 示例：`CopyOnWriteArrayList` 和 `ConcurrentHashMap`。

---

### 9. **HashSet 的底层实现**

- **问题**：`HashSet` 是如何工作的？
- **回答**：
    - **底层实现**：`HashSet` 基于 `HashMap` 实现，所有元素作为 `HashMap` 的键，值为固定的 `Object`（`PRESENT`）。
    - **特点**：元素不允许重复，原因是 `HashMap` 的键不允许重复。

---

### 10. **LinkedHashMap 的工作原理**

- **问题**：`LinkedHashMap` 的特点是什么？
- **回答**：
    - **底层实现**：基于 `HashMap`，同时维护一个双向链表，用于记录插入顺序或访问顺序。
    - **用途**：可用于实现 LRU 缓存，通过 `removeEldestEntry` 方法自定义移除策略。

---

### 11. **List 的遍历方式及性能**

- **问题**：`ArrayList` 有哪些遍历方式？性能如何？
- **回答**：
    - **遍历方式**：
        1. 使用 `for` 循环。
        2. 使用增强的 `for-each` 循环。
        3. 使用迭代器（`Iterator`）。
        4. 使用 `Stream API`。
    - **性能**：
        - `for` 循环：性能较高，适合随机访问。
        - `Iterator`：在操作过程中可以安全地删除元素。
        - `Stream API`：适合对集合进行并行操作或复杂处理。

---

### 12. **如何实现线程安全的集合？**

- **问题**：如何保证集合的线程安全？
- **回答**：
    1. 使用 `Collections.synchronizedList` 或 `Collections.synchronizedMap`。
    2. 使用线程安全的类：
        - `CopyOnWriteArrayList`（线程安全的 List 实现）。
        - `ConcurrentHashMap`（线程安全的 Map 实现）。

---

### 13. **HashMap 扩容过程**

- **问题**：`HashMap` 是如何扩容的？
- **回答**：
    - 当插入的元素数量超过 `容量 x 负载因子` 时，触发扩容。
    - 新数组的大小是原来的两倍。
    - 所有元素重新计算哈希值并重新分配到新数组中。

---

### 14. **优先队列（PriorityQueue）的特点**

- **问题**：`PriorityQueue` 是如何实现的？
- **回答**：
    - `PriorityQueue` 基于最小堆或最大堆实现。
    - 队列中的元素会根据优先级自动排序。
    - 默认是最小堆，可通过自定义比较器实现最大堆。

---
