# Java 和 C++ 的区别

Java 和 C++ 都是面向对象的语言，都支持封装、继承和多态，但是，它们还是有不相同的地方：

  **Java 不提供指针来直接访问内存，程序内存更加安全**

  **Java 的类是单继承的，C++ 支持多重继承；**

**Java 的类不可以多继承，但是接口可以多继承。**

  **Java 有自动内存管理垃圾回收机制(GC)，不需要程序员手动释放无用内存。**

  **C ++同时支持方法重载和操作符重载，但是 Java 只支持方法重载（操作符重载增加了复杂性，这与 Java 最初的设计思想不符）。**

















# JVM vs JDK vs JRE

 

java虚拟机（JVM）是运行 Java 字节码的虚拟机。JVM 有针对不同系统的特定实现（Windows，Linux，macOS），目的是使用相同的字节码，它们都会给出相同的结果。字节码和不同系统的 JVM 实现是 Java 语言“一次编译，随处可以运行”的关键所在。

JVM 并不是只有一种！只要满足 JVM 规范，每个公司、组织或者个人都可以开发自己的专属 JVM。

所有的 java 程序会首先被编译为.class 的类文件，这种类文件可以在虚拟机上执行，也就是说 class 并不直接与机器的操

作系统相对应，而是经过虚拟机间接与操作系统交互，由虚拟机将程序解释给本地系统执行，实现跨平台



<img src="java面试.assets/无标题流程图(1).drawio-16627872626454.png" alt="无标题流程图(1).drawio" style="zoom: 33%;" />



https://javaguide.cn/java/jvm/jvm-intro.html#%E5%89%8D%E8%A8%80





















# JDK 和 JRE

JDK 是 Java Development Kit 缩写，它是功能齐全的 Java SDK。它拥有 JRE 所拥有的一切，还有编译器（javac）和工具（如 javadoc 和 jdb）。它能够创建和编译程序。**（java开发工具包以及JRE java运行环境  java核心类库（API））**



JRE 是 Java 运行时环境。它是运行已编译 Java 程序所需的所有内容的集合，包括 Java 虚拟机（JVM），Java 类库，java 命令和其他的一些基础构件。但是，它不能用于创建新程序。

 













# 字节码

什么是字节码?采用字节码的好处是什么?

在 Java 中，JVM 可以理解的代码就叫做字节码（即扩展名为 .class 的文件），它不面向任何特定的处理器，只面向虚拟机。

Java 语言通过字节码的方式，在一定程度上解决了传统解释型语言执行效率低的问题，同时又保留了解释型语言可移植的特点。所以， Java 程序运行时相对来说还是高效的（不过，和 C++，Rust，Go 等语言还是有一定差距的），而且，由于字节码并不针对一种特定的机器，因此，Java 程序无须重新编译便可在多种不同操作系统的计算机上运行。



<img src="java面试.assets/clip_image002.gif" alt="截图.png" style="zoom:67%;" />

我们需要格外注意的是 

.class->机器码 这一步。在这一步 JVM 类加载器首先加载字节码文件，然后通过解释器逐行解释执行，这种方式的执行速度会相对比较慢。而且，有些方法和代码块是经常需要被调用的(也就是所谓的热点代码)，所以后面引进了 JIT（just-in-time compilation） 编译器，而 JIT 属于运行时编译。当 JIT 编译器完成第一次编译后，其会将字节码对应的机器码保存下来，下次可以直接使用。而我们知道，机器码的运行效率肯定是高于 Java 解释器的。这也解释了我们为什么经常会说 Java 是编译与解释共存的语言 。

 

 









# java主类

## 什么是Java程序的主类？应用程序和小程序的主类有何不同？

一个程序中可以有多个类，但只能有一个类是主类。

在Java应用程序中，这个主类是指包含main()方法的类。

而在Java小程序中，这个主类是一个继承自系统类JApplet或Applet的子类。应用程序的主类不一定要求是public类，但小程序的主类要求必须是public类。主类是Java程序执行的入口点





## java类初始化顺序



![image-20220922093113933](java面试.assets/image-20220922093113933-16638102764881.png)



![image-20220922093145432](java面试.assets/image-20220922093145432-16638103067783.png)



1



![image-20220926211215917](java面试.assets/image-20220926211215917-16641979374463.png)









2

**并不是静态块**最先初始化,而是静态域。

  而静态域中包含静态变量、静态块和静态方法,其中需要初始化的是静态变量和静态块。而他们两个的初始化顺序是靠他们俩的位置决定的。 

  那么最后的执行顺序就是：静态域>main()>构造块>构造方法



![image-20220926171232513](java面试.assets/image-20220926171232513.png)



```
构造块 构造块 静态块 构造块
```



构造块：类中直接用{}定义，每一次创建对象时执行

静态块：用static申明，JVM加载类时执行，仅执行一次







例题

```
class X{
    Y y=new Y();
    public X(){
        System.out.print("X");
    }
}

class Y{
    public Y(){
        System.out.print("Y");
    }
}
public class Z extends X{
    Y y=new Y();
    public Z(){
        System.out.print("Z");
    }
    public static void main(String[] args) {
        new Z();
    }
}
```

输出：YXYZ



**初始化过程：** 

**1.** **初始化父类中的静态成员变量和静态代码块**（静态域） **；** 

**2.** **初始化子类中的静态成员变量和静态代码块** **（静态域）；** 

**3.初始化父类的普通成员变量和代码块，再执行父类的构造方法；**

**4.初始化子类的普通成员变量和代码块，再执行子类的构造方法；** 

 

**（1）初始化父类的普通成员变量和代码块，执行 Y y=new** **Y();** **输出Y** 

**（2）再执行父类的构造方法；输出X**

**（3）** **初始化子类的普通成员变量和代码块，执行 Y y=new**  **Y();** **输出Y** 

**（4）再执行子类的构造方法；输出Z**

 **所以输出YXYZ**







上面的简化例子

<img src="java面试.assets/image-20220926220147154.png" alt="image-20220926220147154" style="zoom:67%;" />

这是父类代码块
这是父类构造方法
这是子类代码块
这是子类构造方法





# 注释



## 什么Java注释

**定义**：用于解释说明程序的文字

**分类**

- 单行注释
   格式： // 注释文字



- 多行注释
   格式： /* 注释文字 */



- 文档注释
   格式：/** 注释文字 */

**作用**

- 在程序中，尤其是复杂的程序中，适当地加入注释可以增加程序的可读性，有利于程序的修改、调试和交流。注释的内容在程序编译的时候会被忽视，不会产生目标代码，注释的部分不会对程序的执行结果产生任何影响。

```
注意事项：多行和文档注释都不能嵌套使用。
```












# 运算符与操作符





## &和&&的区别

- &运算符有两种用法：(1)按位与；(2)逻辑与。
- &&运算符是短路与运算。逻辑与跟短路与的差别是非常巨大的，虽然二者都要求运算符左右两端的布尔值都是true 整个表达式的值才是 true。&&之所以称为短路运算，是因为如果&&左边的表达式的值是 false，右边的表达式会被直接短路掉，不会进行运算。



**| 和 ||，& 和 && 的区别**

我们将 **||** 和 **&&** 定义为逻辑运算符，而 **|** 和 **&** 定义为位运算符。

**&&** 如果两个操作数都非零，则条件为真；

**||** 如果两个操作数中有任意一个非零，则条件为真。

**&** 按位与操作，按二进制位进行"**与**"运算。运算规则：（有 0 则为 0）

```
0&0=0;   
0&1=0;    
1&0=0;     
1&1=1;
```

**|** 按位或运算符，按二进制位进行"**或**"运算。运算规则：（有 1 则为 1）

```
0|0=0;   
0|1=1;   
1|0=1;    
1|1=1;
```

**那么，问题来了，在判断语句中，用 | 还是 ||，& 还是 &&？**

判断语句中为布尔类型，值只有 true 和 false（如果变量值为 0 就是 false，否则为 true）

```
举个例子，a=1 b=2
所以 a>0 这个值为true    b>1 这个值为true     b>2 这个值为 false
如 if(a>0&b>1)  我们可以得出 if(true&true)，条件成立（true不为0，所以true&true不为0）
如 if(a>0&&b>1)  我们可以得出 if(true&&true),条件成立（&&两边操作数都非零，所以条件成立）
如 if(b>2&a>0) 我们可以得出 if(false&true)，条件不成立（false为0，false&true为0，条件不成立）
如 if(b>2&&a>0) 我们可以得出 if(false&&a>0)，条件不成立（&&左侧为false，&&运算到此结束，条件不成立）
```

可以看出 & 和 && 在判断语句中都可以实现“和”这个功能，不过区别在于 & 两边都运算，而 && 先算 && 左侧，若左侧为 false 那么右侧就不运算了。因此从效率上来说，判断语句中推荐使用 &&（换句话就是逻辑运算就老老实实用逻辑运算符，不然它为啥叫逻辑运算符呢？）

而 | 和 || 的比较与上类似，不做赘述。





举例

String s=null;

下面哪个代码片段可能会抛出NullPointerException？

```
if((s!=null)&(s.length()>0))
if((s!=null)&&(s.length()>0))
if((s==null)|(s.length()==0))
if((s==null)||(s.length()==0))
```

s为null，因此只要调用了s.length()都会抛出空指针异常。因此这个题目就是考察if语句的后半部分会不会执行。
A，单个与操作的符号& 用在整数上是按位与，用在布尔型变量上跟&&功能类似，但是区别是无论前面是否为真，后面必定执行，因此抛出异常
B，与操作，前半部分判断为假，后面不再执行
C，这里跟 & 和&& 的区别类似，后面必定执行，因此抛出异常
D，或语句，前面为真，整个结果必定为真，后面不执行

AC














## ==>>==和==>>>==



\>> 代表带符号右位移，当操作的数为负数时，右移后，高位补1

当操作的数为整数时，右移后，高位补0 

  \>>> 代表无符号右位移，不管正负数，右移后，高位都是补0 



    public class Test {
    public static void main(String args[]) {
        int x, y;
        x = 5 >> 2;
        y = x >>> 2;
        System.out.println(y);
    }
    }


  此处5>>2， 5的二进制为 0000 0000 0000 0101 ,右移2位，即： 0000 0000 0000 0001 高位补0 

  x>>>2, 即上面的结果，再右移2位，结果就成了 0000 0000 0000 0000，所以最终的结果是0



**无符号位移例子：**

**2 << 3（左移 3 位相当于乘以 2 的 3 次方，右移 3 位相当于除以 2 的 3 次方）**













## ~ 按位取反



数在计算机中是以补码形式存在的。

数>0,补码 == 反码;

数<0，补码==反码+1.（注意符号位）

举例

```
int i = 5;
int j = 10;
System.out.println(i + ~j);
```



10原码：0000000000000000,0000000000001010； 

  ~10： 1111111111111111,1111111111110101  变为负数，计算机用补码存储 

  ~10反码：10000000000000000,0000000000001010 

  ~10补码：10000000000000000,0000000000001011，等于 -11





**快速做法：**

<img src="java面试.assets/image-20220908112316243-16626073986501.png" alt="image-20220908112316243" style="zoom:67%;" />











 

## floor

floor是向下取整，返回double类型

字面意思





## ceil

ceil是向上取整，返回double类型



字面意思



## round

round是四舍五入：可以理解为：原数据加上0.5，再进行floor，向下取整。



比如：Math.round(2.3) 就是Math.floor(2.8)，即：2.0

比如：Math.round(-2.6) 就是Math.floor(-2.1)，即：-3.0





举例

