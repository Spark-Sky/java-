# Enum
## 导入
```java
//无需导入，其属于java.lang 默认导入
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
### toString()
```java
public final String name() {
    return name;
}
```
- 返回枚举实例的枚举名字符串
```java
Size a = Size.SMALL;
System.out.println(a.toString());
```

### valueOf()
```java
public static <T extends Enum<T>> T valueOf(Class<T> enumType,String name)
```
- `enumType`为枚举类的实现
- `name`为枚举常量的字符串名称
- `return`返回一个枚举类
- 相当于`toString()`的逆方法
```java
Size a = Enum.valueOf(Size.class,"SMALL");//获取Size.SMALL
System.out.println(a);//SMALL
``` 
### values()

```java
//没找到
```
- 返回所有枚举常量的数组
```java
Size[] a = Size.values();
System.out.println(Arrays.toString(a));//[SMALL, MEDIUM, LARGE, EXTRA_LARGE]
```

### ordinal()
```java
public final int ordinal() {
    return ordinal;
}
```
- 返回枚举常量所在的下标
```java
Size a = Size.SMALL;
System.out.println(a.ordinal());//0
```


