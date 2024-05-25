# 接口、lambda表达式与内部类
## 接口
- 如果你的类符合某种接口，那么我将会履行这项服务。例如`Arrays.sort()`承诺可以对对象数组进行排序，但是前提是，你的对象满足的`Comparable`接口的代码




```java
//Employee.java
@Override
public int compareTo(Employee o) {
    return Double.compare(this.salary,o.salary);
}
//Test.java
Arrays.sort(employees);
```
- 接口没有实例，但是可以声明接口变量，可以使用`instanceof`检查一个对象是否属于某一个特定类
- 接口中的方法默认为`public`
- 接口中的字段默认为`public static final`
- 接口可以定义常量，但是接口绝对不能有实例字段
- 一个类只能有一个超类，但可以实现多个接口
- 记录和枚举不能扩展其他类，因为其默认继承了`Record`和`Enum`,不过他们可以实现接口
- 接口可以是密封的，其行为同密封类一样


## 静态和私有方法
- java8 允许接口中添加静态方法。但是通常的方法是将静态方法放在伴随类中
- 伴随类：`Colletion` `Collections` 以及`Path` `Paths`
- 但是java11中`Path`接口提供了等价的静态方法，因此`Paths`类就不在是必要的了
- `private`方法只能作为其他方法的辅助方法，用途很有限。


## 默认方法
- 给接口提供默认实现
```java
public interface Comparable<T>
{
    default int compareTo(T other){ return 0};
    //by default,all elements are the same
}
```
- 使用场景
    - 有些接口函数可能不需要实现，也不能实现
    ```java
    public interface Iterator<E>
    {
        boolean hasNext();
        E next();
        //如果实现的迭代器只是只读的，所以remove方法不能被调用
        default void remove
        {
            throw new UnsupportedOperationException("remove");
        }
    }
    ```
    - 源代码兼容，例如某个接口后来又增加了一个需要实现的方法，但是为了旧代码兼容，因此给其添加为默认实现。
- 默认方法冲突
    - 超类（这个实现类继承了超类同时继承了接口）优先。如果超类提供了一个具体方法，同名且具有相同参数类型的默认方法（接口中的默认方法）会被忽略
    - 如果多个接口有同名方法，则必须必须覆盖这个方法来解决冲突。



## 接口与回调
- 定时器任务
```java
//TimerPrinter.java
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.time.Instant;

/**
 * 这个类实现了ActionListener接口
*/
public class TimerPrinter implements ActionListener {

    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("At the tone, the time is"+ Instant.ofEpochMilli(e.getWhen()));
        Toolkit.getDefaultToolkit().beep();

    }
}

//TestManager.java
import javax.swing.*;

public class TestManager {
    public static void main(String[] args)
    {
        TimerPrinter listener = new TimerPrinter();
        Timer timer = new Timer(1000,listener);//传入之前实现的接口实例，其要执行的方法为actionPerformed
        timer.start();
        JOptionPane.showMessageDialog(null,"quit program?");//整个进程卡在了这里，如果点击窗口确认，则会执行下一行代码推出
        System.exit(0);
    }

}
```
- Comparator 接口
- 传递给`Arrays.sort`一个接口实例，让其按照接口的比较函数来进行比较
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

## 对象clone
- `Object`类中`clone`方法声明的是`protected`
- 实现的类需要重新定义为`public`方法，否则外界访问不到
- 使用标记接口`Cloneable`，继承这个接口只是为了让程序员记得要实现`clone`方法（如果没有继承这个接口，那么将会抛出一个异常）
- 注意子类的的`clone`函数
- 数组类型默认实现一个`clone`函数，此时这个是拷贝所有的副本，而不是引用

```java
public class Employee implements Cloneable{
    public String name;
    private double salary;
    private Date hireDay;

    /**
     * java 1.4 之前 clone 方法的返回类型总是Object,而现在可以指定正确的返回类型，这是返回类型协变的一个例子
    */
    @Override
    public Employee clone() throws CloneNotSupportedException {
        Employee cloned  = (Employee)super.clone();
        cloned.hireDay = (Date) hireDay.clone();
        return cloned;
    }
}

//使用
Employee employee = new Employee("spark",1000);
employee.clone()
```


