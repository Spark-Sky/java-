# funciton
## 导入
```java
import java.util.function.*;
```

## API

### BiFunction 接口
```java
public interface BiFunction<T, U, R>
{
    R apply(T t, U u);
}
```
- 描述了参数类型为`T` `U`返回类型为`R`的函数式接口

```java
BiFunciton<String,String,Iteger> comp = (fisrt,second)-> fisrt.length()-second.length();
```

### Predicate 接口
```java
public interface Predicate<T> 
{
    boolean test(T t);
}
```
- 断言接口
```java
Predicate<Integer> predicate = (Integer integer)-> integer == 3;

ArrayList<Integer> arrayList =  new ArrayList<>();
arrayList.add(1);
arrayList.add(2);
arrayList.add(3);
arrayList.add(4);
arrayList.removeIf(predicate);
/**
 * 等价于
 * arrayList.removeIf(integer-> integer == 3);
*/
System.out.println(arrayList);
```

### Supplier 接口

```java
public interface Supplier<T> {

    T get();
}
```
- 供应者
- 实现懒计算，应为代码块只有需要执行才会执行
```java
LocalDate day = LocalDate.now();
LocalDate hireDay = Objects.requireNonNullElse(day,()->LocalDate.of(1970,1,1));
/**
 * 而不是
 * new localDate(1970,1,1)
*/
```



