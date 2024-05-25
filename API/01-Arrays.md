# Arrays
## 导入
```java
import java.util.Arrays;
```

## API (以`int`为例)
### toString
```java
public static String toString(int[] a);
```
- 返回数组的字符串，自动遍历，而不是数组的地址
```java
 int[] a = new int[]{1,2,3,4};
System.out.println(Arrays.toString(a));//[1, 2, 3, 4]
```
### copyOfRange
```java
public static int[] copyOfRange(int[] original, int from, int to);
```
- `original` 被拷贝的数组
- `from` 拷贝的起始
- `to` 拷贝的终止(不包括)
    - 如果超出原数组，则多出来的初始化为0
```java
int[] a = new int[]{1,2,3,4};
int[] b = Arrays.copyOfRange(a,1,6);
System.out.println(Arrays.toString(b));//[2, 3, 4, 0, 0]
```
### sort
```java
public static void sort(int[] a);
```
- 内部使用快速排序
- 原地算法
- 默认为从小到大
- 也可以为自定义类，但是必须实现了`compareTo`接口

```java
int[] a = new int[]{4,3,2,1};
Arrays.sort(a);
System.out.println(Arrays.toString(a));//[1, 2, 3, 4]
```


### fill
```java
public static void fill(int[] a, int val);
```
- 将数组内部全部填充为`val`

```java
int[] a = new int[4];
Arrays.fill(a,1);
System.out.println(Arrays.toString(a));//[1, 1, 1, 1]
```

### deepToString
```java
public static String deepToString(Object[] a) 
```
- 打印二维数组的，应该可以更高维度的数组

```java
int[][] a = {{1,2,3},{4,5,6},{7,8,9,10}};
System.out.println(Arrays.deepToString(a));
```
