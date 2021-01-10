# Java学习Day4

[toc]

### 1.类和对象

文件:一个文件一个类,一个类有多个函数组成

C语言的程序结构:文件(物理概念)一文件夹中包含若干个函数,一个工程由多个文件组成

从代码角度:C 文件中直接是定义函数	Java文件中是一个类(类的内部是函数)

C语言是一种面向过程的语言	Java是一种面向对象的语言

面向对象和面向过程的区别:

面向,即关注点:

面向过程:关注点是实现的过程,就以函数的方式来定义(一个业务划分为一个函数),由若干个函数从逻辑上组成一个文件,把一个系统划分为多个文件;面向对象核心关注点是在对象,对象的来源就是类;实现具体的业务的时候先确定完成当前业务的对象是哪个,而不是具体的业务怎么实现

例如:实现一个做饭的业务:

​	如果是C语言:关注点是在做饭怎么实现,第一步要做什么,接着做什么,关注点是当前的业务实现细节

​	如果是Java语言:关注点是做饭的业务由谁完成,只有确定了完成业务的对象,在业务对象的内部存在一个做饭的方法(在这个方法中才具体完成业务).

C语言不关注业务是谁完成的,只关注业务的具体实现,所以C的结构就是以函数来组成,因为函数就是具体完成业务的;Java关注点是业务是由谁完成;所以C语言和Java语言的关注点不同,那么所以先按照业务划分对象,以类的方式定义程序

例如:餐厅的管理系统:

C语言,只考虑当前餐厅的所有业务,例如:做饭,收钱,采购......

相应的业务对应函数就可以了(做饭的函数,收钱的函数,采购的函数......);

如果是Java语言,考虑当前的业务由那个对象完成

做饭由厨师完成,收钱由收银员完成,采购由采购的人员完成

分析出来有三个对象,就会先定义三个类(厨师类(做饭的函数),收银员(收钱的函数),采购人员(采购的函数))

在Java世界中一切都是对象,所以要先定义类,才能通过类来创建对象

按照系统的业务分析,分析出当前系统的所有对象,按照分析的对象来定义类(建模过程)

```java
//Java中定义一个类
class 类名{
}
```

一个类中包含函数,还有属性(定义的全局变量)

```java
public class Student{
    public String stuName;//小驼峰
    public String stuSex;
    public int stuAge;
    //定义的三个属性
    public void showStuName(){
        System.out.println("姓名"+stuName);
    }
    public void study(){
        System.out.println(stuName+"正在学习");
    }
    //定义的两个方法(函数)
}
```

一个类中可以包含:

1. 属性:描述当前对象的特征(名词部分)
2. 方法:描述当前对象的行为(动词部分)

* 属性和方法共同来描述一个类
* 类是描述对象的,或者说是定义对象的,类是对象的图纸,对象是类的具体;类主要是定义,规定;对象是具体实现
* 前面说了餐厅的管理系统经过分析,有三个参与对象:厨师,收银员,采购员;当前的系统是需要计算机执行,那么计算机应该也不知道厨师对象应该具有什么行为和属性,那么就需要以代码的形式告诉计算机厨师应该用有哪些属性和方法;需要让计算机知道我们业务分析中厨师,收银员,采购员是什么样子;先建模,建立模型这个过程就是以类的形式告诉计算机当前业务出现的厨师,收银员,采购员是什么样子
* 类其实就是一种类似图纸的东西,就像造汽车的图纸,按照图纸造一个具体的汽车,图纸就是定义的类,按照图纸造出来的汽车就是对象
* 类中的语法规定是可以定义属性和方法的,一个对象的属性和方法可以有很多,一定要按照与当前业务系统相关的属性和方法来定义类

  * 例如:定义一个教学管理系统:在当前系统中业务分析出有教师的参与者对象,教师的属性(特征)和方法(行为)可以有很多,注意一定要选取和当前业务相关的属性和方法来定义,例如:教学管理系统,老师是否结婚(可以是一个属性)这个属性和当前业务系统无关,那么这个属性就不应该出现在当前类中,例如教师是否会做饭(做饭是一种行为,就是方法,但是这个方法和当前业务无关,也不应该出现在当前类中

  * 例如餐厅管理系统,按照业务系统的分析会存在厨师这个对象,分析完有厨师对象那么就应该先定义厨师类来描述厨师对象,当前厨师会不会做饭就很重要,当前类中一定会存在做饭的方法,那么初始会不会唱歌就和当前业务无关,所以这个行为就不应该出现在当前厨师类中),一个类中的属性和方法一定是和当前对象在当下系统中完成的业务来确定的
