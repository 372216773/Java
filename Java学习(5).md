# Java学习(5)

[toc]

### 1.构造方法

1. 构造方法的方法名必须和当前类的名称相同
2. 构造方法没有返回值,并且不能使用void来描述
3. 在创建对象的时候会自动调用构造方法

```java
public class student{
    public student(){
        System.out.println("student的构造方法");
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        student s=new student();
        //在创建对象的时候一定会自动调用构造方法
    }
}
```

**new student()就是在调用student类的构造方法**

* 构造方法的重载:

```java
public class student{
    public student(){
        
    }
    public student(int stuId,String stuName,String Sex){
        
    }
    //一个类中出现多个构造方法,但是构造方法的参数列表不同,称为方法的重载
}
```

***

```java
public class student{
    public int num;
    public String stuName;
    public void work(){
        System.out.println(stuName+"正在上课");
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        student s=new student();
        t.stuName="张三";
        t.work();
    }
}
```

* 这两个类可以正常编译,运行
* **问题:student类中并没有构造方法,但是Test类的main函数中new student()是会调用student类的没有参数的构造方法,那么为什么这两个类可以正常编译,运行**
* 每个Java程序都会经过:编写类的源代码,编译当前的类,编译完成生成一个class文件,class文件存在在磁盘中,Java在运行的时候第一步一定要把磁盘中的class文件加载到内存(获取到图纸),才能根据class的定义创建对象,即分配空间,调用构造方法,创建对象也称为实例化

* **一个Java类在编译时,如果发现当前类中没有显式的定义任何类型(有参或无参)的构造方法,那么在系统编译时会自动为当前类添加一个无参数的空实现(方法体中无语句)构造方法**
* student类在创建对象的时候一定经过了编译阶段,所以student类运行时是会存在一个空实现的无参数构造方法

* 当自己手动为student类添加构造方法(有参无参都可以)时,在编译阶段系统就不会再添加无参数的构造方法了

```java
public class student{
    public int num;
    public String stuName;
    public student(){
        System.out.println("student的构造方法");
    }
    {
        System.out.println("student的匿名代码块2");
    }
    {
        System.out.println("student的匿名代码块1");
    }
    public void work(){
        System.out.println(stuName+"正在上课");
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        student s=new student();
    }/*out:student的匿名代码块2
    	   student的匿名代码块1
    	   student的构造方法
         */
}
```

***

**一个类的所有元素:普通方法,构造方法,代码块,静态代码块**

```java
public class Demo{
    public String name;
    //属性(值可以被修改)
    public final String SCHOOL="";
    //常量属性(这个属性的值不能被修改)
    public static String sex="";
    //类属性(静态属性) 只存在一份属于类,被当前所有类的对象共享
    //在类加载的时候初始化
    public final static String NUMBER="1000001";
    //静态常量属性 只存在一份属于类,被所有当前类的对象共享
    //在类加载的时候初始化,不能被修改值
}
```

#### 总结

在创建对象的时候,匿名代码块和构造方法都会自动调用,执行顺序

①匿名代码块(多个匿名代码块时按定义顺序执行)

②构造方法

***

### 2.static	静态

//为某大学做一套教学管理系统

```java
public class student{
    public String name;
    public String className;//变量命名小驼峰
    public String schoolName;

    public void study(){
        System.out.println(stuName+"正在上课");
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        
        student s=new student();
        s.name="张三";
        s.className="101";
        s.schoolNanme="某大学";
        s.study();
        
        student s1=new student();
        s1.name="李四";
        s1.className="102";
        s1.schoolNanme="某大学";
        s1.study();
}
```

* 上述schoolName这个属性在这个业务中是固定的,每个对象的schoo属性都相同,并且不能修改,解决上述问题,可以再student类中把schoolNanme属性用**final**修饰定义为常量,schoolNanme属性就不会被修改,为固定值,但还是作为当前对象的属性存在

