# java学习(11)

[toc]



## 1.Java集合

1.Java数组可以一次性申请相同数据类型的多变量空间

```java
int[] nums = new int[10];
```

* 一次性开辟连续的可以储存10个int变量的空间,所以数组的访问效率是比较高的(只需移动下标访问数据(指针)),但是数组的操作效率是比较低的,每次的增加,删除,都需要改动其他元素的位置,如果说当前数组的元素个数较多的话,那么操作当前数组的效率就会成倍下降
* 数组的缺点:
  1. 需要提前确定数组的类型,那么数组一旦确定了类型,当前数组就只能储存和数组类型一致的元素
  2. 数组在定义的时候需要提前确定数组的长度,java虚拟机会按照指定的长度来开辟数组的空间,数组的长度一旦确定,那么当前数组的大小不可改变,可能会造成空间的浪费和空间大小不够的问题

* Java引入了集合的体系:
  1. Java集合兼容了数组的功能,也就是说Java集合可以储存数据
  2. Java集合中不同的集合类,不同的地方是在于集合类和集合类之间内部使用的数据结构和算法的不同,所以使用Java集合的时候,需要根据当前的实际业务的特性来确定使用什么集合类,这样就能实现性能的高效

* Java集合体系结构:

  Collection(收集,聚集):

  **接口,包含在java.util包中,collection是一个集合的根接口**,Java中的集合类大部分都是实现这个collection这个接口,有点类似于Java异常体系中的Exception这个类,表示一组元素(对象),有些Collection的实现类是可以重复的,有些类是不可重复的,有些Collection的实现类的元素是有序的,有些实现类是无序的,Java所有的集合类都没有直接实现

## 2.Collection接口

collection接口是Java集合中的一个根集合,那么collection接口中定义的方法,所有实现当前collection接口的的实现类都必须实现他的方法,Collection接口有两个子接口(Set,List)

### 1.Collection中的方法

Add(元素):向集合中添加元素

addAll(Collection c):参数是一个集合,一个Collection接口其实际是一个集合,实参传递的是对象,接口是不能创建对象的,所以参数一定创建了实现类的对象,并把对象传递进来,可以把参数的集合内部元素全部添加到当前集合

clear():清空当前集合的所有元素

contains(Object o):参数是一个对象,判断当前集合中是否存在参数指定的对象,返回的结果是一个boolean类型的值

containsAll(Collection c):参数是一个集合,判断参数集合中的元素是否都包括在当前的集合中,返回boolean类型的值**(注意:参数集合的元素必须全部被包括在当前集合中)**

equals(Object o):判断当前集合是否和参数对象相等

isEmpty():判断当前集合内部是否为空,即判断当前集合是否存在元素,返回boolean值,当前集合中没有任何元素,则返回true

interator():返回当前集合的迭代器

remove(Object o):删除当前集合中和指定对象相等的元素

removeAll(Collection c):删除当前集合中参数集合中存在的所有元素

retainAll(Collection c):保留参数集合中存在的元素(注:例如参数集合中有Integer 1,2,3,8三个元素,当前集合中存在Integer1,2,3,4,5,6,7七个元素,当前这个方法调用后会删除4,5,6,7这四个元素,保留1,2,3,这三个元素)

size():返回当前集合中元素的个数

toArray():可以把当前集合转换为数组形式

**Collection没有直接的实现类,但是存在两个直接的子接口:list和set**

### 2.List接口

是一个接口,包含在java.util包中,List是一个有序的Collection集合**List都是有序的**,此接口的用户可以对列表中的每个元素的插入位置进行精确的控制(即支持下标),用户可以根据元素的整数索引(下标访问)访问元素,并搜索列表中的元素

**List的实现类都是允许元素重复的**,即对于元素e1和e2满足e1.equals(e2),并且列表本身允许null元素的,通常允许多个null元素

#### ArrayList

ArrayList是一个类,包含在java.util包中,用来实现list接口(是list的实现类)

是list接口的大小可变的数组实现,即:

1. List接口的大小可以改变,自动根据存入的元素来动态的扩展内部的空间

2. **List是一个数组实现,也就是说**

   **Array List的内部是一个数组,**

   **即支持下标操作,访问效率高,操作效率低**

* ArrayList是可以存在重复元素的

```java
ArrayList list = new ArrrayList();
list.add("a");
list.add("b");
list.add("b");
list.add("c");
```

```java
ArrayList list = new ArrayList();
list.add("a");
list.add("b");
list.add("c");
//ArrayList的Add方法参数默认是Object类型的,按照语法来说是可以储存任意类型的对象作为元素,但是存在不安全性,所以推荐一个ArrayList内部储存的是同一个类型
ArrayList list1 = new ArrayList();
list1.add("c");
list1.add("d");
```
##### 1.containAll()

