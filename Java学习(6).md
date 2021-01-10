# Java学习(6)

 [toc]

```java
public class Student{
    public String name;
    public String sex;
    public int age;
    public void study(){
        System.out.println(name+"正在学习");
    }
}
```

```java
public class Teacher{
    public String name;
    public String sex;
    public int age;
    public void work(){
        System.out.println(name+"正在工作");
    }
}
```

* 发现这两个类出现相同的属性,但又是两个不同的业务模型,所以分别定义在两个类中,因为类要保持单一原则,就是一个类或者一个函数只能描述一种实物或者一个业务.两个相同的属性定义在量的不同的类中,Student中的name,sex,age是描述Student的特征,Teacher中的name,sex,age是描述Teacher的特征,向上抽象,找到人的抽象级别比Student和Teacher都要高,定义一个Person类与Student类以及Teacher类存在一种继承关系,类的继承是一种类别的继承

```java
public class Person{
    public String name;
    public String sex;
    public int age;
    Person(){
        System.out.println("person的无参构造器");
    }
    public void eat(){
        System.out.println(name+"正在吃饭");
    }
}
```

```java
public class Student extends Person{
    public void study(){
        System.out.println(name+"正在学习");
    }
}
```

```java
public class Teacher extends Person{
    public void work(){
        System.out.println(name+"正在工作");
    }
}
```

* 把Student和Teacher中出现的相同属性和方法都可以定义在Person类中,那么通过继承,Student和Teacher可以继承Person中的属性和方法

* **继承:Java中继承采用的是关键字 extends来描述继承关系**

* extends 左边为子类,右边为父类

* 子类默认就拥有父类中定义的属性和方法

```java
public class Test{
    public static void main(String[] args){
        Student s=new Student();
        s.name="张三";
        s.study();
        Teacher t=new Teacher();
        t.name="张三丰";
        t.work();
    }
}
```

* **一个类只能继承一个父类,Java采用的是单继承,继承可以被传递**
* **创建子类对象的时候会先调用父类的构造方法,早依次往下调用子类的构造器**
* **但子类不能访问(继承)父类中定义的private属性和方法,利用对象调用也是不允许的**

```java
public class Person{
    public String name;
    public String sex;
    public int age;
    private int number;//子类是不能访问的
    Person(){
        System.out.println("person的无参构造器");
    }
    public void eat(){
        System.out.println(name+"正在吃饭");
    }
}
```

***


```java
public class Person{
    public String name;
    public String sex;
    public int age;
    Person(String name,String sex,int age){
        //有参构造器
        this.name=name;
        this.sex=sex;
        this.age=age;
    }
    public void eat(){
        System.out.println(name+"正在吃饭");
    }
}
```

* 当子类再次调用时会报错
* 原因是:子类创建对象时会自动调用父类的的构造器(默认是无参构造器)

* 解决问题:从子类出发:指定调用父类存在的其他构造方法

#### super()

**在子类构造方法中指定调用父类的指定构造方法,使用super关键字**

**super()与this()区别:super()可以调用父类的构造方法,this()是调用当前类自己的构造方法;**

**super()这种形式也必须在子类的构造方法中出现,而且也必须是子类构造方法中的第一行;**

**super()调用父类无参数的构造器,super(String,String,int)这种就是调用父类指定参数的构造方法**

```java
public class Person{
    public String name;
    public String sex;
    public int age;
    public Person(){
        
    }
    public Peron(String name,String sex,int age){
        this.name=name;
        this.sex=sex;
        this.age=age;
    }//被Student子类调用
    public void eat(){
        System.out.println(name+"正在吃饭):
    }
}
```

```java
public class Student extends Person{
    public Student(){
        super("张三","男",22);
        //Student类中调用父类具有相同参数个数,相同类型的构造器
    }
    public void study(){
        System.out.println(name+"正在学习");
    }
}
```

```java
public class Teacher{
    public Teacher(){
        super("李四","男",24);
    }
    public void work(){
        System.out.println(name+"正在工作");
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        Teacher t=new Teacher();
        t.work();
    }
}
```

***

```java
public class Person{
    public String name;
    public String sex;
    public int age;
    public Person(){
        
    }
    public Person(String name,String sex,int age){
        this.name=name;
        this.sex=sex;
        this.age=age;
    }
    public void eat(){
        System.out.println(name+"正在吃饭");
    }
}
```

```java
public class Student extends Person{
    public Student(String name,String sex,int age){
        super(name,sex,age);
    }
    public void study(){
        System.out.println(name+"正在学习");//name
    }
}
```

* Person类中定义了属性name,Student继承Person类,那么默认情况下Student从Person中继承了name属性,也就是说Student不用定义name属性,但是Student中有显式定义了name属性,Java继承中是可以存在这种冲突的
* 创建完Student对象的时候实参先传递到Student类中,利用super(name,sex,age)又传递到Person中,在Person中完成初始化工作,Person中的name中储存的是张无忌,Student继承了Person,那么Person中的name会继承到Student类中,这时候在Student中使用this.name还是那个name(Person中的name和Student中的name是同一个)

