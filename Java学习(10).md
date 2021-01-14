# java学习(10)

[toc]



***

## (1)package(包)

一个软件工程是由多人协作开发的,所以经常会出现类的命名冲突的问题,所以Java提出了包的概念--package

package 包名;//定义当前类所属的包

包名一般是当前机构的域名的倒置,例如:淘宝 开发项目 报名 com.taobao.xxx

不同的包可以存在相同名称的类(防止命名冲突),将功能类似的类放置在同一个包中,利于管理

![image-20200731174010701](D:\wj\Documents\Java\image-20200731174010701.png)

![image-20200731174234546](D:\wj\Documents\Java\image-20200731174234546.png)

包名:com.wj.Demo.Demo

文件夹:com\wj\Demo\Demo.java

如果当前类没有包,那么不存在package的定义;**如果存在,package的定义一定要在当前类的第一行**

```java
package com.wj.Demo;
//定义在第一行
import com.wj.Demo1.Student;
/*
表示在当前类中引入了com.wj.Demo1.Demo类
但是当前代码只是引入了Demo类,其他类并没有引入
一次导入一个包下的一个类
*/
import com.wj.Demo1.Teacher;
import com.wj.Demo1.*;
//" * "表示通配符,导入指定包下面的所有类
public class Demo{
    public static void main(String[] args){
        Student s=new Student();
        //Student类在com.wj.Demo1包中,用import com.wj.Demo1.Student导包即可使用
    }
}
```

如果属于不同包的类需要相互引用,那么就需要使用import,将包导入到当前类

![image-20200731181524738](D:\wj\Documents\Java\image-20200731181524738.png)

* Object类的源代码

![image-20200731181425932](D:\wj\Documents\Java\image-20200731181425932.png)

* String类的源代码

以上两个类的源码中都有package.java.lang,证明Object和String类都定义在java.lang包下

***


```java
package com.wj.Demo;
public class Student{
    public static void main(String[] args){
        String s1=new String("abc");
        Object obj=new Object();
    }
}
```

发现目前在自定义下的工程中package com.wj.Demo包下的Student类引用了Object以及String类,但没有导包(没有 import java.lang.Object 和 import java.lang.String),编译通过,运行

**java.lang包是系统在编译时会自动导入的包,所有使用java.lang包下的类都是不需要导包的**

## (2)访问修饰符

public 共有的,所有包的所有类都能访问

private 私有的,只能当前类访问

protected 受保护的

默认的 (没有明确定义)

default 抽象类中使用,只能用子类(普通类)通过子类对象调用

```java
package com.wj.Demo;

public class Student{
    
	public String name;
    private int age;
    protected String sex;
    int stuId;//没有明确的定义,都可以访问
}
```

```java
package com.wj.Demo;

public class Teacher{
    
    public void method(){
        Student s=new Student();
        s.name="wj";
        s.sex="male";
        s.stuId=1;
        //s.ageStudent类私有,Teacher类不能访问
    }
    
}
```

```java
package com.wj.Demo1;

import com.wj.Demo.Student;

public class Test{
    public static void main(String[] args){
        Student s=new Student();
        s.name="wwj";
        //s.age
        //s.stuId
        //s.sex
    }
}
```

* 不同包的类中只能访问public
* private,protected,默认都不能访问

```java
package com.wj.Demo1;

import com.wj.Demo.Student;

public class Test1 extends Student{
    
    Student s=new Student();
    s.sex="male";
    
}
```

* 通过实例化子类对象来调用子类从父类继承来的属性(public,protected,默认)

**某父类protected权限的成员，子类是可以直接访问的，换一种说话是子类其实继承了父类的除了private成员外的所有成员，包括protected成员，所以与其说是子类访问了父类的protected成员，不如说子类访问了自己的从父类继承来的protected成员。另一方面，如果该子类与父类不在同一个包里，那么通过父类的对象实例是不能访问父类的protected成员的**。

## (3)包装类

包装类一共定义了八种,用于包装八种基本数据类型类,int,byte,short,long,double,float,char,boolean八种基本数据类型不是类,不属于对象的范畴,所以Java专门定义了八种包装类,就是为了把八种基本数据类型包装成对象

| Byte→byte       | Short→short       |    Integer→int     | Long→long           |
| :-------------- | :---------------- | :----------------: | :------------------ |
| **Float→float** | **Double→double** | **Character→char** | **Boolean→boolean** |

​	

想要将字符串"12"转换为整型数字12

简单类型就无法实现,需要自己写方法

类中会拥有很多方法,Integer就有将字符串转换为整型的方法

包装类的意义:类中会拥有很多方法,更方便

​	装箱与拆箱问题:

装箱和拆箱是发生在基本数据类型和包装类之间的问题.Java中存在8种基本数据类型(基本数据类型不属于类和对象的范围),Java又是一种面向对象的语言,在Java语言中一切皆为对象,所有的数据都是使用类和对象来描述的;所以需要把基本数据类型也提升为类和对象,所以Java中定义针对8种基本数据类型的包装类,定义的包装类完全是为了把基本数据类型包装为对象和类;所以在Java中规定包装类的对象和基本数据类型的变量之间都是可以相互转换的

```java
int num=10;
```

定义了一个基本数据类型的变量num,内部储存的是10

```java
Integer num1=num;
```

定义了一个Integer类型的对象num1

按照Java语言的规定,对象是需要通过new关键字创建的,但是发现也可以直接把一个基本数据类型的变量直接赋值给对象

Java允许这种情况发生,Java会自动完成从基本数据类型到包装类的转换

基本数据类型的变量转换为对应的包装类的对象,这个转换成为装箱

​		

​	

