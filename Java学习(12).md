# Java学习(12)

[toc]

## 1.Java进程

进程:一个进程就是一个独立的应用程序,例如打开的word,记事本,在运行的过程中,系统会给进程分配独立的内存空间,分配的内存空间由当前进程独享,所有进程都是由操作系统维护,引入线程会提高处理速度

线程:是在一个的独立的进程中,开辟出来的一个独立的执行流程,把一个进程内的独立的一个执行流程称为线程

线程反映到程序中就是程序的执行流程,每个程序都有一个执行流程

**线程是cpu调度的基本单位，进程是资源分配的基本单位，学了操作系统后你会对这句话有更加深刻的认识**

例如:程序中的main函数,如果当前的程序需要执行,那么当前程序就必须存在一个main函数,(main函数是当前程序执行的入口),main函数就是当前程序的当前程序的执行流程,即是当前进程

一个能运行的应用程序至少存在一个线程

**main方法其实也是一个线程,在java中所以的线程都是同时启动的,至于什么时候,哪个先执行,完全看谁先得到CPU的资源.**

**在java中,每次程序运行至少启动2个线程.一个是main线程,一个是垃圾收集线程.因为每当使用Java命令执行一个类的时候,实际上都会启动一个JVM,每一个JVM实际就是在操作系统中启动了一个进程.**

```java
public class test{
    public static void main(String[] args){
        
        System.out.println(1);
        System.out.println(2);
        System.out.println(3);
        System.out.println(4);
        System.out.println(5);
        System.out.println(6);
        
    }
}//out:
1
2
3
4
5
6
```

从结果来看,程序是按照定义的顺序依次向下执行

```java
public class Test {
    public static void main(String[] args) {
        
		method1();
        method2();

    }

    public static void method1() {

        System.out.println("method1");
        Scanner scanner = new Scanner(System.in);
        System.out.println("in:");
        String name = scanner.next();
        System.out.println(name);

    }
    public static void method2(){

        System.out.println("method2");

    }
}
```

定义了两个方法,method1和method2,在main函数中调用这两个方法,一定是先执行method1(),再执行method2(),method1()如果没有执行完,是不会去执行method2()的.因为当前两个方法的调用都是在main函数中,main一个执行流程,就一定会按照定义的顺序执行

![image-20201024094200159](D:\wj\Documents\Java\image-20201024094200159.png)

method1()出现等待用户输入,此时整个执行流程的hi处于等待状态的,所以method2()将无法执行,会造成性能的浪费

所以Java中引入了多线程的概念,可以把method1()和method2()的执行利用多线程来执行,method1()放入线程1中执行,method2()放入线程2中执行,就需要在Java中开辟线程

### 1.Thread类

#### run()

当前线程启动后,会自动执行run()内的语句

**一个类继承了Thread类,这个类就是一个线程类,可以在这个类的run()中定义线程启动后执行的任务**

#### start()

启动线程

start()方法的调用后并不是立即执行多线程代码，而是使得该线程变为可运行态（Runnable），什么时候运行是由操作系统决定的。

```java
package Demo2.Thread.MyThread;

public class MyThread1 extends Thread{

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            System.out.println("MyThread"+i);
        }
    }
}

```

```java
package Demo2.Thread.MyThread;

public class Test {

    public static void main(String[] args) {

        MyThread1 thread1=new MyThread1();
        thread1.start();
        for (int i = 0; i < 10; i++) {
            System.out.println("main"+i);
        }

    }
}

```

![image-20201024100311076](D:\wj\Documents\Java\image-20201024100311076.png)

* 首先执行main函数,main函数的执行也是一个线程,然后创建线程的对象,并启动线程,一旦启动线程,当前的线程就和main这个线程一样,都是一个独立的执行流程

从结果可以看出start()和main函数中的for循环是交替循环的.thread1这个对象继承了Thread这个类之后就是一个线程对象,当调用线程的start()后,当前线程就会启动,并自动执行线程类内部的run()的内容

