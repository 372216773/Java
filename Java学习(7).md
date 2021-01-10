# Java学习(7)

[toc]

### Object类

```java
public class Student{
    //空类
    //系统会自动添加一个无参构造器
}
```

```java
public class Test{
    public static void main(String[] args){
        Student s=new Student();
        String str=s.toString();
        //通过s这个对象调用了一个名为toString()的方法
        //Student类中并没有定义任何方法,也没有继承任何类
        //按理来说时会报错的,但是编译能通过,也能运行
        //从实际效果来看,Student类中的确是存在toString()方法的
        //toString()方法到底是从何而来
        System.out.println(str);
        //JDK源码:Java提供的所有基础类的源代码
        //System,String,Scanner,这些都是Java提供的基础类
        
        int num=s.hashCode();
        System.out.println(num);
        
        String clStr=getClass().getName();
        //toString(),hashCode(),getClass(),getName()都是Object类的方法
        //但是目前从代码角度来看,Student和Object是两个独立的类,没有任何关系
    }
}
```

* Student类的对象拥有Object类定义的方法,但是Student又没有显式的继承Object类,Java中如果一个类没有显式(就是定义类的时候没有用extends关键字修饰当前类的继承关系)继承,那么Java在编译的时候就会自动让dangqianleijichengObject类:**注意:一定是当前类没有继承任何类**(就像Java在编译时会自动给没有定义任何构造方法的类添加一个无参数构造器).

```java
public class Manager extends Student{
    
}
```

* 定义了一个继承了Manager类,显式地继承了Student类,那么就不会继承Object类,**注意:Java是单继承**,Manager类继承Student类,但是Student又继承(隐式)了Object类,按照继承关系,Manager类还是拥有Object类定义的方法,Manager类也是Object类的子类,**一个类的父类永远都是Object类,Object类的方法在Java中的每个类中都有**

### Object类定义的方法

#### (1)toString()

toString() 返回值类型是String类型,返回当前对象的类的全名+@+当前对象的hashCode值

toString():把一个对象转换为字符串

![image-20200925093246428](D:\wj\Documents\Java\image-20200925093246428.png)

* getClass():返回当前对象的类的实例
* getName():返回当前对象的类的名称,即包名+类名
* Integer.toHexString() 是Integer类的一个静态方法，可以把一个数值转换成十六进制的数值

```java
public class Test{
    public static void main(String[] args){
        Student s=new Student();
        String str=s.toString();
        System.out.println(str);
    }
}//out:Student@15db9752  **数值可能不一样**
```

* 当然可以重写toString()方法

```java
public class Student{
    public String toString(){
        return "这是Student类的toString方法"
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        Student s=new Student();
        String str=s.toString();
        System.out.println(str);//
        System.out.println(s.toString());//结果都一样
    }
}//out:这是Student类的toString方法
//     这是Student类的toString方法
```

#### (2)hashCode()

返回值是int类型的数据,返回当前对象的唯一标识符,每当new一个对象,就会开辟一块空间,hashCode()可以生成每片空间唯一的标识

```java
public class Test{
    public static void main(String[] args){
        Student s=new Student();
        Student s1=new Student();
        //new了两个对象,也就是拥有两片空间
        System.out.println(s.hashCode()==s1.hashCode());
    }//     ==  永远比较的是两个对象的地址
 }//out:false
```

* 因为s和s1分别指向两片不同的空间

```java
public class Test{
    public static void main(String[] args){
        Student s=new Student();
        Student s1=s;
        System.out.println(s.hashCode()==s1.hashCode());
    }
}//out:true
```

* s和s1都指向一片相同空间,**hashCode()返回的是new的这片空间的唯一标识**

```java
public class Person{
    public int idCardNumber;
    public String personname;
    public String personSex;
    public int personAge;
}
```

```java
public class Test{
    public Static void main(String[] args){
        Person p=new Person();
        p.idCardName="tom";
        p.idCardNumber="10101";
        p.personSex="男";
        p.personAge=22;
        
        Person p1=new Person();
        p1.idCardName="tom";
        p1.idCardNumber="10101";
        p1.personSex="男";
        p1.personAge=22;
        
        System.out.println(p==p1);
    }
}//false  new了两个不同的对象
```

* 从空间角度来看,p和p1一定是不同的
* 但是从实际出发看,这两个的对象的所有值都是相同的,应该就是同一人

* 要从实际来判断就需要一个判断标准
* Object类中还定义了一个equals()的方法

#### (3)equals()

* equals()也是定义在Object类中的一个方法,这个方法返回值为Boolean值,如果两个对象相等,返回true,不相等返回false

```java
public boolean equals(Object obj){
    return (this==obj);
}
```

* 以上为equals()方法是在Object类中的源码
* 从源码角度来看,equals(方法的判断两个对象是否相等标准还是"==")
* 当equals方法被重写时，通常有必要重写hashCode方法，以维护hashCode方法的常规约定：值相同的对象必须有相同的hashCode。

* 如果两 个对象的 equals 为 true，那么 hashCode 一定是一致，如果两个对象的 equals 返回 false，那么这两对象的 hashCode 值有可能一样，也有可能不一样
* 对于形参的输入还有一个很重要的知识点，只是比较对象是否相等，至于对象是什么类型是不做限制的，因此用object来表示其类型，这就是多态的体现，对象本身肯定不是object类型

***

Java文件--->编译	字节码文件(编译阶段)

class文件--->运行	(运行阶段)

A计算机上开发Java程序 编译生成的class文件在B计算机上也可以运行

