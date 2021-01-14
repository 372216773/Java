# Java学习(9)

[toc]

## 1.String类

### 1.定义一个字符串

* 定义一个字符串,可以包含任意字符

##### 1. 基本数据类型的赋值方式

```java
public class Test{
public static void main(String[] args){
    String s="abc";
}
}//s是一个对象,直接赋值,自动包装成对象
```

##### 2. 引用类型的赋值方式

```java
public class Test{
public static void main(String[] args){
    String s=new String("efg");
}
}//不用自动包装成对象,本身的定义就是对象
```

* 以上s都是对象
* String s;//当前对象没有指向任何空间 null
* 调用null对象的方法,一定会出现NullPointException异常

***

* String是由final修饰的最终类,实现三接口

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence
```

***

### 2.String定义的方法

#### (1)charAt()

```java
public class Test{
public static void main(String[] args){
    String s=new String("efg");
    char c=s.charAt(1);
    System.out.println(c);
}
}//out:f
```

* 返回s内部的字符串("abc")中指定位置的字符串,s储存的是"abc",按照索引(位置)对应的字符

0:	e		1:	f		2:	g

#### (2)compareTo()

```java
public class Test{
public static void main(String[] args){
    String s=new String("efg");
    String s1=new String("abc");
    int result=s.compareTo(s1);
    System.out.println(result);
}
}//ASCii(efg-abc)
```

* compareTo()比较两个字符串的大小,且返回值为两者相差的Ascii码值

返回值是负数:当前字符串Ascii码值小于参数字符串Ascii码值

返回值是正数:当前字符串Ascii码值大于参数字符串Ascii码值

返回值是零:当前字符串Ascii码值等于于参数字符串Ascii码值

#### (3)concat()

```java
public class Test{
public static void main(String[] args){
    String s=new String("efg");
    String s1=new String("abc");
    String result=s.concat(s1);
    System.out.println("result:"+result);
    System.out.println("s:"+s);
    //输出拼接的字符串
}
}//out:efgabc
 //    efg