Math.floor(-8.5)=( ）

```
(double)-9.0
```

返回小于等于X的double类型











##  instanceof

它的作用是什么？在使用过程中注意事项有哪些？它底层原理是如何实现的

它的作用是什么？

- instanceof是Java的一个二元操作符，和==，>，<是同一类东西。由于它是由字母组成的，所以也是Java的保留关键字。

- 它的作用是测试它左边的对象是否是它右边的类的实例，返回boolean类型的数据。

  

使用过程中注意事项有哪些？

- 类的实例包含本身的实例，以及所有直接或间接子类的实例
- instanceof左边显式**声明的类型**与右边操作元必须是同种类或存在继承关系，也就是说需要位于同一个继承树，否则会编译错误

```java
//比如下面就会编译错误
String s = null;
s instanceof null
s instanceof Integer
```













#                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        堆和栈

<img src="java面试.assets/image-20220821153453288-16610672981568.png" alt="image-20220821153453288" style="zoom:67%;" />

 

## 堆和栈的区别

### **功能不同**

 栈内存用来存储**局部变量和方法调用**。

 而堆内存用来存储Java中的对象。无论是成员变量，局部变量，还是类变量，它们指向的对象都存储在堆内存中。



###  **共享性不同**

栈内存是**线程私有的**。

 堆内存是所有线**程共有的**。



### **异常错误不同如果栈内存或者堆内存不足都会抛出异常。**

栈空间不足：**java.lang.StackOverFlowError**。

 堆空间不足：**java.lang.OutOfMemoryError**。



###  **空间大小**

**栈的空间大小远远小于堆的。**

 

 

 

 

# 修饰符

## **修饰符的范围及其区别**

**public＞protected＞default＞private**

**default 也称为friendly和包权限**



java中有四大修饰符，分别为private,default,protected,public,下面主要是四者之间的区别：





## **private(私有的)**

private可以修饰成员变量，成员方法，构造方法，不能修饰类(**此刻指的是外部类，内部类不加以考虑，**一般不会用来修饰类，除非是内部类。**)。**



**被private修饰的成员只能在其修饰的本类中访问，在其他类中不能调用，但是被private修饰的成员可以通过set和get方法向外界提供访问方式**

 





## **default(默认的)**

defalut即不写任何关键字，它可以修饰类，成员变量，成员方法，构造方法。被默认权限修饰后，其只能被本类以及同包下的其他类访问。





 

## **protected(受保护的)**

protected可以修饰成员变量，成员方法，构造方法，但不能修饰类(此处指的是外部类，内部类不加以考虑，可以修饰内部类)。被protected修饰后，只能被同包下的其他类访问。如果不同包下的类要访问被protected修饰的成员，这个类必须是其子类。



 

## **public(公共的)**

public是权限最大的修饰符，他可以修饰类，成员变量，成员方法，构造方法。被public修饰后，可以再任何一个类中，不管同不同包，任意使用。

 

 

|  作用域   | 当前类 | 同一包 | 其他包的子孙类 | 其他包的类 |
| :-------: | :----: | :----: | :------------: | :--------: |
|  public   |   √    |   √    |       √        |     √      |
| protected |   √    |   √    |       √        |     ×      |
| friendly  |   √    |   √    |       ×        |     ×      |
|  private  |   √    |   ×    |       ×        |     ×      |





   

## 外部类和内部类（浅讲）



- **外部类**：这是一个相对内部类的概念，如果一个类中嵌套了另外一个类，我们就把这个类叫做外部类。

- **内部类**：顾名思义，就是定义在里边的那个类。 内部类可以作用在方法里以及外部类里，作用在方法里称为局部内部类，作用在外部类里分为实例内部类和静态内部类。

  

### 内部类与外部类的互访

**外部类**

- **外部类只有两种访问控制符，即public和default（包访问控制级别）**

  <img src="java面试.assets/image-20220907224737357-16625620589503.png" alt="image-20220907224737357" style="zoom:80%;" />

  

  原因：外部类的上一级程序单元是包，所以它只有两种作用域：同一个包内和任何位置，这样只需要用public和default即可，用public修饰的类可以被任何位置的其他类访问，而不添加访问控制符的类的访问控制权限为包访问级别，即该类只能被同一个包的其他类访问。

  

  

  **内部类：**

  **内部类有四种访问控制符**（public,private,protected），因为内部类实际上就是外部类的一个成员，所以内部类的上一级程序单元为类，因此它有四种作用域：同一个类、同一个包、父子类和任何位置，因此可以使用四种访问控制权限。

  <img src="java面试.assets/image-20220907224923109-16625621652985.png" alt="image-20220907224923109" style="zoom:67%;" />

  





因为局部成员的作用域是所在方法，其他程序单元永远不可能访问另一个方法中的局部变量，所以所有的局部成员都不能使用访问控制修饰符修饰

![image-20220907225640472](java面试.assets/image-20220907225640472-16625626018097.png)

**内部类分为：匿名内部类 ，静态内部类，成员内部类，局部内部类，**

其中成员内部类，静态内部类他们的访问权限修饰词都是和普通成员一样，具有public,protected,缺省,private四种。

当然，你既然都定义成内部类了，就私有化得了，不就是为了不想被人家用嘛  局部内部类，是和局部变量一样，没有访问权限修饰词，因为它只在局部作用域生效，根本没有必要加访问权限



**综上外部类只能被public 默认修饰**

**内部类分情况**

**匿名内部类 不能被修饰符修饰**

**其他的内部类 可以被四种修饰符修饰**

同一个Java源文件中，可以有多个类（class）,但是只能有一个public class.























# java对象

## 创建对象的方式有几种

Java有5种方式来创建对象：

 **1、使用 new 关键字（最常用）：**

```
ObjectName obj = new ObjectName();`
```



 **2、使用反射的Class类的newInstance()方法**：

```
 ObjectName obj = ObjectName.class.newInstance(); 
```



**3、使用反射的Constructor类的newInstance()方法：**

```
 `ObjectName obj = ObjectName.class.getConstructor.newInstance();` 
```



**4、使用对象克隆clone()方法：** 

```
ObjectName obj = obj.clone(); 
```





**5、使用反序列化（ObjectInputStream）的readObject()方法：**

```
 try (
 ObjectInputStream ois = new ObjectInputStream(new FileInputStream(FILE_NAME))) 
 {
 ObjectName obj = ois.readObject();
 }
```

 







## **面向对象和面向过程的区别**

两者的主要区别在于解决问题的方式不同：

 面向过程把解决问题的过程拆成一个个方法，通过一个个方法的执行解决问题。

 面向对象会先抽象出对象，然后用对象执行方法的方式解决问题。

另外，面向对象开发的程序一般更易维护、易复用、易扩展。

 

 



## 面向对象三大特征





### **封装**

封装是指把一个对象的状态信息（也就是属性）隐藏在对象内部，不允许外部对象直接访问对象的内部信息。但是可以提供一些可以被外界访问的方法来操作属性。

```
public class Student {

  private int id;//id属性私有化

  private String name;//name属性私有化

 

  //获取id的方法

  public int getId() {

    return id;

  }

  //设置id的方法

  public void setId(int id) {

   this.id = id;

  }

  //获取name的方法

  public String getName() {

    return name;

  }

  //设置name的方法

  public void setName(String name) {

   this.name = name;

  }

}
```

 









### **继承**

不同类型的对象，相互之间经常有一定数量的共同点。



继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。通过使用继承，可以快速地创建新的类，可以提高代码的重用，程序的可维护性，节省大量创建新类的时间 ，提高我们的开发效率。

**关于继承如下 3 点请记住：**

1. 子类拥有父类对象所有的属性和方法（包括私有属性和私有方法），但是父类中的私有属性和方法 子类是无法访问，只是拥有。

2. 子类可以拥有自己属性和方法，即子类可以对父类进行扩展。

3. 子类可以用自己的方式实现父类的方法。

   

    注意：

   **静态方法：**

   

   **父类的静态方法能够被子类继承，但是不能够被子类重写，即使子类中的静态方法与父类中的静态方法完全一样，也是两个完全不同的方法。**

   

   

   

   **举例**

   ```
   public class parent{
      public static void printA() {
         System.out.println("父类静态方法");
      }
   
      public void printB() {
         System.out.println("父类普通方法");
      }  
   }
   
   
   
   
   
   class child extends parent{
      public static void printA() {
         System.out.println("子类静态方法");
      }
   
      public void printB() {
         System.out.println("子类普通方法");
      }  
   }
   ```

   

   

   测试1：

   ```
   public class Test {
      public static void main(String args[]) {
         child child1=new child();
         child1.printA();
         child1.printB();
      }
   }
   
   子类静态方法
   子类普通方法
   ```

   测试2：

   ```
   public class Test {
      public static void main(String args[]) {
         parent child1=new child();
         child1.printA();
         child1.printB();
      ;
      }
   }
   子类静态方法
   子类普通方法
   ```

   这里看到printA方法输出的是“父类静态方法1”，说明printA方法并没有被覆盖。加上重写的注释看一下，直接报错。

   <img src="java面试.assets/image-20220911131448664-16628732909065.png" alt="image-20220911131448664" style="zoom:67%;" />

   ![image-20220911131749893](java面试.assets/image-20220911131749893-16628734731407.png)

   普通方法应为被覆盖了所以加上重写注解不报错

   

   实际上Java里不管是static方法还是final方法不是不能被覆盖的，那为什么在子类写一个和父类同名的静态方法不会报错，而写一个同名的final方法就报错？

   

   其实final修饰的不管是普通方法还是静态方法，子类中都不允许由同名的方法，这是规定。那子类里的为什么可以有重名的静态方法，可以把它理解为重新定义，静态方法是在类加载时就和类绑定在一起，是静态绑定，子类有同名的静态方法，就是在加载子类的同名静态方法时重新分配一块空间，和父类的静态方法没有任何关系

   

   所以在使用多态的时候，父类调用静态方法实际调用的是自己的静态方法，而调用普通方法的时候，普通方法已经被覆盖直接调用的是子类方法

   

   

    

### **多态**

多态，顾名思义，表示一个对象具有多种的状态，具体表现为父类的引用指向子类的实例。



**多态的特点:**

 对象类型和引用类型之间具有继承（类）/实现（接口）的关系；

 引用类型变量发出的方法调用的到底是哪个类中的方法，必须在程序运行期间才能确定；

 多态不能调用“只在子类存在但在父类不存在”的方法；

 如果子类重写了父类的方法，真正执行的是子类覆盖的方法，如果子类没有覆盖父类的方法，执行的是父类的方法。

 

父类：

```
public class Parent {

    public void Test(){
        System.out.println("这是父类的普通方法");
    }
    public static void  Test1(){
        System.out.println("这是父类的静态方法");
    }
    
     public void ParentMethod(){
        System.out.println("这是父类中独有的方法");
    }


}
```

 子类

```
public class Child extends Parent{

    @Override
    public void Test() {
        System.out.println("这是子类的普通方法");
    }
    public static void Test1()
    {
        System.out.println("这是子类的静态方法");
    }

    public void ChildMethod(){
        System.out.println("这是子类独有的方法");
    }
}
```

测试

```
public class Test {
    public static void main(String[] args) {
        Parent child=new Child();
        child.Test();
        child.Test1();
        Child.Test1();
        child.ParentMethod();

    }
}
这是子类的普通方法
这是父类的静态方法
这是子类的静态方法
这是父类中独有的方法

```

![image-20220912142021446](java面试.assets/image-20220912142021446-16629636242611.png)

**多态不能调用“只在子类存在但在父类不存在”的方法；**如果子类中没有重写父类中的方法，使用多态的对象来调用方法，只能调用父类的方法





 

















# Java常见类与内部类

## Object

Object 类的常见方法有哪些？

Object 类是一个特殊的类，是所有类的父类。它主要提供了以下 11 个方法：



 

```
/*
 * native 方法，用于返回当前运行时对象的 Class 对象，使用了 final 关键字修饰，故不允许子类重写。
 */
public final native Class<?> getClass()
/**



 * native 方法，用于返回对象的哈希码，主要使用在哈希表中，比如 JDK 中的HashMap。
 */
public native int hashCode()
/**


 * 用于比较 2 个对象的内存地址是否相等，String 类对该方法进行了重写以用于比较字符串的值是否相等。
 */
public boolean equals(Object obj)
/**


 * naitive 方法，用于创建并返回当前对象的一份拷贝。
 */
protected native Object clone() throws CloneNotSupportedException


/**
 * 返回类的名字实例的哈希码的 16 进制的字符串。建议 Object 所有的子类都重写这个方法。
 */
public String toString()



/**
 * native 方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。
 */
public final native void notify()



/**
 * native 方法，并且不能重写。跟 notify 一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。
 */
public final native void notifyAll()



/**
 * native方法，并且不能重写。暂停线程的执行。注意：sleep 方法没有释放锁，而 wait 方法释放了锁 ，timeout 是等待时间。
 */
public final native void wait(long timeout) throws InterruptedException



/**
 * 多了 nanos 参数，这个参数表示额外时间（以毫微秒为单位，范围是 0-999999）。 所以超时的时间还需要加上 nanos 毫秒。。
 */
public final void wait(long timeout, int nanos) throws InterruptedException



/**
 * 跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念
 */
public final void wait() throws InterruptedException



/**
 * 实例被垃圾回收器回收的时候触发的操作
 */
protected void finalize() throws Throwable { }
```

 













## String

### **String特点**

 String:字符串，使用一对“ ”引起来表示。



- **String声明为final的，不可被继承**

- String实现了Serializable接口：表示字符串是支持序列化的。

- **实现了Comparable接口：表示String可以比较大小**

- String内部定义了final char[] value用于存储字符串数据

- **String:代表不可变的字符序列。简称：不可变性。**

  

  体现：1.**当对字符串重新赋值时，需要重写指定内存区域赋值，不能使用原有的value进行赋值**。          

  ​          2.**当对现有的字符串进行连接操作时，也需要重新指定内存区域赋值，不能使用原有的value进行赋值**。

  ​          3.**当调用String的replace()方法修改指定字符或字符串时，也需要重新指定内存区域赋值，不能使用原有的value进行赋值。**

- 通过字面量的方式（区别于new）给一个字符串赋值，此时的字符串值声明在字符串常量池中。

- **字符串常量池中是不会存储相同内容的字符串的。**（不可变性）







举例

前提：使用==比较的String等引用类型的时候比较的是其指向的地址（下文会提到）

```
  @Test
    public void Test1(){

        String s1="abc";
        String s2="abc";
        System.out.println(s1==s2);//true
```

s1 s2地址都指向常量池abc，地址都相同所以返回true（String常量池是不会存储相同内容的字符串的---不可变性）





```
@Test
public void Test1(){

    String s1="abc";
    String s2="abc";
    s1="hello";
    System.out.println(s1==s2);
}
```

s1常量池更新（重新赋值），s2地址仍然指向abc，s1 s2两者指向的地址不同。返回false







```
@Test
public void Test1(){
    String s2="abc";
    String s3="abc";
    s3+="def";
    System.out.println(s3==s2);

}
```

s3进行字符串拼接，常量池更新（重新赋值），s2，s3所指向的地址不同





```
@Test
public void Test1(){
    String s4="abc";
    String s5=s4.replace('a','l');
    System.out.println(s5);
    System.out.println(s4==s5);


}
```

replace替换 s4,s5所指向的地址值不同



<img src="java面试.assets/image-20220822113511125.png" alt="image-20220822113511125" style="zoom:50%;" /><img src="java面试.assets/image-20220822113550732-16611393546695.png" alt="image-20220822113550732" style="zoom:50%;" />



















### **String的实例化方式**

​     \* 方式一：通过字面量定义的方式

​     \* 方式二：通过new + 构造器的方式



```
public class Test1 {

    public static void main(String[] args) {
//    通过字面量方式定义 此时的s1和s2的数据声明在方法去的字符串常量池中
    String s1="java";
    String s2="java";




//    new＋构造器方式 此时的s3 s4 保存的地址值 是数据在堆空间中开辟空间对应的地址值
        String s3=new String("java");

        String s4=new String("java");


        System.out.println(s1==s2);//true
        System.out.println(s1==s3);//false
        System.out.println(s1==s4);//false
        System.out.println(s3==s4);//false

        System.out.println("********************************************");
        Person p1=new Person("java",12);
        Person p2 =new Person("java",12);
//        p1 p2 的name String类型是字面量生成 在方法区的字符串常量池中 相同得共享一个不能改变（String的不可变性）
        System.out.println(p1.name.equals(p2.name));//true
        System.out.println(p1.name==p2.name);//true

    }

```









<img src="java面试.assets/image-20220822133402575-16611464442857.png" alt="image-20220822133402575" style="zoom: 50%;" />



相当于 p1 p2都指向了"tom"的常量池。应为String的不可变性（常量池不会存储相同的字符串）所以二者地址都指向同一个









**练习**



```
public class Example{
    String str=new String("tarena");
    char[]ch={'a','b','c'};
    public static void main(String args[]){
        Example ex=new Example();
        ex.change(ex.str,ex.ch);
        System.out.print(ex.str+" and ");
        System.out.print(ex.ch);
    }
    public void change(String str,char ch[]){
   //引用类型变量，传递的是地址，属于引用传递。
        str="test ok";
        ch[0]='g';
    }
}
```



输出：

tarena and gbc





我们需要了解到：

（1）**基本数据类型传值，对形参的修改不会影响实参**； （2）**引用类型传引用，形参和实参指向同一个内存地址**（同一个对象），所以对参数的修改会影响到实际的对象； （3）**String, Integer, Double等包装类有不可变性**（包装类知识点具体看下文基本数据类型篇）





我们创建了String对象str，指向了test ok 地址，由于String的不可变性，后续无论对str进行如何操作其指向的地址都不变，对他的改变如果是直接赋值就是指向jvm的常量池的另一个地址

如果是new的话  都是在堆中创建了一个新的对象，并不是原来的对象。（原来的对象被垃圾回收了）



所以在change方法中，str保持不变，但是char类型数组会发生改变（因为是引用数据类型，实参和形参指向的都是同一个地址，形参改变实参也会改变，即使方法结束被销毁了也是如此），变成gbc，待change方法执行完毕，str的引用会随方法的结束也一起消失（局部变量）

<img src="java面试.assets/image-20220829232812631-16617868942355.png" alt="image-20220829232812631" style="zoom: 67%;" />

















### 为什么string字符串的值是不可变的？

当我们new一个字符串，给它赋值之后，那么当前对象的值就固定了，永远不会改变。

比如String str=new String("test")，那么str的值就是test，这是因为在String源码当中是用char数组来按顺序存储字符串中的每一个字符的，并且这个char数组是用final修饰的，这意味着一旦我们给字符串赋值之后，这个对象的值就永远不会改变。



可是当我们在一个类当中的某个方法里面，给这个对象str赋值了一个新的字符串，它这时候的值是多少呢？比如这时str="good"，str的值就是good，(你可以在这个方法里面写输出语句，输出这个引用，就知道怎么回事了)

可不是说引用的值不可以改变么？



这里改变的不是引用的值，而是引用str指向的常量不一样了而已，而这个引用的生命周期和当前方法的一样的，也就是方法结束，引用被杀死，也结束了，那么它刚才指向good的这个引用，就结束了，所以在这个方法结束之后，再输出引用str的值，自然就是引用str之前指向的值了，也就是test。



（还是上面那个例子）往change方法中添加一个输出语句

```
 public void change(String str,char ch[]){
//引用类型变量，传递的是地址，属于引用传递。
     str="test ok";
     ch[0]='g';
     System.out.println(str);
 }
```



结果



test ok
tarena and gbc













穿插一个值传递和引用传递的思考

### 值传递和引用传递



#### 堆和栈

堆栈是指内存的两种组织形式，堆是指动态分配内存的一块区域，一般由程序员手动分配，比如 Java 中的 `new`、C/C++ 中的 `malloc` 等，都是将创建的对象或者内存块放置在堆区。

而栈是则是由编译器自动分配释放（大概就是你申明一个变量就分配一块相应大小的内存），用于存放函数的参数值，局部变量等。

就拿 Java 来说，基本类型（int、double、long这种）是直接将存储在栈上的，而引用类型（类）则是值存储在堆上，栈上只存储一个对对象的引用（其实这里只是特定了局部变量）



![image-20220912152900582](java面试.assets/image-20220912152900582-16629677420973.png)



```java
public class DemoTest {

    int y;// 分布在堆上
    public static void main(String[] args) {

        int x=1; //分配在栈上
        String name=new String("cat");//数据在堆上，name变量的指针在栈上
        String address="北京";//数据在常量池，属于堆空间，指针在栈
        Integer price=4;//包装类型同样是引用类型，编译时会自动装拆相，所以数据在堆上，指针在栈
    }


}
```





```
public class Test11 {

  public void Test(){
        String name=new String("石晨霖");
        int age=18;
    }

   
}
```



<img src="java面试.assets/无标题流程图(2).drawio-16618255758248.png" alt="无标题流程图(2).drawio" style="zoom: 50%;" />





如果，我们分别对`age`、`name`变量赋值，会发生什么呢？

```text
age = 22;
name = new String("胖虎");
```





<img src="java面试.assets/无标题流程图(2).drawio-166182600427010-166182600565012.png" alt="无标题流程图(2).drawio" style="zoom:67%;" />





`age` 仅仅是将栈上的值修改为 22，而 `name` 由于是 String 引用类型，所以会重新创建一个 String 对象，并且修改 `name`，让其指向新的堆对象。（细心的话，你会发现，图中 name 执行的地址我做了修改）

然后，之前那个对象如果没有其它变量引用的话，就会被垃圾回收器回收掉。

这里也要注意一点，我创建 String 的时候，使用的是 new，如果直接采用字符串赋值，比如：

```text
String name = "静香"
```

那么是会放到 JVM 的常量池去，不会被回收掉，这是字符串两种创建对象的区别，上面String篇已经提到









#### **函数调用栈**

一个函数需要在内存上存储哪些信息呢？

参数、局部变量，理论上这两个就够了，但是当多个函数相互调用的时候，就还需要机制来保证它们顺利的返回和恢复主调函数的栈结构信息。

那这部分就包括返回地址、`ebp`寄存器（基址指针寄存器，指向当前堆栈底部） 以及其它需要保存的寄存器。

所以一个完整的函数调用栈大概长得像下面这个样子：



<img src="java面试.assets/image-20220830102306776-166182618895514.png" alt="image-20220830102306776" style="zoom: 33%;" />











我们再举上面的例子

```
public class Test11 {

    String name=new String("石晨霖");
    int age=18;
    char []str={'a','c','l'};


    public void Test(String name,int age){
        name="胖虎";
        age=20;
        str[0]='s';
    }

    public static void main(String[] args) {
        Test11 test11=new Test11();
        test11.Test(test11.name,test11.age);
        System.out.println(test11.name);
        System.out.println(test11.age);
        System.out.println(test11.str);
    }
}
```

输出：石晨霖 18 scl



<img src="java面试.assets/image-20220830104245636-166182736734120.png" alt="image-20220830104245636" style="zoom:67%;" />



将 `main` 函数内变量 `age  的值拷贝到  Test` 函数参数 age  位置。

将变量 name 的地址，拷贝到 Test  函数参数  age  的位置。

记住这张图，这是函数参数传递的本质，没有其它方式，just copy！

copy 意味着是副本，也就是在子函数的参数永远是主调函数内的副本。

决定是值传递还是所谓的引用传递，在于你 copy 的到底是一个值，还是一个引用（的值）。



<img src="java面试.assets/无标题流程图(2).drawio-166182810745522-166182810983724.png" alt="无标题流程图(2).drawio" style="zoom: 50%;" />









***形参*变量只有在被调用时才分配内存单元，在调用*结束*时， 即刻释放所分配的内存单元。这也就解释了上文提到的**

**基本数据类型传值，对形参的修改不会影响实参； （2）引用类型传引用，形参和实参指向同一个内存地址（同一个对象），所以对参数的修改会影响到实际的对象； （3）String, Integer, Double等包装类有不可变性**



str同理，是char类型数组（引用数据类型），一开始str指向的是“acl”地址，但是在Test方法中str[0]=‘s’,堆中创建了新的对象，地址指向了新的对象，发生了改变，str=‘scl’

即使内存释放，原来main函数的实参str指向的对象也发生了改变，而基本数据类型的形参int age，则随着内存的释放而消失，最后输出的还是main函数中的实参的值。













### String的常用方法

```
String的常用方法
/**
* int length()：返回字符串的长度：return value.length

* char charAt(int index)：返回某索引处的字符return value[index]

* boolean isEmpty()：判断是否是空字符串：return value.length==0

* String toLowerCase()：使用默认语言环境，将String中的所有字符转换为小写

* String toUpperCase()：使用默认语言环境，将String中的所有字符转换为大写

* String trim()：返回字符串的副本，忽略前导空白和尾部空白

* boolean equals(Object obj)：比较字符串的内容是否相同

* boolean equals IgnoreCase(String anotherString)：与equals方法类似，忽略大小写

* String concat(String str)：将指定字符串连接到此字符串的结尾。等价于用“+”

* int compareTo(String anotherString)：比较两个字符串的大小

* String substring(int beginIndex)：返回一个新的字符串，它是此字符串的从beginIndex开始截取到最后的一个子字符
串。

* String substring(int beginIndex,int endIndex)：返回一个新字符串，它是此字符串从beginIndex开始截取到
endIndex(不包含)的一个子字符串。
*/
```









## ValueOf



![image-20220917093841892](java面试.assets/image-20220917093841892-16633787231981.png)



<img src="java面试.assets/image-20220917094047442-16633788498013.png" alt="image-20220917094047442" style="zoom:67%;" />

返回值都是String



```
public class CharToString {
 public static void main(String[] args)
 {
  char myChar = 'g';
  String myStr = Character.toString(myChar);
  System.out.println("String is: "+myStr);
  myStr = String.valueOf(myChar);
  System.out.println("String is: "+myStr);
 }
}
```



输出 g g







## **StringBuffer和StringBuilder**



### **String StringBuilder StringBuffer的异同**



String ：不可变的字符序列  底层使用final 修饰char[]型数组存储

StringBuilder 可变的字符序列   jdk5.0线程不安全 效率高 底层使用char[]型数组存储

StringBuffer  可变的字符序列  线程安全 效率偏低  底层使用char[]型数组存储



#### 1、运行速度

**运行速度或者说执行速度，运行速度的快慢为：StringBuilder > StringBuffer > String**



String最慢的原因：

String为字符串常量，而StringBuilder和StringBuffer均为字符串变量，即String一旦创建之后，该对象是不可更改的，而StringBuilder和StringBuffer是变量，是可以更改的。下面以一段代码为例：

```
String str = "abc";
System.out.println(str);
str = str + "de";
System.out.println(str);
```

如果运行这段代码会发现：先输出 abc 然后再输出 abcde 。看起来好像 str 是被更改了，其实，这只是一种假象而已，JVM对于这几行代码是这样处理的：



首先创建一个 String 对象 str ，并把 abc 赋值给 str ，然后在第三行中，其实 JVM 又创建了一个新的对象也叫做 str ，然后把原来 str 的值和 de 加起来再赋值给新的 str ，而原来的 str 会被 JVM 的垃圾回收机制（GC）给回收掉了，所以，str 其实并没有被更改，也就是前面说的，String 对象一旦创建就不能够更改。



**所以，java 中对 String 对象进行的操作实际上是一个不断创建新的对象，并且将旧的对象回收的一个过程，所以执行速度很慢。**

而 StringBuilder 和 StringBuffer 的对象是变量，对变量进行操作就是直接对该对象进行更改，而不会进行创建和回收的操作，所以速度要比 String 快很多。

另外，有时候我们会这样对字符串进行赋值：



```
String str = "abc" + "de";
StringBuilder stringBuilder = new StringBuilder().append("abc").append("de");
System.out.println(str);
System.out.println(stringBuilder.toString());
```



这样输出的结果也是 abcde 和 abcde ，但是 String 的速度要比 StringBuilder 的反应速度快很多，这是因为第一行中的操作和 String str = "abcde"; 是完全一样的，所以会很快，而如果写成下面这种形式：

```
String str1 = "abc";
String str2 = "de";
String str = str1 + str2;

```

那么JVM就会像上面说的那样，不断的创建、回收对象来进行这个操作了。速度就会很慢。







#### 2、线程安全

在线程安全上：StringBuilder 是线程不安全的，StringBuffer 是线程安全的。

如果一个 StringBuffer 对象在字符串缓冲区被多个线程同时使用，StringBuffer 中很多方法可以带有 synchronized 关键字，所以可以保证线程是安全的，但 StringBuilder 的方法则没有该关键字，所以不能保证线程安全，有可能会出现一些错误的操作。所以，要进行的操作是多线程的，那么就要使用 StringBuffer ，但是在单线程的情况下，建议使用速度比较快的 StringBuilder 。





### **扩容问题** 

如果需要添加的数据底层数组放不下需要扩容底层的数组

  默认情况下扩容为原来的两倍+2 同时将原有数组中的元素复制到新的数组中

​    

​    



 源码分析

```
  String str=new String(); new char[0]

 String str=new String("abc"); new char[]{'a','b','c'}

 StringBuffer sb1=new StringBuffer(); //char[] value= new char[16] 相当于创建了一个长度为16的数组

 

 sb1.append('a');//value[0]='a'

 sb1.append('b')//value[1]='b'
```

 不同于String StringBuffer直接可以改变 而String需要重新声明

 StringBuffer sb1=new StringBuffer(“abc”); //char[] value= new char[“abc”。length+16] 相当于创建了abc的长度加上一个长度为16的数组

 







**当String对象进行"+"操作，编译时会将String类变为String Builder进行append()处理，而append()方法的功能就是字符串拼接**

![image-20220926223129670](java面试.assets/image-20220926223129670-16642026912545.png)









### StringBuffer常用方法

```
/**
 * StringBuffer的常用方法：
 *
 * StringBuffer append(xxx)：提供了很多的append()方法，用于进行字符串拼接
 * StringBuffer delete(int start,int end)：删除指定位置的内容
 * StringBuffer replace(int start, int end, String str)：把[start,end)位置替换为str
 * StringBuffer insert(int offset, xxx)：在指定位置插入xxx
 * StringBuffer reverse() ：把当前字符序列逆转
 * public int indexOf(String str)
 * public String substring(int start,int end):返回一个从start开始到end索引结束的左闭右开区间的子字符串
 * public int length()
 * public char charAt(int n )
 * public void setCharAt(int n ,char ch)
 *
 * 总结：
 *     增：append(xxx)
 *     删：delete(int start,int end)
 *     改：setCharAt(int n ,char ch) / replace(int start, int end, String str)
 *     查：charAt(int n )
 *     插：insert(int offset, xxx)
 *     长度：length();
 *     遍历：for() + charAt() / toString()
 *
 */

```







### 如何将字符串反转？



使用 StringBuilder 或者 stringBuffer 的 reverse() 方法。

示例代码：

```
 //StringBuffer reverse
 StringBuffer stringBuffer = new StringBuffer(); 
 stringBuffer. append("abcdefg"); 
 System. out. println(stringBuffer. reverse()); // gfedcba 
 // StringBuilder reverse 
 StringBuilder stringBuilder = new StringBuilder(); 
 stringBuilder. append("abcdefg");
 System. out. println(stringBuilder. reverse()); // gfedcba
```
















## equals

### == 和 equals() 的区别

**==** 对于基本类型和引用类型的作用效果是不同的：

**1 对于基本数据类型来说，== 比较的是值。**

*（byte,short,char,int,long,float,double,boolean）的变量”==”比较的是两个变量的值是否相等。*

*比如：*

int a = 3; 

int b = 3; 

*a==b;返回就是true*







**2 对于引用数据类型来说，== 比较的是对象的内存地址。**



对于引用类型如String,则比较的是该变量所指向的地址.

拿我们最常用的**String**型来举例：

比如：`String a = “abc”;`

`       String b = “abc”;`

在这种情况下 字符串直接赋值给变量，该字符串会进入到常量池中，当第一次将 “abc”赋值给a的时候，会去常量池中找看有没有”abc”这个字符串，如果有的话，就将a指向该字符串在常量池中的地址，如果没有则在常量池中创建，第二次赋值 将 “abc”赋值给b的时候同样去常量池中找”abc”这个字符串，然后将他的地址赋值给b.

所以我们在做 a==b操作的时候返回的为true

​	上文String中已经明确指出









再来看另一种情况：

String a = new String(“abc”) ;

String b = new String(“abc”);

这种情况下我们在做 a==b操作的时候返回的为false。



**上文String中已经提到（创建新的对象在堆中，二者地址不同）**



当我们使用 String a = new String(“abc”)创建一个字符串对象时在内存里面是这样分配空间的：

首先会去常量池中找”abc”

如果找到了再创建一个”abc”对象存到堆中，并且他的值指向堆中的”abc”

如果没有找到则先在常量池中创建”abc”，再在堆内存开辟一块内存空间创建”abc”，并且他的值指向堆中的”abc”。







**equals()** 不能用于判断基本数据类型的变量，只能用来判断两个对象是否相等。equals()方法存在于Object类中，而Object类是所有类的直接或间接父类，因此所有的类都有equals()方法。

Object 类 equals() 方法：

```
public boolean equals(Object obj) {

   return (this == obj);

}

```

 

可以看到在Object类的equals方法中也是用的”== ”来进行比较，所以在进行比较时它和”==”应该时等价的，但是为什么我们在做 字符串比较的时候 两者比较出来的结果不一样呢？

**原因就是 String类型对equals方法进行了重写**



我们来看源码：

```
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String aString = (String)anObject;
        if (coder() == aString.coder()) {
            return isLatin1() ? StringLatin1.equals(value, aString.value)
                              : StringUTF16.equals(value, aString.value);
        }
    }
    return false;
}
```



从源码我们可以看出，在String的equals方法中对字符串的字符进行了逐一比较如果都相同则返回true.所以对于String中的equals方法比较的是两个字符串的内容对于：

String a = new String(“abc”) ; 

String b = new String(“abc”);

由于a和b的内容相同，返回true.





测试

```
@Test
public void Test1(){

    String s4="abc";
    String s5="abc";
    System.out.println(s4==s5);//true
    System.out.println(s4.equals(s5));//true

    System.out.println("---------------------------------------");
    String s1="asc";
    String s2="sd";
    System.out.println(s1==s2);//false
    System.out.println(s1.equals(s2));//false

    System.out.println("-----------------------------------------");
    String s7=new String("abc");
    String s6=new String("abc");
    System.out.println(s6==s7);//true
    System.out.println(s6.equals(s7));//false



}
```







### equals() 方法存在两种使用情况：

**类没有重写 equals()方法** ：通过equals()比较该类的两个对象时，等价于通过“==”比较这两个对象，使用的默认是 Object类equals()方法。



**类重写了 equals()方法** ：一般我们都重写 equals()方法来比较两个对象中的属性是否相等；若它们的属性相等，则返回 true(比较其中的实体)。





举个例子

```
String a = new String("ab"); // a 为一个引用

String b = new String("ab"); // b为另一个引用,对象的内容一样

String aa = "ab"; // 放在常量池中

String bb = "ab"; // 从常量池中查找

System.out.println(aa == bb);// true

System.out.println(a == b);// false（新的对象 在堆中）

System.out.println(a.equals(b));// true （只比较对象）


```



**单纯比较字符串String的话**

1. 对于字符串的比较“==”比较的是两个字符串的地址

2. 对于字符串的比较 “equals”比较的是两个字符串的内容



 



举例：

```
public class Test {
    private String name = "abc";
    public static void main(String[] args) {
        Test test = new Test();
        Test testB = new Test();
        String result = test.equals(testB) + ",";
        result += test.name.equals(testB.name) + ",";
        result += test.name == testB.name;
        System.out.println(result);
    }
}


```

结果：false true true

 



首先test.equals(testB)，Test类并没有重写equals方法，那么在比较时，调用的依然是Object类的equals()方法，比较的是两个对象在内存中的地址，结果为false。 （两个不同的对象）



  test.name.equals(testB.name)比较name字符串是否相等，二者都是"abc"，结果返回true。 



  name成员变量并没有被static关键字修饰，那么每个Test类的对象被创建时，都会先执行private String name = "abc";（注意这里使用了直接赋值的方式，没有使用new关键字）



在test对象初始化时，JVM会先检查字符串常量池中是否存在该字符串的对象，此时字符串常量池中并不存在"abc"字符串对象，JVM会在常量池中创建这一对象。

当testB被创建时，同样会执行private String name = "abc";，那么此时JVM会在字符串常量池中找到test对象初始化时创建的"abc"字符串对象，那么JVM就会直接返回该字符串在常量池中的内存地址，而不会新建对象。

也就是说test.name和testB.name指向的内存是同一个，那么test.name == testB.name返回true。

本题正确选项为false，true，true



练习：

```
String a="a";
String b="b";
String c=a+b;
String d="a"+"b";
String f="ab";
String g=a+"b";
System.out.println(c);
System.out.println(c.equals(d));//True
System.out.println(c.equals(f));//True
System.out.println(d.equals(f));//True
System.out.println(g.equals(d));//True
System.out.println("-----------------------------");
System.out.println(c==d);//false
System.out.println(c==f);//false只要有一个变量，都看做变量，使用new创建对象，返回堆区地址
System.out.println(d==f);//true常量与常量的拼接，结果是常量，返回常量池中的地址；
System.out.println(g==f);//false
```















**关于equals这个方法，在String类中是被重写了的，可以先判断地址是否相同，再比较值是否相同**

**并不是所有类的equals都是比较值，例如object类的equals方法和==是一样的**























## HashCode

建议看集合篇详细介绍Hashcode，此处仅仅简单介绍





### hashCode() 有什么用？

hashCode() 的作用是获取哈希码（int 整数），也称为散列码。这个哈希码的作用是确定该对象在哈希表中的索引位置。

hashCode()定义在 JDK 的 Object 类中，这就意味着 Java 中的任何类都包含有 hashCode() 函数。另外需要注意的是： Object 的 hashCode() 方法是本地方法，也就是用 C 语言或 C++ 实现的，该方法通常用来将对象的内存地址转换为整数之后返回。

public native int hashCode();



 

散列表存储的是键值对(key-value)，它的特点是：**能根据“键”快速的检索出对应的“值”。这其中就利用到了散列码！（可以快速找到所需要的对象）*



### 为什么要有 hashCode？

我们以“HashSet 如何检查重复”为例子来说明为什么要有 hashCode？



当你把对象加入 HashSet 时，HashSet 会先计算对象的 hashCode 值来判断对象加入的位置，同时也会与其他已经加入的对象的 hashCode 值作比较，如果没有相符的 hashCode，HashSet 会假设对象没有重复出现。但是如果发现有相同 hashCode 值的对象，这时会调用 equals() 方法来检查 hashCode 相等的对象是否真的相同。如果两者相同，HashSet 就不会让其加入操作成功。如果不同的话，就会重新散列到其他位置。这样我们就大大减少了 equals 的次数，相应就大大提高了执行速度。

其实， hashCode() 和 equals()都是用于比较两个对象是否相等。



**那为什么 JDK 还要同时提供这两个方法呢？**

这是因为在一些容器（比如 HashMap、HashSet）中，有了 hashCode() 之后，判断元素是否在对应容器中的效率会更高（参考添加元素进HashSet的过程）！

我们在前面也提到了添加元素进HashSet的过程，如果 HashSet 在对比的时候，同样的 hashCode 有多个对象，它会继续使用 equals() 来判断是否真的相同。也就是说 hashCode 帮助我们大大缩小了查找成本。



**那为什么不只提供** **hashCode() 方法呢？**

这是因为两个对象的hashCode 值相等并不代表两个对象就相等。





**那为什么两个对象有相同的** **hashCode 值，它们也不一定是相等的？**

因为 hashCode() 所使用的哈希算法也许刚好会让多个对象传回相同的哈希值。越糟糕的哈希算法越容易碰撞，但这也与数据值域分布的特性有关（所谓哈希碰撞也就是指的是不同的对象得到相同的 hashCode )。

总结下来就是 ：

如果两个对象的hashCode 值相等，那这两个对象不一定相等（哈希碰撞）。

 如果两个对象的hashCode 值相等并且equals()方法也返回 true，我们才认为这两个对象相等。

 如果两个对象的hashCode 值不相等，我们就可以直接认为这两个对象不相等。



 

### **为什么重写 equals() 时必须重写 hashCode() 方法？**

因为两个相等的对象的 hashCode 值必须是相等。也就是说如果 equals 方法判断两个对象是相等的，那这两个对象的 hashCode 值也要相等。

如果重写 equals() 时没有重写 hashCode() 方法的话就可能会导致 equals 方法判断是相等的两个对象，hashCode 值却不相等。



**总结** ：

equals 方法判断两个对象是相等的，那这两个对象的 hashCode 值也要相等。

两个对象有相同的 hashCode 值，他们也不一定是相等的（哈希碰撞）。

 

 

 













## 内部类

把类定义在另一个类的内部，该类就被称为内部类。

举例：把类Inner定义在类Outer中，类Inner就被称为内部类。

```java
  class Outer {
      class Inner {
      }
  }
```

### 内部类的访问规则

​	1 **内部类可以直接访问外部类的成员，包括私有**

​	2 **外部类要想访问内部类成员，必须创建对象**



### **内部类的分类**

 **成员内部类**

 **局部内部类**

 **静态内部类**

 **匿名内部类**

 





####  **成员内部类**

> 成员内部类——就是位于外部类成员位置的类
> 特点：可以使用外部类中所有的成员变量和成员方法（包括private的）
>
> **创建对象时：**
>
> ```java
>   //成员内部类不是静态的：
>   外部类名.内部类名 对象名 = new 外部类名.new 内部类名();
>   
>   //成员内部类是静态的：
>   外部类名.内部类名 对象名 = new 外部类名.内部类名();  
> ```

成员内部类不是静态时：

```
public class StringTest {

    private String name="石晨霖";

    class  Inner{
        public  void Test(){
            System.out.println(name);
        }
    }

}
class  Test1{

    public static void main(String[] args) {
        StringTest.Inner si=new StringTest().new Inner();
        si.Test();
    }
}
```



#####  **成员内部类常见修饰符：**

###### **private**

如果我们的内部类不想轻易被任何人访问，可以选择使用private修饰内部类，这样我们就无法通过创建对象的方法来访问，想要访问只需要在外部类中定义一个public修饰的方法，间接调用（创建内部类对象）。

这样做的好处就是，我们可以在这个public方法中增加一些判断语句，起到数据安全的作用。



```
public class StringTest {

    static  private String name="石晨霖";

    private class  Inner{
        public void Test(){
            System.out.println("我是石晨霖");
        }
    }


    public void Method(){
        if (name=="石晨霖"){
            Inner inner=new Inner();
            inner.Test();
        }else {
            System.out.println("我不是石晨霖");
        }

    }

    public Inner getInner(){
        return  new Inner();
    }

    public static void main(String[] args) {
        StringTest test=new StringTest();
        StringTest.Inner inner=test.getInner();
        inner.Test();

    }


}
```





###### static

这种被 static 所修饰的内部类，按位置分，属于成员内部类，但也可以称作静态内部类，也常叫做嵌套内部类。具体内容我们在下面详细讲解。



```
public class StringTest {

    public String name="小夫";

    class Inner {
        public String name="胖虎";

        public void Test() {
            String name="大雄";
            System.out.println(name);
            System.out.println(this.name);
            System.out.println(StringTest.this.name);
        }

    }

    public static void main(String[] args) {
        StringTest.Inner inner=new StringTest().new Inner();
        inner.Test();
    }
```



大雄
胖虎
小夫







#### **局部内部类**

> 局部内部类——就是定义在一个方法或者一个作用域里面的类
> 特点：主要是作用域发生了变化，只能在自身所在方法和属性中被使用

格式：

```java
class Outer {
      public void method(){
          class Inner {
          }
      }
  }
```





```
public class StringTest {

    private String name="小夫";
    
        public void Test() {
           final String name1="大雄";
            class Inner{
                public void show(){
                    System.out.println(name);
                    System.out.println(name1);

                }
            }
            Inner inner=new Inner();
            inner.show();



        }
        
}
```



**为什么局部内部类访问局部变量必须加final修饰呢？**

因为**局部变量是随着方法的调用而调用**，**使用完毕就消失**，**而堆内存的数据并不会立即消失**。

所以，堆内存还是用该变量，而该变量已经没有了。**为了让该值还存在，就加final修饰。**

原因是，当我们使用final修饰变量后，堆内存直接存储的**是值**，而**不是变量名**。









#### **静态内部类**

> 我们所知道static是不能用来修饰类的,但是成员内部类可以看做外部类中的一个成员,所以可以用static修饰,这种用static修饰的内部类我们称作静态内部类,也称作嵌套内部类.
> 特点：不能使用外部类的非static成员变量和成员方法



**解释**：非静态内部类编译后会默认的保存一个指向外部类的引用，而静态类却没有。

**简单理解**：

即使没有外部类对象，也可以创建静态内部类对象，而外部类的非static成员必须依赖于对象的调用，静态成员则可以直接使用类调用，不必依赖于外部类的对象，所以静态内部类只能访问静态的外部属性和方法

涉及jvm的理解 static修饰的方法和变量只能通过类来访问，而普通变量和方法需要通过类加载后的对象实例化来访问。对象储存在堆中，当我们在运行程序的时候，类加载首先生成了类，也就是此时我们就可以运行静态方法，但是此时我们还没有对对象进行实例化，如果静态方法中存在非静态变量（需要对象来访问），我们对象都还没有创建如何访问呢？

所以static修饰的方法无法访问非静态的变量和方法，但非静态的却可以访问静态的，原因就是那个时候对象早已实例化完成了，直接通过类访问



```
public class StringTest {

    String name="scl";
    static String anothername="lcs";

    static  class  Inner{
        public void Test(){
            System.out.println(anothername);
//            System.out.println(name); 错误 静态内部类无法访问外部类中非静态变量和方法）

        }
    }
}
class Tests{
    public static void main(String[] args) {
        StringTest.Inner inner=new StringTest.Inner();
        inner.Test();
    }
}
```











####  **匿名内部类**

> 一个没有名字的类，是内部类的简化写法

**格式：**

```java
  new 类名或者接口名() {
      重写方法();
  }
```

本质：其实是继承该类或者实现接口的子类匿名对象

这也就是下例中，可以直接使用 new Inner() {}.show(); 的原因 == **子类**对象.show();

```java
  interface Inner {
      public abstract void show();
  }
  
  class Outer {
      public void method(){
          new Inner() {
              public void show() {
                  System.out.println("HelloWorld");
              }
          }.show();
      }
  }
  
  class Test {
      public static void main(String[] args)  {
          Outer o = new Outer();
          o.method();
      }
  }    
```



如果匿名内部类中有多个方法又该如何调用呢？

```java
  Inter i = new Inner() {  //多态，因为new Inner(){}代表的是接口的子类对象
      public void show() {
      System.out.println("HelloWorld");
      }
  };
```



**匿名内部类在开发中的使用**

我们在开发的时候，会看到[抽象类](https://www.zhihu.com/search?q=抽象类&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A708467570})，或者接口作为参数。

而这个时候，实际需要的是一个子类对象。

如果该方法仅仅调用一次，我们就可以使用匿名内部类的格式简化。







匿名内部类：

1.必须继承一个类或者实现一个接口，而且只能继承一个类和实现一个接口；

2.匿名内部类，没有类名称，无构造函数；

3.匿名内部类中不能有静态变量和静态方法









#### 局部内部类和匿名内部类访问局部变量的时候，为什么变量必须要加上final？

- 局部内部类和匿名内部类访问局部变量的时候，为什么变量必须要加上final呢？它内部原理是什么呢？先看这段代码：

  ```csharp
  public class Outer {
  
      void outMethod(){
          final int a =10;
          class Inner {
              void innerMethod(){
                  System.out.println(a);
              }
          }
      }
  }
  复制代码
  ```

- 以上例子，为什么要加final呢？是因为**生命周期不一致**， 局部变量直接存储在栈中，当方法执行结束后，非final的局部变量就被销毁。而局部内部类对局部变量的引用依然存在，如果局部内部类要调用局部变量时，就会出错。加了final，可以确保局部内部类使用的变量与外层的局部变量区分开，解决了这个问题。








#### 内部类的优点

```
我们为什么要使用内部类呢？因为它有以下优点：
```

- 一个内部类对象可以访问创建它的外部类对象的内容，包括私有数据！
- 内部类不为同一包的其他类所见，具有很好的封装性；
- 内部类有效实现了“多重继承”，优化 java 单继承的缺陷。
- 匿名内部类可以很方便的定义回调。






#### 内部类有哪些应用场景

1. 一些多算法场合
2. 解决一些非面向对象的语句块。
3. 适当使用内部类，使得代码更加灵活和富有扩展性。
4. 当某个类除了它的外部类，不再被其他的类使用时



















# 变量

**定义在类中的变量是类的成员变量，可以不进行初始化，Java会自动进行初始化，如果是引用类型默认初始化为null,如果是基本类型例如int则会默认初始化为0**

**局部变量是定义在方法中的变量，必须要进行初始化，否则不同通过编译**

**被static关键字修饰的变量是静态的，静态变量随着类的加载而加载，所以也被称为类变量**

**被final修饰的变量是常量**

 





## 成员变量与局部变量的区别

**语法形式** ：

从语法形式上看，**成员变量是属于类的**，而局部变量是在**代码块或方法中定义的变量或是方法的参数**；

成员变量可以被 **public,private,static** 等修饰符所修饰，而局部变量不能被**访问控制修饰符及 static** 所修饰；

但是，成员变量和局部变量都能被 **final** 所修饰。



**存储方式** ：

从变量在内存中的存储方式来看,如果成员变量是使用 static 修饰的，那么这个成员变量是属于类的，如果没有使用 static 修饰，这个成员变量是属于实例的。而对象存在于堆内存，局部变量则存在于栈内存。





**生存时间** ：

从变量在内存中的生存时间上看，成员变量是对象的一部分，它随着对象的创建而存在，而局部变量随着方法的调用而自动生成，随着方法的调用结束而消亡。



**默认值** ：

从变量是否有默认值来看，成员变量如果没有被赋初始值，则会自动以类型的默认值而赋值（一种情况例外:被 final 修饰的成员变量也必须显式地赋值），而局部变量则不会自动赋值。



**存储位置**

- 成员变量：随着对象的创建而存在，随着对象的消失而消失，存储在堆内存中。
- 局部变量：在方法被调用，或者语句被执行的时候存在，存储在栈内存中。当方法调用完，或者语句结束后，就自动释放。



**作用域**

- 成员变量：针对整个类有效。
- 局部变量：只在某个范围内有效。(一般指的就是方法,语句体内)

 

 **初始值**

- 成员变量：有默认初始值。
- 局部变量：没有默认初始值，使用前必须赋值。

 









## **创建一个对象用什么运算符?对象实体与对象引用有何不同?**

new 运算符，new 创建对象实例（对象实例在堆内存中），对象引用指向对象实例（对象引用存放在栈内存中）。

一个对象引用可以指向 0 个或 1 个对象（一根绳子可以不系气球，也可以系一个气球）;一个对象可以有 n 个引用指向它（可以用 n 条绳子系住一个气球）。













## **对象的相等和引用相等的区别**

对象的相等一般比较的是内存中存放的内容是否相等。

引用相等一般比较的是他们指向的内存地址是否相等

 类似上文提到的== 与String 在引用数据类型中的比较



 
















# 常量

## 字符型常量和字符串常量的区别?

1. 形式 : 字符常量是单引号引起的一个字符，字符串常量是双引号引起的 0 个或若干个字符。

2. 含义 : 字符常量相当于一个整型值( ASCII 值),可以参加表达式运算; 字符串常量代表一个地址值(该字符串在内存中存放位置)。

3. 占内存大小 ： 字符常量只占 2 个字节; 字符串常量占若干个字节。

(**注意：** **char 在 Java 中占两个字节**)

















 

# 标识符和关键字

## 标识符和关键字的区别是什么？

在我们编写程序的时候，需要大量地为程序、类、变量、方法等取名字，于是就有了 标识符 。简单来说， **标识符就是一个名字** 。

有一些标识符，Java 语言已经赋予了其特殊的含义，只能用于特定的地方，这些特殊的标识符就是 关键字 。简单来说，**关键字是被赋予特殊含义的标识符** 。比如，在我们的日常生活中，如果我们想要开一家店，则要给这个店起一个名字，起的这个“名字”就叫标识符。但是我们店的名字不能叫“警察局”，因为“警察局”这个名字已经被赋予了特殊的含义，而“警察局”就是我们日常生活中的关键字

![截图.png](java面试.assets/clip_image006.gif)

 

 **Java 有没有 goto**

- **goto 是 Java 中的保留字，在目前版本的 Java 中没有使用**







**Java标识符由数字，字母和下划线（_），美元符号（$）或人民币符号（￥）组成。在Java中是区分大小写的，而且还要求首位不能是数字。最重要的是，Java关键字不能当作Java标识符**









## Native

native是由调用本地方法库（如操作系统底层函数），可以由C，C++实现



native的意思就是通知操作系统， 这个函数你必须给我实现，因为我要使用。 所以native关键字的函数都是操作系统实现的， java只能调用。









## final

- final修饰的类不能被继承
- 修饰的方法不能被重写
- 修饰的变量不能被再次赋值

- 被final修饰的变量不可以被改变，被final修饰不可变的是变量的引用，而不是引用指向的内容，引用指向的内容是可以改变的

- final修饰的成员变量在赋值时可以有三种方式。1、在声明时直接赋值。2、在构造器中赋值。3、在初始代码块中进行赋值。





final方法可以访问static修饰的类成员变量，也可以访问非static修饰的普通成员变量

```
public class Base {
    Son son=new Son();
    public void  Test1(){
        int brith=2;
    }

    public final  void Test(){
        System.out.println(son.age);
        System.out.println(Son.name);
    }
}

class Son{

    int age=2;
    static String name="石晨霖";

}
```







### final finally finalize区别

- final可以修饰类、变量、方法，修饰类表示该类不能被继承、修饰方法表示该方法不能被重写、修饰变量表示该变量是一个常量不能被重新赋值。
- finally一般作用在try-catch代码块中，在处理异常的时候，通常我们将一定要执行的代码方法finally代码块 中，表示不管是否出现异常，该代码块都会执行，一般用来存放一些关闭资源的代码。
- finalize是一个方法，属于Object类的一个方法，而Object类是所有类的父类，该方法一般由垃圾回收器来调用，当我们调用System.gc() 方法的时候，由垃圾回收器调用finalize()，回收垃圾，一个对象是否可回收的 最后判断。













## this

- this是自身的一个对象，代表对象本身，可以理解为：指向对象本身的一个指针。

- this的用法在java中大体可以分为3种：

  - 1.**普通的直接引用**，this相当于是指向当前对象本身。

  - 2.**形参与成员名字重名**，用this来区分：

    ```arduino
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    ```

  - 3**.引用本类的构造函数**

    ```arduino
    class Person{
        private String name;
        private int age;
        
        public Person() {
        }
     
        public Person(String name) {
            this.name = name;
        }
        public Person(String name, int age) {
            this(name);
            this.age = age;
        }
    }
    ```














## super

- super可以理解为是指向自己超（父）类对象的一个指针，而这个超类指的是离自己最近的一个父类。

- super也有三种用法：

  - 1.普通的直接引用

    与this类似，super相当于是指向当前对象的父类的引用，这样就可以用super.xxx来引用父类的成员。

  - 2.子类中的成员变量或方法与父类中的成员变量或方法同名时，用super进行区分

    ```typescript
    class Person{
        protected String name;
     
        public Person(String name) {
            this.name = name;
        }
     
    }
     
    class Student extends Person{
        private String name;
     
        public Student(String name, String name1) {
            super(name);
            this.name = name1;
        }
     
        public void getInfo(){
            System.out.println(this.name);      //Child
            System.out.println(super.name);     //Father
        }
     
    }
    
    public class Test {
        public static void main(String[] args) {
           Student s1 = new Student("Father","Child");
           s1.getInfo();
     
        }
    }
    复制代码
    ```

  - 3.引用父类构造函数

    - super（参数）：调用父类中的某一个构造函数（应该为构造函数中的第一条语句）。
    - this（参数）：调用本类中另一种形式的构造函数（应该为构造函数中的第一条语句）









## this与super的区别

- super: 它引用当前对象的直接父类中的成员（用来访问直接父类中被隐藏的父类中成员数据或函数，基类与派生类中有相同成员定义时如：super.变量名 super.成员函数据名（实参）



- this：它代表当前对象名（在程序中易产生二义性之处，应使用this来指明当前对象；如果函数的形参与类中的成员数据同名，这时需用this来指明成员变量名）



- super()和this()类似,区别是，super()在子类中调用父类的构造方法，this()在本类内调用本类的其它构造方法。



- super()和this()均需放在构造方法内第一行。



- 尽管可以用this调用一个构造器，但却不能调用两个。



- this和super不能同时出现在一个构造函数里面，因为this必然会调用其它的构造函数，其它的构造函数必然也会有super语句的存在，所以在同一个构造函数里面有相同的语句，就失去了语句的意义，编译器也不会通过。



- this()和super()都指的是对象，所以，均不可以在static环境中使用。包括：static变量,static方法，static语句块。



- 从本质上讲，this是一个指向本对象的指针, 然而super是一个Java关键字。






## static



### static存在的主要意义

- static的主要意义是在于创建独立于具体对象的域变量或者方法。**以致于即使没有创建对象，也能使用属性和调用方法**！
- static关键字还有一个比较关键的作用就是 **用来形成静态代码块以优化程序性能**。static块可以置于类中的任何地方，类中可以有多个static块。在类初次被加载的时候，会按照static块的顺序来执行每个static块，并且只会执行一次。
- 为什么说static块可以用来优化程序性能，是因为它的特性:只会在类加载的时候执行一次。因此，很多时候会将一些只需要进行一次的初始化操作都放在static代码块中进行






#### static的独特之处

1、被static修饰的变量或者方法是独立于该类的任何对象，也就是说，这些变量和方法**不属于任何一个实例对象，而是被类的实例对象所共享**。

> 怎么理解 “被类的实例对象所共享” 这句话呢？就是说，一个类的静态成员，它是属于大伙的【大伙指的是这个类的多个对象实例，我们都知道一个类可以创建多个实例！】，所有的类对象共享的，不像成员变量是自个的【自个指的是这个类的单个实例对象】
>
> 

2、在该类被第一次加载的时候，就会去加载被static修饰的部分，而且只在类第一次使用时加载并进行初始化，注意这是第一次用就要初始化，后面根据需要是可以再次赋值的。



3、static变量值在类加载的时候分配空间，以后创建类对象的时候不会重新分配。赋值的话，是可以任意赋值的！



4、被static修饰的变量或者方法是优先于对象存在的，也就是说当一个类加载完毕之后，即便没有创建对象，也可以去访问。





### static应用场景

- 因为static是被类的实例对象所共享，因此如果**某个成员变量是被所有对象所共享的，那么这个成员变量就应该定义为静态变量**。
- 因此比较常见的static应用场景有：

> 1、修饰成员变量 2、修饰成员方法 3、静态代码块 4、修饰类【只能修饰内部类也就是静态内部类】 5、静态导包





### static注意事项

- 1、静态只能访问静态。
- 2、非静态既可以访问非静态的，也可以访问静态的。














## continue、break 和 return 的区别是什么？

在循环结构中，当循环条件不满足或者循环次数达到要求时，循环会正常结束。但是，有时候可能需要在循环的过程中，当发生了某种条件之后 ，提前终止循环，这就需要用到下面几个关键词：

1. continue ：指跳出当前的这一次循环，继续下一次循环。

2. break ：指跳出整个循环体，继续执行循环下面的语句。

​     return 用于跳出所在方法，结束该方法的运行。

return 一般有两种用法：

1. return; ：直接使用 return 结束方法执行，用于没有返回值函数的方法

2. return value; ：return 一个特定值，用于有返回值函数的方法

 





# 方法

## 构造方法



### 类的构造方法的作用是什么?

构造方法是一种特殊的方法，主要作用是完成对象的初始化工作。





### 如果一个类没有声明构造方法，该程序能正确执行吗?

如果一个类没有声明构造方法，也可以执行

因为一个类即使没有声明构造方法也会有默认的不带参数的构造方法。如果我们自己添加了类的构造方法（无论是否有参），Java 就不会再添加默认的无参数的构造方法了，我们一直在不知不觉地使用构造方法，这也是为什么我们在创建对象的时候后面要加一个括号（因为要调用无参的构造方法）。如果我们重载了有参的构造方法，记得都要把无参的构造方法也写出来（无论是否用到），因为这可以帮助我们在创建对象的时候少踩坑。







### 构造方法有哪些特点？是否可被 override?

构造方法特点如下：

- 名字与类名相同。
- 没有返回值，但不能用 void 声明构造函数。
- 生成类的对象时自动执行，无需调用。

**构造方法不能被 override（重写）,但是可以 overload（重载）,所以你可以看到一个类中有多个构造函数的情况**。





### 构造方法的修饰符

构造方法可以用private修饰，如果构造方法用private修饰了就不能被继承，也就是没有子类，大部分是public

**一般来说使用private修饰符来修饰构造方法常用于单例模式**











## 构造方法的应用

类的构造方法总是先调用父类的构造方法，如果子类的构造方法没有明显地指明使用父类的哪个构造方法，子类就调用父类不带参数的构造方法。
而父类没有无参的构造函数，所以子类需要在自己的构造函数中显式的调用父类的构造函数。

举例：

```
class Person {
   String name = "No name";
   public Person(String nm) {
      name = nm;
   }
}
class Employee extends Person {
   String empID = "0000";
   public Employee(String id) {
      super("");
      empID = id;
   }
}
public class Test {
   public static void main(String args[]) {
      Employee e = new Employee("123");
      System.out.println(e.empID);
   }
}
```

如果父类中没有声明无参的构造方法，且子类中没有声明使用了父类的哪一个构造方法的话，默认必须显式的声明父类的无参构造方法，否则编译错误（在创建子类的对象时，若不含带参构造函数，将先执行父类的无参构造函数，然后再执行自己的无参构造函数。）

<img src="java面试.assets/image-20220909142352556.png" alt="image-20220909142352556" style="zoom:80%;" />![image-











**用户也可以调用构造方法**

1. 在类内部可以用户可以使用关键字this.()**调用（参数决定调用的是本类对应的构造方法）
2. 在子类中用户可以通过关键字**super.父类构造方法名()**调用（参数决定调用的是父类对应的构造方法。）
3. 反射机制对于任意一个类，都能够知道这个类的所有属性和方法，包括类的构造方法。















## super的作用

 1：特殊变量super，提供了对父类的访问。

 2：可以使用super访问父类被子类隐藏的变量或覆盖的方法。

 3：每个子类构造方法的第一条语句，都是隐含地调用super()，如果父类没有这种形式的构造函数，那么在编译的时候就会报错。
 4：构造是不能被继承的。







## 静态方法



### static 修饰变量

`static` 关键字表示的概念是 `全局的、静态的`，用它修饰的变量被称为`静态变量`。

```java
public class TestStatic {

    static int i = 10; // 定义了一个静态变量 i 
}
```

静态变量也被称为类变量，静态变量是属于这个类所有的。什么意思呢？这其实就是说，**static 关键字只能定义在类的 `{}` 中，而不能定义在任何方法中。**



```
public class StringTest {
   
    static  int a=5;
    public  static  void Test1(){
        int a=4;
    }
}
```



<img src="java面试.assets/image-20220823132047760-16612320490901.png" alt="image-20220823132047760" style="zoom:67%;" />

static 属于类所有，由类来直接调用 static 修饰的变量，它不需要手动实例化类进行调用

```
public class StringTest {

    static  int a=5;

    public static void main(String[] args) {
        System.out.println(StringTest.a);
    }

}
```





**static 修饰方法的注意事项**

- 首先第一点就是最常用的，不用创建对象，直接`类名.变量名` 即可访问；

- static 修饰的方法内部不能调用非静态方法，反之非静态方法可以调用静态方法；

  **static 静态成员属性不能使用 this 关键字调用**



<img src="java面试.assets/image-20220823132850883-16612325325543.png" alt="image-20220823132850883" style="zoom:67%;" />



<img src="java面试.assets/image-20220823132953636-16612325958305.png" alt="image-20220823132953636" style="zoom:67%;" />





### static 修饰代码块

static 关键字可以用来修饰代码块，代码块分为两种，一种是使用 `{}` 代码块；一种是 `static {}` 静态代码块。

static 修饰的代码块被称为静态代码块。静态代码块可以置于类中的任何地方，类中可以有多个 static 块，在类初次被加载的时候，会按照 static 代码块的顺序来执行，每个 static 修饰的代码块只能执行一次。我们会面会说一下代码块的加载顺序。下面是静态代码块的例子

<img src="java面试.assets/2mf7y.png" alt="img" style="zoom:50%;" />

static 代码块可以用来**优化程序执行顺序**，是因为它的特性：只会在类加载的时候执行一次。







### static 用作静态内部类

内部类的使用场景比较少，但是内部类还有具有一些比较有用的。内部类的分类

- 普通内部类
- 局部内部类
- 静态内部类
- 匿名内部类

`静态内部类`就是用 static 修饰的内部类，静态内部类可以包含静态成员，也可以包含非静态成员，但是在非静态内部类中不可以声明静态成员。

静态内部类有许多作用，由于非静态内部类的实例创建需要有外部类对象的引用，所以非静态内部类对象的创建必须依托于外部类的实例；而静态内部类的实例创建只需依托外部类；

并且由于非静态内部类对象持有了外部类对象的引用，因此非静态内部类可以访问外部类的非静态成员；而静态内部类只能访问外部类的静态成员；

- 内部类需要脱离外部类对象来创建实例
- 避免内部类使用过程中出现内存溢出





### **静态方法为什么不能调用非静态成员?**

这个需要结合 JVM 的相关知识，主要原因如下：

静态方法是属于类的，在类加载的时候就会分配内存，可以通过类名直接访问。而非静态成员属于实例对象，只有在对象实例化之后才存在，需要通过类的实例对象去访问。

在类的非静态成员不存在的时候静态成员就已经存在了，此时调用在内存中还不存在的非静态成员，属于非法操作。

 

 







### **静态方法和实例方法有何不同？**

**1、调用方式**

在外部调用静态方法时，可以使用 类名.方法名 的方式，也可以使用 对象.方法名 的方式，而实例方法只有后面这种方式。也就是说，**调用静态方法可以无需创建对象** 。

不过，需要注意的是一般不建议使用 对象.方法名 的方式来调用静态方法。这种方式非常容易造成混淆，静态方法不属于类的某个对象而是属于这个类。

因此，一般建议使用 类名.方法名 的方式来调用静态方法。

```
public class Person {

  public void method() {

   //......

  }

 

  public static void staicMethod(){

   //......

  }

  public static void main(String[] args) {

     Person person = new Person();

    // 调用实例方法

    person.method();

   // 调用静态方法

    Person.staicMethod()

  }

}
```

 

**2、访问类成员是否存在限制**

静态方法在访问本类的成员时，只允许访问静态成员（即静态成员变量和静态方法），不允许访问实例成员（即实例成员变量和实例方法），而实例方法不存在这个限制

 

 



### 如何获取静态属性

1如果是本类使用，可以直接就用静态变量名。

2如果是其他类使用，可以使用类名来调用，也可以创建一个实例对象来调用。

 3如果静态变量所在的类是静态类，那么不管在本类里或者在其他外部类，都可以直接使用静态变量名。

```
public class JTTest {

    static String name="scl";
   
    public void Test(){
        System.out.println(JTTest.name);
        System.out.println(name);
        System.out.println(TestFX.AnotherName);//其他类的静态变量
        
    }
}
```







### static存在的主要意义

static的主要意义是在于创建独立于具体对象的域变量或者方法。 以致于即使没有创建对象， 也能使用属性和调用方法 

static关键字还有一个比较关键的作用就是 用来形成静态代码块以优化程序性能 。

static块可 以置于类中的任何地方，类中可以有多个static块。在类初次被加载的时候，会按照static块的 顺序来执行每个static块，并且只会执行一次。



为什么说static块可以用来优化程序性能，是因为它的特性:只会在类加载的时候执行一次。因此很多时候会将一些只需要进行一次的初始化操作都放在static代码块中进行。











### 举例

```
public class Arraytest{
    int a[] = new int[6];
    public static void main ( String arg[] ) {
        System.out.println ( a[0] );
    }
}
```

结果 **编译错误**

 在static方法里面，是不能直接访问普通成员变量的，类加载顺序而言，此时普通成员变量还没被加载，根本拿不到，所以说要通过new对象点来访问，因为要完成初始化。





```
public class staticTest {

    int arr[]=new int[6];
    public static void main(String[] args) {
        staticTest test=new staticTest();

        System.out.println(test.arr[0]);
    }
}
```

结果为0













 





 

 

 

 



 

 

 

## 重写和重载

### 重载和重写的区别

重载就是同样的一个方法能够根据输入数据的不同，做出不同的处理重写就是当子类继承自父类的相同方法，输入数据一样，但要做出有别于父类的响应时，你就要覆盖父类方法

**重载**

发生在同一个类中（或者父类和子类之间），方法名必须相同，参数类型不同、个数不同、顺序不同，方法返回值和访问修饰符可以不同。

**被重载的方法必须改变参数列表(参数个数或类型不一样)**



**综上：重载就是同一个类中多个同名方法根据不同的传参来执行不同的逻辑处理。**













**重写**

重写发生在运行期，是子类对父类的允许访问的方法的实现过程进行重新编写。

1. 方法名、参数列表必须相同，子类方法返回值类型应比父类方法返回值类型更小或相等，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类。

2. 如果父类方法访问修饰符为 private/final/static 则子类就不能重写该方法，但是被 static 修饰的方法能够被再次声明。

3. 构造方法无法被重写

综上：**重写就是子类对父类方法的重新改造，外部样子不能改变，内部逻辑可以改变。**

| 区别点     | 重载方法 | 重写方法                                                     |
| ---------- | -------- | ------------------------------------------------------------ |
| 发生范围   | 同一个类 | 子类                                                         |
| 参数列表   | 必须修改 | 一定不能修改                                                 |
| 返回类型   | 可修改   | 子类方法返回值类型应比父类方法返回值类型更小或相等           |
| 异常       | 可修改   | 子类方法声明抛出的异常类应比父类方法声明抛出的异常类更小或相等； |
| 访问修饰符 | 可修改   | 一定不能做更严格的限制（可以降低限制）                       |
| 发生阶段   | 编译期   | 运行期                                                       |

**方法的重写要遵循“两同两小一大”**

“两同”即方法名相同、形参列表相同；

 “两小”指的是子类方法返回值类型应比父类方法返回值类型更小或相等，子类方法声明抛出的异常类应比父类方法声明抛出的异常类更小或相等；

“一大”指的是子类方法的访问权限应比父类方法的访问权限更大或相等。

⭐️ 关于 **重写的返回值类型** 这里需要额外多说明一下，上面的表述不太清晰准确：如果方法的返回类型是 void 和基本数据类型，则返回值重写时不可修改。但是如果方法的返回值是引用类型，重写时是可以返回该引用类型的子类的。

```
public class Hero {

  public String name() {

   return "超级英雄";

  }

}

public class SuperMan extends Hero{

  @Override

  public String name() {

   return "超人";

  }

  public Hero hero() {

    return new Hero();

  }

}

 

public class SuperSuperMan extends SuperMan {

  public String name() {

    return "超级超级英雄";

  }

 

  @Override

  public SuperMan hero() {

    return new SuperMan();

  }

}

 
```

 

 







例题：

<img src="java面试.assets/image-20220907221804994-16625602864461.png" alt="image-20220907221804994" style="zoom:67%;" />



选B

ACD都是重载（在一个类中只能存在重载）参数类型，数量不同

B是重写，重写出现的情况只能是两个类（父类与子类中）









在类Tester中定义方法如下，  

   public double max(int x, int y) { // 省略 }   

  则在该类中定义如下哪个方法头是对上述方法的重载(Overload)?   

- ```
  public int max(int a, int b) {}
  ```

- ```
  public int max(double a, double b) {}
  ```

- ```
  public double max(int x, int y) {}
  ```

- ```
  private double max(int a, int b) {}
  ```



选第二个









![image-20220926224053868](java面试.assets/image-20220926224053868-16642032557337.png)



<img src="java面试.assets/image-20220926224122473-16642032838099.png" alt="image-20220926224122473" style="zoom:67%;" />

AB重写 CD重载







# 基本数据类型

 

## Java 中的几种基本数据类型





![image-20220905134323770](java面试.assets/image-20220905134323770-16623566056885.png)







Java 中有 8 种基本数据类型，分别为：

 6 种数字类型：

 4 种整数型：byte、short、int、long

 2 种浮点型：float、double

1 种字符类型：char

 1 种布尔型：boolean。

这 8 种基本数据类型的默认值以及所占空间的大小如下：

 

| 基本类型 | 位数 | 字节 | 默认值  | 取值范围                                    |
| -------- | ---- | ---- | ------- | ------------------------------------------- |
| byte     | 8    | 1    | 0       | -128 ~ 127                                  |
| short    | 16   | 2    | 0       | -32768 ~  32767                             |
| int      | 32   | 4    | 0       | -2147483648  ~ 2147483647                   |
| long     | 64   | 8    | 0L      | -9223372036854775808  ~ 9223372036854775807 |
| char     | 16   | 2    | 'u0000' | 0 ~ 65535                                   |
| float    | 32   | 4    | 0f      | 1.4E-45 ~  3.4028235E38                     |
| double   | 64   | 8    | 0d      | 4.9E-324 ~  1.7976931348623157E308          |
| boolean  | 1    |      | false   | true、false                                 |

对于 boolean，官方文档未明确定义，它依赖于 JVM 厂商的具体实现。逻辑上理解是占用 1 位，但是实际中会考虑计算机高效存储因素。

另外，Java 的每种基本类型所占存储空间的大小不会像其他大多数语言那样随机器硬件架构的变化而变化。这种所占存储空间大小的不变性是 Java 程序比用其他大多数语言编写的程序更具可移植性的原因之一

**注意：**

1. Java 里使用 long 类型的数据一定要在数值后面加上 L，否则将作为整型解析。

2. char a = 'h'char :单引号，String a = "hello" :双引号。

这八种基本类型都有对应的包装类分别为：Byte、Short、Integer、Long、Float、Double、Character、Boolean 。

 











Java语言中，中文字符所占的字节数取决于字符的编码方式，一般情况下，采用ISO8859-1编码方式时，一个中文字符与一个英文字符一样只占1个字节；采用GB2312或GBK编码方式时，一个中文字符占2个字节；而采用UTF-8编码方式时，一个中文字符会占3个字节。





**为什么java里的char是2个字节？**  

   因为java内部都是用unicode的，所以java其实是支持中文变量名的，比如string 世界 =    "我的世界";这样的语句是可以通过的。  

  综上，java中采用GB2312或GBK编码方式时，一个中文字符占2个字节，而char是2个字节，所以是对的









##  Java基本数据类型转换

**byte、short、char—>int—>long—>float—>double**

由低精度到高精度可以自动转换，而高精度到低精度会损失精度，故需要强制转换。







举例

```
public static void main(String[] args){
    Double x=1.2;  
    long l = 1.2;  
    float f =  x/l;
    System.out.println(f);
}


```

编译错误

Double是包装类 会自动装箱

Double /long  =》Double会自动拆箱成double 高精度转换为低精度需要强制类型转换

所有直接出现编译错误





其中float类型需要注意

```
float x = 2;
float xx = 2f;

float x1 = 2.0;  // Error
float x2 = 2.0F; // 定义浮点数不能省略 f（或F）

Float x3 = 2; // Error
Float x4 = 2f; // 一定要加 f 或 F

Float x5 = 2.0; // Error
Float x6 = 2.0f; // 一定要加 f 或 F
```





- 使用包装类型声明一定要加`F`(`f`),
- 使用基本数据类型声明，定义整数可以不加`F`(`f`)，浮点数一定要加 不加会默认为double类型



 









错误的是

```
public class Demo{
 
　　float func1()
　　{
　　　　int i=1;
　　　　return;
　　}
　　float func2()
　　{
　　　　short i=2;
　　　　return i;
　　}
　　float func3()
　　{
　　　　long i=3;
　　　　return i;
　　}
　　float func4()
　　{
　　　　double i=4;
　　　　return i;
　　}
}
```

选择A D















当byte、short、char进行计算时，会默认提升为int再进行计算    

   对于byte、short、char来说，右侧赋值的数值没有超过范围，编译器会自动补上(byte)、(short)、(char)    

   +=类赋值运算符，编译器会自动补上(byte)、(short)、(char)

**复合类型运算符号自动进行类型转换**





```
public class TestBute {
    public static void main(String[] args) {
        byte a=2;
        byte b=3;
        b= (byte) (a+b);
        System.out.println(b);


        short c=3;
        short d=4;
        d= (short) (c+d);
        System.out.println(d);



        char e='2';
        char f='3';
        f= (char) (e+f);
        System.out.println(f);


        System.out.println("------------------------------------------------");

        byte m=2;
        byte n=4;
        n+=m;
        System.out.println(n);
    }
}
```













### 拓展：

switch

**jdk1.``7``之前``byte``,``short,``int,``char
jdk1.``7``之后加入String``**

**``
java8，``switch``支持``10``种类型
基本类型：``byte char short int
包装类 ：Byte,Short,Character,Integer String  enum
实际只支持``int``类型 Java实际只能支持``int``类型的``switch``语句，那其他的类型时如何支持的
a、基本类型``byte char short原因：这些基本数字类型可自动向上转为``int``, 实际还是用的``int``。
b、基本类型包装类Byte,Short,Character,Integer 原因：java的自动拆箱机制 可看这些对象自动转为基本类型
c、String 类型 原因：实际``switch``比较的string.hashCode值，它是一个``int``类型
d、``enum``类型 原因 ：实际比较的是``enum``的ordinal值（表示枚举值的顺序），它也是一个``int``类型 所以也可以说 ``switch``语句只支持``int``类型**







```
public` `class` `Test {
  ``public` `static` `void` `main(String args[]) {
    ``String s = ``"祝你考出好成绩!"``;
    ``System.out.println(s.length());
  ``}
}
8
```

汉字的字符串的准确长度的 1字符=2字节，1字节=8位 英文和数字占一个一个字节，中文占一个字符也就是两个字节









 

 

## 基本类型和包装类型的区别？

**1 成员变量包装类型不赋值就是 null ，而基本类型有默认值且不是 null。**

**2 包装类型可用于泛型，而基本类型不可以。**

**3 基本数据类型的局部变量存放在 Java 虚拟机栈中的局部变量表中，基本数据类型的成员变量（未被 static 修饰 ）存放在 Java 虚拟机的堆中。包装类型属于对象类型，我们知道几乎所有对象实例都存在于堆中。**

**4 相比于对象类型， 基本数据类型占用的空间非常小。**

<img src="java面试.assets/clip_image008.gif" alt="截图.png" style="zoom:67%;" />

<img src="java面试.assets/clip_image010.gif" alt="截图.png" style="zoom:67%;" />

**为什么说是几乎所有对象实例呢？** 这是因为 HotSpot 虚拟机引入了 JIT 优化之后，会对对象进行逃逸分析，如果发现某一个对象并没有逃逸到方法外部，那么就可能通过标量替换来实现栈上分配，而避免堆上分配内存

⚠️注意 ： **基本数据类型存放在栈中是一个常见的误区！** 基本数据类型的成员变量如果没有被 static 修饰的话（不建议这么使用，应该要使用基本数据类型对应的包装类型），就存放在堆中。





**基本类型与包装类型的异同**：

1、在Java中，一切皆对象，但八大基本类型却不是对象。

2、声明方式的不同，基本类型无需通过new关键字来创建，而封装类型需new关键字。

3、存储方式及位置的不同，基本类型是直接存储变量的值保存在堆栈中能高效的存取，封装类型需要通过引用指向实例，具体的实例保存在堆中。

4、初始值的不同，封装类型的初始值为null，基本类型的的初始值视具体的类型而定，比如int类型的初始值为0，boolean类型为false；

5、**使用方式的不同，比如与集合类合作使用时只能使用包装类型。**

6、**什么时候该用包装类，什么时候用基本类型，看基本的业务来定：这个字段允不允许null值，如果允许null值，则必然要用封装类，否则值类型就可以了，用到比如泛型和反射调用函数.，就需要用包装类**

 

 7、存储方式和位置不同，从而性能不同。基本数据类型存储在栈（stack）中（其实得区分局部变量还是成员变量的），包装类则分成引用和实例，引用在栈（stack）中，具体实例在堆（heap）中。可以通过程序来验证速度的不同





## 包装类



**包装类中封装了一系列的方法和属性，其中就包括了静态方法，但是基本数据类型中没有**





### 自动装箱与拆箱了解吗？原理是什么？

**什么是自动拆装箱？**

- **装箱**：将基本类型用它们对应的引用类型包装起来；
- **拆箱**：将包装类型转换为基本数据类型；

举例：



```java
Integer i = 10;  //装箱
int n = i;   //拆箱
```



上面这两行代码对应的字节码为：



```java
   L1

    LINENUMBER 8 L1

    ALOAD 0

    BIPUSH 10

    INVOKESTATIC java/lang/Integer.valueOf (I)Ljava/lang/Integer;

    PUTFIELD AutoBoxTest.i : Ljava/lang/Integer;

   L2

    LINENUMBER 9 L2

    ALOAD 0

    ALOAD 0

    GETFIELD AutoBoxTest.i : Ljava/lang/Integer;

    INVOKEVIRTUAL java/lang/Integer.intValue ()I

    PUTFIELD AutoBoxTest.n : I

    RETURN
```



从字节码中，我们发现装箱其实就是调用了 包装类的`valueOf()`方法，拆箱其实就是调用了 `xxxValue()`方法。

因此，

- `Integer i = 10` 等价于 `Integer i = Integer.valueOf(10)`
- `int n = i` 等价于 `int n = i.intValue()`;

注意：**如果频繁拆装箱的话，也会严重影响系统的性能。我们应该尽量避免不必要的拆装箱操作。**







### 包装类型的缓存机制了解么？

Java 基本数据类型的包装类型的大部分都用到了缓存机制来提升性能。

`Byte`,`Short`,`Integer`,`Long` 这 4 种包装类默认创建了数值 **[-128，127]** 的相应类型的缓存数据，`Character` 创建了数值在 **[0,127]** 范围的缓存数据，`Boolean` 直接返回 `True` or `False`。

**Integer 缓存源码：**



```java
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}
private static class IntegerCache {
    static final int low = -128;
    static final int high;
    static {
        // high value may be configured by property
        int h = 127;
    }
}
```

**`Character` 缓存源码:**



```java
public static Character valueOf(char c) {
    if (c <= 127) { // must cache
      return CharacterCache.cache[(int)c];
    }
    return new Character(c);
}

private static class CharacterCache {
    private CharacterCache(){}
    static final Character cache[] = new Character[127 + 1];
    static {
        for (int i = 0; i < cache.length; i++)
            cache[i] = new Character((char)i);
    }

}
```



**`Boolean` 缓存源码：**



```java
public static Boolean valueOf(boolean b) {
    return (b ? TRUE : FALSE);
}
```

**重点：**

**如果超出对应范围仍然会去创建新的对象，缓存的范围区间的大小只是在性能和资源之间的权衡。**

**两种浮点数类型的包装类 `Float`,`Double` 并没有实现缓存机制**。

 

![image-20220918173121816](java面试.assets/image-20220918173121816-16634934832431.png)

![image-20220918173147144](java面试.assets/image-20220918173147144-16634935085773.png)



### new Integer(123) 与 Integer.valueOf(123)有何区别，请从底层实现分析两者区别？

valueOf() 方法用于返回给定参数的原生 Number 对象值，参数可以是原生数据类型, String等。

该方法是静态方法。该方法可以接收两个参数一个是字符串，一个是基数。





new Integer(123) 与 Integer.valueOf(123) 的区别在于：

- new Integer(123) 每次都会新建一个对象；
- Integer.valueOf(123) 会使用缓存池中的对象，多次调用会取得同一个对象的引用。

```ini
Integer x = new Integer(123);
Integer y = new Integer(123);
System.out.println(x == y);    // false
Integer z = Integer.valueOf(123);
Integer k = Integer.valueOf(123);
System.out.println(z == k);   // true

```

valueOf() 方法的实现比较简单，就是先判断值是否在缓存池中，如果在的话就直接返回缓存池的内容。

```arduino
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}