```java
public class student{
    public String name;
    public String className;
    public final String schoolName;

    public void study(){
        System.out.println(stuName+"正在上课");
    }
}
```

* 但是final作为对象的属性,每次创建对象的时候,还是会创建一份schoolName属性.如果对象非常多,就会造成不必要的重复

* 实际存在一份固定的属性即可,所以就要使用到**static 静态**,每个对象都会共享这个属性(这个属性只有一份)

```java
public class student{
    public String name;
    public String className;
    public static String schoolName="某大学";//也可在Test类中赋值(一次就好)

    public void study(){
        System.out.println(schoolName+"大学"+stuName+"正在上课");
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        student s=new student();
        s.name="张三";
        s.className="101";
        s.study();
        schoolName="某某大学";//如过值被修改的话,下边的所有值都会改变(因为所有对象都共享这一个属性)
        student s1=new student();
        s1.name="李四";
        s1.className="102";
        s1.study();
        //s.schoolName和s1.schoolName都是访问的同一片空间
}//out:某大学张三正在上课
 //    某某大学李四正在上课//(school这个静态属性值被修改了)
```

* 当把一个类的属性定义为static,那么把这个属性成为静态属性(没有使用static关键字属性定义的属性称为普通属性).
* 当一个方法定义为static,那么把这个方法成为静态方法(没有使用static关键字方法定义的方法称为普通方法).

* 静态和普通的属性和方法的区别:

  1. 静态的属性和方法属于类,普通的属性和方法属于对象;类和对象的关系:一个类可以拥有多个对象,对象隶属于类的实现;类只有一份,对象可以是多个;静态属于类,类只有一份,所以静态也只有一份,被所有对象共享,普通的属性和方法会产生多份,每创建一个对象就会开辟一片空间,那么对象和对象之间的属性和方法都是独立存在的,互不干扰,

  2. 初始化时机:普通的属性和方法是在创建对象的时候初始化,普通的属性和方法只有在创建完对象才会存在;所以普通属性和方法必须使用对象调用;静态的属性和方法是在类加载的时候初始化(Java文件-编译--加载--创建对象--运行),所以静态的属性和方法是在没创建完对象的时候(类加载的时候)就存在了.(静态的初始化时机比普通的早)

  3. 静态的属性和方法是在类加载的时候初始化,所以不用创建对象就存在,所以静态属性和方法通过类来调用,也可通过对象来调用(因为想要通过对象来调用时,对象肯定都已经创建完了,那么类肯定已经加载完了,所以静态属性一定存在),普通属性和方法在创建对象的时候才初始化,所以只能通过对象来调用,不能通过类来调用(初始化时机的问题)

#### 匿名静态代码块


```java
public class student{
    public int num;
    public String stuName;
    public student(){
        System.out.println("student的构造方法");
    }
    {
        System.out.println("student的匿名代码块2");
    }
    {
        System.out.println("student的匿名代码块1");
    }
    static{
        System.out.println("student的静态匿名代码块1");
    }
}
```
 执行顺序:

 ①静态匿名代码块
 		②匿名代码块
		 ③构造方法

* 静态匿名代码块只会执行一次(类加载的时候),匿名代码块在每创建一次对象,就会执行一次

### 3.this 关键字

**this是一个运行时语法,表示当前对象自己本身**

```java
public class student{
    public String name;
    public String className;
    public static String schoolName="某大学";

    public void showStudentName(){
        System.out.println(this.name);
    }
}
```

* 目前没有调用这个方法,这个方法只会被编译,不会被运行,所以这个地方的this是空指向
* **当创建完对象,这个方法被调用时,this表示的就是当前创建的对象**

```java
public class Test{
    public static void main(String[] args){
        student s=new student();
        student s1=new student();
        
        t.name="张三";
        t1.name="李四";
        
        t.showStudentName();//这个方法中的this表示t的name
        t1.showStudentName();//这个方法中的this表示t1的name
}
    //类似于现实中:我吃了  (这句话中的我,代表的是谁,那就要看是谁在说话)
    
```