Java运行依靠一个Java虚拟技术(JVM(Java虚拟机))Java在Java虚拟机上运行,但是Java虚拟机又作为当前计算机的一个进程;当Java程序运行的时候就先创建虚拟机进程,在虚拟机上执行程序,当前计算机是不会直接执行Java程序的;Java程序是工作在虚拟机内部的;因为Java编译后的指令成为字节码;字节码是一种与操作系统无关的指令

JVM:

1. ClassLoader:类加载器,负责把class文件从磁盘加载到JVM内部,编译完的class文件是一个物理文件,储存在磁盘中,当Java程序运行的时候,JVM启动,先利用类加载器,把相关类加载到JVM中,类加载器加载类文件是按照执行顺序加载的,并不是一次性加载完

   ClassLoader分为三种(加载的文件不同)

   1. Bootstrap ClassLoader 引导的类加载器

      虚拟机启动时需要使用到的文件,实现是C++,负责加载

      %java_Home%\jre\lib目标下的文件

   2. Extension ClassLoader 扩展类加载器

      负责加载%Java_Home%\jre\lib\ext 目录下的所有class,实现是Java

   3. Application ClassLoader 应用类加载器

      负责加载当前工程的类,程序默认的类加载;后续内容中引用的类加载都是ApplicationClassLoader

2. 内存空间

   ​		JVM启动,JVM其实是一个进程,进程一定会占用物理内存,JVM就会把操作系统分配的物理内存,进行管理,划分为若干区域

   1. 栈

      ​		栈也称为Java虚拟机栈,JVM私有;每个线程创建的时候会创建自己的JVM栈,每个栈之间互不干扰;每个方法执行完毕后,栈就会被销毁

      ​		Java栈是Java方法执行的内存模型,栈采用先进后出的顺序;存储的是方法执行的时候局部变量,包括方法中定义的非静态变量,以及方法的形参;对于基本数据类型变量,则是直接储存变量的值;对于引用类型(对象)的变量,储存的是指向变量的地址

      ​		栈按照程序块来创建,一个方法可能存在多个子栈(栈是私有的,method()方法一定会创建一个属于method()的栈,储存method()方法的局部变量,私有);

   2. 堆

      ​		储存对象的空间(new的空间)以及数组,Student s=new Student();	s开辟的站内的空间,储存着指向S对象的空间的地址,new Student()开辟的堆中的地址;堆是被共享的

      ​		堆又会分为两个区域:新生代,老年代

      ​		刚刚创建的对象以及使用频率目前比较高的都在新生代区域内,创建时间以及频率比较低的进入老年代

   ```java
   public class Student{
           public String stuName;
           public int stuId;
       public boolean equals(Object obj){
           Student s=(Student)obj;
           return this.stuId==s.stuId;
       }
   	public int hashCode(){
   		return this.stuId;
   	}
   }
   ```

   ​		**如果两对象的 equals 返回 true，那么要求两个对象的 hashCode 返回相同的整数** 如果两个对象 equals 返回 false，但是不要求两个对象的 hashCode 返回不同的整数

   ```java
   public class Test{
       public static void main(String[] args){
           
           Student s=new Student();
           s.stuName="张三";
           s.stuId=1;
           
           Student s1=new Student();
           s1.stuName="李四";
           s1.stuId=1;
           boolean bool1=s.equals(s1);
   }
   }
   ```

   Student被new了两次,也就是说:

   ​		堆区中Student的空间有两份;栈区存在两个独立的栈()

   

   3. 程序计数器

      ​		也被称为PC寄存器,程序计数器是会存在多个的;储存当前的代码的地址(执行是从main函数开始的,但是如果存在方法调用,那么会切入被调用的方法中执行,储存当前执行代码的位置)

   4. 本地方法栈

      ​		执行使用native关键字表示的方法;native方法执行使用的空间都在这个空间

   5. 方法区

      ​		储存了每个类的信息(类的名称,修饰符,方法信息,属性的信息),静态的属性,常量,方法信息;方法区也是被共享的;静态的属性和方法都储存在这个区域

3. 执行引擎

   ​		执行器

4. 回收内存空间

#### (4)finalize()

* finalize()作用:是当前垃圾回收器再回收当前对象所占用的空间的时候,会自动调用当前对象的finalize()方法

* 垃圾回收器

堆区主要储存的是new的对象,程序会不断new对象,但JVM的内存是有限的,所以要清理堆区中的内存,Java定义了一个组件,gc(垃圾回收器),主要负责清理不用的对象所占的空间,垃圾回收器只清理堆区的空间

* 当前gc回收对象所占空间时,会调用当前对象的finalize()方法

```java
public class Student{
    public Student(){
        System.out.println("创建Student的对象");
    }
    public void finalize(){
        System.out.println("Student的finalize()");
    }
}
```

```java
public class Test{
    public staitc void main(String[] args){
        Student s=new Student();
        //使s指向new出来的Student的空间
        s=null;
        //使s指向null,那么堆区中new出来的Student空间就没法再被引用
        //这时,new出来的Student的空间就是垃圾对象
        System.gc();
        /*建议垃圾回收器回收一次空间(gc会扫描并且回收堆区中不会被引用的垃		   圾对象),回收new Student这个空间时,会自动调用new Student空         间中的finalize()方法
                */
    }
}//out:创建Student的对象
     //Student的finalize()
```

#### (5)Object类中的final方法

Object类中出现final的方法,但又没有实现

* public final native Class<?>getClass();
* public final native void notify();

修饰为final的方法,子类不能重写,但是当前方法有没有方法体,到底是如何使用的呢

##### native关键字

native关键字(native标识的方法,称为本地方法)

JVM的内存划分:栈,堆,程序计数器,方法区,本地方法栈

使用native修饰的方法是使用C或者C++实现的,Java中只是定义,不用实现,真正的实现是使用C++实现

类加载器bootstrap Class Loader这种类加载器都是C++实现的,用来加载C++实现的方法