```java
System.out.println(list.containAll(list1));
//out:false
//list内部只包含了list1元素的一部分
```
* 判断参数集合中的元素是否都包括在当前的集合中

##### 2.isEmpty()

```java
System.out.println(list.isEmpty(list1));
//out:false
//list1中存在元素
```
* 集合是否为空

##### 3.clear()

```java
list.clear();
System.out.println(list.isEmpty(list1));
//out:true
```
* 清除当前集合中的所有元素

##### 4.indexOf()

```java
int index = list.indexOf("b");
System.out.println(index);
```

* 查找指定元素在当前集合中的位置,从零开始.存在,则返回元素的位置,存在多个元素,默认查找的是第一个找到的元素,若不存在则返回-1

##### 5.lastindexOf()

```java
int index = list.lastIndexOf("b");
System.out.println(index);
```

* 查找指定元素最后一次出现的位置

##### 6.remove()

```java
list.remove("b");
```

* 删除指定元素,如存在重复元素,则只会删除第一个查找到的指定元素

##### 7.subList()

```java
List list2 = list.subList(1,3);
```

* 截取当前集合中的一部分组成一个新的集合

##### 泛型

* 创建了一个ArrayList集合对象,默认情况下当前的ArrayList对象内部的元素类型时Object,那么使用add()是可以添加任何类型的元素,但是这就会产生不安全因素,要避免这种情况,需要在语法上规定   Java为了集合的安全性,引入了泛型的概念,利用泛型会有效地避免集合的不安全性

```java
public static void main(String[] args){
    ArrayList list = new ArrayList();
    
    ArrayList<Integer> list1 = new ArrayList<>();
    list1.add(new Integer(10));
    ❌:list1.add(new User());
    ❌:list1.add("a");
    
}
```

* 创建集合的时候可以利用泛型来规定集合的类型,一旦集合规定了泛型的类型,那么当前集合就必须添加规定类型的元素,如果元素不是规定的类型,就会语法报错
* 泛型支持多态

```java
Integer num = list1.get(0);
```

* 利用了泛型,所以取数据的时候就不需要强转类型

```java
Integer num1 = list.get(0);
```

* list集合没有规定泛型,所以取出来的对象都是Object型的数据,需要强转

##### ArrayList类源码分析

![image-20200921201148282](D:\wj\Documents\Java\image-20200921201148282.png)

![image-20200921201333219](D:\wj\Documents\Java\image-20200921201333219.png)

![image-20200921201416202](D:\wj\Documents\Java\image-20200921201416202.png)

从上图可以看出ArrayList内部定义了一个静态的常量 Object[]数组,默认数组的长度是0,属性elementData也是一个Object数组,但是,这个数组没有初始化

![image-20200921201804568](D:\wj\Documents\Java\image-20200921201804568.png)

创建Array List的对象,一定会调用了构造方法,由默认的构造方法可知,elementData会进行赋值,把Object数组(静态常量,空数组)赋值给了elementData

```java
list.add(new Integer(10));
```

在main方法中调用创建的ArrayList对象的add()

###### add()源码

![image-20200921202508437](D:\wj\Documents\Java\image-20200921202508437.png)

进入add()中,会先调用ensureCapacityInternal(),size之前没有使用,所以为size=0

###### ensureCapacityInternal()

![](D:\wj\Documents\Java\image-20200921203333208.png)

进入ensureCapacityInternal()中,传递的实参是1,即minCapacity=1,elementData是一个空数组

###### calculateCapacity()

![](D:\wj\Documents\Java\image-20200921203704312.png)

进入calculateCapacity()中,

if(elementData==DEFAULTCAPACITY_EMPTY_ELEMENTDATA)

满足条件(创建对象的时候,就已经相等),

所以就会执行

return Math.max(DEFAULT_CAPACITY, minCapacity);

![image-20200921204313598](D:\wj\Documents\Java\image-20200921204313598.png)

DEFAULT_CAPACITY=10,minCapacity=1,则返回10

###### ensureExplicitCapacity()

![image-20200922075313683](D:\wj\Documents\Java\image-20200922075313683.png)

进入ensureExplicitCapacity()中,minCapacity=10,elementData.length=0

If(minCapacity-elementData.length>0)所以条件成立

执行grow()

###### grow()

![image-20200922075648083](D:\wj\Documents\Java\image-20200922075648083.png)

进入grow()中.minCapacity=10

int oldCapacity=elementData.length;则oldCapacity=0

int newCapacity=oldCapacity+(oldCapacity>>1);则newCapacity=0

If(newCapacity-minCapacity<0) 0-10,条件成立

执行newCapacity=minCapacity;则newCapacity=10

![image-20200922080040054](D:\wj\Documents\Java\image-20200922080040054.png)

