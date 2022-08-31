
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

### 类实例化的顺序
#### Java中类实例化顺序：

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