* 一个类只能描述一种实物对象,也就是说一个类不能描述多个实物对象;一个类要保持单一性(包括一个函数也要保持单一性,就是一个函数只能描述一个业务)

### 2.创建类的对象

通常Java中把类的变量都称为对象,Java中的对象其实就是指针指向的那个对象

```java
public class Student{
    public String stuName;//小驼峰
    public String stuSex;
    public int stuAge;
    //定义的三个属性
    public void showStuName(){
        System.out.println("姓名"+stuName);
    }
    public void study(){
        System.out.println(stuName+"正在学习");
    }
    //定义的两个方法(函数)
}
```

```java
//Test类是一个测试的类,测试时需要运行,所以应该存在main函数
public class Test{
    public static void main(String[] args){
        /*
        可以创建Student类的对象
        Student s;定义对象,但是当前对象没有赋值,类似于int num;
        当前Student类中定义的有三个属性,两个方法,三个属性是需要占用内存空间的
        创建对象的时候就是按照类的定义来分配内存空间的,Student()就是图纸
        new Student()就会按照Student类定义的属性和方法来分配空间,然后把new(分配)的这片空间交给 s 这个对象,那么以后 s 就指向new出来的这片空间
        */
        Student student=new Student();
    }
}
```

![image-20200720090049693](D:\wj\Documents\Java\image-20200720090049693.png)

* 访问对象的属性和方法 :
  1. 对象.属性;
  2. 对象.方法名();

```java
public class Test{
    public static void main(String[] args){
        Student student=new Student();
        s.stuName="张无忌";
        s.stuSex="男";
        s.stuAge=20;
        s.showStuName();
    }
}
```

* 一个类可以创建多个对象,都各自拥有类的属性和方法,互不干扰,每创建一个对象,就新创建一次类的属性和方法

```java
public class Test{
    public static void main(String[] args){
        Student student=new Student();
        s.stuName="张无忌";
        s.stuSex="男";
        s.stuAge=20;
        s.showStuName();
        
        Student student1=new Student();
        student1.stuName="赵敏";
        student1.Sex="女";
        student1.Age=18;
        student1.showStuName();
        student1.study();
    }
}//用Student作为一张图纸造出了两个对象,student,student1
```

#### (1)物理内存

Java虚拟机对于当前工作的计算机操作系统来说其实是一个程序,计算机系统会给当前的程序分配空间,Java虚拟机在拿到被分配的内存会划分出不同的区域

Nativemethodstack(本地方法栈)：保存native方法进入区域的地址。

##### 方法区

又叫静态区

​		存放所有的①类（class），②静态变量（static变量），③静态方法，④常量⑤成员方法。

​		1.又叫静态区，跟堆一样，被所有的线程共享。

​		2.方法区中存放的都是在整个程序中永远唯一的元素。这也是方法区被所有的线程共享的原因。

##### 堆区:

​		存储对象本身（包括class对象和异常对象）,成员变量和数组，堆是GC所管理的主要区域（对不需要的对象进行标记，而后进行清除）。![image-20201019210444002](D:\wj\Documents\Java\image-20201019210444002.png)

* 查看当前堆区的空间的大小,默认最小是16MB，最大时64MB；

##### 栈区:

​		栈中只保存基本数据类型的对象和自定义对象的引用，对象都存放在堆区中。每个栈中的数据（原始类型 和 对象引用）都是私有的，其他栈不能访问。