MAX_VALUE 这个常量的值是2的31次方-1

if (newCapacity - MAX_ARRAY_SIZE > 0)条件肯定不成立,不执行条件成立语句

elementData = Arrays.*copyOf*(elementData, newCapacity);

Arrays.copyOf():复制指定的数组,截取或用null填充,以使副本具有指定的长度

执行这句指令后,重新拷贝(elementData)一个新数组,elementData的原始数据不变,长度增加到10

再次进入add()中,执行elementData[size++]=e 储存数据

* 第二次调用的时候,就不会再扩容

![image-20200922075313683](D:\wj\Documents\Java\image-20200922075313683.png)

minCapacity=2,elementData.length=10.条件不成立,不会进行扩容

* 当添加第11个元素的时候

这时候的minCapacity=11,条件成立,执行grow() 扩容方法

![image-20200922075648083](D:\wj\Documents\Java\image-20200922075648083.png)

进入grow()中

oldCapacity=10,newCapacity=10+5=15

后续两个条件的判断都不成立,

据徐执行elementData = Arrays.copyOf(elementData, newCapacity);

进行扩容,这次扩容是上一次的1.5倍

0→10→15→22→33→49

##### 总结

ArrayList是实现了List接口的一个集合类,内部采用数组的结构来储存数据,支持下标访问,Array List的元素顺序默认是按照插入的顺序;允许重复元素,允许null元素,可以有多个null元素.

访问效率高,操作效率低

#### Vector

![img](D:\wj\Documents\Java\clip_image002.jpg)

从源码的角度看Vector 直接继承AbstractList这个抽象类，同时实现了List接口

![img](D:\wj\Documents\Java\clip_image004.jpg)

同时ArrayList类也是继承AbstractList抽象类，实现List接口

从继承链的角度来看Vector和ArrayList很接近

![img](D:\wj\Documents\Java\clip_image001.png)

在Vector的内部也能看见elementData这个Object[]的定义

**Vector也是采用数组结构来存储元素**

* 可以看出vector和ArrayList大部分方法都是一致的

##### vector和ArrayList的不同

###### 1.初始容量

![image-20200922084250971](D:\wj\Documents\Java\image-20200922084250971.png)

![image-20200922084356103](D:\wj\Documents\Java\image-20200922084356103.png)

![image-20200922084421446](D:\wj\Documents\Java\image-20200922084421446.png)

initialCapacity=10,capacityIncrement=0

判断(initialCapacity<0),条件不满足

执行this.elementData = new Object[initialCapacity]; 

elementData为长度为10的数组

执行this.capacityIncrement = capacityIncrement;

* ArrayList内部数组的长度为0,Vector默认是10
* ArrayList在第一次添加元素的时候,即调用add()时扩容,Vector第一次扩容实在构造方法中

###### 2.方法

Vector中存在ArrayList中不存在的方法

* Vector.addElement();

添加元素

* Vector.capacity();

返回当前Vector内部容量

* Vector.elementAt(0)

获取下标为0的元素,类似于ArrayList的get

* Vector.firstElement()

直接获取下标最小(下标为0)的元素

* Vector.lastElement()

直接获取下标最大的元素

###### 3.多线程情况下的安全性

Vector在多线程的情况下是安全的,ArrayList在多线程的情况下是不安全的

###### 4.扩容机制

* 除了上述四点,其余地方没有区别

##### Vector源码分析

Vector初始容量为10,那么添加第11个元素的事就也需要扩容,需要使用到add()

###### add()

![image-20200922090004049](D:\wj\Documents\Java\image-20200922090004049.png)

elementCount表示当前集合的个数,类似于Array List中的size

###### ensureCapacityHelper()

![image-20200922090311648](D:\wj\Documents\Java\image-20200922090311648.png)

进入ensureCapacityHelper()中,

判断(minCapacity-elementData.length) 11-10

条件成立,执行grow()

###### grow()

![image-20200922090611788](D:\wj\Documents\Java\image-20200922090611788.png)

oldCapacity=10,capacityIncrement=0,newCapacity=10+10=20

后续两个判断都不成立

执行elementData = Arrays.copyOf(elementData, newCapacity);

新长度为20

所以Vector的扩容是上次的两倍

10→20→40→80

#### ArrayList与Vector区别

1. Vector线程安全,ArrayList线程不安全

2. Vector在创建对象的时候,就已经把容量设定为10个元素;ArrayList在创建对象的时候容量是0,使用add()方法后才把容量扩充到10

3. Vector在扩容的时候,采用的是上次的二倍(10,20,40,80)

   ArrayList在扩容的时候,采用的是上次的1.5倍(10,15,22,33,49)

   出现多线程的时候,推荐使用Vector,因为Vector能保障在多线程的情况下当前的集合是安全的,在没有多线程的情况下使用ArrayList,因为保证线程安全的前提下是要损失性能的