```



编译器会在自动装箱过程调用 valueOf() 方法，因此多个Integer实例使用自动装箱来创建并且值相同，那么就会引用相同的对象。

```ini
Integer m = 123;
Integer n = 123;
System.out.println(m == n); // true
```













1. **关于equals**

```java
double i0 = 0.1;
Double i1 = new Double(0.1);
Double i2 = new Double(0.1);
System.out.println(i1.equals(i2)); //true 2个包装类比较，比较的是包装的基本数据类型的值
System.out.println(i1.equals(i0)); //true 基本数据类型和包装类型比较时，会先把基本数据类型包装后再比较
```

注意**equals方法比较的是真正的值**。

当使用equals比较包装类和基本数据类型时候 会先把**基本数据类型包装成对应的包装类型，再进行比较**。

int---->Integer



2. **关于==（双等号）**

==对于基本数据类型，==（双等号）比较的是值，而对于包装类型，==（双等号）比较的则是2个对象的内存地址。



```java
double i0 = 0.1;
Double i1 = new Double(0.1);
Double i2 = new Double(0.1);
System.out.println(i1 == i2);    //false new出来的都是新的对象
System.out.println(i1 == i0);    //true 基本数据类型和包装类比较，会先把包装类拆箱
```

注意一点规律，**new出来的都是新的对象**。2个新的对象内存地址不同，那么**==号比较的结果肯定是false。
当使用== ==  时候比较包装类型和基本数据类型，会先把包装类拆箱再进行值比较（和equals是反的）**

Integer---->int



此处Double 拆箱成double i1和i0进行==比较 double i0是基本数据类型， double i2同样如此，   == 在比较基本数据类型时候比较的是本身的值









**举例** 1




```
Integer a1=1;
Integer a2=1;
Integer a3=232323;
Integer a4=232323;
 System.out.println(a1==a2);//true
 System.out.println(a1==a3);//false
 System.out.println(a3==a4);//false
