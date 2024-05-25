# swing
## 导入
```java
import javax.swing.*;
```

## API
### JOptionPane 类
#### showMessageDialog()
```java
public static void showMessageDialog(Component parentComponent,Object message) 
```
- 显示一个对话框，包含一个提示消息和一个ok按钮
- `parent`
    - 位于`parent`组件的中间，如果为`null`则显示在屏幕的中央
- `message`
    - `String` 显示的信息
```java
JOptionPane.showMessageDialog(null,"quit program?");
System.exit(0);
```


### Timer 类
#### Timer()
```java
public Timer(int delay, ActionListener listener)
```
- 构造一个定时器，每经过`interval`毫秒通知`listener`一次
```java
TimerPrinter listener = new TimerPrinter();
Timer timer = new Timer(1000,listener);
```
#### start()
```java
void start()
```
- 启动定时器，一旦启动，定时器将调用监听器的actionPerformed