当前程序的执行,一定还是先执行main函数,创建线程的对象,接着启动线程,线程就会自动执行run(),这时thread1就会作为一个独立的执行流程,与main函数同时执行

  main()这个线程开辟玩这个线程后,如果没有执行的任务,就会处于等待状态,直到所有线程执行完后,线程自动结束,main()才会自动退出,整个JVM结束

#### 线程名称的定义

1. 可以在当前的线程内部自定义一个线程的名称,通过自定义的构造方法来给当前线程设置名称

![image-20201024105832020](D:\wj\Documents\Java\image-20201024105832020.png)

2. 可以使用线程默认的名称,Thread类默认会给当前的线程设置名称(Thread-0,Thread-1),通过**getName()**获取
3. 可以通过Thread类的构造函数来类设置线程的名称

![image-20201024111622459](D:\wj\Documents\Java\image-20201024111622459.png)

***

任何代码的执行都需要CPU来执行,CPU在同一时刻只能执行一个任务,windows是一个多任务的操作系统,所以引入了线程队列的概念,线程队列就是把当前工作的线程都储存起来,由操纵系统从队列中取出一个线程由CPU执行

例如存在三个线程同时启动,每个线程的执行都需要由CPU来执行,但是CPU在同一时刻只能执行一个任务,那么系统是如何调配CPU资源的?

CPU这个硬件是由操作系统管理的,操作系统会把CPU的时间分为一个个短小的时间段,即时间片(单位较小),从线程队列中取出一个线程分配给时间片,那么当前线程就能运行,不断的分配时间片,就会感觉是每个线程在同时执行,线程的执行首先遵循的是操作系统的调配

***

#### 线程的优先级-setPriority()

**线程的执行首先遵循操作系统的调配,然后再看当前线程设定的优先级**

**线程的优先级仍然无法保障线程的执行次序。只不过，优先级高的线程获取CPU资源的概率较大，优先级低的并非没机会执行。**

线程与线程之间存在执行的优先级,即可以设定每个线程的优先级,但是线程首先遵循的是操作系统的调配,如果操作系统调配两个线程的优先级是一致的,那么就遵循线程本身的优先级

Java中把线程的优先级设定为10级,即1~10级,10级最高

Thread类中默认定义了三个常量(三个优先级)

* MIN_PRIORITY=1

* MAX_PRIORITY=10

* NORM_PRIORITY=5

在线程的start方法调用之前,指定线程的优先级。

#### 线程的停止

##### 1.~~stop~~()

停止线程,过时的方法,不安全,不推荐使用

##### 2.isRunning

可以定义一个是否继续执行的变量,当前线程是否执行依赖于isRunning这个变量

```java
public boolean isRunning=true;

    public boolean isRunning(){
        return isRunning;
    }
//查询线程状态
    public void setRunning(boolean running){
        isRunning=running;
    }
//设置线程状态,运行,退出
    @Override
    public void run() {
        for (int i = 0,j=0; i < 10; i++) {
            if(isRunning) {
                System.out.println(getName()+"--"+i);
                System.out.println(isRunning());
                j++;
            }else{
                break;
            }
        }
    }
```

如果想停止线程,那么在main函数中通过setRunning(false),就可以使当前线程退出,较~~stop~~()安全

##### 3.interrupted()

测试当前线程是否已经中断,**线程的状态由该方法清除**

##### 3.1.interrupt()

其作用是中断此线程（此线程不一定是当前线程，而是指调用该方法的Thread实例所代表的线程），但实际上只是给线程设置一个中断标志，线程仍会继续运行。

**interrupt()不能中断在运行中的线程，它只能改变中断状态而已。**

##### 3.2.isInterrupted()

测试当前线程是否已经中断,**线程的状态不受该方法的影响**

#### 面试题

请简述Thread类中的interrupt(),interrupted(),isInterrupted()三个方法之间的区别?

interrput():其作用是中断此线程（此线程不一定是当前线程，而是指调用该方法的Thread实例所代表的线程），但实际上只是给线程设置一个中断标志，线程仍会继续运行。