```

a4 a3作为两个包装类相互比较 ==  ：超过了-128-127的范围，重新创建了新的对象 两个不同的对象指向的地址不同返回false

a1 a3 同理

a1 a2作为两个包装类相互比较 == ：符合包装类的缓存机制 没有创建新的对象



```
Integer a=18;
Integer a1=new Integer(18);
System.out.println(a==a1);//false
```











举例2

![image-20220910140258693](java面试.assets/image-20220910140258693-166278978004410.png)



false false true

第一个 第三个不多解释了

解释第二个false：



String s = "abc"：通过字面量赋值创建字符串。则将栈中的引用直接指向该字符串，如不存在，则在常量池中生成一个字符串，再将栈中的引用指向该字符串 

  String s = “a”+“bc”：编译阶段会直接将“a”和“bc”结合成“abc”，这时如果方法区已存在“abc”，则将s的引用指向该字符串，如不存在，则在方法区中生成字符串“abc”对象，然后再将s的引用指向该字符串 

  String s = "a" + new String("bc"):栈中先创建一个"a"字符串常量，再创建一个"bc"字符串常量，编译阶段不会进行拼接，在运行阶段拼接成"abc"字符串常量并将s的引用指向它，效果相当于String s = new String("abc")，只有'+'两边都是字符串常量才会在编译阶段优化（两个abc 地址不相同）













## parseInt

parseInt()是把String 变成int的基础数据类型；





## intValue

intValue()是把Integer对象类型变成int的基础数据类型；





Valueof()是把String 转化成Integer对象类型；（现在JDK版本支持自动装箱拆箱了。）


```
public static void main(String[] args) {
  Integer a=3;
    int i = a.intValue();
    System.out.println(i);
    System.out.println("_____________________________");

    String name="scl";
    String name1;
    int x =Integer.parseInt("9");
    System.out.println(x);


    System.out.println("________________________________");

    int a1 = Integer.parseInt("1024");
    System.out.println(a1);


    int b = Integer.valueOf("1024").intValue();
    System.out.println(b);

    int c=Integer.valueOf("1024");
    System.out.println(c);

    System.out.println(a1==b);
    System.out.println(a1==c);
    System.out.println(b==c);
```



















#### **综合练习**



<img src="java面试.assets/image-20220901140841743-16620125235103.png" alt="image-20220901140841743" style="zoom:67%;" />





结果为A B



​                            **局部变量                                               成员变量**

**基本数据类型： 变量名和值都在方法栈                            变量名和方法都在堆内存**

**引用数据类型： 变量在方法栈，变量指向的对象在堆中      变量名和变量名指向的对象都在堆中**



A：String a 和String b 默认是成员变量，String是引用数据类型且具有不可变性，a b的值相等，且两者指向的地址是同一个，都在常量池中



B：默认是成员变量 a存在默认值为0，所以满足-128-127的范围，所以两者都在堆中，且指向的地址相同





C int  a=1;  基本数据类型，单纯的变量名在堆内存，没有地址 但是Integer b=new Integer（1） 相当于 创建了一个新的对象 ，所以没法比较



D：int a=1；是一个基本数据类型，且在堆内存 没有指向的地址，Integer b=1 ，是一个引用数据类型，存在指向1的地址，二者使用== 比较 ，Integer进行拆箱，拆成int，所以最后是基本数据类型int之间的比较，只是单纯比较数值是否相同与地址无关。







练习：

![image-20220910133840773](java面试.assets/image-20220910133840773-16627883223588.png)



B

long double都是8字节 64bit











### 总结



1. 基本数据类型：boolean,char,byte,short,int,long,float,double)之间使用  == 比较的是它们的数值，引用数据类型之间使用==，比较的是它们在内存中的存放地址，对于String对象的值的比较，可以使用equals()  





2. int 与 Integer之间的比较涉及到自动装箱和自动拆箱，最后比较的还是int值。int==integer（如果二者数值相同的话）总是true  





3. Integer与Integer进行==比较时，看范围；在大于等于-128小于等于127的范围内为true，  

  在此范围外为false，java虚拟机缓存了Integer、Byte、Short、Character、Boolean（Float Double除外）包装类在-128~127之间的值，如果取值在这个范围内，会从int常量池取出一个int并自动装箱成Integer，超出这个范围就会重新创建一个。  





4. Integer a=n时，如果n大于等于-128小于等于127时，会直接从IntegerCache中取，不在这个范围内，会new一个对象，所以Integer与new Integer进行 ==比较时，结果都是false。  

  new Integer总会在堆创建对象，并且在范围内可以取IntegerCache并自动装箱。但是Integer a=n 要么将a指向常量池，要么超过范围则新建对象，所以比较他们的引用地址总是不同。  





5. 在方法中声明的变量可以是基本类型的变量，也可以是引用类型的变量。 
    （1）当声明是基本类型的变量的时，其变量名及值（变量名及值是两个概念）是放在方法栈中 
    （2）当声明的是引用变量时，所声明的变量（该变量实际上是在方法中存储的是[内存](https://so.csdn.net/so/search?q=内存&spm=1001.2101.3001.7020)地址值）是放在方法的栈中，该变量所指向的对象是放在堆类存中的。  





6. 同样在类中声明的变量即可是基本类型的变量 也可是引用类型的变量 
    （1）当声明的是基本类型的变量其变量名及其值放在堆内存中的 
    （2）引用类型时，其声明的变量仍然会存储一个内存地址值，该内存地址值指向所引用的对象。引用变量名和对应的对象仍然存储在相应的堆中.  

  ![image-20220913145046277](java面试.assets/image-20220913145046277-16630518483431.png)  





7. 在方法中定义的非全局基本数据类型变量的具体内容是存储在栈中的，只要是引用数据类型变量，其具体内容都是存放在堆中的，而栈中存放的是其具体内容所在内存的地址，所以不要说这种模棱两可的话，基本数据类型就没有引用地址。























 

# 多态



相同类型的变量、调用同一个方法时呈现出多种不同的行为特征，这就是多态

编译看左边 运行看右边   

  **一、使用父类类型的引用指向子类的对象；**

  **二、该引用只能调用父类中定义的方法和变量；**

  **三、如果子类中重写了父类中的一个方法，那么在调用这个方法的时候，将会调用子类中的这个方法；（动态连接、动态调用）**

  **四、变量不能被重写（覆盖），”重写“的概念只针对方法，如果在子类中”重写“了父类中的变量，那么在编译时会报错。**

**多态的3个必要条件：**

​    **1.继承  2.重写  3.父类引用指向子类对象。**

 

向上转型： Person p = new Man() ; //向上转型不需要强制类型转化

 

向下转型： Man man = (Man)new Person() ; //必须强制类型转化

 

 

![截图.png](java面试.assets/clip_image012.gif)

 

 

 

 

所谓多态就是指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编程时并不确定，而是在程序运行期间才确定，即一个引用变量倒底会指向哪个类的实例对象，该引用变量发出的方法调用到底是哪个类中实现的方法，必须在由程序运行期间才能决定。

因为在程序运行时才确定具体的类，这样，不用修改源程序代码，就可以让引用变量绑定到各种不同的类实现上，从而导致该引用调用的具体方法随之改变，即不修改程序代码就可以改变程序运行时所绑定的具体代码，让程序可以选择多个运行状态，这就是多态性。



多态分为编译时多态和运行时多态。其中编辑时多态是静态的，主要是指方法的重载，它是根据参数列表的不同来区分不同的函数，通过编辑之后会变成两个不同的函数，在运行时谈不上多态。而运行时多态是动态的，它是通过动态绑定来实现的，也就是我们所说的多态性。






## **多态的实现**

- Java实现多态有三个必要条件：继承、重写、向上转型。
  - 继承：在多态中必须存在有继承关系的子类和父类。
  - 重写：子类对父类中某些方法进行重新定义，在调用这些方法时就会调用子类的方法。
  - 向上转型：在多态中需要将子类的引用赋给父类对象，只有这样该引用才能够具备技能调用父类的方法和子类的方法。

```
只有满足了上述三个条件，我们才能够在同一个继承结构中使用统一的逻辑实现代码处理不同的对象，从而达到执行不同的行为。
对于Java而言，它多态的实现机制遵循一个原则：当超类对象引用变量引用子类对象时，被引用对象的类型而不是引用变量的类型决定了调用谁的成员方法，但是这个被调用的方法必须是在超类中定义过的，也就是说被子类覆盖的方法。
```








































# 抽象类

## 接口和抽象类有什么共同点和区别？

**共同点** ：

都不能被实例化。

都可以包含抽象方法。

 都可以有默认实现的方法（Java 8 可以用 default 关键字在接口中定义默认方法）。

**区别** ：

接口主要用于对类的行为进行约束，你实现了某个接口就具有了对应的行为。抽象类主要用于代码复用，强调的是所属关系（比如说我们抽象了一个发送短信的抽象类，）。

 一个类只能继承一个类，但是可以实现多个接口。

接口中的成员变量只能是 public static final 类型的，不能被修改且必须有初始值，而抽象类的成员变量默认 default，可在子类中被重新定义，也可被重新赋值。

 

 

 ![image-20220823204753237](java面试.assets/image-20220823204753237-166125887465611.png)

 

 **上述接口中的访问修饰符，在JDK9后接口修饰符可以为private**

**java8后可以在接口中定义静态方法 default**（必须要有方法体） 实现的的方法可以不重写

![image-20220919232615638](java面试.assets/image-20220919232615638-16636011786471.png)

 **抽象类中可以有抽象方法和普通方法，但是抽象方法不能有方法体，普通方法必须要有方法体**

**接口在jdk1.9后，接口中的修饰符可以为private，但是必须有方法体；**

```
public interface InterfaceA {
    int a=1;

    private int getSum() {
        return 0;
    }

}
```





 



 接口和抽象类都可以定义成员<img src="java面试.assets/image-20220910141202485-166279032534814.png" alt="image-20220910141202485" style="zoom:67%;" />

------

 默认public static final（可省略） final修饰的属性必须赋值；

<img src="java面试.assets/image-20220910141300677-166279038309316.png" alt="image-20220910141300677" style="zoom:67%;" />

 

 

其中值得注意的是接口中 修饰符public static final 都可省略 且成员变量只能为public 不能是private：

![image-20220910201000045](java面试.assets/image-20220910201000045-16628118014491.png)





抽象类中：同样如此，唯一不同的是。抽象类中的成员变量修饰符与正常类无二，任何修饰符

![image-20220910201447674](java面试.assets/image-20220910201447674-16628120888563.png)





























# 泛型



## **常用的 T，E，K，V，？**

本质上这些个都是通配符，没啥区别，只不过是编码时的一种约定俗成的东西。比如上述代码中的 T ，我们可以换成 A-Z 之间的任何一个 字母都可以，并不会影响程序的正常运行，但是如果换成其他的字母代替 T ，在可读性上可能会弱一些。**通常情况下，T，E，K，V，？是这样约定的：**

- ？表示不确定的 java 类型
- T (type) 表示具体的一个java类型
- K V (key value) 分别代表java键值中的Key Value
- E (element) 代表Element



我有一个父类 Animal 和几个子类，如狗、猫等，现在我需要一个动物的列表，我的第一个想法是像这样的：

```java
List<Animal> listAnimalsCOPY
```



```java
List<? extends Animal> listAnimalsCOPY
```

为什么要使用通配符而不是简单的泛型呢？通配符其实在声明局部变量时是没有什么意义的，但是当你为一个方法声明一个参数时，它是非常重要的。

```java
static int countLegs (List<? extends Animal > animals ) {
    int retVal = 0;
    for ( Animal animal : animals )
    {
        retVal += animal.countLegs();
    }
    return retVal;
}

static int countLegs1 (List< Animal > animals ){
    int retVal = 0;
    for ( Animal animal : animals )
    {
        retVal += animal.countLegs();
    }
    return retVal;
}

public static void main(String[] args) {
    List<Dog> dogs = new ArrayList<>();
  // 不会报错
    countLegs( dogs );
 // 报错
    countLegs1(dogs);
}COPY
```

当调用 countLegs1 时，就会飘红，提示的错误信息如下：

![img](java面试.assets/cf94fa94-682f-45d8-866d-9dd1bd9f9e91.jpg)

所以，对于不确定或者不关心实际要操作的类型，可以使用无限制通配符（尖括号里一个问号，即 <?> ），表示可以持有任何类型。像 countLegs 方法中，限定了上界，但是不关心具体类型是什么，所以对于传入的 Animal 的所有子类都可以支持，并且不会报错。而 countLegs1 就不行。











## **上界通配符 < ? extends E>**

> 上届：用 extends 关键字声明，表示参数化的类型可能是所指定的类型，或者是此类型的子类。

在类型参数中使用 extends 表示这个泛型中的参数必须是 E 或者 E 的子类，这样有两个好处：

- **如果传入的类型不是 E 或者 E 的子类，编译不成功**
- **泛型中可以使用 E 的方法，要不然还得强转成 E 才能使用**

```java
private <K extends A, E extends B> E test(K arg1, E arg2){
    E result = arg2;
    arg2.compareTo(arg1);
    //.....
    return result;
}COPY
```

> 类型参数列表中如果有多个类型参数上限，用逗号分开







## 下界通配符 < ? super E>

> 下界: 用 super 进行声明，表示参数化的类型可能是所指定的类型，或者是此类型的父类型，直至 Object

在类型参数中使用 super 表示这个泛型中的参数必须是 E 或者 E 的父类。

```java
private <T> void test(List<? super T> dst, List<T> src){
    for (T t : src) {
        dst.add(t);
    }
}

public static void main(String[] args) {
    List<Dog> dogs = new ArrayList<>();
    List<Animal> animals = new ArrayList<>();
    new Test3().test(animals,dogs);
}
// Dog 是 Animal 的子类
class Dog extends Animal {

}COPY
```

dst 类型 “大于等于” src 的类型，这里的“大于等于”是指 dst 表示的范围比 src 要大，因此装得下 dst 的容器也就能装 src 。





## **？和 T 的区别**

![img](java面试.assets/6b60e329-6c05-4357-9c58-a4eae41d9dad.jpg)

？和 T 都表示不确定的类型，区别在于我们可以对 T 进行操作，但是对 ？不行，比如如下这种 ：

```java
// 可以
T t = operate();

// 不可以
？car = operate();COPY
```

简单总结下：

T 是一个 确定的 类型，通常用于泛型类和泛型方法的定义，？是一个 不确定 的类型，通常用于泛型方法的调用代码和形参，不能用于定义类和泛型方法。

#### 区别1：通过 T 来 确保 泛型参数的一致性

```java
// 通过 T 来 确保 泛型参数的一致性
public <T extends Number> void
test(List<T> dest, List<T> src)

//通配符是 不确定的，所以这个方法不能保证两个 List 具有相同的元素类型
public void
test(List<? extends Number> dest, List<? extends Number> src)COPY
```

像下面的代码中，约定的 T 是 Number 的子类才可以，但是申明时是用的 String ，所以就会飘红报错。

![img](java面试.assets/0482c982-d8d3-450a-b976-696f03581a1d.jpg)

不能保证两个 List 具有相同的元素类型的情况

```java
GlmapperGeneric<String> glmapperGeneric = new GlmapperGeneric<>();
List<String> dest = new ArrayList<>();
List<Number> src = new ArrayList<>();
glmapperGeneric.testNon(dest,src);
```

上面的代码在编译器并不会报错，但是当进入到 testNon 方法内部操作时（比如赋值），对于 dest 和 src 而言，就还是需要进行类型转换。



#### 区别2：类型参数可以多重限定而通配符不行

![img](java面试.assets/4b4e84bf-6e54-44b3-8e65-645c8844ce52.jpg)

使用 & 符号设定多重边界（Multi Bounds)，指定泛型类型 T 必须是 MultiLimitInterfaceA 和 MultiLimitInterfaceB 的共有子类型，此时变量 t 就具有了所有限定的方法和属性。对于通配符来说，因为它不是一个确定的类型，所以不能进行多重限定。

#### 区别3：通配符可以使用超类限定而类型参数不行

类型参数 T 只具有 一种 类型限定方式：

```java
T extends ACOPY
```

但是通配符 ? 可以进行 两种限定：

```java
? extends A
? super ACOPY
```







## **class<?>和class< T >区别**

前面介绍了 ？和 T 的区别，那么对于，`Class<T>`和 `<Class<?>`又有什么区别呢？`Class<T>`和 `Class<?>`

最常见的是在反射场景下的使用，这里以用一段发射的代码来说明下。

```
// 通过反射的方式生成  multiLimit
// 对象，这里比较明显的是，我们需要使用强制类型转换
MultiLimit multiLimit = (MultiLimit)
Class.forName("com.glmapper.bridge.boot.generic.MultiLimit").newInstance();
```

对于上述代码，在运行期，如果反射的类型不是 MultiLimit 类，那么一定会报 java.lang.ClassCastException 错误。

对于这种情况，则可以使用下面的代码来代替，使得在在编译期就能直接 检查到类型的问题：

![img](java面试.assets/a91dff4d-efd9-4f8c-9a02-6f4aad9802da.jpg)

`Class<T>`在实例化的时候，T 要替换成具体类。`Class<?>`它是个通配泛型，? 可以代表任何类型，所以主要用于声明时的限制情况。比如，我们可以这样做申明：

```java
// 可以
public Class<?> clazz;
// 不可以，因为 T 需要指定类型
public Class<T> clazzT;
```

所以当不知道定声明什么类型的 Class 的时候可以定义一 个Class<?>。

![img](java面试.assets/86ba7d4d-593e-4056-acf7-adde7c14524c.jpg)

那如果也想 `public Class<T> clazzT;`这样的话，就必须让当前的类也指定 T ，

```java
public class Test3<T> {
    public Class<?> clazz;
    // 不会报错
    public Class<T> clazzT;
```





## **使用泛型的好处**

**1，类型安全。** 泛型的主要目标是提高 Java 程序的类型安全。通过知道使用泛型定义的变量的类型限制，编译器可以在一个高得多的程度上验证类型假设。没有泛型，这些假设就只存在于程序员的头脑中（或者如果幸运的话，还存在于代码注释中）。

 

**2，消除强制类型转换。** 泛型的一个附带好处是，消除源代码中的许多强制类型转换。这使得代码更加可读，并且减少了出错机会。

 

**3，潜在的性能收益。** 泛型为较大的优化带来可能。在泛型的初始实现中，编译器将强制类型转换（没有泛型的话，程序员会指定这些强制类型转换）插入生成的字节码中。但是更多类型信息可用于编译器这一事实，为未来版本的 JVM 的优化带来可能。由于泛型的实现方式，支持泛型（几乎）不需要 JVM 或类文件更改。所有工作都在编译器中完成，编译器生成类似于没有泛型（和强制类型转换）时所写的代码，只是更能确保类型安全而已。

 

**所以泛型只是提高了数据传输安全性，并没有改变程序运行的性能**

**泛型仅仅是java的语法糖，它不会影响java虚拟机生成的汇编代码，在编译阶段，虚拟机就会把泛型的类型擦除，还原成没有泛型的代码，顶多编译速度稍微慢一些，执行速度是完全没有什么区别的.**

 

 



1、创建泛型对象的时候，一定要指出类型变量T的具体类型。争取让编译器检查出错误，而不是留给JVM运行的时候抛出类不匹配的异常。

 2、JVM如何理解泛型概念 —— 类型擦除。事实上，JVM并不知道泛型，所有的泛型在编译阶段就已经被处理成了普通类和方法。 处理方法很简单，我们叫做类型变量T的擦除(erased) 。

 总结：泛型代码与JVM ① 虚拟机中没有泛型，只有普通类和方法。

 ② 在编译阶段，所有泛型类的类型参数都会被Object或者它们的限定边界来替换。(类型擦除) 

③ 在继承泛型类型的时候，桥方法的合成是为了避免类型变量擦除所带来的多态灾难。 无论我们如何定义一个泛型类型，相应的都会有一个原始类型被自动提供。原始类型的名字就是擦除类型参数的泛型类型的名字。







```
以下说法错误的是（D） 反射
```

- ```
  虚拟机中没有泛型，只有普通类和普通方法
  ```

- ```
  所有泛型类的类型参数在编译时都会被擦除
  ```

- ```
  创建泛型对象时请指明类型，让编译器尽早的做参数检查
  ```

- ```
  泛型的类型擦除机制意味着不能在运行时动态获取List&lt;T&gt;中T的实际类型
  ```







 

 

# 拷贝Clone

**clone** 顾名思义就是 **复制** ， 在Java语言中， clone方法被对象调用，所以会复制对象。所谓的复制对象，首先要分配一个和源对象同样大小的空间，在这个空间中创建一个新的对象。

在java对象篇已经提到有五种创建对象的方式

<img src="java面试.assets/image-20220823210035752-166125963727913.png" alt="image-20220823210035752" style="zoom: 50%;" />







## **使用new创建 和clone创建的区别**

new操作符的本意是分配内存。程序执行到new操作符时， 首先去看new操作符后面的类型，因为知道了类型，才能知道要分配多大的内存空间。

分配完内存之后，再调用构造函数，填充对象的各个域，这一步叫做对象的初始化，构造方法返回后，一个对象创建完毕，可以把他的引用（地址）发布到外部，在外部就可以使用这个引用操纵这个对象。

而**clone在第一步是和new相似的， 都是分配内存，调用clone方法时，分配的内存和源对象（即调用clone方法的对象）相同，然后再使用原对象中对应的各个域，填充新对象的域， 填充完成之后，clone方法返回，一个新的相同的对象被创建，同样可以把这个新对象的引用发布到外部** 。







## 复制对象 or 复制引用

在Java中，以下类似的代码非常常见：

```csharp
Person p = new Person(23, "zhang");
Person p1 = p;
System.out.println(p);
System.out.println(p1);
```

打印结果：

```avrasm
com.pansoft.zhangjg.testclone.Person@2f9ee1accom.pansoft.zhangjg.testclone.Person@2f9ee1ac
```

可以看出，打印的地址值是相同的，既然地址都是相同的，那么肯定是同一个对象。p和p1只是引用而已，他们都指向了一个相同的对象Person(23, "zhang") 。 可以把这种现象叫做 **引用的复制** 。







上面代码执行完成之后， 内存中的情景如下图所示：

<img src="java面试.assets/image-20220823211253044-166126037441020.png" alt="image-20220823211253044" style="zoom:50%;" />



而下面的代码是真真正正的克隆了一个对象：

```csharp
Person p = new Person(23, "zhang");
Person p1 = (Person) p.clone();
System.out.println(p);
System.out.println(p1);
```

打印结果:

```avrasm
com.pansoft.zhangjg.testclone.Person@2f9ee1accom.pansoft.zhangjg.testclone.Person@67f1fba0
```

以上代码执行完成后， 内存中的情景如下图所示：

<img src="java面试.assets/image-20220823211337373-166126041909325.png" alt="image-20220823211337373" style="zoom:50%;" />





 















**深拷贝和浅拷贝区别了解吗？什么是引用拷贝？**





浅拷贝：浅拷贝会在堆上创建一个新的对象（区别于引用拷贝的一点），不过，如果原对象内部的属性是引用类型的话，浅拷贝会直接复制内部对象的引用地址，也就是说拷贝对象和原对象共用同一个内部对象。



<img src="java面试.assets/image-20220823213000476-166126140288927.png" alt="image-20220823213000476" style="zoom: 67%;" />



 s2浅拷贝了s1,s1中的基本数据类型与s2中的值相同，但是引用数据类型，例如String类型，只是单纯复制了引用地址





深拷贝 ：深拷贝会完全复制整个对象，包括这个对象所包含的内部对象。

<img src="java面试.assets/image-20220823222425791-166126466874029.png" alt="image-20220823222425791" style="zoom: 80%;" />









## **浅拷贝**

浅拷贝的示例代码如下，我们这里实现了 Cloneable 接口，并重写了 clone() 方法。

clone() 方法的实现很简单，直接调用的是父类 Object 的 clone() 方法。

**Father 作为下面的成员变量**

```
class Father{

    String name;
    public Father(String name){
        this.name=name;
    }

    @Override
    public String toString() {
        return "Father{" +
                "name='" + name + '\'' +
                '}';
    }


}
```





 **被拷贝类**

```
class  Son implements  Cloneable{

    int age;
    String name;
    Father father;
    public Son(String name,int age){
        this.name=name;
        this.age=age;
    }

    public Son(String name,int age,Father father){
        this.name=name;
        this.age=age;
        this.father=father;
    }

    @Override
    public String toString() {
        return "Son{" +
                "age=" + age +
                ", name='" + name + '\'' +
                ", father=" + father +
                '}';
    }
    
    @Override
    protected Son clone() throws CloneNotSupportedException {
        return (Son)super.clone();
    }
}
```



**测试**

```
public class CloneTest {


    public static void main(String[] args) throws CloneNotSupportedException {
        Father father=new Father("龙");
        Son son1=new Son("路飞",20);
        son1.father=father;
        Son son2=son1.clone();


        System.out.println(son1);
        System.out.println(son2);
        System.out.println(son1==son2);//false 由于浅拷贝 生成了不同的对象
        System.out.println(son1.age==son2.age);//true 浅拷贝复制了基本数据类型的值 都为20
        System.out.println(son1.name==son2.name);//true  浅拷贝对于引用数据类型（String）只复制了地址，而 == 比较引用数据类型也是比较两者地址值 所以为true
        
        System.out.println(son1.father.name==son2.father.name);//true
        System.out.println("----------------------------------------------------------");


        son1.father.name="小龙";
        son1.name="小路飞"; //相当于Father的地址发生改变 类似String son2=new String（“小路飞”）; 在堆中重新创建一个对象，地址值发生改变
        System.out.println(son1.father==son2.father);//true
        System.out.println(son1.name==son2.name);//false


    }
```



**需要注意**的是其中name初始`==`是相等的，是因为初始浅拷贝它们指向一个相同的String，而后`son1.name="小龙"` 则改变引用指向。

 









## **深拷贝**

对于上述的问题虽然拷贝的两个对象不同，但其内部的一些引用还是相同的，怎么样绝对的拷贝这个对象，使这个对象完全独立于原对象呢？就使用我们的深拷贝了。

深拷贝：**在对引用数据类型进行拷贝的时候，创建了一个新的对象，并且复制其内的成员变量。**



具体实现深拷贝上，这里提供两个方式，重写clone()方法和序列法。

**重写clone()方法**

如果使用重写clone()方法实现深拷贝，那么要将类中所有自定义引用变量的类也去实现Cloneable接口实现clone()方法。对于字符类可以创建一个新的字符串实现拷贝。

对于上述代码，Father类实现Cloneable接口并重写clone()方法。**son的clone()方法需要对各个引用都拷贝一遍**。



**Father类也要拷贝一份**

```
class Father implements Cloneable{

    String name;

    @Override
    protected Father clone() throws CloneNotSupportedException {
        return (Father) super.clone();
    }

    public Father(String name){
        this.name=name;
    }

    @Override
    public String toString() {
        return "Father{" +
                "name='" + name + '\'' +
                '}';
    }


}
```





拷贝类

```
class  Son implements  Cloneable{

    int age;
    String name;
    Father father;
   .................省略构造方法和toString方法...............................


    @Override
    protected Son clone() throws CloneNotSupportedException {

        Son son= (Son)super.clone();
        son.name=new String(name);
        son.father=father.clone();
        return son;
//        return (Son)super.clone();
    }
}
```









**测试**

```
public class CloneTest {


    public static void main(String[] args) throws CloneNotSupportedException {
        Father father=new Father("龙");
        Son son1=new Son("路飞",20);
        son1.father=father;
        Son son2=son1.clone();


        System.out.println(son1);
        System.out.println(son2);
        System.out.println(son1==son2);//false 创建了两个不同的对象
        System.out.println(son1.age==son2.age);// true int 是基本数据类型 拷贝的是值所有返回true
        System.out.println(son1.name==son2.name);//false 深拷贝 拷贝了两个完全不同的对象，包含了引用数据类型，两者地址在堆中不同
        System.out.println(son1.father.name==son2.father.name);//true
        System.out.println("----------------------------------------------------------");


        son1.father.name="小龙";
        son1.name="小路飞";
        System.out.println(son1.father==son2.father);//false
        System.out.println(son1.name==son2.name);//false


    }



}
```





## 引用拷贝



**那什么是引用拷贝呢？** 简单来说，引用拷贝就是两个不同的引用指向同一个对象。

也就是上文所述的Father类，作为Son的成员变量，无论是否改变son中father中的name的值，始终返回的值都是true

（无论是浅拷贝还是深拷贝）

![image-20220823225257984](java面试.assets/image-20220823225257984-166126637976233.png)

类似

```
User user1 = new User();

User user2 = user1

 
```



<img src="java面试.assets/image-20220823225218863-166126634013031.png" alt="image-20220823225218863" style="zoom: 50%;" />



结合引用拷贝的图解我想我们不难理解这个结果是怎么出现的，因为user1和user2其实指向的是同一个对象,所以当我们修改user2的属性时其实修改的也是user1这个对象。（运行看右侧，分析看左侧）

 

 

 



# 包

## JDK 中常用的包有哪些

- java.lang：这个是系统的基础类；
- java.io：这里面是所有输入输出有关的类，比如文件操作等；
- java.nio：为了完善 io 包中的功能，提高 io 包中性能而写的一个新包；
- java.net：这里面是与网络有关的类；
- java.util：这个是系统辅助类，特别是集合类；
- java.sql：这个是数据库操作的类。
- 

## lang




java.lang包 包含 包装类 、String 类、Math 类 、Class 类 、Object 类







## import java和javax有什么区别

- 刚开始的时候 JavaAPI 所必需的包是 java 开头的包，javax 当时只是扩展 API 包来说使用。然而随着时间的推移，javax 逐渐的扩展成为 Java API 的组成部分。但是，将扩展从 javax 包移动到 java 包将是太麻烦了，最终会破坏一堆现有的代码。因此，最终决定 javax 包将成为标准API的一部分。

```
所以，实际上java和javax没有区别。这都是一个名字。
```





```
定义在同一个包（package）内的类可以不经过import而直接相互使用。
```


















# java序列化



## 什么是序列化?什么是反序列化?

如果我们需要持久化 Java 对象比如将 Java 对象保存在文件中，或者在网络传输 Java 对象，这些场景都需要用到序列化。

简单来说：

- **序列化**： 将数据结构或对象转换成二进制字节流的过程
- **反序列化**：将在序列化过程中所生成的二进制字节流的过程转换成数据结构或者对象的过程

对于 Java 这种面向对象编程语言来说，我们序列化的都是对象（Object）也就是实例化后的类(Class)，但是在 C++这种半面向对象的语言中，struct(结构体)定义的是数据结构类型，而 class 对应的是对象类型。

**序列化的主要目的是通过网络传输对象或者说是将对象存储到文件系统、数据库、内存中。**

![img](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2020-8/a478c74d-2c48-40ae-9374-87aacf05188c.png)

![image-20220824091905105](java面试.assets/image-20220824091905105-166130394654836.png)



## 实际开发中有哪些用到序列化和反序列化的场景？

1. 对象在进行网络传输（比如远程方法调用 RPC 的时候）之前需要先被序列化，接收到序列化的对象之后需要再进行反序列化；
2. 将对象存储到文件中的时候需要进行序列化，将对象从文件中读取出来需要进行反序列化。
3. 将对象存储到缓存数据库（如 Redis）时需要用到序列化，将对象从缓存数据库中读取出来需要反序列化。







![image-20220926154431267](java面试.assets/image-20220926154431267-16641782727481.png)

123 0



Data0bject对象中的word和i的值分别为 

  静态成员属于类级别的，所以不能序列化，序列化只是序列化了对象而已，这里“不能序列化”的意思是序列化信息中不包含这个静态成员域，如果测试都在同一个机器（而且是同一个进程），因为这个jvm已经把i加载进来了，所以获取的是加载好的i，即2，如果是传到另一台机器或者关掉程序重新写个程序读入，此时因为别的机器或新的进程是重新加载i的，所以i信息就是初始时的信息，即0。所以，总结来看，静态成员是不能被序列化的，静态成员定以后的默认初始值是0，所以结果i是0 

  一定要注意是在***另外一个jvm中\***  





**补充：** 

​    Transient 关键字**阻止该变量被序列化到文件中**

1. 在变量声明前加上 Transient 关键字，可以阻止该变量被序列化到文件中，在被反序列化后，
    transient 变量的值被设为初始值，**如 int 型的是 0，对象型的是 null。**
2. 服务器端给客户端发送序列化对象数据，对象中有一些数据是敏感的，比如密码字符串等，希望对
    该密码字段在序列化时，进行加密，而客户端如果拥有解密的密钥，只有在客户端进行反序列化
    时，才可以对密码进行读取，这样可以一定程度保证序列化对象的数据安全。









# Comparable和Comparator

```
public class Student {
    private String username;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public Student(String username, int age) {
        this.username = username;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "username='" + username + '\'' +
                ", age=" + age +
                '}';
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    private int age;







}
```





```
public static void main(String[] args) {
    Student [] students=new Student[]{
      new Student("scl",20),
            new Student("静香",22),
            new Student("胖虎",24)


    };
    Arrays.sort(students);
    System.out.println(Arrays.toString(students));
}
```

显示结果

![image-20220829212252913](java面试.assets/image-20220829212252913-16617793743581.png)



程序抛出了异常，Arrays不是对任意的数组都可以进行操作吗？之前我们就用Arrays.toString成功打印了数组元素都是对象的一个数组呀！

Arrays拿什么排了吗？是按姓名 还是按年龄呢？ 

所以我们要告诉编译器那什么来排序，而这就要用到我们的Comparable接口了





## Comparable接口



`Comparable`是一个排序接口。若一个类实现了`Comparable`接口，即代表该类实现了`compareTo`方法，该方法规定了该类的对象的**比较规则**（两个对象如何比较“大小”）。
类通过实现`o1.compareTo(o2)`方法来比较`o1`和`o2`的大小：

- 若返回正数，意味着`o1`大于`o2`；
- 若返回负数，意味着`o1`小于`o2`；
- 若返回零，则意味着`o1`等于`o2`。

*实现Comparable接口，注意Comparable后要加上要比较的对象的类型<Student>*

```
public class Student implements  Comparable<Student> {
    private String username;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public Student(String username, int age) {
        this.username = username;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "username='" + username + '\'' +
                ", age=" + age +
                '}';
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    private int age;


    @Override
    public int compareTo(Student o) {
        if (this.age>o.age){
            return  1;
        }else if (this.age<o.age){
            return -1;

        }else
        return 0;
    }
}
```

结果

[Student{username='scl', age=20}, Student{username='静香', age=22}, Student{username='胖虎', age=24}]

按照年龄升序排序





但这样会有一个问题，那就是一但写好了排序的方法，那就一下子就写死了。你在compareTo方法里实现的是按年龄比较，那你接下来就只能按年龄来比较。如果想按姓名来比较，那只能够在重写compareTo方法。 

那有什么办法能够解决这个问题呢？

有！那就是我们接下来要讲的比较器：Comparator接口 




## Comparatpr接口

`Comparator`是比较器接口。若我们只需要控制某些对象的次序，而类本身并不支持排序(即没有实现`Comparable`接口)，对于这种情况，我们可以通过建立一个**比较器**（即关于该类的一个`Comparator`接口实现类）来进行排序。
**比较器**需实现方法`compare(T o1, T o2)`，其原理与上面的`o1.compareTo(o2)`方法类似，这里就不再描述。

![image-20220829214251935](java面试.assets/image-20220829214251935-16617805737173.png)





```
class AgeComparator implements Comparator<Student> { // 年龄比较器
    @Override  // 实现Comparator接口中的compare方法
    public int compare(Student o1, Student o2) {
        return o1.getAge() - o2.getAge();
    // 反正返回的也是数字，当o1.age>o2.age时返回大于零的数，即o1对象排在o2对象的后面，升序排列，我们之前用Comparable接口时也可以这样简写
    }
    //如果当前对象应排在参数对象之前, 返回小于 0 的数字;
    //如果当前对象应排在参数对象之后, 返回大于 0 的数字;
    //如果当前对象和参数对象不分先后, 返回 0;
}
```





```
public static void main(String[] args) {
    Student [] students=new Student[]{
      new Student("scl",20),
            new Student("静香",22),
            new Student("胖虎",24)


    };
    AgeComparator ageComparator = new AgeComparator(); // 对年龄比较器进行实例化
    Arrays.sort(students, ageComparator);
    // 用类Arrays.sort对students数组进行排序，这里传了两个参数（学生对象和所对应的年龄比较器）
    System.out.println(Arrays.toString(students));

}
```

同样也可以对姓名进行比较



## 两者的联系

`Comparable`接口实现在自身类中，代表默认排序，相当于**内部比较器**；而`Comparator`接口则是为类外实现的一个比较器，相当于**外部比较器**。





 