#### Stack(栈)

![image-20200922091923651](D:\wj\Documents\Java\image-20200922091923651.png)

Stack这个类也是实现了List接口,

Stack是Vector的子类

Stack实现了栈结构的储存方法,**stack内部采用的数组结构**,只是在Vector的基础上,实现了栈的储存方式

通过五个操作对类Vector进行了扩展

![img](https://img-blog.csdn.net/20161127093301536)

Stack提供栈操作的一系列方法:

push(),pop(),peek(),empty(),search()

* peek()

从栈顶获取元素

*  push()

元素添加到内部的数组中

* pop()

通过弹栈操作,从栈顶获取元素

弹栈:把容器中的栈顶元素弹出,即弹出后,站内就不存在弹出的元素

* empty()

判断栈内是否存在元素,没有元素,返回true,否则返回false

* search()

在栈内查找指定的元素,并返回元素的位置,不存在则返回-1

***



* addAll(Collections c)

把参数集合中的元素全部添加到当前集合中

* addElement()

继承自Vector的方法,把元素添加到当前的栈中

* capacity()

返回当前栈的容量

* firstElement()

返回第一次插入的元素,不是栈顶元素

#### Stack和Vector区别

Stack在Vector的基础上实现栈的操作;

Stack类的构造方法是空实现

Stack在Vector的基础上添加了5个方法

push（），peek（），pop（），search（)，empty（）

##### 添加元素的方法:

Stack的push()

![image-20200922124633558](D:\wj\Documents\Java\image-20200922124633558.png)

Vector的addElement()

![image-20200922124804296](D:\wj\Documents\Java\image-20200922124804296.png)

##### push()

* 从源码的角度分析:

Stack的push(),只是把元素添加到内部的数组中,并没有实现栈的操作,添加的第一个元素还是存储在内部数组的第零个元素

peek()只是取栈顶的元素,从分析来看,push()没有实现栈的操作,那么peek()取元素的时候,需要实现栈的操作,取内部数组中下标最大的元素

![image-20200922125428318](D:\wj\Documents\Java\image-20200922125428318.png)

peek()的内部先定义了一个局部变量len,被赋值为size(),(size()记录了当前集合中元素的个数)

判断(len==0)条件不成立,抛出空栈异常,否则利用elementAt(len-1)返回最后添加的元素(这个操作符合栈的出栈操作),但是这个方法只是返回栈顶的元素,没有移除

##### pop()弹栈操作

![image-20200922130025932](D:\wj\Documents\Java\image-20200922130025932.png)

pop()和peek()类似

获取元素时,使用的是removeElement(len-1),获取指定位置的元素,并移除

Stack只是在Vector的基础上添加了5个方法:

push(),peek(),pop(),search(),empty()

##### 栈的使用场景

例如,现在需要实现一个在线售票系统,模拟有四个窗口同时售票,票的总数是100张,那么就需要把所有的票存在一个集合中,不管哪个窗口售出的票需要都需要在总票的集合中移除,那么这种场景中的集合就需要使用Stack就比较合适

#### LinkedList

LinkedList.也是实现了List接口的一个集合类,但是LinkedList内部采用了链表结构(双向链表结构),所以访问元素的效率比较低,操作元素(删除,增加)的效率比较高

![image-20200922220518109](D:\wj\Documents\Java\image-20200922220518109.png)

LinkedList内部定义了一个内部类,Node就是链表的一个元素

##### LinkedList方法

* add()

添加元素,如果是第一次调用,也就是把元素添加到链表的头部,第二次及以后的调用都是把元素添加到链表的尾部

* addFirst()

添加元素到链表的头部

* addLast()
* offer()
* offerLast()

将指定元素添加到链表的尾部

* offerFirst()
* push()

将指定元素添加到链表的头部

* element()
* getFirst()
* peek()
* peekFirst()
* poll()
* pollFirst()

获取链表头部元素

* getLast()
* peekLst()
* pollLast()

获取链表末尾元素

* pop()

弹出链表的头部元素

* get()

获取链表中指定元素

##### 链表操作

1. 添加元素

   LlinkedList内部采用的是双向链表结构,所以不存在扩容问题,元素的空间不是连续的,所以访问必须从链表的头部开始依次访问

2. LinkedList内部的一个元素会分为三个部分(上个元素的位置,元素数据值,下一个元素的位置),添加进来的元素,LinkedList会自动封装成把它封装成内部元素(会自动创建一个对象,对象存在三部分的结构)

添加第一个元素,会先判断当前的LinkedList内部存不存在元素,如果不存在则当前的元素就是第一个,那么就会把上一个元素的地址赋值为null,元素内部不变,下一个元素的地址设置为null