interrputed():测试当前线程是否已经中断,**线程的状态由该方法清除**

isInterrupted():测试当前线程是否已经中断,**线程的状态不受该方法的影响**

**所谓的清除中断状态是指：恢复当前线程的中断状态的默认状态**

举例:

A线程在工作时,调用interrupted()和isInterrupted()都会返回false,当调用interrupt()后

1. 调用interrupted(),返回true,表示被中断了,然后interrupted()会把当前的中断状态修改为false,及恢复默认状态,**以后调用都会返回false**,false即是默认状态
2. 调用isInterrupted(),返回true,并不会修改其中断状态

#### sleep()

**sleep()是Thread类的静态方法**,用来休眠线程

sleep(暂停的**毫秒数**),暂停时间过后,会自动唤醒

sleep()会抛出异常:如果当前线程正处于休眠状态,被其他的线程打断,那么当前方法就会抛出异常,并把当前线程的状态清除,不能去中断一个正在休眠的线程,不能去中断一个正在休眠的线程

多线程中sleep()还有影响其线程执行的先后

sleep()调用目的是不让当前线程独自霸占该进程所获取的CPU资源，以留出一定时间给其他线程执行的机会。

#### yield()

当前线程调用yield的时候，告诉虚拟机它愿意让其他的线程抢占自己的位置。这表明该线程没有紧急的事要做，但这只是一种暗示，并不能保证一定会发生。

使用yield()的目的是让具有相同优先级的线程之间能够适当的轮换执行。但是，实际中无法保证yield()达到让步的目的，因为，让步的线程可能被线程调度程序再次选中。

使用yield方法时要注意的几点：

1.yield是一个静态的本地方法（native）

2.调用yield后，yield告诉当前线程把运行机会交给线程池中有相同优先级的线程。

3.yield不能保证，当前线程迅速从运行状态切换到就绪状态。

4.yield只能是将当前线程从运行状态转换到就绪状态，而不能是等待或者阻塞状态

#### sleep()和yield()的区别

 sleep()使当前线程进入停滞状态，所以执行sleep()的线程在指定的时间内肯定不会被执行；

yield()只是使当前线程重新回到可执行状态，所以执行yield()的线程有可能在进入到可执行状态后马上又被执行。
​    sleep 方法使当前运行中的线程睡眠一段时间，进入不可运行状态，这段时间的长短是由程序设定的

yield 方法使当前线程让出 CPU 占有权，但让出的时间是不可设定的。实际上，yield()方法对应了如下操作：先检测当前是否有相同优先级的线程处于同可运行状态，如有，则把 CPU 的占有权交给此线程，否则，继续运行原来的线程。所以yield()方法称为“退让”，它把运行机会让给了同等优先级的其他线程
​        另外，sleep 方法允许较低优先级的线程获得运行机会，但 yield() 方法执行时，当前线程仍处在可运行状态，所以，不可能让出较低优先级的线程些时获得 CPU 占有权。在一个运行系统中，如果较高优先级的线程没有调用 sleep 方法，又没有受到 I\O 阻塞，那么，较低优先级线程只能等待所有较高优先级的线程运行结束，才有机会运行。

#### suspend()

挂起一个线程.

如果线程处于活动状态则被挂起，且不再有进一步的活动，除非重新开始。

#### resume()

恢复挂起的线程.

如果线程处于活动状态但被挂起，则它会在执行过程中重新开始并允许继续活动。 

#### join()

使得线程之间的**并行执行变为串行执行**

把指定的线程加入到当前线程，可以将两个交替执行的线程合并为顺序执行的线程。

比如在线程B中调用了线程A的Join()方法，直到线程A执行完毕后，才会继续执行线程B。

如果是三个线程,在A中调用B,B会先于A执行完后再执行A,但这并不影响C线程

如果任何线程中断了当前线程。当抛出该异常时，当前线程的中断状态 被清除。

**join()方法必须在线程start()方法调用之后调用才有意义**。

#### 守护线程

**守护线程是JVM在运行的时候默认开启的线程**

例如:

