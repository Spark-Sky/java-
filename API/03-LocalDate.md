# LocalDate
## 导入
```java
import java.time.LocalDate;
```

## 构造(静态方法构造)
### now()
```java
LocalDate.now();
```
- 以当前时间作为自己的时间节点

### of()
```java
public static LocalDate of(int year, int month, int dayOfMonth) 
```
- 参数及其方法说明
```java
LocalDate ld = LocalDate.of(1997,1,1);
System.out.println(ld);//1997-01-01
```

## API
### getYear() getMonthValue() getDayOfMonth()
```java
public int getYear()
public int getMonthValue()
public int getDayOfMonth()
```
- 示例
```java
LocalDate ld = LocalDate.of(1997,1,1);
System.out.println(ld.getYear());//1997
System.out.println(ld.getMonthValue());//1
System.out.println(ld.getDayOfMonth());//1
```

### plusDays()
```java
public LocalDate plusDays(long daysToAdd)
```
- 如果`daysToAdd`=0则返回原对象，否则则返回一个新的对象
```java
LocalDate ld = LocalDate.of(1997,1,1);
LocalDate ld2= ld.plusDays(1000);
System.out.println(ld2);//1999-09-28
```
