# Lombok

Lombok 是一个 Java 库，它通过注解的方式来帮助开发者消除 Java 代码中的样板代码（boilerplate code），提高代码的简洁

```xml
maven依赖
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.34</version>
    <scope>provided</scope>
</dependency>

```

性和可读性。下面列出了 Lombok 中一些常用的注解及其作用：

1. `@Getter` 和 `@Setter`: 自动生成字段的 Getter 和 Setter 方法。
2. `@ToString`: 自动生成 `toString()` 方法。
3. `@EqualsAndHashCode`: 自动生成 `equals()` 和 `hashCode()` 方法。
4. `@NoArgsConstructor`: 自动生成无参构造方法。
5. `@RequiredArgsConstructor`: 自动生成包含必须参数的构造方法。
6. `@AllArgsConstructor`: 自动生成包含所有参数的构造方法。
7. `@Data`: 将 `@ToString`、`@EqualsAndHashCode`、`@Getter`、`@Setter` 和 `@RequiredArgsConstructor` 结合起来的便捷注解。
8. `@Builder`: 用于构建器模式，为类提供复杂的构造方法。
9. `@Value`: 类似于 `@Data`，但生成的类是不可变的（immutable）。
10. `@Slf4j`: 自动生成 `private static final Logger log = LoggerFactory.getLogger(CurrentClassName.class);`。
11. `@NonNull`: 自动在参数上加上空值检查。
12. `@Cleanup`: 自动资源管理，用于自动调用资源的 `close()` 方法。
13. `@SneakyThrows`: 自动生成 `try-catch` 块，隐藏异常。
14. `@Getter(lazy=true)`: 懒加载 Getter 方法。
15. `@Synchronized`: 生成同步方法。
16. `@Value.Style`: 自定义不可变类的风格。
17. `@Value.Immutable`: 创建不可变的 POJO 类。
18. `@Wither`: 生成新的实例，类似于不可变对象的一种更新方式。

这些注解大大简化了 Java 类的开发过程，减少了冗长的样板代码，提高了开发效率。





### 1. 使用@Data注解简化POJO类

```java
import lombok.Data;

@Data
public class Person {
    private String name;
    private int age;
}
```

上面的代码使用 `@Data` 注解，自动为 `Person` 类生成了以下方法：

- 所有字段的 `getters` 方法
- 所有非final字段的 `setters` 方法
- `equals`, `hashCode`, `toString` 方法

### 2. 使用@Getter和@Setter注解

```java
import lombok.Getter;
import lombok.Setter;

@Getter @Setter
public class Car {
    private String make;
    private String model;
    private int year;
}
```

这里的 `@Getter` 和 `@Setter` 注解分别为 `Car` 类生成了 `getters` 和 `setters` 方法。

### 3. 使用@NoArgsConstructor和@AllArgsConstructor注解

```java
import lombok.NoArgsConstructor;
import lombok.AllArgsConstructor;

@NoArgsConstructor
@AllArgsConstructor
public class Book {
    private String title;
    private String author;
    private int pages;
}
```

`@NoArgsConstructor` 自动生成无参构造方法，`@AllArgsConstructor` 自动生成全参构造方法。

### 4. 使用@Builder注解构建对象

```java
import lombok.Builder;
import lombok.Getter;

@Getter @Builder
public class Pizza {
    private String crust;
    private String sauce;
    private String topping;
    private boolean extraCheese;
}
```

`@Builder` 注解为 `Pizza` 类生成了一个建造者模式的构建器，可以通过链式调用来构建对象，例如：

```java
Pizza pizza = Pizza.builder()
                   .crust("Thin")
                   .sauce("Tomato")
                   .topping("Pepperoni")
                   .extraCheese(true)
                   .build();
```

### 5. 使用@ToString注解自动生成toString方法

```java
import lombok.ToString;

@ToString
public class Product {
    private String name;
    private double price;
    private int quantity;
}
```

`@ToString` 注解自动生成了 `Product` 类的 `toString` 方法，输出对象的字段值。







