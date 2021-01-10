# Java学习Day3

[toc]



### 1.位运算符

运算的数据都是二进制数据(需要把数据先转换成二进制的数据,按位进行计算)

##### (1)&	与位运算符

```java
public static void main(String[] args){
    int num=10,num1=2;
    int result=num&num1;
    System.out.println(result);//out:2
}
```

与运算:	

10	00001010

​		   	&

 2	 00000010			 	运算规则:1&1=1,1&0=0,0&0=0

​				=

 2	  00000010

##### (2)|	或运算符

```java
int num=10,num1=2;
int result=num|num1;
System.out.println(result);//out:10
```

或运算:	

10	00001010

​				|

2	  00000010				运算规则:1|1=1,1|0=1,0|0=0

​				=

10	00001010

##### (3)^	异或运算

```java
int num=10,num1=2;
int result=num^num1;
System.out.println(result);//out:8
```

异或运算:

10	00001010

​				^

2	  00000010				运算规则:1^1=0, 1^0=1, 0^1=1, 0^0=0

​				=

8	  00001000

##### (4)~	位取反(符号(正负))

```java
int num=10;
int result=~num;
System.out.println(result);//out:-11
```

0取反为-1,1取反为-2

**注意:~是一个单目运算符(运算符按照操作数的数量分为单目运算符,双目运算符,三目运算符),单目运算符就是只需要一个操作数,单目运算符(++,--,!, ~)**

##### (5)'>>'	位的右移

```java
int num=10;
int result=num>>2;
System.out.println(result);//out:2
```

'>>'	表示向右移动	2	表示移动多少位(2就是移动两位)

00001010 >>2	00000010	结果为:2

例如:	int num=34,	int result=num>>3	result的结果是4

##### (6)'<<'	位的左移

```java
int num=100;
int result=num<<2;
System.out.println(result);//out:400
```

'<<'	表示向左移动	2	表示移动多少位(2就是移动两位)

01100100 <<2   0000000110010000	结果为:400

### 2.数组

* 数组:属于引用类型,用来存引用(地址),引用类型的0值,是null

* Java中的数组存放于堆中,都是在堆上开辟内存

* int num;	//会在内存中开辟一片4个字节的空间(空间的开辟是无序的)

* int num1;  //会先查找当前内存空余空间中分配一个4个字节的空间,下一个变量,num和num1可能不是连续的

* 数组:一次性可以定义多个数据类型相同的变量