![image-20200922215049176](D:\wj\Documents\Java\image-20200922215049176.png)

* 添加的第一个元素

![image-20200922215121924](D:\wj\Documents\Java\image-20200922215121924.png)

* 当添加第二个元素时第一个元素的赋值情况

![image-20200922215343864](D:\wj\Documents\Java\image-20200922215343864.png)

* 第二个元素

![image-20200922215403681](D:\wj\Documents\Java\image-20200922215403681.png)

* 第三个元素

#### LinkedList,ArrayList,Vector,Stack区别

* 内部采用的结构不同,效率就不相同

* List接口下的常用的类(ArrayList,Vector,Stack,LinkedList)

### 3.Set接口

Set接口也是继承了Collection接口,和List一样是Collection接口的重要的子接口

**Set是不包括重复元素的Collection集合**,List是包含重复元素的Collection,即Set不包含满足e1.equals(e2)的元素e1和e2,只能有一个null元素

HashSet判断元素是否重复是通过hashcode()以及equals()来综合判断是否为重复元素

#### Set接口下的实现类

##### 1.HashSet类

由哈希表（实际上是一个 HashMap 实例）支持。

内部是利用HashMap来储存元素的

HashSet实现了Set接口,由Hash表支持,利用Hash算法来确定元素的位置,所以**不保证Set的迭代顺序**,即遍历顺序,特别是他不保证该顺序恒久不变,**此类允许使用null元素**

###### 判断重复依据

**HashSet判断元素的重复，是利用equals()和hashCode()两个方法**

###### 1.HashSet的常用方法

LinkedHashSet()和TreeSet()

HashSet内部采用数组的结构,所以也会存在容量问题,**默认容量为16个**,HashSet储存元素的位置是根据**元素的hashCode值**和**当前hashSet的容量**取&运算得到的

###### 2.元素位置发生改变原因

1. hashCode的值发生改变
2. 对象的hashCode值未发生改变,但是hashSet的容量扩容了,即值发生改变

向HashSet集合中添加一个元素.首先,hashSet会调用当前元素的hashCode值,得到这个值和当前hashSet的初始容量-1,第一次hashSet初始的容量是16,也就是说数组的长度是16,目前是要计算元素的位置,数组的下标是从0开始的,最大是15, 例如:存储的当前的元素hashCode值是23 ,那么计算的表达式是 23&15 的结果，所以HashSet的元素的位置是变化的,即hashSet是不保证遍历的顺序

HashSet不支持下标索引,索引没有定义get(index)方法(获取某一特定元素)

```java
import java.util.*;

public class demo5 {

    private int userId;
    private String userName;
    private String userPwd;

    public int getUserId() {
        return userId;
    }
    public String getUserName() {
        return userName;
    }
    public String getUserPwd() {
        return userPwd;
    }
    //获取私有属性
    public void setUserId(int userId) {
        this.userId = userId;
    }
    public void setUserName(String userName) {
        this.userName = userName;
    }
    public void setUserPwd(String userPwd) {
        this.userPwd = userPwd;
    }

    @Override
    public String toString() {
        return "demo5{" +
                "userId=" + userId +
                ", userName='" + userName + '\'' +
                ", userPwd='" + userPwd + '\'' +
                '}';
    }
    //变为字符串

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        demo5 demo5 = (demo5) o;
        return userId == demo5.userId;
        //是否相等,比较的是userId的值
    }

    @Override
    public int hashCode() {
        return Objects.hash(userId);
        //hashCode保持一致
    }
}
```

```java
import java.util.HashSet;

public class Test1 extends demo5{

    public static void main(String[] args) {
        demo5 d = new demo5();
        d.setUserId(1);
        d.setUserName("张三");
        d.setUserPwd("a111");
        demo5 d1 = new demo5();
        d1.setUserId(2);
        d1.setUserName("李四");
        d1.setUserPwd("a111");
        demo5 d2 = new demo5();
        d2.setUserId(3);
        d2.setUserName("王五");
        d2.setUserPwd("a111");
        HashSet<demo5> hashSet = new HashSet<>();//HashSet是一个集合类
        //使用泛型,使hashSet的类型为demo5
        hashSet.add(d);
        hashSet.add(d1);
        hashSet.add(d2);
        for (demo5 s:hashSet){
            System.out.println(s);
        }
    }

}
```

* 使用泛型,可以规定当前集合只能储存那个类型的对象
* **equals和hashCode方法修改为判断userId属性**,遍历出来为:

![image-20200925081707388](D:\wj\Documents\Java\image-20200925081707388.png)

* **equals和hashCode方法修改为判断userName属性**,会影响hashSet的遍历顺序,即不保证迭代顺序

![image-20200925081610804](D:\wj\Documents\Java\image-20200925081610804.png)

