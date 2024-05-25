# Class
- `Class`对象是类，其可以从`Employee`,`Random`这样的类获取，也可以从`int`这样的基本变量获取
- `Class`实际是一个泛型类，`Employee.Class`实际上为`Class<Employee>`
## 导入
```java
//导入语句
```

## 构造
### getClass()
```java
Employee e;
CLass cl = e.getClass();
```
- 获取描述`Employee`类的`Class`实例
```java
Employee e;
CLass cl = e.getClass();
```

### forName()
```java
public static Class<?> forName(String className)
```
- `className`类的名字
- 根据类名（有的需要包名前缀）获取一个类的`Class`对象，会抛出一个异常
```java
String className = "java.util.Random";
Class cl =  Class.forName(className);
System.out.println(cl);//class java.util.Random
```

### class
```java
Class cl = String.class;
```
- 直接根据类名获取


## API
### getName()
```java
public String getName() 
```
- 获取类的名字
```java
Class cl = Size.SMALL.getClass();
System.out.println(cl.getName());//Size
```

###  getConstructor()
```java
public Constructor<T> getConstructor(Class<?>... parameterTypes)
```
- 根据`Class`实例获取其的构造方法
- 如果其没有无参构造函数，则会抛出异常
```java
Class cl = Class.forName("java.lang.String");
Object object = cl.getConstructor().newInstance();
```

### getFields()
```java
public Field[] getFields()
```
- 获取成员变量
- 只能获得这个类或者超类的公共字段
```java
Class cl = Employee.class;
Field[] fields = cl.getFields();
System.out.println(Arrays.toString(fields));//[public java.lang.String Employee.name]
```

### getDeclaredFields()

```java
public Field[] getDeclaredFields() 
```
- 获取成员变量
- 全部字段包括私有的
```java
Class cl = Employee.class;
Field[] fields = cl.getDeclaredFields();
System.out.println(Arrays.toString(fields));
//[public java.lang.String Employee.name, private double Employee.salary, private java.util.Date Employee.hireDay]
```


### getMethods()

```java
public Method[] getMethods() 
```
- 获取子类和超类的所有公共方法
```java
Class cl = Employee.class;
Method[] methods = cl.getMethods();
System.out.println(Arrays.toString(methods));
```

### getDeclaredMethods()

```java
public Method[] getDeclaredMethods()
```
- 获取类所有方法
```java
Class cl = Employee.class;
Method[] methods = cl.getDeclaredMethods();
System.out.println(Arrays.toString(methods));
//[public java.lang.String Employee.toString(), private java.lang.String Employee.getName()]
```


### getConstructors()
```java
public Constructor<?>[] getConstructors()
```
- 获取类的公开构造方法
```java
Class cl = Employee.class;
Constructor[] constructors = cl.getConstructors();
System.out.println(Arrays.toString(constructors));
//[public Employee(java.lang.String,double,java.util.Date)]
```


### getDeclaredConstructors()
```java
public Constructor<?>[] getDeclaredConstructors()
```
- 获取类的所有构造方法
```java
Class cl = Employee.class;
Constructor[] constructors = cl.getDeclaredConstructors();
System.out.println(Arrays.toString(constructors));
//[public Employee(java.lang.String,double,java.util.Date), private Employee()]
```

### isInterface()
```java
public native boolean isInterface();
```
- 判断该类是否为接口
```java
Class cl = Employee.class;
System.out.println(cl.isInterface());
//false
```

### isEnum()


### isRecord()

### getPackageName()
- 获取包名，例如数组则返回 `java.lang`



### getComponentType()

```java
public native Class<?> getComponentType();
```
- 获取数组`Class`对象的中元素的类型
```java
Class cl = (new Employee[1]).getClass();
System.out.println(cl.getComponentType());//class Employee
```




- 还有其他的我就不写了，用到再写