```java
public class Student extends Person{
    public Student(String name,String sex,int age){
        super(name,sex,age);
    }
    public void study(){
        System.out.println(this.name+"正在学习");//this.name
    }
}
```

```java
public class Student extends Person{
    public Student(String name,String sex,int age){
        super(name,sex,age);
    }
    public void study(){
        System.out.println(super.name+"正在学习");//super.name
    }
}
```

* Person类不变,Student()中name修改为this.name再到super.name,打印结果相同,说明name,this.name,super.name是一致的

![image-20200720085621464](D:\wj\Documents\Java\image-20200720085621464.png)



```java
public class Test{
    public static void main(String[] args){
        Student s=new Student();
        Teacher t=new Teacher();
   }
}
```

* 运行时分别创建Student和Teacher的对象,Person创建两次,顺序为
* Person构造器              Student构造器
* Person构造器              Teacher构造器

```java
public class Person{
    public String name;
    public String sex;
    public int age;
    
}
```

```java
public class Teacher{
    public Teacher(String name,String sex,int age){
        this.name=name;
        this.sex=sex;
        this.age=age;
    }
    public void work(){
        System.out.println(name+"正在工作");
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        Teacher t=new Teacher();
        t.work();
    }
}
```

* 初始化放置在了子类中
* 子类的name属性都是从Person继承来的,不管子类还是父类的初始化,都是父类空间中的name,因为子类中没有定义name属性,都是继承的父类的属性
* 但是父类的private属性和方法是不被继承的

```java
public class Person{
    public String name;
    public String sex;
    public int age;
}
```

```java
public class Student{
    public String name;//父类中也有一个name属性
    public Student(String name,String sex,int age){
        this.name=name;
        this.sex=sex;
        this.age=age;
    }
    public void study(){
        System.out.println("name"+name);
        System.out.println("this.name"+this.name);
        System.out.println("super.name"+super.name);
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        Student s=new Student("张三","男",22);
        s.study();
    }
}//out:
//name张三
//this.name张三
//super.namenull
```

* Person类中存在一个name属性,子类Student中也显式的定义了一个name,**name要从内向外找,找最近定义name就是他,this.name就是student中定义的name,如果使用super.name,那么就一定是使用父类中的name**

#### 上下转型

父子对象之间的转换分为了**向上转型**和**向下转型**,它们区别如下:

- **向上转型** : 通过子类对象(小范围)实例化父类对象(大范围),这种属于自动转换

- **向下转型** : 通过父类对象(大范围)实例化子类对象(小范围),这种属于强制转换

多态:同一个行为具有多个不同表现形式或形态的能力。比如说，有一杯水，我不知道它是温的、冰的还是烫的，但是我一摸我就知道了。我摸水杯这个动作，对于不同温度的水，就会得到不同的结果。这就是多态。

多态应用:

参数化多态

返回值多态

##### ①.向上转型

把子类的空间转换成父类的对象

向上转型，换言之，**就是用父类的引用变量去引用子类的实例**,**当向上转型之后，父类引用变量可以访问子类中属于父类的属性和方法，但是不能访问子类独有的属性和方法。**

向上转型后父类的引用所指向的属性是父类的属性，如果子类重写了父类的方法，那么父类引用指向的或者调用的方法是子类的方法,这个叫动态绑定。

```java
public class Person{
    public String personName;
    public String personSex;
    public int personAge;
    
    public void show(){
        System.out.println("姓名:"+personName+"\t性别:"+personSex+"\t年龄:"+personAge);
    }
    
}
```

```java
public class Student extends Person{
    public int stuId;
    public void study(){
        System.out.println("Student study()");
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        Student s = new Student();
        Person p = s;//上层指向下层
        /*
        向上转型
        把s这个对象转换成Peron
        s和p都是指向同一空间(new Person())
        */
    }
}
```

向上转型时会遗失除与父类对象共有的其他方法

向上转型的作用，减少重复代码，父类为参数，调有时用子类作为参数，就是利用了向上转型。这样使代码变得简洁。体现了JAVA的抽象编程思想。

1. 父类有方法，子类有覆盖方法,执行子类方法。

2. 父类有方法，子类没覆盖方法,执行父类方法（子类继承）。

3. 父类没方法，子类有方法：编译失败，无法执行。

##### ②.向下转型

向下转型存在不安全性的问题,一般不推荐使用向下转型,**并不是所有的对象都可以向下转型，只有当这个对象原本就是子类对象通过向上转型得到的时候才能够成功转型。**

- 向下转型的前提是父类对象指向的是子类对象（也就是说，在向下转型之前，它得先向上转型）
- 向下转型只能转型为本类对象

```java
public class Test{
    public static void main(String[] args){
        Person p = new Student();
        Student s = (Student)p;//下层指向上层,上层需要强制类型转换为下层,会出现不安全性
        /*
        向下转型
        s和p都是指向同一空间(new Person())
        */
    }
}
```

向上转型时会遗失除与父类对象共有的其他方法；可以用向下转型再重新转回，这个和向上转型的作用要结合理解。

人有男人女人，向下转型的话人不一定只是男人，向上转型的话男人是人。