```java
public class student{
    public String name;
    public String className;
    public static String schoolName="某大学";

    public void showStudentName(){
        System.out.println(this.name);
         System.out.println(name);//this.name和name表示的是同一个属性
    }
}
```

* 变量的作用域:定义当前变量的最近的外围的大括号

* Java使用static 关键字进行声明的变量叫做全局变量/属性/静态变量/类变量/类属性

* 全局变量(属性)的作用域:当前类的任意地方都能访问

* 局部变量只能在自己的作用域内被访问

* 成员变量定义在类中，在整个类中都可以被访问,随着对象的建立而建立，随着对象的消失而消失，存在于对象所在的堆内存中。

#### 成员变量和静态变量的区别:

1.两个变量的生命周期不同

- 成员变量随着对象的创建而存在，随着对象被回收而释放。
- 静态变量随着类的加载而存在，随着类的消失而消失。

2.调用方式不同

- 成员变量只能被对象调用。
- 静态变量可以被对象调用，还可以被类名调用。

3.别名不同

- 成员变量也称为实例变量。
- 静态变量也称为类变量。

4.数据存储位置不同

- 成员变量存储在堆内存的对象中，所以也叫对象的特有数据。

- 静态变量数据存储在方法区（共享数据区）的静态区，所以也叫对象的共享数据。

  ***

  

```java
public class student{
    public String name;
    public String className;
    public static String schoolName;//全局变量/属性/静态变量/类变量/类属性
    public void method(){
        int i=10;//局部变量,出了这个方法,这个变量就会失效
    }
    public void method1(){
        int i=10;//所以可以定义相同名称的变量
    }
}
```

**Java中允许局部变量和属性重名(在局部变量和属性重名时,在局部变量的作用域内,局部变量优先于属性)**

```java
public class student{
    public String name;
    public String className;
    public static String schoolName;
    public student(){
        
    }
    public student(String name,String className){
        name=name;
        className=className;
    }
    public String getInfo(){
        return "班级:"+className+"\t姓名:"+name+"正在学习"
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        student s=new student("张三","101");
        String str=s.getInfo();
        System.out.println(str);
        
}//out:班级:null	姓名:null正在学习
```

* 值并没有赋上

原因:局部变量的名称和属性一致时,这种情况下,在局部变量的作用域内,会隐藏属性,直接引用的变量是局部变量 name=name赋值符号前后都是同一变量(局部变量),在当前局部变量和属性重名的情况下,this就体现很大的作用

```java
public class student{
    public String name;
    public String className;
    public static String schoolName;
    public student(){
        
    }
    public student(String name,String className){
        this.name=name;//this指的是当前类对象
        this.className=className;
    }
    public String getInfo(){
        return "班级:"+className+"\t姓名:"+name+"正在学习"
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        student s=new student("张三","101");
        String str=s.getInfo();
        System.out.println(str);
}
  //out:班级:101	姓名:张三正在学习
```

* this是一个对象,属于对象级别.利用对象来调用,一定只能调用属性和方法,根据类创建对象之后,对象会拥有类中定义的属性和方法,而局部变量对象是不可能调用的(局部变量属于一个方法或者一个程序块)

* **可以利用this调用当前对象的属性和方法**
* 一个类中的方法可以直接调用当前类的其他方法.使用this(),构造方法也可以互相调用

```java
public class Manager{
    private int number;
    private String name;
    private String sex;
    private int age;
    public Manager(){
    }
    public Manager(int number){
        this();
        this.number=number;
    }//注意:this()只能在当前类的构造方法中这样调用当前类的构造方法
    //this()必须是当前构造方法的第一行有效代码
    //也就是说一个构造方法中只能调用一个构造方法
    public Manager(int number,String name){
        //this.number=number;
        this(number);//调用当前类中有一个int型参数的构造方法
        this.name=name;
    }
    public Manager(int number,String name,String sex){
        /*this.number=number;
        this.name=name;
        */
        this(number,name);//调用当前类中有一个int,String型参数的构造方法
        this.sex=sex;
    }
    public Manager(int number,String name,String sex,int age){
        /*this.number=number;
        this.name=name;
        this.sex=sex;
        */
        this(number,name,sex);//调用当前类中有一个int,String,String型参数的构造方法
        this.age=age;
    }
    public String getInfo(){
        return "编号"+number+"\t姓名:"+name+"\t性别:"+sex+"\t年龄"+age;
    }
}
```