​		栈用于存储程序运行时在方法中声明的所有局部变量（栈主要存储方法中的局部变量）。

**JVM会为每一个方法分配一个对应的空间，这个空间称为该方法的栈帧。**一个栈帧对应一个正在调用的方法，栈帧中存储了该方法的参数、局部变量等数据。当某一方法调用完成后，其对应的栈帧将被清除，局部变量失效。（方法结束，局部变量失效，从栈中清除）

每个栈帧里面都会存储有相应的一下内容：
        1.局部变量表
        2.操作数栈
        3.动态链接
        4.返回地址

##### 程序计数器:

Java虚拟机中最小的一片内存空间,作用是存储当前程序执行的字节码的行号

![image-20200913090237916](D:\wj\Documents\Java\image-20200913090237916.png)

1. 加载Student.class文件进内存
2. 在堆中给学生对象开辟空间
3. 对学生对象的成员变量进行默认初始化(NULL,0......)
4. 对学生对象的成员变量进行显式初始化(显式赋值)
5. 通过构造方法对学生对象进行构造初始化
6. 学生对象初始化完成,把对象的地址值赋给s变量

****



![image-20210102154246850](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210102154246850.png)

![image-20210102154419578](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210102154419578.png)

![image-20210102154440453](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210102154440453.png)

![image-20210102154729436](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210102154729436.png)

Thread:线程

每个线程都会拥有这三块内存

堆,方法区共享

局部变量存储在栈中

#### (2)修饰符

可修饰类,成员变量,内部类成员变量,方法

在上述举例中,stuName是定义在Student这个类中,访问是在Test类中,属性的定义访问跨类了,public的作用就显示出来了,目前Student类中定义的stuName使用的是public那么在Test类中访问是没有问题的(是可以访问的)

public	公开的(访问修饰符,访问级别,定义能访问的范围)	public在任何位置都能访问(只要是当前工程的任何类都能通过创建对象,通过	对象.属性   来访问),属性和方法一定要先创建对象后,才会存在

private	私有的,也是一种访问修饰符,private的范围就只在当前类中可以访问,除了当前类的范围就不能访问(当前类范围:public class Student{}在当前类的大括号中)

***

#### (3)属性

```java
public class Student{//默认值
    public int stuId;//0
	public String stuName;//null
	public String stuSex;//null
	public int stuAge;//0
}
```

* **属性(成员变量)没有赋值,但会有默认值,在创建对象分配空间时,系统会给默认值.局部变量需要赋初始值才能使用**
* 局部变量会存储于栈上,函数结束后,栈空间就会被回收	

***

#### (4)方法(函数)

* Java中没有函数的声明,~~public static void 方法名(形参列表);~~  

一个类中可定义多个方法,定义方法的语法:

访问修饰符 返回值类型 方法名称(参数列表类型 参数名称){

​		方法体内容;

}

* 在Java中只有按值传递,地址引用,其实也是一个值

```java
public class Student{
    
    public int stuId;
	public String stuName;
	public String stuSex;
	public int stuAge;
    
public void method(){
    System.out.println("method");
}
    
//定义方法
/*
	public 是当前方法的访问修饰符(定义当前方法能被调用的范围)
	void 是当前方法的返回值类型,无返回值类型,不用return返回值(目前所学的数据类型都可以当作返回值类型)
	method 方法的名称
	() 表示当前方法的参数列表
*/
    
    public int getStuId(){
        return stuId;
        System.out.println("vsvsdvsda");//无法访问的语句,一个方法在return数据之后就认为当前方法执行完毕
    }
    
    /*
    方法的定义部分:访问修饰符 返回值类型 方法名 参数   ----方法签名
    一个方法只能返回一个数据 不能用return来返回多个数据
    */
    
    public String getName(){
        return stuName;
    }
    
}
```

* 方法的调用(前提是要创建对象,只有创建了对象,方法和属性才会存在)