### 2.多态在异常体系中的应用

一个try块可以携带多个catch块

```java
public class Demo{
public static void main(String[] args){

    Scanner scanner=new Scanner(System.in);
    int result=0;
    try{
        int num=scanner.nextInt();
        result=10/num;
    }catch(InputMismatchException e){
        System.out.println("输入错误,请输入正确的数字");
    }catch(AirthmeticException e){
        System.out.println("除数不能为零");
    }
    System.out.println("result:"+result);
}
}
```

```java
public class Demo{
public static void main(String[] args){

    Scanner scanner=new Scanner(System.in);
    int result=0;
    try{
        int num=scanner.nextInt();
        result=10/num;
    }catch(Exception e){
        System.out.println("输入错误,请输入正确的数字");
    }
    System.out.println("result:"+result);
}
}
```

* 定义的异常的类型是Exception类型,按照异常的继承体系链,所有的异常都是继承自Exception类,所以不管try块中发生什么异常,当前这个catch都可以捕获当前异常,现在这个catch块的e对象可以指向任何异常空间

```java
public int calc(int i,int j)throws Exception{
    if(j==0){
        throw new AirthmeticException();
    }
    int result=i/j;
    return result;
}
```

* 在方法的内部实际抛出的是数学异常,但是声名的是异常,比实际抛出的异常类型要大(从继承链来看)



### 3.方法的重写

**方法的重载:发生在一个类中,存在两个或两个以上的具有相同方法名称,不同参数列表(参数的个数以及参数的类型)的方法,称为方法重载**

方法的重写:发生在多个类中,并且这多个类是具有继承关系,子类如果存在和父类中方法的定义一致的情况下(方法的定义包含:返回值,方法名,参数列表都一致),叫做子类重写了父类的方法,称为方法重写(Person中定义的有name属性,Student中也定义有name属性,这种就可以算作属性的重写(但没有这种称呼))

```java
public class Person{
    public String name;
    public String sex;
    public int age;
    public void work(){
        System.out.println("Person中work");
    }
}
```

```java
public class Student extends{
    public int stuId;
    public void showInfo(){
        System.out.println("编号:"+stuId+"\t姓名:"+name+"\t性别"+sex+"\t年龄"+age);
    }
    public void work(){
        System.out.println("Student中的work");
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        Student s=new Student();
        s.work();
    }
}//out:Student中的work
```

* 创建了Student的对象调用work()时,发现调用的是Student类中定义的work(),Student类中定义了一个和Person(父类)中一样的方法work();

```java
public class Person{
    public String name;
    public String sex;
    public int age;
    public void work(){
        System.out.println("Person中work");
    }
    public void method(){
        this.work();
        super.work();
    }
}
```

```java
public class Student extends{
    public int stuId;
    public void showInfo(){
        System.out.println("编号:"+stuId+"\t姓名:"+name+"\t性别"+sex+"\t年龄"+age);
    }
    public void work(){
        System.out.println("Student中的work");
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        Student s=new Student();
        //s.work();
        s.method();
    }
}//out:Student中的work
   //  Person中work
```

* this.work()是Student类中的方法,super.work()是Person(父类)的方法

```java
public class Student extends{
    public int stuId;
    public void showInfo(){
        System.out.println("编号:"+stuId+"\t姓名:"+name+"\t性别"+sex+"\t年龄"+age);
    }
        public void method(){
        this.work();
            work();
        super.work();
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        Student s=new Student();
        s.method();
    }
}//out:Person中work
   //  Person中work
   //  Person中work
```

* Studentleizhongmeiyoudingyiwork()方法,所以work(),this.work(),super.work()都代表父类中的work()方法

```java
public class Person{
    public String name;
    public String sex;
    public int age;
    public void work(){
        System.out.println("Person中work");
    }
}
```

```java
public class Student extends{
    public int stuId;
    public void showInfo(){
        System.out.println("编号:"+stuId+"\t姓名:"+name+"\t性别"+sex+"\t年龄"+age);
    }
    public void work(){//重写work();
        System.out.println("Student中work");
    }
}
```

```java
public class Teacher extends{//没有重写work();
    public int teacherId;
    public void showInfo(){
        System.out.println("编号:"+teacherId+"\t姓名:"+name+"\t性别"+sex+"\t年龄"+age);
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        Student s=new Student();
        s.work();
        //因为new的是Student的对象,所以work()是调用子类重写的work()
        Person p=new Person();
        p.work();
        //因为new的是Person的对象,所以work()是直接调用父类原始的work()
        Teacher t=new Teacher();
        t.work();
         //因为Teacher类中没有重写work(),所以work()是调用父类的work()
    }
}
```

* 一个类储存的属性和方法:是当前类自己定义的属性和方法以及整个继承链继承下来的属性和方法,当对象调用属性和方法时,现在当前空间中查找,有直接就用,没有则向上在父类的空间中寻找
* **子类重写父类的方法,子类访问修饰符大于等于父类的访问修饰符**
* 访问修饰符由大到小:public→protected(跨包子类可以访问)→默认→private 