* 一个集合的遍历是反映当前元素在集合内部的顺序
* 如果是equals和hashCode判断的是userId属性,那么当前三个对象在hashSet内部的顺序是以userId为顺序,即1,2,3
* 如果equals和hashCode方法判断的是userName属性,那么当前三个对象在hashSet内部的顺序就会发生改变

###### 3.碰撞问题

hashset类默认初始内部空间容量是16,内部也使用数组结构来储存元素,当需要把一个对象储存到hashset类中,hashset就要把这个对象储存到一个位置上,如果是list集合下ArrayList之类的集合类,一定是把这个对象储存到下标为0的位置上,但是hashset不同

首先先得到这个对象的hashcode值,hashcode值是一个数值类的值,计算当前对象的hashcode值与当前容量-1的&运算(位于运算),得到的值就是当前对象在hashset中的位置

例如:当前对象的hashcode值为23,目前hashset的容量为16,则计算23&(16-1),即 10111&1111=0011,转换为十进制就是7,就会把这个对象储存到hashset内部数组中下标为7的位置上

如果两个元素的hashcode值相同,equals()有可能返回false,有可能返回true,这个时候就要进一步调用equals(),如果equals()返回true,那么hashset就会认为当前元素为重复元素,就不会再进行添加

两个对象的hashcode值不一致,但是计算出的结果一致,例如:一个元素的hashcode值为39,计算39&15=7,与第一个对象的值相同,就会出现两个元素一个位置的情况,把这种情况称为碰撞

**hashset处理碰撞问题的方式:**

把相同位置的元素形成一个链表,hashset内部数组这个位置就只储存当前链表的链头地址,当一个位置的链表个数大于等于8个,那么当前位置的链表就会变成树结构(红黑树),以减少搜索时间.

##### 2.LinkedHashSet类

具有预知迭代顺序的Set接口的哈希表和链接列表的实现.与HashSet不同的是,LinkedHashSet维护者一个运行于所有条目的双重链接列表,此链接列表定义了迭代顺序,即按照插入顺序

![image-20201023171427631](D:\wj\Documents\Java\image-20201023171427631.png)

从源码看,LishinkedHashSet直接继承了HashSet,从继承承关系来看,LinkedHashSet是对HashSet的拓展,HashSet是无序的,LinkedHashSet是有序的

###### 1.LinkedHashSet保证顺序的原因

LInkedHashSet在HashSet的基础上添加了一个链表,用来记录元素元素的位置,顺序是插入的顺序,迭代是迭代这个链表,元素的存储还是原来的HashSet数组的方式,迭代和储存是分开的

###### 2.LinkedHashSet与HashSet不同

LInkedHashSet在HashSet的基础上添加了一个链表用来记录元素的顺序,按照插入的先后顺序来形成链表,hashcode值的变化,hashset的扩容都不影响链表的顺序,其他基本相同

LInkedHashSet只是弥补了HashSet不保证迭代顺序的这个问题

##### 3.TreeSet类

使用红黑树结构来储存元素,TreeSet类的元素都必须是有序且没有重复元素,所以TreeSet在添加元素的时候,会调用元素的自然排序,**TreeSet类储存的元素类必须实现comparable接口**

TreeSet判断元素的重复是通过comparaTo().当comparaTo()返回0,则判断为重复元素,否则不为重复元素.

```java
import java.util.TreeSet;

public class Person extends TreeSet implements Comparable{

    private int age;
    private String name;
    private String school;


    @Override
    public int compareTo(Object o) {
        Person p=(Person)o;
        return this.age-p.age;
    }//返回的数是0代表两个元素相同，正数说明大于，负数说明小于
        //升序
}
```

实现了Comparable接口,并实现了compareTo(),设置为按照Student的grade属性进行排序

##### 4.红黑树

红黑树是一种自平衡二叉查找树 , 它们当中每一个节点的比较值都必须大于或等于在它的左子树中的所有节点，并且小于或等于在它的右子树中的所有节点。这确保红黑树运作时能够快速的在树中查找给定的值。

##### 5.TreeSet与HashSet的区别

TreeSet类有序,自然排序;HashSet无序

HashSet判断元素的重复是通过equals()和hashcode()

TreeSet判断元素的重复是通过comparaTo().当comparaTo()返回0,则判断为重复元素,否则不为重复元素.

### 4.Queue接口

队列是一种特殊的线性表，它只允许在表的前端进行删除操作，而在表的后端进行插入操作。
LinkedList类实现了Queue接口，因此我们可以把LinkedList当成Queue来用。



## 3.hashSet常见问题

1. hashSet的元素的位置

元素的hashcode值和当前hashset容量-1,进行位于运算得出的值就是元素的位置


2. hashSet的碰撞是什么意思

