
## StringJoiner
### 什么是StringJoiner？
StringJoiner是 Java 8 新增的一个 API，它基于 StringBuilder 实现，用于实现对字符串之间通过分隔符拼接的场景。  

StringJoiner 有两个构造方法，第一个构造要求依次传入分隔符、前缀和后缀。第二个构造则只要求传入分隔符即可（前缀和后缀默认为空字符串）。

```java
StringJoiner(CharSequence delimiter, CharSequence prefix, CharSequence suffix)
StringJoiner(CharSequence delimiter)
```
有些字符串拼接场景，使用 StringBuffer 或 StringBuilder 则显得比较繁琐。

比如下面的例子：
```java
List<Integer> values = Arrays.asList(1, 3, 5);
StringBuilder sb = new StringBuilder("(");
for (int i = 0; i < values.size(); i++) {  
    sb.append(values.get(i));  
    if (i != values.size() -1) {  
        sb.append(",");  
    }  
}  
sb.append(")");  

```
而通过StringJoiner来实现拼接List的各个元素，代码看起来更加简洁。  

```java
List<Integer> values = Arrays.asList(1, 3, 5);  
StringJoiner sj = new StringJoiner(",", "(", ")");    
for (Integer value : values) {  
	sj.add(value.toString());  
}  
```
```java
Output:
(1,3,5)
```

## 深拷贝和浅拷贝？(Clone and Deep Clone)
### 浅拷贝:拷⻉对象和原始对象的引⽤类型 `引用同⼀个对象`。

以下例子，Cat对象里面有个Person对象，调用clone之后，克隆对象和原对象的Person引用的是同一个对象，这就是浅拷贝。  

```java
public class Cat implements Cloneable {
    private String name;
    private Person owner;

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }

    public static void main(String[] args) throws CloneNotSupportedException {
        Cat c = new Cat();
        Person p = new Person(18, "程序员大彬");
        c.owner = p;

        Cat cloneCat = (Cat) c.clone();
        p.setName("大彬");
        System.out.println(cloneCat.owner.getName());
    }
    //output
    //大彬(随着克隆对象的修改而改变)
}
```

### 深拷贝:拷贝对象和原始对象的引用类型 `引用不同的对象`。  

以下例子，在clone函数中不仅调用了super.clone，而且调用Person对象的clone方法（Person也要实现Cloneable接口并重写clone方法），从而实现了深拷贝。可以看到，拷贝对象的值不会受到原对象的影响。  

```java
public class Cat implements Cloneable {
    private String name;
    private Person owner;

    @Override
    protected Object clone() throws CloneNotSupportedException {
        Cat c = null;
        c = (Cat) super.clone();
        c.owner = (Person) owner.clone();//拷贝Person对象
        return c;
    }

    public static void main(String[] args) throws CloneNotSupportedException {
        Cat c = new Cat();
        Person p = new Person(18, "程序员大彬");
        c.owner = p;

        Cat cloneCat = (Cat) c.clone();
        p.setName("大彬");
        System.out.println(cloneCat.owner.getName());
    }
    //output
    //程序员大彬(不随着克隆对象的修改而改变，真正的独立克隆个体)
}
```

## 类实例化的顺序
### Java中类实例化顺序：

1. 静态属性，静态代码块。  
2. 普通属性，普通代码块。  
3. 构造方法。   

```java
public class LifeCycle {
    // 静态属性
    private static String staticField = getStaticField();

    // 静态代码块
    static {
        System.out.println(staticField);
        System.out.println("静态代码块初始化");
    }

    // 普通属性
    private String field = getField();

    // 普通代码块
    {
        System.out.println(field);
        System.out.println("普通代码块初始化");
    }

    // 构造方法
    public LifeCycle() {
        System.out.println("构造方法初始化");
    }

    // 静态方法
    public static String getStaticField() {
        String statiFiled = "静态属性初始化";
        return statiFiled;
    }

    // 普通方法
    public String getField() {
        String filed = "普通属性初始化";
        return filed;
    }

    public static void main(String[] argc) {
        new LifeCycle();
    }

    /**
     *      静态属性初始化
     *      静态代码块初始化
     *      普通属性初始化
     *      普通代码块初始化
     *      构造方法初始化
     */
}
```

## equals和==有什么区别？  
对于基本数据类型，==比较的是他们的值。基本数据类型没有equal方法；  
对于复合数据类型，==比较的是它们的存放地址(是否是同一个对象)。equals()默认比较地址值，重写的话按照重写逻辑去比较。  
  
  