```java
public class test{
    public static void main(String[] args){
        Student s=new Student();
        s.stuName="张无忌";
        s.stuId=10;
        s.method();
        int id=s.getStuId();
        //getStuId()会有返回值,用返回值类型相同的类型的变量接受返回值
        System.out.println(id);
    }
}
```

* 方法的返回值类似于方法的数据出口,方法的内部数据对于方法外是隐藏的,外部是不能访问方法内部的数据

```java
public class Student{
public int method1(){
    int num=10;
    /*
    定义在方法内部,属于方法内部数据
    只能在当前方法的内部使用,出了当前方法的范围,数据无效
    包括当前类的其他方法也是不能访问当前方法的内部数据
    */
}
}
```

```java
public class Test{
    public static void main(String[] args){
        Student s=new Student();
        s.method1();
        /*
        不能访问method1内部的数据(num)
        这个地方需要获取method1内部数据(num)只能通过返回值的方式把内部数据返回给调用者
        */
    }
}

```

#### (5)方法的参数

方法的参数是当前方法的数据的入口;可以把外部的数据传递到方法的内部;返回值是方法的出口,可以把方法内部的数据传递到方法的外部;

public void method(参数的定义),参数的定义包括两个部分:1. 参数的类型 2. 参数的名称

方法定义时候的参数 称为 方法的形参

方法被调用时候的参数 称为 方法的实参

```java
public class Student{
    public int calc(int a,int b){//定义形参
        return a+b;
    }
    private void sayHello(){//修饰符为private,只可在当前类中调用
        System.out.println("Hello");
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        Student s=new Student();
        int result=s.calc(1,2);//传递实参
        //一个方法定义了形参,调用时就必须按照形参定义形式给定实参(按照顺序赋值且类型匹配)
        System.out.println(result);
    }
}
```

#### (6)匿名代码块

```java
public class Student{//一个类中可以拥有匿名代码块
    
    {//和定义方法类似,但没有方法的定义部分
        System.out.println("匿名代码块");
    }
}
```

匿名代码块的执行,匿名代码块不能调用,因为调用都是通过名称调用

在创建对象(new)的时候自动会调用,每创建一次就会自动调用一次

#### (7)匿名对象

```java
new Student();//没有名字,不会在栈中开辟空间,每new一次就会在堆中开辟新的空间,用完后就会变成垃圾对象被回收,**可当作参数来传递**
new Student().study();//当然也可以使用对象的方法
```



#### (8)类中定义常量

```java
public class Calculator{
    public String brand;
    public double price;
    
    public final int NUM=10;//常量命名规范:大写字母
    //必须赋值
    
}
```

```java
public class Test{
    public static void main(String[] args){
        Calculator c=new Calculator();
        System.out.println(c.NUM);//✔
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        Calculator c=new Calculator();
        c.NUM=2;//❌常量不可再赋值
        System.out.println(c.NUM);
    }
}
```

#### (9)方法的重载

方法的重载,一定是在当前类中(一个类当中),存在两个或两个以上的**相同方法名称,但是参数(不同的参数类型和参数个数)不同**的多个方法,称为方法的重载

**!!方法的返回值类型不作为判断依据!!**

```java
public class Calculator{
    public int calcAdd(int i,int j){
        return i+j;
    }
❌: public double calcAdd(int i,int j){
        return i+j;
    }
    //会报错,重复定义
}
```

```java
public class Calculator{
    public int calcAdd(int i){
        return i+10;
    }
    public int calcAdd(int i,int j){
        return i+j;
    }
    public double calcAdd(double i,double j){
        return i+j;
    }
    public double calcAdd(int i,double j){
        return i+j;
    }
    public double calcAdd(double i,double j,double k){
        return i+j;
    }
}//相同方法名,不同的参数类型和参数个数,统称为方法的重载
//系统会根据参数的类型和个数自动匹配调用的方法
```