* ```java
  int[] nums=new int[10];//定义一个长度为10的int类型的数组
  nums[0]=10;
  ......
  nums[9]=10;
  ```

  二维数组内存结构:
  
  ![image-20210102212604409](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210102212604409.png)
  
  不规则二维数组:
  
  ```java
  int[][] array4 = new int[2][];
  ```
  
  ![image-20210102213225990](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210102213225990.png)
  
  解决:自己指定列
  
  ```java
  array4[0] = new int[]{1,2,3};
  array4[1] = new int[]{4,5};
  ```
  
  ![image-20210102213359735](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210102213359735.png)
  
  打印不规则二维数组:
  
  ```java
  Arrays.deepTostring(array4);
  ```
  
  打印数组:
  
  ```java
  1:
  for(int i=0;i<array.length;i++) {//length:5
      System.out.println(array[i]+" ");
  }
  2:
  for(int x:array) {
      System.out.println(array[i]+" ");
  }
  3:
  String str = Arrays.toString(array);//将数组以字符串的形式打印出来
  ```
  
  #### Ⅰ.数组的定义方式
  
  ```java
  //静态初始化
  int[] array = {1,2,3,4,5};
  //动态初始化
  int[] array1 = new int[4];//定义一个数组,但是没有初始化,会赋值为零
  int[] array2 = new int[]{1,2,3,4,5};
  
  int[][] array3 = {{1,2,3},{4,5,6}};
  int[][] array4 = new int[][]{{1,2,3},{4,5,6}};
  int[][] array5 = new int[2][3];
  ```
  
  Arrays类:操作数组的工具类
  
  包含有对数组操作的方法
  
  ```java
  array = null;
  //当置为空,原来的对象就会被GC自动回收
  ```
  
  
  
  数组越界:
  
  ![image-20210102145727982](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210102145727982.png)
  
  空指针异常:
  
  <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210102150312847.png" alt="image-20210102150312847"  />
  
  数组作为参数传递,为引用传递
  
  ![image-20210102153026524](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210102153026524.png)
  
  数组作为返回值	
  
  变量作为参数传递,为值传递
  
  ![image-20210102154524598](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210102154524598.png)
  
  ***
  
  数组的拷贝:
  
  1. 通过for循环拷贝
  
     ```java
     public static int[] copyArray(int[] array) {
         int[] array2 = new int[array.length];
     	for(int i=0;i<array.length;i++) {
         array2[i]=array[i];
     	}
         return array2;
     }
     ```
  
     ![image-20210102184154279](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210102184154279.png)
  
  2. ```java
     int[] array2=Arrays.copyOf(array,array.length);
     //也是通过调用arraycopy写的
     ```
  
  3. ```java
     System.arraycopy(array,0,array2,0,length);
     /*
     	1.native:本地方法
     	2.src:原数组
     	  srcPos:原数组的pos位置开始拷贝
     	  dest:拷贝到的目的数组
     	  destPos:拷贝到的目的数组的pos位置
     	  length:拷贝多长
     	3.arraycopy:拷贝速度最快
     	
     */
     ```
  
  4. ```java
     int[] array2 = array.clone();
     //产生需要克隆的对象的副本
     ```
  
     **浅拷贝:拷贝的是引用**
  
     ![image-20210102191050062](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210102191050062.png)
  
     **深拷贝:拷贝的是对象**
  
     ![image-20210102191035209](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210102191035209.png)
  
  Java中区间范围一般都为[a,b)
  
  二分查找:
  
  ![image-20210102194142096](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210102194142096.png)
  
  Arrays.sort();//数组排序
  
  ![image-20210102200934975](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210102200934975.png)
  
  如果数组为null,先判断array.length==0就会出现空指针异常


### 3.Java程序的控制流程

程序都是按照至上而下,从左到右的执行顺序执行:需要按照业务的不同(条件的不同)判断是否执行,或者多次执行相同的代码,需要使用控制流程

程序中的{}程序一般一行指令是一个执行单元,需要多条执行组合成一个业务,那么就需要把多条指令组合成一个执行单元,多条指令的表现就是使用大括号{}

#### Ⅰ.判断:

条件:C中非零为真,但是Java中不适用

```java
int a=10;
if(a) {
    System.out.println("123");
} else {
    System.out.println("abc");
}
//报错,错误: 不兼容类型: int无法转换为boolean
```

	Java中条件表达式必须是boolean表达式

要么是true,要么是false

true: "a == 10" , "a != 10" 

#### Ⅱ.switch语句:(1.判断情况较多,2.都是等额判断)

```java
/*switch的参数最大为4个字节,char类型可以
switch参数表达式不能过于复杂
switch可以嵌套,但是太乱
    4个不能作为switch参数的类型
    从long转换到int可能会有损失
    从float转换到int可能会有损失
    从double转换到int可能会有损失
    boolean无法转换为int
    */
//在JDK1.5 枚举也是可以作为switch的参数,字符串也可以作为参数
switch(a) {
    case :
        break;
    case :
        break;
    case :
        break;
    default :
        break;
}
```

continue:结束本次循环

break:跳出整个循环

#### Ⅲ.循环:

Java中提供四种循环

1. ```java
   do{//限制性后判断
       执行语句;
   }while(循环条件);
   ```

2. ```java
   while(循环条件){//先判断后执行
       执行语句;
   }
   ```

3. ```java
   for(循环条件初始;循环条件;循环变量的递增){
       执行语句;
   }
   ```

4. ```java
   foreach循环(针对数组)
       for(数组元素的数据类型 变量:数组){
           当次循环的变量是每一个数组元素;
       }
   
   int[] nums=new int[n];
   for(int n:nums){
       System.out.println(n);
   }
   /*
   foreach遍历数组的效率比for的高,但是foreach循环内部没有下标,而且foreach不能访问一部分,foreach内部不能操作下标
   */
   ```

