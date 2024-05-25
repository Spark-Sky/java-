# 
## 导入
```java
//java.lang;
```

### BiFunction 接口
```java
public interface Runnable {
    public abstract void run();
}

```
- 描述了一个`run()`函数的运行接口

```java
public class Test
{
    public static void main(String[] args) throws CloneNotSupportedException {

        repeat(10,()-> System.out.println("hello world!"));//打印10次hello world

    }
    public static void repeat(int n,Runnable action)
    {
        for(int i=0;i<n;i++) action.run();
    }
}
```