## lambda表达式
- 语法简介
- lambda表达式 是一个可传递的代码块，常用于接口回调传递的接口实例。其目的都是将一段代码块传递给某个类去使用。
- 只包含一个返回值语句的lambda表达式
```java
(String fisrt,String second)-> fisrt.length() - second.length()
```
- 代码块
```java
(String fisrt.String second)->{
    if(fisrt.length() < second.length()) return -1;
    else if(fisrt.length() > second.length()) return 1;
    else return 0;
}
```
- 无参
```java
()->{for(int i=100;i>=0;i--) System.out.println(i);}
```
- 参数类型可以推导
```java
Comparator<String> cmp = (fisrt,second)-> fisrt.length()-second.length();
```
- 一个参数，且可以推导类型
```java
ActionListener listener = event-> System.out.println("At the tone, the time is"+ Instant.ofEpochMilli(e.getWhen()));
```
- 无需指定lambda表达式的返回类型，返回类型总是由上下文推导得出的


### 函数式接口
- 对于只有一个抽象方法的接口，需要这种接口的对象时，就可以提供一个lambda表达式。这种接口称为函数式接口
```java
Arrays.sort(words,(fisrt,second)-> fisrt.length()-second.length());
```
- java.util.funciton 包定义了许多非常通用的函数式接口。
```java
BiFunciton<String,String,Iteger> comp = (fisrt,second)-> fisrt.length()-second.length();
```

## 方法引用
- 有时lambda表达式只涉及一个方法
```java
var timer = new Timer(1000,event -> System.out.println(event));
```
- 可以直接把方法传递给构造器
```java
var timer = new Timer(1000,System.out::println);
/**
 * 它指示编译器生成一个函数式接口的实例，覆盖这个接口的抽象方法来调用给定的方法
 * 要用::分割方法名和对象名或者类名
*/
```
`::`的使用场景
```java
object::instanceMethod
Class::instanceMethod
Class::staticMethod
```
- 第一种情况 等价于一个lambad表达式，其参数要传递到方法.`System.out::println` 对象是`System.out`,所以等价于`x->System::out.println(x)`
- 第二种情况 第一个参数会成为方法的隐式参数 `String::compareToIgnoreCase` 等价于`(x,y)->x.compareToIgnoreCase(y)`
- 第三种情况 所有的参数都传递到静态方法。`Math::pow`等价于`(x,y)->Math.path(x,y)`
- 注意，只有当`lambda`表达式的体只调用一个方法而不做其他的操作时，才可以把lambda表达式重写为方法引用
- 可以在方法引用中时候`this` `super`来指定是哪一个方法



## 构造器引用
- 和方法引用很类似
- `ClassName::new`
```java
ArrayList<String> name = ...;
//Steam 库还没有学到
Steam<Person> stream = names.stream().map(Person::new);
List<Person> people = stream.toList();
```
## 变量作用域
- lambda 表达式访问外围方法或类中变量
```java
public static void  repeatMessage(String text,int delay)
{
    ActionListener listener = event-> {
        System.out.println(text);
        Toolkit.getDefaultToolkit().beep();
    };
    new Timer(delay,listener).start();
}
//repeatMessage("hello",1000);
```
- lambda表达式 
    - 一个代码块的块
    - 参数
    - 自由变量（闭包）的值。指的是非参数而且不在代码中定义的变量
`text`自由变量 需要被lambda表达式所存储
- 具体实现细节，可以吧lambda表达式转换为一个包含一个方法的对象，这样自由变量的值就会复制到这个对象的实例变量
- lambda表达式可以捕获外围作用域中变量的值。在java中，为了确保所捕获的值是明确定义的，这里有个重要的限制。在lambda表达式中，只能引用值不会改变的变量（因为如果并发执行就会出现问题）
- 如果lambda表达式中引用一个变量，而这个变量可能在外部改变，这也是非法的
- lambda表达式捕获的变量必须是**事实最终变量**（初始化后就不会再给赋新值，例如fori循环，这里的i就不可以被捕获）
```java
public static void cutDown(int start,int delay)
{
    ActionListener listener = e -> {
        start--;//非法
        System.out.println(start);
    };
    new Timer(delay,listener).start();
}
```
- lambda表达式的体与嵌套块有相同的作用域，因此不能声明与一个局部变量同名的参数
- `this`指针问题，是指创建这个lambda表达式的方法的`this`参数，而不是lambda表达式所创建实例的`this`
```java
ActionListener listener = event-> {
    //this不是指listener的this
    System.out.println(text);
    Toolkit.getDefaultToolkit().beep();
};
```