例如:一个元素的hashcode值为39,计算39&15=7,与23&15值相同,就会出现两个元素一个位置的情况,把这种情况称为碰撞


3. hashSet发生碰撞以后的处理方法；

会在发生碰撞的位置以链表的方式储存碰撞的元素,碰撞位置中储存的是链表的头节点


4. HashSet的初始容量为什么是16（思考的问题)

从空间性能角度考虑16最为合适,而且降低碰撞的几率


5. HashSet为什么不保证迭代顺序；

hashset中元素的位置的确定是通过计算元素的hashcode值与当前hashset容量-1进行位于运算得到的,当hashcode值改变,或者容量改变都会影响到hashset中元素位置的改变


6. hashSet元素存储完成以后，位置会发生改变吗?为什么

会,当前容量储存满的时候,扩容会引起位置的变化


7. HashSet判断重复元素的标准

1."==",判断地址             2.equals()          3.hashcode值


8. 两个对象的hashCode()值返回的是相同的整数，那么调用这个两个对象的equals()应该返回true还是false

Java中对象的规则，两个对象的equals()返回true，那么要求这两个对象的hashCode()返回相同值，如果两个对象的equals()返回false，那么不要求这两个对象的hashCode()返回不同的值

如果根据 equals(Object) 方法，两个对象是相等的，那么对这两个对象中的每个对象调用 hashCode 方法都必须生成相同值

如果根据 equals(java.lang.Object) 方法，两个对象不相等，那么对这两个对象中的任一对象上调用 hashCode 方法不要求一定生成不同的整数结果。

所以

两个对象的equals()返回true,hashcode值不一定一致;

两个对象的hashcode值一致时,equals()可能返回true,有可能返回false


9. HashSet元素发生碰撞以后,链表达到8级以后会发生什么改变

当一个位置的链表个数大于等于8个,那么当前位置的链表就会变成树结构(红黑树)

10. HashSet在什么时候会扩容，扩容的条件是什么？

HashSet内部定义了一个扩容因子,值为0.75.这个扩容因子就是计算扩容条件的,当前储存的元素个数达到(当前容量*0.75)个,就开始扩容.可达到空间和时间上的平衡,减少碰撞几率

List集合下的实现类都是储存空间不够的时候才开始扩容



## 4.Comparable接口

Comparable接口为排序接口,Comparable接口规定了一个方法**comparaTo()**,这个方法就是当前类的默认排序方法,实现了Comparable接口的comparaTo(),称为当前类的自然排序,强行对实现它的对象进行整体排序

![image-20201027175714264](D:\wj\Documents\Java\image-20201027175714264.png)

当compareTo方法返回0的时候集合中只有一个元素
当compareTo方法返回正数的时候集合会怎么存就怎么取
当compareTo方法返回负数的时候集合会倒序存储

为什么返回0，只会存一个元素，返回-1会倒序存储，返回1会怎么存就怎么取呢？原因在于TreeSet底层其实是一个二叉树机构，且每插入一个新元素(第一个除外)都会调用compareTo()方法去和上一个插入的元素作比较，并按二叉树的结构进行排列。

1. 如果将compareTo()返回值写死为0，元素值每次比较，都认为是相同的元素，这时就不再向TreeSet中插入除第一个外的新元素。所以TreeSet中就只存在插入的第一个元素。
2. 如果将compareTo()返回值写死为1，元素值每次比较，都认为新插入的元素比上一个元素大，于是二叉树存储时，会存在根的右侧，读取时就是正序排列的。
3. 如果将compareTo()返回值写死为-1，元素值每次比较，都认为新插入的元素比上一个元素小，于是二叉树存储时，会存在根的左侧，读取时就是倒序序排列的。

## 5.Map接口

Map接口没有继承Collection接口

将键映射到值的对象.一个映射不能包含重复的键,每个键最多只能映射到一个值

Map接口是存储key-value的映射键值对的集合,内部元素都是一个key对应一个value,那么在Map集合中查找时,操作元素都是利用key来操作<K,V>,K代表key的类型,V表示value的类型,默认key和value都是Object类型

### 1.HashMap类

基于一个Hash表的Map接口的实现

Map接口中key是不能重复的,HashMap允许null键,也允许null值

HashMap的储存方式和HashSet一致,都是利用元素的(hashcode值&当前容量-1)来确定元素存储的位置,所以HashMap也是无序的,不保证迭代顺序的恒久不变

Map是不能直接迭代的,不能直接使用foreach来循环,HashMap都是使用key来获取元素的

一个Map内部由三个集合组成

1. values集合存储所有的value
2. keySet集合存储所有的key
3. entrySet集合存储key和value的映射

