# Java学习(8)

[TOC]

## (1)参数传递

### 1.值传递:

```java
public class Test{
    public static void main(String[] args){
        int i=20;
        int result=calc(i);
        System.out.println(result);
    }
    public static int calc(int i){
        //定义为静态,main方法是静态,调用时不会出现错误
        return i+10;
    }
}//out:30
```

首先执行main函数,一定在栈中开辟一个空间,这个栈属于main函数,压入i=20;int result=calc(i)函数,开辟一个栈空间,这个空间压入i(作为calc函数的形参)

参数传递,内部储存的数据直接传递;main函数中i中储存的是20,直接把20传递到calc方法的形参i上(相当于从main中复制了一份),calc方法中也会存在20,return 30,直接把30返回,30为临时数据,不储存,执行完后30就被释放了,30被返回到main函数中的变量result中

### 2.引用传递

```java
public class Student{
    public int stuId;
    public String stuName;
    public String toString(){
        return "编号:"+this.stuId+"\t姓名:"+this.stuName;
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        Student s=new Student();
        s.stuId=1;
        s.stuName="张三";
        method();
        System.out.println(s);
    }
    public static void method(){
        s.stuId=1000;
        s.stuName="张三丰";
    }
}
```

在main方法栈中储存了一个对象变量(对象变量和变量的区别就在于类型不同)

* Student s=new Student();

s指向了堆中的一个空间(new Student()这个空间,储存在堆区中)

* s.stuId=1;

利用s中储存的地址访问了堆区中的new Student() 属性stuId 储存1

* s.stuName="张三";

利用s中储存的地址访问了堆区中的new Student() 属性stuName 储存 张三

* method(s);

方法调用,传递的是s,s是一个对象变量,内部储存的是堆区中的地址(指向对象),所以传递的是指向对象的地址,吧main栈中s储存的地址传递到method栈中的s

对象访问永远访问的是堆区中的空间

* System.out.println(s);

输出的是main方法栈中的s,但是在输出前,在method方法中利用method栈中的s(指向对象),修改了stuId,stuName属性的值(main栈和method栈是互不影响的,但是储存的s值一样,都指向一片空间)

#### ①.数组传递

```java
public class Test{
    public static void main(String[] args){
        int[] nums={1,2,3,4,5,6,7,8};
        
        change(nums);
        
        for(int n : nums){
            System.out.print(n+" ");
        }
        
    }
    
    public static void change(int...nums){
        nums[5]=1000;
    }
    
}//out:1 2 3 4 5 1000 7 8
```

数组作为形参的时候定义可以写成**int nums**或者**int[] nums**

nums储存的是数组的首地址,main栈中储存着nums变量,nums中储存着另外一片空间的地址,当调用change方法的时候,change栈中也储存了一个nums变量(参数传递过来的),指向的地址相同

**数组和对象采用的是引用传递**

**基本数据类型采用的是值传递**

## (2)抽象类

​	抽象类属于类,是一种特殊的类,在抽象类中可以存在未实现的方法(抽象方法),**存在抽象方法的类肯定是抽象类,但抽象类中不一定有抽象方法**,普通方法中不能有抽象方法

在一些特殊情况下,有些类的行为无法确定其实现细节,但是具备当前行为,就可以使用抽象类

```java
public abstract class Person{
    //定义为抽象类,用关键字abstract
    public int PersonId;
    public String PersonName;
    
    public void showPersonName(){
        System.out.println(this.PersonName);
    }
    public String toString(){
        return "编号:"+this.PersonId+"\t姓名:"+this.PersonName;
    }
    
    public abstract void eat();
    //定义抽象方法
}
```

* 抽象类不能创建对象,因为抽象类是一种没有完成的类定义,可能有未实现抽象方法,是一个不完整的类,一旦创建对象,调用方法的时候就会出现问题

* 不能被实例化,必须有子类或者实现完成抽象的方法,但是抽象类和接口可以作为上层类,实现多态

当一个类继承的是抽象类,分为两种情况:

1. 子类是普通类,那么这个子类就必须实现整个继承链中所有抽象方法,因为普通类中不能存在抽象方法

2. 子类是抽象类,那么对于抽象方法,可以随意实现,因为抽象类可以存在抽象方法
* 普通类方法不能缺少方法体

```java
public abstract class Person{
    public abstract void eat();//未实现的抽象方法
    
    public abstract void work;
}
```

```java
public abstract class Manager extends Person{
    public abstract void method();
}
```