## 处理lambda表达式
- 使用lambda表达式的重点是**延迟执行**
    - 在一个单独的线程中运行代码
    - 多次运行代码
    - 在算法的适当位置运行代码（例如，排序中的比较操作）
    - 发生某种情况的时才运行代码（例如，点击了一个按钮，数据已经到达）
    - 只有在必要时才运行代码

```java
public class Test
{
    public static void main(String[] args) throws CloneNotSupportedException {

        repeat(10,()-> System.out.println("hello world!"));

    }
    public static void repeat(int n,Runnable action)
    {
        for(int i=0;i<n;i++) action.run();
    }
}
```

- 接收一个Int值
```java
public class Test
{
    public static void main(String[] args) throws CloneNotSupportedException {

        repeat(10,i-> System.out.println("CutDown: "+(9-i)));

    }
    public static void repeat(int n, IntConsumer action)
    {
        for(int i=0;i<n;i++) action.accept(i);
    }
}
// CutDown: 9
// CutDown: 8
// CutDown: 7
// CutDown: 6
// CutDown: 5
// CutDown: 4
// CutDown: 3
// CutDown: 2
// CutDown: 1
// CutDown: 0
```

- 如果设计自己的接口，并且只有一个抽象方法，则可以用`@FunctionalInterface`注解来标记这个接口
    - 如果你无意增加了一个抽象方法，则会给出错误消息
    - `javadoc`也会指出这是一个函数式接口

## 再谈Comparator
- 其包含许多静态方法来创建比较器
- 静态`comparing`方法接收一个键提取器的函数
```java
Arrays.sort(people,Comparator::comparing(Person::getName));
```
- 可以使用`thenComparing`串起来
```java
Arrays.sort(people,Comparator.comparing(Person::getLastName).thenComparing(Person::getFirstName))
```
```java
Arrays.sort(people,Comparator.comparing(Person::getName,(s,t)->Integer.compare(s.length(),t.length())));
```


## 内部类
- inner class 定义在另一个类中的类
- 使用原因
    - 内部类可以对同一个包中的其他类隐藏
    - 内部类方法可以访问定义这些方法的作用域中的数据，包括原本私有的数据
    - 内部类可以可以简介的实现回调
- 内部类的对象有一个隐式引用，指向实例化这个对象的外部类对象。通过这个对象，可以访问外部对象的全部状态。


```java
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.time.Instant;

public class TalkingClock
{
    private int interval;
    private boolean beep;
    public TalkingClock(int interval,boolean beep)
    {
        this.interval = interval;
        this.beep = beep;
    }

    public void start()
    {

        TimerPrinter listener = new TimerPrinter();//创建实现了ActionListener的内部类
        Timer timer = new Timer(1000,listener);
        timer.start();
    }

    //内部类
    public class TimePrinter implements ActionListener{

        @Override
        public void actionPerformed(ActionEvent e) {
            System.out.println("At the tone, the time is"+ Instant.ofEpochMilli(e.getWhen()));
            //内部类访问外部类对象的字段
            /**
             * 外部类的引用在构造器中设置，编译器会修改所有内部类构造器，添加一个对应外部类引用的参数this
             * 如果TimePrinter是一个普通类，则需要通过getBeep()来获得，但是这是一个内部类可以直接访问
             * 也可以把TimePrinter设置为private，这样就只有TalkingClock可以访问它
            */
            if(beep) Toolkit.getDefaultToolkit().beep();
        }
    }
}
```

- 特殊的语法规则
`OuterClass.this`表示外部类引用，例如
```java
//内部类中访问外部类的字段beep
TalkingClock.this.beep
```
- 使用`OuterClass.new InnerClass(construction parameters)`来明确的编写内部类对象的构造器
```java
//外部类中创建内部类
ActionListener  listener = this.new TimePrinter();
```
- `OuterClass.InnerClass`在外部类外部引用内部类
- 内部类中声明的所有静态字段都必须是`final`,并初始化一个编译器常量。
- 内部类中不能有`static`方法，语言规定，觉得这个没有什么意义，或者坏处多余好处


## 内部类 有用? 必要? 安全?
- 内部类转换为常规的类文件,用`$`来分割外部类名和内部类名，例如`TalkingClock$TimePrinter.class`
- 是否有用
    - 内部类有用更大的访问权限。可以直接访问到外部类的私有字段