![](https://www.hollischuang.com/wp-content/uploads/2019/12/15757045632047.jpg)

* 添加元素:put()
* 通过key获取对应元素:get(key)

### 2.HashTable类

基于Hash表的实现,**不允许null值和null键**,HashMap允许null键,也允许null值.HashTable类线程安全,HashMap类线程不安全,但是在没有多线程的情况下,HashMap效率高于HashTable.其他一致

### 3.LinkedHashMap类

继承自HashMap类

LinkedHashMap类是在HashMap类的基础上添加了一个链表结构来记录元素的顺序,来保障迭代顺序,元素还是存储在HashMap上

### 4.LinkedHashSet与LinkedHashMap

HashSet和HashMap都是不保证迭代顺序的,内部储存方式相同,只是HashMap除了储存元素本身,还储存key

所以Java的集合类中添加了两个类:

1. LinkedHashSet:继承自HashSet,在HashSet的基础上添加一个链表来维护元素的顺序不变

2. LinkedHashMap:继承自HashMap,在HashMap的基础上添加一个链表来维护元素的顺序不变

### 5.TreeMap类

基于红黑树的实现,TreeMap中的元素默认按照keys的自然排序排列,所以TreeMap的元素不用实现排序接口

（对Integer来说，其自然排序就是数字的升序；对String来说，其自然排序就是按照字母表排序）

和TreeSet实现方法几乎一致

### 6.如何选择合适的Map

- HashMap可实现快速存储和检索，但其缺点是其包含的元素是无序的，这导致它在存在大量迭代的情况下表现不佳。
- LinkedHashMap保留了HashMap的优势，且其包含的元素是有序的。它在有大量迭代的情况下表现更好。
- TreeMap能便捷的实现对其内部元素的各种排序，但其一般性能比前两种map差。

**LinkedHashMap映射减少了HashMap排序中的混乱，且不会导致TreeMap的性能损失。**

## 6.集合框架体系图

![7d1cddbb3ba82e9e259d20bb3b1f7058](D:\wj\Documents\Java\7d1cddbb3ba82e9e259d20bb3b1f7058.png)

## 7.泛型

1. 在定义类的时候内部类型不确定的情况下,可以使用泛型,在创建对象的时候再确定其类型

```java
public class MyList{
    private Object[] elementData;
    private int size;
    public MyList(){
        elementData=new Object[10];
    }
    public void add(Object obj){
        elementData[size++]=obj;
    }
    public Object get(int index){
        return elementData[index];
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        MyList list=new MyList();
        list.add(new Integer(10));
        list.add(new Integer(20));
        list.add("abc");
    }
}
```

* add()参数类型是Object,,即只要是对象都能存储进来,那么就会存在类型不安全的问题
* 在一个集合中出现不同类型的元素,那么在取元素的时候会发生类型问题

2. 定义泛型可以为任何类型<T>,T可以是任何类型

```java
public class MyList<T>{
    private Object[] elementData;
    private int size;
    public MyList(){
        elementData=new Object[10];
    }
    public void add(T obj){
        elementData[size++]=obj;
    }
    public T get(int index){
        return (T)elementData[index];
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        MyList<Integer> list=new MyList<>();
        list.add(new Integer(10));
        list.add(new Integer(20));
         MyList<String> list1=new MyList<>();
        list.add("abc");
    }
}
```

* list会自动把类中的T替换成Integer
* list1会自动把类中的T替换成String

3. 定义并约束了泛型,<T extends Number>表示当前泛型必须是Number的子类类型

**Number是数值类型的包装类的父类,Long,Double,Float,Integer,Short都是Number的子类**

```java
public class MyList<T extends Number>{
    private Object[] elementData;
    private int size;
    public MyList(){
        elementData=new Object[10];
    }
    public void add(T obj){
        elementData[size++]=obj;
    }
    public T get(int index){
        return (T)elementData[index];
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        MyList<Integer> list=new MyList<>();
        list.add(new Integer(10));
        list.add(new Integer(20));
    }
}
```

* MyList<<u>String</u>> list1=new MyList<<u>String</u>>();
          list.add("abc");

当定义当前泛型为String时,就会报错,String不是Number的子类

4. 泛型是接口,<T extends Comparable>,约束接口也使用的是extends

```java
public class MyList<T extends Comparable>{
    private Object[] elementData;
    private int size;
    public MyList(){
        elementData=new Object[10];
    }
    public void add(T obj){
        elementData[size++]=obj;
    }
    public T get(int index){
        return (T)elementData[index];
    }
}
```

```java
public class User{
    
}
```

```java
public class Test{
    public static void main(String[] args){
        MyList<Integer> list=new MyList<>();
         MyList<String> list1=new MyList<String>();
    }
}
```

* MyList<<u>User</u>> list2=new MyList<<u>User</u>>();

自定义的User没有实现Comparable接口,所以泛型不能定义为User

==高亮==