垃圾回收器线程就是一个典型守护线程.守护线程一般是需要等待所有的非守护线程执行完毕后才会退出

如果当前的JVM内部存在的都是守护线程,JVM就会退出,JVM不会等待守护线程执行完毕,只要当前JVM内所有的用户线程执行完毕,JVM就会退出

#### 非守护线程

就是使用Java多线程技术开启的线程,即继承Thread类,调用线程的start()开启的线程.**手动开启的都是非守护线程**

但是手动开启的线程可以在当前线程开启前,通过**setDaemon(true)**,可以把当前线程设置为守护线程

可以通过isDaemon()测试当前线程是否为一个守护线程

#### 线程的生命周期的状态

1. 初始状态(就绪状态):当一个线程的对象被创建,但没有调用start()启动前,为就绪状态
2. 可运行状态:当一个线程被调用了start()后,进入可运行状态.但是一个线程用了start()以后,并不一定就开始执行,需要等待CPU时间片.所以当调用了一个线程的start()以后,当前线程就会进入到线程队列中
3. 运行状态:当一个线程被调用start(),并获取了CPU的执行的时间片
4. 阻塞休眠状态:当前一个线程处于任务执行中,但是在等待某种资源

例如:在线程执行的内部需要获取用户输入的数据,那么在用户输入数据之前,当前线程是处于等待状态,或者被休眠,挂起等,当前线程没有执行完毕也没有执行处于等待状态,称为阻塞状态

5. 死亡状态:一个线程被调用stop()或者线程执行完毕,都处于死亡状态

其中第四种状态可以装换到第二种状态,第四种状态不能直接转换到第三种状态

