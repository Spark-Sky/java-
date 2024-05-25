
### Comparator 接口

```java
public interface Comparator<T> {
    int compare(T o1, T o2);
}

```
- 比较器
- 实现比较接口
```java
import java.util.Comparator;

public class LengthComparator implements Comparator<String> {
    @Override
    public int compare(String o1, String o2) {
        return o1.length() - o2.length();
    }
}

//使用代码
LengthComparator cmp = new LengthComparator();
String[] friends = {"Peter","Paul","Mary"};
Arrays.sort(friends,cmp);
System.out.println(Arrays.toString(friends));//[Paul, Mary, Peter]
```