```java
public class Manager{
    private int number;
    private String name;
    private String sex;
    private int age;
    public Manager(){
    }
    public Manager(int number){
        this.number=number;
    }
 public Manager(int number,String name){
        this.number=number;
		 this.name=name;
    }
public Manager(int number,String name,String sex){
		this.number=number;
        this.name=name;
 		this.sex=sex;
    }
public Manager(int number,String name,String sex,int age){
		this.number=number;
        this.name=name;
        this.sex=sex;
	    this.age=age;
    }
```



```java
public class Test{
    public static void main(String[] args){
        Manager s=new Manager(1,"张三","男",20);
        String str=s.getInfo();
        System.out.println(str);
    }
}
```

* **Java内部类:不推荐使用,但有一些特殊情况需要使用到内部类**
* **只要是类都具有构造方法**

```java
public class Outer{
    public String outerName;
    public class Inner{
        public int number;
        public void innerShow(){
            System.out.println(number);
        }
        public void showOuterInfo(){
            System.out.println(outerName);
            //内部类可以访问外部类的属性和方法
        }
        public void showOuterMethod(){
            outerShow();
            //内部类可以访问外部类的方法
        }
    }
    //但是外部类不能直接访问内部类的属性和方法
    public void outerShow(){
        System.out.println(outerName);
    }
}
```

* Outer是外部类,Inner是属于Outer的内部类

```java
public class Test{
    public static void main(String[] args){
        Outer outer=new Outer();
        outer.outerName="张三";
        outer.outerShow();
    }//创建外部类对象
}
```

```java
public class Test{
    public static void main(String[] args){
        Outer outer=new Outer();
        Outer.Inner inner=outer.new Inner();
        //Outer.Inner(内部类的定义)
        //inner对象变量 outer是外部类的对象  outer.new Inner();
        inner.number=100;
        //调用内部类的属性
        inner.innerShow();
        //调用内部类的方法
    }//创建内部类对象
    /*
    创建Outer类内部的Inner类的对象
    第一种方式:先创建外部类的对象,再利用外部类的对象创建内部类的对象
    */
}
```

```java
public class Test{
    public static void main(String[] args){
        Outer.Inner inner=new Outer().new Inner();
        inner.number=10000;
        inner.innerShow();
    }//创建内部类对象
}
```

```java
public class Outer{
    public String outerName;
    public class Inner{
        public int number;
        public void innerShow(){
            System.out.println(number);
        }
        public void showOuterInfo(){
            System.out.println(outerName);
            //内部类可以访问外部类的属性和方法
        }
        public void showOuterMethod(){
            outerShow();
            //内部类可以访问外部类的方法
        }
    }
    //但是外部类不能直接访问内部类的属性和方法
    //外部类可以在需要调用内部类的属性和方法的地方,先创建内部类的对象,再通过对象来调用属性和方法
    public void outerShow(){
        Inner inner=new Inner();
        inner.number=1000;
        System.out.println(inner.number);
    }
}
```

* **当内部类定义为static**

```java
public class Outer{
    public String outerName;
    //定义内部类为static
    public static class Inner{
        public int number;
        public void innerShow(){
            System.out.println(number);
        }
    }
    public void outerShow(){
        System.out.println(outerName);
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        Outer.Inner inner=new Outer.Inner();//不用再用                                                     //newInner()
        inner.number=10000;
        inner.innerShow();
    }
}
```

