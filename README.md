
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