## 局部内部类
- 如果一个内部类的名字只出现一次，可以考虑使用局部内部类
- 局部内部类没有访问修饰符，默认为只有本方法内可以访问。
- 局部内部类，可以访问方法中的局部变量。当然局部变量和lambda表达式一样，需要为事实最终变量，一旦赋值就不会改变。
```java
public void start(int interval,boolean beep)
    {

        /**
         * 内部类
        */

        class TimePrinter implements ActionListener
        {

            @Override
            public void actionPerformed(ActionEvent e)
            {
                System.out.println("At the tone, the time is"+ Instant.ofEpochMilli(e.getWhen()));
                //局部内部类访问方法的局部变量
                if(beep) Toolkit.getDefaultToolkit().beep();
            }
        }

        ActionListener  listener = new TimePrinter();//创建局部内部类实例
        Timer timer = new Timer(1000,listener);//传入之前实现的接口实例，其要执行的方法为actionPerformed
        timer.start();
    }
```
- 微妙的问题
    - 过程
    - 调用`start`方法
    - 调用内部类`TimePrinter`的构造器，从而初始化对象变量`listener`
    - 将`listener`引用传递给`Timer`构造器，定时器开始计时，`start`方法退出。此时`beep`参数变量不在存在
    - 1秒之后，`actionPerformed`方法执行`if(beep)`
    - 因此，`TimePrinter`必须要在`beep`消失之前，将其设置为局部变量
## 匿名内部类
- 假如只想创建这个类的实例，甚至不需要名字，可以使用匿名内部类。
```java
public void start()
{
    ActionListener  listener = new ActionListener() {
        @Override
        public void actionPerformed(ActionEvent e) {
            System.out.println("At the tone, the time is"+ Instant.ofEpochMilli(e.getWhen()));
            //内部类访问外部类对象的字段
            if(beep) Toolkit.getDefaultToolkit().beep();

        }
    };
    Timer timer = new Timer(1000,listener);//传入之前实现的接口实例，其要执行的方法为actionPerformed
    timer.start();
}
```

- 一般性语法
```java
//继承于SuperType
new SuperType(construction parameters){
    //inner class methods and data;
};
//实现接口
new Interface(){
    //methods and data
}
```
- 细致的区别
```java
var queen = new Person("Mary");// a Person object
var count = new Person("Dracula"){....}//an object of an inner class extending Person
```

- 对象初始化块
    - 匿名类没有构造器，但是可以使用初始块来模仿构造器的功能
```java
var count = new Person("Dracula")
{
    {
        //initalization code
    }
};
```

- 一些用法
- var引用变量
```java
/**
 * 用var来讲匿名内部类存储在一个变量中，因为这个匿名内部类为一个不可指示的类型
*/
var bob = new Object(){String name = "Bob";}
System.out.println(bob.name);
```
- 双括号初始化
```java
var friends = new ArrayList<String>();
friends.add("Harry");
friends.add("Tony");
invite(friends);
/**
 * 如果不在需要这个数组列表，最好让它作为一个匿名列表，添加元素使用双括号初始化
 * 这个技巧很少用
 * 等同于
 * invite(List.of("Harry","Tony"))
*/
invite(new ArrayList<String>(){{
    add("Harry");
    add("Tony");
}});
```
- 静态方法通过匿名内获取类名(`this.getClass()`，方法获取类名一般调用`this`,但是静态方法没有`this`)
```java
public class TimerTest {
    public static void main(String[] args) {
        System.out.println(new Object(){}.getClass().getEnclosingClass());//class TimerTest
    }
}
```

## 静态内部类
- 有时内部类只是为了把一个类隐藏在另外一个类的内部，并不需要内部类有外部类对象的引用，可以讲内部类声明为`static`，就像静态方法没有`this`指针一样
- 使用场景
    - `Pair`类在很多大型项目中都会有，但是希望有一个自己的，这样可能会出现名字冲突。可以使用公共内部类，这样通过引用`OutClass.Pair`来引用，同时希望其不带有`OutClass`的指针，因此将其命为`static`
- 与常规内部类不同，静态内部类可以有静态字段和方法
- 在接口中声明是内部类自动是`static`和`public`
- 类中声明的接口，记录，枚举都自动为`static`