![img](https://img-blog.csdn.net/20180418230837516?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDI3MTgzOA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

***

### 2.Rununable接口

多线程可以通过继承Thread类,也可以实现Runnable接口

**Java中线程存在了一个Thread类,为什么还要定义一个Runnable接口?**

答:例如有些类已经继承了某个类,但是还需要实现线程的功能,那么这个时候就可以使用Runnable接口,**因为Java是单继承,多实现**

#### Runnable接口源码:

![image-20201025074823464](D:\wj\Documents\Java\image-20201025074823464.png)

* 同样,Runnable接口中也定义了一个run(),所以实现Runnable接口,就必须实现run()

#### 启动线程

**实际上所有的多线程代码都是通过运行Thread的start()方法来运行的**

Runnable接口比没有继承其他接口,那么Runable就无法使用定义在Thread类中的start()来开启线程

##### Thread类中构造方法

​	![image-20201025075629310](D:\wj\Documents\Java\image-20201025075629310.png)

Thread类中存在一个构造方法,会自动把传递进来的Runnable接口对象当成Thread的对象

事实上，当传入一个Runnable target参数给Thread后，Thread的run()方法就会调用target.run()

***

### 3.Thread和Runnable的区别

如果一个类继承Thread，则不适合资源共享。但是如果实现了Runable接口的话，则很容易的实现资源共享。

**实现Runnable接口比继承Thread类所具有的优势：**

**1）：适合多个相同的程序代码的线程去处理同一个资源**

**2）：可以避免java中的单继承的限制**

**3）：增加程序的健壮性，代码可以被多个线程共享，代码和数据独立**

**4）：线程池只能放入实现Runable或callable类线程，不能直接放入继承Thread的类**

***

### 4.Callable接口

**返回值的任务必须实现Callable接口，类似的，无返回值的任务必须Runnable接口**

Java中实现多线程任务是不能存在返回值的,因为现成的任务是执行run(),而run()定义为void,所以不能通过独立的线程来计算数据

例如:

需要开辟一个线程计算1~100的和,利用Thread类和Runnable接口,都无法获取线程内部的数据,所以Java中又定义了一个接口**Callable接口**

#### call()

Callable接口中定义了call(),类似于Thread类和Runnable接口中的run(),call()存在返回值,返回值是利用了泛型,即实现Callable接口时候定义的是什么类型,那么call()就返回什么类型

#### Callable接口源码:

![image-20201025090859299](D:\wj\Documents\Java\image-20201025090859299.png)

### 5.Future接口

启动Callable接口,必须使用Future接口,Future 本身也是一种设计模式，它是用来取得异步任务的结果(异步是指不是同一执行流程,也就是线程)

通过返回的Future对象查询执行状态

#### isDone()

判断异步计算的方法是否执行完成,以等待计算完成,并通过**get()**获取计算结果,如有必要,在计算完成前可以通过**cancel()**阻塞此方法,取消get()的执行

Future接口还提供了其他方法以确定任务是否正常完成还是被取消.一旦计算完成,就不能取消计算.

如果为了可取消性而使用Future接口,则可以声名为Future<?>形式的类型,并返回null作为底层任务的结果

#### FutureTask类

实现了Runnable接口,FutureTask就是Runnnable的子类,所以利用**Thread(Runnable targe)**的这个形式的构造方法,用thread对象来启动Callable接口的线程

可使用 **FutureTask**包装 **Callable** 或  **Runnable** 对象

FutureTask()就是一个异步的任务的状态  对象  ,或者说是一个异步任务的  控制对象,构造方法传递的是Callable()的对象,那么当前FutureTask()的对象就监控的是那个实现了Callable接口的类的对象

FutureTask()可以监控异步的任务是否执行完成,还可以取消这个异步任务,获取这个异步任务的结果

**FutureTask()的  对象  管理** **实现了Callable接口的类的**  **对象**

#### 通过Callable和FutureTask创建线程

a:创建Callable接口的实现类 ，并实现Call方法
		b:创建Callable实现类的实现，使用FutureTask类包装Callable对象，该	FutureTask对象封装了Callable对象的Call方法的返回值
		c:使用FutureTask对象作为Thread对象的target创建并启动线程
		d:调用FutureTask对象的get()来获取子线程执行结束的返回值

```java
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

public class Test {

    public static void main(String[] args) throws ExecutionException, InterruptedException {

		MyThread1 myThread1=new MyThread1();
        FutureTask<Integer> futureTask=new 			   				FutureTask<Integer>(myThread1);
        Thread thread=new Thread(futureTask);
        thread.start();
        System.out.println(futureTask.get());
    }
}
```

```java
import java.util.concurrent.Callable;

public class MyThread1 implements Callable<Integer> {

    @Override
    public Integer call() throws Exception {
        int result=0;
        for (int i = 0; i <= 100; i++) {
            result+=i;
        }
        return result;
    }
}
```

1. MyThread1 myThread1=new MyThread1();

* MyThread1实现了Callable接口,**定义了当前的任务的内容**

2. FutureTask<Integer> futureTask=new FutureTask<Integer>(myThread1);

* FutureTask类实现了Future接口,Runnable接口,又把MyThread类的对象传递到当前FutureTask类的对象中,是**监控管理Callable任务的控制接口**

3. Thread thread=new Thread(futureTask);

* 创建一个线程,利用线程来执行futureTask这个对象管理的任务,即myThread1的call(),当线程启动,就会**使用线程来执行实现了Callable接口的类的对象中定义的call()**,线程开始执行时,futureTask就一直监控当前任务的执行,所有的状态就会发送到futureTask这个对象上

### 6.实现线程的方式

1. 继承Thread类
2. 实现Runnable接口
3. 实现Callable接口,可以存在返回值

```java
System.out.println("a");
System.out.println("b");
System.out.println("c");
```

在同一流程中,即同一线程,输出顺序为a,b,c

如果是在三个线程中:哪个线程先抢站到执行权,就先输出那个,这就是异步执行.

多线程的情况下,代码的执行都是异步的.,没有引入多线程的情况下,main函数的执行就是一个执行流程(一个线程中执行代码为同步执行)

在多线程执行的情况下,同一时刻只会存在一个线程执行,线程同步.

### 7.Synchronized

Java中线程同步使用Synchronized关键字标识,Synchronized可以定义同步块,同步块可以自由地包含指定代码,被包含的代码在多线程执行的情况下,必须先获取线程锁对象,才能执行同步块内部的代码,没有获得线程锁对象的线程只能处于等待状态

```java
 Synchronized("abc"){
     代码;
 }
```

Synchronized也可以标识方法,称为同步方法,当前方法是线程安全的,同步方法的线程锁对象是当前同步方法的对象(this)

```java
public Synchronized void print() throws InterruptedException{
    System.out.println("");
}
```

### 8.监视器

监视器就是锁,即线程锁对象,这个理解是针对于Synchronized关键字的锁,后期的Lock的引入把线程锁对象和监视器对象分割开来

多个同步块使用同一个线程锁对象,实现多个同步块同步,多个同步块的锁一定要是同一个线程锁对象才能实现

### 7.wait()

可以让线程处于等待状态

wait(time):可以指定等待时间

wait()一旦休眠,那么在休眠之前会释放锁,当前所就会公开出来由其他线程来抢,那么在其他线程抢到锁之后在没有释放锁之前,其他线程都处于等待状态.其中包括wait()之后被唤醒的线程(不管是wait()时间到,还是被notify()唤醒)

### 8.sleep()与wait()的区别

1. sleep()是定义在Thread类中的静态方法;wait()是定义在Object类中的方法
2. sleep()休眠时间到达后会自动唤醒执行;wait()除了指定休眠时间到达后自动唤醒执行,还可以调用notify(),notifyAll()来唤醒
3. sleep()休眠时间的时候不释放锁;wait()休眠的时候会释放锁
4. sleep()是静态方法,可以在任何地方发调用是当前线程休眠;wait()必须在同步方法或者同步块中,使用线程锁对象来调用

### 9.notify()与notifyAll()的区别

notify()是随机唤醒一个线程,notifyAll()是唤醒所有正在等待的线程

***

线程锁对象可以是任何对象,那么反过来说任何对象都可能是线程锁对象

wait(),notify(),notifyAll()都必须通过线程锁对象调用,所以Java将这三个方法都定义在了Object类中

### 10.Lock接口

是一个线程锁的接口,也提供了synchronized的功能,保证了线程的同步,

实现了Lock接口的类也是一个线程锁,锁是存在状态的,线程锁的状态就是Condition接口的对象

通过lock()来获取锁,最后一定要手动通过unlock()来释放锁,

### 11.Condition接口

Condition接口封装了Lock对象的await(),notify(),notifyAll(),通过锁对象可以获得当前锁对象的监视状态的对象

### 12.synchronized和Lock的区别

1. synchronized是java内置的一个关键字;Lock是一个接口
2. synchronized加锁,释放锁是JDK实现的,Lock需要手动的加锁,释放锁
3. synchronized关键字修饰的代码在遇到异常的时候,JDK会自动释放线程占有的锁,不需要手动去释放锁,因此不会出现死锁的情况;Lock在发生异常的时候,如果没有在catch块或者finally块中调用unlock(),那么Lock是不会释放锁的,所以Lock在发生异常的时候有可能会出现死锁的情况
4. lock等待锁过程中可以用interrupt来中断等待，而synchronized只能等待锁的释放，不能响应中断；
5. synchronized是一种非公平锁,Lock默认为非公平锁,但可以手动设置为公平锁或者非公平锁

**公平锁:是指多线程在启动后,如果当前线程都是公平锁,那么会按照顺序去获取锁的对象,也就是每个线程获取锁的次数是公平的;如果是非线程锁,每个线程都会去抢锁对象,有可能一个线程多次抢占到锁,其他线程一次都抢占不到**

`ReentrantLock`对象的构造方法提供创建的锁是否为公平锁,默认为非公平锁

6. Lock锁只能用于代码块,sychronized可以用于代码块,方法较Lock适用范围更广
7. Lock可以设置唤醒的线程,synchronzied不能设置缓存的是哪个线程,要么随即唤醒一个,要么唤醒全部?

### 13.线程锁机制

## 2.实现生产者与消费者模式

利用线程锁对象,暂停不同的业务,唤醒不同的业务