* **static修饰类时只能修饰内部类,不能定义普通类**

```java
public static class Demo{
    
}
//错误❌: 此处不允许使用修饰符static
```

```java
public class Outer{
    public String outerName;
    public void outerMethod(){
        //把类定义在方法内部,局部内部类
        //作用于就是当前函数,创建这个类的对象只能在这个方法中
        class MethodInner{
            public int number;
            public void showNumber(){
                System.out.println(number);
            }
        }
        MethodInner inner=new MethodInner();
        inner.number=10;
        inner.showNumber();
    }
}
```

创建这个类的对象只能在这个方法中

```java
public class Test{
    public static void main(String[] args){
        Outer outer=new Outer();
        outer.outMethod();
        //局部类的定义和使用
    }
}//out:10
```

### 4.private修饰符

(封装).private修饰的属性和方法就只能在当前类中使用(称为私有属性和方法,私有属性和方法可以保护数据的安全,私有方法,保护当前业务的安全)

* 使用private修饰构造方法

```java
public class Cookit{
    private String cookieName;
    private Cookit(){
        
    }
    public void showName(){
        System.out.println(cookitName);
    }
}
```

```java
public class Test{
    public static void main(String[] args){
      ❌:  Cookit cookit=new Cookit();
    }
}//错误❌:Cookit()在Cookit中是private控制访问的
```

* 当前有一个构造方法,但是被私有化了,当前这个类就不能再创建对象了
* 解决方法①:外部类不能访问,但是私有化的类的内部可以访问,私有化的类的内部定义一个方法,这个方法专门用来创建当前类的对象,然后把创建完的对象通过返回值返回给外部类

```java
public class Cookit{
    private String cookieName;
    private Cookit(){
        
    }
    public void showName(){
        System.out.println(cookitName);
    }
    //因为当前类不能创建对象,所以外部是不能调用普通方法的,所以要把这个方法定义为static的,因为static的方法不用创建对象就能调用
    public static Cookit getInstance(){
        return new Cookit();
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        Cookit cookit=Cookit.getInstance();
        //可直接调用Cookit类中的static方法
        cookit.cookitName="张三";
        cookit.showName();
        Cookit cookit1=Cookit.getInstance();
        cookit.cookitName="李四";
        cookit.showName();
        Cookit cookit2=Cookit.getInstance();
        cookit.cookitName="王五";
        cookit.showName();
    }
}
```

* 解决问题,但是每次调用getInstance()静态方法都会创建(new)一个新对象
* 那么怎么修改这个代码可以让 getInstance()调用 N 次都是返回的同一个对象

### 5.单例模式

 **单例模式:在当前工程运行过程中当前类只会被创建一个对象**

#### (1)饱汉机制(懒汉)

```java
public class Cookit{
    private Cookit(){
    }
    private static Cookit cookit=null;
    //static保证了cookit只有一份
    public static Cookit getInstance(){
        //static要能是这个方法被调用
        if(cookit==null)
            cookit=new Cookit();
        return cookit;
    }
    //饱汉机制:不是很急,啥时候用啥时候创建
    //其实通常了讲，就是资源的使用频率，高的我们在加载就实例化出来，低的就在使用的时候在实例化
}
```

#### (2)饿汉机制

```java
public class Cookit{
    private Cookit(){
    }
    //加载类的时候就会创建一个对象---单例模式
    private static Cookit cookit=new Cookit();
    //已经创建好对象
    public static Cookit getInstance(){
        return cookit;
    //每次直接返回静态的cookit属性(每次调用getInstance()方法只是返回加载时创建好的对象)
    }
    //饿汉机制:很急,要用的时候,已经做好了,可以直接用
}
```


```java
public class Test{
    
    public static void main(String[] args){
        
        Cookit c=Cookit.getInstance();
    }
    
}
```



```java
    public static Person person2=new Person();
//使用公开属性,可以是外部直接获取到Person的对象,不安全
//因为可能会被修改为null,就会出现空指针
```