```java
public class Emloyee extends Manager{
    public void method(){//"已实现"的"空实现"方法
        
    }
    
    public void work(){
        
    }
    
    public void eat(){
        
    }
    
}
```

* Emloyee是一个普通类,继承了Manager,Manager又继承了Person,在整个继承链中,一共有三个抽象方法,所以要实现三个抽象方法
* 抽象类是可以继承普通类的,所以抽象类的顶层父类也是Object类

***

### 抽象类与普通类的唯一区别

* 普通类可以创建对象,抽象类不能创建对象

## (3)接口

* 使用interface定义,不再使用class定义,**在接口中只能定义抽象方法不能存在普通方法**,所以接口不能继承普通类以及抽象类(可能含有普通方法),所以Object类也不会继承了
* 接口是一种完全的抽象类,接口相较于抽象类更加抽象,是一种通用的行为定义,不用确定是哪一类别的行为,在不确定类别的行为时,一般定义接口
* 接口比类轻量级,只是规定实现者的行为,而不规定实现者是什么
* 接口可以被普通类实现(不是继承),**一个类可以实现多个接口,子类使用 ** **implements**关键字标识实现的接口,实现多个接口时,之间用逗号隔开,如果一个抽象类实现接口,是可以实现也可以不实现接口定义的方法
* 例如:Collection,List,Set这些接口都是集合的接口,集合的操作类型是什么,集合并不会规定

```java
public interface MyInter{
//定义了一个接口,只能定义抽象方法
//接口不能创建对象(含未实现的抽象方法)
    public void sayHello();
//当前应该为抽象方法,但没有使用关键字abstract
//理由:接口中的方法默认都是抽象方法,所以关键字
//abstract可有可无
}
```

```java
public class Student implements MyInter{
    public void show(){}//空实现的普通方法
    
    public void sayHello(){};
    
}
```

```java
public interface MyInter1{
    public void eat();
    public void sing();//未实现的抽象方法
}
```

* 普通类实现了接口,必须实现接口的方法

```java
public abstract class Teacher implements MyInter{
    
}
```

```java
public abstract class Teacher implements MyInter{
    public void show(){}//空实现的普通方法
    
    public void sayHello(){};
    
}
```

* 抽象类实现类接口可以实现也可以不实现

```java
public class Student implements MyIner,MyIner1{
    public void eat(){};
    public void sing(){};
    public void show(){};
    public void sayHello(){};
}
```

```java
public abstract class Teacher implements MyInter,MyIner1{
    
}
```

* 普通类和抽象类都可以实现多个接口

  子类实现接口,分为两种情况:

  1. 子类为普通类,就必须实现接口的方法
2. 子类为抽象类,可以实现也可以不实现,当前类了以实现多个接口
  
* 接口内部方法默认都是抽象方法,也就是说接口的方法都不能存在方法体,内部定义的属性默认都是静态常量

* 抽象类中可以定义的属性:常量,普通属性,静态属性

* **接口中只能定义静态常量和抽象方法,并且定义接口属性必须赋值**

* 接口中的方法访问修饰符只能是public和默认

```java
public interface MyInter{
    public String name="张三";
}//接口不能创建对象
```


```java
public class Student implements MyInter{
    
}//普通类实现接口
```

```java
public class Test{
    public static void main(String[] args){
        System.out.println(Student.name);
    }
}//out:张三
```

* MyInter中应该是定义一个常量,三十没有使用final和static关键字修饰,Student类实现了MyInter接口,但是Student类并没有定义任何的属性和方法,在Test类中也没有创建Student类的对象,直接输出了Student.name,从调用来看,是利用了类名调用,所以一定是静态的属性

**在接口中定义的属性,一定是静态常量,如果定义的时候没有使用关键字final和static,那么系统编译的时候会自动加上final和static**

* JDK1.8后,接口还可以存在两种实现的方法
  1. 使用default标识的方法
  2. 使用static标识的方法,但是static修饰的方法必须实现

### 在接口中default以及static标识的方法是可以实现的

```java
public interface MyInter{
    public String name="张三";
    
    public void eat();
    
    //public default void defaultMethod也可以
    default public void defaultMethod(){
        System.out.println("MyInter default method");
    }
    
    //public static void staticMethod也可以
    static public void staticmethod(){
        System.out.println("MyInter static method");
    }
    
}
```

* **default标识的方法必须使用对象调用,接口是不能创建对象的,所以必须创建子类的对象,使用子类的对象调用**

* **static标识的方法只能通过接口来调用(与调用类的静态方法形式类似)**

```java
public class Student implements MyInter{
    public void eat(){
        System.out.println("Student eat");
    }
    //普通方法实现接口,必须实现抽象方法
}
```

