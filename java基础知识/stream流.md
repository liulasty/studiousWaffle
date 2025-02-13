Java 8 引入了 Stream API，使得处理数据集合变得更加简洁和高效。Stream API 提供了一种函数式编程风格，可以用于对集合进行各种操作，如过滤、映射、排序和汇总等。下面是一些常用的 Stream API 操作示例。

### 示例数据

我们将使用一个简单的 `Person` 类和一个 `List<Person>` 数据集合进行演示：

```java
import java.util.Arrays;
import java.util.List;

public class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return name + " (" + age + ")";
    }

    public static List<Person> getPeople() {
        return Arrays.asList(
            new Person("John", 30),
            new Person("Jane", 25),
            new Person("Jack", 20),
            new Person("Jill", 27)
        );
    }
}
```

### 1. 过滤（Filter）

过滤集合中的元素，保留符合条件的元素：

```java
import java.util.List;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<Person> people = Person.getPeople();

        List<Person> filteredPeople = people.stream()
                                            .filter(person -> person.getAge() > 25)
                                            .collect(Collectors.toList());

        filteredPeople.forEach(System.out::println);
    }
}
```

### 2. 映射（Map）

将集合中的元素转换为另一种形式：

```java
import java.util.List;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<Person> people = Person.getPeople();

        List<String> names = people.stream()
                                   .map(Person::getName)
                                   .collect(Collectors.toList());

        names.forEach(System.out::println);
    }
}
```

### 3. 排序（Sorted）

对集合中的元素进行排序：

```java
import java.util.List;

public class StreamExample {
    public static void main(String[] args) {
        List<Person> people = Person.getPeople();

        List<Person> sortedPeople = people.stream()
                                          .sorted((p1, p2) -> p1.getAge() - p2.getAge())
                                          .collect(Collectors.toList());

        sortedPeople.forEach(System.out::println);
    }
}
```

### 4. 汇总操作（Reduce）

对集合中的元素进行汇总操作，例如求和：

```java
import java.util.List;

public class StreamExample {
    public static void main(String[] args) {
        List<Person> people = Person.getPeople();

        int totalAge = people.stream()
                             .map(Person::getAge)
                             .reduce(0, Integer::sum);

        System.out.println("Total age: " + totalAge);
    }
}
```

### 5. 统计（Statistics）

使用 `Collectors` 提供的统计方法进行统计分析：

```java
import java.util.IntSummaryStatistics;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<Person> people = Person.getPeople();

        IntSummaryStatistics ageStatistics = people.stream()
                                                   .collect(Collectors.summarizingInt(Person::getAge));

        System.out.println("Average age: " + ageStatistics.getAverage());
        System.out.println("Total age: " + ageStatistics.getSum());
        System.out.println("Max age: " + ageStatistics.getMax());
        System.out.println("Min age: " + ageStatistics.getMin());
    }
}
```

### 6. 分组（Grouping）

根据某个属性对集合中的元素进行分组：

```java
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<Person> people = Person.getPeople();

        Map<Integer, List<Person>> peopleByAge = people.stream()
                                                       .collect(Collectors.groupingBy(Person::getAge));

        peopleByAge.forEach((age, persons) -> {
            System.out.println("Age " + age + ": " + persons);
        });
    }
}
```

通过这些示例，可以看到 Stream API 提供了丰富的操作方法，使得对数据集合的处理更加直观和简洁。Stream API 支持链式调用，使代码更加易读，并且充分利用了 Java 8 的函数式编程特性。