```java
public class Test{
    public static void main(String[] args){
        Calculator c=new Calculator();
        int result=c.calcAdd( 2 );
        //返回值是int型,用int型接收
        System.out.println(result);
        int result1=c.calAdd( 1 , 2 );
        //返回值是int型,用int型接收
        System.out.println(result);
        double result2=c.calcAdd( 2.1 , 4.50 );
        //返回值是double型,用double型接收
        System.out.println(result);
        double result3=c.calcAdd( 2 , 7.8 );
        //返回值是double型,用double型接收
        System.out.println(result);
        double result4=c.calcAdd( 1.5 , 2.0 , 3.6 );
        //返回值是double型,用double型接收
        System.out.println(result);
    }
}
```

* 私有方法,静态方法也是可以被重载的,静态方法只是存在份数和初始化时机的问题

### 3.内部类

内部类不推荐使用

内部类是在一个类中存在一个完整的类,内部类会破坏类的封装性,因为一个类是描述一个事物的抽象描述,要保持单一性,而内部类会破坏这种单一性,所以内部类是特殊情况下使用的一个方式

内部类,同样遵循类的原则,可以定义普通属性和方法(当内部类为静态时,才可定义静态属性和方法)

#### (1)创建内部类

**创建一个外部类(Outer),在Outer类的内部创建一个普通的内部类(Inner),存在普通属性和方法**

```java
public class Outer{
    
    public String outerName;
    
    public void outerMethod(){
        System.out.println(outerName);
    }
    
    public class Inner{
        public String innerName;
        public void innerMethod(){
            System.out.println(outerName);
            //内部类可以访问外部类的成员
            System.out.println(innerName);
        }
    }
}
```

#### (2)创建外部类对象

```java
public class test{
    public static void main(String[] args){
        
        Outer outer = new Outer();
        outer.outerName="张三";
        outer.outerMethod();
        
    }
}
```

#### (3)创建内部类对象

方式一:(利用创建外部类的对象来创建内部类)

```java
Outer outer = new Outer();
OUter.Inner inner = outer.new Inner();//外部类的对象创建内部类
inner.innerName="李四";
inner.innerMethod();
```

方式二:没有创建外部类的对象的情况

必须先创建外部类的对象,因为当前类是普通类,普通类相当于和外部类的属性和方法一个级别,一个类的对象和方法必须是创建对象才存在

```java
Outer.Inner inner1 = new Outer().new Inner();
inner1.innerName = "王五";
inner1.innerMethod();
```

#### (4)静态内部类

```java
public class Outer{
    
    public String outerName;
    
    public void outerMethod(){
        System.out.println(outerName);
    }
    
    public static class Inner{
        public String innerName;
        public void innerMethod(){
         ❌:System.out.println(outerName);//不能访问外部类的非静态属性
            System.out.println(innerName);
        }
    }
}
```

* 静态内部类与静态的方法与属性是同一级别
* 静态类当中可以定义普通属性,不能访问外部类的非静态属性和方法,可以访问外部类的静态属性和方法,静态类中的非静态方法可以访问外部类的静态方法和属性,这与初始化时机有关

#### (5)创建静态内部类的对象

```java
public static void main(String[] args){

    Outer.Inner inner = new Outer.Inner();
    inner.innerName = "李四";
    inner.innerMethod();
    
}
```

* 创建静态内部类的时候,不需要创建外部类的对象,相当于外部类的静态属性和方法

#### (6)局部内部类

```java
public class Outer{
    public String outerName;
    public void outerMethod(){
        
        class Inner{
            public String innerName;
            
            public void innerMethod(){
                System.out.println(innerName);
            }
        }
        Inner inner = new Inner();
        inner.innerName = "王五";
        inner.innerMethod();
    }
}
```

* 局部内部类在方法中定义,只能在当前方法中创建内部类的对象,所以在外部类只用创建外部类的对象,利用外部类的对象调用,定义局部内部类的方法即可完成对局部内部类的对象的创建

```java
public static void main(String[] args){
    Outer outer = new Outer();
    outer.outerName = "张三";
    outer.outerMethod();
    //调用这个方法后就创建了局部内部类的对象
}
```