```java
public class StaticInnerClassTest {
    public static void main(String[] args) {
        //创建一个20长度的double数组
        double[] values = new double[20];
        for(int i=0;i<values.length;i++)
        {
            values[i] = 100*Math.random();
        }

        //使用ArrayAlg.Pair去引用
        ArrayAlg.Pair  p = ArrayAlg.minmax(values);
        System.out.println(p.getFirst());
        System.out.println(p.getSecond());
    }
}


class ArrayAlg
{
    //静态内部类
    public static class Pair
    {
        private double first;
        private double second;

        public Pair(double f,double s)
        {
            first = f;
            second = s;
        }

        public double getFirst() {
            return first;
        }

        public double getSecond() {
            return second;
        }
    }

    //具体的算法
    public static Pair minmax(double[] values)
    {
        double min = Double.POSITIVE_INFINITY;
        double max = Double.NEGATIVE_INFINITY;
        for(double v:values)
        {
            if(min > v) min = v;
            if(min < v) max = v;
        }
        return new Pair(min,max);
    }
}
```

## 服务加载器
- 失败

## 代理
- 利用代理可以在运行时创建实现了一组给定接口的新类。只有在编译时无法确定需要实现哪个接口时才有必要使用代理。（高级技术）
- 使用场景
    - 假设你想构造一个类的对象，这个类实现了一个或多个接口，但是在编译时你可能并不知道这些接口是什么。要想构造一个具体的类，只需要在使用`newInstance`方法或者反射找出构造器。但是，不能实例化接口。需要在运行的程序中定义一个新类
- 代理类可以在运行时创建全新的类，这样一个代理类能够实现你指定的接口。因此代理类包含以下方法
    - 指定接口所需要的全部方法
    - `Object`类中定义的全部方法`toString` `enquals`
- 创建代理对象
    - 需要使用`Proxy`类的`newProxyInstance`方法
    - 参数
        - 一个类加载器
        - 一个`Class`对象数组。每个元素对应需要实习的各个接口
        - 一个调用处理器
- 使用代理的目的
    - 将方法调用路由到远程服务器
    - 将用户界面事件与正在运行的程序中的动作关联起来
    - 为了调试而跟踪方法调用
- 以调试跟踪为例子
```java
//TraceHandler.java
package proxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

public class TraceHandler implements InvocationHandler {

    private Object target;//被代理的对象

    public TraceHandler(Object t)
    {
        target = t;
    }

    //相当于在直接调用对象方法之前拦截了一些信息
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable
    {
        System.out.print(target);//打印这个对象
        System.out.print("."+method.getName()+"(");//获取方法名
        if(args !=null)//代理方法传入的参数
        {
            for (int i = 0; i < args.length; i++) {
                System.out.print(args[i]);
                if(i<args.length-1) System.out.print(", ");
            }
        }
        System.out.println(")");
        return method.invoke(target,args);//调用target.method(args)
    }
}

//ProxyTest.java
package proxy;

import java.lang.reflect.Proxy;
import java.util.Arrays;

public class ProxyTest {

    public static void main(String[] args) {
        Object[] elements = new Object[100];//查找数组
        for (int i = 0; i < elements.length; i++) {
            Integer value = i+1;//从1-100
            TraceHandler handler = new TraceHandler(value);//获取handler对象
            Object proxy = Proxy.newProxyInstance(ClassLoader.getSystemClassLoader(),new Class[]{Comparable.class},handler);//获取代理对象
            elements[i] = proxy;//将代理对象放到数组中

        }

        Integer key  = (int)(Math.random()*elements.length)+1;//要查找的值
        int result = Arrays.binarySearch(elements,key);//执行二分查找
        if(result >0 ) System.out.println(elements[result]);//如果找到，打印这个值
    }
}

// 50.compareTo(4)
// 25.compareTo(4)
// 12.compareTo(4)
// 6.compareTo(4)
// 3.compareTo(4)
// 4.compareTo(4)
// 4.toString()
// 4

```

- 一些特性
    - 代理类是在程序运行过程中动态创建的。不过，一旦创建，他们就是常规的类
    - 所有的代理都扩展`Proxy`类，一个代理类只有一个实例字段，即调用处理器，完成代理对象所需要的任何**额外**数据的都存储在调用处理器中。
    - 所有的代理类都要覆盖`toString` `equals` `hashCode`方法
