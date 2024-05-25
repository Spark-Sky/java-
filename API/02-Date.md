# Date
## 导入
```java
import java.util.Date;
```

## 构造
### Date()
```java
public Date() {this(System.currentTimeMillis());}
```
- 使用当前系统时间来构造
```java
new Date();
```
## API
### toString()
```java
public String toString()
```
- 返回时间的字符串形式
```java
System.out.println(new Date());//Thu May 23 14:22:55 CST 2024
```