## final 修饰符
1. 基本数据类型用final修饰，则不能修改，是常量；对象引用用final修饰，则引用只能指向该对象，不能指向别的对象，但是对象本身可以修改。
2. final修饰的方法不能被子类重写
3. final修饰的类不能被继承。

`final 变量`：

final 表示"最后的、最终的"含义，变量一旦赋值后，不能被重新赋值。被 final 修饰的实例变量必须显式指定初始值。

final 修饰符通常和 static 修饰符一起使用来创建类常量。

```java
public class Test{
  final int value = 10;
  // 下面是声明常量的实例
  public static final int BOXWIDTH = 6;
  static final String TITLE = "Manager";
 
  public void changeValue(){
     value = 12; //将输出一个错误
  }
}
```

`final 方法`

父类中的 final 方法可以被子类继承，但是不能被子类重写。

声明 final 方法的主要目的是防止该方法的内容被修改。

如下所示，使用 final 修饰符声明方法。
```java
public class Test{
    public final void changeName(){
       // 方法体
    }
}
```

`final 类`

final 类不能被继承，没有类能够继承 final 类的任何特性。

```java
public final class Test {
   // 类体
}
```

## 方法重载(overload)和重写(override)的区别？
[More details](https://www.runoob.com/java/java-override-overload.html)  

同个类中的多个方法可以有相同的方法名称，但是有`不同的参数列表`，这就称为方法重载。参数列表又叫参数签名，包括参数的类型、参数的个数、参数的顺序，只要有一个不同就叫做参数列表不同。

重载`overload`是面向对象的一个基本特性。

```java
public class OverloadTest {
    void setPerson() { }
    
    void setPerson(String name) {
        //set name
    }
    
    void setPerson(String name, int age) {
        //set name and age
    }
}
```

方法的重写`override`描述的是父类和子类之间的。当父类的功能无法满足子类的需求，可以在子类对方法进行重写。  
方法重写时， `方法名与形参列表必须一致`。

如下代码，Person为父类，Student为子类，在Student中重写了dailyTask方法。

```java
public class Person {
    private String name;
    
    public void dailyTask() {
        System.out.println("work eat sleep");
    }
}


public class Student extends Person {
    @Override
    public void dailyTask() {
        System.out.println("study eat sleep");
    }
}
```

## 接口与抽象类区别？  
[More details](https://blog.csdn.net/guibar/article/details/110092550)  

1、语法层面上的区别  

抽象类可以有方法实现，而接口的方法中只能是抽象方法（Java 8 开始接口方法可以有默认实现）；
抽象类中的成员变量可以是各种类型的，接口中的成员变量只能是public static final类型；
接口中不能含有静态代码块以及静态方法，而抽象类可以有静态代码块和静态方法；
一个类只能继承一个抽象类，而一个类却可以实现多个接口。  

2、设计层面上的区别  

抽象层次不同。抽象类是对整个`类整体`进行抽象，包括属性、行为，但是接口只是对`类行为(方法)`进行抽象。继承抽象类是一种"是不是"的关系，而接口实现则是 "有没有"的关系。如果一个类继承了某个抽象类，则子类必定是抽象类的种类，而接口实现则是具备不具备的关系，比如鸟是否能飞。
继承抽象类的是具有相似特点的类，而实现接口的却可以不同的类。
门和警报的例子：
```java
class AlarmDoor extends Door implements Alarm {
    //code
}

class BMWCar extends Car implements Alarm {
    //code
}
```

```java
// Abstract class
public abstract class Employee
{
   private String name;
   private String address;
   private int number;
   
   public abstract double computePay();
   
   //其余代码
}
```

```java
// Interface
//电源接口
interface Adapter {
    public void input();//充电
}
//USB 接口 连接鼠标
interface USB {
    public void mouse();
}
//网络接口   可以联网
interface Net {
    public void internet();
}
class Computer implements Adapter, USB, Net{
    @Override
    public void internet() {
        // TODO Auto-generated method stub
        System.out.println("联网");
    }

    @Override
    public void mouse() {
        // TODO Auto-generated method stub
        System.out.println("连接鼠标");
    }

    @Override
    public void input() {
        // TODO Auto-generated method stub
        System.out.println("充电！！！");
    }
    
}
public class Demo3 {
    public static void main(String[] args) {
        Computer computer = new Computer();
        computer.mouse();
        computer.input();
        computer.internet();
    }

}
```
