# Comparable
## 导入
```java
//java.lang
```

## 需要实现方法
### compareTo
```java
public int compareTo(T o);
```

### Double
```java
@Override
public int compareTo(Employee o) {
    return Double.compare(this.salary,o.salary);
}
```
