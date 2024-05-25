# Objects
## 导入
```java
import java.util.Objects;
```

## 构造
### 构造函数名
```java
//方法原型
```
- 参数及其方法说明
```java
//代码实例
```

## API
### requireNonNull()
```java
public static <T> T requireNonNull(T obj)
```
- 参数及方法说明
```java
String name = Objects.requireNonNull("spark");
String name = Objects.requireNonNull(null);//运行时异常
```

### equals()
|a|b|return|
|:-:|:-:|:-:|
|null|null|true|
|null|非null|false|
|非null|null|false|
|非null|非null|a.equals(b)|
```java
public static boolean equals(Object a, Object b) 
{
    return (a == b) || (a != null && a.equals(b));
}
```

### hashCode()
- 比起直接从对象中调用，如果被调用的参数是Null，则直接返回0

```java
double salary;
Objects.hashCode(salary);
```

### hash()
- 组合多个散列值,会依次调用`Objects.hashCode()`，并组合这些散列值
```java
String name;
double salary;
Date hireDay;
Objects.hash(name,salary,hireDay);
```

## isNull()
```java
public static boolean isNull(Object obj) {
    return obj == null;
}
```
- 测试`obj`对象是否为`null`
常用于[方法引用](../06-接口、lambda表达式与内部类.md#方法引用)