```

* s中储存的是efg,concat(s1)是把当前字符串的内容和参数字符串拼接成以一个新的字符串,原字符串内容不变

#### (4)contains()

```java
public class Test{
public static void main(String[] args){
    String s=new String("efg");
    boolean result=s.contains("g");
    System.out.println(result);
}
}//out:true
```

* 判断当前字符串是否含有指定字符(判断s内存中是收存在参数字符)

#### (5)endsWith()

```java
public class Test{
public static void main(String[] args){
    String s=new String("efg");
    String s1=new String("abc");
    boolean result=s.endsWith("fg");
    System.out.println(result);
}
}//out:true
```

* 判断当前字符串末尾是否和参数匹配

#### (6)startsWith()

```java
public class Test{
public static void main(String[] args){
    String s=new String("efg");
    String s1=new String("abc");
    boolean result=s.startsWith("ef");
    System.out.println(result);
}
}//out:true
```

* 判断当前字符串开始是否与参数匹配

#### (7)equals()

```java
public class Test{
public static void main(String[] args){
    String s=new String("efg");
    String s1=new String("abc");
    System.out.println(s.equals(s1));
    //false
    System.out.println(s==s1);
    //false 比较地址
}
}
```

String类是把equals()方法重写了,重写后:

* 判断两个字符串的内容是否一致(区分大小写)

#### (8)hashCode()

```java
public class Test{
public static void main(String[] args){
    String s=new String("efg");
    String s1=new String("abc");
    System.out.println("s:"+s.hashCode());     System.out.println("s1"+s1.hashCode());
}
}
//out:s:100326
//    s1:96354
```

* String类重写了equals()那么肯定重写了hashCode(),因为当前String的equals()判断内容是否相等
* hashCode()也是返回和内容相关的数据,内容不同,数据也不同

##### equals()和hashcode()的联系:

equals()和hashcode()都是定义在Object类中的两个方法

![image-20201020222025481](D:\wj\Documents\Java\image-20201020222025481.png)

从Object类的equals()方法的源码中可以看出,Object定义的equals()是判断当前对象和参数对象是否指向同一空间(对象的"=="是用来判断当前对象指向的空间地址)

hashcode()主要作用是返回当前对象的唯一标识码

**equals返回true那么hashcode()就应该返回相同的标识**

**调用equals()返回false的两个对象,不要求两个对象返回不同的hashcode值,这是一个标准,Java对象的规范**

程序中是要求遵循当前的规范的,所以在重写equals()时,一定要重写hashcode()

#### (9)equalslanoreCase()

```java
public class Test{
public static void main(String[] args){
    String s=new String("ABC");
    String s1=new String("abc");
    System.out.println(s.equalslanoreCase(s1));
    //true
}
}
```

* 判断两个字符串的内容是否一致(不区分大小写)

#### (10)getBytes()

```java
public class Test{
public static void main(String[] args){
    String s=new String("efg");
    byte[] bs=s.getBytes();
    for(byte b:bs){	//foreach
        System.out.println(b);
    }
}//out:e f g
```

* 返回当前字符串的字节数组
* "abc"-->bs[3]={'a','b','c'};

#### (11)indexOf()

```java
public class Test{
public static void main(String[] args){
    String s=new String("abcabcabc");
   int index=s.indexOf("b");
    System.out.println(index);
}
}//out:1
```

* 返回参数字符串在当前字符串中第一次出现的开始位置
* 从0开始,没有找到返回-1

```java
public class Test{
public static void main(String[] args){
    String s=new String("abcabcabc");
   int index=s.indexOf(97);
    //b的ASCii码:97
    System.out.println(index);
}
}//out:1
```

* 自动把整数转换成字符,开始查找

```java
public class Test{
public static void main(String[] args){
    String s=new String("abcabcabc");
   int index=s.indexOf("b",3);
    System.out.println(index);
}
}//out:4
```

* indexOf()重载,第二个参数"3"是规定查找开始位置

#### (12)lastIndexOf()

```java
public class Test{
public static void main(String[] args){
    String s=new String("abcabcabc");
   int index=s.lastIndexOf("b");
    System.out.println(index);
}
}//out:7
```

* 查找最后一次出现的指定字符串的位置

```java
public class Test{
public static void main(String[] args){
    String s=new String("abcabcabc");
   int index=s.lastIndexOf("b",3);
    System.out.println(index);
}
}//out:1
```

* 从指定位置开始(指定位置为第0个)查找最后一次出现的位置

#### (13)isEmpty()

```java
public class Test{
public static void main(String[] args){
    String s=new String("abcabcabc");
    System.out.println(s.isEmpty());
}
}//out:false
```

#### (14)length()

```java
public class Test{
public static void main(String[] args){
    String s=new String("abcabcabc");
    System.out.println(s.length());
}
}//out:9
```

#### (15)replace()

```java
public class Test{
public static void main(String[] args){
    String s=new String("abc");
    System.out.println(s.replace("a","hello"));
}
}//out:hellobc
```

* 将前字符串"a"用后字符串"hello"替换

#### (16)split()

```java
public class Test {
    public static void main(String[] args) {
        String ss="a-b-c-e-f-g";
        String[] strs=ss.split("-");
        for(String s2 : strs){
            System.out.println(s2);
        }
    }
}//out:a
//     b
//     c
//     d
//     e
//     f
//     g
```

* 按照指定的字符串来分割当前字符串

#### (17)substring()

```java
public class Test {
    public static void main(String[] args) {
        String s="abcdef";
        String ss=s.substring(3);
        System.out.println(ss);
    }
}//out:def
```

* 从参数给定位置开始到字符串末端截取字符串(从零开始)

```java
public class Test {
    public static void main(String[] args) {
        String s="abcdefgh";
        String ss=s.substring(3,6);
        System.out.println(ss);
    }
}//out:def
```

* substring()重载,从前一个参数给定位置开始到后一个参数位置结束截取字符串(从零开始)(3≤ss<6)

#### (18)toCharArray()

```java
public class Test {
    public static void main(String[] args) {
        String s="abcdefg";
        char[] cs=s.toCharString();
        for(char c:cs){
            System.out.println(c);
        }
    }
}//out:a
//     b
//     c
//     d
//     e
//     f
//     g
```

* 把当前字符串转换成一个字符数组

#### (19)toLowerCase(),toUpperCase(),toString()

```java
public class Test {
    public static void main(String[] args) {
        String s="AhIYVUvhbKJIyv";
        String ss=s.toLowerCase();
        String sss=s.toUpperCase();

        System.out.println(ss);
        System.out.println(sss);
    }
}//out:ahiyvuvhbkjiyv
 //    AHIYVUVHBKJIYV
```

*        
String ss=s.toLowerCase();
*        是把当前字符串全部转换成小写
*         String sss=s.toUpperCase();
*        是把当前字符串全部转换成大写
*        toString()
*        把当前的对象转换成字符串,定义在Object类中,返回当前对象的类名+@+当前对象的hashcode值

![image-20201019183539181](D:\wj\Documents\Java\image-20201019183539181.png)

* getClass():返回当前对象的实例
* getName():返回当前对象的类的名称(包名+类名)
* Integer.toHexString():是Integer类的一个静态方法,可以将数值转换成十六进制的数值

### 3.String类静态常量池

```java
public class Student{
    public static void main(String[] args){
        String s="abc";
        String s1="abc";
        System.out.println(s==s1);

    }
}//true
```

* 两个String类型的变量，他们所引用的是同一个string对象（指向同一个内存堆），即返回true。

```java
public class Student{
    public static void main(String[] args){
        String s="abc";
        String s1="abc";
        System.out.println(s.equals(s1));

    }
}//true
```

* String类重写了equals(),比较的是内容

```java
public class Student{
    public static void main(String[] args){
        String s=new String("abc");
        String s1=new String("abc");
        System.out.println(s==s1);
        //false
        System.out.println(s.equals(s1));
        //true
    }
}
```

* String s=new String("abc");

  String s1=new String("abc");

* 分别创建了两个对象,s指向堆区中的一个对象,内部储存的是"abc",s1也指向堆区中的一个对象,内部储存的是"abc",

* 所以,s和s1的地址不同,因而s==s1-->false

* equals()比较的是两个字符串的内容是否相同,因而s.equals(s1)-->true

***

#### String类两种定义方式的区别

#### 1.基本数据类型的赋值方式(String s="abc")

```java
String s="abc";
String s1="abc";
```

String内部维护着一个静态常量池,目的是为了提高String的操作效率,(池:就是一个集合(数组),储存的是静态常量),这个数组是由String类私有的,在String类被加载的时候初始化

如果定义String类的对象,使用的方式是:

* String s="abc";

**直接赋值,没有使用new,那么这种方式的对象都是储存在静态常量池中.**

这种方式不会去临时创建对象,而是先判断静态常量池中是否存在"abc"这个常量字符串

1. 存在:

   s指向常量池中的"abc"的位置

2. 不存在:

   把"abc"储存到静态常量池中,并且s指向静态常量池中的"abc"

**如果没有new对象,直接赋值的字符串,都是储存在静态常量池中的,堆区中是不存在的**

```java
System.out.println(s==s1);
//out:true
```

* String s="abc";

先判断静态常量池中的是否存在"abc",第一次不存在,那么把"abc"存入静态常量池中,s指向静态常量池中的"abc"

* String s1="abc";

先判断静态常量池中是否存在"abc",已经存在了,那么直接让s1指向静态常量池中的"abc"

#### 2.引用类型的赋值方式(String s=new String("abc"))

```java
String s=new String("abc");
String s1=new String("abc");
```

明确的new了两个对象,那么一定是储存在堆区中对象,两个对象内部储存的都是"abc"

```java
String s="abc";
String s1=new String("abc");
```

* String s1=new String("abc");

**首先会在堆区中开辟空间,创建一个对象,储存"abc",s1指向堆区中的空间,然后再判断字符串池中存不存在abc,如果不存在则向静态常量池中储存一份"abc",如果存在则不影响静态常量池**

new的效率低于静态常量池

***

#### 面试题

* new String("abc");

问:创建了几个对象?

答:一个或者两个

一个:如果在创建对象的时候,String类中的静态常量池中已经存在了"abc",那么这行代码只会创建堆区中的对象

两个:如果在创建对象的时候,String类中的静态常量池中不存在了"abc",那么这行代码不仅会创建堆区中的对象,内部储存abc,还会在静态常量池中创建对象abc

***

#### intern()

```java
String s="abc";
String s1=new String("abc");
System.out.println(s==s1);
//false 地址不同
System.out.println(s==s1.intern());
//true
```

s本身指向的是静态常量池中的一个对象,s1本身是指向堆区中的一个对象,s1.intern()返回当前对象在静态常量池中的对象

而s1的内容与s的内容都为"abc",那么s1与s共同指向静态常量池的"abc"