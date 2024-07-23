在Java中，对象的哈希值（hash code）是由对象的`hashCode()`方法计算得出的。每个对象都继承了`Object`类中的`hashCode()`方法，但是可以根据需要进行重写。

### 默认的`hashCode()`方法：

如果不重写`hashCode()`方法，Java会使用默认的实现。默认实现中，对象的哈希码是对象的内存地址转换成整数。具体来说，通常会执行以下操作：

- 对象的内存地址转换成一个整数值。
- 这个整数值通常是一个对象在内存中的物理位置标识，因此每个对象的哈希码是唯一的。

默认的`hashCode()`方法由`Object`类提供，其实现如下（简化版）：
```java
public native int hashCode();
```
这里的 `native` 关键字表示这个方法的实现是依赖于具体的底层实现，通常是由JVM提供的。

### 自定义`hashCode()`方法：

通常情况下，当我们需要根据对象的逻辑内容来判断对象是否相等时，需要重写`hashCode()`方法。在重写`hashCode()`方法时，应该遵循以下几个原则：

1. **一致性**：如果两个对象通过`equals()`方法比较返回`true`，那么它们的`hashCode()`方法应返回相同的整数。
   
2. **相等对象的hashCode()值必须相同**：如果两个对象的`equals()`方法返回`true`，它们的`hashCode()`值必须相同，但反过来不一定成立。
   
3. **分布均匀**：尽可能避免不同对象产生相同的hashCode()值。这可以减少哈希冲突，提高哈希表的性能。

示例：如果我们有一个类`Person`，需要根据姓名和年龄来判断两个`Person`对象是否相等，可以重写`hashCode()`方法如下：

```java
public class Person {
    private String name;
    private int age;

    // 构造方法、getter、setter等省略

    @Override
    public int hashCode() {
        int result = 17; // 选择一个非零的常数初始值
        result = 31 * result + name.hashCode();
        result = 31 * result + age;
        return result;
    }

    @Override
    public boolean equals(Object obj) {
        if (obj == this) {
            return true;
        }
        if (!(obj instanceof Person)) {
            return false;
        }
        Person other = (Person) obj;
        return name.equals(other.name) && age == other.age;
    }
}
```

在上面的示例中：
- `hashCode()`方法通过计算 `31 * result + name.hashCode()` 和 `31 * result + age` 来生成哈希码，其中31是一个选择良好的质数，用于帮助生成良好的散列码。
  
- `equals()`方法用来判断两个`Person`对象是否相等，如果`equals()`返回`true`，那么它们的`hashCode()`方法应该返回相同的值。

通过这样的设计，可以确保在使用`Person`对象作为哈希表的键时，能够正确地处理相等性和哈希码的计算。