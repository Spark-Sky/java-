# ArrayList
- 底层是连续数组
## 导入
```java
import java.util.ArrayList;
```

## 构造
### 构造函数名
```java
//方法原型
```
- 参数及其方法说明
```java
ArrayList<Integer> arrayList  = new ArrayList<Integer>();
ArrayList<Integer> arrayList  = new ArrayList<>();//后面的类型可以省略，前提是没有使用var声明
ArrayList<Integer> arrayList  = new ArrayList<>(100);//预先申请空间，不影响长度
```

## API
### add
```java
public boolean add(E e)
public void add(int index, E element) 
```
- `index`要被添加的位置，插入到`index`的索引位置，`index`及其之后的元素后移
- `e`要被添加到数组列表中的元素
```java
ArrayList<Integer> arrayList  = new ArrayList<>();
arrayList.add(new Integer(1));
arrayList.add(new Integer(2));
arrayList.add(new Integer(3));
System.out.println(arrayList);[1, 2, 3]
```

### remove
```java
public E remove(int index) 
```
- 删除索引为`index`的元素

```java
ArrayList<Integer> arrayList  = new ArrayList<>(2);
arrayList.add(new Integer(1));
arrayList.add(new Integer(2));
arrayList.add(new Integer(3));
arrayList.remove(2);
System.out.println(arrayList);//[1, 2]
```

### ensureCapacity()
```java
public void ensureCapacity(int minCapacity)
```
- `minCapacity` 预先分配的容量，不会影响长度
```java
arrayList.ensureCapacity(100);
ArrayList<Integer> arrayList  = new ArrayList<>(100);//相同的效果
```

### size()
```java
public int size() {
    return size;
}
```
- 返回数组的长度


### trimToSize()

```java
public void trimToSize() 
```
- 将内存块的大小调整为保存当前元素数量所需要的存储空间
- 应当确认不会再向数组列表中添加任何元素时候才调用`trimTosize()`，否则虽然可以成功扩容添加元素，但是需要移动复制整个数组


### get() set() 访问数组元素
```java
public E get(int index)
public E set(int index, E element)
```
- 返回`index`下标的元素
- 设置`index`下标的元素为`element`并且返回之前的元素