```java
public class Test{
    public static void main(String[] args){
        System.out.println(Student.name);
        //静态属性的调用通过类调用
        MyInter.staticMethod();
        //接口中定义的静态方法使用接口调用
        Student s=new Student();
        s.defaultMethod();
        s.staticMethod();
        //接口中定义的default方法只能通过对象调用
        //静态属性也可以通过对象调用
    }
}
```

### 接口作为参数类型的使用

#### 方式一:定义参数需要的接口

```java
public interface MyInterFace{
    public void interfaceMethod();
}
```

* 定义一个接口,接口内部定义一个方法

```java
public class MyTest{
    public void testMethod(MyInterFace interFace){
        interFace.interfaceMethod();
    }
}
```

* 定义类,类中存在一个方法,这个方法的参数类型是接口,方法内部调用接口的方法

```java
public class MyInter implements MyInterFace{
    public void interfaceMethod(){
        System.out.println("MyInterFace的interfaceMethod");
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        MyTest test = new MyTest();
        test.testMethod(new MyInter());
    }
}
```

* 定义测试类,测试类中需要调用MyTest类中的testMethod()方法,那么就需要给testMethod()方法传递参数(实参),但是这个实参的类型是一个接口,接口是不能直接实例化的,所以需要定义一个类来实现接口,然后创建这个实现类的对象传递到testMethod()中

#### 第二种方式:不用定义 实现接口的类

```java
public interface MyInterface{
    public void interfaceMethod();
}
```

```java
public class MyTest{
    public void testMethod(MyInterface myInterface){
        myInterface.interfaceMethod();
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        MyTest test = new MyTest();
        test.testMethod(
         ********************************************
         new MyInterFace(){
            public void interfaceMethod(){
                System.out.println("直接创建MyInterface接口的匿名对象");
            }
        });
        *********************************************
        test.testMethod(new MyInterface(){
            public void interfaceMethod(){
                System.out.println("hello java");
            }
        });
    }
}
```

* 实参直接创建接口的匿名对象(相当于有一个匿名类实现了接口,然后创建匿名类的对象),传入到MyInter的方法中

## 总结(类,抽象类,接口)

* 接口不能继承类或者抽象类,但接口可以继承且可以继承多个接口(extends,但类与类之间是单继承),接口和接口之间是继承关系,只有类,抽象类和接口之间是实现(implements)关系

* 如果一个类继承了其他类,并且同时又实现了接口,那么必须先继承后实现(extends在implements之前)

### 抽象类与接口的区别:

  从语法角度:

抽象类使用abstract class定义,抽象类中可以定义普通属性,常量,静态常量,普通方法和静态方法

接口使用interface定义,只能定义静态常量,抽象方法

从面向对象的设计角度:

抽象类是类别抽象,(类是一组具有相同属性和行为的一种抽像描述),一定是当前实物归属是什么类别

接口是行为抽象,接口只是描述行为,当前实物应该具有什么样的行为

类是规定当前实物是哪一类别,接口只是规定当前实物应该具有哪些行为

类和抽象类都是类的范畴,接口是行为的范畴

## (4)final关键字

### 1.final修饰变量

**一个类的某一属性被修饰为final,当前变量就称为常量,常量值就不能修改,只能被赋值一次**

```java
public class Person{
    public final int NUMBER=10;
}
```

```java
public class Student extends Person{
    public void method(){
        this.NUMBER=20;
    }
}
```

* 报错:无法为最终变量NUMBER分配值

### 2.final修饰方法

```java
public class Person{
    public final void show(){
        System.out.println("Person show");
    }
}
```

```java
public class Sutdent extends Person{
    public void show(){
        System.out.println("Student rewrite show()")
    }
}
```

* 错误:Student中的show()无法覆盖Person中的show();
* 被覆盖的方法为final
* java中的类的方法默认是可以被重写的,**当一个方法被修饰为final时,那么当前的方法就不能被子类重写,当前方法可以重载,子类只能重写父类中没有定义为final的方法**
* 如果一个类的某一方法不希望被重写,就可以定义为final

### 3.final修饰类

```java
public final class Person{
    public void show(){
        System.out.println("Person show");
    }
}
```

```java
public class Sutdent extends Person{
    public void show(){
        System.out.println("Student rewrite show()")
    }
}
```

* 错误:无法从最终类Person进行继承;
* **如果一个类被定义为final,那么当前类就不能被继承,不会有子类,叫做最终类(密封类),但可以被实例化(参考String类),可以创建对象**