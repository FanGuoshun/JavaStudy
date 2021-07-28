# Java中的访问控制修饰符

Java中，可以使用访问控制符来保护对类、变量、方法和构造方法的访问。Java 支持 4 种不同的访问权限。

- **default** (即默认，什么也不写）: 在同一包内可见，不使用任何修饰符。使用对象：类、接口、变量、方法。
- **private** : 在同一类内可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**
- **public** : 对所有类可见。使用对象：类、接口、变量、方法。
- **protected** : 对同一包内的类和所有子类可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**。
  - **子类与基类在同一包中**：被声明为 protected 的变量、方法和构造器能被同一个包中的任何其他类访问；
  - **子类与基类不在同一包中**：那么在子类中，子类实例可以访问其从基类继承而来的 protected 方法，而不能访问基类实例的protected方法。（只能继承不能访问）

| 修饰符      | 当前类 | 同一包内 | 子孙类(同一包) | 子孙类(不同包)                                               | 其他包 |
| :---------- | :----- | :------- | :------------- | :----------------------------------------------------------- | :----: |
| `public`    | Y      | Y        | Y              | Y                                                            |   Y    |
| `protected` | Y      | Y        | Y              | Y/N（[说明](https://www.runoob.com/java/java-modifier-types.html#protected-desc)） |   N    |
| `default`   | Y      | Y        | Y              | N                                                            |   N    |
| `private`   | Y      | N        | N              | N                                                            |   N    |

# Java中重写的注意事项

- 参数列表与被重写方法的参数列表必须完全相同。
- 返回类型与被重写方法的返回类型可以不相同，但是必须是父类返回值的派生类（java5 及更早版本返回类型要一样，java7 及更高版本可以不同）。
- 访问权限不能比父类中被重写的方法的访问权限更低。例如：如果父类的一个方法被声明为 public，那么在子类中重写该方法就不能声明为 protected。
- 父类的成员方法只能被它的子类重写。
- 声明为 final 的方法不能被重写。
- 声明为 static 的方法不能被重写，但是能够被再次声明。
- **子类和父类在同一个包中，那么子类可以重写父类所有方法，除了声明为 private 和 final 的方法。**
- **子类和父类不在同一个包中，那么子类只能够重写父类的声明为 public 和 protected 的非 final 方法。**
- 重写的方法能够抛出任何非强制异常，无论被重写的方法是否抛出异常。但是，重写的方法不能抛出新的强制性异常，或者比被重写方法声明的更广泛的强制性异常，反之则可以。
- 构造方法不能被重写。
- 如果不能继承一个类，则不能重写该类的方法。

# Java中super和this关键字

**this**

this是自身的一个对象，代表对象本身，可以理解为：指向对象本身的一个指针。

this的用法在java中大体可以分为3种：

**1.普通的直接引用**

这种就不用讲了，this相当于是指向当前对象本身。

**2.形参与成员名字重名，用this来区分：**

```java
class Person {
    private int age = 10;
    public Person(){
    System.out.println("初始化年龄："+age);
}
    public int GetAge(int age){
        this.age = age;
        return this.age;
    }
}
 
public class test1 {
    public static void main(String[] args) {
        Person Harry = new Person();
        System.out.println("Harry's age is "+Harry.GetAge(12));
    }
}       
```

​    

**运行结果：**

**初始化年龄：10**
**Harry's age is 12**

可以看到，这里age是GetAge成员方法的形参，this.age是Person类的成员变量。

**3.引用构造函数**

这个和super放在一起讲，见下面。

**super**

super可以理解为是指向自己超（父）类对象的一个指针，而这个超类指的是离自己最近的一个父类。

super也有三种用法：

1.普通的直接引用

与this类似，super相当于是指向当前对象的父类，这样就可以用super.xxx来引用父类的成员。

2.子类中的成员变量或方法与父类中的成员变量或方法同名

```java
class Country {
    String name;
    void value() {
       name = "China";
    }
}
  
class City extends Country {
    String name;
    void value() {
    name = "Shanghai";
    super.value();      //调用父类的方法
    System.out.println(name);
    System.out.println(super.name);
    }
  
    public static void main(String[] args) {
       City c=new City();
       c.value();
       }
}
```



**运行结果：**

**Shanghai**
**China**

可以看到，这里既调用了父类的方法，也调用了父类的变量。若不调用父类方法value()，只调用父类变量name的话，则父类name值为默认值null。

3.引用构造函数

super（参数）：调用父类中的某一个构造函数（应该为构造函数中的第一条语句）。

this（参数）：调用本类中另一种形式的构造函数（应该为构造函数中的第一条语句）。

```java
class Person { 
    public static void prt(String s) { 
       System.out.println(s); 
    } 
   
    Person() { 
       prt("父类·无参数构造方法： "+"A Person."); 
    }//构造方法(1) 
    
    Person(String name) { 
       prt("父类·含一个参数的构造方法： "+"A person's name is " + name); 
    }//构造方法(2) 
} 
    
public class Chinese extends Person { 
    Chinese() { 
       super(); // 调用父类构造方法（1） 
       prt("子类·调用父类”无参数构造方法“： "+"A chinese coder."); 
    } 
    
    Chinese(String name) { 
       super(name);// 调用父类具有相同形参的构造方法（2） 
       prt("子类·调用父类”含一个参数的构造方法“： "+"his name is " + name); 
    } 
    
    Chinese(String name, int age) { 
       this(name);// 调用具有相同形参的构造方法（3） 
       prt("子类：调用子类具有相同形参的构造方法：his age is " + age); 
    } 
    
    public static void main(String[] args) { 
       Chinese cn = new Chinese(); 
       cn = new Chinese("codersai"); 
       cn = new Chinese("codersai", 18); 
    } 
}
```



**运行结果：**

**父类·无参数构造方法： A Person.**
**子类·调用父类”无参数构造方法“： A chinese coder.**
**父类·含一个参数的构造方法： A person's name is codersai**
**子类·调用父类”含一个参数的构造方法“： his name is codersai**
**父类·含一个参数的构造方法： A person's name is codersai**
**子类·调用父类”含一个参数的构造方法“： his name is codersai**
**子类：调用子类具有相同形参的构造方法：his age is 18**

从本例可以看到，可以用super和this分别调用父类的构造方法和本类中其他形式的构造方法。

例子中Chinese类第三种构造方法调用的是本类中第二种构造方法，而第二种构造方法是调用父类的，因此也要先调用父类的构造方法，再调用本类中第二种，最后是重写第三种构造方法。

**super和this的异同：**

- super（参数）：调用基类中的某一个构造函数（应该为构造函数中的第一条语句） 
- this（参数）：调用本类中另一种形成的构造函数（应该为构造函数中的第一条语句）
- super:　它引用当前对象的直接父类中的成员（用来访问直接父类中被隐藏的父类中成员数据或函数，**基类与派生类中有相同成员定义时如：super.变量名  super.成员函数据名（实参）**
- this：它代表当前对象名（在程序中易产生二义性之处，应使用this来指明当前对象；如果函数的形参与类中的成员数据同名，这时需用this来指明成员变量名）
- **调用super()必须写在子类构造方法的第一行，否则编译不通过。每个子类构造方法的第一条语句，都是隐含地调用super()，如果父类没有这种形式的构造函数，那么在编译的时候就会报错。**
- super()和this()类似,区别是，super()从子类中调用父类的构造方法，this()在同一类内调用其它方法。
- **super()和this()均需放在构造方法内第一行。**
- 尽管可以用this调用一个构造器，但却不能调用两个。
- this和super不能同时出现在一个构造函数里面，因为this必然会调用其它的构造函数，其它的构造函数必然也会有super语句的存在，所以在同一个构造函数里面有相同的语句，就失去了语句的意义，编译器也不会通过。
- this()和super()都指的是对象，所以，均不可以在static环境中使用。包括：static变量,static方法，static语句块。
- 从本质上讲，this是一个指向本对象的指针, 然而super是一个Java关键字。

# 接口和抽象类

对于面向对象编程来说，抽象是它的一大特征之一。在Java中，可以通过两种形式来体现OOP的抽象：接口和抽象类。这两者有太多相似的地方，又有太多不同的地方。很多人在初学的时候会以为它们可以随意互换使用，但是实际则不然。今天我们就一起来学习一下Java中的接口和抽象类。下面是本文的目录大纲：

　　一.抽象类

　　二.接口

　　三.抽象类和接口的区别

　　若有不正之处，请多多谅解并欢迎批评指正，不甚感激。

　　请尊重作者劳动成果，转载请标明原文链接：

　　http://www.cnblogs.com/dolphin0520/p/3811437.html

## 一.抽象类

　　在了解抽象类之前，先来了解一下抽象方法。抽象方法是一种特殊的方法：它只有声明，而没有具体的实现。抽象方法的声明格式为：

```
abstract` `void` `fun();
```

　　抽象方法必须用abstract关键字进行修饰。如果一个类含有抽象方法，则称这个类为抽象类，抽象类必须在类前用abstract关键字修饰。因为抽象类中含有无具体实现的方法，所以不能用抽象类创建对象。

　　下面要注意一个问题：在《JAVA编程思想》一书中，将抽象类定义为“包含抽象方法的类”，但是后面发现如果一个类不包含抽象方法，只是用abstract修饰的话也是抽象类。也就是说抽象类不一定必须含有抽象方法。个人觉得这个属于钻牛角尖的问题吧，因为如果一个抽象类不包含任何抽象方法，为何还要设计为抽象类？所以暂且记住这个概念吧，不必去深究为什么。

```
[``public``] ``abstract` `class` `ClassName {``  ``abstract` `void` `fun();``}
```

　　从这里可以看出，抽象类就是为了继承而存在的，如果你定义了一个抽象类，却不去继承它，那么等于白白创建了这个抽象类，因为你不能用它来做任何事情。对于一个父类，如果它的某个方法在父类中实现出来没有任何意义，必须根据子类的实际需求来进行不同的实现，那么就可以将这个方法声明为abstract方法，此时这个类也就成为abstract类了。

　　包含抽象方法的类称为抽象类，但并不意味着抽象类中只能有抽象方法，它和普通类一样，同样可以拥有成员变量和普通的成员方法。注意，抽象类和普通类的主要有三点区别：

　　1）抽象方法必须为public或者protected（因为如果为private，则不能被子类继承，子类便无法实现该方法），缺省情况下默认为public。

　　2）抽象类不能用来创建对象；

　　3）如果一个类继承于一个抽象类，则子类必须实现父类的抽象方法。如果子类没有实现父类的抽象方法，则必须将子类也定义为为abstract类。

　　在其他方面，抽象类和普通的类并没有区别。

## 二.接口

　　接口，英文称作interface，在软件工程中，接口泛指供别人调用的方法或者函数。从这里，我们可以体会到Java语言设计者的初衷，它是对行为的抽象。在Java中，定一个接口的形式如下：

```
[``public``] ``interface` `InterfaceName {` `}
```

　　接口中可以含有 变量和方法。但是要注意，接口中的变量会被隐式地指定为public static final变量（并且只能是public static final变量，用private修饰会报编译错误），而方法会被隐式地指定为public abstract方法且只能是public abstract方法（用其他关键字，比如private、protected、static、 final等修饰会报编译错误），并且接口中所有的方法不能有具体的实现，也就是说，接口中的方法必须都是抽象方法。从这里可以隐约看出接口和抽象类的区别，接口是一种极度抽象的类型，它比抽象类更加“抽象”，并且一般情况下不在接口中定义变量。

　　要让一个类遵循某组特地的接口需要使用implements关键字，具体格式如下：

```
class` `ClassName ``implements` `Interface1,Interface2,[....]{``}
```

　　可以看出，允许一个类遵循多个特定的接口。如果一个非抽象类遵循了某个接口，就必须实现该接口中的所有方法。对于遵循某个接口的抽象类，可以不实现该接口中的抽象方法。

## 三.抽象类和接口的区别

1.语法层面上的区别

　　**1）抽象类可以提供成员方法的实现细节，而接口中只能存在public abstract 方法；**

　　**2）抽象类中的成员变量可以是各种类型的，而接口中的成员变量只能是public static final类型的；**

　　**3）接口中不能含有静态代码块以及静态方法，而抽象类可以有静态代码块和静态方法；**

　　**4）一个类只能继承一个抽象类，而一个类却可以实现多个接口。**

2.设计层面上的区别

　　1）抽象类是对一种事物的抽象，即对类抽象，而接口是对行为的抽象。抽象类是对整个类整体进行抽象，包括属性、行为，但是接口却是对类局部（行为）进行抽象。举个简单的例子，飞机和鸟是不同类的事物，但是它们都有一个共性，就是都会飞。那么在设计的时候，可以将飞机设计为一个类Airplane，将鸟设计为一个类Bird，但是不能将 飞行 这个特性也设计为类，因此它只是一个行为特性，并不是对一类事物的抽象描述。此时可以将 飞行 设计为一个接口Fly，包含方法fly( )，然后Airplane和Bird分别根据自己的需要实现Fly这个接口。然后至于有不同种类的飞机，比如战斗机、民用飞机等直接继承Airplane即可，对于鸟也是类似的，不同种类的鸟直接继承Bird类即可。从这里可以看出，继承是一个 "是不是"的关系，而 接口 实现则是 "有没有"的关系。如果一个类继承了某个抽象类，则子类必定是抽象类的种类，而接口实现则是有没有、具备不具备的关系，比如鸟是否能飞（或者是否具备飞行这个特点），能飞行则可以实现这个接口，不能飞行就不实现这个接口。

　　2）设计层面不同，抽象类作为很多子类的父类，它是一种模板式设计。而接口是一种行为规范，它是一种辐射式设计。什么是模板式设计？最简单例子，大家都用过ppt里面的模板，如果用模板A设计了ppt B和ppt C，ppt B和ppt C公共的部分就是模板A了，如果它们的公共部分需要改动，则只需要改动模板A就可以了，不需要重新对ppt B和ppt C进行改动。而辐射式设计，比如某个电梯都装了某种报警器，一旦要更新报警器，就必须全部更新。也就是说对于抽象类，如果需要添加新的方法，可以直接在抽象类中添加具体的实现，子类可以不进行变更；而对于接口则不行，如果接口进行了变更，则所有实现这个接口的类都必须进行相应的改动。

　　下面看一个网上流传最广泛的例子：门和警报的例子：门都有open( )和close( )两个动作，此时我们可以定义通过抽象类和接口来定义这个抽象概念：

```
abstract` `class` `Door {``  ``public` `abstract` `void` `open();``  ``public` `abstract` `void` `close();``}
```

　　或者：

```
interface` `Door {``  ``public` `abstract` `void` `open();``  ``public` `abstract` `void` `close();``}
```

　　但是现在如果我们需要门具有报警alarm( )的功能，那么该如何实现？下面提供两种思路：

　　1）将这三个功能都放在抽象类里面，但是这样一来所有继承于这个抽象类的子类都具备了报警功能，但是有的门并不一定具备报警功能；

　　2）将这三个功能都放在接口里面，需要用到报警功能的类就需要实现这个接口中的open( )和close( )，也许这个类根本就不具备open( )和close( )这两个功能，比如火灾报警器。

　　从这里可以看出， Door的open() 、close()和alarm()根本就属于两个不同范畴内的行为，open()和close()属于门本身固有的行为特性，而alarm()属于延伸的附加行为。因此最好的解决办法是单独将报警设计为一个接口，包含alarm()行为,Door设计为单独的一个抽象类，包含open和close两种行为。再设计一个报警门继承Door类和实现Alarm接口。

```
interface` `Alram {``  ``void` `alarm();``}` `abstract` `class` `Door {``  ``void` `open();``  ``void` `close();``}` `class` `AlarmDoor ``extends` `Door ``implements` `Alarm {``  ``void` `oepn() {``   ``//....``  ``}``  ``void` `close() {``   ``//....``  ``}``  ``void` `alarm() {``   ``//....``  ``}``}
```

# Java中成员变量，局部变量和静态变量

|          | **成员变量**        | **局部变量**                       | **静态变量**        |
| -------- | ------------------- | ---------------------------------- | ------------------- |
| 定义位置 | *在类中**,**方法外* | *方法中**,**或者方法的形式参数*    | *在类中**,**方法外* |
| 初始化值 | 有默认初始化值      | *无**,**先定义**,**赋值后才能使用* | 有默认初始化值      |
| 调用方式 | 对象调用            | ---                                | 对象调用，类名调用  |
| 存储位置 | 堆中                | 栈中                               | 方法区              |
| 生命周期 | 与对象共存亡        | 与方法共存亡                       | 与类共存亡          |
| 别名     | 实例变量            | ---                                | 类变量              |

# Java中equals()和hashCode（）

**1. 第一种 不会创建“类对应的散列表”**

​     这里所说的“不会创建类对应的散列表”是说：我们不会在HashSet, Hashtable, HashMap等等这些本质是散列表的数据结构中，用到该类。例如，不会创建该类的HashSet集合。

​    在这种情况下，该类的“hashCode() 和 equals() ”没有半毛钱关系的！
​    这种情况下，equals() 用来比较该类的两个对象是否相等。而hashCode() 则根本没有任何作用，所以，不用理会hashCode()。

下面，我们通过示例查看类的**两个对象相等** 以及 **不等**时hashCode()的取值。

**源码如下** (NormalHashCodeTest.java)：

```java
 1 import java.util.*;
 2 import java.lang.Comparable;
 3 
 4 /**
 5  * @desc 比较equals() 返回true 以及 返回false时， hashCode()的值。
 6  *
 7  * @author skywang
 8  * @emai kuiwu-wang@163.com
 9  */
10 public class NormalHashCodeTest{
11 
12     public static void main(String[] args) {
13         // 新建2个相同内容的Person对象，
14         // 再用equals比较它们是否相等
15         Person p1 = new Person("eee", 100);
16         Person p2 = new Person("eee", 100);
17         Person p3 = new Person("aaa", 200);
18         System.out.printf("p1.equals(p2) : %s; p1(%d) p2(%d)\n", p1.equals(p2), p1.hashCode(), p2.hashCode());
19         System.out.printf("p1.equals(p3) : %s; p1(%d) p3(%d)\n", p1.equals(p3), p1.hashCode(), p3.hashCode());
20     }
21 
22     /**
23      * @desc Person类。
24      */
25     private static class Person {
26         int age;
27         String name;
28 
29         public Person(String name, int age) {
30             this.name = name;
31             this.age = age;
32         }
33 
34         public String toString() {
35             return name + " - " +age;
36         }
37 
38         /** 
39          * @desc 覆盖equals方法 
40          */  
41         public boolean equals(Object obj){  
42             if(obj == null){  
43                 return false;  
44             }  
45               
46             //如果是同一个对象返回true，反之返回false  
47             if(this == obj){  
48                 return true;  
49             }  
50               
51             //判断是否类型相同  
52             if(this.getClass() != obj.getClass()){  
53                 return false;  
54             }  
55               
56             Person person = (Person)obj;  
57             return name.equals(person.name) && age==person.age;  
58         } 
59     }
60 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

**运行结果**：

```
p1.equals(p2) : true; p1(1169863946) p2(1901116749)
p1.equals(p3) : false; p1(1169863946) p3(2131949076)
```

从结果也可以看出：**p1和p2相等的情况下，hashCode()也不一定相等。**

 

**2. 第二种 会创建“类对应的散列表”**

​    这里所说的“会创建类对应的散列表”是说：我们会在HashSet, Hashtable, HashMap等等这些本质是散列表的数据结构中，用到该类。例如，会创建该类的HashSet集合。

​    在这种情况下，该类的“hashCode() 和 equals() ”是有关系的：
​    1)、如果两个对象相等，那么它们的hashCode()值一定相同。
​       这里的相等是指，通过equals()比较两个对象时返回true。
​    2)、如果两个对象hashCode()相等，它们并不一定相等。
​        因为在散列表中，hashCode()相等，即两个键值对的哈希值相等。然而哈希值相等，并不一定能得出键值对相等。补充说一句：“两个不同的键值对，哈希值相等”，这就是哈希冲突。

​    此外，在这种情况下。若要判断两个对象是否相等，除了要覆盖equals()之外，也要覆盖hashCode()函数。否则，equals()无效。
例如，创建Person类的HashSet集合，必须同时覆盖Person类的equals() 和 hashCode()方法。
​    如果单单只是覆盖equals()方法。我们会发现，equals()方法没有达到我们想要的效果。

**参考代码** (ConflictHashCodeTest1.java)：

```java
 1 import java.util.*;
 2 import java.lang.Comparable;
 3 
 4 /**
 5  * @desc 比较equals() 返回true 以及 返回false时， hashCode()的值。
 6  *
 7  * @author skywang
 8  * @emai kuiwu-wang@163.com
 9  */
10 public class ConflictHashCodeTest1{
11 
12     public static void main(String[] args) {
13         // 新建Person对象，
14         Person p1 = new Person("eee", 100);
15         Person p2 = new Person("eee", 100);
16         Person p3 = new Person("aaa", 200);
17 
18         // 新建HashSet对象 
19         HashSet set = new HashSet();
20         set.add(p1);
21         set.add(p2);
22         set.add(p3);
23 
24         // 比较p1 和 p2， 并打印它们的hashCode()
25         System.out.printf("p1.equals(p2) : %s; p1(%d) p2(%d)\n", p1.equals(p2), p1.hashCode(), p2.hashCode());
26         // 打印set
27         System.out.printf("set:%s\n", set);
28     }
29 
30     /**
31      * @desc Person类。
32      */
33     private static class Person {
34         int age;
35         String name;
36 
37         public Person(String name, int age) {
38             this.name = name;
39             this.age = age;
40         }
41 
42         public String toString() {
43             return "("+name + ", " +age+")";
44         }
45 
46         /** 
47          * @desc 覆盖equals方法 
48          */  
49         @Override
50         public boolean equals(Object obj){  
51             if(obj == null){  
52                 return false;  
53             }  
54               
55             //如果是同一个对象返回true，反之返回false  
56             if(this == obj){  
57                 return true;  
58             }  
59               
60             //判断是否类型相同  
61             if(this.getClass() != obj.getClass()){  
62                 return false;  
63             }  
64               
65             Person person = (Person)obj;  
66             return name.equals(person.name) && age==person.age;  
67         } 
68     }
69 }
```



**运行结果**：

```
p1.equals(p2) : true; p1(1169863946) p2(1690552137)
set:[(eee, 100), (eee, 100), (aaa, 200)]
```

**结果分析**：

​    我们重写了Person的equals()。但是，很奇怪的发现：HashSet中仍然有重复元素：p1 和 p2。为什么会出现这种情况呢？

​    这是因为虽然p1 和 p2的内容相等，但是它们的hashCode()不等；所以，HashSet在添加p1和p2的时候，认为它们不相等。



下面，我们同时覆盖equals() 和 hashCode()方法。

参考代码 (ConflictHashCodeTest2.java)：

```java
 1 import java.util.*;
 2 import java.lang.Comparable;
 3 
 4 /**
 5  * @desc 比较equals() 返回true 以及 返回false时， hashCode()的值。
 6  *
 7  * @author skywang
 8  * @emai kuiwu-wang@163.com
 9  */
10 public class ConflictHashCodeTest2{
11 
12     public static void main(String[] args) {
13         // 新建Person对象，
14         Person p1 = new Person("eee", 100);
15         Person p2 = new Person("eee", 100);
16         Person p3 = new Person("aaa", 200);
17         Person p4 = new Person("EEE", 100);
18 
19         // 新建HashSet对象 
20         HashSet set = new HashSet();
21         set.add(p1);
22         set.add(p2);
23         set.add(p3);
24 
25         // 比较p1 和 p2， 并打印它们的hashCode()
26         System.out.printf("p1.equals(p2) : %s; p1(%d) p2(%d)\n", p1.equals(p2), p1.hashCode(), p2.hashCode());
27         // 比较p1 和 p4， 并打印它们的hashCode()
28         System.out.printf("p1.equals(p4) : %s; p1(%d) p4(%d)\n", p1.equals(p4), p1.hashCode(), p4.hashCode());
29         // 打印set
30         System.out.printf("set:%s\n", set);
31     }
32 
33     /**
34      * @desc Person类。
35      */
36     private static class Person {
37         int age;
38         String name;
39 
40         public Person(String name, int age) {
41             this.name = name;
42             this.age = age;
43         }
44 
45         public String toString() {
46             return name + " - " +age;
47         }
48 
49         /** 
50          * @desc重写hashCode 
51          */  
52         @Override
53         public int hashCode(){  
54             int nameHash =  name.toUpperCase().hashCode();
55             return nameHash ^ age;
56         }
57 
58         /** 
59          * @desc 覆盖equals方法 
60          */  
61         @Override
62         public boolean equals(Object obj){  
63             if(obj == null){  
64                 return false;  
65             }  
66               
67             //如果是同一个对象返回true，反之返回false  
68             if(this == obj){  
69                 return true;  
70             }  
71               
72             //判断是否类型相同  
73             if(this.getClass() != obj.getClass()){  
74                 return false;  
75             }  
76               
77             Person person = (Person)obj;  
78             return name.equals(person.name) && age==person.age;  
79         } 
80     }
81 }
```

**运行结果**：

```
p1.equals(p2) : true; p1(68545) p2(68545)
p1.equals(p4) : false; p1(68545) p4(68545)
set:[aaa - 200, eee - 100]
```

**结果分析**：

​    这下，equals()生效了，HashSet中没有重复元素。
​    *比较p1和p2*，我们发现：它们的hashCode()相等，通过equals()比较它们也返回true。所以，p1和p2被视为相等。
​    *比较p1和p4*，我们发现：虽然它们的hashCode()相等；但是，通过equals()比较它们返回false。所以，p1和p4被视为不相等。

  # Java中只有值传递

首先回顾一下在程序设计语言中有关将参数传递给方法（或函数）的一些专业术语。**按值调用(call by value)表示方法接收的是调用者提供的值，而按引用调用（call by reference)表示方法接收的是调用者提供的变量地址。一个方法可以修改传递引用所对应的变量值，而不能修改传递值调用所对应的变量值。** 它用来描述各种程序设计语言（不只是 Java)中方法参数传递方式。

**Java 程序设计语言总是采用按值调用。也就是说，方法得到的是所有参数值的一个拷贝，也就是说，方法不能修改传递给它的任何参数变量的内容。**

**下面通过 3 个例子来给大家说明**

> **example 1**

```java
public static void main(String[] args) {
    int num1 = 10;
    int num2 = 20;

    swap(num1, num2);

    System.out.println("num1 = " + num1);
    System.out.println("num2 = " + num2);
}

public static void swap(int a, int b) {
    int temp = a;
    a = b;
    b = temp;

    System.out.println("a = " + a);
    System.out.println("b = " + b);
}
```

**结果：**

```
a = 20
b = 10
num1 = 10
num2 = 20
```

**解析：**

![example 1 ](https://user-gold-cdn.xitu.io/2020/1/10/16f8fd19492a80e2?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

在 swap 方法中，a、b 的值进行交换，并不会影响到 num1、num2。因为，a、b 中的值，只是从 num1、num2 的复制过来的。也就是说，a、b 相当于 num1、num2 的副本，副本的内容无论怎么修改，都不会影响到原件本身。

**通过上面例子，我们已经知道了一个方法不能修改一个基本数据类型的参数，而对象引用作为参数就不一样，请看 example2.**

> **example 2**

```java
    public static void main(String[] args) {
        int[] arr = { 1, 2, 3, 4, 5 };
        System.out.println(arr[0]);
        change(arr);
        System.out.println(arr[0]);
    }

    public static void change(int[] array) {
        // 将数组的第一个元素变为0
        array[0] = 0;
    }
```

**结果：**

```
1
0
```

**解析：**

![example 2](https://user-gold-cdn.xitu.io/2020/1/10/16f8fd19700b5ebf?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

array 被初始化 arr 的拷贝也就是一个对象的引用，也就是说 array 和 arr 指向的是同一个数组对象。 因此，外部对引用对象的改变会反映到所对应的对象上。

**通过 example2 我们已经看到，实现一个改变对象参数状态的方法并不是一件难事。理由很简单，方法得到的是对象引用的拷贝，对象引用及其他的拷贝同时引用同一个对象。**

**很多程序设计语言（特别是，C++和 Pascal)提供了两种参数传递的方式：值调用和引用调用。有些程序员（甚至本书的作者）认为 Java 程序设计语言对对象采用的是引用调用，实际上，这种理解是不对的。由于这种误解具有一定的普遍性，所以下面给出一个反例来详细地阐述一下这个问题。**

> **example 3**

```java
public class Test {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        Student s1 = new Student("小张");
        Student s2 = new Student("小李");
        Test.swap(s1, s2);
        System.out.println("s1:" + s1.getName());
        System.out.println("s2:" + s2.getName());
    }

    public static void swap(Student x, Student y) {
        Student temp = x;
        x = y;
        y = temp;
        System.out.println("x:" + x.getName());
        System.out.println("y:" + y.getName());
    }
}
```

**结果：**

```
x:小李
y:小张
s1:小张
s2:小李复制代码
```

**解析：**

交换之前：

![img](https://user-gold-cdn.xitu.io/2020/1/10/16f8fd199efb83af?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

交换之后：

![img](https://user-gold-cdn.xitu.io/2020/1/10/16f8fd19c6058795?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

通过上面两张图可以很清晰的看出： **方法并没有改变存储在变量 s1 和 s2 中的对象引用。swap 方法的参数 x 和 y 被初始化为两个对象引用的拷贝，这个方法交换的是这两个拷贝**

> **总结**

Java 程序设计语言对对象采用的不是引用调用，实际上，对象引用是按值传递的。

下面再总结一下 Java 中方法参数的使用情况：

- **一个方法不能修改一个基本数据类型的参数（即数值型或布尔型）。**
- **一个方法可以改变一个对象参数的状态。**
- **一个方法不能让对象参数引用一个新的对象。**

# yield和join方法

一、线程与进程
1、进程是程序（任务）执行过程，持有资源（共享内存，共享文件）和线程，进程是动态性的，如果程序没有执行就不算一个进程。
2、线程是系统中最小的执行单元，同一进程中有多个线程，线程共享进程的资源

Java中创建现成的方式就不再赘述了，有两种：（1）继承Thread类，重写run()方法，（2）实现Runnable接口，重写run()方法。

二、yield()方法
在jdk文档中，yield()方法是这样描述的：暂停当前正在执行的线程对象，并执行其他线程。在多线程的情况下，由CPU决定执行哪一个线程，而yield()方法就是暂停当前的线程，让给其他线程（包括它自己）执行，具体由谁执行由CPU决定。

yield()方法实例
YieldRunnable.java
public class YieldRunnable implements Runnable{
	public volatile boolean isRunning = true;
	
	@Override
	public void run() {
		String name = Thread.currentThread().getName();
		System.out.println(name + "开始执行！");
		
		while(isRunning) {
			for(int i = 1; i < 6; i++) {
				System.out.println(name + "执行了[" + i + "]次");
				//注意，yield是静态方法
				Thread.yield();
			}
		}
		
		System.out.println(name + "执行结束！");
	}
}
执行的main函数
	public static void main(String[] args) {
		YieldRunnable runnable1 = new YieldRunnable();
		YieldRunnable runnable2 = new YieldRunnable();
		Thread thread1 = new Thread(runnable1, "线程1");
		Thread thread2 = new Thread(runnable2, "线程2"); 
		
		System.out.println("两个线程准备开始执行");
		thread1.start();
		thread2.start();
		
		try {
			Thread.sleep(10);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		
		runnable1.isRunning = false;
		runnable2.isRunning = false;
	}
选取部分执行结果分析：
1、第一种情况：

……

线程1执行了[1]次
线程2执行了[1]次
线程2执行了[2]次
线程2执行了[3]次
线程2执行了[4]次
线程1执行了[2]次
……
从这个执行结果可以看出，线程1在执行了一次之后，让出执行权，CPU将执行权给了线程2，线程2执行了一次之后让出执行权，但是CPU仍将执行权给2，就这样线程2反复执行了几次，CPU一直都是将执行权给线程2，知道线程2执行完第4次之后，CPU才将执行权给线程1。

2、第二种情况：

……

线程1执行了[1]次
线程2执行了[3]次
线程1执行了[2]次
线程2执行了[4]次
线程1执行了[3]次
线程2执行了[5]次
……

从这个执行结果可以看出，线程1执行一次后，让出执行权，CPU将执行权给了线程2，线程2执行一次后，CPU将执行权又给了1，就这样交替执行了几次线程1和线程2。

从上面两种执行结果来看，一个线程执行yield()方法暂停后，CPU决定接下来有哪一个线程执行，可以是其他线程，也可以是原来的那个线程。


三、join()方法
join()方法是指等待调用join()方法的线程执行结束，程序才会继续执行下去，这个方法适用于：一个执行程序必须等待另一个线程的执行结果才能够继续运行的情况。
join()方法实例
定义一个线程：
JoinRunnable.java
public class JoinRunnable implements Runnable {
	@Override
	public void run() {
		String name = Thread.currentThread().getName();
		System.out.println(name + "开始执行！");
		
		for(int i = 1; i < 6; i++) {
			System.out.println(name + "执行了[" + i + "]次");
		}
	}
}

先来看一下没有使用join()方法的情况：
	public static void main(String[] args) {
		JoinRunnable runnable1 = new JoinRunnable();
		Thread thread1 = new Thread(runnable1, "线程1");

		System.out.println("主线程开始执行！");
		thread1.start();
	 
		System.out.println("主线程执行结束！");
	}
执行结果：
主线程开始执行！
主线程执行结束！
线程1开始执行！
线程1执行了[1]次
线程1执行了[2]次
线程1执行了[3]次
线程1执行了[4]次
线程1执行了[5]次

从执行结果可以看出，如果没有使用join方法，那么main方法所在的线程（暂定叫主线程）在启动了JoinRunnable线程（暂定叫子线程）之后，没有等待子线程执行结束，就先执行结束了。

如果使用了join方法：
	public static void main(String[] args) {
		JoinRunnable runnable1 = new JoinRunnable();
		Thread thread1 = new Thread(runnable1, "线程1");

		System.out.println("主线程开始执行！");
		thread1.start();
		
		try {
			thread1.join();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		
		System.out.println("主线程执行结束！");
	}
执行结果：
主线程开始执行！
线程1开始执行！
线程1执行了[1]次
线程1执行了[2]次
线程1执行了[3]次
线程1执行了[4]次
线程1执行了[5]次
主线程执行结束！

从执行结果可以看出，加入join()方法，主线程启动了子线程之后，在等待子线程执行完毕才继续执行下面的操作。

注意：join()方法只有在start()方法之后才可以生效。可以参考下面的代码，将join()方法写在start()方法之前。
	public static void main(String[] args) {
		JoinRunnable runnable1 = new JoinRunnable();
		Thread thread1 = new Thread(runnable1, "线程1");

		System.out.println("主线程开始执行！");
		try {
			thread1.join();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		thread1.start();
	 
		System.out.println("主线程执行结束！");
	}
执行结果：
主线程开始执行！
主线程执行结束！
线程1开始执行！
线程1执行了[1]次
线程1执行了[2]次
线程1执行了[3]次
线程1执行了[4]次
线程1执行了[5]次

虽然加入了join()方法，但是是在线程启动的start()方法之前调用的，所以join()方法不生效。具体原因可以参考一下源码
    public final void join()
        throws InterruptedException
    {
        join(0L);
    }
    public final synchronized void join(long l)
        throws InterruptedException
    {
        long l1 = System.currentTimeMillis();
        long l2 = 0L;
        if(l < 0L)
            throw new IllegalArgumentException("timeout value is negative");
        if(l == 0L)
            for(; isAlive(); wait(0L));//isAlive()是判断当前线程的状态，wait(0)是指等待直到线程结束
        else
            do
            {
                if(!isAlive())
                    break;
                long l3 = l - l2;
                if(l3 <= 0L)
                    break;
                wait(l3);
                l2 = System.currentTimeMillis() - l1;
            } while(true);
    }
从源码可以看出，join()方法调用了join(long l)方法，参数l的值为0，之后在判断中如果l的值为0，则执行一个循环，循环执行的条件为当前线程是否isAlive()，就是指线程是否已经运行，如果线程未启动（即未调用start方法），则循环条件不成立，直接退出，也就是说join()方法不生效。
在看这里的源码的时候发现了一个小问题，对于isAlive()和wait()方法的作用对象是有点让人疑惑的，isAlive()判断当前线程的状态，还好理解，在上面的例子中就是指thread1。但是wait()方法的作用对象到底是谁，产生了疑问，按照jdk的说法等待的应该是当前线程，那就是thread1，但是实际运行中，等待的是主线程，有点让人摸不着头脑。之后在网上找到 这篇文章，里面说当我们在synchronized块 中调用锁对象的wait()方法，那么调用线程就会阻塞在wait()语句那里，直到其他线程调用线程的notify()方法。这里的调用线程也就是指主线程，因为wai()方法是在Thread类中的synchronized方法join(long l)中调用的，所以主线程阻塞，直到线程执行完毕调用了notify()方法才继续执行，这个说法先记录一下，以备以后考证。

最后一点小插曲：
在编码的时候尝试过在start()方法和join()方法之间加入一些内容，也就是说调用start()方法启动线程之后不立即调用join()方法，想看一下是否出现：先启动子线程，执行相关操作，然后再执行主线程中的内容，再等待子线程执行结束的情况。示例代码：
	public static void main(String[] args) {
		JoinRunnable runnable1 = new JoinRunnable();
		Thread thread1 = new Thread(runnable1, "线程1");

		System.out.println("主线程开始执行！");
		thread1.start();
		//启动子线程之后主线程休眠1秒	
<span style="white-space:pre">		</span>try {
			Thread.sleep(1000);
		} catch (InterruptedException e1) {
			e1.printStackTrace();
		}
		
		try {
			thread1.join();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	 
		System.out.println("主线程执行结束！");
	}
执行结果：
主线程开始执行！
线程1开始执行！
线程1执行了[1]次
线程1执行了[2]次
线程1执行了[3]次
线程1执行了[4]次
线程1执行了[5]次
主线程执行结束！

实际执行的时候主线程休眠1秒是在子线程执行结束之后才执行的，由此可以推断，只要在start()方法之后调用了join()方法，不管这两个方法之间加入多少内容，主线程都会优先等待子线程执行结束才会继续往下执行。
————————————————
版权声明：本文为CSDN博主「蛋蛋要学编程」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/u011024652/article/details/51583134

# final、finally、 finalize的区别

## 1. final关键字

### 1.1 修饰类

​	用final修饰一个类时，表明这个类不能被继承。final类中的成员变量可以根据需要设为final，但是要注意final类中的所有成员方法都会被隐式地指定为final方法。在使用final修饰类的时候，要注意谨慎选择，除非这个类真的在以后不会用来继承或者出于安全的考虑，尽量不要将类设计为final类。

### 1.2 修饰方法

​	**用final修饰方法时，表明该方法不可被重写，以防止任何继承类修改它的含义。类的private方法会被指定为final方法。**

### 1.3 修饰变量

​	**对于一个final变量，如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改；如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象。**

#### 1.3.1 final不是immutable

```java
 final List<String> strList = new ArrayList<>();
 strList.add("Hello");
 strList.add("world");  
 //上述代码不会报错，因为final 只能约束strList 这个引用不可以被再次赋值，但是 strList 对象行为不被 final 影响，添加元素等操作是完全正常的。
 List<String> unmodifiableStrList = List.of("hello", "world");
 unmodifiableStrList.add("again");
 //最后这句add会抛出异常，因为List.of方法创建的是不可变List
```

​	在这里说句题外话，Immutable 在很多场景是非常棒的选择，某种意义上说，Java 语言目前并没有原生的不可变支持，如果要实现 immutable 的类，我们需要做到：

* 将class自身声明为final。
* 将所有成员变量定义为 private 和 final，并且不要实现 setter 方法。
* 通常构造对象时，成员变量使用深度拷贝来初始化，而不是直接赋值，这是一种防御措施，因为你无法确定输入对象不被其他人修改。
* 如果确实需要实现 getter 方法，或者其他可能会返回内部状态的方法，使用 copy-on-write 原则，创建私有的 copy。

#### 1.3.2 final变量和普通变量对的区别

​	当用final作用于类的成员变量时，成员变量（注意是类的成员变量，局部变量只需要保证在使用之前被初始化赋值即可）必须在定义时或者构造器中进行初始化赋值，而且final变量一旦被初始化赋值之后，就不能再被赋值了。

​	我们先来看下下面这个代码示例：

```java
public class Test {
    public static void main(String[] args)  {
        String a = "hello2"; 
        final String b = "hello";
        String d = "hello";
        String c = b + 2; 
        String e = d + 2;
        System.out.println((a == c));//输出true
        System.out.println((a == e));//输出false
    }
}
```

​	大家可以先想一下这道题的输出结果。为什么第一个比较结果为true，而第二个比较结果为false。这里面就是final变量和普通变量的区别了，**当final变量是基本数据类型以及String类型时，如果在编译期间能知道它的确切值，则编译器会把它当做编译期常量使用。也就是说在用到该final变量的地方，相当于直接访问的这个常量，不需要在运行时确定。**这种和C语言中的宏替换有点像。因此在上面的一段代码中，由于变量b被final修饰，因此会被当做编译器常量，所以在使用到b的地方会直接将变量b 替换为它的 值。而对于变量d的访问却需要在运行时通过链接来进行。想必其中的区别大家应该明白了，不过要注意，**只有在编译期间能确切知道final变量值的情况下，编译器才会进行这样的优化**，比如下面的这段代码就不会进行优化：

```java
public class Test {
    public static void main(String[] args)  {
        String a = "hello2"; 
        final String b = getHello();
        String c = b + 2; 
        System.out.println((a == c));//输出false
    }
    public static String getHello() {
        return "hello";
    }
}
```

#### 1.3.3 final和static

​	很多时候会容易把static和final关键字混淆，**static作用于成员变量用来表示只保存一份副本，而final的作用是用来保证变量不可变**。看下面这个例子：

```java
public class Test {
    public static void main(String[] args)  {
        MyClass myClass1 = new MyClass();
        MyClass myClass2 = new MyClass();
        System.out.println(myClass1.i);
        System.out.println(myClass1.j);
        System.out.println(myClass2.i);//这个i值和上一个i值不同
        System.out.println(myClass2.j);//两个j值都是相同的
    }
}
class MyClass {
    public final double i = Math.random();
    public static double j = Math.random();
}
```

#### 1.3.4 使用final参数真的有用吗？

​	有人曾说“当你在方法中不需要改变作为参数的对象变量时，明确使用final进行声明，会防止你无意的修改而影响到调用方法外的变量”，我个人觉得这么讲根本就不准确：

```java
public class Test{
    public static void main(String[] args){
        MyClass myClass = new MyClass();
        int i = 0;
        myClass.changeValue(i);
    }
}

class MyClass{
    void changeValue(final int i){
        i++; //此处会报错
    }
}
```

​	上面这段代码好像让人觉得用final修饰之后，就不能在方法中更改变量i的值了。殊不知，方法changeValue和main方法中的变量i根本就不是一个变量，因为Java参数传递采用的是值传递，对于基本类型的变量，相当于直接将变量进行了拷贝。所以即使没有final修饰的情况下，在方法内部改变了变量i的值也不会影响方法外的i。

```java
public class Test {
    public static void main(String[] args)  {
        MyClass myClass = new MyClass();
        StringBuffer buffer = new StringBuffer("hello");
        myClass.changeValue(buffer);
        System.out.println(buffer.toString());//输出helloworld
    }
}
 
class MyClass {
    void changeValue(final StringBuffer buffer) {
        buffer.append("world");
    }
}
```

​	运行这段代码就会发现输出结果为 helloworld。很显然，用final进行修饰并没有阻止在changeValue中改变buffer指向的对象的内容。如果我们把final去掉，在changeValue()中让buffer指向其他的对象也不会对方法外的buffer产生什么影响。事实上，Java采用的是值传递，对于引用变量，传递的是引用的值，也就是说让实参和形参同时指向了同一个对象，因此让形参重新指向另一个对象对实参并没有任何影响。

## 2. finally关键字

​	**finally关键字用于try后面，finally块中的代码总是执行，不论是否发生异常。**一般用于清理工作、关闭链接等类型的语句。如果我们在开发中需要关闭连接等资源，更推荐使用 Java 7 中添加的 try-with-resources 语句，因为通常 Java 平台能够更好地处理异常情况，编码量也要少很多。

```java
try {
  // do something
  System.exit(1);
} finally{
  System.out.println(“Print from finally”);
}
```

​	注意上述代码中finally中代码不会被执行，这是一个特例。

## 3. finalize关键字

​	**finalize 是基础类 java.lang.Object 的一个方法，它的设计目的是保证对象在被垃圾收集前完成特定资源的回收。**finalize 机制现在已经不推荐使用，并且在 JDK 9 开始被标记为 deprecated。

​	finalize的调用前提条件：

* 所有对象被Garbage Collection自动调用，比如运行System.gc()的时候。
* 程序退出时为每个对象调用finalize()方法。
* 显式的调用finalize方法。

​     不建议使用finalize方法完成非内存资源的清理工作，但建议用于：

* 清理本地对象（通过JNI创建的对象）。
* 作为确保某些非内存资源的释放（socket，文件，端口等等）。

```java
public class FinalizationDemo {
    public static void main(String[] args) {
        Cake c1 = new Cake(1);
        Cake c2 = new Cake(2);
        Cake c3 = new Cake(3);
        c2 = c3 = null;
        System.gc(); //Invoke the Java garbage collector  
        /**
        运行结果如下：

		Cake Object 1 is created

		Cake Object 2 is created

		Cake Object 3 is created

		Cake Object 3 is disposed

		Cake Object 2 is disposed
        */
    }
}
class Cake extends Object {
    private int id;
    public Cake(int id) {
        this.id = id;
        System.out.println("Cake Object " + id + " is created");
    }
    protected void finalize() throws java.lang.Throwable {
        super.finalize();
        System.out.println("Cake Object " + id + " is disposed");
    }
}  
```

*本博客部分内容参考了杨晓峰的Java核心技术面试精选和归田的博客https://blog.csdn.net/qq924862077/article/details/47950043*

# 枚举类型equals比较

# Exception和Error的区别

## 1.基本概念

​	Exception和Error都是继承自Throwable类，在Java中只有 Throwable 类型的实例才可以被抛出（throw）或者捕获（catch），它是异常处理机制的基本组成类型。

<img src="https://i.loli.net/2020/04/21/Tq975HjLxViNBkQ.png" style="zoom:80%;" />

​	Exception指的是Java程序运行中可预料的异常情况。这种异常可以被捕获进行相应处理。

​	Error指的是错误而不是异常，而是脱离程序员控制的问题。错误在代码中通常被忽略。例如，当栈溢出时，一个错误就发生了。它们在编译期无法被检测到。

​	在Java中Exception又分为Checked Exception和Unchecked Exception。

​	Checked Exception指可检查异常，这类异常在源代码中必须进行显式的捕获处理，这是编译期检查的一部分。从程序语法角度讲这是必须进行处理的异常，如果不处理，程序就不能编译通过。如IOException、SQLException等以及用户自定义的Exception异常，一般情况下不自定义检查异常。 

​	Unchecked Exception指不可检查异常，通常称为运行时异常，类似NullPointerException、ArrayIndexOutOfBoundsException 之类。这类异常只有在代码运行时才能被检测出来，程序中可以选择捕获处理，也可以不处理。这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生。

<img src="https://i.loli.net/2020/04/21/OdslEkhtDZMzgS9.jpg" style="zoom:80%;" />

## 2. ClassNotFoundException和NoClassDefFoundError的区别

​	NoClassDefFoundError是一个错误(Error)，而ClassNOtFoundException是一个异常，在Java中错误和异常是有区别的，我们可以从异常中恢复程序但却不应该尝试从错误中恢复程序。

​	ClassNotFoundException有以下几个产生原因：

​	**Java支持使用Class.forName方法来动态地加载类，任意一个类的类名如果被作为参数传递给这个方法都将导致该类被加载到JVM内存中，如果这个类在类路径中没有被找到，那么此时就会在运行时抛出ClassNotFoundException异常。**

​	要解决这个问题很容易，唯一需要做的就是要确保所需的类连同它依赖的包存在于类路径中。当Class.forName被调用的时候，类加载器会查找类路径中的类，如果找到了那么这个类就会被成功加载，如果没找到，那么就会抛出ClassNotFountException，除了Class.forName，ClassLoader.loadClass、ClassLOader.findSystemClass在动态加载类到内存中的时候也可能会抛出这个异常。

​	**另外还有一个导致ClassNotFoundException的原因就是：当一个类已经某个类加载器加载到内存中了，此时另一个类加载器又尝试着动态地从同一个包中加载这个类。**

​	由于类的动态加载在某种程度上是被开发者所控制的，所以他可以选择catch这个异常然后采取相应的补救措施。有些程序可能希望忽略这个异常而采取其他方法。还有一些程序则会终止程序然后让用户再次尝试前做点事情。

​	NoClassDefFoundError产生的原因：

​	**如果JVM或者ClassLoader实例尝试加载（可以通过正常的方法调用，也可能是使用new来创建新的对象）类的时候却找不到类的定义。要查找的类在编译的时候是存在的，运行的时候却找不到了。这个错误往往是你使用new操作符来创建一个新的对象但却找不到该对象对应的类。这个时候就会导致NoClassDefFoundError.**

​	由于NoClassDefFoundError是有JVM引起的，所以不应该尝试捕捉这个错误。

​	解决这个问题的办法就是：查找那些在开发期间存在于类路径下但在运行期间却不在类路径下的类。

​	我们这里也提供以下三种方法解决NoClassDefFoundError：

* 类在运行的时候依赖于其它的一个jar包，但是该jar包没有加载到classpath中或者是该jar包的名字被其他人改了，就像tibo.jar改为了tibco_v3.jar。
* 运行的类不在classpath中，这个问题没有一个确定的方法去知道，但是很多时候你可以通过`System.getproperty(”java.classpath“)`方法，该方法能让你至少可以领略到实际存在的运行期间的classpath。
* 试着通过`-classpath`命令明确指出你认为正确的classpath，如果能够正常执行的话就说明你使用的classpath是正确的，而系统中的classpath已经被修该过了

​	最后我们再总结一下它们的区别：

​	ClassNotFoundException发生在装入阶段。 当应用程序试图通过类的字符串名称，使用常规的三种方法装入类，但却找不到指定名称的类定义时就抛出该异常。

​	NoClassDefFoundError发生在当目前执行的类已经编译，但是找不到它的定义时。也就是说你如果编译了一个类B，在类A中调用，编译完成以后，你又删除掉B，运行A的时候那么就会出现这个错误。

​	通俗的说，程序在加载时从外存储器找不到需要的class就出现ClassNotFoundException，连接时从内存找不到需要的class就出现NoClassDefFoundError。

## 3.异常处理的两个原则

### 3.1 捕获特定异常而不是通用异常

​	在日常编程中，我们希望我们的代码能够体现出更多的信息，而Exception之类则与我们的目的背道而驰。 另外我们也要保证程序不会捕获到我们不想捕获的异常。Unchecked Exception这类异常不希望被程序捕获，如果我们不小心捕获到了这类异常，可能会造成麻烦。

### 3.2 不要生吞异常

​	在捕获到异常时，如果我们直接忽略掉异常而不对它进行处理，往往会导致难以想象的后果。如果我们不把异常抛出来，或者也没有输出到日志（Logger）之类，程序可能在后续代码以不可控的方式结束。没人能够轻易判断究竟是哪里抛出了异常，以及是什么原因产生了异常。

```java
try {
   // 业务代码
   // …
} catch (IOException e) {
    e.printStackTrace();
}
```

​	上述代码就是一个错误的示范，因为在printStackTrace()函数中，程序会把错误信息打印到标准出错流，这不是一个合适的输出选项，你很难判断错误输出到哪去了。我们在catch到异常时，首先应该将异常输出到错误日志中去。

## 4.Throw early, catch late

```java
public void readPreferences(String fileName){
	 //...perform operations... 
	InputStream in = new FileInputStream(fileName);
	 //...read the preferences file...
}
```

​	大家可以品味上面这段代码。如果 fileName 是 null，那么程序就会抛出 NullPointerException，但是由于没有第一时间暴露出问题，堆栈信息可能非常令人费解，往往需要相对复杂的定位。我们可以修改一下上面这段代码，让异常“throw early”，对应的异常信息就非常直观了。

```java
public void readPreferences(String filename) {
	Objects. requireNonNull(filename);
	//...perform other operations... 
	InputStream in = new FileInputStream(filename);
	 //...read the preferences file...
}
```

​	至于“catch late”，其实是我们经常苦恼的问题，捕获异常后，需要怎么处理呢？最差的处理方式，就是我前面提到的“生吞异常”，本质上其实是掩盖问题。如果实在不知道如何处理，可以选择保留原有异常的 cause 信息，直接再抛出或者构建新的异常抛出去。在更高层面，因为有了清晰的（业务）逻辑，往往会更清楚合适的处理方式是什么。

## 5. 从性能角度来看Java异常处理机制

​	不要使用一个大的try-catch包住一大段代码，而是包住需要捕获异常的代码。因为try-catch代码段会产生额外的性能开销，影响到JVM对代码的优化。

​	Java 每实例化一个 Exception，都会对当时的栈进行快照，这是一个相对比较重的操作。如果发生的非常频繁，这个开销可就不能被忽略了。在程序运行缓慢的时候，我们也可以检查一下Exception是否发生太频繁了。





# Java中transient关键字

Java `transient`关键字用于类属性/变量，表示该类的序列化过程在为该类的任何实例创建持久字节流时应该忽略此类变量。

`transient`变量是不能序列化的变量。根据Java语言规范[[jls-8.3.1.30](https://docs.oracle.com/javase/specs/jls/se7/html/jls-8.html#jls-8.3.1.3)] -“变量可以标记为`transient`，以表明它们不是对象的持久状态的一部分。”

在这篇文章中，我将讨论有关在`serialization`上下文中使用transient`关键字的各种概念。

正文目录如下

## 1. Java transient关键字是什么？

java中的修饰符transient可以应用于类的字段成员，以关闭这些字段成员的序列化。每个标记为`transient`的字段将不会序列化。使用`transient`关键字向java虚拟机表明，`transient`变量不是对象的持久状态的一部分。

让我们写一个非常基本的例子来理解上面的类比到底是什么意思。我将创建一个`Employee`类并定义3个属性，即`firstName`，`lastName`和`confidentialInfo`。由于某些原因，我们不想存储/保存`confidentialInfo`，因此我们将该字段标记为`transient`。



```java
public class Employee implements Serializable {
    private static final long serialVersionUID = 2624368016355021172L;

    private String           firstName;
    private String           lastName;
    private transient String confidentialInfo;

    // Getter and Setter
}
```

现在让我们序列化一个`Employee`类的实例



```csharp
public class TransSerializationTest {
    public static void main(String[] args) {
        try {
            Employee emp = new Employee();
            emp.setFirstName("Chen");
            emp.setLastName("Shuaishuai");
            emp.setConfidentialInfo("password");

            System.out.println("Read before Serialization:");
            System.out.println("firstName: " + emp.getFirstName());
            System.out.println("lastName: " + emp.getLastName());
            System.out.println("confidentialInfo: " + emp.getConfidentialInfo());

            ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("E:/chenss.txt"));
            //Serialize the object
            oos.writeObject(emp);
            oos.close();
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```

现在我们反序列化到Java对象中，并校验`confidentialInfo`对象是否被保存下来？



```csharp
public class TransDeSerializationTest {
    public static void main(String[] args) {
        try {
            ObjectInputStream ooi = new ObjectInputStream(new FileInputStream("E:/chenss.txt"));
            //Read the object back
            Employee readEmpInfo = (Employee) ooi.readObject();
            System.out.println("Read From Serialization:");
            System.out.println("firstName: " + readEmpInfo.getFirstName());
            System.out.println("lastName: " + readEmpInfo.getLastName());
            System.out.println("confidentialInfo: " + readEmpInfo.getConfidentialInfo());
            ooi.close();
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```

运行结果输出：



```csharp
Read From Serialization:
Chen
Shuaishuai
null
```

很明显，`confidentialInfo`在序列化的时候没有被保存到持久对象，这也正是我们在java中使用`transient`关键字的原因。

## 2. 什么时候应该在java中使用transient关键字？

现在我么对于`transient`关键字非常了解了。让我们通过确定需要使用`transient`关键字的场景来扩展理解。

1. 首先，非常符合逻辑的情况是，您可能有**从类实例中的其他字段派生/计算的字段**。它们应该每次都以编程方式计算，而不是通过序列化来持久化状态。一个例子是基于时间戳的值；例如一个人的年龄或时间戳与当前时间戳之间的持续时间。在这两种情况下，您将根据当前系统时间而不是在序列化实例时计算变量的值。
2. 第二个逻辑示例可以是不应该以任何形式(数据库或字节流)泄漏到JVM外部的**任何安全信息**。
3. 另一个例子是JDK或应用程序代码中**没有标记为`Serializable`的字段**。不实现可序列化接口且在任何可序列化类中引用的类不能序列化；并将抛出异常`java.io.NotSerializableException`。在序列化主类之前，这些不可序列化的引用应该标记为`transient`。
4. 最后，有时**序列化某些字段是没有意义的**。例如，在任何类中，如果您添加了一个`logger`引用，那么序列化该`logger`实例有什么用呢？绝对用不上。逻辑上，你只会序列化表示实例状态的信息。`Loggers`从不共享实例的状态。它们只是用于编程/调试目的的实用程序。类似的例子可以参照线程类的引用。线程表示进程在任何给定时间点的状态，并且不需要用实例存储线程状态；仅仅因为它们不构成类实例的状态。

以上四个用例是您应该在引用变量中使用`transient`关键字的时候。如果您有更多可以使用`transient`的逻辑情况，请与我分享，我会在这里更新列表，让每个人都能从你的知识中受益。

> 阅读更多：[实现可序列化接口的简单指南](https://howtodoinjava.com/java/serialization/a-mini-guide-for-implementing-serializable-interface-in-java/)

## 3. Transient 和 final

我说的是在`final`关键字中使用`transient`，因为它在不同的情况下有不同的行为，而java中的其他关键字通常不是这样。

为了使这个概念更实际，我对`Employee`类进行了如下修改:



```java
public class Employee implements Serializable {
    private static final long serialVersionUID = 2624368016355021172L;

    private String           firstName;
    private String           lastName;
    //final field 1
    public final transient String confidentialInfo = "password";
    //final field 2
    private final transient Logger logger = Logger.getLogger("demo");

    //Getter and Setter
}
```

现在当我重新运行序列化（写/读）的时候，会有如下的输出内容：



```csharp
Read From Serialization:
firstName: Chen
lastName: Shuaishuai
confidentialInfo: password
logger: null
```

很奇怪。我们已将`confidentialInfo`标记为`transient`；字段仍然被序列化了。而对于类似的声明，`logger`却没有被序列化。为什么?

原因是，无论何时将任何`final`字段/引用计算为“常量表达式”，JVM都会对其进行序列化，忽略`transient`关键字的存在。

在上面的例子中，值`password`是一个常量表达式，`logger` `demo`的实例是引用。因此，根据规则，`confidentialInfo`被持久化，而`logger`没有被持久化。

您是否在想，如果我从两个字段中删除`transient`呢？那么，实现可序列化引用的字段将保持不变。因此，如果在上面的代码中删除`transient`，`String`(实现`Serializable`)将被持久化；而`Logger`(不实现`Serializable`)将不会被持久化，并且将会抛出异常`java.io.NotSerializableException`。

*如果希望持久保存不可序列化字段的状态，那么可以使用`readObject()`和`writeObject()`方法。`writeObject()`/`readObject()`通常在内部链接到序列化/反序列化机制中，因此会自动调用。*



## 4. 摘要说明

1. 修饰符`transient`可以应用于类的字段成员，以关闭这些字段成员的序列化。
2. 你可以在需要对现有状态字段进行保护或计算的字段的类中使用`transient`关键字。当序列化那些字段(如日志记录器和线程)毫无意义时，可以使用它。
3. 序列化不关心访问修饰符，如`private`；所有非`transient`字段都被认为是对象持久状态的一部分，并且都符合持久状态的条件。
4. 无论何时将任何`final`字段/引用计算为“常量表达式”，JVM都会对其进行序列化，忽略`transient`关键字的存在。
5. `HashMap`类是java中`transient`关键字的一个很好的用例。

# IO流面试题

IO流
先要明白一个基础问题：

1.什么是比特(Bit)？什么是字节(Byte)？什么是字符(Char)？以及他们的区别？

Bit 位，是计算机最小的二进制单位 ，取0或1，主要用于计算机操作。
Byte 字节，是数据的最小单位，由8位bit组成，取值（-128-127），主要用于计算机操作数据。
Char 字符，是用户可读写的最小单位，由16位bit（2个byte）组成，取值（0-65535），主要用于用户操数数据。

2.IO流的概念：什么是IO流？
它是指数据从源头 流到 目的地，所以常把这种数据流叫做IO流。

常用来处理设备之间的数据传输、文件的上传、下载、拷贝。

流分输入和输出，输入流从文件中读取数据存储到进程中，输出流从进程中读取数据然后写入到目标文件。

3.再看 =》 IO流分类架构图，看不懂先跳过去看下面的问题：

​                

3.流按照传输的单位怎么分类？分成哪两种流,并且他们的父类叫什么？说一下常用的IO流？

流按照传输单位分为 字节流和字符流 
字节流的抽象基类（父类）是：java.io.InputStream、java.io.OutputStream
字符流的抽象基类（父类）是：java.io.Reader 、 java.io.Writer

面向字节的操作以8为为单位对二进制数据进行操作，对数据不需要进行转换，所有的类都是InputStream和OutputStream的子类（以InputStream和OutputStream为后缀）。

面向字符的操作以字符为单位对数据进行操作，在读取的时候将二进制数据转换成字符，在写的时候则是将字符转换成二进制数据，这些类都是Reader和Writer的子类（以Reader和Writer为后缀）。

常用的io流
    InputStream,OutputStream,
        FileInputStream,FileOutputStream,
        BufferedInputStream,BufferedOutputStream
    Reader,Writer
        BufferedReader,BufferedWriter

IO类设计时使用了装饰者设计模式。

4.流按照传输的方向怎么分类？

相对于内存来说，流按照传输方向，可以分为输入流InputStream、输出流OutputStream

5.流按实现功能怎么分？
按照功能分为：节点流OutputStream、处理流 OutputStreamWriter

节点流 ：直接与数据源相连，用于输入或输出
处理流：在节点流的基础上对之进行加工，进行一些功能的扩展
处理流的构造器必须要 传入节点流的子类

6.BufferedReader属于哪种流,它主要是用来做什么的,它里面有那些经典的方法?

属于处理流中的缓冲流，可以将读取的内容存在内存里面，有readLine()方法读取一行文本，从字符输入流中读取文本，缓冲各个字符，从而提供字符、数组和行的高效读取。

这种处理流的构造器需要传入字点流。

BufferedReaderbr = new BufferedReader( new InputStreamReader(newFileInputStream("hello.txt")));
String data= null;
while((data= br.readLine())!=null){
　System.out.println(data); 
}

InputStream inStream = (new URL(“UTF-8”)).openStream();
BufferedReader bufferedReader = newBufferedReader(new InputStreamReader(inStream,"UTF-8"));
7 InputStreamReader类

    是字节流通向字符流的桥梁,封裝了InputStream在里头, 它以较高级的方式,一次读取一个一个字符，以文本格式输入 / 输出，可以指定编码格式；

一般用法：         

InputStreamReader isr = newInputStreamReader(new FileInputStream("ming.txt"));
while((ch =isr.read())!=-1){
　System.out.print((char)ch); 
}

//示例
public static String getHtmlSource( String url) throws MalformedURLException, IOException {
URLConnection uc = newURL(url).openConnection();
uc.setConnectTimeout(10000);
uc.setDoOutput(true);
InputStream in = newBufferedInputStream(uc.getInputStream());
InputStreamReader rd = newInputStreamReader(in,"gb2312");
int c = 0;
StringBuffer temp = new StringBuffer();
while((c = rd.read())!= -1){
      temp.append((char)c);
}
in.close();
return temp.toString();
}
7.FileInputStream和FileOutputStream是什么

这是在拷贝文件操作的时候，经常用到的两个类。在处理小文件的时候，他们性能表现还不错，在大文件的时候，最好使用BufferedInputStream(或BufferedReader)和BufferedOutputStream(或BufferedWriter)

8.如果要对字节流进行大量的从硬盘读取,要用那个流,为什么?

   BufferedInputStream 使用缓冲流能够减少对硬盘的损伤

9.字节流和字符流，你更喜欢使用哪一个

更喜欢使用字符流，因为她们更新一些，许多在字符流中存在的特性，字节流中不存在。比如使用BufferedReader而不是BufferedInputStream或DataInputStream，使用newLine()方法来读取下一行，但是在字节流中我们需要做额外的操作。

10.System.out.println()是什么

println是PrintStream的一个方法。Out是一个静态PrintStream类型的成员变量，system是一个java.lang包中的类，用于和底层的操作系统进行交互。

11.如果我要打印出不同类型的数据到数据源,那么最适合的流是那个流,为什么?

   Printwriter 可以打印各种数据类型

12.怎么样把我们控制台的输出改成输出到一个文件里面,这个技术叫什么?

   SetOut（printWriter,printStream）重定向

13.怎么样把输出字节流转换成输出字符流,说出它的步骤?

   使用 转换处理流OutputStreamWriter 可以将字节流转为字符流

New OutputStreamWriter(new FileOutputStream(File file));
14.把包括基本类型在内的数据和字符串按顺序输出到数据源，或者按照顺序从数据源读入，一般用哪两个流？

   DataInputStream DataOutputStream

15.把一个对象写入数据源或者从一个数据源读出来,用哪两个流？

   ObjectInputStream ObjectOutputStream

16.什么是序列化和反序列化，实现对象序列化需要做哪些工作？

   对象序列化是将对象以二进制的形式保存在硬盘上或者传输到网络；而反序列化则是将二进制的文件转化为对象读取。

   Jre本身就提供了序列化支持，我们可以调OutputStream的writeObject方法来做，如果要让java 帮我们做，要被传输的对象必须实现serializable接口，这样，javac编译时就会进行特殊处理，编译的类才可以被writeObject方法操作，这就是所谓的序列化。

   serializable接口是一个mini接口，其中没有需要实现的方法，implements Serializable只是为了标注该对象是可被序列化的。
   如果不想让字段放在硬盘上就加transient。

17.在实现序列化接口是时候一般要生成一个serialVersionUID字段,它叫做什么,一般有什么用？

  是版本号，要保持版本号的一致 来进行序列化，主要是为了防止序列化出错

18 请问你在什么情况下会在你得java代码中使用可序列化？ 如何实现java序列化？
  把一个对象写入数据源或者从一个数据源读出来，使用可序列化，需要实现Serializable接口

19.InputStream里的read()返回的是什么,read(byte[] data)是什么意思,返回的是什么值？

   read()返回的是所读取的字节的int型（范围0-255）
   read（byte [ ] data）将读取的字节储存在这个数组，  返回的就是传入数组参数个数

   Read 字节读取字节  字符读取字符

20.OutputStream里面的write()是什么意思？write(byte b[], int off, int len)这个方法里面的三个参数分别是什么意思？

   write将指定字节传入数据源
   Byte b[ ]是byte数组，b[off]是传入的第一个字符，b[off+len-1]是传入的最后的一个字符 ，len是实际长度

21.流一般需要不需要关闭？怎么关闭？一般要在那个代码块里面关闭比较好，处理流是怎么关闭的，如果有多个流互相调用传入是怎么关闭的？

   流一旦打开就必须关闭，要使用close方法关闭，一般放在finally语句块中（finally 语句一定会执行）执行。

   多个流互相调用只关闭最外层的流即可。

22 写一段代码读取一个序列化的对象一般使用哪种Stream？
   A、InputStream B、FileReader C、DataInputStream D、ObjectStream

23 io流怎样读取文件的？
  使用File对象获取文件路径;

  通过字符流Reader加入文件;

  使用字符缓存流BufferedReader处理Reader;

  再定义一个字符串，循环遍历出文件。

  代码如下：

File file = new File("d:/spring.txt");
try {
  Reader reader = new FileReader(file);
  BufferedReader buffered = new BufferedReader(reader);
  String data = null;
  while((data = buffered.readLine())!=null){
    System.out.println(data);
  }
} catch (Exception e) {
  e.printStackTrace();
}finally{
  buffered.close();
}
24 用什么把对象动态的写入磁盘中，写入要实现什么接口。
     ObjectInputStream，需要实现Serializable接口
25  FileInputStream 创建详情，就是怎样的创建不报错，它列出了几种形式!
    FileInputStream是InputStream的子类，通过接口定义，子类实现创建FileInputStream,

26 用io流中的技术，指定一个文件夹的目录，获取此目录下的所有子文件夹路径 

27 PrintStream、BufferedWriter、PrintWriter的比较? 
  PrintStream类的输出功能非常强大，通常如果需要输出文本内容，都应该将输出流包装成PrintStream后进行输出。它还提供其他两项功能。与其他输出流不同，PrintStream 永远不会抛出 IOException；而是，异常情况仅设置可通过 checkError 方法测试的内部标志。另外，为了自动刷新，可以创建一个 PrintStream
 BufferedWriter:将文本写入字符输出流，缓冲各个字符从而提供单个字符，数组和字符串的高效写入。通过write()方法可以将获取到的字符输出，然后通过newLine()进行换行操作。BufferedWriter中的字符流必须通过调用flush方法才能将其刷出去。并且BufferedWriter只能对字符流进行操作。如果要对字节流操作，则使用BufferedInputStream。
 PrintWriter的println方法自动添加换行，不会抛异常，若关心异常，需要调用checkError方法看是否有异常发生，PrintWriter构造方法可指定参数，实现自动刷新缓存（autoflush）；

28.什么是Filter流

Filter Stream是一种IO流主要作用是用来对存在的流增加一些额外的功能，像给目标文件增加源文件中不存在的行数，或者增加拷贝的性能。

29.有哪些可用的Filter流？

在java.io包中主要有4个可用的filterStream。两个字节filterStream，两个字符filterStream。分别是FilterInputStream，FilterOutputStream，FilterReader and FilterWriter。这些类是抽象类，不能被实例化。

有些Filter流的子类：

LineNumberInputStream给目标文件增加行号

DataInputStream有些特殊的方法如readInt()，readDouble()和readLine()等可以读取一个int，double和一个string一次性的

BufferedInputStream增加性能

PushbackInputStream推送要求的字节到系统中

30.SequenceInputStream的作用

这个类的作用是将多个输入流合并成一个输出流，通过SequenceInputStream类包装后形成新的一个总的输入流。在拷贝多个文件到一个目标文件的时候是非常有用的。可以使用很少的代码实现。

31.PrintStream和PrintWriter

它们两个的功能相同，但是属于不同的分类。字节流和字符流，它们都有println()方法。

32.在拷贝的时候，哪一种流能提升更多的性能？

在字节流的时候，使用BufferedInputStream和BufferedOutputStream。

在字符流的时候，使用BufferedReader和BufferedWriter

33.管道流

有四种管道流，PipedInputStream，PipedOutputStream，PipedReader和PipedWriter。在多个线程或进程中传递数据的时候管道流非常有用。

34.File类

它不属于IP流，也不是用于文件的操作，它主要用于指导一个文件的属性，读写权限，大小等信息。注意：Java7中文件IO发生了很大的变化，专门引入了很多新的类来取代原来的基于java.io.File的文件IO操作方式。

35.RandomAccessFile

它在java.io包中是一个特殊的类，既不是输入流也不是输出流，它两者都可以做到。他是Object的直接子类。通常来说，一个流只有一个功能，要么读，要么写。但是RandomAccessFile既可以读文件，也可以写文件。DataInputStream和DataOutStream有的方法，在RandomAccessFile都存在。

IO流练习
1. 读写原始数据，一般采用什么流？（AC ）
A InputStream   B DataInputStream  C OutputStream   D BufferedInputStream
2. 为了提高读写性能，可以采用什么流？（ DF）
A InputStream   B DataInputStream   C BufferedReader    D BufferedInputStream   E OutputStream   F BufferedOutputStream
3. 对各种基本数据类型和String类型的读写，采用什么流？（ AD）
A DataInputStream  B BufferedReader  C PrintWriter  D DataOutputStream  E ObjectInputStream  F ObjectOutputStream
4. 能指定字符编码的I/O流类型是：（BH ）
A Reader   B InputStreamReader    C BufferedReader   D Writer   E PrintWriter
F ObjectInputStream   G ObjectOutputStream   H OutputStreamWriter
5. File类型中定义了什么方法来判断一个文件是否存在？（ D）
A createNewFile   B renameTo   C delete   D exists
6. File类型中定义了什么方法来创建一级目录？（ CD）
A createNewFile   B exists   C mkdirs  D mkdir

File类的mkdir方法根据抽象路径创建目录；File类的mkdirs方法根据抽象路径创建目录，包括创建必需但不存在的父目录

7. 对文本文件操作用什么I/O流？（AD ）
A FileReader    B FileInputStream   C RandomAccessFile   D FileWriter
8. 在unix服务器www.openlab.com.cn上提供了基于TCP的时间服务应用，该应用使用port为13。创建连接到此服务器的语句是：（A ）
A Socket s = new Socket(“www.openlab.com.cn”, 13);
B Socket s = new Socket(“www.openlab.com.cn:13”);
C Socket s = accept(“www.openlab.com.cn”, 13);
9. 创建一个TCP客户程序的顺序是：（DACBE ）
A 获得I/O流    B 关闭I/O流    C 对I/O流进行读写操作    D 建立socket    E 关闭socket
10. 创建一个TCP服务程序的顺序是：（BCADEGF ）
A 创建一个服务线程处理新的连接
B 创建一个服务器socket
C 从服务器socket接受客户连接请求
D 在服务线程中，从socket中获得I/O流
E 对I/O流进行读写操作，完成与客户的交互
F 关闭socket
G 关闭I/O流
11. Java UDP编程主要用到的两个类型是：（ BD）
A UDPSocket
B DatagramSocket
C UDPPacket
D DatagramPacket
12. TCP/IP是一种：（ B）
A 标准 
B 协议  
C 语言  
D 算法

Java IO 分类
Java BIO： 同步并阻塞，服务器实现模式为一个连接一个线程，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销，当然可以通过线程池机制改善。

Java NIO： 同步非阻塞，服务器实现模式为一个请求一个线程，即当一个连接创建后，不需要对应一个线程，这个连接会被注册到多路复用器上面，所以所有的连接只需要一个线程就可以搞定，当这个线程中的多路复用器进行轮询的时候，发现连接上有请求的话，才开启一个线程进行处理，也就是一个请求一个线程模式。BIO与NIO一个比较重要的不同，是我们使用BIO的时候往往会引入多线程，每个连接一个单独的线程；而NIO则是使用单线程或者只使用少量的多线程，每个连接共用一个线程。

Java AIO(NIO.2)： 异步非阻塞，服务器实现模式为一个有效请求一个线程，客户端的I/O请求都是由OS先完成了再通知服务器应用去启动线程进行处理。

名词解释

同步:指的是用户进程触发IO操作需要等待或者轮询的去查看IO操作执行完成才能执行其他操作.这种方式性能比较差，只有一些对数据安全性要求比较高的场景中才会使用．

异步:异步是指用户进程触发IO操作以后便开始做自己的事情，而当IO操作已经完成的时候会得到IO完成的通知（异步的特点就是通知）

阻塞：所谓阻塞方式的意思是指, 当试图对该文件描述符进行读写时, 如果当时没有东西可读,或者暂时不可写, 程序就进入等待 状态, 直到有东西可读或者可写为止

非阻塞：非阻塞状态下, 如果没有东西可读, 或者不可写, 读写函数马上返回, 而不会等待

BIO、NIO、AIO适用场景分析

BIO方式适用于连接数目比较小且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4以前的唯一选择，但程序直观简单易理解。

NIO方式适用于连接数目多且连接比较短（轻操作）的架构，比如聊天服务器，并发局限于应用中，编程比较复杂，JDK1.4开始支持。

AIO方式使用于连接数目多且连接比较长（重操作）的架构，比如相册服务器，充分调用OS参与并发操作，编程比较复杂，JDK7开始支持。

Java NIO和IO的主要区别

面向流与面向缓冲. Java NIO和IO之间第一个最大的区别是，IO是面向流的，NIO是面向缓冲区的。Java IO面向流意味着每次从流中读一个或多个字节，直至读取所有字节，它们没有被缓存在任何地方。此外，它不能前后移动流中的数据。如果需要前后移动从流中读取的数据，需要先将它缓存到一个缓冲区。 Java NIO的缓冲导向方法略有不同。数据读取到一个它稍后处理的缓冲区，需要时可在缓冲区中前后移动。这就增加了处理过程中的灵活性。

阻塞与非阻塞IO Java IO的各种流是阻塞的。这意味着，当一个线程调用read() 或 write()时，该线程被阻塞，直到有一些数据被读取，或数据完全写入。该线程在此期间不能再干任何事情了。 Java NIO的非阻塞模式，使一个线程从某通道发送请求读取数据，但是它仅能得到目前可用的数据，如果目前没有数据可用时，该线程可以继续做其他的事情。 非阻塞写也是如此。一个线程请求写入一些数据到某通道，但不需要等待它完全写入，这个线程同时可以去做别的事情。线程通常将非阻塞IO的空闲时间用于在其它通道上执行IO操作，所以一个单独的线程现在可以管理多个输入和输出通道（channel）。

选择器（Selectors） Java NIO的选择器允许一个单独的线程来监视多个输入通道，你可以注册多个通道使用一个选择器，然后使用一个单独的线程来“选择”通道：这些通道里已经有可以处理的输入，或者选择已准备写入的通道。这种选择机制，使得一个单独的线程很容易来管理多个通道。
————————————————
版权声明：本文为CSDN博主「Terence Jing」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/CSDN_Terence/article/details/89513420

# Java中同步，异步，阻塞以及非阻塞

| 编号 | 名词   | 解释                                                         | 举例                                                         |
| ---- | ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | 同步   | 指的是用户进程触发IO操作并等待或者轮询的去查看IO操作是否就绪 | 自己上街买衣服，自己亲自干这件事，别的事干不了。             |
| 2    | 异步   | 异步是指用户进程触发IO操作以后便开始做自己的事情，而当IO操作已经完成的时候会得到IO完成的通知（异步的特点就是通知） | 告诉朋友自己合适衣服的尺寸，大小，颜色，让朋友委托去卖，然后自己可以去干别的事。（使用异步IO时，Java将IO读写委托给OS处理，需要将数据缓冲区地址和大小传给OS） |
| 3    | 阻塞   | 所谓阻塞方式的意思是指, 当试图对该文件描述符进行读写时, 如果当时没有东西可读,或者暂时不可写, 程序就进入等待 状态, 直到有东西可读或者可写为止 | 去公交站充值，发现这个时候，充值员不在（可能上厕所去了），然后我们就在这里等待，一直等到充值员回来为止。（当然现实社会，可不是这样，但是在计算机里确实如此。） |
| 4    | 非阻塞 | 非阻塞状态下, 如果没有东西可读, 或者不可写, 读写函数马上返回, 而不会等待， | 银行里取款办业务时，领取一张小票，领取完后我们自己可以玩玩手机，或者与别人聊聊天，当轮我们时，银行的喇叭会通知，这时候我们就可以去了。 |

# Java IO流

IO的方式通常分为几种，同步阻塞的BIO、同步非阻塞的NIO、异步非阻塞的AIO。

#### 1.IO场景

**同步阻塞IO（BIO）：**

> 在此种方式下，用户进程在发起一个IO操作以后，必须等待IO操作的完成，只有当真正完成了IO操作以后，用户进程才能运行。JAVA传统的IO模型属于此种方式！

**同步非阻塞IO（NIO）:**

> 在此种方式下，用户进程发起一个IO操作以后便可返回做其它事情，但是用户进程需要时不时的询问IO操作是否就绪，这就要求用户进程不停的去询问，从而引入不必要的CPU资源浪费。其中目前JAVA的NIO就属于同步非阻塞IO。

**异步阻塞IO（NIO）：**

> 此种方式下是指应用发起一个IO操作以后，不等待内核IO操作的完成，等内核完成IO操作以后会通知应用程序，这其实就是同步和异步最关键的区别，同步必须等待或者主动的去询问IO是否完成，那么为什么说是阻塞的呢？因为此时是通过select系统调用来完成的，而select函数本身的实现方式是阻塞的，而采用select函数有个好处就是它可以同时监听多个文件句柄，从而提高系统的并发性！

**异步非阻塞IO（AIO）:**

> 在此种模式下，用户进程只需要发起一个IO操作然后立即返回，等IO操作真正的完成以后，应用程序会得到IO操作完成的通知，此时用户进程只需要对数据进行处理就好了，不需要进行实际的IO读写操作，因为真正的IO读取或者写入操作已经由内核完成了。

#### 2.Reactor模式和Proactor模式

其中NIO和AIO的概念比较容易混淆，**NIO基于模式Reactor，AIO基于Proactor模式，**先来看下这两个模式的区别，

首先来看看Reactor模式，Reactor模式应用于同步I/O的场景。我们以读操作为例来看看Reactor中的具体步骤：
 **读取操作：**

> 1. 应用程序注册读就绪事件和相关联的事件处理器到时间分离器上（对应的selector）
> 2. 事件分离器等待事件的发生
> 3. 当发生读就绪事件的时候，事件分离器调用第一步注册的事件处理器
> 4. 事件处理器首先执行实际的读取操作，然后根据读取到的内容进行进一步的处理

写入操作类似于读取操作，只不过第一步注册的是写就绪事件。

下面我们来看看Proactor模式中读取操作和写入操作的过程：
 **读取操作：**

> 1. 应用程序初始化一个异步读取操作，然后注册相应的事件处理器，**此时事件处理器不关注读取就绪事件，而是关注读取完成事件，这是区别于Reactor的关键。**
> 2. 事件分离器等待读取操作完成事件
> 3. 在事件分离器等待读取操作完成的时候，操作系统调用内核线程完成读取操作（异步IO都是操作系统负责将数据读写到应用传递进来的缓冲区供应用程序操作，操作系统扮演了重要角色），并将读取的内容放入用户传递过来的缓存区中。这也是区别于Reactor的一点，Proactor中，应用程序需要传递缓存区。
> 4. 事件分离器捕获到读取完成事件后，激活应用程序注册的事件处理器，**事件处理器直接从缓存区读取数据，而不需要进行实际的读取操作。**

Proactor中写入操作和读取操作，只不过感兴趣的事件是写入完成事件。

从上面可以看出

> **Reactor和Proactor模式的主要区别就是真正的读取和写入操作是有谁来完成的，Reactor中需要应用程序自己读取或者写入数据，而Proactor模式中，应用程序不需要进行实际的读写过程，它只需要从缓存区读取或者写入即可，操作系统会读取缓存区或者写入缓存区到真正的IO设备.**

#### 小结

BIO，同步阻塞式IO，简单理解：一个连接一个线程
 NIO，同步非阻塞IO，简单理解：一个io请求一个线程,总线轮询方式来获取io的消息
 AIO，异步非阻塞IO，简单理解：一个有效请求一个线程，把io的工作分出去，仅做通知后的回调程序即可。

BIO、NIO、AIO适用场景分析:



```css
BIO方式适用于连接数目比较小且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4以前的唯一选择，但程序直观简单易理解。 

NIO方式适用于连接数目多且连接比较短（轻操作）的架构，比如聊天服务器，并发局限于应用中，编程比较复杂，JDK1.4开始支持。 

AIO方式使用于连接数目多且连接比较长（重操作）的架构，比如相册服务器，充分调用OS参与并发操作，编程比较复杂，JDK7开始支持。 
```



作者：激情的狼王
链接：https://www.jianshu.com/p/d23f6d261a04
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

# Java NIO

NIO（Non-blocking I/O，在Java领域，也称为New I/O），是一种同步非阻塞的I/O模型，也是I/O多路复用的基础，已经被越来越多地应用到大型应用服务器，成为解决高并发与大量连接、I/O处理问题的有效方式。

那么NIO的本质是什么样的呢？它是怎样与事件模型结合来解放线程、提高系统吞吐的呢？

本文会从传统的阻塞I/O和线程池模型面临的问题讲起，然后对比几种常见I/O模型，一步步分析NIO怎么利用事件模型处理I/O，解决线程池瓶颈处理海量连接，包括利用面向事件的方式编写服务端/客户端程序。最后延展到一些高级主题，如Reactor与Proactor模型的对比、Selector的唤醒、Buffer的选择等。

注：本文的代码都是伪代码，主要是为了示意，不可用于生产环境。

让我们先回忆一下传统的服务器端同步阻塞I/O处理（也就是BIO，Blocking I/O）的经典编程模型：

```
{
 ExecutorService executor = Excutors.newFixedThreadPollExecutor(100);//线程池

 ServerSocket serverSocket = new ServerSocket();
 serverSocket.bind(8088);
 while(!Thread.currentThread.isInturrupted()){//主线程死循环等待新连接到来
 Socket socket = serverSocket.accept();
 executor.submit(new ConnectIOnHandler(socket));//为新的连接创建新的线程
}

class ConnectIOnHandler extends Thread{
    private Socket socket;
    public ConnectIOnHandler(Socket socket){
       this.socket = socket;
    }
    public void run(){
      while(!Thread.currentThread.isInturrupted()&&!socket.isClosed()){死循环处理读写事件
          String someThing = socket.read()....//读取数据
          if(someThing!=null){
             ......//处理数据
             socket.write()....//写数据
          }

      }
    }
}
```

这是一个经典的每连接每线程的模型，之所以使用多线程，主要原因在于socket.accept()、socket.read()、socket.write()三个主要函数都是同步阻塞的，当一个连接在处理I/O的时候，系统是阻塞的，如果是单线程的话必然就挂死在那里；但CPU是被释放出来的，开启多线程，就可以让CPU去处理更多的事情。其实这也是所有使用多线程的本质： 1. 利用多核。 2. 当I/O阻塞系统，但CPU空闲的时候，可以利用多线程使用CPU资源。

现在的多线程一般都使用线程池，可以让线程的创建和回收成本相对较低。在活动连接数不是特别高（小于单机1000）的情况下，这种模型是比较不错的，可以让每一个连接专注于自己的I/O并且编程模型简单，也不用过多考虑系统的过载、限流等问题。线程池本身就是一个天然的漏斗，可以缓冲一些系统处理不了的连接或请求。

不过，这个模型最本质的问题在于，严重依赖于线程。但线程是很”贵”的资源，主要表现在： 1. 线程的创建和销毁成本很高，在Linux这样的操作系统中，线程本质上就是一个进程。创建和销毁都是重量级的系统函数。 2. 线程本身占用较大内存，像Java的线程栈，一般至少分配512K～1M的空间，如果系统中的线程数过千，恐怕整个JVM的内存都会被吃掉一半。 3. 线程的切换成本是很高的。操作系统发生线程切换的时候，需要保留线程的上下文，然后执行系统调用。如果线程数过高，可能执行线程切换的时间甚至会大于线程执行的时间，这时候带来的表现往往是系统load偏高、CPU sy使用率特别高（超过20%以上)，导致系统几乎陷入不可用的状态。 4. 容易造成锯齿状的系统负载。因为系统负载是用活动线程数或CPU核心数，一旦线程数量高但外部网络环境不是很稳定，就很容易造成大量请求的结果同时返回，激活大量阻塞线程从而使系统负载压力过大。

所以，当面对十万甚至百万级连接的时候，传统的BIO模型是无能为力的。随着移动端应用的兴起和各种网络游戏的盛行，百万级长连接日趋普遍，此时，必然需要一种更高效的I/O处理模型。

很多刚接触NIO的人，第一眼看到的就是Java相对晦涩的API，比如：Channel，Selector，Socket什么的；然后就是一坨上百行的代码来演示NIO的服务端Demo……瞬间头大有没有？

我们不管这些，抛开现象看本质，先分析下NIO是怎么工作的。

## 常见I/O模型对比

所有的系统I/O都分为两个阶段：等待就绪和操作。举例来说，读函数，分为等待系统可读和真正的读；同理，写函数分为等待网卡可以写和真正的写。

需要说明的是等待就绪的阻塞是不使用CPU的，是在“空等”；而真正的读写操作的阻塞是使用CPU的，真正在”干活”，而且这个过程非常快，属于memory copy，带宽通常在1GB/s级别以上，可以理解为基本不耗时。

下图是几种常见I/O模型的对比：![emma_1](https://awps-assets.meituan.net/mit-x/blog-images-bundle-2016/77752ed5.jpg)

emma_1



以socket.read()为例子：

传统的BIO里面socket.read()，如果TCP RecvBuffer里没有数据，函数会一直阻塞，直到收到数据，返回读到的数据。

对于NIO，如果TCP RecvBuffer有数据，就把数据从网卡读到内存，并且返回给用户；反之则直接返回0，永远不会阻塞。

最新的AIO(Async I/O)里面会更进一步：不但等待就绪是非阻塞的，就连数据从网卡到内存的过程也是异步的。

换句话说，BIO里用户最关心“我要读”，NIO里用户最关心”我可以读了”，在AIO模型里用户更需要关注的是“读完了”。

NIO一个重要的特点是：socket主要的读、写、注册和接收函数，在等待就绪阶段都是非阻塞的，真正的I/O操作是同步阻塞的（消耗CPU但性能非常高）。

## 如何结合事件模型使用NIO同步非阻塞特性

回忆BIO模型，之所以需要多线程，是因为在进行I/O操作的时候，一是没有办法知道到底能不能写、能不能读，只能”傻等”，即使通过各种估算，算出来操作系统没有能力进行读写，也没法在socket.read()和socket.write()函数中返回，这两个函数无法进行有效的中断。所以除了多开线程另起炉灶，没有好的办法利用CPU。

NIO的读写函数可以立刻返回，这就给了我们不开线程利用CPU的最好机会：如果一个连接不能读写（socket.read()返回0或者socket.write()返回0），我们可以把这件事记下来，记录的方式通常是在Selector上注册标记位，然后切换到其它就绪的连接（channel）继续进行读写。

下面具体看下如何利用事件模型单线程处理所有I/O请求：

NIO的主要事件有几个：读就绪、写就绪、有新连接到来。

我们首先需要注册当这几个事件到来的时候所对应的处理器。然后在合适的时机告诉事件选择器：我对这个事件感兴趣。对于写操作，就是写不出去的时候对写事件感兴趣；对于读操作，就是完成连接和系统没有办法承载新读入的数据的时；对于accept，一般是服务器刚启动的时候；而对于connect，一般是connect失败需要重连或者直接异步调用connect的时候。

其次，用一个死循环选择就绪的事件，会执行系统调用（Linux 2.6之前是select、poll，2.6之后是epoll，Windows是IOCP），还会阻塞的等待新事件的到来。新事件到来的时候，会在selector上注册标记位，标示可读、可写或者有连接到来。

注意，select是阻塞的，无论是通过操作系统的通知（epoll）还是不停的轮询(select，poll)，这个函数是阻塞的。所以你可以放心大胆地在一个while(true)里面调用这个函数而不用担心CPU空转。

所以我们的程序大概的模样是：

```
   interface ChannelHandler{
      void channelReadable(Channel channel);
      void channelWritable(Channel channel);
   }
   class Channel{
     Socket socket;
     Event event;//读，写或者连接
   }

   //IO线程主循环:
   class IoThread extends Thread{
   public void run(){
   Channel channel;
   while(channel=Selector.select()){//选择就绪的事件和对应的连接
      if(channel.event==accept){
         registerNewChannelHandler(channel);//如果是新连接，则注册一个新的读写处理器
      }
      if(channel.event==write){
         getChannelHandler(channel).channelWritable(channel);//如果可以写，则执行写事件
      }
      if(channel.event==read){
          getChannelHandler(channel).channelReadable(channel);//如果可以读，则执行读事件
      }
    }
   }
   Map<Channel，ChannelHandler> handlerMap;//所有channel的对应事件处理器
  }
```

这个程序很简短，也是最简单的Reactor模式：注册所有感兴趣的事件处理器，单线程轮询选择就绪事件，执行事件处理器。

## 优化线程模型

由上面的示例我们大概可以总结出NIO是怎么解决掉线程的瓶颈并处理海量连接的：

NIO由原来的阻塞读写（占用线程）变成了单线程轮询事件，找到可以进行读写的网络描述符进行读写。除了事件的轮询是阻塞的（没有可干的事情必须要阻塞），剩余的I/O操作都是纯CPU操作，没有必要开启多线程。

并且由于线程的节约，连接数大的时候因为线程切换带来的问题也随之解决，进而为处理海量连接提供了可能。

单线程处理I/O的效率确实非常高，没有线程切换，只是拼命的读、写、选择事件。但现在的服务器，一般都是多核处理器，如果能够利用多核心进行I/O，无疑对效率会有更大的提高。

仔细分析一下我们需要的线程，其实主要包括以下几种： 1. 事件分发器，单线程选择就绪的事件。 2. I/O处理器，包括connect、read、write等，这种纯CPU操作，一般开启CPU核心个线程就可以。 3. 业务线程，在处理完I/O后，业务一般还会有自己的业务逻辑，有的还会有其他的阻塞I/O，如DB操作，RPC等。只要有阻塞，就需要单独的线程。

Java的Selector对于Linux系统来说，有一个致命限制：同一个channel的select不能被并发的调用。因此，如果有多个I/O线程，必须保证：一个socket只能属于一个IoThread，而一个IoThread可以管理多个socket。

另外连接的处理和读写的处理通常可以选择分开，这样对于海量连接的注册和读写就可以分发。虽然read()和write()是比较高效无阻塞的函数，但毕竟会占用CPU，如果面对更高的并发则无能为力。

![emma_1](https://awps-assets.meituan.net/mit-x/blog-images-bundle-2016/5c0ca404.png)

emma_1



通过上面的分析，可以看出NIO在服务端对于解放线程，优化I/O和处理海量连接方面，确实有自己的用武之地。那么在客户端上，NIO又有什么使用场景呢?

常见的客户端BIO+连接池模型，可以建立n个连接，然后当某一个连接被I/O占用的时候，可以使用其他连接来提高性能。

但多线程的模型面临和服务端相同的问题：如果指望增加连接数来提高性能，则连接数又受制于线程数、线程很贵、无法建立很多线程，则性能遇到瓶颈。

## 每连接顺序请求的Redis

对于Redis来说，由于服务端是全局串行的，能够保证同一连接的所有请求与返回顺序一致。这样可以使用单线程＋队列，把请求数据缓冲。然后pipeline发送，返回future，然后channel可读时，直接在队列中把future取回来，done()就可以了。

伪代码如下：

```
class RedisClient Implements ChannelHandler{
 private BlockingQueue CmdQueue;
 private EventLoop eventLoop;
 private Channel channel;
 class Cmd{
  String cmd;
  Future result;
 }
 public Future get(String key){
   Cmd cmd= new Cmd(key);
   queue.offer(cmd);
   eventLoop.submit(new Runnable(){
        List list = new ArrayList();
        queue.drainTo(list);
        if(channel.isWritable()){
         channel.writeAndFlush(list);
        }
   });
}
 public void ChannelReadFinish(Channel channel，Buffer Buffer){
    List result = handleBuffer();//处理数据
    //从cmdQueue取出future，并设值，future.done();
}
 public void ChannelWritable(Channel channel){
   channel.flush();
}
}
```

这样做，能够充分的利用pipeline来提高I/O能力，同时获取异步处理能力。

## 多连接短连接的HttpClient

类似于竞对抓取的项目，往往需要建立无数的HTTP短连接，然后抓取，然后销毁，当需要单机抓取上千网站线程数又受制的时候，怎么保证性能呢?

何不尝试NIO，单线程进行连接、写、读操作？如果连接、读、写操作系统没有能力处理，简单的注册一个事件，等待下次循环就好了。

如何存储不同的请求/响应呢？由于http是无状态没有版本的协议，又没有办法使用队列，好像办法不多。比较笨的办法是对于不同的socket，直接存储socket的引用作为map的key。

## 常见的RPC框架，如Thrift，Dubbo

这种框架内部一般维护了请求的协议和请求号，可以维护一个以请求号为key，结果的result为future的map，结合NIO+长连接，获取非常不错的性能。

## Proactor与Reactor

一般情况下，I/O 复用机制需要事件分发器（event dispatcher）。 事件分发器的作用，即将那些读写事件源分发给各读写事件的处理者，就像送快递的在楼下喊: 谁谁谁的快递到了， 快来拿吧！开发人员在开始的时候需要在分发器那里注册感兴趣的事件，并提供相应的处理者（event handler)，或者是回调函数；事件分发器在适当的时候，会将请求的事件分发给这些handler或者回调函数。

涉及到事件分发器的两种模式称为：Reactor和Proactor。 Reactor模式是基于同步I/O的，而Proactor模式是和异步I/O相关的。**在Reactor模式中，事件分发器等待某个事件或者可应用或个操作的状态发生（比如文件描述符可读写，或者是socket可读写），事件分发器就把这个事件传给事先注册的事件处理函数或者回调函数，由后者来做实际的读写操作。**

而在Proactor模式中，**事件处理者（或者代由事件分发器发起）直接发起一个异步读写操作（相当于请求），而实际的工作是由操作系统来完成的。**发起时，需要提供的参数包括用于存放读到数据的缓存区、读的数据大小或用于存放外发数据的缓存区，以及这个请求完后的回调函数等信息。事件分发器得知了这个请求，它默默等待这个请求的完成，然后转发完成事件给相应的事件处理者或者回调。**举例来说，在Windows上事件处理者投递了一个异步IO操作（称为overlapped技术），事件分发器等IO Complete事件完成。**这种异步模式的典型实现是基于操作系统底层异步API的，所以我们可称之为“系统级别”的或者“真正意义上”的异步，因为具体的读写是由操作系统代劳的。

举个例子，将有助于理解Reactor与Proactor二者的差异，以读操作为例（写操作类似）。

#### 在Reactor中实现读

- 注册读就绪事件和相应的事件处理器。
- 事件分发器等待事件。
- 事件到来，激活分发器，分发器调用事件对应的处理器。
- 事件处理器完成实际的读操作，处理读到的数据，注册新的事件，然后返还控制权。

#### 在Proactor中实现读：

- 处理器发起异步读操作（注意：操作系统必须支持异步IO）。在这种情况下，处理器无视IO就绪事件，它关注的是完成事件。
- 事件分发器等待操作完成事件。
- 在分发器等待过程中，操作系统利用并行的内核线程执行实际的读操作，并将结果数据存入用户自定义缓冲区，最后通知事件分发器读操作完成。
- 事件分发器呼唤处理器。
- 事件处理器处理用户自定义缓冲区中的数据，然后启动一个新的异步操作，并将控制权返回事件分发器。

可以看出，两个模式的相同点，都是对某个I/O事件的事件通知（即告诉某个模块，这个I/O操作可以进行或已经完成)。在结构上，两者也有相同点：事件分发器负责提交IO操作（异步)、查询设备是否可操作（同步)，然后当条件满足时，就回调handler；**不同点在于，异步情况下（Proactor)，当回调handler时，表示I/O操作已经完成；同步情况下（Reactor)，回调handler时，表示I/O设备可以进行某个操作（can read 或 can write)。**

下面，我们将尝试应对为Proactor和Reactor模式建立可移植框架的挑战。在改进方案中，我们将Reactor原来位于事件处理器内的Read/Write操作移至分发器（不妨将这个思路称为“模拟异步”），以此寻求将Reactor多路同步I/O转化为模拟异步I/O。以读操作为例子，改进过程如下： - 注册读就绪事件和相应的事件处理器。并为分发器提供数据缓冲区地址，需要读取数据量等信息。 - 分发器等待事件（如在select()上等待）。 - 事件到来，激活分发器。分发器执行一个非阻塞读操作（它有完成这个操作所需的全部信息），最后调用对应处理器。 - 事件处理器处理用户自定义缓冲区的数据，注册新的事件（当然同样要给出数据缓冲区地址，需要读取的数据量等信息），最后将控制权返还分发器。 如我们所见，通过对多路I/O模式功能结构的改造，可将Reactor转化为Proactor模式。改造前后，模型实际完成的工作量没有增加，只不过参与者间对工作职责稍加调换。没有工作量的改变，自然不会造成性能的削弱。对如下各步骤的比较，可以证明工作量的恒定：

#### 标准/典型的Reactor：

- 步骤1：等待事件到来（Reactor负责）。
- 步骤2：将读就绪事件分发给用户定义的处理器（Reactor负责）。
- 步骤3：读数据（用户处理器负责）。
- 步骤4：处理数据（用户处理器负责）。

#### 改进实现的模拟Proactor：

- 步骤1：等待事件到来（Proactor负责）。
- 步骤2：得到读就绪事件，执行读数据（现在由Proactor负责）。
- 步骤3：将读完成事件分发给用户处理器（Proactor负责）。
- 步骤4：处理数据（用户处理器负责）。

对于不提供异步I/O API的操作系统来说，这种办法可以隐藏Socket API的交互细节，从而对外暴露一个完整的异步接口。借此，我们就可以进一步构建完全可移植的，平台无关的，有通用对外接口的解决方案。

代码示例如下：

```
interface ChannelHandler{
      void channelReadComplate(Channel channel，byte[] data);
      void channelWritable(Channel channel);
   }
   class Channel{
     Socket socket;
     Event event;//读，写或者连接
   }

   //IO线程主循环：
   class IoThread extends Thread{
   public void run(){
   Channel channel;
   while(channel=Selector.select()){//选择就绪的事件和对应的连接
      if(channel.event==accept){
         registerNewChannelHandler(channel);//如果是新连接，则注册一个新的读写处理器
         Selector.interested(read);
      }
      if(channel.event==write){
         getChannelHandler(channel).channelWritable(channel);//如果可以写，则执行写事件
      }
      if(channel.event==read){
          byte[] data = channel.read();
          if(channel.read()==0)//没有读到数据，表示本次数据读完了
          {
          getChannelHandler(channel).channelReadComplate(channel，data;//处理读完成事件
          }
          if(过载保护){
          Selector.interested(read);
          }

      }
     }
    }
   Map<Channel，ChannelHandler> handlerMap;//所有channel的对应事件处理器
   }
```

## Selector.wakeup()

### 主要作用

解除阻塞在Selector.select()/select(long)上的线程，立即返回。

两次成功的select之间多次调用wakeup等价于一次调用。

如果当前没有阻塞在select上，则本次wakeup调用将作用于下一次select——“记忆”作用。

为什么要唤醒？

注册了新的channel或者事件。

channel关闭，取消注册。

优先级更高的事件触发（如定时器事件），希望及时处理。

### 原理

Linux上利用pipe调用创建一个管道，Windows上则是一个loopback的tcp连接。这是因为win32的管道无法加入select的fd set，将管道或者TCP连接加入select fd set。

wakeup往管道或者连接写入一个字节，阻塞的select因为有I/O事件就绪，立即返回。可见，wakeup的调用开销不可忽视。

## Buffer的选择

通常情况下，操作系统的一次写操作分为两步： 1. 将数据从用户空间拷贝到系统空间。 2. 从系统空间往网卡写。同理，读操作也分为两步： ① 将数据从网卡拷贝到系统空间； ② 将数据从系统空间拷贝到用户空间。

对于NIO来说，缓存的使用可以使用DirectByteBuffer和HeapByteBuffer。如果使用了DirectByteBuffer，一般来说可以减少一次系统空间到用户空间的拷贝。但Buffer创建和销毁的成本更高，更不宜维护，通常会用内存池来提高性能。

如果数据量比较小的中小应用情况下，可以考虑使用heapBuffer；反之可以用directBuffer。

使用NIO != 高性能，当连接数<1000，并发程度不高或者局域网环境下NIO并没有显著的性能优势。

NIO并没有完全屏蔽平台差异，它仍然是基于各个操作系统的I/O系统实现的，差异仍然存在。使用NIO做网络编程构建事件驱动模型并不容易，陷阱重重。

推荐大家使用成熟的NIO框架，如Netty，MINA等。解决了很多NIO的陷阱，并屏蔽了操作系统的差异，有较好的性能和编程模型。

最后总结一下到底NIO给我们带来了些什么：

> - 事件驱动模型
> - 避免多线程
> - 单线程处理多任务
> - 非阻塞I/O，I/O读写不再阻塞，而是返回0
> - 基于block的传输，通常比基于流的传输更高效
> - 更高级的IO函数，zero-copy
> - IO多路复用大大提高了Java网络应用的可伸缩性和实用性

本文抛砖引玉，诠释了一些NIO的思想和设计理念以及应用场景，这只是从冰山一角。关于NIO可以谈的技术点其实还有很多，期待未来有机会和大家继续探讨。

### 作者简介

王烨，现在是美团旅游后台研发组的RD，之前曾经在百度、去哪儿和优酷工作过，专注Java后台开发。对于网络编程和并发编程具有浓厚的兴趣，曾经做过一些基础组件，也翻过一些源码，属于比较典型的宅男技术控。期待能够与更多知己，在coding的路上并肩前行~ 联系邮箱：wangye03@meituan.com

# Linux中select, poll, epoll

# 一 概念说明

在进行解释之前，首先要说明几个概念：
\- 用户空间和内核空间
\- 进程切换
\- 进程的阻塞
\- 文件描述符
\- 缓存 I/O

## 用户空间与内核空间

现在操作系统都是采用虚拟存储器，那么对32位操作系统而言，它的寻址空间（虚拟存储空间）为4G（2的32次方）。操作系统的核心是内核，独立于普通的应用程序，可以访问受保护的内存空间，也有访问底层硬件设备的所有权限。为了保证用户进程不能直接操作内核（kernel），保证内核的安全，操心系统将虚拟空间划分为两部分，一部分为内核空间，一部分为用户空间。针对linux操作系统而言，将最高的1G字节（从虚拟地址0xC0000000到0xFFFFFFFF），供内核使用，称为内核空间，而将较低的3G字节（从虚拟地址0x00000000到0xBFFFFFFF），供各个进程使用，称为用户空间。

## 进程切换

为了控制进程的执行，内核必须有能力挂起正在CPU上运行的进程，并恢复以前挂起的某个进程的执行。这种行为被称为进程切换。因此可以说，任何进程都是在操作系统内核的支持下运行的，是与内核紧密相关的。

从一个进程的运行转到另一个进程上运行，这个过程中经过下面这些变化：
\1. 保存处理机上下文，包括程序计数器和其他寄存器。
\2. 更新PCB信息。
\3. 把进程的PCB移入相应的队列，如就绪、在某事件阻塞等队列。
\4. 选择另一个进程执行，并更新其PCB。
\5. 更新内存管理的数据结构。
\6. 恢复处理机上下文。

注：**总而言之就是很耗资源**，具体的可以参考这篇文章：[进程切换](http://guojing.me/linux-kernel-architecture/posts/process-switch/)

## 进程的阻塞

正在执行的进程，由于期待的某些事件未发生，如请求系统资源失败、等待某种操作的完成、新数据尚未到达或无新工作做等，则由系统自动执行阻塞原语(Block)，使自己由运行状态变为阻塞状态。可见，进程的阻塞是进程自身的一种主动行为，也因此只有处于运行态的进程（获得CPU），才可能将其转为阻塞状态。`当进程进入阻塞状态，是不占用CPU资源的`。

## 文件描述符fd

文件描述符（File descriptor）是计算机科学中的一个术语，是一个用于表述指向文件的引用的抽象化概念。

文件描述符在形式上是一个非负整数。实际上，它是一个索引值，指向内核为每一个进程所维护的该进程打开文件的记录表。当程序打开一个现有文件或者创建一个新文件时，内核向进程返回一个文件描述符。在程序设计中，一些涉及底层的程序编写往往会围绕着文件描述符展开。但是文件描述符这一概念往往只适用于UNIX、Linux这样的操作系统。

## 缓存 I/O

缓存 I/O 又被称作标准 I/O，大多数文件系统的默认 I/O 操作都是缓存 I/O。在 Linux 的缓存 I/O 机制中，操作系统会将 I/O 的数据缓存在文件系统的页缓存（ page cache ）中，也就是说，数据会先被拷贝到操作系统内核的缓冲区中，然后才会从操作系统内核的缓冲区拷贝到应用程序的地址空间。

**缓存 I/O 的缺点：**
数据在传输过程中需要在应用程序地址空间和内核进行多次数据拷贝操作，这些数据拷贝操作所带来的 CPU 以及内存开销是非常大的。

# 二 IO模式

刚才说了，对于一次IO访问（以read举例），数据会先被拷贝到操作系统内核的缓冲区中，然后才会从操作系统内核的缓冲区拷贝到应用程序的地址空间。所以说，当一个read操作发生时，它会经历两个阶段：
\1. 等待数据准备 (Waiting for the data to be ready)
\2. 将数据从内核拷贝到进程中 (Copying the data from the kernel to the process)

正式因为这两个阶段，linux系统产生了下面五种网络模式的方案。
\- 阻塞 I/O（blocking IO）
\- 非阻塞 I/O（nonblocking IO）
\- I/O 多路复用（ IO multiplexing）
\- 信号驱动 I/O（ signal driven IO）
\- 异步 I/O（asynchronous IO）

注：由于signal driven IO在实际中并不常用，所以我这只提及剩下的四种IO Model。

## 阻塞 I/O（blocking IO）

在linux中，默认情况下所有的socket都是blocking，一个典型的读操作流程大概是这样：
![clipboard.png](https://segmentfault.com/img/bVm1c3)

当用户进程调用了recvfrom这个系统调用，kernel就开始了IO的第一个阶段：准备数据（对于网络IO来说，很多时候数据在一开始还没有到达。比如，还没有收到一个完整的UDP包。这个时候kernel就要等待足够的数据到来）。这个过程需要等待，也就是说数据被拷贝到操作系统内核的缓冲区中是需要一个过程的。而在用户进程这边，整个进程会被阻塞（当然，是进程自己选择的阻塞）。当kernel一直等到数据准备好了，它就会将数据从kernel中拷贝到用户内存，然后kernel返回结果，用户进程才解除block的状态，重新运行起来。

> 所以，blocking IO的特点就是在IO执行的两个阶段都被block了。

## 非阻塞 I/O（nonblocking IO）

linux下，可以通过设置socket使其变为non-blocking。当对一个non-blocking socket执行读操作时，流程是这个样子：
![clipboard.png](https://segmentfault.com/img/bVm1c4)

当用户进程发出read操作时，如果kernel中的数据还没有准备好，那么它并不会block用户进程，而是立刻返回一个error。从用户进程角度讲 ，它发起一个read操作后，并不需要等待，而是马上就得到了一个结果。用户进程判断结果是一个error时，它就知道数据还没有准备好，于是它可以再次发送read操作。一旦kernel中的数据准备好了，并且又再次收到了用户进程的system call，那么它马上就将数据拷贝到了用户内存，然后返回。

> 所以，nonblocking IO的特点是用户进程需要**不断的主动询问**kernel数据好了没有。

## I/O 多路复用（ IO multiplexing）

IO multiplexing就是我们说的select，poll，epoll，有些地方也称这种IO方式为event driven IO。select/epoll的好处就在于单个process就可以同时处理多个网络连接的IO。它的基本原理就是select，poll，epoll这个function会不断的轮询所负责的所有socket，当某个socket有数据到达了，就通知用户进程。

![clipboard.png](https://segmentfault.com/img/bVm1c5)

`当用户进程调用了select，那么整个进程会被block`，而同时，kernel会“监视”所有select负责的socket，当任何一个socket中的数据准备好了，select就会返回。这个时候用户进程再调用read操作，将数据从kernel拷贝到用户进程。

> 所以，I/O 多路复用的特点是通过一种机制一个进程能同时等待多个文件描述符，而这些文件描述符（套接字描述符）其中的任意一个进入读就绪状态，select()函数就可以返回。

这个图和blocking IO的图其实并没有太大的不同，事实上，还更差一些。因为这里需要使用两个system call (select 和 recvfrom)，而blocking IO只调用了一个system call (recvfrom)。但是，用select的优势在于它可以同时处理多个connection。

所以，如果处理的连接数不是很高的话，使用select/epoll的web server不一定比使用multi-threading + blocking IO的web server性能更好，可能延迟还更大。select/epoll的优势并不是对于单个连接能处理得更快，而是在于能处理更多的连接。）

在IO multiplexing Model中，实际中，对于每一个socket，一般都设置成为non-blocking，但是，如上图所示，整个用户的process其实是一直被block的。只不过process是被select这个函数block，而不是被socket IO给block。

## 异步 I/O（asynchronous IO）

inux下的asynchronous IO其实用得很少。先看一下它的流程：
![clipboard.png](https://segmentfault.com/img/bVm1c8)

用户进程发起read操作之后，立刻就可以开始去做其它的事。而另一方面，从kernel的角度，当它受到一个asynchronous read之后，首先它会立刻返回，所以不会对用户进程产生任何block。然后，kernel会等待数据准备完成，然后将数据拷贝到用户内存，当这一切都完成之后，kernel会给用户进程发送一个signal，告诉它read操作完成了。

## 总结

### blocking和non-blocking的区别

调用blocking IO会一直block住对应的进程直到操作完成，而non-blocking IO在kernel还准备数据的情况下会立刻返回。

### synchronous IO和asynchronous IO的区别

在说明synchronous IO和asynchronous IO的区别之前，需要先给出两者的定义。POSIX的定义是这样子的：
\- A synchronous I/O operation causes the requesting process to be blocked until that I/O operation completes;
\- An asynchronous I/O operation does not cause the requesting process to be blocked;

两者的区别就在于synchronous IO做”IO operation”的时候会将process阻塞。按照这个定义，之前所述的blocking IO，non-blocking IO，IO multiplexing都属于synchronous IO。

有人会说，non-blocking IO并没有被block啊。这里有个非常“狡猾”的地方，定义中所指的”IO operation”是指真实的IO操作，就是例子中的recvfrom这个system call。non-blocking IO在执行recvfrom这个system call的时候，如果kernel的数据没有准备好，这时候不会block进程。但是，当kernel中数据准备好的时候，recvfrom会将数据从kernel拷贝到用户内存中，这个时候进程是被block了，在这段时间内，进程是被block的。

而asynchronous IO则不一样，当进程发起IO 操作之后，就直接返回再也不理睬了，直到kernel发送一个信号，告诉进程说IO完成。在这整个过程中，进程完全没有被block。

**各个IO Model的比较如图所示：**
![clipboard.png](https://segmentfault.com/img/bVm1c9)

通过上面的图片，可以发现non-blocking IO和asynchronous IO的区别还是很明显的。在non-blocking IO中，虽然进程大部分时间都不会被block，但是它仍然要求进程去主动的check，并且当数据准备完成以后，也需要进程主动的再次调用recvfrom来将数据拷贝到用户内存。而asynchronous IO则完全不同。它就像是用户进程将整个IO操作交给了他人（kernel）完成，然后他人做完后发信号通知。在此期间，用户进程不需要去检查IO操作的状态，也不需要主动的去拷贝数据。

# 三 I/O 多路复用之select、poll、epoll详解

select，poll，epoll都是IO多路复用的机制。I/O多路复用就是通过一种机制，一个进程可以监视多个描述符，一旦某个描述符就绪（一般是读就绪或者写就绪），能够通知程序进行相应的读写操作。但select，poll，epoll本质上都是同步I/O，因为他们都需要在读写事件就绪后自己负责进行读写，也就是说这个读写过程是阻塞的，而异步I/O则无需自己负责进行读写，异步I/O的实现会负责把数据从内核拷贝到用户空间。（这里啰嗦下）

## select

```
int select (int n, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout);
```

select 函数监视的文件描述符分3类，分别是writefds、readfds、和exceptfds。调用后select函数会阻塞，直到有描述副就绪（有数据 可读、可写、或者有except），或者超时（timeout指定等待时间，如果立即返回设为null即可），函数返回。当select函数返回后，可以 通过遍历fdset，来找到就绪的描述符。

select目前几乎在所有的平台上支持，其良好跨平台支持也是它的一个优点。select的一 个缺点在于单个进程能够监视的文件描述符的数量存在最大限制，在Linux上一般为1024，可以通过修改宏定义甚至重新编译内核的方式提升这一限制，但 是这样也会造成效率的降低。

## poll

```
int poll (struct pollfd *fds, unsigned int nfds, int timeout);
```

不同与select使用三个位图来表示三个fdset的方式，poll使用一个 pollfd的指针实现。

```
struct pollfd {
    int fd; /* file descriptor */
    short events; /* requested events to watch */
    short revents; /* returned events witnessed */
};
```

pollfd结构包含了要监视的event和发生的event，不再使用select“参数-值”传递的方式。同时，pollfd并没有最大数量限制（但是数量过大后性能也是会下降）。 和select函数一样，poll返回后，需要轮询pollfd来获取就绪的描述符。

> 从上面看，select和poll都需要在返回后，`通过遍历文件描述符来获取已经就绪的socket`。事实上，同时连接的大量客户端在一时刻可能只有很少的处于就绪状态，因此随着监视的描述符数量的增长，其效率也会线性下降。

## epoll

epoll是在2.6内核中提出的，是之前的select和poll的增强版本。相对于select和poll来说，epoll更加灵活，没有描述符限制。epoll使用一个文件描述符管理多个描述符，将用户关系的文件描述符的事件存放到内核的一个事件表中，这样在用户空间和内核空间的copy只需一次。

### 一 epoll操作过程

epoll操作过程需要三个接口，分别如下：

```
int epoll_create(int size)；//创建一个epoll的句柄，size用来告诉内核这个监听的数目一共有多大
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event)；
int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);
```

**1. int epoll_create(int size);**
创建一个epoll的句柄，size用来告诉内核这个监听的数目一共有多大，这个参数不同于select()中的第一个参数，给出最大监听的fd+1的值，`参数size并不是限制了epoll所能监听的描述符最大个数，只是对内核初始分配内部数据结构的一个建议`。
当创建好epoll句柄后，它就会占用一个fd值，在linux下如果查看/proc/进程id/fd/，是能够看到这个fd的，所以在使用完epoll后，必须调用close()关闭，否则可能导致fd被耗尽。

**2. int epoll_ctl(int epfd, int op, int fd, struct epoll_event \*event)；**
函数是对指定描述符fd执行op操作。
\- epfd：是epoll_create()的返回值。
\- op：表示op操作，用三个宏来表示：添加EPOLL_CTL_ADD，删除EPOLL_CTL_DEL，修改EPOLL_CTL_MOD。分别添加、删除和修改对fd的监听事件。
\- fd：是需要监听的fd（文件描述符）
\- epoll_event：是告诉内核需要监听什么事，struct epoll_event结构如下：

```
struct epoll_event {
  __uint32_t events;  /* Epoll events */
  epoll_data_t data;  /* User data variable */
};

//events可以是以下几个宏的集合：
EPOLLIN ：表示对应的文件描述符可以读（包括对端SOCKET正常关闭）；
EPOLLOUT：表示对应的文件描述符可以写；
EPOLLPRI：表示对应的文件描述符有紧急的数据可读（这里应该表示有带外数据到来）；
EPOLLERR：表示对应的文件描述符发生错误；
EPOLLHUP：表示对应的文件描述符被挂断；
EPOLLET： 将EPOLL设为边缘触发(Edge Triggered)模式，这是相对于水平触发(Level Triggered)来说的。
EPOLLONESHOT：只监听一次事件，当监听完这次事件之后，如果还需要继续监听这个socket的话，需要再次把这个socket加入到EPOLL队列里
```

**3. int epoll_wait(int epfd, struct epoll_event \* events, int maxevents, int timeout);**
等待epfd上的io事件，最多返回maxevents个事件。
参数events用来从内核得到事件的集合，maxevents告之内核这个events有多大，这个maxevents的值不能大于创建epoll_create()时的size，参数timeout是超时时间（毫秒，0会立即返回，-1将不确定，也有说法说是永久阻塞）。该函数返回需要处理的事件数目，如返回0表示已超时。

### 二 工作模式

　epoll对文件描述符的操作有两种模式：**LT（level trigger）**和**ET（edge trigger）**。LT模式是默认模式，LT模式与ET模式的区别如下：
　　**LT模式**：当epoll_wait检测到描述符事件发生并将此事件通知应用程序，`应用程序可以不立即处理该事件`。下次调用epoll_wait时，会再次响应应用程序并通知此事件。
　　**ET模式**：当epoll_wait检测到描述符事件发生并将此事件通知应用程序，`应用程序必须立即处理该事件`。如果不处理，下次调用epoll_wait时，不会再次响应应用程序并通知此事件。

#### 1. LT模式

LT(level triggered)是缺省的工作方式，并且同时支持block和no-block socket.在这种做法中，内核告诉你一个文件描述符是否就绪了，然后你可以对这个就绪的fd进行IO操作。如果你不作任何操作，内核还是会继续通知你的。

#### 2. ET模式

ET(edge-triggered)是高速工作方式，只支持no-block socket。在这种模式下，当描述符从未就绪变为就绪时，内核通过epoll告诉你。然后它会假设你知道文件描述符已经就绪，并且不会再为那个文件描述符发送更多的就绪通知，直到你做了某些操作导致那个文件描述符不再为就绪状态了(比如，你在发送，接收或者接收请求，或者发送接收的数据少于一定量时导致了一个EWOULDBLOCK 错误）。但是请注意，如果一直不对这个fd作IO操作(从而导致它再次变成未就绪)，内核不会发送更多的通知(only once)

ET模式在很大程度上减少了epoll事件被重复触发的次数，因此效率要比LT模式高。epoll工作在ET模式的时候，必须使用非阻塞套接口，以避免由于一个文件句柄的阻塞读/阻塞写操作把处理多个文件描述符的任务饿死。

#### 3. 总结

**假如有这样一个例子：**
\1. 我们已经把一个用来从管道中读取数据的文件句柄(RFD)添加到epoll描述符
\2. 这个时候从管道的另一端被写入了2KB的数据
\3. 调用epoll_wait(2)，并且它会返回RFD，说明它已经准备好读取操作
\4. 然后我们读取了1KB的数据
\5. 调用epoll_wait(2)......

**LT模式：**
如果是LT模式，那么在第5步调用epoll_wait(2)之后，仍然能受到通知。

**ET模式：**
如果我们在第1步将RFD添加到epoll描述符的时候使用了EPOLLET标志，那么在第5步调用epoll_wait(2)之后将有可能会挂起，因为剩余的数据还存在于文件的输入缓冲区内，而且数据发出端还在等待一个针对已经发出数据的反馈信息。只有在监视的文件句柄上发生了某个事件的时候 ET 工作模式才会汇报事件。因此在第5步的时候，调用者可能会放弃等待仍在存在于文件输入缓冲区内的剩余数据。

当使用epoll的ET模型来工作时，当产生了一个EPOLLIN事件后，
读数据的时候需要考虑的是当recv()返回的大小如果等于请求的大小，那么很有可能是缓冲区还有数据未读完，也意味着该次事件还没有处理完，所以还需要再次读取：

```
while(rs){
  buflen = recv(activeevents[i].data.fd, buf, sizeof(buf), 0);
  if(buflen < 0){
    // 由于是非阻塞的模式,所以当errno为EAGAIN时,表示当前缓冲区已无数据可读
    // 在这里就当作是该次事件已处理处.
    if(errno == EAGAIN){
        break;
    }
    else{
        return;
    }
  }
  else if(buflen == 0){
     // 这里表示对端的socket已正常关闭.
  }

 if(buflen == sizeof(buf){
      rs = 1;   // 需要再次读取
 }
 else{
      rs = 0;
 }
}
```

> **Linux中的EAGAIN含义**

Linux环境下开发经常会碰到很多错误(设置errno)，其中EAGAIN是其中比较常见的一个错误(比如用在非阻塞操作中)。
从字面上来看，是提示再试一次。这个错误经常出现在当应用程序进行一些非阻塞(non-blocking)操作(对文件或socket)的时候。

例如，以 O_NONBLOCK的标志打开文件/socket/FIFO，如果你连续做read操作而没有数据可读。此时程序不会阻塞起来等待数据准备就绪返回，read函数会返回一个错误EAGAIN，提示你的应用程序现在没有数据可读请稍后再试。
又例如，当一个系统调用(比如fork)因为没有足够的资源(比如虚拟内存)而执行失败，返回EAGAIN提示其再调用一次(也许下次就能成功)。

### 三 代码演示

下面是一段不完整的代码且格式不对，意在表述上面的过程，去掉了一些模板代码。

```
#define IPADDRESS   "127.0.0.1"
#define PORT        8787
#define MAXSIZE     1024
#define LISTENQ     5
#define FDSIZE      1000
#define EPOLLEVENTS 100

listenfd = socket_bind(IPADDRESS,PORT);

struct epoll_event events[EPOLLEVENTS];

//创建一个描述符
epollfd = epoll_create(FDSIZE);

//添加监听描述符事件
add_event(epollfd,listenfd,EPOLLIN);

//循环等待
for ( ; ; ){
    //该函数返回已经准备好的描述符事件数目
    ret = epoll_wait(epollfd,events,EPOLLEVENTS,-1);
    //处理接收到的连接
    handle_events(epollfd,events,ret,listenfd,buf);
}

//事件处理函数
static void handle_events(int epollfd,struct epoll_event *events,int num,int listenfd,char *buf)
{
     int i;
     int fd;
     //进行遍历;这里只要遍历已经准备好的io事件。num并不是当初epoll_create时的FDSIZE。
     for (i = 0;i < num;i++)
     {
         fd = events[i].data.fd;
        //根据描述符的类型和事件类型进行处理
         if ((fd == listenfd) &&(events[i].events & EPOLLIN))
            handle_accpet(epollfd,listenfd);
         else if (events[i].events & EPOLLIN)
            do_read(epollfd,fd,buf);
         else if (events[i].events & EPOLLOUT)
            do_write(epollfd,fd,buf);
     }
}

//添加事件
static void add_event(int epollfd,int fd,int state){
    struct epoll_event ev;
    ev.events = state;
    ev.data.fd = fd;
    epoll_ctl(epollfd,EPOLL_CTL_ADD,fd,&ev);
}

//处理接收到的连接
static void handle_accpet(int epollfd,int listenfd){
     int clifd;     
     struct sockaddr_in cliaddr;     
     socklen_t  cliaddrlen;     
     clifd = accept(listenfd,(struct sockaddr*)&cliaddr,&cliaddrlen);     
     if (clifd == -1)         
     perror("accpet error:");     
     else {         
         printf("accept a new client: %s:%d\n",inet_ntoa(cliaddr.sin_addr),cliaddr.sin_port);                       //添加一个客户描述符和事件         
         add_event(epollfd,clifd,EPOLLIN);     
     } 
}

//读处理
static void do_read(int epollfd,int fd,char *buf){
    int nread;
    nread = read(fd,buf,MAXSIZE);
    if (nread == -1)     {         
        perror("read error:");         
        close(fd); //记住close fd        
        delete_event(epollfd,fd,EPOLLIN); //删除监听 
    }
    else if (nread == 0)     {         
        fprintf(stderr,"client close.\n");
        close(fd); //记住close fd       
        delete_event(epollfd,fd,EPOLLIN); //删除监听 
    }     
    else {         
        printf("read message is : %s",buf);        
        //修改描述符对应的事件，由读改为写         
        modify_event(epollfd,fd,EPOLLOUT);     
    } 
}

//写处理
static void do_write(int epollfd,int fd,char *buf) {     
    int nwrite;     
    nwrite = write(fd,buf,strlen(buf));     
    if (nwrite == -1){         
        perror("write error:");        
        close(fd);   //记住close fd       
        delete_event(epollfd,fd,EPOLLOUT);  //删除监听    
    }else{
        modify_event(epollfd,fd,EPOLLIN); 
    }    
    memset(buf,0,MAXSIZE); 
}

//删除事件
static void delete_event(int epollfd,int fd,int state) {
    struct epoll_event ev;
    ev.events = state;
    ev.data.fd = fd;
    epoll_ctl(epollfd,EPOLL_CTL_DEL,fd,&ev);
}

//修改事件
static void modify_event(int epollfd,int fd,int state){     
    struct epoll_event ev;
    ev.events = state;
    ev.data.fd = fd;
    epoll_ctl(epollfd,EPOLL_CTL_MOD,fd,&ev);
}

//注：另外一端我就省了
```

### 四 epoll总结

在 select/poll中，进程只有在调用一定的方法后，内核才对所有监视的文件描述符进行扫描，而**epoll事先通过epoll_ctl()来注册一 个文件描述符，一旦基于某个文件描述符就绪时，内核会采用类似callback的回调机制，迅速激活这个文件描述符，当进程调用epoll_wait() 时便得到通知**。(`此处去掉了遍历文件描述符，而是通过监听回调的的机制`。这正是epoll的魅力所在。)

**epoll的优点主要是一下几个方面：**
\1. 监视的描述符数量不受限制，它所支持的FD上限是最大可以打开文件的数目，这个数字一般远大于2048,举个例子,在1GB内存的机器上大约是10万左 右，具体数目可以cat /proc/sys/fs/file-max察看,一般来说这个数目和系统内存关系很大。select的最大缺点就是进程打开的fd是有数量限制的。这对 于连接数量比较大的服务器来说根本不能满足。虽然也可以选择多进程的解决方案( Apache就是这样实现的)，不过虽然linux上面创建进程的代价比较小，但仍旧是不可忽视的，加上进程间数据同步远比不上线程间同步的高效，所以也不是一种完美的方案。

1. IO的效率不会随着监视fd的数量的增长而下降。epoll不同于select和poll轮询的方式，而是通过每个fd定义的回调函数来实现的。只有就绪的fd才会执行回调函数。

> 如果没有大量的idle -connection或者dead-connection，epoll的效率并不会比select/poll高很多，但是当遇到大量的idle- connection，就会发现epoll的效率大大高于select/poll。

## select和epoll的区别(面试常考)

- 首先select是posix支持的，而epoll是linux特定的系统调用，因此，epoll的可移植性就没有select好，但是考虑到epoll和select一般用作服务器的比较多，而服务器中大多又是linux，所以这个可移植性的影响应该不会很大。
- 其次，select可以监听的文件描述符有限，最大值为1024，而epoll可以监听的文件描述符则是系统对整个进程限制的最大文件描述符。
- 接下来就要谈epoll和select的性能比较了，这个一般情况下应该是epoll表现好一些，否则linux也不会去特定实现epoll函数了，那么epoll为什么比select更高效呢？原因有很多，第一点，epoll通过每次有就绪事件时都将其插入到一个就绪队列中，使得epoll_wait的返回结果中只存储了已经就绪的事件，而select则返回了所有被监听的事件，事件是否就绪需要应用程序去检测，那么如果已被监听但未就绪的事件较多的话，对性能的影响就比较大了。第二点，每一次调用select获得就绪事件时都要将需要监听的事件重复传递给操作系统内核，而epoll对监听文件描述符的处理则和获得就绪事件的调用分开，这样获得就绪事件的调用epoll_wait就不需要重新传递需要监听的事件列表，这种重复的传递需要监听的事件也是性能低下的原因之一。除此之外，epoll的实现中使用了mmap调用使得内核空间和用户空间共享内存，从而避免了过多的内核和用户空间的切换引起的开销。
- 然后就是epoll提供了两种工作模式，一种是水平触发模式，这种模式和select的触发方式是一样的，要只要文件描述符的缓冲区中有数据，就永远通知用户这个描述符是可读的，这种模式对block和noblock的描述符都支持，编程的难度也比较小；而另一种更高效且只有epoll提供的模式是边缘触发模式，只支持nonblock的文件描述符，他只有在文件描述符有新的监听事件发生的时候（例如有新的数据包到达）才会通知应用程序，在没有新的监听时间发生时，即使缓冲区有数据（即上一次没有读完，或者甚至没有读），epoll也不会继续通知应用程序，使用这种模式一般要求应用程序收到文件描述符读就绪通知时，要一直读数据直到收到EWOULDBLOCK/EAGAIN错误，使用边缘触发就必须要将缓冲区中的内容读完，否则有可能引起死等,尤其是当一个listen_fd需要监听到达连接的时候，如果多个连接同时到达，如果每次只是调用accept一次，就会导致多个连接在内核缓冲区中滞留，处理的办法是用while循环抱住accept，直到其出现EAGAIN。这种模式虽然容易出错，但是性能要比前面的模式更高效，因为只需要监听是否有事件发生，发生了就直接将描述符加入就绪队列即可。

## select

## 一、什么是select

**系统提供select函数来实现多路复用输入/输出模型.**select系统调用是用来让我们的程序监视多个文件描述符的状态变化的;程序会停在select这里等待，直到被监视的文件描述符有一个或多个发生了状态改变。

## 1.select函数原型

select的函数原型如下: #include <sys/select.h>

```text
int select(int nfds, fd_set *readfds, fd_set *writefds,
fd_set *exceptfds, struct timeval *timeout);
```

## 2.参数解释

- fd_set:描述符集合 这个结构体中有一个数组，作用是用于向数组中添加描述符，将描述符添加到集合中，实际上是将描述符这个数字对应的比特位置1；而这个位图中能够添加多少描述符取决于一个宏：_FD_SETSIZE=1024,因此select模型所能够监控的描述符是有最大数量限制的；
- 参数nfds是需要监视的最大的文件描述符值+1；
- rdset,wrset,exset分别对应于需要检测的可读文件描述符的集合，可写文件描述符的集 合及异常文件描述符的集合;
- 参数timeout为结构timeval，用来设置select()的等待时间

## 3.参数timeout取值

- NULL：则表示select（）没有timeout，select将一直被阻塞，直到某个文件描述符上发生了事件;
- 0：仅检测描述符集合的状态，然后立即返回，并不等待外部事件的发生。
- 特定的时间值：如果在指定的时间段里没有事件发生，select将超时返回

## 4.返回值

- ( >0 ) 表示当前集合中多少描述符就绪了
- ( ==0 ) 表示等待超时了(在阻塞的时间段内，一直没有就绪的描述符)
- ( <0 ) 表示监控出错了(select本次监控出错)

## 5.监控原理

- 1.用户向定义各个自己关心的描述符集合，将描述符添加到相应的集合中。
- 2.调用select接口，将集合传入，将集合中数据拷贝到内核中进行监控，监控原理：在内核中不断进行轮询遍历，判断哪个描述符就绪`可读就绪：读缓冲区中，数据大小大于低水位标记(通常是1个字节)可写就绪：写缓冲区中，剩余空间大小大于低水位标记(通常是1个字节)当前任意一个集合中有描述符就绪，则遍历完集合之后select调用返回select在调用返回之前，将集合中所有未就绪的描述符从集合中移除了select返回的集合是一个就绪描述符集合
- 3.用户在select调用返回之后虽然无法立即获取就绪的描述符，但是可以通过判断当前哪个描述符还在集合中来判断描述符就是就绪描述符，然后进行相应操作。
- 4.因为集合被select在就绪返回前被修改了，仅仅保留了就绪的描述符，因此每次重新监控前需要重新添加到描述符集合中。

## 二、select就绪条件

## 1.读就绪

- socket内核中, 接收缓冲区中的字节数, 大于等于低水位标记SO_RCVLOWAT. 此时可以无阻塞的读该文件描述符,
  并且返回值大于0;
- socket TCP通信中, 对端关闭连接, 此时对该socket读, 则返回0;
- 监听的socket上有新的连接请求;
- socket上有未处理的错误；

## 2.写就绪

- socket内核中, 发送缓冲区中的可用字节数(发送缓冲区的空闲位置大小), 大于等于低水位标记SO_SNDLOWAT,
  此时可以无阻塞的写, 并且返回值大于0;
- socket的写操作被关闭(close或者shutdown). 对一个写操作被关闭的socket进行写操作, 会触发SIGPIPE信号;
- socket使用非阻塞connect连接成功或失败之后;
- socket上有未读取的错误

## 三、select的特点

1. 可监控的文件描述符个数取决于sizeof(fd_set)的值.
   我这边服务器上sizeof(fd_set)＝512，每bit表示一个文件描述符，则我服务器上支持的最大文件描述符是512*8=4096.
2. 将fd加入select监控集的同时，还要再使用一个数据结构array保存放到select监控集中的fd， ①是用于在select 返回后，array作为源数据和fd_set进行FD_ISSET判断。 ②是select返回后会把以前加入的但并无事件发生的fd清空，则每次开始select前都要重新从array取得fd逐一加入(FD_ZERO最先)，扫描array的同时取得fd最大值maxfd，用于select的第一个参数。

## 四、select的优缺点

## 1.缺点

- \1. select所你能监控的描述符有最大数量上限，上限取决于__FD_SETSIZE默认等于1024
- \2. select需要每次将集合数据从用户态拷贝到内核态进行监控
- \3. select在内核中使用轮询遍历进行监控，监控性能随着描述符增多而降低
- \4. select不会直接返回给用户就绪的描述符，而是返回的一个就绪集合，需要用户自己遍历判断才能找出就绪的描述符，效率较低，增加了代码复杂度
- \5. select每次调用返回时，都会修改集合，因此每次监控前都需要重新向集合中添加描述符

## 2.优点

- 1.遵循posix标准，拥有良好的跨平台移植性
- 2.监控时间可以精细到微秒

## 五、select使用实例

使用 select 实现字典服务器

tcp_select_server.hpp

```text
#pragma once
#include <vector>
#include <unordered_map>
#include <functional>
#include <sys/select.h>
#include "tcp_socket.hpp"
// 必要的调试函数
inline void PrintFdSet(fd_set* fds, int max_fd) {
printf("select fds: ");
for (int i = 0; i < max_fd + 1; ++i) {
if (!FD_ISSET(i, fds)) {
continue;
}
printf("%d ", i);
}
printf("\n");
}
typedef std::function<void (const std::string& req, std::string* resp)> Handler;
// 把 Select 封装成一个类. 这个类虽然保存很多 TcpSocket 对象指针, 但是不管理内存
class Selector {
public:
Selector() {
// [注意!] 初始化千万别忘了!!
max_fd_ = 0;
FD_ZERO(&read_fds_);
}
bool Add(const TcpSocket& sock) {
int fd = sock.GetFd();
printf("[Selector::Add] %d\n", fd);
if (fd_map_.find(fd) != fd_map_.end()) {
printf("Add failed! fd has in Selector!\n");
return false;
}
fd_map_[fd] = sock;
FD_SET(fd, &read_fds_);
if (fd > max_fd_) {
max_fd_ = fd;
}
return true;
}
bool Del(const TcpSocket& sock) {
int fd = sock.GetFd();
printf("[Selector::Del] %d\n", fd);
if (fd_map_.find(fd) == fd_map_.end()) {
printf("Del failed! fd has not in Selector!\n");
return false;
}
fd_map_.erase(fd);
FD_CLR(fd, &read_fds_);
// 重新找到最大的文件描述符, 从右往左找比较快
for (int i = max_fd_; i >= 0; --i) {
if (!FD_ISSET(i, &read_fds_)) {
continue;
}
max_fd_ = i;
break;
}
return true;
}
// 返回读就绪的文件描述符集
bool Wait(std::vector<TcpSocket>* output) {
output->clear();
// [注意] 此处必须要创建一个临时变量, 否则原来的结果会被覆盖掉
fd_set tmp = read_fds_;
// DEBUG
PrintFdSet(&tmp, max_fd_);
int nfds = select(max_fd_ + 1, &tmp, NULL, NULL, NULL);
if (nfds < 0) {
perror("select");
return false;
}
// [注意!] 此处的循环条件必须是 i < max_fd_ + 1
for (int i = 0; i < max_fd_ + 1; ++i) {
if (!FD_ISSET(i, &tmp)) {
continue;
}
output->push_back(fd_map_[i]);
}
return true;
}
private:
fd_set read_fds_;
int max_fd_;
// 文件描述符和 socket 对象的映射关系
std::unordered_map<int, TcpSocket> fd_map_;
};
class TcpSelectServer {
public:
TcpSelectServer(const std::string& ip, uint16_t port) : ip_(ip), port_(port) {
}
bool Start(Handler handler) const {
// 1. 创建 socket
TcpSocket listen_sock;
bool ret = listen_sock.Socket();
if (!ret) {
return false;
}
// 2. 绑定端口号
ret = listen_sock.Bind(ip_, port_);
if (!ret) {
return false;
}
// 3. 进行监听
ret = listen_sock.Listen(5);
if (!ret) {
return false;
}
// 4. 创建 Selector 对象
Selector selector;
selector.Add(listen_sock);
// 5. 进入事件循环
for (;;) {
std::vector<TcpSocket> output;
bool ret = selector.Wait(&output);
if (!ret) {
continue;
}
// 6. 根据就绪的文件描述符的差别, 决定后续的处理逻辑
for (size_t i = 0; i < output.size(); ++i) {
if (output[i].GetFd() == listen_sock.GetFd()) {
// 如果就绪的文件描述符是 listen_sock, 就执行 accept, 并加入到 select 中
TcpSocket new_sock;
listen_sock.Accept(&new_sock, NULL, NULL);
selector.Add(new_sock);
} else {
// 如果就绪的文件描述符是 new_sock, 就进行一次请求的处理
std::string req, resp;
bool ret = output[i].Recv(&req);
if (!ret) {
selector.Del(output[i]);
// [注意!] 需要关闭 socket
output[i].Close();
continue;
}
// 调用业务函数计算响应
handler(req, &resp);
// 将结果写回到客户端
output[i].Send(resp);
}
} // end for
} // end for (;;)
return true;
}
private:
std::string ip_;
uint16_t port_;
};
```

[http://dict_server.cc](https://link.zhihu.com/?target=http%3A//dict_server.cc)这个代码和之前相同, 只是把里面的 server 对象改成 TcpSelectServer 类即可.客户端和之前的客户端完全相同, 无需单独开发

## poll

int poll(struct poofd*fds,nfds_t nfds,int timeout)

struct pollfd{int fd; //文件描述符short events //对当前描述符实际就绪的事件short revents //当前描述符实际就绪的事件}

**poll对描述符进行监控，是对最关心的描述符组织一个事件结构，填充信息：events中填充，用户关心的事件，进行监控后，若描述符就绪了整个事件，则将这个事件在revents中进行记录**

- 1.用户定义一个struct pollfd时间结构数组，将关心的描述符相关的信息填充进去
- 2.将调用poll接口，将数据拷贝到内核进行监控，监控原理：在内核中，对每一个事件进行轮询遍历监控，当有描述符就绪时，则将就绪的事件信息记录到相应的结构数组节点的revents这个成员中，调用返回。
- 3.用户遍历数组，通过判断每一个节点revents，判断描述是否就绪，进而直接对节点中fd进行操作

## epoll

epoll是为了处理大量的句柄而改进的poll

## 一、epoll_create

```text
int epoll_create(int size);
```

## 创建一个epoll的句柄

- 自从linux2.6.8之后，size参数是被忽略的.
- 用完之后, 必须调用close()关闭

## 二、epoll_wait

```text
int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);
```

## 1.收集在epoll监控的事件中已经发送的事件

1. 参数events是分配好的epoll_event结构体数组.
2. epoll将会把发生的事件赋值到events数组中
   (events不可以是空指针，内核只负责把数据复制到这个events数组中，不会去帮助我们在用户态中分配内存).
3. maxevents告之内核这个events有多大，这个 maxevents的值不能大于创建epoll_create()时的size.
4. 参数timeout是超时时间 (毫秒，0会立即返回，-1是永久阻塞).
5. 如果函数调用成功，返回对应I/O上已准备好的文件描述符数目，如返回0表示已超时, 返回小于0表示函数失败.

## 2.epoll的优点(和 select 的缺点对应)

1. 接口使用方便: 虽然拆分成了三个函数, 但是反而使用起来更方便高效. 不需要每次循环都设置关注的文件描述符, 也做到了输入输出参数分离开
2. 数据拷贝轻量: 只在合适的时候调用 EPOLL_CTL_ADD 将文件描述符结构拷贝到内核中,
   这个操作并不频繁(而select/poll都是每次循环都要进行拷贝)
3. 事件回调机制: 避免使用遍历, 而是使用回调函数的方式, 将就绪的文件描述符结构加入到就绪队列中,epoll_wait
   返回直接访问就绪队列就知道哪些文件描述符就绪. 这个操作时间复杂度O(1). 即使文件描述符数目很多, 效率也不会受到影响.
4. 没有数量限制: 文件描述符数目无上限

## 三、epoll的使用场景

- epoll的高性能, 是有一定的特定场景的. 如果场景选择的不适宜, epoll的性能可能适得其反。
- 对于多连接, 且多连接中只有一部分连接比较活跃时, 比较适合使用epoll。

例如, 典型的一个需要处理上万个客户端的服务器, 例如各种互联网APP的入口服务器, 这样的服务器就很适合epoll.如果只是系统内部, 服务器和服务器之间进行通信, 只有少数的几个连接, 这种情况下用epoll就并不合适. 具体要根据需求和场景特点来决定使用哪种IO模型。

# Java中equals和hashcode方法

1.使用Object默认的equals()和hashCode()方法：

public class HashCode {
    private String name;
    private int age;

    public HashCode(String name,int age) {
        this.name = name;
        this.age = age;
    }
    public static void main(String[] args) {
        HashCode code_1 = new HashCode("Chen",10);
        HashCode code_2 = new HashCode("Chen",10);
        boolean b = code_1.equals(code_2);
        System.out.println(b);
        System.out.println(code_1.hashCode());
        System.out.println(code_2.hashCode());
    }
}
结果：false
1163157884
1956725890  

2.使用重写的equals()和hashCode()方法：

public class HashCode {
    private String name;
    private int age;

    public HashCode(String name,int age) {
        this.name = name;
        this.age = age;
    }
    /*重写的equals方法：会经过三个步骤比较，（1）比较内存地址（2）判断两个对象的类型是否一致
    * （3）判断对象的属性是否相等*/
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof HashCode)) return false;
        HashCode code = (HashCode) o;
        if (age != code.age) return false;
        return name.equals(code.name);
    }
    @Override
    public int hashCode() {
        int result = name.hashCode();
        result = 31 * result + age;
        return result;
    }
    public static void main(String[] args) {
        HashCode code_1 = new HashCode("Chen",10);
        HashCode code_2 = new HashCode("Chen",10);
        boolean b = code_1.equals(code_2);
        System.out.println(b);
        System.out.println(code_1.hashCode());
        System.out.println(code_2.hashCode());
    }
}
结果：true
65074652
65074652

从两次结果可以看出，使用Object默认的equals()和hashCode()方法，与重写之后，运行结果完全不一样。

/*因为Object中默认的equals方法，内部还是使用==来比较对象在内存中的地址，所以结果位false*/
/*如果重写了equals方法，那么如果两个对象的属性值相同，那么程序会在第三步判断中返回true，
* hashCode()方法，它是一个本地方法，底层是运用对象的内存地址通过哈希函数来计算的。
* 问题：为什么重写equals时，一定需要重写hashCode()方法？
* 因为重写了equals()之后，判断两个属性值相同的对象时，会返回true，如果没有重写hashCode(),
* 那么程序还是按照默认的使用内存地址的方法去计算，那么一定会返回false，
* java中规定：两个对象的equals()相同，hashCode一定相同。hashCode相同，但equals不一定相同
* 所以在重写equals时，一定需要重写hashCode。*/
/*什么时候需要重写类的equals()和hashCode()方法？
* 1.当我们需要重新定义两个对象是否相等的条件时，需要进行重写。比如通常情况下，我们认为两个不同对象的某些属性值相同时，
* 就认为这两个对象是相同的。
* 例如：我们在HashMap中添加元素时，我们认为当key相同时，两个元素就相同，但是默认的Object中的equals(),只是单纯的比较两个元素  的内存地址是否相同，不能满足我们的要求，所以需要重写。
* 2.当我们自定义一个类时，想要把它的实例保存在集合时，就需要重写equals()和hashCode()方法。*/
————————————————
版权声明：本文为CSDN博主「稻草人……」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_42493179/article/details/103406616

# Java中枚举类中==和equals

### 问题

我知道[Java](http://lib.csdn.net/base/java)枚举会被编译成一个包含私有构造参数和一堆静态方法的类，当去比较两个枚举的时候，总是使用equals()方法，例如：

```
public useEnums(SomeEnum a)
{
    if(a.equals(SomeEnum.SOME_ENUM_VALUE))
    {
        ...
    }
    ...
}
```

除此之外，我也可以使用 == 替代equals() 方法

```
public useEnums2(SomeEnum a)
{
    if(a == SomeEnum.SOME_ENUM_VALUE)
    {
        ...
    }
    ...
}
```

我有5年以上的java编程经验，并且我想我也懂得 == 和 equals() 之间的区别，但是我仍然觉得很困惑，哪一个操作符才是我该使用的。

### 答案

二者皆对，如果你看过枚举的源码，你会发现在源码中，equals也仅仅非常简单的 == 。 我使用 == ，因为无论如何，这个左值是可以为 null的

译者补充 java.lang.Enum 中Equals 代码：

```
public final boolean equals(Object other) {
    return this==other;
}
```

### 额外答案

#### 能在枚举中使用 == 进行判断？

答案是肯定的，因为枚举有着严格的实例化控制，所以你可以用 == 去做比较符，这个用法，在官方文档中也有明确的说明。

> JLS 8.9 Enums 一个枚举类型除了定义的那些枚举常量外没有其他实例了。 试图明确地说明一种枚举类型是会导致编译期异常。在枚举中final clone方法确保枚举常量从不会被克隆，而且序列化机制会确保从不会因为反序列化而创造复制的实例。枚举类型的反射实例化也是被禁止的。总之，以上内容确保了除了定义的枚举常量之外，没有枚举类型实例。

因为每个枚举常量只有一个实例，所以如果在比较两个参考值，至少有一个涉及到枚举常量时，允许使用“==”代替equals()。（equals()方法在枚举类中是一个final方法，在参数和返回结果时，很少调用父类的equals()方法，因此是一种恒等的比较。）

#### 什么时候 == 和 equals 不一样？

As a reminder, it needs to be said that generally, == is NOT a viable alternative to equals. When it is, however (such as with enum), there are two important differences to consider: 通常来说 == 不是一个 equals的一个备选方案，无论如何有2个重要的不同处需要考虑：

##### == 不会抛出 NullPointerException

```
enum Color { BLACK, WHITE };

Color nothing = null;
if (nothing == Color.BLACK);      // runs fine
if (nothing.equals(Color.BLACK)); // throws NullPointerException
```

##### == 在编译期检测类型兼容性

```
enum Color { BLACK, WHITE };
enum Chiral { LEFT, RIGHT };

if (Color.BLACK.equals(Chiral.LEFT)); // compiles fine
if (Color.BLACK == Chiral.LEFT);      // DOESN'T COMPILE!!! Incompatible types!
```

#### 什么时候使用 == ？

Bloch specifically mentions that immutable classes that have proper control over their instances can guarantee to their clients that == is usable. enum is specifically mentioned to exemplify. 具体来说，那些提供恰当实例控制的不可变类能够保证 == 是可用的，枚举刚好符合这个条件。

考虑静态工厂方法代替构造器 它使得不可变的类可以确保不会存在两个相等的实例，即当且仅当a==b的时候才有a.equals(b)为true。如果类保证了这一点，它的客户端可以使用“==”操作符来代替equals（Object）方法，这样可以提升性能。枚举类型保证了这一点

总而言之，在枚举比较上使用 == ， 因为： 1. 能正常工作 2. 更快 3. 运行时是安全的 4. 编译期也是安全的

stackoverlfow链接：http://stackoverflow.com/questions/1750435/comparing-java-enum-members-or-equals

# Java 中 final, static, this, super 关键字总结

### 1. final 关键字

final关键字主要用在三个地方：变量、方法、类。

- 对于一个final**变量**，如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改；如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象。
- 当用final修饰一个**类**时，表明这个类不能被继承。final类中的所有成员方法都会被隐式地指定为final方法。
- 使用final **方法**的原因有两个:
  1. 把方法锁定，以防任何继承类修改它的含义；
  2. 效率。在早期的Java实现版本中，会将final方法转为内嵌调用。但是如果方法过于庞大，可能看不到内嵌调用带来的任何性能提升（现在的Java版本已经不需要使用final方法进行这些优化了）。类中所有的private方法都隐式地指定为final。

### 2. static 关键字

static 关键字主要有以下四种使用场景：

1. **修饰成员变量和成员方法**: 被 static修饰的成员属于类，不属于单个这个类的某个对象，**被类中所有对象共享**，可以并且建议通过类名调用。被static 声明的成员变量属于静态成员变量，静态变量 存放在 Java **内存区域的方法区**。

   调用格式：类名.静态变量名 类名.静态方法名()

2. **静态代码块**: 静态代码块定义在类中方法外, 静态代码块在非静态代码块之前执行(静态代码块—>非静态代码块—>构造方法)。 该类不管创建多少对象，静态代码块只执行一次.

3. **静态内部类**（static修饰类的话只能修饰内部类）： 静态内部类与非静态内部类之间存在一个最大的区别: **非静态内部类在编译完成之后会隐含地保存着一个引用，该引用是指向创建它的外围类，但是静态内部类却没有。**
   没有这个引用就意味着：

   1. **它的创建是不需要依赖外围类的创建。**
   2. **它不能使用任何外围类的非static成员变量和方法。**

4. **静态导包**(用来导入类中的静态资源，1.5之后的新特性): 格式为：`import static` 这两个关键字连用可以指定导入某个类中的指定静态资源，并且不需要使用类名调用类中静态成员，可以直接使用类中静态成员变量和成员方法。

### 3. this 关键字

this关键字用于引用类的当前实例。 例如：

```java
class Manager {
    Employees[] employees;
     
    void manageEmployees() {
        int totalEmp = this.employees.length;
        System.out.println("Total employees: " + totalEmp);
        this.report();
    }
     
    void report() { }
}
1234567891011
```

在上面的示例中，this关键字用于两个地方：

`this.employees.length`：访问类Manager的当前实例的变量。

`this.report()`：调用类Manager的当前实例的方法。

此关键字是可选的，这意味着如果上面的示例在不使用此关键字的情况下表现相同。 但是，使用此关键字可能会使代码更易读或易懂。

### 4. super 关键字

super关键字用于从子类访问父类的变量和方法。 例如：

```java
public class Super {
    protected int number;
     
    protected showNumber() {
        System.out.println("number = " + number);
    }
}
1234567
    public class Sub extends Super {
        void bar() {
            super.number = 10;
            super.showNumber();
        }
    }
123456
```

在上面的例子中，`Sub` 类访问父类成员变量 `number` 并调用其其父类 `Super` 的 `showNumber()` 方法。

#### 使用 this 和 super 要注意的问题：

super 调用父类中的其他构造方法时，调用时要放在构造方法的首行！this 调用本类中的其他构造方法时，也要放在首行。

this、super不能用在static方法中。

简单解释一下：

被 static 修饰的成员属于类，不属于单个这个类的某个对象，被类中所有对象共享。

- 而 this 代表对本类对象的引用，指向本类对象；
- 而 super 代表对父类对象的引用，指向父类对象；

所以， this和super是属于对象范畴的东西，而静态方法是属于类范畴的东西。

# iterator和iterable的区别

在Java中，我们可以对List集合进行如下几种方式的遍历：

```java
List<Integer> list = new ArrayList<>();
list.add(5);
list.add(23);
list.add(42);
for (int i = 0; i < list.size(); i++) {
    System.out.print(list.get(i) + ",");
}

Iterator it = list.iterator();
while (it.hasNext()) {
    System.out.print(it.next() + ",");
}

for (Integer i : list) {
    System.out.print(i + ",");
}
```

第一种就是普通的for循环，第二种为迭代器遍历，第三种是for each循环。后面两种方式涉及到Java中的iterator和iterable对象，接下来我们来看看这两个对象的区别以及如何在自定义类中实现for each循环。

## Iterator与Iterable

iterator为Java中的迭代器对象，是能够对List这样的集合进行迭代遍历的底层依赖。而iterable接口里定义了返回iterator的方法，相当于对iterator的封装，同时实现了iterable接口的类可以支持for each循环。

### iterator内部细节

jdk中Iterator接口主要方法如下：

```java
public interface Iterator<E> {
	boolean hasNext();
  	E next();
}
```

iterator通过以上两个方法定义了对集合迭代访问的方法，而具体的实现方式依赖于不同的实现类，具体的集合类实现Iterator接口中的方法以实现迭代。

可以发现，在List中并没有实现Iterator接口，而是实现的Iterable接口。进一步观察Iterable接口的源码可以发现其只是返回了一个Iterator对象。

```java
public interface Iterable<T> {
  Iterator<T> iterator();
}
```

所以我们可以使用如下方式来对List进行迭代了（通过调用iterator()方法）

```java
Iterator it = list.iterator();
while (it.hasNext()) {
    System.out.print(it.next() + ",");
}
```

同时实现了Iterable接口的还可以使用for each循环。

### for each原理

其实for each循环内部也是依赖于Iterator迭代器，只不过Java提供的语法糖，Java编译器会将其转化为Iterator迭代器方式遍历。我们对以下for each循环进行反编译：

```java
 for (Integer i : list) {
       System.out.println(i);
   }
```

反编译后：

```java
Integer i;
for(Iterator iterator = list.iterator(); iterator.hasNext(); System.out.println(i)){
        i = (Integer)iterator.next();        
    }
```

可以看到Java的for each增强循环是通过iterator迭代器方式实现的。

### 深入探讨Iterable与Iterator关系

有一个问题，为什么不直接将hasNext()，next()方法放在Iterable接口中，其他类直接实现就可以了？

原因是有些集合类可能不止一种遍历方式，实现了Iterable的类可以再实现多个Iterator内部类，例如`LinkedList`中的`ListItr`和`DescendingIterator`两个内部类，就分别实现了双向遍历和逆序遍历。通过返回不同的`Iterator`实现不同的遍历方式，这样更加灵活。如果把两个接口合并，就没法返回不同的`Iterator`实现类了。ListItr相关源码如下：

```java
    public ListIterator<E> listIterator(int index) {
        checkPositionIndex(index);
        return new ListItr(index);
    }

    private class ListItr implements ListIterator<E> {
		...
        ListItr(int index) {
            // assert isPositionIndex(index);
            next = (index == size) ? null : node(index);
            nextIndex = index;
        }

        public boolean hasNext() {
            return nextIndex < size;
        }
    	...
```

如上所示可以通过调用`list.listIterator()`方法返回iterator迭代器（`list.iterator()`只是其默认实现）

`DescendingIterator`源码如下：

```java
    public Iterator<E> descendingIterator() {
        return new DescendingIterator();
    }
    private class DescendingIterator implements Iterator<E> 	{
        private final ListItr itr = new ListItr(size());
        public boolean hasNext() {
            return itr.hasPrevious();
        }
        public E next() {
            return itr.previous();
        }
        public void remove() {
            itr.remove();
        }
    }
```

同样可以通过`list.descendingIterator()`使用该迭代器。

## 实现自己的迭代器

我们现在有一个自定义类ArrayMap，现在如果对其进行如下for each遍历：

```java
ArrayMap<String, Integer> am = new ArrayMap<>();
am.put("hello", 5);
am.put("syrups", 10);

for (String s: am) {
   System.out.println(s);
}
```

由于我们并没有实现hashNext和next抽象方法，所以无法对其进行遍历。

### 自定义迭代器类

我们首先自定义一个迭代器类实现hashNext和next方法，并将其作为ArrayMap的内部类，相关代码如下:

```java
   public class KeyIterator implements Iterator<K> {
        private int ptr;

        public KeyIterator() {
            ptr = 0;
        }

        @Override
        public boolean hasNext() {
            return (ptr != size);
        }

        @Override
        public K next() {
            K returnItem = keys[ptr];
            ptr += 1;
            return returnItem;
        }
    }
```

可以看到我们在next中指定的遍历规则是根据ArrayMap的key值进行遍历。有了上述迭代器类，我们就可以使用iterator方式在外部对其进行遍历了，遍历代码如下：

```java
ArrayMap<String, Integer> am = new ArrayMap<>();
am.put("hello", 5);
am.put("syrups", 10);
ArrayMap.KeyIterator ami = am.new KeyIterator();
while (ami.hasNext()) {
    System.out.println(ami.next());
}
```

如上所示，通过创建KeyIterator对象进行迭代访问（注意外部类创建内部类对象的方式）。

### 支持for each循环

现在还不能支持for each循环访问，因为我们还没有实现iterable接口，首先在ArrayMap中实现Iterable接口：

```java
public class ArrayMap<K, V> implements Iterable<K> {

    private K[] keys;
    private V[] values;
    int size;

    public ArrayMap() {
        keys = (K[]) new Object[100];
        values = (V[]) new Object[100];
        size = 0;
    }
  ....
}
```

然后重写iterator()方法，并在其中返回我们自己的迭代器对象(iterator)

```java
    @Override
    public Iterator<K> iterator() {
        return new KeyIterator();
    }
```

注意我们自定义的KeyIterator类必须要实现Iterator接口，否则在iterator()方法中返回的类型不匹配。

## 总结与感想

（1）学会深入思考，一点点抽丝剥茧，多想想为什么这样实现，很多问题没有自己想象中的那么复杂。

（2）遇到疑惑不放弃，这是提升自己最好的机会，遇到某个疑难的点，解决的过程中会挖掘出很多相关东西。

# ArrayList的扩容机制

这里结合源码分析一下ArrayList的扩容机制，基于JDK8 [参考文章，点击这里](https://github.com/Snailclimb/JavaGuide/blob/master/docs/java/collection/ArrayList-Grow.md)
首先分析ArrayList源码中的属性

```java
 /**
     * 默认初始容量大小
     */
    private static final int DEFAULT_CAPACITY = 10;   
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
/**
     * The array buffer into which the elements of the ArrayList are stored.
     * The capacity of the ArrayList is the length of this array buffer.
     * 【 Any empty ArrayList with elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA
     * will be expanded to DEFAULT_CAPACITY when the first element is added.】
     */
    transient Object[] elementData; // non-private to simplify nested class access
    /**
     *默认构造函数，使用初始容量10构造一个空列表(无参数构造)
     */
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }    

```

上述注释中有这样一段话

> Any empty ArrayList with elementData ==DEFAULTCAPACITY_EMPTY_ELEMENTDATA will be expanded to DEFAULT_CAPACITY when the first element is added.

意思是，**以无参数构造方法创建 ArrayList 时，实际上初始化赋值的是一个空数组。当真正对数组进行添加元素操作时，才真正分配容量。即向数组中添加第一个元素时，数组容量扩为10**
1）当我们向 ArrayList添加数据时，首先调用add()方法

```java
public boolean add(E e) {
        //添加元素之前，先调用ensureCapacityInternal方法
        ensureCapacityInternal(size + 1); // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
```

2）调用ensureCapacityInternal()，该方法是为了得到最小扩容量，比如说，默认参数是10，如果这时候只有一个参数，容量为1，就令minCapacity = 10，如果添加的元素大于10比如说11，则令minCapacity = 11

```java
 //得到最小扩容量，注意不是容量
    private void ensureCapacityInternal(int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
              // 获取默认的容量和传入参数的较大值
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
        }
        ensureExplicitCapacity(minCapacity);
    }
```

3）调用ensureExplicitCapacity()方法，判断是否需要扩容

```java
//判断是否需要扩容
    private void ensureExplicitCapacity(int minCapacity) {
        modCount++;
        // 如果最小扩容量 - 现有的容量 > 0，则需要扩容
        if (minCapacity - elementData.length > 0)
            //调用grow方法进行扩容，调用此方法代表已经开始扩容了
            grow(minCapacity);
    }
```

- 例如：当我们 add 第1个元素到 ArrayList 时，elementData.length = 0 ，因为执行了 ensureCapacityInternal() 方法 ，所以 minCapacity =10。此时，minCapacity - elementData.length > 0 成立，所以会进入 grow(minCapacity) 方法。
- 添加第2，3，4···到第10个元素时，不会执行grow方法，因为minCapacity - elementData.length < 0 ，数组容量都为10。
- 直到添加第11个元素，minCapacity(为11)比elementData.length（为10）要大。进入grow方法进行扩容。

4）调用grow()扩容方法
其中最重要的一句话是**int newCapacity = oldCapacity + (oldCapacity >> 1)**，所以ArrayList每次扩容后会是原来的1.5倍

```java
 private void grow(int minCapacity) {
        // oldCapacity为旧容量，newCapacity为新容量
        int oldCapacity = elementData.length;
        //将oldCapacity 右移一位，其效果相当于oldCapacity /2
        int newCapacity = oldCapacity + (oldCapacity >> 1);
       //然后检查新容量是否大于最小需要容量，若还是小于最小需要容量，那么就把最小需要容量当作数组的新容量，
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
// 如果新容量大于 MAX_ARRAY_SIZE,进入 hugeCapacity() 方法来比较 minCapacity 和 MAX_ARRAY_SIZE，
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```

5）如果扩容1.5倍仍不满足，调用hugeCapacity() 方法

```java
    //对minCapacity和MAX_ARRAY_SIZE进行比较
     //若minCapacity大，将Integer.MAX_VALUE（整型的最大值）作为新数组的大小
    //若MAX_ARRAY_SIZE大，将MAX_ARRAY_SIZE作为新数组的大小（MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8,这个在之前的定义里）
 private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }
```

画一个流程图总结一下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200207200248491.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FubmllMjAxNzk5,size_16,color_FFFFFF,t_70)

# ==和equals的区别

**一、java当中的数据类型和“==”的含义：**

- 基本数据类型（也称原始数据类型） ：byte,short,char,int,long,float,double,boolean。他们之间的比较，应用双等号（==）,比较的是他们的值。
- 引用数据类型：当他们用（==）进行比较的时候，比较的是他们在内存中的存放地址（确切的说，是**堆内存**地址）。

注：对于第二种类型，除非是同一个new出来的对象，他们的比较后的结果为true，否则比较后结果为false。因为每new一次，都会重新开辟堆内存空间。

 

**二、equals()方法介绍：**

JAVA当中所有的类都是继承于Object这个超类的，在Object类中定义了一个equals的方法，equals的源码是这样写的：

```java
public boolean equals(Object obj) {
    //this - s1
    //obj - s2
    return (this == obj);
}
```

可以看到，这个方法的初始默认行为是比较对象的内存地址值，一般来说，意义不大。所以，在一些类库当中这个方法被重写了，如String、Integer、Date。在这些类当中equals有其自身的实现（一般都是用来比较对象的成员变量值是否相同），而不再是比较类在堆内存中的存放地址了。
所以说，对于复合数据类型之间进行equals比较，在没有覆写equals方法的情况下，他们之间的比较还是内存中的存放位置的地址值，跟双等号（==）的结果相同；如果被复写，按照复写的要求来。

我们对上面的两段内容做个总结吧：

 **== 的作用：**
　　基本类型：比较的就是值是否相同
　　引用类型：比较的就是地址值是否相同
**equals 的作用:**
　　引用类型：默认情况下，比较的是地址值。
注：不过，我们可以根据情况自己重写该方法。一般重写都是自动生成，比较对象的成员变量值是否相同

# Java中HashMap讲解

Java为数据结构中的映射定义了一个接口java.util.Map，此接口主要有四个常用的实现类，分别是HashMap、Hashtable、LinkedHashMap和TreeMap，类继承关系如下图所示：

![img](https://pic2.zhimg.com/80/26341ef9fe5caf66ba0b7c40bba264a5_720w.png)

下面针对各个实现类的特点做一些说明：

(1) HashMap：它根据键的hashCode值存储数据，大多数情况下可以直接定位到它的值，因而具有很快的访问速度，但遍历顺序却是不确定的。 **HashMap最多只允许一条记录的键为null，允许多条记录的值为null。**HashMap非线程安全，即任一时刻可以有多个线程同时写HashMap，可能会导致数据的不一致。如果需要满足线程安全，可以用 Collections的synchronizedMap方法使HashMap具有线程安全的能力，或者使用ConcurrentHashMap。

(2) Hashtable：Hashtable是遗留类，很多映射的常用功能与HashMap类似，不同的是它承自Dictionary类，并且是线程安全的，任一时间只有一个线程能写Hashtable，并发性不如ConcurrentHashMap，因为ConcurrentHashMap引入了分段锁。Hashtable不建议在新代码中使用，不需要线程安全的场合可以用HashMap替换，需要线程安全的场合可以用ConcurrentHashMap替换。

(3) LinkedHashMap：LinkedHashMap是HashMap的一个子类，保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的，也可以在构造时带参数，按照访问次序排序。

(4) TreeMap：TreeMap实现SortedMap接口，能够把它保存的记录根据键排序，默认是按键值的升序排序，也可以指定排序的比较器，当用Iterator遍历TreeMap时，得到的记录是排过序的。如果使用排序的映射，建议使用TreeMap。在使用TreeMap时，key必须实现Comparable接口或者在构造TreeMap传入自定义的Comparator，否则会在运行时抛出java.lang.ClassCastException类型的异常。

对于上述四种Map类型的类，要求映射中的key是不可变对象。不可变对象是该对象在创建后它的哈希值不会被改变。如果对象的哈希值发生变化，Map对象很可能就定位不到映射的位置了。

通过上面的比较，我们知道了HashMap是Java的Map家族中一个普通成员，鉴于它可以满足大多数场景的使用条件，所以是使用频度最高的一个。下文我们主要结合源码，从存储结构、常用方法分析、扩容以及安全性等方面深入讲解HashMap的工作原理。

搞清楚HashMap，首先需要知道HashMap是什么，即它的存储结构-字段；其次弄明白它能干什么，即它的功能实现-方法。下面我们针对这两个方面详细展开讲解。

## 存储结构-字段

从结构实现来讲，HashMap是数组+链表+红黑树（JDK1.8增加了红黑树部分）实现的，如下如所示。

![img](https://pic1.zhimg.com/80/8db4a3bdfb238da1a1c4431d2b6e075c_720w.png)

这里需要讲明白两个问题：数据底层具体存储的是什么？这样的存储方式有什么优点呢？

(1) 从源码可知，HashMap类中有一个非常重要的字段，就是 Node[] table，即哈希桶数组，明显它是一个Node的数组。我们来看Node[JDK1.8]是何物。

```text
static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;    //用来定位数组索引位置
        final K key;
        V value;
        Node<K,V> next;   //链表的下一个node

        Node(int hash, K key, V value, Node<K,V> next) { ... }
        public final K getKey(){ ... }
        public final V getValue() { ... }
        public final String toString() { ... }
        public final int hashCode() { ... }
        public final V setValue(V newValue) { ... }
        public final boolean equals(Object o) { ... }
}
```

Node是HashMap的一个内部类，实现了Map.Entry接口，本质是就是一个映射(键值对)。上图中的每个黑色圆点就是一个Node对象。

(2) HashMap就是使用哈希表来存储的。哈希表为解决冲突，可以采用开放地址法和链地址法等来解决问题，Java中HashMap采用了链地址法。链地址法，简单来说，就是数组加链表的结合。在每个数组元素上都一个链表结构，当数据被Hash后，得到数组下标，把数据放在对应下标元素的链表上。例如程序执行下面代码：

```text
    map.put("美团","小美");
```

系统将调用"美团"这个key的hashCode()方法得到其hashCode 值（该方法适用于每个Java对象），然后再通过Hash算法的后两步运算（高位运算和取模运算，下文有介绍）来定位该键值对的存储位置，有时两个key会定位到相同的位置，表示发生了Hash碰撞。当然Hash算法计算结果越分散均匀，Hash碰撞的概率就越小，map的存取效率就会越高。

如果哈希桶数组很大，即使较差的Hash算法也会比较分散，如果哈希桶数组数组很小，即使好的Hash算法也会出现较多碰撞，所以就需要在空间成本和时间成本之间权衡，其实就是在根据实际情况确定哈希桶数组的大小，并在此基础上设计好的hash算法减少Hash碰撞。那么通过什么方式来控制map使得Hash碰撞的概率又小，哈希桶数组（Node[] table）占用空间又少呢？答案就是好的Hash算法和扩容机制。

在理解Hash和扩容流程之前，我们得先了解下HashMap的几个字段。从HashMap的默认构造函数源码可知，构造函数就是对下面几个字段进行初始化，源码如下：

```text
     int threshold;             // 所能容纳的key-value对极限 
     final float loadFactor;    // 负载因子
     int modCount;  
     int size;
```

首先，Node[] table的初始化长度length(默认值是16)，Load factor为负载因子(默认值是0.75)，threshold是HashMap所能容纳的最大数据量的Node(键值对)个数。threshold = length * Load factor。也就是说，在数组定义好长度之后，负载因子越大，所能容纳的键值对个数越多。

结合负载因子的定义公式可知，threshold就是在此Load factor和length(数组长度)对应下允许的最大元素数目，超过这个数目就重新resize(扩容)，扩容后的HashMap容量是之前容量的两倍。默认的负载因子0.75是对空间和时间效率的一个平衡选择，建议大家不要修改，除非在时间和空间比较特殊的情况下，如果内存空间很多而又对时间效率要求很高，可以降低负载因子Load factor的值；相反，如果内存空间紧张而对时间效率要求不高，可以增加负载因子loadFactor的值，这个值可以大于1。

size这个字段其实很好理解，就是HashMap中实际存在的键值对数量。注意和table的长度length、容纳最大键值对数量threshold的区别。而modCount字段主要用来记录HashMap内部结构发生变化的次数，主要用于迭代的快速失败。强调一点，内部结构发生变化指的是结构发生变化，例如put新键值对，但是某个key对应的value值被覆盖不属于结构变化。

在HashMap中，哈希桶数组table的长度length大小必须为2的n次方(一定是合数)，这是一种非常规的设计，常规的设计是把桶的大小设计为素数。相对来说素数导致冲突的概率要小于合数，具体证明可以参考[http://blog.csdn.net/liuqiyao_01/article/details/14475159](https://link.zhihu.com/?target=http%3A//blog.csdn.net/liuqiyao_01/article/details/14475159)，Hashtable初始化桶大小为11，就是桶大小设计为素数的应用（Hashtable扩容后不能保证还是素数）。HashMap采用这种非常规设计，主要是为了在取模和扩容时做优化，同时为了减少冲突，HashMap定位哈希桶索引位置时，也加入了高位参与运算的过程。

这里存在一个问题，即使负载因子和Hash算法设计的再合理，也免不了会出现拉链过长的情况，一旦出现拉链过长，则会严重影响HashMap的性能。于是，在JDK1.8版本中，对数据结构做了进一步的优化，引入了红黑树。而当链表长度太长（默认超过8）时，链表就转换为红黑树，利用红黑树快速增删改查的特点提高HashMap的性能，其中会用到红黑树的插入、删除、查找等算法。本文不再对红黑树展开讨论，想了解更多红黑树数据结构的工作原理可以参考[http://blog.csdn.net/v_july_v/article/details/6105630](https://link.zhihu.com/?target=http%3A//blog.csdn.net/v_july_v/article/details/6105630)。

## 功能实现-方法

HashMap的内部功能实现很多，本文主要从根据key获取哈希桶数组索引位置、put方法的详细执行、扩容过程三个具有代表性的点深入展开讲解。

### 1. 确定哈希桶数组索引位置

不管增加、删除、查找键值对，定位到哈希桶数组的位置都是很关键的第一步。前面说过HashMap的数据结构是数组和链表的结合，所以我们当然希望这个HashMap里面的元素位置尽量分布均匀些，尽量使得每个位置上的元素数量只有一个，那么当我们用hash算法求得这个位置的时候，马上就可以知道对应位置的元素就是我们要的，不用遍历链表，大大优化了查询的效率。HashMap定位数组索引位置，直接决定了hash方法的离散性能。先看看源码的实现(方法一+方法二):

```text
方法一：
static final int hash(Object key) {   //jdk1.8 & jdk1.7
     int h;
     // h = key.hashCode() 为第一步 取hashCode值
     // h ^ (h >>> 16)  为第二步 高位参与运算
     return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
方法二：
static int indexFor(int h, int length) {  //jdk1.7的源码，jdk1.8没有这个方法，但是实现原理一样的
     return h & (length-1);  //第三步 取模运算
}
```

这里的Hash算法本质上就是三步：**取key的hashCode值、高位运算、取模运算**。

对于任意给定的对象，只要它的hashCode()返回值相同，那么程序调用方法一所计算得到的Hash码值总是相同的。我们首先想到的就是把hash值对数组长度取模运算，这样一来，元素的分布相对来说是比较均匀的。但是，模运算的消耗还是比较大的，在HashMap中是这样做的：调用方法二来计算该对象应该保存在table数组的哪个索引处。

这个方法非常巧妙，它通过h & (table.length -1)来得到该对象的保存位，而HashMap底层数组的长度总是2的n次方，这是HashMap在速度上的优化。当length总是2的n次方时，h& (length-1)运算等价于对length取模，也就是h%length，但是&比%具有更高的效率。

在JDK1.8的实现中，优化了高位运算的算法，通过hashCode()的高16位异或低16位实现的：(h = k.hashCode()) ^ (h >>> 16)，主要是从速度、功效、质量来考虑的，这么做可以在数组table的length比较小的时候，也能保证考虑到高低Bit都参与到Hash的计算中，同时不会有太大的开销。

下面举例说明下，n为table的长度。

![img](https://pic2.zhimg.com/80/8e8203c1b51be6446cda4026eaaccf19_720w.png)

### 2. 分析HashMap的put方法

HashMap的put方法执行过程可以通过下图来理解，自己有兴趣可以去对比源码更清楚地研究学习。

![img](https://pic3.zhimg.com/80/58e67eae921e4b431782c07444af824e_720w.png)

①.判断键值对数组table[i]是否为空或为null，否则执行resize()进行扩容；

②.根据键值key计算hash值得到插入的数组索引i，如果table[i]==null，直接新建节点添加，转向⑥，如果table[i]不为空，转向③；

③.判断table[i]的首个元素是否和key一样，如果相同直接覆盖value，否则转向④，这里的相同指的是**hashCode以及equals；**

④.判断table[i] 是否为treeNode，即table[i] 是否是红黑树，如果是红黑树，则直接在树中插入键值对，否则转向⑤；

⑤.遍历table[i]，判断链表长度是否大于8，大于8的话把链表转换为红黑树，在红黑树中执行插入操作，否则进行链表的插入操作；遍历过程中若发现key已经存在直接覆盖value即可；

⑥.插入成功后，判断实际存在的键值对数量size是否超多了最大容量threshold，如果超过，进行扩容。

JDK1.8HashMap的put方法源码如下:

```text
 1 public V put(K key, V value) {
 2     // 对key的hashCode()做hash
 3     return putVal(hash(key), key, value, false, true);
 4 }
 5 
 6 final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
 7                boolean evict) {
 8     Node<K,V>[] tab; Node<K,V> p; int n, i;
 9     // 步骤①：tab为空则创建
10     if ((tab = table) == null || (n = tab.length) == 0)
11         n = (tab = resize()).length;
12     // 步骤②：计算index，并对null做处理 
13     if ((p = tab[i = (n - 1) & hash]) == null) 
14         tab[i] = newNode(hash, key, value, null);
15     else {
16         Node<K,V> e; K k;
17         // 步骤③：节点key存在，直接覆盖value
18         if (p.hash == hash &&
19             ((k = p.key) == key || (key != null && key.equals(k))))
20             e = p;
21         // 步骤④：判断该链为红黑树
22         else if (p instanceof TreeNode)
23             e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
24         // 步骤⑤：该链为链表
25         else {
26             for (int binCount = 0; ; ++binCount) {
27                 if ((e = p.next) == null) {
28                     p.next = newNode(hash, key,value,null);
                        //链表长度大于8转换为红黑树进行处理
29                     if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st  
30                         treeifyBin(tab, hash);
31                     break;
32                 }
                    // key已经存在直接覆盖value
33                 if (e.hash == hash &&
34                     ((k = e.key) == key || (key != null && key.equals(k))))                                          break;
36                 p = e;
37             }
38         }
39         
40         if (e != null) { // existing mapping for key
41             V oldValue = e.value;
42             if (!onlyIfAbsent || oldValue == null)
43                 e.value = value;
44             afterNodeAccess(e);
45             return oldValue;
46         }
47     }

48     ++modCount;
49     // 步骤⑥：超过最大容量 就扩容
50     if (++size > threshold)
51         resize();
52     afterNodeInsertion(evict);
53     return null;
54 }
```

### 3. 扩容机制

扩容(resize)就是重新计算容量，向HashMap对象里不停的添加元素，而HashMap对象内部的数组无法装载更多的元素时，对象就需要扩大数组的长度，以便能装入更多的元素。当然Java里的数组是无法自动扩容的，方法是使用一个新的数组代替已有的容量小的数组，就像我们用一个小桶装水，如果想装更多的水，就得换大水桶。

我们分析下resize的源码，鉴于JDK1.8融入了红黑树，较复杂，为了便于理解我们仍然使用JDK1.7的代码，好理解一些，本质上区别不大，具体区别后文再说。

```text
 1 void resize(int newCapacity) {   //传入新的容量
 2     Entry[] oldTable = table;    //引用扩容前的Entry数组
 3     int oldCapacity = oldTable.length;         
 4     if (oldCapacity == MAXIMUM_CAPACITY) {  //扩容前的数组大小如果已经达到最大(2^30)了
 5         threshold = Integer.MAX_VALUE; //修改阈值为int的最大值(2^31-1)，这样以后就不会扩容了
 6         return;
 7     }
 8  
 9     Entry[] newTable = new Entry[newCapacity];  //初始化一个新的Entry数组
10     transfer(newTable);                         //！！将数据转移到新的Entry数组里
11     table = newTable;                           //HashMap的table属性引用新的Entry数组
12     threshold = (int)(newCapacity * loadFactor);//修改阈值
13 }
```

这里就是使用一个容量更大的数组来代替已有的容量小的数组，transfer()方法将原有Entry数组的元素拷贝到新的Entry数组里。

```text
 1 void transfer(Entry[] newTable) {
 2     Entry[] src = table;                   //src引用了旧的Entry数组
 3     int newCapacity = newTable.length;
 4     for (int j = 0; j < src.length; j++) { //遍历旧的Entry数组
 5         Entry<K,V> e = src[j];             //取得旧Entry数组的每个元素
 6         if (e != null) {
 7             src[j] = null;//释放旧Entry数组的对象引用（for循环后，旧的Entry数组不再引用任何对象）
 8             do {
 9                 Entry<K,V> next = e.next;
10                 int i = indexFor(e.hash, newCapacity); //！！重新计算每个元素在数组中的位置
11                 e.next = newTable[i]; //标记[1]
12                 newTable[i] = e;      //将元素放在数组上
13                 e = next;             //访问下一个Entry链上的元素
14             } while (e != null);
15         }
16     }
17 }
```

newTable[i]的引用赋给了e.next，也就是使用了单链表的头插入方式，同一位置上新元素总会被放在链表的头部位置；这样先放在一个索引上的元素终会被放到Entry链的尾部(如果发生了hash冲突的话），这一点和Jdk1.8有区别，下文详解。在旧数组中同一条Entry链上的元素，通过重新计算索引位置后，有可能被放到了新数组的不同位置上。

下面举个例子说明下扩容过程。假设了我们的hash算法就是简单的用key mod 一下表的大小（也就是数组的长度）。其中的哈希桶数组table的size=2， 所以key = 3、7、5，put顺序依次为 5、7、3。在mod 2以后都冲突在table[1]这里了。这里假设负载因子 loadFactor=1，即当键值对的实际大小size 大于 table的实际大小时进行扩容。接下来的三个步骤是哈希桶数组 resize成4，然后所有的Node重新rehash的过程。 

![img](https://pic1.zhimg.com/80/e5aa99e811d1814e010afa7779b759d4_720w.png)

下面我们讲解下JDK1.8做了哪些优化。经过观测可以发现，我们使用的是2次幂的扩展(指长度扩为原来2倍)，所以，元素的位置要么是在原位置，要么是在原位置再移动2次幂的位置。看下图可以明白这句话的意思，n为table的长度，图（a）表示扩容前的key1和key2两种key确定索引位置的示例，图（b）表示扩容后key1和key2两种key确定索引位置的示例，其中hash1是key1对应的哈希与高位运算结果。

![img](https://pic2.zhimg.com/80/a285d9b2da279a18b052fe5eed69afe9_720w.png)

元素在重新计算hash之后，因为n变为2倍，那么n-1的mask范围在高位多1bit(红色)，因此新的index就会发生这样的变化：

![img](https://pic2.zhimg.com/80/b2cb057773e3d67976c535d6ef547d51_720w.png)

因此，我们在扩充HashMap的时候，不需要像JDK1.7的实现那样重新计算hash，只需要看看原来的hash值新增的那个bit是1还是0就好了，是0的话索引没变，是1的话索引变成“原索引+oldCap”，可以看看下图为16扩充为32的resize示意图：

![img](https://pic3.zhimg.com/80/544caeb82a329fa49cc99842818ed1ba_720w.png)

这个设计确实非常的巧妙，既省去了重新计算hash值的时间，而且同时，由于新增的1bit是0还是1可以认为是随机的，因此resize的过程，均匀的把之前的冲突的节点分散到新的bucket了。这一块就是JDK1.8新增的优化点。有一点注意区别，JDK1.7中rehash的时候，旧链表迁移新链表的时候，如果在新表的数组索引位置相同，则链表元素会倒置，但是从上图可以看出，JDK1.8不会倒置。有兴趣的同学可以研究下JDK1.8的resize源码，写的很赞，如下:

```text
 1 final Node<K,V>[] resize() {
 2     Node<K,V>[] oldTab = table;
 3     int oldCap = (oldTab == null) ? 0 : oldTab.length;
 4     int oldThr = threshold;
 5     int newCap, newThr = 0;
 6     if (oldCap > 0) {
 7         // 超过最大值就不再扩充了，就只好随你碰撞去吧
 8         if (oldCap >= MAXIMUM_CAPACITY) {
 9             threshold = Integer.MAX_VALUE;
10             return oldTab;
11         }
12         // 没超过最大值，就扩充为原来的2倍
13         else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
14                  oldCap >= DEFAULT_INITIAL_CAPACITY)
15             newThr = oldThr << 1; // double threshold
16     }
17     else if (oldThr > 0) // initial capacity was placed in threshold
18         newCap = oldThr;
19     else {               // zero initial threshold signifies using defaults
20         newCap = DEFAULT_INITIAL_CAPACITY;
21         newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
22     }
23     // 计算新的resize上限
24     if (newThr == 0) {
25 
26         float ft = (float)newCap * loadFactor;
27         newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
28                   (int)ft : Integer.MAX_VALUE);
29     }
30     threshold = newThr;
31     @SuppressWarnings({"rawtypes"，"unchecked"})
32         Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
33     table = newTab;
34     if (oldTab != null) {
35         // 把每个bucket都移动到新的buckets中
36         for (int j = 0; j < oldCap; ++j) {
37             Node<K,V> e;
38             if ((e = oldTab[j]) != null) {
39                 oldTab[j] = null;
40                 if (e.next == null)
41                     newTab[e.hash & (newCap - 1)] = e;
42                 else if (e instanceof TreeNode)
43                     ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
44                 else { // 链表优化重hash的代码块
45                     Node<K,V> loHead = null, loTail = null;
46                     Node<K,V> hiHead = null, hiTail = null;
47                     Node<K,V> next;
48                     do {
49                         next = e.next;
50                         // 原索引
51                         if ((e.hash & oldCap) == 0) {
52                             if (loTail == null)
53                                 loHead = e;
54                             else
55                                 loTail.next = e;
56                             loTail = e;
57                         }
58                         // 原索引+oldCap
59                         else {
60                             if (hiTail == null)
61                                 hiHead = e;
62                             else
63                                 hiTail.next = e;
64                             hiTail = e;
65                         }
66                     } while ((e = next) != null);
67                     // 原索引放到bucket里
68                     if (loTail != null) {
69                         loTail.next = null;
70                         newTab[j] = loHead;
71                     }
72                     // 原索引+oldCap放到bucket里
73                     if (hiTail != null) {
74                         hiTail.next = null;
75                         newTab[j + oldCap] = hiHead;
76                     }
77                 }
78             }
79         }
80     }
81     return newTab;
82 }
```

# **线程安全性**

在多线程使用场景中，应该尽量避免使用线程不安全的HashMap，而使用线程安全的ConcurrentHashMap。那么为什么说HashMap是线程不安全的，下面举例子说明在并发的多线程使用场景中使用HashMap可能造成死循环。代码例子如下(便于理解，仍然使用JDK1.7的环境)：

```text
public class HashMapInfiniteLoop {  

    private static HashMap<Integer,String> map = new HashMap<Integer,String>(2，0.75f);  
    public static void main(String[] args) {  
        map.put(5， "C");  

        new Thread("Thread1") {  
            public void run() {  
                map.put(7, "B");  
                System.out.println(map);  
            };  
        }.start();  
        new Thread("Thread2") {  
            public void run() {  
                map.put(3, "A);  
                System.out.println(map);  
            };  
        }.start();        
    }  
}
```

其中，map初始化为一个长度为2的数组，loadFactor=0.75，threshold=2*0.75=1，也就是说当put第二个key的时候，map就需要进行resize。

通过设置断点让线程1和线程2同时debug到transfer方法(3.3小节代码块)的首行。注意此时两个线程已经成功添加数据。放开thread1的断点至transfer方法的“Entry next = e.next;” 这一行；然后放开线程2的的断点，让线程2进行resize。结果如下图。

![img](https://pic4.zhimg.com/80/fa10635a66de637fe3cbd894882ff0c7_720w.png)

注意，Thread1的 e 指向了key(3)，而next指向了key(7)，其在线程二rehash后，指向了线程二重组后的链表。

线程一被调度回来执行，先是执行 newTalbe[i] = e， 然后是e = next，导致了e指向了key(7)，而下一次循环的next = e.next导致了next指向了key(3)。

![img](https://pic4.zhimg.com/80/d39d7eff6e8e04f98f5b53bebe2d4d7f_720w.png)

e.next = newTable[i] 导致 key(3).next 指向了 key(7)。注意：此时的key(7).next 已经指向了key(3)， 环形链表就这样出现了。

![img](https://pic2.zhimg.com/80/5f3cf5300f041c771a736b40590fd7b1_720w.png)

于是，当我们用线程一调用map.get(11)时，悲剧就出现了——Infinite Loop。

# HashMap多线程使用时resize()发生死循环

假设HashMap初始化大小为4，插入个3节点，不巧的是，这3个节点都hash到同一个位置，如果按照默认的负载因子的话，插入第3个节点就会扩容，为了验证效果，假设负载因子是1.

```
void transfer(Entry[] newTable, boolean rehash) {
    int newCapacity = newTable.length;
    for (Entry<K,V> e : table) {
        while(null != e) {
            Entry<K,V> next = e.next;
            if (rehash) {
                e.hash = null == e.key ? 0 : hash(e.key);
            }
            int i = indexFor(e.hash, newCapacity);
            e.next = newTable[i];
            newTable[i] = e;
            e = next;
        }
    }
}
复制代码
```

以上是节点移动的相关逻辑。



![img](https://user-gold-cdn.xitu.io/2018/1/23/16120e3142a82124?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



插入第4个节点时，发生rehash，假设现在有两个线程同时进行，线程1和线程2，两个线程都会新建新的数组。



![img](https://user-gold-cdn.xitu.io/2018/1/23/16120e3142b81091?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



假设 **线程2** 在执行到`Entry<K,V> next = e.next;`之后，cpu时间片用完了，这时变量e指向节点a，变量next指向节点b。

**线程1**继续执行，很不巧，a、b、c节点rehash之后又是在同一个位置7，开始移动节点

第一步，移动节点a



![img](https://user-gold-cdn.xitu.io/2018/1/23/16120e31431ba11a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



第二步，移动节点b



![img](https://user-gold-cdn.xitu.io/2018/1/23/16120e3144660942?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



注意，这里的顺序是反过来的，继续移动节点c



![img](https://user-gold-cdn.xitu.io/2018/1/23/16120e3144846033?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



这个时候 **线程1** 的时间片用完，内部的table还没有设置成新的newTable， **线程2** 开始执行，这时内部的引用关系如下：



![img](https://user-gold-cdn.xitu.io/2018/1/23/16120e3144bf6189?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



这时，在 **线程2** 中，变量e指向节点a，变量next指向节点b，开始执行循环体的剩余逻辑。

```
Entry<K,V> next = e.next;
int i = indexFor(e.hash, newCapacity);
e.next = newTable[i];
newTable[i] = e;
e = next;
复制代码
```

执行之后的引用关系如下图



![img](https://user-gold-cdn.xitu.io/2018/1/23/16120e316b5e168a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



执行后，变量e指向节点b，因为e不是null，则继续执行循环体，执行后的引用关系



![img](https://user-gold-cdn.xitu.io/2018/1/23/16120e316ccca72d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



变量e又重新指回节点a，只能继续执行循环体，这里仔细分析下： 1、执行完`Entry<K,V> next = e.next;`，目前节点a没有next，所以变量next指向null； 2、`e.next = newTable[i];` 其中 newTable[i] 指向节点b，那就是把a的next指向了节点b，这样a和b就相互引用了，形成了一个环； 3、`newTable[i] = e` 把节点a放到了数组i位置； 4、`e = next;` 把变量e赋值为null，因为第一步中变量next就是指向null；

所以最终的引用关系是这样的：



![img](https://user-gold-cdn.xitu.io/2018/1/23/16120e316d17308b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



节点a和b互相引用，形成了一个环，当在数组该位置get寻找对应的key时，就发生了死循环。

另外，如果线程2把newTable设置成到内部的table，节点c的数据就丢了，看来还有数据遗失的问题。

# Comparable和Comparator的区别

Java 中为我们提供了两种比较机制：Comparable 和 Comparator，他们之间有什么区别呢？今天来了解一下。

## Comparable 自然排序

Comparable 在 java.lang 包下，是一个接口，内部只有一个方法 compareTo()：

```
public interface Comparable<T> {
    public int compareTo(T o);
}
123
```

Comparable 可以让实现它的类的对象进行比较，具体的比较规则是按照 compareTo 方法中的规则进行。这种顺序称为 **自然顺序**。

compareTo 方法的返回值有三种情况：

- e1.compareTo(e2) > 0 即 e1 > e2
- e1.compareTo(e2) = 0 即 e1 = e2
- e1.compareTo(e2) < 0 即 e1 < e2

注意：

> 1.由于 null 不是一个类，也不是一个对象，因此在重写 compareTo 方法时应该注意 e.compareTo(null) 的情况，即使 e.equals(null) 返回 false，compareTo 方法也应该主动抛出一个空指针异常 NullPointerException。
>
> 2.Comparable 实现类重写 compareTo 方法时一般要求 e1.compareTo(e2) == 0 的结果要和 e1.equals(e2) 一致。这样将来使用 SortedSet 等**根据类的自然排序进行排序**的集合容器时可以保证保存的数据的顺序和想象中一致。

有人可能好奇上面的第二点如果违反了会怎样呢？

举个例子，如果你往一个 SortedSet 中先后添加两个对象 a 和 b，a b 满足 (!a.equals(b) && a.compareTo(b) == 0)，同时也没有另外指定个 Comparator，那当你添加完 a 再添加 b 时会添加失败返回 false, SortedSet 的 size 也不会增加，因为在 SortedSet 看来它们是相同的，而 SortedSet 中是不允许重复的。

实际上所有实现了 Comparable 接口的 Java 核心类的结果都和 equlas 方法保持一致。
实现了 Comparable 接口的 List 或则数组可以使用 `Collections.sort()` 或者 `Arrays.sort()` 方法进行排序。

实现了 Comparable 接口的对象才能够直接被用作 SortedMap (SortedSet) 的 key，要不然得在外边指定 Comparator 排序规则。

因此自己定义的类如果想要使用有序的集合类，需要实现 Comparable 接口，比如：

```
**
 * description: 测试用的实体类 书, 实现了 Comparable 接口，自然排序
 * <br/>
 * author: shixinzhang
 * <br/>
 * data: 10/5/2016
 */
public class BookBean implements Serializable, Comparable {
    private String name;
    private int count;


    public BookBean(String name, int count) {
        this.name = name;
        this.count = count;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getCount() {
        return count;
    }

    public void setCount(int count) {
        this.count = count;
    }

    /**
     * 重写 equals
     * @param o
     * @return
     */
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof BookBean)) return false;

        BookBean bean = (BookBean) o;

        if (getCount() != bean.getCount()) return false;
        return getName().equals(bean.getName());

    }

    /**
     * 重写 hashCode 的计算方法
     * 根据所有属性进行 迭代计算，避免重复
     * 计算 hashCode 时 计算因子 31 见得很多，是一个质数，不能再被除
     * @return
     */
    @Override
    public int hashCode() {
        //调用 String 的 hashCode(), 唯一表示一个字符串内容
        int result = getName().hashCode();
        //乘以 31, 再加上 count
        result = 31 * result + getCount();
        return result;
    }

    @Override
    public String toString() {
        return "BookBean{" +
                "name='" + name + '\'' +
                ", count=" + count +
                '}';
    }

    /**
     * 当向 TreeSet 中添加 BookBean 时，会调用这个方法进行排序
     * @param another
     * @return
     */
    @Override
    public int compareTo(Object another) {
        if (another instanceof BookBean){
            BookBean anotherBook = (BookBean) another;
            int result;

            //比如这里按照书价排序
            result = getCount() - anotherBook.getCount();     

          //或者按照 String 的比较顺序
          //result = getName().compareTo(anotherBook.getName());

            if (result == 0){   //当书价一致时，再对比书名。 保证所有属性比较一遍
                result = getName().compareTo(anotherBook.getName());
            }
            return result;
        }
        // 一样就返回 0
        return 0;
    }
```

上述代码还重写了 equlas(), hashCode() 方法，自定义的类将来可能会进行比较时，建议重写这些方法。

感谢 @[li1019865596](http://blog.csdn.net/li1019865596) 指出，这里我想表达的是在有些场景下 equals 和 compareTo 结果要保持一致，这时候不重写 equals，使用 Object.equals 方法得到的结果会有问题，比如说 HashMap.put() 方法，会先调用 key 的 equals 方法进行比较，然后才调用 compareTo。

**后面重写 compareTo 时，要判断某个相同时对比下一个属性，把所有属性都比较一次。**

Comparable 接口属于 Java 集合框架的一部分。

## Comparator 定制排序

Comparator 在 java.util 包下，也是一个接口，JDK 1.8 以前只有两个方法：

```
public interface Comparator<T> {

    public int compare(T lhs, T rhs);

    public boolean equals(Object object);
}
```

JDK 1.8 以后又新增了很多方法：

![shixinzhang](https://img-blog.csdn.net/20161129194956317)

基本上都是跟 Function 相关的，这里暂不介绍 1.8 新增的。

从上面内容可知使用自然排序需要类实现 Comparable，并且在内部重写 comparaTo 方法。

而 Comparator 则是在外部制定排序规则，然后作为排序策略参数传递给某些类，比如 Collections.sort(), Arrays.sort(), 或者一些内部有序的集合（比如 SortedSet，SortedMap 等）。

使用方式主要分三步：

1. 创建一个 Comparator 接口的实现类，并赋值给一个对象
   - 在 compare 方法中针对自定义类写排序规则
2. 将 Comparator 对象作为参数传递给 排序类的某个方法
3. 向排序类中添加 compare 方法中使用的自定义类

举个例子：

```
        // 1.创建一个实现 Comparator 接口的对象
        Comparator comparator = new Comparator() {
            @Override
            public int compare(Object object1, Object object2) {
                if (object1 instanceof NewBookBean && object2 instanceof NewBookBean){
                    NewBookBean newBookBean = (NewBookBean) object1;
                    NewBookBean newBookBean1 = (NewBookBean) object2;
                    //具体比较方法参照 自然排序的 compareTo 方法，这里只举个栗子
                    return newBookBean.getCount() - newBookBean1.getCount();
                }
                return 0;
            }
        };

        //2.将此对象作为形参传递给 TreeSet 的构造器中
        TreeSet treeSet = new TreeSet(comparator);

        //3.向 TreeSet 中添加 步骤 1 中 compare 方法中设计的类的对象
        treeSet.add(new NewBookBean("A",34));
        treeSet.add(new NewBookBean("S",1));
        treeSet.add( new NewBookBean("V",46));
        treeSet.add( new NewBookBean("Q",26));
```

其实可以看到，Comparator 的使用是一种策略模式，不熟悉策略模式的同学可以[点这里查看： 策略模式：网络小说的固定套路](http://blog.csdn.net/u011240877/article/details/52346671) 了解。

排序类中持有一个 Comparator 接口的引用：

```
Comparator<? super K> comparator;
1
```

而我们可以传入各种自定义排序规则的 Comparator 实现类，对同样的类制定不同的排序策略。

## 总结

Java 中的两种排序方式：

1. Comparable 自然排序。（实体类实现）
2. Comparator 是定制排序。（无法修改实体类时，直接在调用方创建）

**同时存在时采用 Comparator（定制排序）的规则进行比较。**

对于一些普通的数据类型（比如 String, Integer, Double…），它们默认实现了Comparable 接口，实现了 compareTo 方法，我们可以直接使用。

而对于一些自定义类，它们可能在不同情况下需要实现不同的比较策略，我们可以新创建 Comparator 接口，然后使用特定的 Comparator 实现进行比较。

这就是 Comparable 和 Comparator 的区别。

# Java内存常见面试题

## 1 概述

对于 Java 程序员来说，在虚拟机自动内存管理机制下，不再需要像C/C++程序开发程序员这样为内一个 new 操作去写对应的 delete/free 操作，不容易出现内存泄漏和内存溢出问题。正是因为 Java 程序员把内存控制权利交给 Java 虚拟机，一旦出现内存泄漏和溢出方面的问题，如果不了解虚拟机是怎样使用内存的，那么排查错误将会是一个非常艰巨的任务。

## 2 运行时数据区域

Java 虚拟机在执行 Java 程序的过程中会把它管理的内存划分成若干个不同的数据区域。 [![img](https://camo.githubusercontent.com/f85d2760d64d5828dca7bd8ee4f8745c5055aa3f8511b7f2d19a532384b9beb3/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d332f4a564d2545382542462539302545382541312538432545362539372542362545362539352542302545362538442541452545352538432542412545352539462539462e706e67)](https://camo.githubusercontent.com/f85d2760d64d5828dca7bd8ee4f8745c5055aa3f8511b7f2d19a532384b9beb3/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d332f4a564d2545382542462539302545382541312538432545362539372542362545362539352542302545362538442541452545352538432542412545352539462539462e706e67)

这些组成部分一些是线程私有的，其他的则是线程共享的。

**线程私有的：**

- 程序计数器
- 虚拟机栈
- 本地方法栈

**线程共享的：**

- 堆
- 方法区
- 直接内存

### 2.1 程序计数器

程序计数器是一块较小的内存空间，可以看作是当前线程所执行的字节码的行号指示器。**字节码解释器工作时通过改变这个计数器的值来选取下一条需要执行的字节码指令，分支、循环、跳转、异常处理、线程恢复等功能都需要依赖这个计数器来完。**

另外，**为了线程切换后能恢复到正确的执行位置，每条线程都需要有一个独立的程序计数器，各线程之间计数器互不影响，独立存储，我们称这类内存区域为“线程私有”的内存。**

**从上面的介绍中我们知道程序计数器主要有两个作用：**

1. 字节码解释器通过改变程序计数器来依次读取指令，从而实现代码的流程控制，如：顺序执行、选择、循环、异常处理。
2. 在多线程的情况下，程序计数器用于记录当前线程执行的位置，从而当线程被切换回来的时候能够知道该线程上次运行到哪儿了。

**注意：程序计数器是唯一一个不会出现OutOfMemoryError的内存区域，它的生命周期随着线程的创建而创建，随着线程的结束而死亡。**

### 2.2 Java 虚拟机栈

**与程序计数器一样，Java虚拟机栈也是线程私有的，它的生命周期和线程相同，描述的是 Java 方法执行的内存模型。**

**Java 内存可以粗糙的区分为堆内存（Heap）和栈内存(Stack),其中栈就是现在说的虚拟机栈，或者说是虚拟机栈中局部变量表部分。** （实际上，Java虚拟机栈是由一个个栈帧组成，而每个栈帧中都拥有：局部变量表、操作数栈、动态链接、方法出口信息。）

**局部变量表主要存放了编译器可知的各种数据类型**（boolean、byte、char、short、int、float、long、double）、**对象引用**（reference类型，它不同于对象本身，可能是一个指向对象起始地址的引用指针，也可能是指向一个代表对象的句柄或其他与此对象相关的位置）。

**Java 虚拟机栈会出现两种异常：StackOverFlowError 和 OutOfMemoryError。**

- **StackOverFlowError：** 若Java虚拟机栈的内存大小不允许动态扩展，那么当线程请求栈的深度超过当前Java虚拟机栈的最大深度的时候，就抛出StackOverFlowError异常。
- **OutOfMemoryError：** 若 Java 虚拟机栈的内存大小允许动态扩展，且当线程请求栈时内存用完了，无法再动态扩展了，此时抛出OutOfMemoryError异常。

Java 虚拟机栈也是线程私有的，每个线程都有各自的Java虚拟机栈，而且随着线程的创建而创建，随着线程的死亡而死亡。

### 2.3 本地方法栈

和虚拟机栈所发挥的作用非常相似，区别是： **虚拟机栈为虚拟机执行 Java 方法 （也就是字节码）服务，而本地方法栈则为虚拟机使用到的 Native 方法服务。** 在 HotSpot 虚拟机中和 Java 虚拟机栈合二为一。

本地方法被执行的时候，在本地方法栈也会创建一个栈帧，用于存放该本地方法的局部变量表、操作数栈、动态链接、出口信息。

方法执行完毕后相应的栈帧也会出栈并释放内存空间，也会出现 StackOverFlowError 和 OutOfMemoryError 两种异常。

### 2.4 堆

Java 虚拟机所管理的内存中最大的一块，Java 堆是所有线程共享的一块内存区域，在虚拟机启动时创建。**此内存区域的唯一目的就是存放对象实例，几乎所有的对象实例以及数组都在这里分配内存。**

Java 堆是垃圾收集器管理的主要区域，因此也被称作**GC堆（Garbage Collected Heap）**.从垃圾回收的角度，由于现在收集器基本都采用分代垃圾收集算法，所以Java堆还可以细分为：新生代和老年代：再细致一点有：Eden空间、From Survivor、To Survivor空间等。**进一步划分的目的是更好地回收内存，或者更快地分配内存。**

[![img](https://camo.githubusercontent.com/4cfa98917a27659dc6b4753cd6e20135cb1444a951a05703a28d4ff2d1aca81a/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f382f32352f313635373033343461323963333433333f773d35393926683d32353026663d706e6726733d38393436)](https://camo.githubusercontent.com/4cfa98917a27659dc6b4753cd6e20135cb1444a951a05703a28d4ff2d1aca81a/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f382f32352f313635373033343461323963333433333f773d35393926683d32353026663d706e6726733d38393436)

**在 JDK 1.8中移除整个永久代，取而代之的是一个叫元空间（Metaspace）的区域（永久代使用的是JVM的堆内存空间，而元空间使用的是物理内存，直接受到本机的物理内存限制）。**

推荐阅读：

- 《Java8内存模型—永久代(PermGen)和元空间(Metaspace)》：http://www.cnblogs.com/paddix/p/5309550.html

### 2.5 方法区

**方法区与 Java 堆一样，是各个线程共享的内存区域，它用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。虽然Java虚拟机规范把方法区描述为堆的一个逻辑部分，但是它却有一个别名叫做 Non-Heap（非堆），目的应该是与 Java 堆区分开来。**

HotSpot 虚拟机中方法区也常被称为 **“永久代”**，本质上两者并不等价。仅仅是因为 HotSpot 虚拟机设计团队用永久代来实现方法区而已，这样 HotSpot 虚拟机的垃圾收集器就可以像管理 Java 堆一样管理这部分内存了。但是这并不是一个好主意，因为这样更容易遇到内存溢出问题。

**相对而言，垃圾收集行为在这个区域是比较少出现的，但并非数据进入方法区后就“永久存在”了。**

### 2.6 运行时常量池

运行时常量池是方法区的一部分。Class 文件中除了有类的版本、字段、方法、接口等描述信息外，还有常量池信息（用于存放编译期生成的各种字面量和符号引用）

既然运行时常量池时方法区的一部分，自然受到方法区内存的限制，当常量池无法再申请到内存时会抛出 OutOfMemoryError 异常。

**JDK1.7及之后版本的 JVM 已经将运行时常量池从方法区中移了出来，在 Java 堆（Heap）中开辟了一块区域存放运行时常量池。**

[![img](https://camo.githubusercontent.com/0f899d9813fbf1b8fc74724089a917ff7bdd93d60e8c4f685f0e850b51969154/687474703a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f31382d392d31342f32363033383433332e6a7067)](https://camo.githubusercontent.com/0f899d9813fbf1b8fc74724089a917ff7bdd93d60e8c4f685f0e850b51969154/687474703a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f31382d392d31342f32363033383433332e6a7067) ——图片来源：https://blog.csdn.net/wangbiao007/article/details/78545189

推荐阅读：

- 《Java 中几种常量池的区分》： https://blog.csdn.net/qq_26222859/article/details/73135660

### 2.7 直接内存

直接内存并不是虚拟机运行时数据区的一部分，也不是虚拟机规范中定义的内存区域，但是这部分内存也被频繁地使用。而且也可能导致 OutOfMemoryError 异常出现。

JDK1.4中新加入的 **NIO(New Input/Output) 类**，引入了一种基于**通道（Channel）** 与**缓存区（Buffer）** 的 I/O 方式，它可以直接使用Native函数库直接分配堆外内存，然后通过一个存储在 Java 堆中的 DirectByteBuffer 对象作为这块内存的引用进行操作。这样就能在一些场景中显著提高性能，因为**避免了在 Java 堆和 Native 堆之间来回复制数据**。

本机直接内存的分配不会收到 Java 堆的限制，但是，既然是内存就会受到本机总内存大小以及处理器寻址空间的限制。

## 3 HotSpot 虚拟机对象探秘

通过上面的介绍我们大概知道了虚拟机的内存情况，下面我们来详细的了解一下 HotSpot 虚拟机在 Java 堆中对象分配、布局和访问的全过程。

### 3.1 对象的创建

下图便是 Java 对象的创建过程，我建议最好是能默写出来，并且要掌握每一步在做什么。 [![Java对象的创建过程](https://camo.githubusercontent.com/f223f2f3ab0f1f6ecdbadf06dbc8b0692e0c9625d83a78c59597704f6282fb68/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f382f32322f313635363165353961343133353836393f773d39353026683d32373926663d706e6726733d3238353239)](https://camo.githubusercontent.com/f223f2f3ab0f1f6ecdbadf06dbc8b0692e0c9625d83a78c59597704f6282fb68/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f382f32322f313635363165353961343133353836393f773d39353026683d32373926663d706e6726733d3238353239)

**①类加载检查：** 虚拟机遇到一条 new 指令时，首先将去检查这个指令的参数是否能在常量池中定位到这个类的符号引用，并且检查这个符号引用代表的类是否已被加载过、解析和初始化过。如果没有，那必须先执行相应的类加载过程。

**②分配内存：** 在**类加载检查**通过后，接下来虚拟机将为新生对象**分配内存**。对象所需的内存大小在类加载完成后便可确定，为对象分配空间的任务等同于把一块确定大小的内存从 Java 堆中划分出来。**分配方式**有 **“指针碰撞”** 和 **“空闲列表”** 两种，**选择那种分配方式由 Java 堆是否规整决定，而Java堆是否规整又由所采用的垃圾收集器是否带有压缩整理功能决定**。

**内存分配的两种方式：（补充内容，需要掌握）**

选择以上两种方式中的哪一种，取决于 Java 堆内存是否规整。而 Java 堆内存是否规整，取决于 GC 收集器的算法是"标记-清除"，还是"标记-整理"（也称作"标记-压缩"），值得注意的是，复制算法内存也是规整的

[![img](https://camo.githubusercontent.com/adc1deb61d8307a66021809c7e39e9a4d89e4d9f0298e4a4da59010a744756c5/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f382f32322f313635363165353961343061326333643f773d3134323626683d33333326663d706e6726733d3236333436)](https://camo.githubusercontent.com/adc1deb61d8307a66021809c7e39e9a4d89e4d9f0298e4a4da59010a744756c5/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f382f32322f313635363165353961343061326333643f773d3134323626683d33333326663d706e6726733d3236333436)

**内存分配并发问题（补充内容，需要掌握）**

在创建对象的时候有一个很重要的问题，就是线程安全，因为在实际开发过程中，创建对象是很频繁的事情，作为虚拟机来说，必须要保证线程是安全的，通常来讲，虚拟机采用两种方式来保证线程安全：

- **CAS+失败重试：** CAS 是乐观锁的一种实现方式。所谓乐观锁就是，每次不加锁而是假设没有冲突而去完成某项操作，如果因为冲突失败就重试，直到成功为止。**虚拟机采用 CAS 配上失败重试的方式保证更新操作的原子性。**
- **TLAB：** 为每一个线程预先在Eden区分配一块儿内存，JVM在给线程中的对象分配内存时，首先在TLAB分配，当对象大于TLAB中的剩余内存或TLAB的内存已用尽时，再采用上述的CAS进行内存分配

**③初始化零值：** 内存分配完成后，虚拟机需要将分配到的内存空间都初始化为零值（不包括对象头），这一步操作保证了对象的实例字段在 Java 代码中可以不赋初始值就直接使用，程序能访问到这些字段的数据类型所对应的零值。

**④设置对象头：** 初始化零值完成之后，**虚拟机要对对象进行必要的设置**，例如这个对象是那个类的实例、如何才能找到类的元数据信息、对象的哈希吗、对象的 GC 分代年龄等信息。 **这些信息存放在对象头中。** 另外，根据虚拟机当前运行状态的不同，如是否启用偏向锁等，对象头会有不同的设置方式。

**⑤执行 init 方法：** 在上面工作都完成之后，从虚拟机的视角来看，一个新的对象已经产生了，但从 Java 程序的视角来看，对象创建才刚开始，`<init>` 方法还没有执行，所有的字段都还为零。所以一般来说，执行 new 指令之后会接着执行 `<init>` 方法，把对象按照程序员的意愿进行初始化，这样一个真正可用的对象才算完全产生出来。

### 3.2 对象的内存布局

在 Hotspot 虚拟机中，对象在内存中的布局可以分为3块区域：**对象头**、**实例数据**和**对齐填充**。

**Hotspot虚拟机的对象头包括两部分信息**，**第一部分用于存储对象自身的自身运行时数据**（哈希码、GC分代年龄、锁状态标志等等），**另一部分是类型指针**，即对象指向它的类元数据的指针，虚拟机通过这个指针来确定这个对象是那个类的实例。

**实例数据部分是对象真正存储的有效信息**，也是在程序中所定义的各种类型的字段内容。

**对齐填充部分不是必然存在的，也没有什么特别的含义，仅仅起占位作用。** 因为Hotspot虚拟机的自动内存管理系统要求对象起始地址必须是8字节的整数倍，换句话说就是对象的大小必须是8字节的整数倍。而对象头部分正好是8字节的倍数（1倍或2倍），因此，当对象实例数据部分没有对齐时，就需要通过对齐填充来补全。

### 3.3 对象的访问定位

建立对象就是为了使用对象，我们的Java程序通过栈上的 reference 数据来操作堆上的具体对象。对象的访问方式有虚拟机实现而定，目前主流的访问方式有**①使用句柄**和**②直接指针**两种：

1. **句柄：** 如果使用句柄的话，那么Java堆中将会划分出一块内存来作为句柄池，reference 中存储的就是对象的句柄地址，而句柄中包含了对象实例数据与类型数据各自的具体地址信息； [![使用句柄](https://camo.githubusercontent.com/0c2ec3deeeb289135b00768f269d886281ac45cb393a1b3f60ec0a5d58e5b358/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f342f32372f313633303662393537333936383934363f773d37383626683d33363226663d706e6726733d313039323031)](https://camo.githubusercontent.com/0c2ec3deeeb289135b00768f269d886281ac45cb393a1b3f60ec0a5d58e5b358/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f342f32372f313633303662393537333936383934363f773d37383626683d33363226663d706e6726733d313039323031)
2. **直接指针：** 如果使用直接指针访问，那么 Java 堆对象的布局中就必须考虑如何放置访问类型数据的相关信息，而reference 中存储的直接就是对象的地址。

[![使用直接指针](https://camo.githubusercontent.com/633f7706c4ea6f059345fa0fe1a067e6b7cc419b68b3d165482a0d522b1c8b3b/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f342f32372f313633303662613361343162366236353f773d37363626683d33353326663d706e6726733d3939313732)](https://camo.githubusercontent.com/633f7706c4ea6f059345fa0fe1a067e6b7cc419b68b3d165482a0d522b1c8b3b/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f342f32372f313633303662613361343162366236353f773d37363626683d33353326663d706e6726733d3939313732)

**这两种对象访问方式各有优势。使用句柄来访问的最大好处是 reference 中存储的是稳定的句柄地址，在对象被移动时只会改变句柄中的实例数据指针，而 reference 本身不需要修改。使用直接指针访问方式最大的好处就是速度快，它节省了一次指针定位的时间开销。**

## 四 重点补充内容

### String 类和常量池

**1 String 对象的两种创建方式：**

```
     String str1 = "abcd";
     String str2 = new String("abcd");
     System.out.println(str1==str2);//false
```

这两种不同的创建方法是有差别的，第一种方式是在常量池中拿对象，第二种方式是直接在堆内存空间创建一个新的对象。 [![img](https://camo.githubusercontent.com/0f815ad9004e7a91c5f76b2a2e4d34f0b3b36a289637a5bf8089049768c6e8de/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f382f32322f313635363165353961353963303837333f773d36393826683d33353526663d706e6726733d3130343439)](https://camo.githubusercontent.com/0f815ad9004e7a91c5f76b2a2e4d34f0b3b36a289637a5bf8089049768c6e8de/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f382f32322f313635363165353961353963303837333f773d36393826683d33353526663d706e6726733d3130343439) 记住：只要使用new方法，便需要创建新的对象。

**2 String 类型的常量池比较特殊。它的主要使用方法有两种：**

- 直接使用双引号声明出来的 String 对象会直接存储在常量池中。
- 如果不是用双引号声明的 String 对象，可以使用 String 提供的 intern 方法。String.intern() 是一个 Native 方法，它的作用是：如果运行时常量池中已经包含一个等于此 String 对象内容的字符串，则返回常量池中该字符串的引用；如果没有，则在常量池中创建与此 String 内容相同的字符串，并返回常量池中创建的字符串的引用。

```
	      String s1 = new String("计算机");
	      String s2 = s1.intern();
	      String s3 = "计算机";
	      System.out.println(s2);//计算机
	      System.out.println(s1 == s2);//false，因为一个是堆内存中的String对象一个是常量池中的String对象，
	      System.out.println(s3 == s2);//true，因为两个都是常量池中的String对象
```

**3 String 字符串拼接**

```
		  String str1 = "str";
		  String str2 = "ing";
		  
		  String str3 = "str" + "ing";//常量池中的对象
		  String str4 = str1 + str2; //在堆上创建的新的对象	  
		  String str5 = "string";//常量池中的对象
		  System.out.println(str3 == str4);//false
		  System.out.println(str3 == str5);//true
		  System.out.println(str4 == str5);//false
```

[![img](https://camo.githubusercontent.com/8e3c4e4b8ea9401ccc7600275e0ae520a5f56641fb23cf30c7afb392386652e4/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f382f32322f313635363165353961346431336639323f773d35393326683d36303326663d706e6726733d3232323635)](https://camo.githubusercontent.com/8e3c4e4b8ea9401ccc7600275e0ae520a5f56641fb23cf30c7afb392386652e4/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f382f32322f313635363165353961346431336639323f773d35393326683d36303326663d706e6726733d3232323635)

尽量避免多个字符串拼接，因为这样会重新创建对象。如果需要改变字符串的话，可以使用 StringBuilder 或者 StringBuffer。

### String s1 = new String("abc");这句话创建了几个对象？

**创建了两个对象。**

**验证：**

```
		String s1 = new String("abc");// 堆内存的地址值
		String s2 = "abc";
		System.out.println(s1 == s2);// 输出false,因为一个是堆内存，一个是常量池的内存，故两者是不同的。
		System.out.println(s1.equals(s2));// 输出true
```

**结果：**

```
false
true
```

**解释：**

先有字符串"abc"放入常量池，然后 new 了一份字符串"abc"放入Java堆(字符串常量"abc"在编译期就已经确定放入常量池，而 Java 堆上的"abc"是在运行期初始化阶段才确定)，然后 Java 栈的 str1 指向Java堆上的"abc"。

### 8种基本类型的包装类和常量池

- **Java 基本类型的包装类的大部分都实现了常量池技术，即Byte,Short,Integer,Long,Character,Boolean；这5种包装类默认创建了数值[-128，127]的相应类型的缓存数据，但是超出此范围仍然会去创建新的对象。**
- **两种浮点数类型的包装类 Float,Double 并没有实现常量池技术。**

```
		Integer i1 = 33;
		Integer i2 = 33;
		System.out.println(i1 == i2);// 输出true
		Integer i11 = 333;
		Integer i22 = 333;
		System.out.println(i11 == i22);// 输出false
		Double i3 = 1.2;
		Double i4 = 1.2;
		System.out.println(i3 == i4);// 输出false
```

**Integer 缓存源代码：**

```
/**
*此方法将始终缓存-128到127（包括端点）范围内的值，并可以缓存此范围之外的其他值。
*/
    public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
```

**应用场景：**

1. Integer i1=40；Java 在编译的时候会直接将代码封装成Integer i1=Integer.valueOf(40);，从而使用常量池中的对象。
2. Integer i1 = new Integer(40);这种情况下会创建新的对象。

```
  Integer i1 = 40;
  Integer i2 = new Integer(40);
  System.out.println(i1==i2);//输出false
```

**Integer比较更丰富的一个例子:**

```
  Integer i1 = 40;
  Integer i2 = 40;
  Integer i3 = 0;
  Integer i4 = new Integer(40);
  Integer i5 = new Integer(40);
  Integer i6 = new Integer(0);
  
  System.out.println("i1=i2   " + (i1 == i2));
  System.out.println("i1=i2+i3   " + (i1 == i2 + i3));
  System.out.println("i1=i4   " + (i1 == i4));
  System.out.println("i4=i5   " + (i4 == i5));
  System.out.println("i4=i5+i6   " + (i4 == i5 + i6));   
  System.out.println("40=i5+i6   " + (40 == i5 + i6));     
```

结果：

```
i1=i2   true
i1=i2+i3   true
i1=i4   false
i4=i5   false
i4=i5+i6   true
40=i5+i6   true
```

解释：

语句i4 == i5 + i6，因为+这个操作符不适用于Integer对象，首先i5和i6进行自动拆箱操作，进行数值相加，即i4 == 40。然后Integer对象无法与数值进行直接比较，所以i4自动拆箱转为int值40，最终这条语句转为40 == 40进行数值比较。

# Java匿名内部类

匿名内部类也就是没有名字的内部类

正因为没有名字，所以匿名内部类只能使用一次，它通常用来简化代码编写

但使用匿名内部类还有个前提条件：必须继承一个父类或实现一个接口

#### 实例1:不使用匿名内部类来实现抽象方法

```java
abstract class Person {
    public abstract void eat();
}
 
class Child extends Person {
    public void eat() {
        System.out.println("eat something");
    }
}
 
public class Demo {
    public static void main(String[] args) {
        Person p = new Child();
        p.eat();
    }
}
```

**运行结果：**eat something

可以看到，我们用Child继承了Person类，然后实现了Child的一个实例，将其向上转型为Person类的引用

但是，如果此处的Child类只使用一次，那么将其编写为独立的一个类岂不是很麻烦？

这个时候就引入了匿名内部类

 

#### 实例2：匿名内部类的基本实现

```java
abstract class Person {
    public abstract void eat();
}
 
public class Demo {
    public static void main(String[] args) {
        Person p = new Person() {
            public void eat() {
                System.out.println("eat something");
            }
        };
        p.eat();
    }
}
```

**运行结果：**eat something

可以看到，我们直接将抽象类Person中的方法在大括号中实现了

这样便可以省略一个类的书写

并且，匿名内部类还能用于接口上

####  

#### 实例3：在接口上使用匿名内部类

```java
interface Person {
    public void eat();
}
 
public class Demo {
    public static void main(String[] args) {
        Person p = new Person() {
            public void eat() {
                System.out.println("eat something");
            }
        };
        p.eat();
    }
}
```

**运行结果：**eat something

 

由上面的例子可以看出，只要一个类是抽象的或是一个接口，那么其子类中的方法都可以使用匿名内部类来实现

最常用的情况就是在多线程的实现上，因为要实现多线程必须继承Thread类或是继承Runnable接口

 

#### 实例4：Thread类的匿名内部类实现

```java

public class Demo {
    public static void main(String[] args) {
        Thread t = new Thread() {
            public void run() {
                for (int i = 1; i <= 5; i++) {
                    System.out.print(i + " ");
                }
            }
        };
        t.start();
    }
}
```

**运行结果：**1 2 3 4 5

 

#### 实例5：Runnable接口的匿名内部类实现

```java
public class Demo {
    public static void main(String[] args) {
        Runnable r = new Runnable() {
            public void run() {
                for (int i = 1; i <= 5; i++) {
                    System.out.print(i + " ");
                }
            }
        };
        Thread t = new Thread(r);
        t.start();
    }
}
```

**运行结果：**1 2 3 4 5

# AtomicInteger类的原理

Java中的AtomicInteger大家应该很熟悉，它是为了解决多线程访问Integer变量导致结果不正确所设计的一个基于多线程并且支持原子操作的Integer类。

它的使用也非常简单：

```
AtomicInteger ai = new AtomicInteger(0);
ai.addAndGet(5); // 5
ai.getAndAdd(1); // 5
ai.get(); // 6
```

AtomicInteger内部有一个变量UnSafe：

```
private static final Unsafe unsafe = Unsafe.getUnsafe();
```

Unsafe类是一个可以执行不安全、容易犯错的操作的一个特殊类。虽然Unsafe类中所有方法都是public的，但是这个类只能在一些被信任的代码中使用。Unsafe的源码可以在这里看 -> [UnSafe源码](http://www.docjar.com/html/api/sun/misc/Unsafe.java.html)。

Unsafe类可以执行以下几种操作：

1. 分配内存，释放内存：在方法allocateMemory，reallocateMemory，freeMemory中，有点类似c中的malloc，free方法
2. 可以定位对象的属性在内存中的位置，可以修改对象的属性值。使用objectFieldOffset方法
3. 挂起和恢复线程，被封装在LockSupport类中供使用
4. CAS操作(CompareAndSwap，比较并交换，是一个原子操作)

AtomicInteger中用的就是Unsafe的CAS操作。

Unsafe中的int类型的CAS操作方法：

```
public final native boolean compareAndSwapInt(Object o, long offset,
                                                int expected,
                                                int x);
```

参数o就是要进行cas操作的对象，offset参数是内存位置，expected参数就是期望的值，x参数是需要更新到的值。

也就是说，如果我把1这个数字属性更新到2的话，需要这样调用：

```
compareAndSwapInt(this, valueOffset, 1, 2)
```

valueOffset字段表示内存位置，可以在AtomicInteger对象中使用unsafe得到：

```
static {
  try {
    valueOffset = unsafe.objectFieldOffset
        (AtomicInteger.class.getDeclaredField("value"));
  } catch (Exception ex) { throw new Error(ex); }
}
```

AtomicInteger内部使用变量value表示当前的整型值，这个整型变量还是volatile的，表示内存可见性，一个线程修改value之后保证对其他线程的可见性：

```
private volatile int value;
```

AtomicInteger内部还封装了一下CAS，定义了一个compareAndSet方法，只需要2个参数：

```
public final boolean compareAndSet(int expect, int update) {
    return unsafe.compareAndSwapInt(this, valueOffset, expect, update);
}
```

addAndGet方法，addAndGet方法内部使用一个死循环，先得到当前的值value，然后再把当前的值加一，加完之后使用cas原子操作让当前值加一处理正确。当然cas原子操作不一定是成功的，所以做了一个死循环，当cas操作成功的时候返回数据。这里由于使用了cas原子操作，所以不会出现多线程处理错误的问题。比如线程A得到current为1，线程B也得到current为1；线程A的next值为2，进行cas操作并且成功的时候，将value修改成了2；这个时候线程B也得到next值为2，当进行cas操作的时候由于expected值已经是2，而不是1了；所以cas操作会失败，下一次循环的时候得到的current就变成了2；也就不会出现多线程处理问题了：

```
public final int addAndGet(int delta) {
    for (;;) {
        int current = get();
        int next = current + delta;
        if (compareAndSet(current, next))
            return next;
    }
}
```

incrementAndGet方法，跟addAndGet方法类似，只不过next值变成了current+1：

```
public final int incrementAndGet() {
    for (;;) {
        int current = get();
        int next = current + 1;
        if (compareAndSet(current, next))
            return next;
    }
}
```

getAndAdd方法，跟addAndGet方法一样，返回值变成了current：

```
public final int getAndAdd(int delta) {
    for (;;) {
        int current = get();
        int next = current + delta;
        if (compareAndSet(current, next))
            return current;
    }
}
```

缺点：

虽然AtomicInteger中的cas操作可以实现非阻塞的原子操作，但是会产生ABA问题，

# [Java—synchronized和ReentrantLock锁详解](https://www.cnblogs.com/Andya/p/12850659.html)

# 1 synchronized

## 1.1 synchronized介绍

1. synchronized机制提供了对每个对象相关的隐式监视器锁，并强制所有锁的获取和释放都必须在同一个块结构中。当获取了多个锁时，必须以**相反的顺序释放**。即**synchronized对于锁的释放是隐式的**。
2. synchronized同步块对于同一条线程是**可重入的**，不会出现把自己锁死的问题。
3. synchronized可以修饰类、方法（包括静态方法）、代码块。修饰类和静态方法时，锁的对象是Class对象；修饰普通方法时，锁的是调用该方法的对象；修饰代码块时，锁的是方法块括号里的对象。
4. synchronized性能中**避免“读/读”操作**，但读操作频繁，通过ReentrantLock提供的ReadWriteLock读写锁来解决该问题；阻塞线程时，需要OS不断的从用户态转到核心态，消耗处理器时间，通过自适应自旋来解决该问题。
5. synchronized的**锁是存放在Java对象头**里的。

## 1.2 synchronized的三种使用场景

### 1.2.1 修饰实例方法

1）说明：对当前对象实例this加锁
2）示例：

```java
public class Demo1 {
	public synchronized void methodA() {
		System.out.println("synchronized 修饰 普通实例方法");
	}
}
```

### 1.2.2 修饰静态方法

1）对当前类的Class对象加锁
2）示例

```java
public class Demo2 {
	public synchronized static void methodB() {
		System.out.println("synchronized 修饰 静态方法");
	}
}
```

### 1.2.3 修饰代码块

1）给指定对象加锁
2）说明

```java
public class Demo3 {
	public void methodC() {
		synchronized(this) {
			System.out.println("synchronized代码块对当前对象加锁");
		}
	}

	public void methodC() {
		synchronized(Demo3.Class) {
			System.out.println("synchronized代码块对当前类的Class对象加锁");
		}
	}
}
```

## 1.3 synchronized实现同步的基础

Java中每一个对象都可以作为锁，这是synchronized实现同步的基础：

1. 普通同步方法：锁是当前实例对象；
2. 静态同步方法：锁是当前类的Class对象；
3. 同步方法块：锁是括号里面的对象；

**同步代码块**：**monitorenter指令插入到同步代码块的开始位置，monitorexit指令插入到同步代码块的结束位置，JVM需要保证每一个monitorexit和monitorenter相对应**，任何对象都有一个monitor与之相对应，任何对象都有一个monitor与之相关联，当且一个monitor被持有之后，他处于锁定状态，线程执行到monitorenter指令时，将会尝试获取对象所对应的monitor所有权，即尝试获取对象的锁。

**同步方法**：synchronized方法则会被翻译成普通的方法调用和返回指令如:invokevirtual、areturn指令，在VM字节码层面并没有任何特别的指令来实现被synchronized修饰的方法，**而是在Class文件的方法表中将该方法的access_flags字段中的synchronized标志位置1，表示该方法是同步方法并使用调用该方法的对象或该方法所属的Class在JVM的内部对象表示Klass做为锁对象。**

## **1.4 synchronized反编译**字节码

synchronized字节码层面实现的锁（锁计数器）。

1. synchronized关键字经编译后，在同步块前后分别形成monitorenter和monitorexit两个字节码指令；
2. **在执行monitorenter指令时，首先要尝试获取对象的锁，若这个对象没被锁定，或当前线程已经拥有了那个对象的锁，就把锁的计数器加1；**
3. **在执行monitorexit指令时会将锁计数器减1，当计数器为0时，锁就会被释放；**
4. 若获取对象锁失败，当前线程进入阻塞等待，直到对象锁被另外一个线程释放为止。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200508110536426.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FuZHlhX25ldA==,size_16,color_FFFFFF,t_70)

### 1.4.1 monitorenter

（重入锁的体现）每个对象都有一个监视器锁（monitor），当monitor被占用时就会处于锁定状态，线程执行monitorenter指令时尝试获取monitor的所有权。

1. 如果monitor的进入数为0，则该线程进入monitor，然后将进入数设置为1，该线程即为monitor的所有者。
2. 如果线程已经占有该monitor，只是重新进入，则进入monitor的进入数加1。
3. 如果其他线程已经占用了monitor，则该线程进入阻塞状态，直至monitor的进入数为0，再重新尝试获取monitor的所有权。

### 1.4.2 monitorexit

1. 执行monitorexit的线程必须是object所对应的monitor的所有者。
2. 指令执行时，monitor的进入数减1，如果减1后进入数为0，那么线程就会退出monitor，不再是这个monitor的所有者。其他被这个monitor阻塞的线程可以尝试去获取这个monitor的所有权。

## 1.5 Java对象

### 1.5.1 Java对象组成

  Java对象存储在堆内存中，对象由**对象头、实例变量和填充字节**组成。

### 1.5.2 对象头

1. synchronized用的锁是存放在**Java对象头**里。Hotspot虚拟机的对象头主要包括两部分数据：**Mark Word（标记字段）**、**Klass Pointer（类型指针）**。
2. **Klass Point**是对象指向它的**类元数据的指针**，虚拟机通过这个指针来**确定这个对象是哪个类的实例**。
3. **Mark Word**用于**存储对象自身的运行时数据**，它是实现轻量级锁和偏向锁的关键。如**哈希码（HashCode）、GC分代年龄、锁状态标志、线程持有的锁、偏向线程 ID、偏向时间戳**等等。Java对象头一般占有两个机器码（在32位虚拟机中，1个机器码等于4字节，也就是32bit），但是如果对象是数组类型，则需要三个机器码，因为JVM虚拟机可以通过Java对象的元数据信息确定Java对象的大小，但是无法从数组的元数据来确认数组的大小，所以用一块来记录数组长度。

下图是Java对象头的存储结构（32位虚拟机）：

| 对象的hashCode | 对象的分代年龄 | 是否为偏向锁 | 锁标志位 |
| -------------- | -------------- | ------------ | -------- |
| 25bit          | 4bit           | 1bit         | 2bit     |

对象头信息是与对象自身定义的数据无关的额外存储成本，单考虑VM的空间小了，Mark Word设计成一个非固定的数据结构以便在极小的空间内存存储尽量多的数据，会根据对象的状态复用自己的存储空间，即Mark Word是随着程序的运行发生变化。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200508094955764.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FuZHlhX25ldA==,size_16,color_FFFFFF,t_70)

### 1.5.3 实例数据

是**对象存储的真正有效信息**，存储着自身定义的和从父类继承下来的实例字段，字段的存储顺序会受到虚拟机的分配策略和字段在Java源码中定义顺序的影响，这部分内存按4字节对齐。

### 1.5.4 对齐填充字段

由于虚拟机要求对象起始地址必须是8字节的整数倍。填充数据**不是必须**存在的，**仅仅是为了字节对齐**。

# 2 ReentrantLock

## 2.1 ReentrantLock介绍

1. Lock提供一种**无条件的、可轮询的、定时**的以及**可中断**的锁获取操作，所有**加锁和解锁**都是**显式**的。
2. ReentrantLock**实现Lock接口**，并提供与synchronized相同的**互斥性和内存可见性**。
3. ReentrantLock提供和synchronized一样的**可重入**的加锁语义。
4. ReentrantLock是**显式锁**，需要显式进行**lock以及unlock**操作。形式比内置锁复杂，必须在finally块中释放锁，否则如果在被保护的代码中抛出异常，这个锁就永远都无法释放。加锁时，需要考虑在try块中抛出异常的情况，如果可能使对象处于某种不一致的状态，则需要更多的try-catch或try-finally代码块。

## 2.2 ReentrantLock示例

```java
Lock lock = new ReentrantLock();
...
lock.lock();
try{
	//更新对象状态
	//捕获异常，并在必要时恢复不变性条件
}finally{
	lock.unlock();
}
```

## 2.3 使用ReentrantLock的灵活性

（也是与synchronized的区别）

### 2.3.1 等待可中断

  使用`lock.lockInterruptibly()`可以使得线程在等待锁支持响应中断；
  **使用`lock.tryLock()`可以使线程在等待一段时间过后如果还未获得锁就停止等待而非一直等待，更好的避免饥饿和死锁问题；**

### 2.3.2 公平锁

  **默认**情况下是**非公平锁**，但是也可以是公平锁，公平锁就是锁的等待队列的FIFO，不建议使用，会浪费许多时钟周期，达不到最大利用率。

### 2.3.3 锁可绑定多个条件

  与ReentrantLock搭配的通信方式是`Condition`,且可以为多个线程建立不同的Codition。

```java
private Lock lock = new ReentrantLock();
private Condition condition = lock.newCondition();
condition.await();  //等价于synchronized中的wait()方法
condition.signal(); //等价于notify()
condition.signalAll(); //等价于notifyAll()
```

## 2.4 读-写锁

### 2.4.1 读写锁的出现原因

  ReentrantLock实现一种标准的**互斥锁**，**每次最多只有一个线程能持有ReentrantLock**，限制了并发性，互斥是一种保守的加锁策略，虽然避免了“写/写”冲突和“写/读”冲突，但也避免了“读/读”冲突，而大部分情况下读操作比较多，如果此时能够放宽加锁需求，允许多个读操作的线程同时访问数据结构，可以提升程序的性能（只要每个线程保证读取到最新的数据，并且在读取数据时不会有其他线程修改数据就行）

### 2.4.2 ReentrantLock提供的非互斥的读写锁的定义

  一个资源可以被多个读操作访问，或者被一个写操作访问，但两者不能读写操作同时进行。（多读、一写、不可同读写）
  读-写锁是一种性能优化措施，可以实现更高的并发性，提高程序的性能。
  当锁的持有时间较长并且大部分操作都不会修改被守护的资源时，读-写锁可以提高并发性。

# 3 Lock接口

## 3.1 Lock接口介绍

  Lock接口提供一种无条件的、可轮询的、定时的以及可中断的锁获取操作，所有加锁和解锁都是显示的。

## 3.2 Lock方法

1. `void lock();`：获取锁。
2. `void lockInterruptibly() throws InterruptedException;`：若当前线程未被中断，获取锁。
3. `boolean tryLock();`：仅当调用时锁为空闲状态才获取锁。
4. `boolean tryLock(long time, TimeUnit unit) throws InterruptedException;`：如果锁在给定的等待时间内空闲，并且当前线程未被中断，则获取锁。
5. `void unlock();`：释放锁。
6. `Condition newCondition();`：返回绑定在此Lock实例的新Condition实例。

# 4 锁优化

## 4.1 jdk1.6对锁进行优化：

  自旋锁、适应性自旋锁、锁消除、锁粗化、偏向锁、轻量级锁。

## 4.2 锁的四个状态：

  无锁状态、偏向锁状态、轻量级锁状态、重量级锁状态。
  锁可以升级（随着锁的竞争激烈程度而升级），但是不可降级。

## 4.3 自旋锁

1. **引入原因**：**线程的阻塞和唤醒需要CPU从用户态转为核心态以及核心态转为用户态，频繁的阻塞和唤醒对CPU来说是一件负担很重的工作**，势必会给系统的并发性能带来很大的压力。同时我们发现在许多应用上面，对象锁的锁状态只会持续很短一段时间，**为了这一段很短的时间频繁地阻塞和唤醒线程是非常不值得的。**
2. **原理**：就是让该线程等待一段时间（执行一段无意义的循环即可（自旋）），不会被立即挂起，看持有锁的线程是否会很快释放锁。**（线程自循环等待，不立即挂起）**
3. **缺点**：自旋不能代替阻塞，可以避免线程切换带来的开销，但是会占用处理器的时间，若持有锁的线程很快释放锁，则自旋效率好；反之，自旋的线程会白白消耗掉处理器的资源，带来性能上的浪费。
4. **针对缺点的解决方案**：自旋等待时间有限度——超过定义的时间没有获得锁就应该被挂起。自旋锁是JDK1.4.2引入，默认关闭，使用-XX:UseSpining开启，在JDK1.6默认开启，默认自旋次数为10次，可以通过-XX:PreBlockSpin调整。最终是通过自适应自旋锁来解决这一问题。

## 4.4 自适应自旋锁

1. **引入原因**：自旋锁的自旋次数是固定的，当持有锁不能很快释放锁时，效率低下，浪费处理器资源，自旋次数需要通过-XX:PreBlockSpin调整。所以引入自适应自旋锁不再固定自旋次数。
2. **原理**：由前一次在同一个锁上的自旋时间及锁的拥有者状态决定。线程如果自旋成功了，那么下次自旋的次数会更加多，因为虚拟机认为既然上次成功了，那么此次自旋也很有可能会再次成功，那么它就会允许自旋等待持续的次数更多。反之，如果对于某个锁，很少有自旋能够成功的，那么在以后要或者这个锁的时候自旋的次数会减少甚至省略掉自旋过程，以免浪费处理器资源。**（主要就是看前一次自旋成功与否，成功多就增加自旋次数；成功少，就减少自旋次数）**

## 4.5 锁消除

1. **引入原因**：在某些情况下，JVM检测到不可能存在共享数据竞争，如果继续加锁，降低性能，无意义。
2. **原理**：锁消除依据是逃逸分析的数据支持，检测到不存在共享数据竞争，就消除锁，从而节省请求锁的时间。**（逃逸分析检测是否有共享数据竞争，若有，消除锁）**

## 4.6 锁粗化

1. **引入原因**：一系列的连续加锁解锁操作，导致不必要的性能损耗。
2. **原理**：将多个连续的加锁、解锁操作连接在一起，扩展成一个范围更大的锁。如Vector的add操作操作，JVM检测到对同一个对象（vector）连续加锁、解锁操作，则合并成一个更大范围的加锁、解锁操作，进行锁粗化。**（多个连续加锁、解锁合并成一个更大范围的锁）**

## 4.7 轻量级锁（00）

1. **引入原因**：在没有多线程竞争的前提下，减少传统的重量级锁使用操作系统互斥量产生的性能损耗。
2. **原理**：当关闭偏向锁功能或者多个线程竞争偏向锁导致偏向锁升级为轻量级锁，则会尝试获取轻量级锁，其步骤如下：

**获取锁**

- 1）判断当前对象是否处于无锁状态（hashcode、0、01），若是，则JVM首先将在当前线程的栈帧中建立一个名为锁记录（Lock Record）的空间，用于存储锁对象目前的Mark Word的拷贝（官方把这份拷贝加了一个Displaced前缀，即Displaced Mark Word）；否则执行步骤 3）；
- 2）JVM利用CAS操作尝试将对象的Mark Word更新为指向Lock Record的指针，如果成功表示竞争到锁，则将锁标志位变成00（表示此对象处于轻量级锁状态），执行同步操作；如果失败则执行步骤 3）；
- 3）判断当前对象的Mark Word是否指向当前线程的栈帧，如果是则表示当前线程已经持有当前对象的锁，则直接执行同步代码块；否则只能说明该锁对象已经被其他线程抢占了，这时轻量级锁需要膨胀为重量级锁，锁标志位变成10，后面等待的线程将会进入阻塞状态；

**释放锁**：轻量级锁的释放也是**通过CAS操作**来进行的，主要步骤如下：

- 1）取出在获取轻量级锁保存在Displaced Mark Word中的数据；
- 2）用CAS操作将取出的数据替换当前对象的Mark Word中，如果成功，则说明释放锁成功，否则执行 3）；
- 3）如果CAS操作替换失败，说明有其他线程尝试获取该锁，则需要在释放锁的同时需要唤醒被挂起的线程。

1. 性能分析：对于轻量级锁，其性能提升的依据是“对于绝大部分的锁，在整个生命周期内都是不会存在竞争的”，如果打破这个依据则除了互斥的开销外，还有额外的CAS操作，因此在有多线程竞争的情况下，轻量级锁比重量级锁更慢；

## 4.8 偏向锁（01）

1. 引入原因：为了在无多线程竞争的情况下尽量减少不必要的轻量级锁执行路径。轻量级锁的加锁解锁操作是需要依赖多次CAS原子指令的。
2. 原理

**获取锁**：

- 1）检测Mark Word是否为可偏向状态，即是否为偏向锁1，锁标识位为01；
- 2）若为可偏向状态，则测试线程ID是否为当前线程ID，如果是，则执行步骤 5），否则执行步骤 3）；
- 3）如果线程ID不为当前线程ID，则通过CAS操作竞争锁，竞争成功，则将Mark Word的线程ID替换为当前线程ID，否则执行步骤 4）；
- 4）通过CAS竞争锁失败，证明当前存在多线程竞争情况，当到达全局安全点，获得偏向锁的线程被挂起，偏向锁升级为轻量级锁，然后被阻塞在安全点的线程继续往下执行同步代码块；
- 5）执行同步代码块。

**释放锁**：偏向锁的释放采用了一种**只有竞争才会释放锁的机制**，线程是不会主动去释放偏向锁，需要等待其他线程来竞争。偏向锁的撤销需要等待全局安全点（这个时间点是上没有正在执行的代码）。其步骤如下：

- 1）暂停拥有偏向锁的线程，判断锁对象是否还处于被锁定状态；
- 2）撤销偏向锁，恢复到无锁状态（01）或者轻量级锁的状态；

## 4.9 重量级锁（10）

  重量级锁通过对象内部的监视器（monitor）实现，其中monitor的本质是依赖于底层操作系统的Mutex Lock实现，操作系统实现线程之间的切换需要从**用户态到内核态的切换**，切换成本非常高。

## 4.10 锁的优缺点的对比

| 锁       | 优点                                                         | 缺点                               | 适用场景                         |
| -------- | ------------------------------------------------------------ | ---------------------------------- | -------------------------------- |
| 偏向锁   | 加解锁不需要额外操作，速度较快 若是发生线程间的锁竞争，会产生锁撤销的消耗 | 适用于只有一个线程访问同步块的场景 |                                  |
| 轻量级锁 | 竞争的线程不会阻塞，程序响应速度块                           | 不断的自旋会消耗CPU资源            | 追求响应时间，同步块执行速度很快 |
| 重量级锁 | 线程竞争不使用自旋，CPU消耗少                                | 线程会阻塞，相应慢                 | 追求吞吐量，同步块执行时间较长   |

# Q&A

### synchronized方法和synchronized块的区别

1. **synchronized块**：是一种**细粒度**的并发控制，只会将块中的代码同步，位于方法内、synchronized块之外的代码是可以被多个线程同时访问到的，锁的是方法块后面括号里的对象；**synchronized方法**是一种**粗粒度**的并发控制，某一时刻，只能有一个线程执行该synchronized方法，锁的是调用该方法的对象。
2. **同步代码块**：monitorenter指令插入到同步代码块的开始位置，monitorexit指令插入到同步代码块的结束位置，JVM需要保证每一个monitorexit和monitorenter相对应，任何对象都有一个monitor与之相对应，任何对象都有一个monitor与之相关联，当且一个monitor被持有之后，他处于锁定状态，线程执行到monitorenter指令时，将会尝试获取对象所对应的monitor所有权，即尝试获取对象的锁；**同步方法**：synchronized方法则会被翻译成普通的方法调用和返回指令如:invokevirtual、areturn指令，在VM字节码层面并没有任何特别的指令来实现被synchronized修饰的方法，**而是在Class文件的方法表中将该方法的access_flags字段中的synchronized标志位置1**，表示该方法是同步方法并使用调用该方法的对象或该方法所属的Class在JVM的内部对象表示Klass做为锁对象。在常量池中添加了ACC_ SYNCHRONIZED标识符，JVM就是根据该标识符来实现方法的同步。

### synchronized和ReentrantLock的异同

**同**：

1. 都是可重入的；
2. 都属于同步互斥的手段；

**异：**
1 **底层**：**synchronized**是**原生语法**层面的互斥锁；**ReentrantLock**是**API**层面的互斥锁；
\2. **加锁释放锁范式**：**synchronized**是内置锁，获取多个锁后，以**相反的顺序隐式释放锁**；**ReentrantLock**必须**显式加锁释放锁**，且可以自由的顺序释放锁。
\3. **功能**：ReentrantLock增加高级功能：等待可中断、可实现公平锁、锁可以绑定多个条件。

| 功能               | 说明                                                         |
| ------------------ | ------------------------------------------------------------ |
| **等待可中断**     | 当持有锁的线程长期不释放锁的时候，正在等待的线程可以选择放弃等待，改为处理其他事情，可中断特性适用于处理执行时间非常长的同步块。 |
| **公平锁**         | ReentrantLock可以是公平锁，默认为非公平锁，通过带布尔值的构造函数使用公平锁，公平锁是多个线程在等待同一个锁时，必须按照申请锁的时间顺序来一次获得锁；synchronized是非公平锁，非公平锁是不保证的，在锁释放时，任何一个等待锁的线程都有机会获得锁。 |
| **锁绑定多个条件** | 指一个ReentrantLock对象可同时绑定多个Condition对象，多次调用newCondition()方法；而在synchronized中，锁对象的wait()和notify()或notifyAll()方法可以实现一个隐含的条件，若要和多个条件关联，需要额外的添加一个锁。 |

### 什么是可重入锁？

  重入锁实现重入性：每个锁关联**一个线程持有者**和**计数器**：

1. 当计数器为0时表示该锁没有被任何线程持有，那么任何线程都可能获得该锁而调用相应的方法；
2. 当某一线程请求成功后，JVM会记下锁的持有线程，并且将计数器置为1；
3. 此时其它线程请求该锁，则必须等待；而该持有锁的线程如果再次请求这个锁，就可以再次拿到这个锁，同时计数器会递增+1；
4. 当线程退出同步代码块时，计数器会递减-1，如果计数器为0，则释放该锁。

### synchronized锁的存储位置？

1. synchronized锁存放的位置是Java对象头里，如果对象是数组类型，则JVM用3个字节宽存储对象头，如果对象是非数组类型，则用2字节宽度存储对象头。
2. 对象头由mark word+类型指针组成。mark word默认存储对象的hashcode、分代年龄和锁标志位。

### synchronized锁存储在对象头，何为对象头？

对象头中包括两部分数据：**标记字段和类型指针**；

1. **类型指针Klass Point**是对象指向它的**类元数据的指针**，虚拟机通过这个指针来确定这个对象是哪个类的实例；
2. **标记字段Mark Word**是一个非固定的数据结构，用于**存储对象自身的运行时数据**，如哈希码（HashCode）、GC分代年龄、锁状态标志、线程持有的锁、偏向线程 ID、偏向时间戳等等。

### Lock与synchronized的主要区别：

1. **类型**：Lock是一个接口；synchronized是Java中的关键字，synchronized是内置的语言实现；
2. **异常**：Lock在发生异常时，如果没有主动通过unLock释放锁，可能造成死锁现象，使用Lock时需要在finally块中释放锁；synchronized在发生异常时，会自动释放线程中的占有的锁，不会导致死锁现象发生。
3. **加锁释放锁**：synchronized机制提供了对与每个对象相关的隐式的监视器锁，并强制所有锁的获取和释放都必须在同一个块结构中。当获取了多个锁时，他们必须以相反的顺序释放。即synchronized对于锁的释放是隐式的。而Lock机制必须显式的调用Lock对象的unlock()方法，这样，获取锁和释放锁可以不在同一个块中，这样可以以更自由的顺序释放锁。
4. **响应中断**：Lock可以让等待锁的线程响应中断；synchronized不可以让等待锁的线程响应中断，等待的线程会一直等待下去，不能够响应中断；
5. **获取锁成功与否**：通过Lock可以知道有没有成功获取锁；synchronized不能；
6. **读操作**：Lock可以提高多个线程进行读操作的效率（通过ReadWriteLock）；而synchronized避免读/读操作

### 当一个线程进入一个对象的一个synchronized()方法后，其他线程能进入此对象的什么样的方法？

1. 当一个线程在调用synchronized()方法的过程中，另一个线程可以访问同一个对象的非synchronized()方法；
2. 当一个线程在调用synchronized()方法的过程中，另一个线程可以访问静态synchronized()方法，因为静态方法的同步锁是当前类的字节码，与非静态方法不能同步；
3. 当一个线程在调用synchronized()方法的过程中，在这个方法内部调用了wait()方法，则另一个线程就可以访问同一个对象的其他synchronized()方法。

### 什么是同步代码块和内置锁？

**同步代码块**

1. 同步代码块包括两部分：一个作为锁定的对象引用，一个作为由这个锁保护的代码块。
2. 静态的synchronized方法以Class对象作为锁；

**内置锁**

1. 每个Java对象都可以用于做一个实现同步的锁，则这些锁称为内置锁或监视器锁。
2. 获得内置锁的唯一途径是进入由这个锁保护的同步代码块或方法。
3. 内置锁：互斥且可重入。

### 为什么要创建一种与内置锁相似的新加锁机制？

  内置锁能很好的工作，但是在功能上存在一些局限性，如**synchronized无法中断一个正在等待获取锁的线程**，或者**无法在请求获取一个锁的时候无限地等待下去**，内置锁必须在获取该锁的代码块中释放，简化了编码工作，且与异常处理操作实现很好的交互，但无法实现非阻塞结构的加锁机制。所以需要提供一种更灵活的加锁机制来提供更好的活跃性或性能。

### synchronized和ReentrantLock的抉择

1. **高级功能**：当需要可定时的、可轮询的、可中断锁、公平锁以及非块结构的锁（锁分段技术）时，才使用ReentrantLock，否则优先使用synchronized，毕竟现在JVM内置的是synchronized。
2. **ReentrantLock的危险性**：如果在try-finally中，finally未进行unlock，就会导致锁没有释放，无法追踪最初发生错误的位置。

### ReentrantLock中断和非中断加锁区别？

ReentrantLock的中断和非中断加锁模式的区别在于：线程尝试获取锁操作失败后，在等待过程中，如果该线程被其他线程中断了，它是如何响应中断请求的。`lock()`方法会忽略中断请求，继续获取锁直到成功；而`lockInterruptibly()`则直接抛出中断异常来立即响应中断，由上层调用者处理中断。

# ThreadLocal内存泄漏

ThreadLocal 基本用法本文就不介绍了，如果有不知道的小伙伴可以先了解一下，本文只研究 ThreadLocal 内存泄漏这一问题。

## ThreadLocal 会发生内存泄漏吗？

先给出结论：**如果你使用不当是有可能发生内存泄露的**

![img](https://segmentfault.com/img/remote/1460000022704088)

<br/>

每个 Thread 里面都有一个 ThreadLocalMap，而 ThreadLocalMap 中真正承载数据的是一个 Entry 数组，Entry 的 Key 是 threadlocal 对象的弱引用。

这里或许有的小伙伴有疑问，Entry 的 Key 是 threadlocal 对象的弱引用，为什么要用弱引用，换成强引用行不行？

首先，我们先了解一下什么是弱引用？

> 弱引用一般是用来描述非必需对象的，被弱引用关联的对象只能生存到下一次垃圾收集发生之前。**当垃圾收集器工作时，无论当前内存是否足够，都会回收掉只被弱引用关联的对象**。

实际开发中，当我们不需要 threadlocal 后，为了 GC 将 threadlocal 变量置为 null，没有任何强引用指向堆中的 threadlocal 对象时，堆中的 threadlocal 对象将会被 GC 回收，假设现在 Key 持有的是 threadlocal 对象的强引用，如果当前线程仍然在运行，那么从当前线程一直到 threadlocal 对象还是存在强引用，由于当前线程仍在运行的原因导致 threadlocal 对象无法被 GC，这就发生了内存泄漏。相反，弱引用就不存在此问题，当栈中的 threadlocal 变量置为 null 后，堆中的 threadlocal 对象只有一个 Key 的弱引用关联，下一次 GC 的时候堆中的 threadlocal 对象就会被回收，**使用弱引用对于 threadlocal 对象而言是不会发生内存泄漏的**。

那么，第二个问题来了，是不是 Key 持有的是 threadlocal 对象的弱引用就一定不会发生内存泄漏呢？

结论是：**如果你使用不当还是有可能发生内存泄露**，但是，这里发生内存泄漏的地方和上面不同。

当 threadlocal 使用完后，将栈中的 threadlocal 变量置为 null，threadlocal 对象下一次 GC 会被回收，那么 Entry 中的与之关联的弱引用 key 就会变成 null，如果此时当前线程还在运行，那么 Entry 中的 key 为 null 的 Value 对象并不会被回收（存在强引用），这就发生了内存泄漏，当然这种内存泄漏分情况，如果当前线程执行完毕会被回收，那么 Value 自然也会被回收，但是如果使用的是线程池呢，线程跑完任务以后放回线程池（线程没有销毁，不会被回收），Value 会一直存在，这就发生了内存泄漏。

## 如何更好的降低内存泄漏的风险呢？

ThreadLocal 为了降低内存泄露的可能性，**在 set，get，remove 的时候都会清除此线程 ThreadLocalMap 里 Entry 数组中所有 Key 为 null 的 Value**。所以，当前线程使用完 threadlocal 后，我们可以通过调用 ThreadLocal 的 remove 方法进行清除从而降低内存泄漏的风险。

由于ThreadLocalMap的生命周期跟Thread一样长，如果没有手动删除对应key就会导致内存泄漏，我觉得是这种数据结构导致，会产生内存溢出的问题

Java为了最小化减少内存泄露的可能性和影响，在ThreadLocal的get,set的时候都会清除线程Map里所有key为null的value。所以最怕的情况就是，threadLocal对象设null了，开始发生“内存泄露”，然后使用线程池，这个线程结束，线程放回线程池中不销毁，这个线程一直不被使用，或者分配使用了又不再调用get,set方法，那么这个期间就会发生真正的内存泄露。

## 前言

`ThreadLocal` 的作用是提供线程内的局部变量，这种变量在线程的生命周期内起作用，减少同一个线程内多个函数或者组件之间一些公共变量的传递的复杂度。但是如果滥用`ThreadLocal`，就可能会导致内存泄漏。下面，我们将围绕三个方面来分析`ThreadLocal` 内存泄漏的问题

- `ThreadLocal` 实现原理
- `ThreadLocal`为什么会内存泄漏
- `ThreadLocal` 最佳实践

## ThreadLocal 实现原理



![img](https:////upload-images.jianshu.io/upload_images/5959612-df3da0d24c26271b.png?imageMogr2/auto-orient/strip|imageView2/2/w/714/format/webp)

[image](http://www.importnew.com/?attachment_id=22042)



ThreadLocal

`ThreadLocal`的实现是这样的：每个`Thread` 维护一个 `ThreadLocalMap` 映射表，这个映射表的 `key` 是 `ThreadLocal`实例本身，`value` 是真正需要存储的 `Object`。

也就是说 `ThreadLocal` 本身并不存储值，它只是作为一个 `key` 来让线程从 `ThreadLocalMap` 获取 `value`。值得注意的是图中的虚线，表示 `ThreadLocalMap` 是使用 `ThreadLocal` 的弱引用作为 `Key` 的，弱引用的对象在 GC 时会被回收。

## `ThreadLocal`为什么会内存泄漏

`ThreadLocalMap`使用`ThreadLocal`的弱引用作为`key`，如果一个`ThreadLocal`没有外部强引用来引用它，那么系统 GC 的时候，这个`ThreadLocal`势必会被回收，这样一来，`ThreadLocalMap`中就会出现`key`为`null`的`Entry`，就没有办法访问这些`key`为`null`的`Entry`的`value`，如果当前线程再迟迟不结束的话，这些`key`为`null`的`Entry`的`value`就会一直存在一条强引用链：`Thread Ref -> Thread -> ThreaLocalMap -> Entry -> value`永远无法回收，造成内存泄漏。

其实，`ThreadLocalMap`的设计中已经考虑到这种情况，也加上了一些防护措施：在`ThreadLocal`的`get()`,`set()`,`remove()`的时候都会清除线程`ThreadLocalMap`里所有`key`为`null`的`value`。

但是这些被动的预防措施并不能保证不会内存泄漏：

- 使用`static`的`ThreadLocal`，延长了`ThreadLocal`的生命周期，可能导致的内存泄漏（参考[ThreadLocal 内存泄露的实例分析](http://blog.xiaohansong.com/2016/08/09/ThreadLocal-leak-analyze/)）。
- 分配使用了`ThreadLocal`又不再调用`get()`,`set()`,`remove()`方法，那么就会导致内存泄漏。

### 为什么使用弱引用

从表面上看内存泄漏的根源在于使用了弱引用。网上的文章大多着重分析`ThreadLocal`使用了弱引用会导致内存泄漏，但是另一个问题也同样值得思考：为什么使用弱引用而不是强引用？

我们先来看看官方文档的说法：

> To help deal with very large and long-lived usages, the hash table entries use WeakReferences for keys.
>  为了应对非常大和长时间的用途，哈希表使用弱引用的 key。

下面我们分两种情况讨论：

- **key 使用强引用**：引用的`ThreadLocal`的对象被回收了，但是`ThreadLocalMap`还持有`ThreadLocal`的强引用，如果没有手动删除，`ThreadLocal`不会被回收，导致`Entry`内存泄漏。
- **key 使用弱引用**：引用的`ThreadLocal`的对象被回收了，由于`ThreadLocalMap`持有`ThreadLocal`的弱引用，即使没有手动删除，`ThreadLocal`也会被回收。`value`在下一次`ThreadLocalMap`调用`set`,`get`，`remove`的时候会被清除。

比较两种情况，我们可以发现：由于`ThreadLocalMap`的生命周期跟`Thread`一样长，如果都没有手动删除对应`key`，都会导致内存泄漏，但是使用弱引用可以多一层保障：**弱引用`ThreadLocal`不会内存泄漏，对应的`value`在下一次`ThreadLocalMap`调用`set`,`get`,`remove`的时候会被清除**。

因此，`ThreadLocal`内存泄漏的根源是：由于`ThreadLocalMap`的生命周期跟`Thread`一样长，如果没有手动删除对应`key`就会导致内存泄漏，而不是因为弱引用。

## ThreadLocal 最佳实践

综合上面的分析，我们可以理解`ThreadLocal`内存泄漏的前因后果，那么怎么避免内存泄漏呢？

- 每次使用完`ThreadLocal`，都调用它的`remove()`方法，清除数据。

在使用线程池的情况下，没有及时清理`ThreadLocal`，不仅是内存泄漏的问题，更严重的是可能导致业务逻辑出现问题。所以，使用`ThreadLocal`就跟加锁完要解锁一样，用完就清理。



作者：小小少年Boy
链接：https://www.jianshu.com/p/1342a879f523
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

# volatile关键字

# 1. Java内存模型

Java虚拟机规范试图定义一种Java内存模型（JMM）,来屏蔽掉各种硬件和操作系统的内存访问差异，让Java程序在各种平台上都能达到一致的内存访问效果。简单来说，由于CPU执行指令的速度是很快的，但是内存访问的速度就慢了很多，相差的不是一个数量级，所以搞处理器的那群大佬们又在CPU里加了好几层高速缓存。

在Java内存模型里，对上述的优化又进行了一波抽象。JMM规定所有变量都是存在主存中的，类似于上面提到的普通内存，每个线程又包含自己的工作内存，方便理解就可以看成CPU上的寄存器或者高速缓存。所以线程的操作都是以工作内存为主，它们只能访问自己的工作内存，且工作前后都要把值在同步回主内存。

![df8bc01119b116b9f77fce86b1aec61.png](https://i.loli.net/2020/04/26/1nMxFoIz8ETP7UK.png)

上图为JMM中的内存模型示意图。在线程执行时，首先会从主存中read变量值，再load到工作内存中的副本中，然后再传给处理器执行，执行完毕后再给工作内存中的副本赋值，随后工作内存再把值传回给主存，主存中的值才更新。

使用工作内存和主存，虽然加快的速度，但是也带来了一些问题。比如看下面一个例子：

```java
i = i + 1;
```

假设i初值为0，当只有一个线程执行它时，结果肯定得到1，当两个线程执行时，会得到结果2吗？这倒不一定了。可能存在这种情况：

```java
线程1： load i from 主存    // i = 0
        i + 1  // i = 1
线程2： load i from主存  // 因为线程1还没将i的值写回主存，所以i还是0
        i +  1 //i = 1
线程1:  save i to 主存
线程2： save i to 主存
```

如果两个线程按照上面的执行流程，那么i最后的值居然是1了。如果最后的写回生效的慢，你再读取i的值，都可能是0，这就是缓存不一致问题。

# 2. 并发编程的三个特性

由于会有上面的缓存不一致情况发生，并发编程必须保证原子性、可见性和有序性。

## 2.1 原子性（Atomicity）：

Java中，对基本数据类型的读取和赋值操作是原子性操作，所谓原子性操作就是指这些操作是不可中断的，要做一定做完，要么就没有执行。

```java
i = 2;
j = i;
i++;
i = i + 1；
```

上面4个操作中，`i=2`是读取操作，必定是原子性操作，`j=i`你以为是原子性操作，其实吧，分为两步，一是读取i的值，然后再赋值给j,这就是2步操作了，称不上原子操作，`i++`和`i = i + 1`其实是等效的，读取i的值，加1，再写回主存，那就是3步操作了。所以上面的举例中，最后的值可能出现多种情况，就是因为满足不了原子性。

这么说来，只有简单的读取，赋值是原子操作，还只能是用数字赋值，用变量的话还多了一步读取变量值的操作。有个例外是，虚拟机规范中允许对64位数据类型(long和double)，分为2次32为的操作来处理，但是最新JDK实现还是实现了原子操作的。

JMM只实现了基本的原子性，像上面`i++`那样的操作，必须借助于`synchronized`和`Lock`来保证整块代码的原子性了。线程在释放锁之前，必然会把`i`的值刷回到主存的。

## 2.2 可见性（Visibility）：

说到可见性，Java就是利用volatile来提供可见性的。 **当一个变量被volatile修饰时，那么对它的修改会立刻刷新到主存，当其它线程需要读取该变量时，会去内存中读取新值**。而普通变量则不能保证这一点。

其实通过synchronized和Lock也能够保证可见性，线程在释放锁之前，会把共享变量值都刷回主存，但是synchronized和Lock的开销都更大。

  Synchronized能够实现原子性和可见性；在Java内存模型中，synchronized规定，线程在加锁时，先清空工作内存→在主内存中拷贝最新变量的副本到工作内存→执行完代码→将更改后的共享变量的值刷新到主内存中→释放互斥锁。

## 2.3 有序性

JMM是允许编译器和处理器对指令重排序的，但是规定了as-if-serial语义，即不管怎么重排序，程序的执行结果不能改变。比如下面的程序段：

```java
double pi = 3.14;    //A
double r = 1;        //B
double s= pi * r * r;//C
```

上面的语句，可以按照`A->B->C`执行，结果为3.14,但是也可以按照`B->A->C`的顺序执行，因为A、B是两句独立的语句，而C则依赖于A、B，所以A、B可以重排序，但是C却不能排到A、B的前面。JMM保证了重排序不会影响到单线程的执行，但是在多线程中却容易出问题。

```java
int a = 0;
bool flag = false;

public void write() {
    a = 2;              //1
    flag = true;        //2
}

public void multiply() {
    if (flag) {         //3
        int ret = a * a;//4
    }
    
}
```

假如有两个线程执行上述代码段，线程1先执行write，随后线程2再执行multiply，最后ret的值一定是4吗？结果不一定：

假如程序执行顺序是这样的`线程1(Flag=True;)->线程2(if(flag))->线程2(int ret = a*a;)->线程1(a=2;)`。即write方法里的1和2做了重排序，线程1先对flag赋值为true，随后执行到线程2，ret直接计算出结果，再到线程1，这时候a才赋值为2,很明显迟了一步。

这时候可以为flag加上volatile关键字，禁止重排序，可以确保程序的“有序性”，也可以上重量级的synchronized和Lock来保证有序性,它们能保证那一块区域里的代码都是一次性执行完毕的。

另外，JMM具备一些先天的**有序性**,即不需要通过任何手段就可以保证的有序性，通常称为**happens-before**原则。

> **程序顺序规则**： 一个线程中的每个操作，happens-before于该线程中的任意后续操作
>
> **监视器锁规则**：对一个线程的解锁，happens-before于随后对这个线程的加锁
>
> **volatile变量规则**： 对一个volatile域的写，happens-before于后续对这个volatile域的读
>
> **传递性**：如果A happens-before B ,且 B happens-before C, 那么 A happens-before C
>
> **start()规则**： 如果线程A执行操作`ThreadB_start()`(启动线程B) ,  那么A线程的`ThreadB_start()`happens-before 于B中的任意操作
>
> **join()原则**： 如果A执行`ThreadB.join()`并且成功返回，那么线程B中的任意操作happens-before于线程A从`ThreadB.join()`操作成功返回。
>
> **interrupt()原则**： 对线程`interrupt()`方法的调用先行发生于被中断线程代码检测到中断事件的发生，可以通过`Thread.interrupted()`方法检测是否有中断发生
>
> **finalize()原则**：一个对象的初始化完成先行发生于它的`finalize()`方法的开始

# 3. 深入volatile关键字

一旦一个共享变量（类的成员变量、类的静态成员变量）被volatile修饰之后，那么就具备了两层语义：

　　1）保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。

　　2）禁止进行指令重排序。

对一个volatile域的写，happens-before于后续对这个volatile域的读。 这条再拎出来说，其实就是如果一个变量声明成是volatile的，那么当我读变量时，总是能读到它的最新值，这里最新值是指不管其它哪个线程对该变量做了写操作，都会立刻被更新到主存里，我也能从主存里读到这个刚写入的值。也就是说volatile关键字可以保证可见性以及有序性。

继续拿上面的一段代码举例：

```java
int a = 0;
bool flag = false;

public void write() {
   a = 2;              //1
   flag = true;        //2
}

public void multiply() {
   if (flag) {         //3
       int ret = a * a;//4
   }
   
}
```

这段代码不仅仅受到重排序的困扰，即使1、2没有重排序。3也不会那么顺利的执行的。假设还是线程1先执行`write`操作，线程2再执行`multiply`操作，由于线程1是在工作内存里把flag赋值为1，不一定立刻写回主存，所以线程2执行时，`multiply`再从主存读flag值，仍然可能为false，那么括号里的语句将不会执行。

如果改成下面这样：

```java
int a = 0;
volatile bool flag = false;

public void write() {
   a = 2;              //1
   flag = true;        //2
}

public void multiply() {
   if (flag) {         //3
       int ret = a * a;//4
   }
}
```

那么线程1先执行`write`,线程2再执行`multiply`。根据happens-before原则，这个过程会满足以下3类规则：

1. 程序顺序规则：1 happens-before 2; 3 happens-before 4; (volatile限制了指令重排序，所以1 在2 之前执行)
2. volatile规则：2 happens-before 3
3. 传递性规则：1 happens-before 4

从内存语义上来看，**当写一个volatile变量时，JMM会把该线程对应的工作内存中的共享变量刷新到主内存。当读一个volatile变量时，JMM会把该线程对应的工作内存置为无效，线程接下来将从主内存中读取共享变量**。

volatile关键字禁止指令重排序有两层意思：

　　1）**当程序执行到volatile变量的读操作或者写操作时，在其前面的操作的更改肯定全部已经进行，且结果已经对后面的操作可见；在其后面的操作肯定还没有进行；**

　　2）在进行指令优化时，不能将在对volatile变量访问的语句放在其后面执行，也不能把volatile变量后面的语句放到其前面执行。

代码示例：

```java
//x、y为非volatile变量
//flag为volatile变量
 
x = 2;        //语句1
y = 0;        //语句2
flag = true;  //语句3
x = 4;         //语句4
y = -1;       //语句5
```

由于flag变量为volatile变量，那么在进行指令重排序的过程的时候，不会将语句3放到语句1、语句2前面，也不会讲语句3放到语句4、语句5后面。但是要注意语句1和语句2的顺序、语句4和语句5的顺序是不作任何保证的。

并且volatile关键字能保证，执行到语句3时，语句1和语句2必定是执行完毕了的，且语句1和语句2的执行结果对语句3、语句4、语句5是可见的。

尽管volatile能保证可见性和有序性，但是它不能够保证原子性。要说能保证，也只是对单个volatile变量的读/写具有原子性，但是对于类似volatile++这样的复合操作就无能为力了，比如下面的例子：

```java
public class Test {
    public volatile int inc = 0;
 
    public void increase() {
        inc++;
    }
 
    public static void main(String[] args) {
        final Test test = new Test();
        for(int i=0;i<10;i++){
            new Thread(){
                public void run() {
                    for(int j=0;j<1000;j++)
                        test.increase();
                };
            }.start();
        }
 
        while(Thread.activeCount()>1)  //保证前面的线程都执行完
            Thread.yield();
        System.out.println(test.inc);
    }
```

按道理来说结果是10000，但是运行下很可能是个小于10000的值。有人可能会说volatile不是保证了可见性啊，一个线程对inc的修改，另外一个线程应该立刻看到啊！可是这里的操作inc++是个复合操作啊，包括读取inc的值，对其自增，然后再写回主存。

假设线程A，读取了inc的值为10，这时候被阻塞了，因为没有对变量进行修改，触发不了volatile规则。

线程B此时也读读inc的值，主存里inc的值依旧为10，做自增，然后立刻就被写回主存了，为11。

此时又轮到线程A执行，由于工作内存里保存的是10，所以继续做自增，再写回主存，11又被写了一遍。所以虽然两个线程执行了两次increase()，结果却只加了一次。

有人说，**volatile不是会使缓存行无效的吗**？但是这里线程A读取到线程B也进行操作之前，并没有修改inc值，所以线程B读取的时候，还是读的10。

又有人说，线程B将11写回主存，**不会把线程A的缓存行设为无效吗**？但是线程A的读取操作已经做过了啊，只有在做读取操作时，发现自己缓存行无效，才会去读主存的值，所以这里线程A只能继续做自增了。

综上所述，在这种复合操作的情景下，原子性的功能是维持不了了。但是volatile在上面那种设置flag值的例子里，由于对flag的读/写操作都是单步的，所以还是能保证原子性的。

要想保证原子性，只能借助于synchronized,Lock以及并发包下的atomic的原子操作类了，即对基本数据类型的 自增（加1操作），自减（减1操作）、以及加法操作（加一个数），减法操作（减一个数）进行了封装，保证这些操作是原子性操作。

# 4. volatile的底层实现机制

如果把加入volatile关键字的代码和未加入volatile关键字的代码都生成汇编代码，会发**现加入volatile关键字的代码会多出一个lock前缀指令**。lock前缀指令实际相当于一个内存屏障，内存屏障提供了以下功能：

> 1 . 重排序时不能把后面的指令重排序到内存屏障之前的位置 
>
> 2 . 使得本CPU的Cache写入内存 
>
> 3 . 写入动作也会引起别的CPU或者别的内核无效化其Cache，相当于让新写入的值对别的线程可见。

# 5. 使用volatile关键字的场景

synchronized关键字是防止多个线程同时执行一段代码，那么就会很影响程序执行效率，而volatile关键字在某些情况下性能要优于synchronized，但是要注意volatile关键字是无法替代synchronized关键字的，因为volatile关键字无法保证操作的原子性。通常来说，使用volatile必须具备以下2个条件：

* 对变量的写操作不依赖于当前值

* 该变量没有包含在具有其他变量的不变式中

实际上，这些条件表明，可以被写入 volatile 变量的这些有效值独立于任何程序的状态，包括变量的当前状态。

事实上，我的理解就是上面的2个条件需要保证操作是原子性操作，才能保证使用volatile关键字的程序在并发时能够正确执行。

下面列举几个Java中使用volatile的几个场景。

状态量标记，就如上面对flag的标记：

```java
int a = 0;
volatile bool flag = false;

public void write() {
    a = 2;              //1
    flag = true;        //2
}

public void multiply() {
    if (flag) {         //3
        int ret = a * a;//4
    }
}
```

这种对变量的读写操作，标记为volatile可以保证修改对线程立刻可见。

单例模式的实现，典型的双重检查锁定（DCL）:

```java
class Singleton{
    private volatile static Singleton instance = null;
 
    private Singleton() {
 
    }
 
    public static Singleton getInstance() {
        if(instance==null) {
            synchronized (Singleton.class) {
                if(instance==null)
                    instance = new Singleton();
            }
        }
        return instance;
    }
}
```

这是一种懒汉的单例模式，使用时才创建对象，而且为了避免初始化操作的指令重排序，给instance加上了volatile。

> 本博客部分内容来自https://juejin.im/post/5a2b53b7f265da432a7b821c

# 双重检验锁方式实现单例模式原理

单例模式大概是Java编程中最常用的设计模式之一了，之前也有文章说过什么是单例模式，链接如下：

https://blog.csdn.net/weixin_39309402/article/details/98126883

虽然这篇文章中也分析了如何利用同步锁机制保证懒汉式单例模式的线程安全问题，同步方法，同步代码块等，但都非最优的解决方法，今天我们就来讲讲什么是双重检验锁方式实现单例模式，包括它的特点和原理。

```java
/**

 * 双重检验锁方式实现单例模式
   */
   public class DualLazySingleTon {
   // 静态实例变量
   private volatile static DualLazySingleTon instance;

   // 私有化构造函数
   private DualLazySingleTon() {
   	System.out.println(Thread.currentThread().getName() + "\t" + "进入构造方法");
   }

   // 静态public方法，向整个应用提供单例获取方式
   public static DualLazySingleTon getInstance() {
   	if (instance == null) { //第1重判断
   		synchronized (DualLazySingleTon.class) {
   			if (instance == null) { //第2重判断
   				instance = new DualLazySingleTon();	
   			}
   		}
   	}
   	return instance;
   }

}
/**

 * 查看双重检验锁方式实现单例模式在多线程环境下线程是否安全
   */
   public class DualLazyMyThread extends Thread {

   @Override
   public void run() {
   	System.out.println(DualLazySingleTon.getInstance().hashCode());
   }

   public static void main(String[] args) {

   	DualLazyMyThread[] mts = new DualLazyMyThread[10];
   	for (int i = 0; i < mts.length; i++) {
   		mts[i] = new DualLazyMyThread();
   	}
   	 
   	for (int j = 0; j < mts.length; j++) {
   		mts[j].start();
   	}

   }
   }
```

第一次判断是否为null：
第一次判断是在Synchronized同步代码块外，理由是单例模式只会创建一个实例，并通过getInstance方法返回singleton对象，所以如果已经创建了singleton对象，就不用进入同步代码块，不用竞争锁，直接返回前面创建的实例即可，这样大大提升效率。

第二次判断singleton是否为null：
第二次判断原因是为了保证同步，假若线程A通过了第一次判断，进入了同步代码块，但是还未执行，线程B就进来了（线程B获得CPU时间片），线程B也通过了第一次判断（线程A并未创建实例，所以B通过了第一次判断），准备进入同步代码块，假若这个时候不判断，就会存在这种情况：线程A创建了实例，此时恰好B也获得执行时间片，如果不加以判断，那么线程B也会创建一个实例，就会造成多实例的情况。

所以，为了满足单例模式的要求，双重校验是必不可少的。

声明变量时为什么要用volatile关键字进行修饰？

volatile关键字可以防止jvm指令重排优化，使用了volatile关键字可用来保证其线程间的可见性和有序性；

因为对象的创建并非一步完成，而是需要分为3个步骤执行的，比如：singleton = new Singleton();  

指令1：获取singleton对象的内存地址
指令2：初始化singleton对象
指令3：将内存地址指向引用变量singleton

因为Volatile禁止JVM对指令进行重排序，所以创建对象时会严格按照指令1-2-3的顺序执行，假若如果没有Volatile关键字，单线程环境下不会出现问题，但是在多线程环境下会导致一个线程获得还没有初始化的实例。比如
线程A正常创建一个实例，执行了1-3，**此时线程B调用getInstance()后发现instance不为空，因此会直接返回instance，但此时instance并未被初始化，所以需要用volatile关键字修饰。**
————————————————
版权声明：本文为CSDN博主「南丘xf」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_39309402/article/details/106421795

# ReentrantLock和Synchronized的区别

### 1.Synchronize和ReentrantLock区别

#### 1.1 相似点：

- 这两种同步方式有很多相似之处，它们都是加锁方式同步，而且都是阻塞式的同步，也就是说当如果一个线程获得了对象锁，进入了同步块，其他访问该同步块的线程都必须阻塞在同步块外面等待，而进行线程阻塞和唤醒的代价是比较高的（操作系统需要在用户态与内核态之间来回切换，代价很高，不过可以通过对锁优化进行改善）。

#### 1.2 区别：

##### 1.2.1 API层面

- 这两种方式最大区别就是对于Synchronized来说，它是java语言的关键字，是原生语法层面的互斥，需要jvm实现。而ReentrantLock它是JDK 1.5之后提供的API层面的互斥锁，需要lock()和unlock()方法配合try/finally语句块来完成。

- synchronized既可以修饰方法，也可以修饰代码块。

  ```
  //synchronized修饰一个方法时，这个方法叫同步方法。
  public synchronized void test() {
  //方法体``
  
  }
  
  synchronized（Object） {
  //括号中表示需要锁的对象.
  //线程执行的时候会对Object上锁
  }
  复制代码
  ```

- ReentrantLock使用

  ```
  private ReentrantLock lock = new ReentrantLock();
  public void run() {
      lock.lock();
      try{
          for(int i=0;i<5;i++){
              System.out.println(Thread.currentThread().getName()+":"+i);
          }
      }finally{
          lock.unlock();
      }
  }
  复制代码
  ```

##### 1.2.2 等待可中断

- 等待可中断是指当持有锁的线程长期不释放锁的时候，正在等待的线程可以选择放弃等待，改为处理其他事情。可等待特性对处理执行时间非常长的同步快很有帮助。
- 具体来说，假如业务代码中有两个线程，Thread1 Thread2。假设 Thread1 获取了对象object的锁，Thread2将等待Thread1释放object的锁。
  - 使用synchronized。如果Thread1不释放，Thread2将一直等待，不能被中断。synchronized也可以说是Java提供的原子性内置锁机制。内部锁扮演了互斥锁（mutual exclusion lock ，mutex）的角色，一个线程引用锁的时候，别的线程阻塞等待。
  - 使用ReentrantLock。如果Thread1不释放，Thread2等待了很长时间以后，可以中断等待，转而去做别的事情。

##### 1.2.3 公平锁

- 公平锁是指多个线程在等待同一个锁时，必须按照申请的时间顺序来依次获得锁；而非公平锁则不能保证这一点。非公平锁在锁被释放时，任何一个等待锁的线程都有机会获得锁。
- synchronized的锁是非公平锁，ReentrantLock默认情况下也是非公平锁，但可以通过带布尔值的构造函数要求使用公平锁。
  - ReentrantLock 构造器的一个参数是boolean值，它允许您选择想要一个公平（fair）锁，还是一个不公平（unfair）锁。公平锁：使线程按照请求锁的顺序依次获得锁, 但是有成本;不公平锁：则允许讨价还价
  - 那么如何用代码设置公平锁呢？如下所示
  - ![image](https://user-gold-cdn.xitu.io/2018/10/18/16687054177950c6?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

##### 1.2.4 锁绑定多个条件

- ReentrantLock可以同时绑定多个Condition对象，只需多次调用newCondition方法即可。
- synchronized中，锁对象的wait()和notify()或notifyAll()方法可以实现一个隐含的条件。但如果要和多于一个的条件关联的时候，就不得不额外添加一个锁。

#### 1.3 什么是线程安全问题？如何理解

- 如果你的代码所在的进程中有多个线程在同时运行，而这些线程可能会同时运行这段代码。如果每次运行结果和单线程运行的结果是一样的，而且其他的变量的值也和预期的是一样的，就是线程安全的，或者说:一个类或者程序所提供的接口对于线程来说是原子操作或者多个线程之间的切换不会导致该接口的执行结果存在二义性,也就是说我们不用考虑同步的问题 。

#### 1.4 线程安全需要保证几个基本特性

- 1、原子性，简单说就是相关操作不会中途被其他线程干扰，一般通过同步机制实现。
- 2、可见性，是一个线程修改了某个共享变量，其状态能够立即被其他线程知晓，通常被解释为将线程本地状态反映到主内存上，volatile 就是负责保证可见性的。
- 3、有序性，是保证线程内串行语义，避免指令重排等。

### 2.Synchronize在编译时如何实现锁机制

- Synchronized进过编译，会在同步块的前后分别形成monitorenter和monitorexit这个两个字节码指令。在执行monitorenter指令时，首先要尝试获取对象锁。如果这个对象没被锁定，或者当前线程已经拥有了那个对象锁，把锁的计算器加1，相应的，在执行monitorexit指令时会将锁计算器就减1，当计算器为0时，锁就被释放了。如果获取对象锁失败，那当前线程就要阻塞，直到对象锁被另一个线程释放为止。

### 3.ReentrantLock使用方法

- ReentrantLock是java.util.concurrent包下提供的一套互斥锁，相比Synchronized，ReentrantLock类提供了一些高级功能，主要有以下3项：

  - 1.等待可中断，持有锁的线程长期不释放的时候，正在等待的线程可以选择放弃等待，这相当于Synchronized来说可以避免出现死锁的情况。
  - 2.公平锁，多个线程等待同一个锁时，必须按照申请锁的时间顺序获得锁，Synchronized锁非公平锁，ReentrantLock默认的构造函数是创建的非公平锁，可以通过参数true设为公平锁，但公平锁表现的性能不是很好。
  - 3.锁绑定多个条件，一个ReentrantLock对象可以同时绑定对个对象。

- 使用方法代码如下

  ```
  private ReentrantLock lock = new ReentrantLock();
  public void run() {
      lock.lock();
      try{
          for(int i=0;i<5;i++){
              System.out.println(Thread.currentThread().getName()+":"+i);
          }
      }finally{
          lock.unlock();
      }
  }
  复制代码
  ```

- 注意问题：为保证锁释放，每一个 lock() 动作，建议都立即对应一都立即对应一个 try-catch-finally

### 4.ReentrantLock锁机制测试案例分析

#### 4.1 代码案例分析

- 代码如下所示

  ```
  private void test2() {
      Runnable t1 = new MyThread();
      new Thread(t1,"t1").start();
      new Thread(t1,"t2").start();
  }
  
  class MyThread implements Runnable {
      private ReentrantLock lock = new ReentrantLock();
      public void run() {
          lock.lock();
          try{
              for(int i=0;i<5;i++){
                  System.out.println(Thread.currentThread().getName()+":"+i);
              }
          }finally{
              lock.unlock();
          }
      }
  }
  
  //打印值如下所示
  10-17 17:06:59.222 6531-6846/com.yc.cn.ycbaseadapter I/System.out: t1:0
  10-17 17:06:59.222 6531-6846/com.yc.cn.ycbaseadapter I/System.out: t1:1
  10-17 17:06:59.222 6531-6846/com.yc.cn.ycbaseadapter I/System.out: t1:2
  10-17 17:06:59.222 6531-6846/com.yc.cn.ycbaseadapter I/System.out: t1:3
  10-17 17:06:59.222 6531-6846/com.yc.cn.ycbaseadapter I/System.out: t1:4
  10-17 17:06:59.224 6531-6847/com.yc.cn.ycbaseadapter I/System.out: t2:0
  10-17 17:06:59.225 6531-6847/com.yc.cn.ycbaseadapter I/System.out: t2:1
  10-17 17:06:59.225 6531-6847/com.yc.cn.ycbaseadapter I/System.out: t2:2
  10-17 17:06:59.225 6531-6847/com.yc.cn.ycbaseadapter I/System.out: t2:3
  10-17 17:06:59.225 6531-6847/com.yc.cn.ycbaseadapter I/System.out: t2:4
  复制代码
  ```

#### 4.2 什么时候选择用ReentrantLock

- 适用场景：时间锁等候、可中断锁等候、无块结构锁、多个条件变量或者锁投票

- 在确实需要一些 synchronized所没有的特性的时候，比如时间锁等候、可中断锁等候、无块结构锁、多个条件变量或者锁投票。 ReentrantLock 还具有可伸缩性的好处，应当在高度争用的情况下使用它，但是请记住，大多数 synchronized 块几乎从来没有出现过争用，所以可以把高度争用放在一边。我建议用 synchronized 开发，直到确实证明 synchronized 不合适，而不要仅仅是假设如果使用 ReentrantLock “性能会更好”。请记住，这些是供高级用户使用的高级工具。（而且，真正的高级用户喜欢选择能够找到的最简单工具，直到他们认为简单的工具不适用为止。）。一如既往，首先要把事情做好，然后再考虑是不是有必要做得更快。

- 使用场景代码展示【摘自ThreadPoolExecutor类，这个类中很多地方用到了这个锁。自己可以查看】：

  ```
  /**
   * Rolls back the worker thread creation.
   * - removes worker from workers, if present
   * - decrements worker count
   * - rechecks for termination, in case the existence of this
   *   worker was holding up termination
   */
  private void addWorkerFailed(Worker w) {
      final ReentrantLock mainLock = this.mainLock;
      mainLock.lock();
      try {
          if (w != null)
              workers.remove(w);
          decrementWorkerCount();
          tryTerminate();
      } finally {
          mainLock.unlock();
      }
  }
  复制代码
  ```

#### 4.3 公平锁和非公平锁有何区别

- 公平性是指在竞争场景中，当公平性为真时，会倾向于将锁赋予等待时间最久的线程。公平性是减少线程“饥饿”（个别线程长期等待锁，但始终无法获取）情况发生的一个办法。

  - 1、公平锁能保证：老的线程排队使用锁，新线程仍然排队使用锁。
  - 2、非公平锁保证：老的线程排队使用锁；但是无法保证新线程抢占已经在排队的线程的锁。
  - 看下面代码案例所示：可以得出结论，公平锁指的是哪个线程先运行，那就可以先得到锁。非公平锁是不管线程是否是先运行，新的线程都有可能抢占已经在排队的线程的锁。

  ```
  private void test3() {
      Service service = new Service();
      ThreadClass tcArray[] = new ThreadClass[10];
      for(int i=0;i<10;i++){
          tcArray[i] = new ThreadClass(service);
          tcArray[i].start();
      }
  }
  
  public class Service {
      ReentrantLock lock = new ReentrantLock(true);
      Service() {
      }
  
      void getThreadName() {
          System.out.println(Thread.currentThread().getName() + " 已经被锁定");
      }
  }
  public class ThreadClass extends Thread{
      private Service service;
      ThreadClass(Service service) {
          this.service = service;
      }
      public void run(){
          System.out.println(Thread.currentThread().getName() + " 抢到了锁");
          service.lock.lock();
          service.getThreadName();
          service.lock.unlock();
      }
  }
  //当ReentrantLock设置true，也就是公平锁时
  10-17 19:32:22.422 6459-6523/com.yc.cn.ycbaseadapter I/System.out: Thread-5 抢到了锁
  10-17 19:32:22.422 6459-6523/com.yc.cn.ycbaseadapter I/System.out: Thread-5 已经被锁定
  10-17 19:32:22.424 6459-6524/com.yc.cn.ycbaseadapter I/System.out: Thread-6 抢到了锁
  10-17 19:32:22.424 6459-6524/com.yc.cn.ycbaseadapter I/System.out: Thread-6 已经被锁定
  10-17 19:32:22.427 6459-6525/com.yc.cn.ycbaseadapter I/System.out: Thread-7 抢到了锁
  10-17 19:32:22.427 6459-6526/com.yc.cn.ycbaseadapter I/System.out: Thread-8 抢到了锁
  10-17 19:32:22.427 6459-6525/com.yc.cn.ycbaseadapter I/System.out: Thread-7 已经被锁定
  10-17 19:32:22.427 6459-6526/com.yc.cn.ycbaseadapter I/System.out: Thread-8 已经被锁定
  10-17 19:32:22.427 6459-6527/com.yc.cn.ycbaseadapter I/System.out: Thread-9 抢到了锁
  10-17 19:32:22.427 6459-6527/com.yc.cn.ycbaseadapter I/System.out: Thread-9 已经被锁定
  10-17 19:32:22.428 6459-6528/com.yc.cn.ycbaseadapter I/System.out: Thread-10 抢到了锁
  10-17 19:32:22.428 6459-6528/com.yc.cn.ycbaseadapter I/System.out: Thread-10 已经被锁定
  10-17 19:32:22.429 6459-6529/com.yc.cn.ycbaseadapter I/System.out: Thread-11 抢到了锁
  10-17 19:32:22.429 6459-6529/com.yc.cn.ycbaseadapter I/System.out: Thread-11 已经被锁定
  10-17 19:32:22.430 6459-6530/com.yc.cn.ycbaseadapter I/System.out: Thread-12 抢到了锁
  10-17 19:32:22.430 6459-6530/com.yc.cn.ycbaseadapter I/System.out: Thread-12 已经被锁定
  10-17 19:32:22.431 6459-6532/com.yc.cn.ycbaseadapter I/System.out: Thread-14 抢到了锁
  10-17 19:32:22.431 6459-6532/com.yc.cn.ycbaseadapter I/System.out: Thread-14 已经被锁定
  10-17 19:32:22.432 6459-6531/com.yc.cn.ycbaseadapter I/System.out: Thread-13 抢到了锁
  10-17 19:32:22.433 6459-6531/com.yc.cn.ycbaseadapter I/System.out: Thread-13 已经被锁定
  
  
  //当ReentrantLock设置false，也就是非公平锁时
  10-17 19:34:58.102 7089-7183/com.yc.cn.ycbaseadapter I/System.out: Thread-5 抢到了锁
  10-17 19:34:58.102 7089-7184/com.yc.cn.ycbaseadapter I/System.out: Thread-6 抢到了锁
  10-17 19:34:58.103 7089-7183/com.yc.cn.ycbaseadapter I/System.out: Thread-5 已经被锁定
  10-17 19:34:58.103 7089-7185/com.yc.cn.ycbaseadapter I/System.out: Thread-7 抢到了锁
  10-17 19:34:58.103 7089-7185/com.yc.cn.ycbaseadapter I/System.out: Thread-7 已经被锁定
  10-17 19:34:58.103 7089-7184/com.yc.cn.ycbaseadapter I/System.out: Thread-6 已经被锁定
  10-17 19:34:58.104 7089-7186/com.yc.cn.ycbaseadapter I/System.out: Thread-8 抢到了锁
  10-17 19:34:58.105 7089-7186/com.yc.cn.ycbaseadapter I/System.out: Thread-8 已经被锁定
  10-17 19:34:58.108 7089-7187/com.yc.cn.ycbaseadapter I/System.out: Thread-9 抢到了锁
  10-17 19:34:58.108 7089-7187/com.yc.cn.ycbaseadapter I/System.out: Thread-9 已经被锁定
  10-17 19:34:58.111 7089-7188/com.yc.cn.ycbaseadapter I/System.out: Thread-10 抢到了锁
  10-17 19:34:58.112 7089-7188/com.yc.cn.ycbaseadapter I/System.out: Thread-10 已经被锁定
  10-17 19:34:58.112 7089-7189/com.yc.cn.ycbaseadapter I/System.out: Thread-11 抢到了锁
  10-17 19:34:58.113 7089-7189/com.yc.cn.ycbaseadapter I/System.out: Thread-11 已经被锁定
  10-17 19:34:58.113 7089-7193/com.yc.cn.ycbaseadapter I/System.out: Thread-14 抢到了锁
  10-17 19:34:58.113 7089-7193/com.yc.cn.ycbaseadapter I/System.out: Thread-14 已经被锁定
  10-17 19:34:58.115 7089-7190/com.yc.cn.ycbaseadapter I/System.out: Thread-12 抢到了锁
  10-17 19:34:58.115 7089-7190/com.yc.cn.ycbaseadapter I/System.out: Thread-12 已经被锁定
  10-17 19:34:58.116 7089-7191/com.yc.cn.ycbaseadapter I/System.out: Thread-13 抢到了锁
  10-17 19:34:58.116 7089-7191/com.yc.cn.ycbaseadapter I/System.out: Thread-13 已经被锁定
  复制代码
  ```

### 5.问答测试题

#### 5.1 ReentrantLock和synchronized使用分析

- ReentrantLock是Lock的实现类，是一个互斥的同步器，在多线程高竞争条件下，ReentrantLock比synchronized有更加优异的性能表现。
- 1 用法比较
  - Lock使用起来比较灵活，但是必须有释放锁的配合动作
  - Lock必须手动获取与释放锁，而synchronized不需要手动释放和开启锁
  - Lock只适用于代码块锁，而synchronized可用于修饰方法、代码块等
- 2 特性比较
  - ReentrantLock的优势体现在：
    - 具备尝试非阻塞地获取锁的特性：当前线程尝试获取锁，如果这一时刻锁没有被其他线程获取到，则成功获取并持有锁
    - 能被中断地获取锁的特性：与synchronized不同，获取到锁的线程能够响应中断，当获取到锁的线程被中断时，中断异常将会被抛出，同时锁会被释放
    - 超时获取锁的特性：在指定的时间范围内获取锁；如果截止时间到了仍然无法获取锁，则返回
- 3 注意事项
  - 在使用ReentrantLock类的时，一定要注意三点：
    - 在finally中释放锁，目的是保证在获取锁之后，最终能够被释放
    - 不要将获取锁的过程写在try块内，因为如果在获取锁时发生了异常，异常抛出的同时，也会导致锁无故被释放。
    - ReentrantLock提供了一个newCondition的方法，以便用户在同一锁的情况下可以根据不同的情况执行等待或唤醒的动作。

# Java中创建线程的方式

Java中创建线程主要有三种方式：

一、继承Thread类创建线程类

（1）定义Thread类的子类，并重写该类的run方法，该run方法的方法体就代表了线程要完成的任务。因此把run()方法称为执行体。

（2）创建Thread子类的实例，即创建了线程对象。

（3）调用线程对象的start()方法来启动该线程。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
package com.nf147.Constroller;

public class FirstThreadTest extends Thread {

    int i = 0;

    //重写run方法，run方法的方法体就是现场执行体
    public void run() {
        for (; i < 100; i++) {
            System.out.println(getName() + "  " + i);
        }
    }

    public static void main(String[] args) {

        for (int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName() + "  : " + i);
            if (i == 50) {
                new FirstThreadTest().start();
                new FirstThreadTest().start();
            }
        }
    }


}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

上述代码中Thread.currentThread()方法返回当前正在执行的线程对象。GetName()方法返回调用该方法的线程的名字。

 

 

 

 

二、通过Runnable接口创建线程类

 

（1）定义runnable接口的实现类，并重写该接口的run()方法，该run()方法的方法体同样是该线程的线程执行体。

 

（2）创建 Runnable实现类的实例，并依此实例作为Thread的target来创建Thread对象，该Thread对象才是真正的线程对象。

 

（3）调用线程对象的start()方法来启动该线程。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
package com.nf147.Constroller;

public class RunnableThreadTest implements Runnable{
        private int i;
        public void run()
        {
            for(i = 0;i <100;i++)
            {
                System.out.println(Thread.currentThread().getName()+" "+i);
            }
        }
        public static void main(String[] args)
        {
            for(int i = 0;i < 100;i++)
            {
                System.out.println(Thread.currentThread().getName()+" "+i);
                if(i==20)
                {
                    RunnableThreadTest rtt = new RunnableThreadTest();
                    new Thread(rtt,"新线程1").start();
                    new Thread(rtt,"新线程2").start();
                }
            }

        }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

 

三、通过Callable和Future创建线程

 

（1）创建Callable接口的实现类，并实现call()方法，该call()方法将作为线程执行体，并且有返回值。

 

（2）创建Callable实现类的实例，使用FutureTask类来包装Callable对象，该FutureTask对象封装了该Callable对象的call()方法的返回值。

 

（3）使用FutureTask对象作为Thread对象的target创建并启动新线程。

 

（4）调用FutureTask对象的get()方法来获得子线程执行结束后的返回值

 

实例代码：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
package com.nf147.Constroller;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

public class CallableThreadTest implements Callable<Integer> {


    public static void main(String[] args) {
        CallableThreadTest ctt = new CallableThreadTest();
        FutureTask<Integer> ft = new FutureTask<>(ctt);
        for (int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName() + " 的循环变量i的值" + i);
            if (i == 20) {
                new Thread(ft, "有返回值的线程").start();
            }
        }
        try {
            System.out.println("子线程的返回值：" + ft.get());
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }

    }

    @Override
    public Integer call() throws Exception {
        int i = 0;
        for (; i < 100; i++) {
            System.out.println(Thread.currentThread().getName() + " " + i);
        }
        return i;
    }


}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

二、创建线程的三种方式的对比

 

采用实现Runnable、Callable接口的方式创见多线程时，优势是：

 

线程类只是实现了Runnable接口或Callable接口，还可以继承其他类。

 

在这种方式下，多个线程可以共享同一个target对象，所以非常适合多个相同线程来处理同一份资源的情况，从而可以将CPU、代码和数据分开，形成清晰的模型，较好地体现了面向对象的思想。

 

劣势是：

 

编程稍微复杂，如果要访问当前线程，则必须使用Thread.currentThread()方法。

 

使用继承Thread类的方式创建多线程时优势是：

 

编写简单，如果需要访问当前线程，则无需使用Thread.currentThread()方法，直接使用this即可获得当前线程。

 

劣势是：

 

线程类已经继承了Thread类，所以不能再继承其他父类。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
package com.nf147.Constroller;

public class FirstThreadTest extends Thread {

    int i = 0;

    //重写run方法，run方法的方法体就是现场执行体
public void run() {
        for (; i < 100; i++) {
            System.out.println(getName() + "  " + i);
        }
    }

    public static void main(String[] args) {

        for (int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName() + "  : " + i);
            if (i == 50) {
                new FirstThreadTest().start();
                new FirstThreadTest().start();
            }
        }
    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

# HashMap线程不安全的体现

根据其他信息整理而来：

哈希碰撞和扩容导致的线程不安全！！

回答：HashMap的实现里没有锁的机制，因此它是线程不安全的。

其实只要有锁的机制，可以通过锁实现线程安全，我们在读写HashMap对象的时候加锁，以保障这个对象的线程安全，但不代表HashMap本身是线程安全的，因为是外力（你自己加的锁）使然。

为啥不在HashMap内部加锁让它变成线程安全？
这样会增加单线程访问的资源消耗，即使没有多线程访问，也要每次检查、加锁、解锁。
实际上有线程安全的Map，Collections里面有个静态方法可以返回一个线程安全版本的HashMap

hashMap出现线程不安全的表现：

表现1：
多个线程同时操作一个hashmap就可能出现不安全的情况：

比如A B两个线程(A线程获数据 B线程存数据) 同时操作myHashMap
1. B线程执行存放数据
modelHashMap.put("1","2");
2. A线程执行get获取数据
modelHashMap.get("1");
A线程获取的值本来应该是2，但是如果A线程在刚到达获取的动作还没执行的时候，线程执行的机会又跳到线程B，此时线程B又对modelHashMap赋值 如：modelHashMap.put("1","3");
然后线程虚拟机又执行线程A，A取到的值为3，这样map中第一个存放的值 就会丢失。
要保证值的准确，就要保证操作的原子性，就是保证A操作从头开始不能被打断。。所有要用同步关键字，或者使用java 1.5中的current新包中的ConcurrentHashMap,这是线程安全的，在java最新的并发包中，对之前非线程安全的工具，如hashMap List 都做了同步封转。

表现2：
一般我们声明HashMap时，使用的都是默认的构造方法：HashMap< K,V >，看了代码你会发现，它还有其它的构造方法：HashMap(int initialCapacity, float loadFactor)，其中参数initialCapacity为初始容量，loadFactor为加载因子，而之前我们看到的threshold = (int)(capacity * loadFactor);如果在默认情况下，一个HashMap的容量为16，加载因子为0.75，那么阀值就是12，所以在往HashMap中put的值到达12时，它将自动扩容两倍，如果两个线程同时遇到HashMap的大小达到12的倍数时，就很有可能会出现在将oldTable转移到newTable的过程中遇到问题，从而导致最终的HashMap的值存储异常。

表现3：
构造entry<K,V>单链表时，也会出现不安全的情况。

深度讲解：

resize死循环
我们都知道HashMap初始容量大小为16,一般来说，当有数据要插入时，都会检查容量有没有超过设定的thredhold，如果超过，需要增大Hash表的尺寸，但是这样一来，整个Hash表里的元素都需要被重算一遍。这叫rehash，这个成本相当的大。

```java
void resize(int newCapacity) {
        Entry[] oldTable = table;
        int oldCapacity = oldTable.length;
        if (oldCapacity == MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return;
        }

        Entry[] newTable = new Entry[newCapacity];
        transfer(newTable, initHashSeedAsNeeded(newCapacity));
        table = newTable;
        threshold = (int)Math.min(newCapacity * loadFactor, MAXIMUM_CAPACITY + 1);

}

void transfer(Entry[] newTable, boolean rehash) {
        int newCapacity = newTable.length;
        for (Entry<K,V> e : table) {
            while(null != e) {
                Entry<K,V> next = e.next;
                if (rehash) {
                    e.hash = null == e.key ? 0 : hash(e.key);
                }
                int i = indexFor(e.hash, newCapacity);
                e.next = newTable[i];
                newTable[i] = e;
                e = next;
            }
        }
}
```



大概看下transfer：

对索引数组中的元素遍历
对链表上的每一个节点遍历：用 next 取得要转移那个元素的下一个，将 e 转移到新 Hash 表的头部，使用头插法插入节点。
循环2，直到链表节点全部转移
循环1，直到所有索引数组全部转移
经过这几步，会发现转移的时候是逆序的。假如转移前链表顺序是1->2->3，那么转移后就会变成3->2->1。这时候就有点头绪了，死锁问题不就是因为1->2的同时2->1造成的吗？所以，HashMap 的死锁问题就出在这个transfer()函数上。

1.1 单线程 rehash 详细演示
单线程情况下，rehash 不会出现任何问题：

假设hash算法就是最简单的 key mod table.length（也就是数组的长度）。
最上面的是old hash 表，其中的Hash表的 size = 2, 所以 key = 3, 7, 5，在 mod 2以后碰撞发生在 table[1]
接下来的三个步骤是 Hash表 resize 到4，并将所有的 < key,value > 重新rehash到新 Hash 表的过程
如图所示：

1.2 多线程 rehash 详细演示
为了思路更清晰，我们只将关键代码展示出来

```java
while(null != e) {
    Entry<K,V> next = e.next;
    e.next = newTable[i];
    newTable[i] = e;
    e = next;
}
```

Entry<K,V> next = e.next;——因为是单链表，如果要转移头指针，一定要保存下一个结点，不然转移后链表就丢了
e.next = newTable[i];——e 要插入到链表的头部，所以要先用 e.next 指向新的 Hash 表第一个元素（为什么不加到新链表最后？因为复杂度是 O（N））
newTable[i] = e;——现在新 Hash 表的头指针仍然指向 e 没转移前的第一个元素，所以需要将新 Hash 表的头指针指向 e
e = next——转移 e 的下一个结点
假设这里有两个线程同时执行了put()操作，并进入了transfer()环节

```java
while(null != e) {
    Entry<K,V> next = e.next; //线程1执行到这里被调度挂起了
    e.next = newTable[i];
    newTable[i] = e;
    e = next;
}
```


那么现在的状态为：


从上面的图我们可以看到，因为线程1的 e 指向了 key(3)，而 next 指向了 key(7)，在线程2 rehash 后，就指向了线程2 rehash 后的链表。

然后线程1被唤醒了：

执行e.next = newTable[i]，于是 key(3)的 next 指向了线程1的新 Hash 表，因为新 Hash 表为空，所以e.next = null，
执行newTable[i] = e，所以线程1的新 Hash 表第一个元素指向了线程2新 Hash 表的 key(3)。好了，e 处理完毕。
执行e = next，将 e 指向 next，所以新的 e 是 key(7)
然后该执行 key(3)的 next 节点 key(7)了:

现在的 e 节点是 key(7)，首先执行Entry< K,V > next = e.next,那么 next 就是 key(3)了
执行e.next = newTable[i]，于是key(7) 的 next 就成了 key(3)
执行newTable[i] = e，那么线程1的新 Hash 表第一个元素变成了 key(7)
执行e = next，将 e 指向 next，所以新的 e 是 key(3)
这时候的状态图为：


然后又该执行 key(7)的 next 节点 key(3)了：

现在的 e 节点是 key(3)，首先执行Entry< K,V > next = e.next,那么 next 就是 null
执行e.next = newTable[i]，于是key(3) 的 next 就成了 key(7)
执行newTable[i] = e，那么线程1的新 Hash 表第一个元素变成了 key(3)
执行e = next，将 e 指向 next，所以新的 e 是 key(7)
这时候的状态如图所示：



很明显，环形链表出现了！！当然，现在还没有事情，因为下一个节点是 null，所以transfer()就完成了，等put()的其余过程搞定后，HashMap 的底层实现就是线程1的新 Hash 表了。
————————————————
版权声明：本文为CSDN博主「Java星」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/yangxingpa/article/details/81056306

# 强引用、软引用、弱引用、幻象引用的区别

## 1. 强引用

​	所谓强引用（“Strong” Reference），就是我们最常见的普通对象引用，**只要还有强引用指向一个对象，就能表明对象还“活着”，垃圾收集器不会碰这种对象**。对于一个普通的对象，如果没有其他的引用关系，只要超过了引用的作用域或者显式地将相应（强）引用赋值为 null，就是可以被垃圾收集的了，当然具体回收时机还是要看垃圾收集策略。

​	我们来看一个代码示例：

```java
A a = new A();//a为强引用
a = null;//给a赋值为null，gc就会回收a
```

​	下面这个代码示例稍有不同：

```java
A a = new A(); //a为强引用
B b = new B(a); //b为强引用，并且b依赖于a
a = null; //给a赋值为null，但此时a还不能被回收，因为b还依赖于a，这样就会造成内存泄漏
```

## 2. 软引用

​	软引用（SoftReference），是一种相对强引用弱化一些的引用，可以让对象豁免一些垃圾收集，**只有当 JVM 认为内存不足时，才会去试图回收软引用指向的对象。**JVM 会确保在抛出 OutOfMemoryError 之前，清理软引用指向的对象。**软引用通常用来实现内存敏感的缓存**，如果还有空闲内存，就可以暂时保留缓存，当内存不足时清理掉，这样就保证了使用缓存的同时，不会耗尽内存。软引用可以和一个引用队列（ReferenceQueue）联合使用，如果软引用所引用的对象被垃圾回收，JAVA虚拟机就会把这个软引用加入到与之关联的引用队列中。 

```java
public class Test {  
    public static void main(String[] args){  
        System.out.println("开始");            
        A a = new A();            
        SoftReference<A> sr = new SoftReference<A>(a); //设置sr为指向对象A的软引用
        a = null; //设置a为null
        if(sr!=null){  
            a = (A)sr.get(); //还没被回收，直接获取
        }  
        else{  //由于内存不够，软引用指向的对象会被直接回收
            a = new A();  
            sr = new SoftReference<A>(a); //重新创建新的软引用
        }            
        System.out.println("结束");     
    }       
}  

class A{  
    int[] a;  
    public A(){  
        a = new int[100000000];  
    }  
}  

```

## 3. 弱引用

​	弱引用（WeakReference）并不能使对象豁免垃圾收集，仅仅是提供一种访问在弱引用状态下对象的途径。一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。不过，由于垃圾回收器是一个优先级很低的线程， 因此不一定会很快发现那些只具有弱引用的对象。 这就可以用来构建一种没有特定约束的关系，比如，维护一种非强制性的映射关系，如果试图获取时对象还在，就使用它，否则重新实例化。**它同样是很多缓存实现的选择**。

```java
public class Car {
  private double price;
  private String colour;
 
  public Car(double price, String colour){
    this.price = price;
    this.colour = colour;
  }
 
  public double getPrice() {
    return price;
  }
  public void setPrice(double price) {
    this.price = price;
  }
  public String getColour() {
    return colour;
  }
  public void setColour(String colour) {
    this.colour = colour;
  }
 
  public String toString(){
    return colour +"car costs $"+price;
  }
}

public class TestWeakReference {
  public static void main(String[] args) {
    Car car = new Car(22000,"silver");
    WeakReference<Car> weakCar = new WeakReference<Car>(car);
    int i=0;
    while(true){
      if(weakCar.get()!=null){
        i++;
        System.out.println("Object is alive for "+i+" loops - "+weakCar);
      }else{
        System.out.println("Object has been collected.");
        break;
      }
    }
  }
}
```

## 4. 幻象引用

​	对于幻象引用，有时候也翻译成虚引用，你不能通过它访问对象。幻象引用仅仅是提供了一种确保对象被 finalize 以后，做某些事情的机制，比如，通常用来做所谓的 Post-Mortem 清理机制。

```java
Object obj = new Object();
PhantomReference<Object> pf = new PhantomReference<Object>(obj);
obj=null;
//永远返回null
pf.get();
//返回是否从内存中已经删除
pf.isEnQueued();　
```



![img](https://i.loli.net/2020/04/21/qGjYn76uWNVT3Zy.jpg)

## 5. 对象可达性分析

<img src="https://i.loli.net/2020/04/21/Kyu69qWA8dY2Lmi.jpg" alt="img" style="zoom:150%;" />	

判断对象可达性，是 JVM 垃圾收集器决定如何处理对象的一部分考虑。

* 强可达（Strongly Reachable），就是当一个对象可以有一个或多个线程可以不通过各种引用访问到的情况。比如，我们新创建一个对象，那么创建它的线程对它就是强可达。
* 软可达（Softly Reachable），就是当我们只能通过软引用才能访问到对象的状态。
* 弱可达（Weakly Reachable），类似前面提到的，就是无法通过强引用或者软引用访问，只能通过弱引用访问时的状态。这是十分临近 finalize 状态的时机，当弱引用被清除的时候，就符合 finalize 的条件了。
* 幻象可达（Phantom Reachable），没有强、软、弱引用关联，并且 finalize 过了，只有幻象引用指向这个对象的时候。
* 当然，还有一个最后的状态，就是不可达（unreachable），意味着对象可以被清除了。

## 6. Reachability Fence

​	我们也可以通过底层 API 来达到强引用的效果，这就是所谓的设置**reachability fence**。

​	为什么需要这种机制呢？考虑一下这样的场景，按照 Java 语言规范，如果一个对象没有指向强引用，就符合垃圾收集的标准，有些时候，对象本身并没有强引用，但是也许它的部分属性还在被使用，这样就导致诡异的问题，所以我们需要一个方法，在没有强引用情况下，通知 JVM 对象是在被使用的。说起来有点绕，我们来看看 Java 9 中提供的案例。

```java
class Resource {
 private static ExternalResource[] externalResourceArray = ...
 int myIndex; 
 Resource(...) {
   myIndex = ...
   externalResourceArray[myIndex] = ...;
     ...
 }
 protected void finalize() {
     externalResourceArray[myIndex] = null;
     ...
 }
 public void action() {
 try {
     // 需要被保护的代码
     int i = myIndex;
     Resource.update(externalResourceArray[i]);
 } finally {
     // 调用 reachbilityFence，明确保障对象 strongly reachable
     Reference.reachabilityFence(this);
 }
 }
 private static void update(ExternalResource ext) {
    ext.status = ...;
 }
} 
```

​	方法 action 的执行，依赖于对象的部分属性，所以被特定保护了起来。否则，如果我们在代码中像下面这样调用，那么就可能会出现困扰，因为没有强引用指向我们创建出来的 Resource 对象，JVM 对它进行 finalize 操作是完全合法的。

## 7. 生存或死亡

​	一个对象要真正“死亡”至少要经历**两次标记**过程：如果对象在进行可达性分析后发现没有与GC Roots相连接的引用链，那它将会被第一次标记并且进行一次筛选，筛选的条件是此对象是否有必要执行finalize()方法。**当对象没有覆盖finalize()方法，或者finalize()方法已经被虚拟机调 用过，虚拟机将这两种情况都视为“没有必要执行”。**

​	如果这个对象被判定为有必要执行finalize()方法，那么这个对象将会放置在一个叫做F-Queue的队列之中，并在稍后被一个由虚拟机自动建立的、低优先级的Finalizer线程去执行它。这里所谓的“执行”是指虚拟机会触发这个方法，但并不承诺会等待它运行结束，这样做的原因是，如果一个对象在finalize()方法中执行缓慢，或者发生了死循环（更极端的情况），将很可能会导致F-Queue队列中其他对象永久处于等待，甚至导致整个内存回收系统奔溃。finalize()方法是对象逃脱死亡命运的最后一次机会，稍后GC将对F-Queue中的对象进行第二次小规模的标记，如果对象要在finalize()中成功拯救自己——只要重新与引用链上的任何一个对象建立关联即可。譬如把自己（this关键字）赋值给某个类变量或者对象的成员变量，那在第二次标记时它将被移除出“即将回收”的集合；如果对象这时候还没有逃脱，那基本上它就真的被回收了。

​	**任何一个对象的finalize()方法都只会被系统自动调用一次，如果对象面临下一次回收，它的finalize()方法不会被再次执行。**

## 8. 总结

​	所有引用类型都是抽象类java.lang.ref.Reference的子类，子类里提供了get()方法。通过上面的分析中可以得知，除了幻象引用（因为get永远返回null），如果对象还没有被销毁，都可以通过get方法获取原有对象。其实有个非常关键的注意点，利用软引用和弱引用，我们可以将访问到的对象，重新指向强引用，也就是人为的改变了对象的可达性状态。所以对于软引用、弱引用之类，垃圾收集器可能会存在二次确认的问题，以确保处于弱引用状态的对象没有改变为强引用。

*本博客部分内容来自杨晓峰的Java核心技术36讲*

# 枚举

http://c.biancheng.net/view/1100.html

https://blog.csdn.net/javazejian/article/details/71333103

# GC roots

***\*什么是是可达性分析算法？\****

现代虚拟机基本都是采用可达性分析算法来判断对象是否存活，可达性算法的原理是以一系列叫做  **GC Root** 的对象为起点出发，引出它们指向的下一个节点，再以下个节点为起点，引出此节点指向的下一个结点。这样通过 GC Root 串成的一条线就叫引用链），直到所有的结点都遍历完毕,如果相关对象不在任意一个以 **GC Root** 为起点的引用链中，则这些对象会被判断为垃圾对象,会被 GC 回收。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9PeXdleXNDU2VMVXJZcVBpY2pWd2p1TUNoUHJQaWNOSGRYdlpMbXg5SGRlTmliUlFqbkZad1VCRlhUUTZtSnk3OWFqeDRCZHFGTWljSWlhVzRCTlFhR1RaV2liZy82NDA?x-oss-process=image/format,png)

如图示，如果用可达性算法即可解决上述循环引用的问题，因为从**GC Root** 出发没有到达 a,b,所以 a，b 可回收。

**a, b 对象可回收，就一定会被回收吗?**

并不是，对象的 finalize 方法给了对象一次垂死挣扎的机会，当对象不可达（可回收）时，当发生GC时，会先判断对象是否执行了 finalize 方法，如果未执行，则会先执行 finalize 方法，我们可以在此方法里将当前对象与 GC Roots 关联，这样执行 finalize 方法之后，GC 会再次判断对象是否可达，如果不可达，则会被回收，如果可达，则不回收！

**注意：** finalize 方法只会被执行一次，如果第一次执行 finalize 方法此对象变成了可达确实不会回收，但如果对象再次被 GC，则会忽略 finalize 方法，对象会被回收！这一点切记!

**GC Roots 到底是什么东西呢，哪些对象可以作为 GC Root 呢？**

- 虚拟机栈（栈帧中的本地变量表）中引用的对象
- 本地方法栈中 JNI（即一般说的 Native 方法）引用的对象
- 方法区中类静态属性引用的对象
- 方法区中常量引用的对象

便于记忆，称他为两栈两方法！下面我们一一介绍一下：

1、虚拟机栈中引用的对象

如下代码所示，a 是栈帧中的本地变量，当 a = null 时，由于此时 a 充当了 **GC Root** 的作用，a 与原来指向的实例 **new Test()** 断开了连接，所以对象会被回收。

```
publicclass Test {



    public static  void main(String[] args) {



	Test a = new Test();



	a = null;



    }



}
```

2、方法区中类静态属性引用的对象

如下代码所示，当栈帧中的本地变量 a = null 时，由于 a 原来指向的对象与 GC Root (变量 a) 断开了连接，所以 a 原来指向的对象会被回收，而由于我们给 s 赋值了变量的引用，s 在此时是类静态属性引用，充当了 GC Root 的作用，它指向的对象依然存活!

```
public class Test {



    public static Test s;



    public static  void main(String[] args) {



	Test a = new Test();



	a.s = new Test();



	a = null;



    }



}
```

3、方法区中常量引用的对象

如下代码所示，常量 s 指向的对象并不会因为 a 指向的对象被回收而回收

```
public class Test {



	public static final Test s = new Test();



        public static void main(String[] args) {



	    Test a = new Test();



	    a = null;



        }



}
```

4、本地方法栈中 JNI 引用的对象

这是简单给不清楚本地方法为何物的童鞋简单解释一下：所谓本地方法就是一个 java 调用非 java 代码的接口，该方法并非 Java 实现的，可能由 C 或 Python等其他语言实现的， Java 通过 JNI 来调用本地方法， 而本地方法是以库文件的形式存放的（在 WINDOWS 平台上是 DLL 文件形式，在 UNIX 机器上是 SO 文件形式）。通过调用本地的库文件的内部方法，使 JAVA 可以实现和本地机器的紧密联系，调用系统级的各接口方法，还是不明白？见文末参考，对本地方法定义与使用有详细介绍。

当调用 Java 方法时，虚拟机会创建一个栈桢并压入 Java 栈，而当它调用的是本地方法时，虚拟机会保持 Java 栈不变，不会在 Java 栈祯中压入新的祯，虚拟机只是简单地动态连接并直接调用指定的本地方法。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9PeXdleXNDU2VMVXJZcVBpY2pWd2p1TUNoUHJQaWNOSGRYdWhiZDlIQ0xCSVlSQmVrUXFpY3M3WEJyajB2VlVWU1BUSFRpYkpidXRyZEdSV2liMU1sRGQxNkpRLzY0MA?x-oss-process=image/format,png)

```
JNIEXPORT void JNICALL Java_com_pecuyu_jnirefdemo_MainActivity_newStringNative(JNIEnv *env, jobject instance，jstring jmsg) {



...



   // 缓存String的class



   jclass jc = (*env)->FindClass(env, STRING_PATH);



}
```

如上代码所示，当 java 调用以上本地方法时，jc 会被本地方法栈压入栈中, jc 就是我们说的本地方法栈中 JNI 的对象引用，因此只会在此本地方法执行完成后才会被释放。

# Java类加载过程

一个Java文件从编码完成到最终执行，一般主要包括两个过程

- 编译
- 运行

**编译**，即把我们写好的java文件，通过javac命令编译成字节码，也就是我们常说的.class文件。

**运行**，则是把编译生成的.class文件交给Java虚拟机(JVM)执行。

而我们所说的类加载过程即是指JVM虚拟机把.class文件中类信息加载进内存，并进行解析生成对应的class对象的过程。

举个通俗点的例子来说，JVM在执行某段代码时，遇到了class A， 然而此时内存中并没有class A的相关信息，于是JVM就会到相应的class文件中去寻找class A的类信息，并加载进内存中，这就是我们所说的类加载过程。

由此可见，JVM不是一开始就把所有的类都加载进内存中，而是只有第一次遇到某个需要运行的类时才会加载，且**只加载一次**。

## **类加载**

类加载的过程主要分为三个部分：

- 加载
- 链接
- 初始化

而链接又可以细分为三个小部分：

- 验证
- 准备
- 解析



![img](https://pic4.zhimg.com/80/v2-ecf6c3d0f5146029e9693d6223d23afb_720w.jpg)



## **加载**

简单来说，加载指的是把class字节码文件从各个来源通过类加载器装载入内存中。

这里有两个重点：

- **字节码来源**。一般的加载来源包括从本地路径下编译生成的.class文件，从jar包中的.class文件，从远程网络，以及动态代理实时编译
- **类加载器**。一般包括**启动类加载器**，**扩展类加载器**，**应用类加载器**，以及用户的**自定义类加载器**。

**注：为什么会有自定义类加载器？**

- 一方面是由于java代码很容易被反编译，如果需要对自己的代码加密的话，可以对编译后的代码进行加密，然后再通过实现自己的自定义类加载器进行解密，最后再加载。
- 另一方面也有可能从非标准的来源加载代码，比如从网络来源，那就需要自己实现一个类加载器，从指定源进行加载。

## **验证**

主要是为了**保证加载进来的字节流符合虚拟机规范**，不会造成安全错误。

包括对于**文件格式的验证**，比如常量中是否有不被支持的常量？文件中是否有不规范的或者附加的其他信息？

对于**元数据的验证**，比如该类是否继承了被final修饰的类？类中的字段，方法是否与父类冲突？是否出现了不合理的重载？

对于**字节码的验证**，保证程序语义的合理性，比如要保证类型转换的合理性。

对于**符号引用的验证**，比如校验符号引用中通过全限定名是否能够找到对应的类？校验符号引用中的访问性（private，public等）是否可被当前类访问？

## **准备**

主要是为类变量（注意，不是实例变量）分配内存，并且赋予**初值**。

特别需要注意，**初值，不是代码中具体写的初始化的值**，而是Java虚拟机根据不同变量类型的默认初始值。

比如8种**基本类型**的初值，默认为0；**引用类型**的初值则为null；**常量**的初值即为代码中设置的值，final static tmp = 456， 那么该阶段tmp的初值就是456

## **解析**

将常量池内的符号引用替换为直接引用的过程。

两个重点：

- **符号引用**。即一个字符串，但是这个字符串给出了一些能够唯一性识别一个方法，一个变量，一个类的相关信息。
- **直接引用**。可以理解为一个内存地址，或者一个偏移量。比如**类方法，类变量**的直接引用是指向**方法区**的**指针**；而**实例方法，实例变量**的直接引用则是从实例的头指针开始算起到这个实例变量位置的**偏移量**

举个例子来说，现在调用方法hello()，这个方法的地址是1234567，那么hello就是符号引用，1234567就是直接引用。

在解析阶段，虚拟机会把所有的类名，方法名，字段名这些符号引用替换为具体的内存地址或偏移量，也就是直接引用。

## **初始化**

这个阶段主要是对**类变量**初始化，是执行类构造器的过程。

换句话说，只对static修饰的变量或语句进行初始化。

如果初始化一个类的时候，其父类尚未初始化，则优先初始化其父类。

如果同时包含多个静态变量和静态代码块，则按照自上而下的顺序依次执行。

## **总结**

类加载过程只是一个类生命周期的一部分，在其前，有编译的过程，只有对源代码编译之后，才能获得能够被虚拟机加载的字节码文件；在其后还有具体的类使用过程，当使用完成之后，还会在方法区垃圾回收的过程中进行卸载。如果想要了解Java类整个生命周期的话，可以自行上网查阅相关资料，这里不再多做赘述。

# 双亲委派模型

## 类的加载阶段

类加载阶段分为加载、连接、初始化三个阶段，而加载阶段需要通过类的全限定名来获取定义了此类的二进制字节流。**Java特意把这一步抽出来用类加载器来实现**。把这一步骤抽离出来使得应用程序可以按需自定义类加载器。并且得益于类加载器，OSGI、热部署等领域才得以在JAVA中得到应用。

在Java中**任意一个类都是由这个类本身和加载这个类的类加载器来确定这个类在JVM中的唯一性**。也就是你用你A类加载器加载的`com.aa.ClassA`和你B类加载器加载的`com.aa.ClassA`它们是不同的，也就是用`instanceof`这种对比都是不同的。所以即使都来自于同一个class文件但是由不同类加载器加载的那就是两个独立的类。

类加载器除了能用来加载类，还能用来作为类的层次划分。Java自身提供了3种类加载器

1、启动类加载器(Bootstrap ClassLoader),它是属于虚拟机自身的一部分，用C++实现的，**主要负责加载`<JAVA_HOME>\lib`目**录中或被-Xbootclasspath指定的路径中的并且文件名是被虚拟机识别的文件。它等于是所有类加载器的爸爸。

2、**扩展类加载器(Extension ClassLoader),它是Java实现的，独立于虚拟机，主要负责加载`<JAVA_HOME>\lib\ext`目录**中或被java.ext.dirs系统变量所指定的路径的类库。

3、应用程序类加载器(Application ClassLoader),它是Java实现的，独立于虚拟机。**主要负责加载用户类路径(classPath)上的类库**，如果我们没有实现自定义的类加载器那这玩意就是我们程序中的默认加载器。

## 双亲委派模型

知道上面这几个概念就能来看看双亲委派模型了。



![图来自百度](https://user-gold-cdn.xitu.io/2019/5/6/16a8d30870f9a8e5?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



**双亲委派的意思是如果一个类加载器需要加载类，那么首先它会把这个类请求委派给父类加载器去完成，每一层都是如此。一直递归到顶层，当父加载器无法完成这个请求时，子类才会尝试去加载。**这里的双亲其实就指的是父类，没有mother。父类也不是我们平日所说的那种继承关系，只是调用逻辑是这样。

```
        {
            // First, check if the class has already been loaded 先判断class是否已经被加载过了
            Class<?> c = findLoadedClass(name);
            if (c == null) {
                long t0 = System.nanoTime();
                try {
                    if (parent != null) {
                        c = parent.loadClass(name, false);  //找他爸爸去加载
                    } else {
                        c = findBootstrapClassOrNull(name);  //没爸爸说明是顶层了就用Bootstrap ClassLoader去加载
                    }
                } catch (ClassNotFoundException e) {
                    // ClassNotFoundException thrown if class not found
                    // from the non-null parent class loader
                }

                if (c == null) {
                    // If still not found, then invoke findClass in order
                    // to find the class.
                    long t1 = System.nanoTime();
                    c = findClass(name);    //最后如果没找到，那就自己找

                    // this is the defining class loader; record the stats
                    sun.misc.PerfCounter.getParentDelegationTime().addTime(t1 - t0);
                    sun.misc.PerfCounter.getFindClassTime().addElapsedTimeFrom(t1);
                    sun.misc.PerfCounter.getFindClasses().increment();
                }
            }
            if (resolve) {
                resolveClass(c);
            }
            return c;
        }
复制代码
```

**双亲委派模型不是一种强制性约束，也就是你不这么做也不会报错怎样的，它是一种JAVA设计者推荐使用类加载器的方式。**

双亲委派有啥好处呢？它使得类有了层次的划分。就拿`java.lang.Object`来说，你加载它经过一层层委托最终是由`Bootstrap ClassLoader`来加载的，也就是最终都是由`Bootstrap ClassLoader`去找`<JAVA_HOME>\lib`中rt.jar里面的`java.lang.Object`加载到JVM中。

这样如果有不法分子自己造了个`java.lang.Object`,里面嵌了不好的代码，如果我们是按照双亲委派模型来实现的话，最终加载到JVM中的只会是我们rt.jar里面的东西，也就是这些核心的基础类代码得到了保护。因为这个机制使得系统中只会出现一个`java.lang.Object`。不会乱套了。你想想如果我们JVM里面有两个Object,那岂不是天下大乱了。

因此既然推荐使用这种模型当然是有道理了。

但是人生不如意事十之八九，**有些情况不得不违反这个约束**，例如JDBC。

你先得知道SPI(Service Provider Interface)，这玩意和API不一样，它是面向拓展的，也就是我定义了这个SPI，具体如何实现由扩展者实现。我就是定了个规矩。

JDBC就是如此，在rt.jar里面定义了这个SPI，那mysql有mysql的jdbc实现，oracle有oracle的jdbc实现，反正我java不管你内部如何实现的，反正你们都得统一按我这个来，这样我们java开发者才能容易的调用数据库操作。所以因为这样那就不得不违反这个约束啊，`Bootstrap ClassLoader`就得委托子类来加载数据库厂商们提供的具体实现。因为它的手只能摸到`<JAVA_HOME>\lib`中，其他的它无能为力。这就违反了自下而上的委托机制了。

Java就搞了个线程上下文类加载器，通过`setContextClassLoader()`默认情况就是应用程序类加载器然后`Thread.current.currentThread().getContextClassLoader()`获得类加载器来加载。

# 三次握手和四次挥手

常用的熟知端口号
应用程序	FTP	TFTP	TELNET	SMTP	DNS	HTTP	SSH	MYSQL
熟知端口	21,20	69	23	25	53	80	22	3306
传输层协议	TCP	UDP	TCP	TCP	UDP	TCP	TCP	TCP
TCP的概述
TCP把连接作为最基本的对象，每一条TCP连接都有两个端点，这种断点我们叫作套接字（socket），它的定义为端口号拼接到IP地址即构成了套接字，例如，若IP地址为192.3.4.16 而端口号为80，那么得到的套接字为192.3.4.16:80。

TCP报文首部
源端口和目的端口，各占2个字节，分别写入源端口和目的端口；
序号，占4个字节，TCP连接中传送的字节流中的每个字节都按顺序编号。例如，一段报文的序号字段值是 301 ，而携带的数据共有100字段，显然下一个报文段（如果还有的话）的数据序号应该从401开始；
确认号，占4个字节，是期望收到对方下一个报文的第一个数据字节的序号。例如，B收到了A发送过来的报文，其序列号字段是501，而数据长度是200字节，这表明B正确的收到了A发送的到序号700为止的数据。因此，B期望收到A的下一个数据序号是701，于是B在发送给A的确认报文段中把确认号置为701；
数据偏移，占4位，它指出TCP报文的数据距离TCP报文段的起始处有多远；
保留，占6位，保留今后使用，但目前应都位0；
紧急URG，当URG=1，表明紧急指针字段有效。告诉系统此报文段中有紧急数据；
确认ACK，仅当ACK=1时，确认号字段才有效。TCP规定，在连接建立后所有报文的传输都必须把ACK置1；
推送PSH，当两个应用进程进行交互式通信时，有时在一端的应用进程希望在键入一个命令后立即就能收到对方的响应，这时候就将PSH=1；
复位RST，当RST=1，表明TCP连接中出现严重差错，必须释放连接，然后再重新建立连接；
同步SYN，在连接建立时用来同步序号。当SYN=1，ACK=0，表明是连接请求报文，若同意连接，则响应报文中应该使SYN=1，ACK=1；
终止FIN，用来释放连接。当FIN=1，表明此报文的发送方的数据已经发送完毕，并且要求释放；
窗口，占2字节，指的是通知接收方，发送本报文你需要有多大的空间来接受；
检验和，占2字节，校验首部和数据这两部分；
紧急指针，占2字节，指出本报文段中的紧急数据的字节数；
选项，长度可变，定义一些其他的可选的参数。
TCP连接的建立（三次握手）


最开始的时候客户端和服务器都是处于CLOSED状态。主动打开连接的为客户端，被动打开连接的是服务器。

TCP服务器进程先创建传输控制块TCB，时刻准备接受客户进程的连接请求，此时服务器就进入了LISTEN（监听）状态；
TCP客户进程也是先创建传输控制块TCB，然后向服务器发出连接请求报文，这是报文首部中的同部位SYN=1，同时选择一个初始序列号 seq=x ，此时，TCP客户端进程进入了 SYN-SENT（同步已发送状态）状态。TCP规定，SYN报文段（SYN=1的报文段）不能携带数据，但需要消耗掉一个序号。
TCP服务器收到请求报文后，如果同意连接，则发出确认报文。确认报文中应该 ACK=1，SYN=1，确认号是ack=x+1，同时也要为自己初始化一个序列号 seq=y，此时，TCP服务器进程进入了SYN-RCVD（同步收到）状态。这个报文也不能携带数据，但是同样要消耗一个序号。
TCP客户进程收到确认后，还要向服务器给出确认。确认报文的ACK=1，ack=y+1，自己的序列号seq=x+1，此时，TCP连接建立，客户端进入ESTABLISHED（已建立连接）状态。TCP规定，ACK报文段可以携带数据，但是如果不携带数据则不消耗序号。
当服务器收到客户端的确认后也进入ESTABLISHED状态，此后双方就可以开始通信了。

**为什么TCP客户端最后还要发送一次确认呢？**
**一句话，主要防止已经失效的连接请求报文突然又传送到了服务器，从而产生错误。**

**如果使用的是两次握手建立连接，假设有这样一种场景，客户端发送了第一个请求连接并且没有丢失，只是因为在网络结点中滞留的时间太长了，由于TCP的客户端迟迟没有收到确认报文，以为服务器没有收到，此时重新向服务器发送这条报文，此后客户端和服务器经过两次握手完成连接，传输数据，然后关闭连接。此时此前滞留的那一次请求连接，网络通畅了到达了服务器，这个报文本该是失效的，但是，两次握手的机制将会让客户端和服务器再次建立连接，这将导致不必要的错误和资源的浪费。**

**如果采用的是三次握手，就算是那一次失效的报文传送过来了，服务端接受到了那条失效报文并且回复了确认报文，但是客户端不会再次发出确认。由于服务器收不到确认，就知道客户端并没有请求连接。**

TCP连接的释放（四次挥手）


数据传输完毕后，双方都可释放连接。最开始的时候，客户端和服务器都是处于ESTABLISHED状态，然后客户端主动关闭，服务器被动关闭。

客户端进程发出连接释放报文，并且停止发送数据。释放数据报文首部，FIN=1，其序列号为seq=u（等于前面已经传送过来的数据的最后一个字节的序号加1），此时，客户端进入FIN-WAIT-1（终止等待1）状态。 TCP规定，FIN报文段即使不携带数据，也要消耗一个序号。
服务器收到连接释放报文，发出确认报文，ACK=1，ack=u+1，并且带上自己的序列号seq=v，此时，服务端就进入了CLOSE-WAIT（关闭等待）状态。TCP服务器通知高层的应用进程，客户端向服务器的方向就释放了，这时候处于半关闭状态，即客户端已经没有数据要发送了，但是服务器若发送数据，客户端依然要接受。这个状态还要持续一段时间，也就是整个CLOSE-WAIT状态持续的时间。
客户端收到服务器的确认请求后，此时，客户端就进入FIN-WAIT-2（终止等待2）状态，等待服务器发送连接释放报文（在这之前还需要接受服务器发送的最后的数据）。
服务器将最后的数据发送完毕后，就向客户端发送连接释放报文，FIN=1，ack=u+1，由于在半关闭状态，服务器很可能又发送了一些数据，假定此时的序列号为seq=w，此时，服务器就进入了LAST-ACK（最后确认）状态，等待客户端的确认。
客户端收到服务器的连接释放报文后，必须发出确认，ACK=1，ack=w+1，而自己的序列号是seq=u+1，此时，客户端就进入了TIME-WAIT（时间等待）状态。注意此时TCP连接还没有释放，必须经过2∗ *∗MSL（最长报文段寿命）的时间后，当客户端撤销相应的TCB后，才进入CLOSED状态。
服务器只要收到了客户端发出的确认，立即进入CLOSED状态。同样，撤销TCB后，就结束了这次的TCP连接。可以看到，服务器结束TCP连接的时间要比客户端早一些。

为什么客户端最后还要等待2MSL？
MSL（Maximum Segment Lifetime），TCP允许不同的实现可以设置不同的MSL值。

**第一，保证客户端发送的最后一个ACK报文能够到达服务器，因为这个ACK报文可能丢失，站在服务器的角度看来，我已经发送了FIN+ACK报文请求断开了，客户端还没有给我回应，应该是我发送的请求断开报文它没有收到，于是服务器又会重新发送一次，而客户端就能在这个2MSL时间段内收到这个重传的报文，接着给出回应报文，并且会重启2MSL计时器。**

**第二，防止类似与“三次握手”中提到了的“已经失效的连接请求报文段”出现在本连接中。客户端发送完最后一个确认报文后，在这个2MSL时间中，就可以使本连接持续的时间内所产生的所有报文段都从网络中消失。这样新的连接中不会出现旧连接的请求报文。**

为什么建立连接是三次握手，关闭连接确是四次挥手呢？

**建立连接的时候， 服务器在LISTEN状态下，收到建立连接请求的SYN报文后，把ACK和SYN放在一个报文里发送给客户端。**
**而关闭连接时，服务器收到对方的FIN报文时，仅仅表示对方不再发送数据了但是还能接收数据，而自己也未必全部数据都发送给对方了，所以己方可以立即关闭，也可以发送一些数据给对方后，再发送FIN报文给对方来表示同意现在关闭连接，因此，己方ACK和FIN一般都会分开发送，从而导致多了一次。**

**关闭连接时，客户端向服务端发送 FIN 时，仅仅表示客户端不再发送数据了但是还能接收数 据。 服务器收到客户端的 FIN 报文时，先回一个 ACK 应答报文，而服务端可能还有数据需要处理 和发送，等服务端不再发送数据时，才发送 FIN 报文给客户端来表示同意现在关闭连接。**

如果已经建立了连接，但是客户端突然出现故障了怎么办？
**TCP还设有一个保活计时器，显然，客户端如果出现故障，服务器不能一直等下去，白白浪费资源。服务器每收到一次客户端的请求后都会重新复位这个计时器，时间通常是设置为2小时，若两小时还没有收到客户端的任何数据，服务器就会发送一个探测报文段，以后每隔75秒发送一次。若一连发送10个探测报文仍然没反应，服务器就认为客户端出了故障，接着就关闭连接。**

————————

# TCP中的滑动窗口和流量控制


原文链接：https://blog.csdn.net/guoweimelon/article/details/50879588

# TCP的流量控制和拥塞控制

**TCP的流量控制**

\1. 利用滑动窗口实现流量控制

  如果发送方把数据发送得过快，接收方可能会来不及接收，这就会造成数据的丢失。所谓**流量控制**就是让发送方的发送速率不要太快，要让接收方来得及接收。

  利用滑动窗口机制可以很方便地在TCP连接上实现对发送方的流量控制。

  设A向B发送数据。在连接建立时，B告诉了A：“我的接收窗口是 rwnd = 400 ”(这里的 rwnd 表示 receiver window) 。因此，发送方的发送窗口不能超过接收方给出的接收窗口的数值。请注意，TCP的窗口单位是字节，不是报文段。TCP连接建立时的窗口协商过程在图中没有显示出来。再设每一个报文段为100字节长，而数据报文段序号的初始值设为1。大写ACK表示首部中的确认位ACK，小写ack表示确认字段的值ack。



![img](https://img-blog.csdn.net/20140509220855687?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveWVjaGFvZGVjaHVudGlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

  从图中可以看出，B进行了三次流量控制。第一次把窗口减少到 rwnd = 300 ，第二次又减到了 rwnd = 100 ，最后减到 rwnd = 0 ，即不允许发送方再发送数据了。这种使发送方暂停发送的状态将持续到主机B重新发出一个新的窗口值为止。B向A发送的三个报文段都设置了 ACK = 1 ，只有在ACK=1时确认号字段才有意义。

  TCP为每一个连接设有一个持续计时器(persistence timer)。只要TCP连接的一方收到对方的零窗口通知，就启动持续计时器。若持续计时器设置的时间到期，就发送一个零窗口控测报文段（携1字节的数据），那么收到这个报文段的一方就重新设置持续计时器。

\2. 必须考虑传输速率

  可以用不同的机制来控制TCP报文段的发送时机。如： <1>. TCP维持一个变量，它等于最大报文段长度MSS。只要缓存中存放的数据达到MSS字节时，就组装成一个TCP报文段发送出去。<2>. 由发送方的应用进程指明要求发送报文段，即TCP支持的推送( push )操作。<3>. 发送方的一个计时器期限到了，这时就把已有的缓存数据装入报文段(但长度不能超过MSS)发送出去。

  Nagle算法：若发送应用进程把要发送的数据逐个字节地送到TCP的发送缓存，则发送方就把第一个数据字节先发送出去，把后面到达的数据字节都缓存起来。当发送方接收对第一个数据字符的确认后，再把发送缓存中的所有数据组装成一个报文段再发送出去，同时继续对随后到达的数据进行缓存。只有在收到对前一个报文段的确认后才继续发送下一个报文段。当数据到达较快而网络速率较慢时，用这样的方法可明显地减少所用的网络带宽。Nagle算法还规定：当到达的数据已达到 发送窗口大小的一半或已达到报文段的最大长度时，就立即发送一个报文段。

  另，**糊涂窗口综合证：**TCP接收方的缓存已满，而交互式的应用进程一次只从接收缓存中读取1字节（这样就使接收缓存空间仅腾出1字节），然后向发送方发送确认，并把窗口设置为1个字节（但发送的数据报为40字节的的话）。接收，发送方又发来1个字节的数据（发送方的IP数据报是41字节）。接收方发回确认，仍然将窗口设置为1个字节。这样，网络的效率很低。要解决这个问题，可让接收方等待一段时间，使得或者接收缓存已有足够空间容纳一个最长的报文段，或者等到接收方缓存已有一半空闲的空间。只要出现这两种情况，接收方就发回确认报文，并向发送方通知当前的窗口大小。此外，发送方也不要发送太小的报文段，而是把数据报积累成足够大的报文段，或达到接收方缓存的空间的一半大小。

 

**TCP的拥塞控制**

\1. 拥塞：即对资源的需求超过了可用的资源。若网络中许多资源同时供应不足，网络的性能就要明显变坏，整个网络的吞吐量随之负荷的增大而下降。

  拥塞控制：**防止过多的数据注入到网络中，这样可以使网络中的路由器或链路不致过载。**拥塞控制所要做的都有一个**前提：网络能够承受现有的网络负荷。**拥塞控制是一个**全局性的过程**，涉及到所有的主机、路由器，以及与降低网络传输性能有关的所有因素。

  流量控制：指点对点通信量的控制，是端到端正的问题。流量控制所要做的就是抑制发送端发送数据的速率，以便使接收端来得及接收。

  拥塞控制代价：需要获得网络内部流量分布的信息。在实施拥塞控制之前，还需要在结点之间交换信息和各种命令，以便选择控制的策略和实施控制。这样就产生了额外的开销。拥塞控制还需要将一些资源分配给各个用户单独使用，使得网络资源不能更好地实现共享。

\2. 几种拥塞控制方法

  慢开始( slow-start )、拥塞避免( congestion avoidance )、快重传( fast retransmit )和快恢复( fast recovery )。

2.1 慢开始和拥塞避免

  发送方维持一个拥塞窗口 cwnd ( congestion window )的状态变量。拥塞窗口的大小取决于网络的拥塞程度，并且动态地在变化。**发送方让自己的发送窗口等于拥塞。**

  发送方控制拥塞窗口的原则是：只要网络没有出现拥塞，拥塞窗口就再增大一些，以便把更多的分组发送出去。但只要网络出现拥塞，拥塞窗口就减小一些，以减少注入到网络中的分组数。

  慢开始算法：当主机开始发送数据时，如果立即所大量数据字节注入到网络，那么就有可能引起网络拥塞，因为现在并不清楚网络的负荷情况。因此，较好的方法是先探测一下，即由小到大逐渐增大发送窗口，也就是说，由小到大逐渐增大拥塞窗口数值。通常在刚刚开始发送报文段时，先把拥塞窗口 cwnd 设置为一个最大报文段MSS的数值。而在每收到一个对新的报文段的确认后，把拥塞窗口增加至多一个MSS的数值。用这样的方法逐步增大发送方的拥塞窗口 cwnd ，可以使分组注入到网络的速率更加合理。

![img](https://img-blog.csdn.net/20140509220932437?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveWVjaGFvZGVjaHVudGlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 

**每经过一个传输轮次，拥塞窗口 cwnd 就加倍。一个传输轮次所经历的时间其实就是往返时间RTT。不过“传输轮次”更加强调：把拥塞窗口cwnd所允许发送的报文段都连续发送出去，并收到了对已发送的最后一个字节的确认。**

**另，慢开始的“慢”并不是指cwnd的增长速率慢，而是指在TCP开始发送报文段时先设置cwnd=1，使得发送方在开始时只发送一个报文段（目的是试探一下网络的拥塞情况），然后再逐渐增大cwnd。**

  为了防止拥塞窗口cwnd增长过大引起网络拥塞，还需要设置一个慢开始门限ssthresh状态变量（如何设置ssthresh）。慢开始门限ssthresh的用法如下：

  当 cwnd < ssthresh 时，使用上述的慢开始算法。

  当 cwnd > ssthresh 时，停止使用慢开始算法而改用拥塞避免算法。

  当 cwnd = ssthresh 时，既可使用慢开始算法，也可使用拥塞控制避免算法。

**拥塞避免算法：让拥塞窗口cwnd缓慢地增大，即每经过一个往返时间RTT就把发送方的拥塞窗口cwnd加1，而不是加倍。这样拥塞窗口cwnd按线性规律缓慢增长，比慢开始算法的拥塞窗口增长速率缓慢得多。**

  无论在慢开始阶段还是在拥塞避免阶段，只要发送方判断网络出现拥塞（其根据就是没有收到确认），就**要把慢开始门限ssthresh设置为出现拥塞时的发送方窗口值的一半（但不能小于2）。然后把拥塞窗口cwnd重新设置为1，执行慢开始算法。这样做的目的就是要迅速减少主机发送到网络中的分组数，使得发生拥塞的路由器有足够时间把队列中积压的分组处理完毕。**

  如下图，用具体数值说明了上述拥塞控制的过程。现在发送窗口的大小和拥塞窗口一样大。

![img](https://img-blog.csdn.net/20140509221015859?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveWVjaGFvZGVjaHVudGlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

<1>. 当TCP连接进行初始化时，把拥塞窗口cwnd置为1。前面已说过，为了便于理解，图中的窗口单位不使用字节而使用报文段的个数。慢开始门限的初始值设置为16个报文段，即 cwnd = 16 。

<2>. 在执行慢开始算法时，拥塞窗口 cwnd 的初始值为1。以后发送方每收到一个对新报文段的确认ACK，就把拥塞窗口值另1，然后开始下一轮的传输（图中横坐标为传输轮次）。因此拥塞窗口cwnd随着传输轮次按指数规律增长。当拥塞窗口cwnd增长到慢开始门限值ssthresh时（即当cwnd=16时），就改为执行拥塞控制算法，拥塞窗口按线性规律增长。

<3>. 假定拥塞窗口的数值增长到24时，网络出现超时（这很可能就是网络发生拥塞了）。更新后的ssthresh值变为12（**即变为出现超时时的拥塞窗口数值24的一半**），拥塞窗口再重新设置为1，并执行慢开始算法。当cwnd=ssthresh=12时改为执行拥塞避免算法，拥塞窗口按线性规律增长，每经过一个往返时间增加一个MSS的大小。

强调：“拥塞避免”并非指完全能够避免了拥塞。利用以上的措施要完全避免网络拥塞还是不可能的。“拥塞避免”是说在拥塞避免阶段将拥塞窗口控制为按线性规律增长，**使网络比较不容易出现拥塞。**

2.2 快重传和快恢复

**如果发送方设置的超时计时器时限已到但还没有收到确认，那么很可能是网络出现了拥塞，致使报文段在网络中的某处被丢弃。这时，TCP马上把拥塞窗口 cwnd 减小到1，并执行慢开始算法，同时把慢开始门限值ssthresh减半。这是不使用快重传的情况。**

  快重传算法首先要求接收方每收到一个失序的报文段后就立即发出重复确认（为的是使发送方及早知道有报文段没有到达对方）而不要等到自己发送数据时才进行捎带确认。

![img](https://img-blog.csdn.net/20140509221032109?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveWVjaGFvZGVjaHVudGlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

接收方收到了M1和M2后都分别发出了确认。现在假定接收方没有收到M3但接着收到了M4。显然，接收方不能确认M4，因为M4是收到的失序报文段。根据可靠传输原理，接收方可以什么都不做，也可以在适当时机发送一次对M2的确认。**但按照快重传算法的规定，接收方应及时发送对M2的重复确认，这样做可以让发送方及早知道报文段M3没有到达接收方**。发送方接着发送了M5和M6。接收方收到这两个报文后，也还要再次发出对M2的重复确认。这样，发送方共收到了接收方的四个对M2的确认，其中后三个都是重复确认。**快重传算法还规定，发送方只要一连收到三个重复确认就应当立即重传对方尚未收到的报文段M3，而不必继续等待M3设置的重传计时器到期。由于发送方尽早重传未被确认的报文段，因此采用快重传后可以使整个网络吞吐量提高约20%。**

  与快重传配合使用的还有快恢复算法，其过程有以下两个要点：

  **<1>. 当发送方连续收到三个重复确认，就执行“乘法减小”算法，把慢开始门限ssthresh减半。这是为了预防网络发生拥塞。请注意：接下去不执行慢开始算法。**

  **<2>. 由于发送方现在认为网络很可能没有发生拥塞，因此与慢开始不同之处是现在不执行慢开始算法（即拥塞窗口cwnd现在不设置为1），而是把cwnd值设置为慢开始门限ssthresh减半后的数值，然后开始执行拥塞避免算法（“加法增大”），使拥塞窗口缓慢地线性增大。**

  下图给出了快重传和快恢复的示意图，并标明了“TCP Reno版本”。

  区别：新的 TCP Reno 版本在快重传之后采用快恢复算法而不是采用慢开始算法。

![img](https://img-blog.csdn.net/20140509221048265?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveWVjaGFvZGVjaHVudGlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) 

 也有的快重传实现是把开始时的拥塞窗口cwnd值再增大一点，即等于 ssthresh + 3 X MSS 。这样做的理由是：既然发送方收到三个重复的确认，就表明有三个分组已经离开了网络。这三个分组不再消耗网络 的资源而是停留在接收方的缓存中。可见现在网络中并不是堆积了分组而是减少了三个分组。因此可以适当把拥塞窗口扩大了些。

  在采用快恢复算法时，慢开始算法只是在TCP连接建立时和网络出现超时时才使用。

  采用这样的拥塞控制方法使得TCP的性能有明显的改进。

  **接收方根据自己的接收能力设定了接收窗口rwnd，并把这个窗口值写入TCP首部中的窗口字段，传送给发送方。因此，接收窗口又称为通知窗口。因此，从接收方对发送方的流量控制的角度考虑，发送方的发送窗口一定不能超过对方给出的接收窗口rwnd 。**

  **发送方窗口的上限值 = Min [ rwnd, cwnd ]**

  **当rwnd < cwnd 时，是接收方的接收能力限制发送方窗口的最大值。**

  **当cwnd < rwnd 时，则是网络的拥塞限制发送方窗口的最大值。**

# 输入url到显示页面的过程

1、根据域名查询域名的IP地址，DNS解析。
浏览器查询 DNS，获取域名对应的 IP 地址：具体过程包括浏览器搜索自身的 DNS 缓存、搜索操作系统的 DNS 缓存、读取本地的 Host 文件和向本地 DNS 服务器进行查询等。

![DNS解析过程](https://segmentfault.com/img/bVDM45?w=1928&h=1248)2、TCP连接
浏览器获得域名对应的 IP 地址以后，浏览器向服务器请求建立连接，发起三次握手；
3、发送HTTP请求
TCP 连接建立起来后，浏览器向服务器发送 HTTP 请求；
4、服务器处理请求并返回HTTP报文
服务器接收到这个请求，并根据路径参数映射到特定的请求处理器进行处理，并将处理结果及相应的视图返回给浏览器；
5、浏览器解析渲染页面
浏览器解析并渲染视图，若遇到对 js 文件、css 文件及图片等静态资源的引用，则重复上述步骤并向服务器请求这些资源；浏览器根据其请求到的资源、数据渲染页面，最终向用户呈现一个完整的页面。
6、连接结束。

使用的协议：
DNS：(获取域名的IP的地址);
TCP：(与服务器建立TCP连接)；
IP：(建立TCP协议时，需发送数据，在网络层用到IP协议)；
OPSF：(IP数据包在路由之间传送，路由选择使用OPSF协议)；
ARP：(路由器与服务器通信时，将IP地址转化为MAC地址，使用ARP协议)
HTTP：(TCP建立之后，使用HTTP协议访问网页)；

DNS寻址：先查找浏览器缓存，如果没命中，查询系统缓存，即hosts文件。如果没命中，查询路由器缓存。如果没命中，请求本地域名服务器解析域名，没有命中就进入根服务器进行查询。没有命中就返回顶级域名服务器IP给本地DNS服务器。本地DNS服务器请求顶级域名服务器解析，没有命中就返回主域名服务器给本地DNS服务器。本地DNS服务器请求主域名服务器解析域名，将结果返回给本地域名服务器。本地域名服务器缓存结果并反馈给客户端。

# Cookie和Session

## 第一层楼

什么是 Cookie 和 Session ?初级程序员高频面试题。

**什么是 Cookie**

HTTP Cookie（也叫 Web Cookie或浏览器 Cookie）是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。Cookie 使基于无状态的 HTTP 协议记录稳定的状态信息成为了可能。

Cookie 主要用于以下三个方面：

- 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）
- 个性化设置（如用户自定义设置、主题等）
- 浏览器行为跟踪（如跟踪分析用户行为等）

**什么是 Session**

Session 代表着服务器和客户端一次会话的过程。Session 对象存储特定用户会话所需的属性及配置信息。这样，当用户在应用程序的 Web 页之间跳转时，存储在 Session 对象中的变量将不会丢失，而是在整个用户会话中一直存在下去。当客户端关闭会话，或者 Session 超时失效时会话结束。

## 第二层楼

Cookie 和 Session 有什么不同？

- 作用范围不同，Cookie 保存在客户端（浏览器），Session 保存在服务器端。
- 存取方式的不同，Cookie 只能保存 ASCII，Session 可以存任意数据类型，一般情况下我们可以在 Session 中保持一些常用变量信息，比如说 UserId 等。``
- 有效期不同，Cookie 可设置为长时间保持，比如我们经常使用的默认登录功能，Session 一般失效时间较短，客户端关闭或者 Session 超时都会失效。
- 隐私策略不同，Cookie 存储在客户端，比较容易遭到不法获取，早期有人将用户的登录名和密码存储在 Cookie 中导致信息被窃取；Session 存储在服务端，安全性相对 Cookie 要好一些。
- 存储大小不同， 单个 Cookie 保存的数据不能超过 4K，Session 可存储数据远高于 Cookie。

前两层楼内容，绝大部分同学都可以准确回答

## 第三层楼

为什么需要 Cookie 和 Session，他们有什么关联？

说起来为什么需要 Cookie ，这就需要从浏览器开始说起，我们都知道浏览器是没有状态的(HTTP 协议无状态)，这意味着浏览器并不知道是张三还是李四在和服务端打交道。这个时候就需要有一个机制来告诉服务端，本次操作用户是否登录，是哪个用户在执行的操作，那这套机制的实现就需要 Cookie 和 Session 的配合。

那么 Cookie 和 Session 是如何配合的呢？我画了一张图大家可以先了解下。

![img](http://www.itmind.net/assets/images/2019/it/Cookie-Session.png)

用户第一次请求服务器的时候，服务器根据用户提交的相关信息，创建创建对应的 Session ，请求返回时将此 Session 的唯一标识信息 SessionID 返回给浏览器，**浏览器接收到服务器返回的 SessionID 信息后，会将此信息存入到 Cookie 中，同时 Cookie 记录此 SessionID 属于哪个域名**。

当用户第二次访问服务器的时候，请求会自动判断此域名下是否存在 Cookie 信息，如果存在自动将 Cookie 信息也发送给服务端，服务端会从 Cookie 中获取 SessionID，再根据 SessionID 查找对应的 Session 信息，如果没有找到说明用户没有登录或者登录失效，如果找到 Session 证明用户已经登录可执行后面操作。

根据以上流程可知，SessionID 是连接 Cookie 和 Session 的一道桥梁，大部分系统也是根据此原理来验证用户登录状态。

三层楼的内容，大部分同学可以讲清楚。

## 第四层楼

既然服务端是根据 Cookie 中的信息判断用户是否登录，那么如果浏览器中禁止了 Cookie，如何保障整个机制的正常运转。

第一种方案，每次请求中都携带一个 SessionID 的参数，也可以 Post 的方式提交，也可以在请求的地址后面拼接 `xxx?SessionID=123456...`。

第二种方案，Token 机制。Token 机制多用于 App 客户端和服务器交互的模式，也可以用于 Web 端做用户状态管理。

Token 的意思是“令牌”，是服务端生成的一串字符串，作为客户端进行请求的一个标识。Token 机制和 Cookie 和 Session 的使用机制比较类似。

当用户第一次登录后，服务器根据提交的用户信息生成一个 Token，响应时将 Token 返回给客户端，以后客户端只需带上这个 Token 前来请求数据即可，无需再次登录验证。

四层楼的内容，一部分同学可以讲清楚。

## 第五层楼

如何考虑分布式 Session 问题？

在互联网公司为了可以支撑更大的流量，后端往往需要多台服务器共同来支撑前端用户请求，那如果用户在 A 服务器登录了，第二次请求跑到服务 B 就会出现登录失效问题。

分布式 Session 一般会有以下几种解决方案：

- Nginx ip_hash 策略，服务端使用 Nginx 代理，每个请求按访问 IP 的 hash 分配，这样来自同一 IP 固定访问一个后台服务器，避免了在服务器 A 创建 Session，第二次分发到服务器 B 的现象。
- Session 复制，任何一个服务器上的 Session 发生改变（增删改），该节点会把这个 Session 的所有内容序列化，然后广播给所有其它节点。
- 共享 Session，服务端无状态话，将用户的 Session 等信息使用缓存中间件来统一管理，保障分发到每一个服务器的响应结果都一致。

建议采用第三种方案。

## 第六层楼

如何解决跨域请求？Jsonp 跨域的原理是什么？

说起跨域请求，必须要了解浏览器的同源策略，同源策略/SOP（Same origin policy）是一种约定，由 Netscape 公司 1995年引入浏览器，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到 XSS、CSFR 等攻击。所谓同源是指"协议+域名+端口"三者相同，即便两个不同的域名指向同一个 ip 地址，也非同源。

解决跨域请求的常用方法是：

- 通过代理来避免，比如使用 Nginx 在后端转发请求，避免了前端出现跨域的问题。
- 通过 Jsonp 跨域
- 其它跨域解决方案

重点谈一下 Jsonp 跨域原理。浏览器的同源策略把跨域请求都禁止了，但是页面中的 `<script><img><iframe>`标签是例外，不受同源策略限制。Jsonp 就是利用 `<script>` 标签跨域特性进行跨域数据访问。

JSONP 的理念就是，与服务端约定好一个回调函数名，服务端接收到请求后，将返回一段 Javascript，在这段 Javascript 代码中调用了约定好的回调函数，并且将数据作为参数进行传递。当网页接收到这段 Javascript 代码后，就会执行这个回调函数，这时数据已经成功传输到客户端了。

JSONP 的缺点是：它只支持 GET 请求，而不支持 POST 请求等其他类型的 HTTP 请求。

以上就是有关 Cookie 和 Session 常见的面试点，不知道有多少同学可以在面试中准确回答所有问题。

# TCP和UDP的首部字段

TCP概念:
        TCP提供一种面向连接的、可靠的、字节流服务。
        面向连接：面向连接即意味着两个使用TCP的应用(通常为服务器与客户端)在彼此交换数据之前必须先建立一个TCP连接。
        字节流：两个应用程序通过TCP交换8bit字节构成的字节流,TCP不在字节流中插入记录标识符。我们将这称为字节流服务（ byte stream service）。
        注意：
        1.如果一方的应用程序先传10字节，又传20字节,再传50字节,连接的另一方将无法了解发方每次发送了多少字节。收方可以分 4次接收这80个字节,每次接收20字节。一端将字节流放到TCP连接上，同样的字节流将出现在TCP连接的另一端。
        2.TCP对字节流的内容不作任何解释。 TCP不知道传输的数据字节流是二进制数据,还是ASCII字符、EBCDIC字符或者其他类型数据。对字节流的解释由TCP连接双方的应用层解释。

TCP通过下列方式提供可靠性：
     1.应用数据被分割成TCP认为最适合发送的数据块。
     2.当TCP发出一个段后.它启动一个定时器,等待目的端确认收到这个报文段。如果不能及时收到一个确认,将重发这个报文段。
     3.当TCP收到发自TCP连接另一端的数据,它将发送一个确认。这个确认不是立即发送,通常将推迟几分之一秒。
     4.TCP将保持它首部和数据的检验和。这是一个端到端的检验和,目的是检测数据在传输过程中的任何变化。如果收到段的检验和有差错,TCP将丢弃这个报文段和不确认收到此报文段（希望发端超时并重发）。
     5. 既然TCP报文段作为I P数据报来传输,而 I P数据报的到达可能会失序,因此 TCP报文段的到达也可能会失序。如果必要, TCP将对收到的数据进行重新排序，将收到的数据以正确的顺序交给应用层。
     6.既然I P数据报会发生重复,TCP的接收端必须丢弃重复的数据。
     7.TCP还能提供流量控制。TCP连接的每一方都有固定大小的缓冲空间。TCP的接收端只允许另一端发送接收端缓冲区所能接纳的数据。这将防止较快主机致使较慢主机的缓冲区溢出。

TCP的首部：

1.长度：从0-31，所以每一行为32位，占4个字节。至少占5行，TCP报头最小20个字节，最大60字节。
2.16位源端口号：发送端端口号
3.16位目的端口号：接收端端口号
4.32位序号：即编号，初始值：第一个数据在第一次交互时由系统随机生成。
        序号值如何变化？第一个为随机值，第二个就是发送的数据在整个字节流中的偏移量 + 第一次生成的值
       数据值也是从小到达排列：可以保证数据不乱序
       如图所示：假设由100个字节数据要发送，初始值为2000，初始值+偏移量即为下一个的序号值。
      
5.32位确认号：数据发送出去接收端接收后，接收端给发送端回馈确认机制。
                            若接收端接收到2000，则回复2001。
                            确认号还能够处理重复的报文段，一旦接收到相同的序号就丢弃
6.4位头部长度：15个4字节，最多60个字节。
         对于底层而言，控制协议是一堆数据，发送的还是一堆数据，接收端接收到后如何确定哪些是头部哪些是所携带的数据？
4位头部长度可确认，前20个字节为报头后面是携带的数据。若选项部分携带4字节，则前24字节为头部后面为携带数据。
7.6位保留：占6位，保留为今后使用，目前应设置为0
8.控制位UGR、ACK、PSH、RST、SYN、FIN：
     紧急UGR:当UGR置1时，发送应用进程就告诉发送方的TCP有紧急数据要传送。于是发送方的TCP就把紧急数据插入到本报文段数据的最前面，而在紧急数据后面的数据仍是普通数据。
     确认ACK:确认报文段，仅当ACK=1时确认号字段才有效。当ACK=0时，确认号无效。
     推送PSH:当两个应用进程进行交互式的通信时，有时在一端的应用进程希望在键入一个命令后立即就能够收到对方响应。在这种情况下，TCP可以使用PSUH(推送操作)。这时，发送方TCP把PSH置1，并立即创建一个报文段发送出去。接收方TCP收到PSH=1的报文段，就尽快(推送)交付给接收应用进程，而不在等整个缓存都填满了再向上交付。
     复位RST:当RST=1时，表明TCP连接出现了严重差错，必须释放连接，然后重新建立新运输连接。**RST=1还可以用来拒接一个非法报文段或拒绝打开一个连接。
     同步SYN:在建立连接时用来同步序号。当SYN=1，ACK=0时，表明这是一个连接请求报文段；对方若同意连接，则应在相应的报文段中使SYN=1,ACK=1。因此SYN置1就表示这是一个连接请求或连接接受报文段。
     终止FIN：用来释放一个连接，当FIN=1时，表明此报文段的发送方数据已经发送完毕，并要求释放运输连接。
9.16位窗口大小：流量管控,窗口值是[0~216-1]之间的整数。窗口值告诉了对方，从本报文段的确认号算起，允许对方发送的数据量。
     心跳包机制：
10.16位校验和：CRC循环冗余检测算法
11.16位紧急指针：TCP的紧急指针是发送端向接收端发送紧急数据的方法。紧急指针是一个正的偏移量。它和序号字段的值相加表示最后一个紧急数据的下一字节的序号。这个字段是紧急指针相对当前序号的偏移，不妨称之为紧急偏移。
12.选项部分：长度可变，最小0字节，最长达40字节。当没有使用选项部分时，TCP的首部为20字节。

MTU限制：以太网为例1500字节，IP头部20个字节，TCP头部和数据加起来共1480个字节，又因为TCP头部20个字节，所以它所能携带的上层数据为1460个字节。

UDP概念:
        UDP提供一种无连接的、不可靠的、数据报服务。
        无连接：发送数据之前不需要建立连接，因此减少了开销和发送数据之前的时延。
        不可靠：尽最大努力交付，不保证能到达对端
        数据报服务：UDP对应用层交下来的报文，既不合并，也不拆分，而是保留这些报文的边界。

UDP的首部：
首部字段很简单，只有8个字节，由4个字段组成，每个字段的长度都是2个字节。
UDP报头结构：携带数据大小长度 - 8

1.源端口号：在需要对方回信时选用，不需要时可全用0
2.目的端口号：在终点交付时使用
3.长度：UDP用户数据报的长度，最小值是8
4.检验和：检测UDP用户数据报在传输中是否有错，有错就丢弃
————————————————
版权声明：本文为CSDN博主「硕～」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_42441693/article/details/102773890

# http和https的区别

超文本传输协议HTTP协议被用于在Web浏览器和网站服务器之间传递信息，HTTP协议以明文方式发送内容，不提供任何方式的数据加密，如果攻击者截取了Web浏览器和网站服务器之间的传输报文，就可以直接读懂其中的信息，因此，HTTP协议不适合传输一些敏感信息，比如：信用卡号、密码等支付信息。

为了解决HTTP协议的这一缺陷，需要使用另一种协议：安全套接字层超文本传输协议HTTPS，为了数据传输的安全，HTTPS在HTTP的基础上加入了SSL协议，SSL依靠证书来验证服务器的身份，并为浏览器和服务器之间的通信加密。

**一、HTTP和HTTPS的基本概念**

HTTP：是互联网上应用最为广泛的一种网络协议，是一个客户端和服务器端请求和应答的标准（TCP），用于从WWW服务器传输超文本到本地浏览器的传输协议，它可以使浏览器更加高效，使网络传输减少。

HTTPS：是以安全为目标的HTTP通道，简单讲是HTTP的安全版，即HTTP下加入SSL层，HTTPS的安全基础是SSL，因此加密的详细内容就需要SSL。

HTTPS协议的主要作用可以分为两种：一种是建立一个信息安全通道，来保证数据传输的安全；另一种就是确认网站的真实性。

**二、HTTP与HTTPS有什么区别？**

HTTP协议传输的数据都是未加密的，也就是明文的，因此使用HTTP协议传输隐私信息非常不安全，为了保证这些隐私数据能加密传输，于是网景公司设计了SSL（Secure Sockets Layer）协议用于对HTTP协议传输的数据进行加密，从而就诞生了HTTPS。

简单来说，HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，要比http协议安全。

HTTPS和HTTP的区别主要如下：

1、https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。

2、http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。

3、http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。

4、http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。

**三、HTTPS的工作原理**

我们都知道HTTPS能够加密信息，以免敏感信息被第三方获取，所以很多银行网站或电子邮箱等等安全级别较高的服务都会采用HTTPS协议。

**1、客户端发起HTTPS请求**

这个没什么好说的，就是用户在浏览器里输入一个https网址，然后连接到server的443端口。

**2、服务端的配置**

采用HTTPS协议的服务器必须要有一套数字证书，可以自己制作，也可以向组织申请，区别就是自己颁发的证书需要客户端验证通过，才可以继续访问，而使用受信任的公司申请的证书则不会弹出提示页面(startssl就是个不错的选择，有1年的免费服务)。

这套证书其实就是一对公钥和私钥，如果对公钥和私钥不太理解，可以想象成一把钥匙和一个锁头，只是全世界只有你一个人有这把钥匙，你可以把锁头给别人，别人可以用这个锁把重要的东西锁起来，然后发给你，因为只有你一个人有这把钥匙，所以只有你才能看到被这把锁锁起来的东西。

**3、传送证书**

这个证书其实就是公钥，只是包含了很多信息，如证书的颁发机构，过期时间等等。

**4、客户端解析证书**

这部分工作是有客户端的TLS来完成的，首先会验证公钥是否有效，比如颁发机构，过期时间等等，如果发现异常，则会弹出一个警告框，提示证书存在问题。

如果证书没有问题，那么就生成一个随机值，然后用证书对该随机值进行加密，就好像上面说的，把随机值用锁头锁起来，这样除非有钥匙，不然看不到被锁住的内容。

**5、传送加密信息**

这部分传送的是用证书加密后的随机值，目的就是让服务端得到这个随机值，以后客户端和服务端的通信就可以通过这个随机值来进行加密解密了。

**6、服务段解密信息**

服务端用私钥解密后，得到了客户端传过来的随机值(私钥)，然后把内容通过该值进行对称加密，所谓对称加密就是，将信息和私钥通过某种算法混合在一起，这样除非知道私钥，不然无法获取内容，而正好客户端和服务端都知道这个私钥，所以只要加密算法够彪悍，私钥够复杂，数据就够安全。

**7、传输加密后的信息**

这部分信息是服务段用私钥加密后的信息，可以在客户端被还原。

**8、客户端解密信息**

客户端用之前生成的私钥解密服务段传过来的信息，于是获取了解密后的内容，整个过程第三方即使监听到了数据，也束手无策。

**六、HTTPS的优点**

正是由于HTTPS非常的安全，攻击者无法从中找到下手的地方，从站长的角度来说，HTTPS的优点有以下2点：

**1、SEO方面**

谷歌曾在2014年8月份调整搜索引擎算法，并称“比起同等HTTP网站，采用HTTPS加密的网站在搜索结果中的排名将会更高”。

**2、安全性**

尽管HTTPS并非绝对安全，掌握根证书的机构、掌握加密算法的组织同样可以进行中间人形式的攻击，但HTTPS仍是现行架构下最安全的解决方案，主要有以下几个好处：

（1）、使用HTTPS协议可认证用户和服务器，确保数据发送到正确的客户机和服务器；

（2）、HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，要比http协议安全，可防止数据在传输过程中不被窃取、改变，确保数据的完整性。

（3）、HTTPS是现行架构下最安全的解决方案，虽然不是绝对安全，但它大幅增加了中间人攻击的成本。

**七、HTTPS的缺点**

虽然说HTTPS有很大的优势，但其相对来说，还是有些不足之处的，具体来说，有以下2点：

**1、SEO方面**

据ACM CoNEXT数据显示，使用HTTPS协议会使页面的加载时间延长近50%，增加10%到20%的耗电，此外，HTTPS协议还会影响缓存，增加数据开销和功耗，甚至已有安全措施也会受到影响也会因此而受到影响。

而且HTTPS协议的加密范围也比较有限，在黑客攻击、拒绝服务攻击、服务器劫持等方面几乎起不到什么作用。

最关键的，SSL证书的信用链体系并不安全，特别是在某些国家可以控制CA根证书的情况下，中间人攻击一样可行。

**2、经济方面**

（1）、SSL证书需要钱，功能越强大的证书费用越高，个人网站、小网站没有必要一般不会用。

（2）、SSL证书通常需要绑定IP，不能在同一IP上绑定多个域名，IPv4资源不可能支撑这个消耗（SSL有扩展可以部分解决这个问题，但是比较麻烦，而且要求浏览器、操作系统支持，Windows XP就不支持这个扩展，考虑到XP的装机量，这个特性几乎没用）。

（3）、HTTPS连接缓存不如HTTP高效，大流量网站如非必要也不会采用，流量成本太高。

（4）、HTTPS连接服务器端资源占用高很多，支持访客稍多的网站需要投入更大的成本，如果全部采用HTTPS，基于大部分计算资源闲置的假设的VPS的平均成本会上去。

（5）、HTTPS协议握手阶段比较费时，对网站的相应速度有负面影响，如非必要，没有理由牺牲用户体验。

# 虚拟内存的那点事儿

### 概述

------

我们都知道一个进程是与其他进程共享CPU和内存资源的。正因如此，操作系统需要有一套完善的内存管理机制才能防止进程之间内存泄漏的问题。

为了更加有效地管理内存并减少出错，现代操作系统提供了一种对主存的抽象概念，即是虚拟内存（Virtual Memory）。**虚拟内存为每个进程提供了一个一致的、私有的地址空间，它让每个进程产生了一种自己在独享主存的错觉（每个进程拥有一片连续完整的内存空间）**。

理解不深刻的人会认为虚拟内存只是“使用硬盘空间来扩展内存“的技术，这是不对的。**虚拟内存的重要意义是它定义了一个连续的虚拟地址空间**，使得程序的编写难度降低。并且，**把内存扩展到硬盘空间只是使用虚拟内存的必然结果，虚拟内存空间会存在硬盘中，并且会被内存缓存（按需），有的操作系统还会在内存不够的情况下，将某一进程的内存全部放入硬盘空间中，并在切换到该进程时再从硬盘读取**（这也是为什么Windows会经常假死的原因...）。

虚拟内存主要提供了如下三个重要的能力：

- **它把主存看作为一个存储在硬盘上的虚拟地址空间的高速缓存，并且只在主存中缓存活动区域（按需缓存）。**
- **它为每个进程提供了一个一致的地址空间，从而降低了程序员对内存管理的复杂性。**
- **它还保护了每个进程的地址空间不会被其他进程破坏。**

介绍了虚拟内存的基本概念之后，接下来的内容将会从虚拟内存在硬件中如何运作逐渐过渡到虚拟内存在操作系统（Linux）中的实现。

> 本文作者为[SylvanasSun(sylvanas.sun@gmail.com)](https://github.com/SylvanasSun)，首发于[SylvanasSun’s Blog](https://sylvanassun.github.io/)。
> 原文链接：[sylvanassun.github.io/2017/10/29/…](https://sylvanassun.github.io/2017/10/29/2017-10-29-virtual_memory/)
> （转载请务必保留本段声明，并且保留超链接。）

### CPU寻址

------

内存通常被组织为一个由M个连续的字节大小的单元组成的数组，每个字节都有一个唯一的物理地址（Physical Address PA），作为到数组的索引。CPU访问内存最简单直接的方法就是使用物理地址，这种寻址方式被称为物理寻址。

现代处理器使用的是一种称为虚拟寻址（Virtual Addressing）的寻址方式。**使用虚拟寻址，CPU需要将虚拟地址翻译成物理地址，这样才能访问到真实的物理内存。**



![虚拟寻址](https://user-gold-cdn.xitu.io/2017/10/31/64ebc813fa579e80d52459ae25618925?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)虚拟寻址



虚拟寻址需要硬件与操作系统之间互相合作。**CPU中含有一个被称为内存管理单元（Memory Management Unit, MMU）的硬件，它的功能是将虚拟地址转换为物理地址。MMU需要借助存放在内存中的页表来动态翻译虚拟地址，该页表由操作系统管理。**

### 页表

------

虚拟内存空间被组织为一个存放在硬盘上的M个连续的字节大小的单元组成的数组，每个字节都有一个唯一的虚拟地址，作为到数组的索引（这点其实与物理内存是一样的）。

**操作系统通过将虚拟内存分割为大小固定的块来作为硬盘和内存之间的传输单位，这个块被称为虚拟页（Virtual Page, VP），每个虚拟页的大小为`P=2^p`字节。物理内存也会按照这种方法分割为物理页（Physical Page, PP），大小也为`P`字节。**

CPU在获得虚拟地址之后，需要通过MMU将虚拟地址翻译为物理地址。而在翻译的过程中还需要借助页表，所谓**页表就是一个存放在物理内存中的数据结构，它记录了虚拟页与物理页的映射关系。**

**页表是一个元素为页表条目（Page Table Entry, PTE）的集合，每个虚拟页在页表中一个固定偏移量的位置上都有一个PTE**。下面是PTE仅含有一个有效位标记的页表结构，该有效位代表这个虚拟页是否被缓存在物理内存中。



![img](https://user-gold-cdn.xitu.io/2017/10/31/f37cc0b690138449ffd9fed42b41c44e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



虚拟页`VP 0`、`VP 4`、`VP 6`、`VP 7`被缓存在物理内存中，虚拟页`VP 2`和`VP 5`被分配在页表中，但并没有缓存在物理内存，虚拟页`VP 1`和`VP 3`还没有被分配。

在进行动态内存分配时，例如`malloc()`函数或者其他高级语言中的`new`关键字，操作系统会在硬盘中创建或申请一段虚拟内存空间，并更新到页表（分配一个PTE，使该PTE指向硬盘上这个新创建的虚拟页）。

**由于CPU每次进行地址翻译的时候都需要经过PTE，所以如果想控制内存系统的访问，可以在PTE上添加一些额外的许可位（例如读写权限、内核权限等）**，这样只要有指令违反了这些许可条件，CPU就会触发一个一般保护故障，将控制权传递给内核中的异常处理程序。一般这种异常被称为“段错误（Segmentation Fault）”。

#### 页命中

------



![页命中](https://user-gold-cdn.xitu.io/2017/10/31/3c181692ba0db9c6dcad263a8cd7cf47?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)页命中



如上图所示，MMU根据虚拟地址在页表中寻址到了`PTE 4`，该PTE的有效位为1，代表该虚拟页已经被缓存在物理内存中了，最终MMU得到了PTE中的物理内存地址（指向`PP 1`）。

#### 缺页

------



![缺页](https://user-gold-cdn.xitu.io/2017/10/31/ae4f92f5f10d92a8332864ae0604a636?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)缺页



如上图所示，MMU根据虚拟地址在页表中寻址到了`PTE 2`，该PTE的有效位为0，代表该虚拟页并没有被缓存在物理内存中。**虚拟页没有被缓存在物理内存中（缓存未命中）被称为缺页。**

**当CPU遇见缺页时会触发一个缺页异常，缺页异常将控制权转向操作系统内核，然后调用内核中的缺页异常处理程序，该程序会选择一个牺牲页，如果牺牲页已被修改过，内核会先将它复制回硬盘（采用写回机制而不是直写也是为了尽量减少对硬盘的访问次数），然后再把该虚拟页覆盖到牺牲页的位置，并且更新PTE。**

**当缺页异常处理程序返回时，它会重新启动导致缺页的指令，该指令会把导致缺页的虚拟地址重新发送给MMU**。由于现在已经成功处理了缺页异常，所以最终结果是页命中，并得到物理地址。

这种在硬盘和内存之间传送页的行为称为页面调度（paging）：页从硬盘换入内存和从内存换出到硬盘。**当缺页异常发生时，才将页面换入到内存的策略称为按需页面调度（demand paging），所有现代操作系统基本都使用的是按需页面调度的策略。**

**虚拟内存跟CPU高速缓存（或其他使用缓存的技术）一样依赖于局部性原则**。虽然处理缺页消耗的性能很多（毕竟还是要从硬盘中读取），而且程序在运行过程中引用的不同虚拟页的总数可能会超出物理内存的大小，但是**局部性原则保证了在任意时刻，程序将趋向于在一个较小的活动页面（active page）集合上工作，这个集合被称为工作集（working set）**。根据空间局部性原则（**一个被访问过的内存地址以及其周边的内存地址都会有很大几率被再次访问**）与时间局部性原则（**一个被访问过的内存地址在之后会有很大几率被再次访问**），只要将工作集缓存在物理内存中，接下来的地址翻译请求很大几率都在其中，从而减少了额外的硬盘流量。

如果一个程序没有良好的局部性，将会使工作集的大小不断膨胀，直至超过物理内存的大小，这时程序会产生一种叫做抖动（thrashing）的状态，页面会不断地换入换出，如此多次的读写硬盘开销，性能自然会十分“恐怖”。**所以，想要编写出性能高效的程序，首先要保证程序的时间局部性与空间局部性。**

#### 多级页表

------

我们目前为止讨论的只是单页表，但在实际的环境中虚拟空间地址都是很大的（一个32位系统的地址空间有`2^32 = 4GB`，更别说64位系统了）。在这种情况下，使用一个单页表明显是效率低下的。

**常用方法是使用层次结构的页表**。假设我们的环境为一个32位的虚拟地址空间，它有如下形式：

- 虚拟地址空间被分为4KB的页，每个PTE都是4字节。
- 内存的前2K个页面分配给了代码和数据。
- 之后的6K个页面还未被分配。
- 再接下来的1023个页面也未分配，其后的1个页面分配给了用户栈。

下图是为该虚拟地址空间构造的二级页表层次结构（真实情况中多为四级或更多），一级页表（1024个PTE正好覆盖4GB的虚拟地址空间，同时每个PTE只有4字节，这样一级页表与二级页表的大小也正好与一个页面的大小一致都为4KB）的每个PTE负责映射虚拟地址空间中一个4MB的片（chunk），每一片都由1024个连续的页面组成。二级页表中的每个PTE负责映射一个4KB的虚拟内存页面。



![img](https://user-gold-cdn.xitu.io/2017/10/31/9eb1c115f4d96c533c61c01ea4c5ef04?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



这个结构看起来很像是一个`B-Tree`，这种层次结构有效的减缓了内存要求：

- 如果一个一级页表的一个PTE是空的，那么相应的二级页表也不会存在。这代表一种巨大的潜在节约（对于一个普通的程序来说，虚拟地址空间的大部分都会是未分配的）。
- 只有一级页表才总是需要缓存在内存中的，这样虚拟内存系统就可以在需要时创建、页面调入或调出二级页表（只有经常使用的二级页表才会被缓存在内存中），这就减少了内存的压力。

### 地址翻译的过程

------

从形式上来说，**地址翻译是一个N元素的虚拟地址空间中的元素和一个M元素的物理地址空间中元素之间的映射。**

下图为MMU利用页表进行寻址的过程：



![img](https://user-gold-cdn.xitu.io/2017/10/31/c7bf4fc683ff989b37bad182e4fda0f9?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



页表基址寄存器（PTBR）指向当前页表。**一个n位的虚拟地址包含两个部分，一个p位的虚拟页面偏移量（Virtual Page Offset, VPO）和一个（n - p）位的虚拟页号（Virtual Page Number, VPN）。**

**MMU根据VPN来选择对应的PTE**，例如`VPN 0`代表`PTE 0`、`VPN 1`代表`PTE 1`....因为物理页与虚拟页的大小是一致的，所以物理页面偏移量（Physical Page Offset, PPO）与VPO是相同的。那么之后**只要将PTE中的物理页号（Physical Page Number, PPN）与虚拟地址中的VPO串联起来，就能得到相应的物理地址**。

多级页表的地址翻译也是如此，只不过因为有多个层次，所以VPN需要分成多段。**假设有一个k级页表，虚拟地址会被分割成k个VPN和1个VPO，每个`VPN i`都是一个到第i级页表的索引**。为了构造物理地址，MMU需要访问k个PTE才能拿到对应的PPN。



![img](https://user-gold-cdn.xitu.io/2017/10/31/04c2514a46f77fb489eb028d3ca57ad9?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



#### TLB

------

页表是被缓存在内存中的，尽管内存的速度相对于硬盘来说已经非常快了，但与CPU还是有所差距。**为了防止每次地址翻译操作都需要去访问内存，CPU使用了高速缓存与TLB来缓存PTE。**

在最糟糕的情况下（不包括缺页），MMU需要访问内存取得相应的PTE，这个代价大约为几十到几百个周期，如果PTE凑巧缓存在L1高速缓存中（如果L1没有还会从L2中查找，不过我们忽略多级缓冲区的细节），那么性能开销就会下降到1个或2个周期。然而，许多系统甚至需要消除即使这样微小的开销，TLB由此而生。



![img](https://user-gold-cdn.xitu.io/2017/10/31/06e5eb158cd818b9e04056ab959f3060?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



TLB（Translation Lookaside Buffer, TLB）被称为翻译后备缓冲器或翻译旁路缓冲器，它是**MMU中的一个缓冲区，其中每一行都保存着一个由单个PTE组成的块。用于组选择和行匹配的索引与标记字段是从VPN中提取出来的，如果TLB中有`T = 2^t`个组，那么TLB索引（TLBI）是由VPN的t个最低位组成的，而TLB标记（TLBT）是由VPN中剩余的位组成的。**

下图为地址翻译的流程（TLB命中的情况下）：



![img](https://user-gold-cdn.xitu.io/2017/10/31/bce825a3d3d87894a65e550fdba92f36?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



- 第一步，CPU将一个虚拟地址交给MMU进行地址翻译。
- 第二步和第三步，MMU通过TLB取得相应的PTE。
- 第四步，MMU通过PTE翻译出物理地址并将它发送给高速缓存/内存。
- 第五步，高速缓存返回数据到CPU（如果缓存命中的话，否则还需要访问内存）。

**当TLB未命中时，MMU必须从高速缓存/内存中取出相应的PTE，并将新取得的PTE存放到TLB（如果TLB已满会覆盖一个已经存在的PTE）。**



![img](https://user-gold-cdn.xitu.io/2017/10/31/92dbaef67abacddbeff7cfd76039e57a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### Linux中的虚拟内存系统

------

**Linux为每个进程维护了一个单独的虚拟地址空间**。虚拟地址空间分为内核空间与用户空间，用户空间包括代码、数据、堆、共享库以及栈，内核空间包括内核中的代码和数据结构，内核空间的某些区域被映射到所有进程共享的物理页面。**Linux也将一组连续的虚拟页面（大小等于内存总量）映射到相应的一组连续的物理页面，这种做法为内核提供了一种便利的方法来访问物理内存中任何特定的位置。**



![img](https://user-gold-cdn.xitu.io/2017/10/31/dffc20ef2fa8bfb5c6dde65ab9938c8d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



**Linux将虚拟内存组织成一些区域（也称为段）的集合，区域的概念允许虚拟地址空间有间隙。一个区域就是已经存在着的已分配的虚拟内存的连续片（chunk）**。例如，代码段、数据段、堆、共享库段，以及用户栈都属于不同的区域，**每个存在的虚拟页都保存在某个区域中，而不属于任何区域的虚拟页是不存在的，也不能被进程所引用。**

内核为系统中的每个进程维护一个单独的任务结构（task_struct）。**任务结构中的元素包含或者指向内核运行该进程所需的所有信息（PID、指向用户栈的指针、可执行目标文件的名字、程序计数器等）。**



![img](https://user-gold-cdn.xitu.io/2017/10/31/523e8ef97804fd93a450859c74c4a69e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



- mm_struct：描述了虚拟内存的当前状态。pgd指向一级页表的基址（当内核运行这个进程时，pgd会被存放在CR3控制寄存器，也就是页表基址寄存器中），mmap指向一个vm_area_structs的链表，其中每个vm_area_structs都描述了当前虚拟地址空间的一个区域。
- vm_starts：指向这个区域的起始处。
- vm_end：指向这个区域的结束处。
- vm_prot：描述这个区域内包含的所有页的读写许可权限。
- vm_flags：描述这个区域内的页面是与其他进程共享的，还是这个进程私有的以及一些其他信息。
- vm_next：指向链表的下一个区域结构。

### 内存映射

------

**Linux通过将一个虚拟内存区域与一个硬盘上的文件关联起来，以初始化这个虚拟内存区域的内容，这个过程称为内存映射（memory mapping）。这种将虚拟内存系统集成到文件系统的方法可以简单而高效地把程序和数据加载到内存中。**

一个区域可以映射到一个普通硬盘文件的连续部分，例如一个可执行目标文件。文件区（section）被分成页大小的片，每一片包含一个虚拟页的初始内容。**由于按需页面调度的策略，这些虚拟页面没有实际交换进入物理内存，直到CPU引用的虚拟地址在该区域的范围内**。如果区域比文件区要大，那么就用零来填充这个区域的余下部分。

**一个区域也可以映射到一个匿名文件，匿名文件是由内核创建的，包含的全是二进制零**。当CPU第一次引用这样一个区域内的虚拟页面时，内核就在物理内存中找到一个合适的牺牲页面，如果该页面被修改过，就先将它写回到硬盘，之后用二进制零覆盖牺牲页并更新页表，将这个页面标记为已缓存在内存中的。

简单的来说：**普通文件映射就是将一个文件与一块内存建立起映射关系，对该文件进行IO操作可以绕过内核直接在用户态完成（用户态在该虚拟地址区域读写就相当于读写这个文件）。匿名文件映射一般在用户空间需要分配一段内存来存放数据时，由内核创建匿名文件并与内存进行映射，之后用户态就可以通过操作这段虚拟地址来操作内存了。匿名文件映射最熟悉的应用场景就是动态内存分配（malloc()函数）。**

Linux很多地方都采用了“懒加载”机制，自然也包括内存映射。不管是普通文件映射还是匿名映射，Linux只会先划分虚拟内存地址。只有当CPU第一次访问该区域内的虚拟地址时，才会真正的与物理内存建立映射关系。

**只要虚拟页被初始化了，它就在一个由内核维护的交换文件（swap file）之间换来换去。交换文件又称为交换空间（swap space）或交换区域（swap area）。swap区域不止用于页交换，在物理内存不够的情况下，还会将部分内存数据交换到swap区域（使用硬盘来扩展内存）。**

#### 共享对象

------

虚拟内存系统为每个进程提供了私有的虚拟地址空间，这样可以保证进程之间不会发生错误的读写。但多个进程之间也含有相同的部分，例如每个C程序都使用到了C标准库，如果每个进程都在物理内存中保持这些代码的副本，那会造成很大的内存资源浪费。

**内存映射提供了共享对象的机制，来避免内存资源的浪费。一个对象被映射到虚拟内存的一个区域，要么是作为共享对象，要么是作为私有对象的。**

如果一个进程将一个共享对象映射到它的虚拟地址空间的一个区域内，那么这个进程对这个区域的任何写操作，对于那些也把这个共享对象映射到它们虚拟内存的其他进程而言，也是可见的。相对的，对一个映射到私有对象的区域的任何写操作，对于其他进程来说是不可见的。一个映射到共享对象的虚拟内存区域叫做共享区域，类似地，也有私有区域。

**为了节约内存，私有对象开始的生命周期与共享对象基本上是一致的（在物理内存中只保存私有对象的一份副本），并使用写时复制的技术来应对多个进程的写冲突。**



![img](https://user-gold-cdn.xitu.io/2017/10/31/ee41a18da08ff1117d2f6f94e70b679f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



只要没有进程试图写它自己的私有区域，那么多个进程就可以继续共享物理内存中私有对象的一个单独副本。然而，只要有一个进程试图对私有区域的某一页面进行写操作，就会触发一个保护异常。在上图中，进程B试图对私有区域的一个页面进行写操作，该操作触发了保护异常。**异常处理程序会在物理内存中创建这个页面的一个新副本，并更新PTE指向这个新的副本，然后恢复这个页的可写权限。**

还有一个典型的例子就是`fork()`函数，该函数用于创建子进程。当`fork()`函数被当前进程调用时，内核会为新进程创建各种必要的数据结构，并分配给它一个唯一的PID。为了给新进程创建虚拟内存，它复制了当前进程的`mm_struct`、`vm_area_struct`和页表的原样副本。并将两个进程的每个页面都标为只读，两个进程中的每个区域都标记为私有区域（写时复制）。

这样，父进程和子进程的虚拟内存空间完全一致，只有当这两个进程中的任一个进行写操作时，再使用写时复制来保证每个进程的虚拟地址空间私有的抽象概念。

### 动态内存分配

------

虽然可以使用内存映射（`mmap()`函数）来创建和删除虚拟内存区域来满足运行时动态内存分配的问题。然而，为了更好的移植性与便利性，还需要一个更高层面的抽象，也就是动态内存分配器（dynamic memory allocator）。

**动态内存分配器维护着一个进程的虚拟内存区域，也就是我们所熟悉的“堆（heap）”，内核中还维护着一个指向堆顶的指针brk（break）。动态内存分配器将堆视为一个连续的虚拟内存块（chunk）的集合，每个块有两种状态，已分配和空闲。已分配的块显式地保留为供应用程序使用，空闲块则可以用来进行分配，它的空闲状态直到它显式地被应用程序分配为止。已分配的块要么被应用程序显式释放，要么被垃圾回收器所释放。**



![img](https://user-gold-cdn.xitu.io/2017/10/31/90463587870795921ceb3f480e75c44c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



本文只讲解动态内存分配的一些概念，关于动态内存分配器的实现已经超出了本文的讨论范围。如果有对它感兴趣的同学，可以去参考[dlmalloc](http://gee.cs.oswego.edu/)的源码，它是由Doug Lea（就是写Java并发包的那位）实现的一个设计巧妙的内存分配器，而且源码中的注释十分多。

#### 内存碎片

------

**造成堆的空间利用率很低的主要原因是一种被称为碎片（fragmentation）的现象，当虽然有未使用的内存但这块内存并不能满足分配请求时，就会产生碎片**。有以下两种形式的碎片：

- 内部碎片：在一个已分配块比有效载荷大时发生。例如，程序请求一个5字（这里我们不纠结字的大小，假设一个字为4字节，堆的大小为16字并且要保证边界双字对齐）的块，内存分配器为了保证空闲块是双字边界对齐的（具体实现中对齐的规定可能略有不同，但对齐是肯定会有的），只好分配一个6字的块。在本例中，已分配块为6字，有效载荷为5字，内部碎片为已分配块减去有效载荷，为1字。
- 外部碎片：当空闲内存合计起来足够满足一个分配请求，但是没有一个单独的空闲块足够大到可以来处理这个请求时发生。外部碎片难以量化且不可预测，所以分配器通常采用启发式策略来试图维持少量的大空闲块，而不是维持大量的小空闲块。分配器也会根据策略与分配请求的匹配来分割空闲块与合并空闲块（必须相邻）。

#### 空闲链表

------

**分配器将堆组织为一个连续的已分配块和空闲块的序列，该序列被称为空闲链表**。空闲链表分为隐式空闲链表与显式空闲链表。

- 隐式空闲链表，是一个单向链表，并且每个空闲块仅仅是通过头部中的大小字段隐含地连接着的。
- 显式空闲链表，即是将空闲块组织为某种形式的显式数据结构（为了更加高效地合并与分割空闲块）。例如，将堆组织为一个双向空闲链表，在每个空闲块中，都包含一个前驱节点的指针与后继节点的指针。

查找一个空闲块一般有如下几种策略：

- 首次适配：从头开始搜索空闲链表，选择第一个遇见的合适的空闲块。它的优点在于趋向于将大的空闲块保留在链表的后面，缺点是它趋向于在靠近链表前部处留下碎片。
- 下一次适配：每次从上一次查询结束的地方开始进行搜索，直到遇见合适的空闲块。这种策略通常比首次适配效率高，但是内存利用率则要低得多了。
- 最佳适配：检查每个空闲块，选择适合所需请求大小的最小空闲块。最佳适配的内存利用率是三种策略中最高的，但它需要对堆进行彻底的搜索。

对一个链表进行查找操作的效率是线性的，为了减少分配请求对空闲块匹配的时间，**分配器通常采用分离存储（segregated storage）的策略，即是维护多个空闲链表，其中每个链表的块有大致相等的大小。**

一种简单的分离存储策略：分配器维护一个空闲链表数组，然后将所有可能的块分成一些等价类（也叫做大小类（size class）），每个大小类代表一个空闲链表，并且每个大小类的空闲链表包含大小相等的块，每个块的大小就是这个大小类中最大元素的大小（例如，某个大小类的范围定义为（17~32），那么这个空闲链表全由大小为32的块组成）。

当有一个分配请求时，我们检查相应的空闲链表。如果链表非空，那么就分配其中第一块的全部。如果链表为空，分配器就向操作系统请求一个固定大小的额外内存片，将这个片分成大小相等的块，然后将这些块链接起来形成新的空闲链表。

要释放一个块，分配器只需要简单地将这个块插入到相应的空闲链表的头部。

#### 垃圾回收

------

在编写C程序时，一般只能显式地分配与释放堆中的内存（`malloc()`与`free()`），程序员不仅需要分配内存，还需要负责内存的释放。

许多现代编程语言都内置了自动内存管理机制（通过引入自动内存管理库也可以让C/C++实现自动内存管理），**所谓自动内存管理，就是自动判断不再需要的堆内存（被称为垃圾内存），然后自动释放这些垃圾内存。**

自动内存管理的实现是垃圾收集器（garbage collector），它是一种动态内存分配器，它会自动释放应用程序不再需要的已分配块。

垃圾收集器一般采用以下两种（之一）的策略来判断一块堆内存是否为垃圾内存：

- 引用计数器：在数据的物理空间中添加一个计数器，当有其他数据与其相关时（引用），该计数器加一，反之则减一。通过定期检查计数器的值，只要为0则认为是垃圾内存，可以释放它所占用的已分配块。使用引用计数器，实现简单直接，但缺点也很明显，它无法回收循环引用的两个对象（假设有对象A与对象B，它们2个互相引用，但实际上对象A与对象B都已经是没用的对象了）。
- 可达性分析：垃圾收集器将堆内存视为一张有向图，然后选出一组根节点（例如，在Java中一般为类加载器、全局变量、运行时常量池中的引用类型变量等），根节点必须是足够“活跃“的对象。然后计算从根节点集合出发的可达路径，只要从根节点出发不可达的节点，都视为垃圾内存。

垃圾收集器进行回收的算法有如下几种：

- 标记-清除：该算法分为标记（mark）和清除（sweep）两个阶段。首先标记出所有需要回收的对象，然后在标记完成后统一回收所有被标记的对象。标记-清除算法实现简单，但它的效率不高，而且会产生许多内存碎片。
- 标记-整理：标记-整理与标记-清除算法基本一致，只不过后续步骤不是直接对可回收对象进行清理，而是让所有存活的对象都向一端移动，然后直接清理掉边界以外的内存。
- 复制：**将程序所拥有的内存空间划分为大小相等的两块，每次都只使用其中的一块。当这一块的内存用完了，就把还存活着的对象复制到另一块内存上，然后将已使用过的内存空间进行清理**。这种方法不必考虑内存碎片问题，但内存利用率很低。这个比例不是绝对的，像HotSpot虚拟机为了避免浪费，将内存划分为Eden空间与两个Survivor空间，每次都只使用Eden和其中一个Survivor。当回收时，将Eden和Survivor中还存活着的对象一次性地复制到另外一个Survivor空间上，然后清理掉Eden和刚才使用过的Survivor空间。HotSpot虚拟机默认的Eden和Survivor的大小比例为8：1，只有10%的内存空间会被闲置浪费。
- 分代：**分代算法根据对象的存活周期的不同将内存划分为多块，这样就可以对不同的年代采用不同的回收算法**。一般分为新生代与老年代，新生代存放的是存活率较低的对象，可以采用复制算法；老年代存放的是存活率较高的对象，如果使用复制算法，那么内存空间会不够用，所以必须使用标记-清除或标记-整理算法。

### 总结

------

虚拟内存是对内存的一个抽象。支持虚拟内存的CPU需要通过虚拟寻址的方式来引用内存中的数据。CPU加载一个虚拟地址，然后发送给MMU进行地址翻译。地址翻译需要硬件与操作系统之间紧密合作，MMU借助页表来获得物理地址。

- 首先，MMU先将虚拟地址发送给TLB以获得PTE（根据VPN寻址）。
- 如果恰好TLB中缓存了该PTE，那么就返回给MMU，否则MMU需要从高速缓存/内存中获得PTE，然后更新缓存到TLB。
- MMU获得了PTE，就可以从PTE中获得对应的PPN，然后结合VPO构造出物理地址。
- 如果在PTE中发现该虚拟页没有缓存在内存，那么会触发一个缺页异常。缺页异常处理程序会把虚拟页缓存进物理内存，并更新PTE。异常处理程序返回后，CPU会重新加载这个虚拟地址，并进行翻译。

虚拟内存系统简化了内存管理、链接、加载、代码和数据的共享以及访问权限的保护：

- 简化链接，独立的地址空间允许每个进程的内存映像使用相同的基本格式，而不管代码和数据实际存放在物理内存的何处。
- 简化加载，虚拟内存使向内存中加载可执行文件和共享对象文件变得更加容易。
- 简化共享，独立的地址空间为操作系统提供了一个管理用户进程和内核之间共享的一致机制。
- 访问权限保护，每个虚拟地址都要经过查询PTE的过程，在PTE中设定访问权限的标记位从而简化内存的权限保护。

操作系统通过将虚拟内存与文件系统结合的方式，来初始化虚拟内存区域，这个过程称为内存映射。应用程序显式分配内存的区域叫做堆，通过动态内存分配器来直接操作堆内存。

# 多级页表节约内存

在学习计算机组成原理时，书中谈到，"使用多级页表可以压缩页表占用的内存"，在了解了多级页表的原理后，恐怕对这句话还是理解不了：把页表换成多级页表了就能节约内存了？不是还是得映射所有的虚拟地址空间么？

比如做个简单的数学计算，假如虚拟地址空间为32位（即4GB）、每个页面映射4KB以及每条页表项占4B，则进程需要1M个页表项（`4GB / 4KB = 1M`），即页表（每个进程都有一个页表）占用4MB（`1M * 4B = 4MB`）的内存空间。而假如我们使用二级页表，还是上述条件，但一级页表映射4MB、二级页表映射4KB，则需要1K个一级页表项（`4GB / 4MB = 1K`）、每个一级页表项对应1K个二级页表项（`4MB / 4KB = 1K`），这样页表占用4.004MB（`1K * 4B + 1K * 1K * 4B = 4.004MB`）的内存空间。多级页表的内存空间占用反而变大了？

其实我们应该换个角度来看问题，还记得计算机组成原理里面无处不在的局部性原理么？



## 如何节约内存

我们分两方面来谈这个问题：第一，二级页表可以不存在；第二，二级页表可以不在主存。



### 二级页表可以不存在

我们反过来想，每个进程都有4GB的虚拟地址空间，而显然对于大多数程序来说，其使用到的空间远未达到4GB，何必去映射不可能用到的空间呢？

也就是说，一级页表覆盖了整个4GB虚拟地址空间，但如果某个一级页表的页表项没有被用到，也就不需要创建这个页表项对应的二级页表了，即可以在需要时才创建二级页表。做个简单的计算，假设只有20%的一级页表项被用到了，那么页表占用的内存空间就只有0.804MB（`1K * 4B + 0.2 * 1K * 1K * 4B = 0.804MB`），对比单级页表的4M是不是一个巨大的节约？

那么为什么不分级的页表就做不到这样节约内存呢？我们从页表的性质来看，保存在主存中的页表承担的职责是将虚拟地址翻译成物理地址；假如虚拟地址在页表中找不到对应的页表项，计算机系统就不能工作了。所以页表一定要覆盖全部虚拟地址空间，**不分级的页表就需要有1M个页表项来映射，而二级页表则最少只需要1K个页表项**（此时一级页表覆盖到了全部虚拟地址空间，二级页表在需要时创建）。



### 二级页表可以不在主存

其实这就像是把页表当成了页面。回顾一下请求分页存储管理，当需要用到某个页面时，将此页面从磁盘调入到内存；当内存中页面满了时，将内存中的页面调出到磁盘，这是利用到了程序运行的局部性原理。我们可以很自然发现，虚拟内存地址存在着局部性，那么负责映射虚拟内存地址的页表项当然也存在着局部性了！这样我们再来看二级页表，根据局部性原理，1024个第二级页表中，只会有很少的一部分在某一时刻正在使用，我们岂不是可以把二级页表都放在磁盘中，在需要时才调入到内存？我们考虑极端情况，只有一级页表在内存中，二级页表仅有一个在内存中，其余全在磁盘中（虽然这样效率非常低），则此时页表占用了8KB（`1K * 4B + 1 * 1K * 4B = 8KB`），对比上一步的0.804MB，占用空间又缩小了好多倍！



## 总结

我们把二级页表再推广到多级页表，就会发现页表占用的内存空间更少了，这一切都要归功于对局部性原理的充分应用。

回头想想，这么大幅度地解决内存空间，我们失去了什么呢？计算机的很多问题无外乎就是时间换空间和空间换时间了，而多级页表就是典型的时间换空间的例子了，动态创建二级页表、调入和调出二级页表都是需要花费额外时间的，远没有不分级的页表来的直接；而我们也仅仅是利用局部性原理让这个额外时间开销降得比较低了而已。

# 多级反馈队列

通常在使用多级队列调度算法时，进程进入系统时被永久地分配到某个队列。例如，如果前台和后台进程分别具有单独队列，那么进程并不从一个队列移到另一个队列，这是因为进程不会改变前台或后台的性质。这种设置的优点是调度开销低，缺点是不够灵活。

相反，多级反馈队列调度算法允许进程在队列之间迁移。这种想法是，根据不同 CPU 执行的特点来区分进程。如果进程使用过多的 CPU 时间，那么它会被移到更低的优先级队列。这种方案将 I/O 密集型和交互进程放在更高优先级队列上。 此外，在较低优先级队列中等待过长的进程会被移到更高优先级队列。这种形式的老化可阻止饥饿的发生。

![多级反馈队列](http://c.biancheng.net/uploads/allimg/181106/2-1Q106162444303.gif)
图 1 多级反馈队列


例如，一个多级反馈队列的调度程序有三个队列，从 0 到 2（图 1）。调度程序首先执行队列 0 内的所有进程。只有当队列 0 为空时，它才能执行队列 1 内的进程。类似地，只有队列 0 和 1 都为空时，队列 2 的进程才能执行。到达队列 1 的进程会抢占队列 2 的进程。同样，到达队列 0 的进程会抢占队列 1 的进程。

**每个进程在进入就绪队列后，就被添加到队列 0 内。队列 0 内的每个进程都有 8ms 的时间片。如果一个进程不能在这一时间片内完成，那么它就被移到队列 1 的尾部。如果队列 0 为空，队列 1 头部的进程会得到一个 16ms 的时间片。如果它不能完成，那么将被抢占，并添加到队列 2。只有当队列 0 和 1 为空时，队列 2 内的进程才可根据[ FCFS](http://c.biancheng.net/view/1242.html) 来运行。**

这种调度算法将给那些 CPU 执行不超过 8ms 的进程最高优先级。这类进程可以很快得到 CPU，完成 CPU 执行，并且处理下个 I/O 执行。

所需超过 8ms 但不超过 24ms 的进程也会很快得以服务，但是它们的优先级要低一点。长进程会自动沉入队列 2，队列 0 和 1 不用的 CPU 周期按 FCFS 顺序来服务。

通常，多级反馈队列调度程序可由下列参数来定义：

1. 队列数量。
2. 每个队列的调度算法。
3. 用以确定何时升级到更高优先级队列的方法。
4. 用以确定何时降级到更低优先级队列的方法。
5. 用以确定进程在需要服务时将会进入哪个队列的方法。


多级反馈队列调度程序的定义使其成为最通用的 CPU 调度算法。通过配置，它能适应所设计的特定系统。遗憾的是，由于需要一些方法来选择参数以定义最佳的调度程序，所以它也是最复杂的算法。

# MySQL索引原理

### 索引目的

索引的目的在于提高查询效率，可以类比字典，如果要查“mysql”这个单词，我们肯定需要定位到m字母，然后从下往下找到y字母，再找到剩下的sql。如果没有索引，那么你可能需要把所有单词看一遍才能找到你想要的，如果我想找到m开头的单词呢？或者ze开头的单词呢？是不是觉得如果没有索引，这个事情根本无法完成？

### 索引原理

除了词典，生活中随处可见索引的例子，如火车站的车次表、图书的目录等。它们的原理都是一样的，通过不断的缩小想要获得数据的范围来筛选出最终想要的结果，同时把随机的事件变成顺序的事件，也就是我们总是通过同一种查找方式来锁定数据。

数据库也是一样，但显然要复杂许多，因为不仅面临着等值查询，还有范围查询(>、<、between、in)、模糊查询(like)、并集查询(or)等等。数据库应该选择怎么样的方式来应对所有的问题呢？我们回想字典的例子，能不能把数据分成段，然后分段查询呢？最简单的如果1000条数据，1到100分成第一段，101到200分成第二段，201到300分成第三段……这样查第250条数据，只要找第三段就可以了，一下子去除了90%的无效数据。但如果是1千万的记录呢，分成几段比较好？稍有算法基础的同学会想到搜索树，其平均复杂度是lgN，具有不错的查询性能。但这里我们忽略了一个关键的问题，复杂度模型是基于每次相同的操作成本来考虑的，数据库实现比较复杂，数据保存在磁盘上，而为了提高性能，每次又可以把部分数据读入内存来计算，因为我们知道访问磁盘的成本大概是访问内存的十万倍左右，所以简单的搜索树难以满足复杂的应用场景。

#### 磁盘IO与预读

前面提到了访问磁盘，那么这里先简单介绍一下磁盘IO和预读，磁盘读取数据靠的是机械运动，每次读取数据花费的时间可以分为寻道时间、旋转延迟、传输时间三个部分，寻道时间指的是磁臂移动到指定磁道所需要的时间，主流磁盘一般在5ms以下；旋转延迟就是我们经常听说的磁盘转速，比如一个磁盘7200转，表示每分钟能转7200次，也就是说1秒钟能转120次，旋转延迟就是1/120/2 = 4.17ms；传输时间指的是从磁盘读出或将数据写入磁盘的时间，一般在零点几毫秒，相对于前两个时间可以忽略不计。那么访问一次磁盘的时间，即一次磁盘IO的时间约等于5+4.17 = 9ms左右，听起来还挺不错的，但要知道一台500 -MIPS的机器每秒可以执行5亿条指令，因为指令依靠的是电的性质，换句话说执行一次IO的时间可以执行40万条指令，数据库动辄十万百万乃至千万级数据，每次9毫秒的时间，显然是个灾难。下图是计算机硬件延迟的对比图，供大家参考：

![various-system-software-hardware-latencies](https://awps-assets.meituan.net/mit-x/blog-images-bundle-2014/7f46a0a4.png)

various-system-software-hardware-latencies



考虑到磁盘IO是非常高昂的操作，计算机操作系统做了一些优化，当一次IO时，不光把当前磁盘地址的数据，而是把相邻的数据也都读取到内存缓冲区内，因为局部预读性原理告诉我们，当计算机访问一个地址的数据的时候，与其相邻的数据也会很快被访问到。每一次IO读取的数据我们称之为一页(page)。具体一页有多大数据跟操作系统有关，一般为4k或8k，也就是我们读取一页内的数据时候，实际上才发生了一次IO，这个理论对于索引的数据结构设计非常有帮助。

#### 索引的数据结构

前面讲了生活中索引的例子，索引的基本原理，数据库的复杂性，又讲了操作系统的相关知识，目的就是让大家了解，任何一种数据结构都不是凭空产生的，一定会有它的背景和使用场景，我们现在总结一下，我们需要这种数据结构能够做些什么，其实很简单，那就是：每次查找数据时把磁盘IO次数控制在一个很小的数量级，最好是常数数量级。那么我们就想到如果一个高度可控的多路搜索树是否能满足需求呢？就这样，b+树应运而生。

#### 详解b+树

![b+树](https://awps-assets.meituan.net/mit-x/blog-images-bundle-2014/7af22798.jpg)

b+树



如上图，是一颗b+树，关于b+树的定义可以参见[B+树](http://zh.wikipedia.org/wiki/B%2B树)，这里只说一些重点，浅蓝色的块我们称之为一个磁盘块，可以看到每个磁盘块包含几个数据项（深蓝色所示）和指针（黄色所示），如磁盘块1包含数据项17和35，包含指针P1、P2、P3，P1表示小于17的磁盘块，P2表示在17和35之间的磁盘块，P3表示大于35的磁盘块。真实的数据存在于叶子节点即3、5、9、10、13、15、28、29、36、60、75、79、90、99。非叶子节点只不存储真实的数据，只存储指引搜索方向的数据项，如17、35并不真实存在于数据表中。

#### b+树的查找过程

如图所示，如果要查找数据项29，那么首先会把磁盘块1由磁盘加载到内存，此时发生一次IO，在内存中用二分查找确定29在17和35之间，锁定磁盘块1的P2指针，内存时间因为非常短（相比磁盘的IO）可以忽略不计，通过磁盘块1的P2指针的磁盘地址把磁盘块3由磁盘加载到内存，发生第二次IO，29在26和30之间，锁定磁盘块3的P2指针，通过指针加载磁盘块8到内存，发生第三次IO，同时内存中做二分查找找到29，结束查询，总计三次IO。真实的情况是，3层的b+树可以表示上百万的数据，如果上百万的数据查找只需要三次IO，性能提高将是巨大的，如果没有索引，每个数据项都要发生一次IO，那么总共需要百万次的IO，显然成本非常非常高。

#### b+树性质

1.通过上面的分析，我们知道IO次数取决于b+数的高度h，假设当前数据表的数据为N，每个磁盘块的数据项的数量是m，则有h=㏒(m+1)N，当数据量N一定的情况下，m越大，h越小；而m = 磁盘块的大小 / 数据项的大小，磁盘块的大小也就是一个数据页的大小，是固定的，如果数据项占的空间越小，数据项的数量越多，树的高度越低。这就是为什么每个数据项，即索引字段要尽量的小，比如int占4字节，要比bigint8字节少一半。这也是为什么b+树要求把真实的数据放到叶子节点而不是内层节点，一旦放到内层节点，磁盘块的数据项会大幅度下降，导致树增高。当数据项等于1时将会退化成线性表。

2.当b+树的数据项是复合的数据结构，比如(name,age,sex)的时候，b+数是按照从左到右的顺序来建立搜索树的，比如当(张三,20,F)这样的数据来检索的时候，b+树会优先比较name来确定下一步的所搜方向，如果name相同再依次比较age和sex，最后得到检索的数据；但当(20,F)这样的没有name的数据来的时候，b+树就不知道下一步该查哪个节点，因为建立搜索树的时候name就是第一个比较因子，必须要先根据name来搜索才能知道下一步去哪里查询。比如当(张三,F)这样的数据来检索时，b+树可以用name来指定搜索方向，但下一个字段age的缺失，所以只能把名字等于张三的数据都找到，然后再匹配性别是F的数据了， 这个是非常重要的性质，即索引的最左匹配特性。

## 前言

当提到 MySQL 数据库的时候，我们的脑海里会想起几个关键字：索引、事务、数据库锁等等，**索引是 MySQL 的灵魂，是平时进行查询时的利器，也是面试中的重中之重**。



可能你了解索引的底层是 b+树，会加快查询，也会在表中建立索引，但这是远远不够的，这里列举几个索引常见的面试题：



**1、索引为什么要用 b+树这种数据结构？**



**2、聚集索引和非聚集索引的区别？**



**3、索引什么时候会失效，最左匹配原则是什么？**



当遇到这些问题的时候，可能会发现自己对索引还是一知半解，今天我们一起学习 MySQL 的索引。



## 一、一条查询语句是如何执行的

首先来看 **在 MySQL 数据库中，一条查询语句是如何执行的，索引出现在哪个环节，起到了什么作用**。



### 1.1 应用程序发现 SQL 到服务端

当执行 SQL 语句时，应用程序会连接到相应的数据库服务器，然后服务器对 SQL 进行处理。



### 1.2 查询缓存

接着数据库服务器会先去查询是否有该 SQL 语句的缓存，key 是查询的语句，value 是查询的结果。如果你的查询能够直接命中，就会直接从缓存中拿出 value 来返回客户端。



**注：查询不会被解析、不会生成执行计划、不会被执行。**



### 1.3 查询优化处理，生成执行计划

如果没有命中缓存，则开始第三步。



- **解析 SQL**：生成解析树，验证关键字如 select,where,left join 等）是否正确。
- **预处理**：进一步检查解析树是否合法，如 **检查数据表和列是否存在**，验证用户权限等。
- **优化 SQL**：**决定使用哪个索引**，或者在多个表相关联的时候决定表的连接顺序。紧接着，**将 SQL 语句转成执行计划**。



### 1.4 将查询结果返回客户端

最后，数据库服务器将查询结果返回给客户端。(如果查询可以缓存，MySQL 也会将结果放到查询缓存中)



![img](https://static001.infoq.cn/resource/image/43/fe/4383d050326b869fde80b6e5b937a2fe.png)



这就是一条查询语句的执行流程，可以看到索引出现在优化 SQL 的流程步骤中，接下来了解索引到底是什么？



## 二、索引概述

先简单地了解一下索引的基本概念。



### 2.1 索引是什么

**索引是帮助数据库高效获取数据的数据结构。**



### 2.2 索引的分类

#### 1）从存储结构上来划分

- Btree 索引（B+tree，B-tree)
- 哈希索引
- full-index 全文索引
- RTree



#### 2）从应用层次上来划分

- 普通索引：即一个索引只包含单个列，一个表可以有多个单列索引。
- 唯一索引：索引列的值必须唯一，但允许有空值。
- 复合索引：一个索引包含多个列。



#### 3）从表记录的排列顺序和索引的排列顺序是否一致来划分

- 聚集索引：表记录的排列顺序和索引的排列顺序一致。
- 非聚集索引：表记录的排列顺序和索引的排列顺序不一致。



### 2.3 聚集索引和非聚集索引

#### 1）简单概括

- 聚集索引：就是以主键创建的索引。
- 非聚集索引：就是以非主键创建的索引（也叫做二级索引）。



#### 2）详细概括

- **聚集索引**



聚集索引表记录的排列顺序和索引的排列顺序一致，所以查询效率快，因为只要找到第一个索引值记录，其余的连续性的记录在物理表中也会连续存放，一起就可以查询到。



**缺点**：新增比较慢，因为为了保证表中记录的物理顺序和索引顺序一致，在记录插入的时候，会对数据页重新排序。



- **非聚集索引**



索引的逻辑顺序与磁盘上行的物理存储顺序不同，非聚集索引在叶子节点存储的是主键和索引列，当我们使用非聚集索引查询数据时，需要**拿到叶子上的主键再去表中查到想要查找的数据。这个过程就是我们所说的回表。**



#### 3）聚集索引和非聚集索引的区别

- **聚集索引在叶子节点存储的是表中的数据。**
- **非聚集索引在叶子节点存储的是主键和索引列。**



**举个例子**



比如汉语字典，想要查「阿」字，只需要翻到字典前几页，a 开头的位置，接着「啊」「爱」都会出来。也就是说，**字典的正文部分本身就是一个目录，不需要再去查其他目录来找到需要找的内容**。我们 **把这种正文内容本身就是一种按照一定规则排列的目录称为==聚集索引==**。



如果遇到不认识的字，只能根据“偏旁部首”进行查找，然后根据这个字后的页码直接翻到某页来找到要找的字。但结合 **部首目录** 和 **检字表** 而查到的字的排序并不是真正的正文的排序方法。



![img](https://static001.infoq.cn/resource/image/e3/66/e32adeb5df653d52d274f60ac12e5866.png)



比如要查“玉”字，我们可以看到在查部首之后的检字表中“玉”的页码是 587 页，然后是珏，是 251 页。很显然，在字典中这两个字并没有挨着，现在看到的连续的“玉、珏、莹”三字实际上就是他们 **在非聚集索引中的排序，是字典正文中的字在非聚集索引中的映射**。我们可以通过这种方式来找到所需要的字，但它需要两个过程，先找到目录中的结果，然后再翻到结果所对应的页码。我们 **把这种目录纯粹是目录，正文纯粹是正文的排序方式称为==非聚集索引==**。



### 2.4 MySQL 如何添加索引

#### 1）添加 PRIMARY KEY（主键索引）

```
ALTER TABLE `table_name` ADD PRIMARY KEY ( `column` )
```

#### 2）添加 UNIQUE（唯一索引）

```
ALTER TABLE `table_name` ADD UNIQUE (`column`)
```

#### 3）添加 INDEX（普通索引）

```
ALTER TABLE `table_name` ADD INDEX index_name (`column` )
```

#### 4）添加 FULLTEXT（全文索引）

```
ALTER TABLE `table_name` ADD FULLTEXT (`column`)
```

#### 5）添加多列索引

```
ALTER TABLE `table_name` ADD INDEX index_name (`column1`,`column2`,`column3`)
```



## 三、索引底层数据结构

了解了索引的基本概念后，可能最好奇的就是 **索引的底层是怎么实现的呢？为什么索引可以如此高效地进行数据的查找？如何设计数据结构可以满足我们的要求？** 下文通过一般程序员的思维来想一下如果是我们来设计索引，要如何设计来达到索引的效果。



### 3.1 哈希索引

可能直接想到的就是用哈希表来实现快速查找，就像我们平时用的 hashmap 一样，value = get(key) O(1)时间复杂度一步到位，确实，**哈希索引** 是一种方式。



#### 1）定义

哈希索引就是采用一定的哈希算法，只需一次哈希算法即可立刻定位到相应的位置，速度非常快。**本质上就是把键值换算成新的哈希值，根据这个哈希值来定位**。



![img](https://static001.infoq.cn/resource/image/4f/71/4fcef0bbdee325d31a1c8728ffbcf471.png)



#### 2）局限性

- **哈希索引没办法利用索引完成排序。**
- **不能进行多字段查询。**
- **在有大量重复键值的情况下，哈希索引的效率也是极低的（出现哈希碰撞问题）。**
- **不支持范围查询。**



在 MySQL 常用的 InnoDB 引擎中，还是使用 B+树索引比较多。InnoDB 是自适应哈希索引的（hash 索引的创建由 **==InnoDB 存储引擎自动优化创建==**，我们干预不了）。



### 3.2 如何设计索引的数据结构呢

假设要查询某个区间的数据，我们只需要拿到区间的起始值，然后在树中进行查找。



如数据为：



![img](https://static001.infoq.cn/resource/image/f5/ab/f552f764b2c06e92b87d1a26197ad0ab.png)



#### 1）查询[7,30]区间的数据

![img](https://static001.infoq.cn/resource/image/fc/8f/fcf3d0cba537bbaa06b39c19b7df548f.png)



![img](https://static001.infoq.cn/resource/image/4b/90/4b96d12398e99211336163106fb30390.png)



当查找到起点节点 10 后，再顺着链表进行遍历，直到链表中的节点数据大于区间的终止值为止。**所有遍历到的数据，就是符合区间值的所有数据**。



#### 2）还可以怎么优化呢？

利用二叉查找树，区间查询的功能已经实现了。但是，为了节省内存，我们只能把树存储在硬盘中。



那么，**每个节点的读取或者访问，都对应一次硬盘 IO 操作。每次查询数据时磁盘 IO 操作的次数，也叫做==IO 渐进复杂度==，也就是==树的高度==**。



所以，我们要减少磁盘 IO 操作的次数，也就是 **要==降低树的高度==**。



结构优化过程如下图所示：



![img](https://static001.infoq.cn/resource/image/d3/56/d343fd0b5eb32559afc836b5b428a156.png)



![img](https://static001.infoq.cn/resource/image/6b/e6/6bd5c8355935ebda9dd83a37b143dee6.png)



![img](https://static001.infoq.cn/resource/image/85/9a/857daaac0c45811ca948a06500c2e99a.png)



这里将二叉树变为了 M 叉树，降低了树的高度，那么这个 M 应该选择多少才合适呢？



**问题：对于相同个数的数据构建 m 叉树索引，m 叉树中的 m 越大，那树的高度就越小，那 m 叉树中的 m 是不是越大越好呢？到底多大才合适呢？**



不管是内存中的数据还是磁盘中的数据，操作系统都是按页（一页的大小通常是 4kb，这个值可以通过`getconfig(PAGE_SIZE)`命令查看）来读取的，一次只会读取一页的数据。



如果要读取的数据量超过了一页的大小，就会触发多次 IO 操作。所以在选择 m 大小的时候，要尽量让每个节点的大小等于一个页的大小。



一般实际应用中，出度 d（树的分叉数）是非常大的数字，通常超过 100；**==树的高度（h）非常小，通常不超过 3==**。



### 3.3 B 树

顺着解决问题的思路知道了我们想要的数据结构是什么。目前索引常用的数据结构是 B+树，先介绍一下什么是 B 树（也就是 B-树）。



#### 1）B 树的特点：

- 关键字分布在整棵树的所有节点。
- 任何一个关键字 **出现且只出现在一个节点中**。
- 搜索有可能在 **非叶子节点** 结束。
- 其搜索性能等价于在关键字全集内做一次二分查找。如下图所示：



![img](https://static001.infoq.cn/resource/image/73/07/738dad15de38d0f053e0379be06d3607.png)



### 3.4 B+树

了解了 B 树，再来看一下 B+树，也是 MySQL 索引大部分情况所使用的数据结构。



**下图是一个 M=3 阶的 B+树**



![img](https://static001.infoq.cn/resource/image/05/96/0551516c9afd2623a96838cd0900c396.png)



#### 1）B+树基本特点

- 非叶子节点的子树指针与关键字个数相同。
- 非叶子节点的子树指针 P[i]，指向关键字属于 **[k[i],K[i+1])** 的子树（**注意：区间是前闭后开**)。
- **为所有叶子节点增加一个链指针**。
- **所有关键字都在叶子节点出现**。



这些基本特点是为了满足以下的特性。



#### 2）B+树的特性

- 所有的关键字 **都出现在叶子节点的链表中**，且链表中的关键字是有序的。
- **搜索只在叶子节点命中**。
- 非叶子节点相当于是 **叶子节点的索引层**，叶子节点是 **存储关键字数据的数据层**。



#### 3）相对 B 树，B+树做索引的优势

- B+树的磁盘读写代价更低。**B+树的内部没有指向关键字具体信息的指针**，所以其内部节点相对 B 树更小，如果把所有关键字存放在同一块盘中，那么盘中所能容纳的关键字数量也越多，一次性读入内存的需要查找的关键字也就越多，**相应的，IO 读写次数就降低了**。
- **树的查询效率更加稳定**。B+树所有数据都存在于叶子节点，所有关键字查询的路径长度相同，每次数据的查询效率相当。而 B 树可能在非叶子节点就停止查找了，所以查询效率不够稳定。
- **B+树只需要去遍历叶子节点就可以实现整棵树的遍历**。



### 3.5 MongoDB 的索引为什么选择 B 树，而 MySQL 的索引是 B+树？

因为 MongoDB 不是传统的关系型数据库，而是以 Json 格式作为存储的 NoSQL 非关系型数据库，目的就是高性能、高可用、易扩展。摆脱了关系模型，所以 **范围查询和遍历查询的需求就没那么强烈了**。



### 3.6 MyISAM 存储引擎和 InnoDB 的索引有什么区别

#### 1）MyISAM 存储引擎

![img](https://static001.infoq.cn/resource/image/51/46/515d027348d36efafa472c8bf71e0e46.png)



- **主键索引**



MyISAM 的索引文件（.MYI）和数据文件（.MYD）文件是分离的，索引文件仅保存记录所在页的指针（物理位置），通过这些指针来读取页，进而读取被索引的行。



树中的叶子节点保存的是对应行的物理位置。通过该值，**==存储引擎能顺利地进行回表查询，得到一行完整记录==**。



同时，每个叶子也保存了指向下一个叶子的指针，从而 **方便叶子节点的范围遍历**。



- **辅助索引**



在 MyISAM 中，主键索引和辅助索引在结构上没有任何区别，**==只是主键索引要求 key 是唯一的，而辅助索引的 key 可以重复==**。



#### 1）Innodb 存储引擎

Innodb 的主键索引和辅助索引之前提到过，再回顾一次。



- **主键索引**



![img](https://static001.infoq.cn/resource/image/34/b9/34235677a706a75af85de7fe5663b6b9.png)



InnoDB 主键索引中既存储了主健值，又存储了行数据。



- **辅助索引**



![img](https://static001.infoq.cn/resource/image/d3/27/d3c56fb09cc09705dd49b75311ad3d27.png)



对于辅助索引，InnoDB 采用的方式是在叶子节点中保存主键值，通过这个主键值来回表查询到一条完整记录，因此 **按辅助索引检索其实进行了二次查询，效率是没有主键索引高的**。



## 四、MySQL 索引失效

在上一节中了解了索引的多种数据结构，以及 B 树和 B+树的对比等，大家应该对索引的底层实现有了初步的了解。这一节从应用层的角度出发，看一下如何建索引更能满足我们的需求，以及 MySQL 索引什么时候会失效的问题。



先来思考一个小问题。



**问题：当查询条件为 2 个及 2 个以上时，是创建多个单列索引还是创建一个联合索引好呢？它们之间的区别是什么？哪个效率高呢？**



先来建立一些单列索引进行测试：



![img](https://static001.infoq.cn/resource/image/1f/9c/1fd3039e724fd7f2c31fc17bbcdf8a9c.png)



这里建立了一张表，里面建立了三个单列索引 userId,mobile,billMonth。



然后进行多列查询。



```
explain select * from `t_mobilesms_11` where userid = '1' and mobile = '13504679876' and billMonth = '1998-03'
```

复制代码



![img](https://static001.infoq.cn/resource/image/35/da/35724730ebd9d577943a1a50f08123da.png)



我们发现查询时只用到了 userid 这一个单列索引，这是为什么呢？因为这取决于 **MySQL 优化器的优化策略**。



**当多条件联合查询时，优化器会评估哪个条件的索引效率高，它会选择最佳的索引去使用。也就是说，此处三个索引列都可能被用到，只不过优化器判断只需要使用 userid 这一个索引就能完成本次查询，故最终 explain 展示的 key 为 userid。**



### 4.1 总结

多个单列索引在多条件查询时优化器会选择最优索引策略，可能 **只用一个索引，也可能将多个索引都用上**。



但是多个单列索引底层会建立多个 B+索引树，比较占用空间，也会浪费搜索效率 所以 **多条件联合查询时最好建联合索引**。



那联合索引就可以三个条件都用到了吗？会出现索引失效的问题吗？



### 4.2 联合索引失效问题

该部分参考并引用文章：一张图搞懂 MySQL 的索引失效（https://segmentfault.com/a/1190000021464570）



创建 user 表，然后建立 name, age, pos, phone 四个字段的联合索引 全值匹配（索引最佳）。



![img](https://static001.infoq.cn/resource/image/21/9e/21525fccce14b1a9e5ca1dc9525d4d9e.png)



索引生效，这是最佳的查询。



**那么时候会失效呢？**



#### 1）违反最左匹配原则

**最左匹配原则**：最左优先，以最左边的为起点任何连续的索引都能匹配上，如不连续，则匹配不上。



如：建立索引为(a,b)的联合索引，那么只查 where b = 2 则不生效。换句话说：如果建立的索引是(a,b,c)，也只有(a),(a,b),(a,b,c)三种查询可以生效。



![img](https://static001.infoq.cn/resource/image/2f/1a/2f9507c0f9305cdc47fc5de87108fa1a.png)



这里跳过了最左的 name 字段进行查询，发现索引失效了。



**遇到范围查询（>、<、between、like）就会停止匹配。**



比如：a= 1 and b = 2 and c>3 and d =4 如果建立(a,b,c,d)顺序的索引，d 是用不到索引的，因为 **c 字段是一个范围查询，它之后的字段会停止匹配**。



#### 2）在索引列上做任何操作

如计算、函数、（手动或自动）类型转换等操作，会导致索引失效而进行全表扫描。



```
explain select * from user where left(name,3) = 'zhangsan' and age =20
```

复制代码



![img](https://static001.infoq.cn/resource/image/43/61/43d20f1a563964bf25e2e53144b61961.png)



这里对 name 字段进行了 left 函数操作，导致索引失效。



#### 3）使用不等于（!= 、<>）

```
explain select * from user where age != 20;
```

复制代码



![img](https://static001.infoq.cn/resource/image/16/04/16c5e93e5bbc603127e90900b10f1c04.png)



```
explain select * from user where age <> 20;
```

复制代码



![img](https://static001.infoq.cn/resource/image/9d/cf/9d83216f5ef5c5845198ba36d38edccf.png)



#### 4）like 中以通配符开头(’%abc’)

索引失效



```
explain select * from user where name like ‘%zhangsan’;
```



![img](https://static001.infoq.cn/resource/image/21/0e/21581a7a3d8a1f7c78f8db76428b050e.png)



索引生效



```
explain select * from user where name like ‘zhangsan%’;
```

![img](https://static001.infoq.cn/resource/image/3e/4a/3e11738e3088e88f4142936c7afcc44a.png)



#### 5）字符串不加单引号索引失效

```
explain select * from user where name = 2000;
```

复制代码



![img](https://static001.infoq.cn/resource/image/8f/4e/8ff09dbbfd43ae00dc9268cd23c1034e.png)



#### 6）or 连接索引失效

```
explain select * from user where name = ‘2000’ or age = 20 or pos =‘cxy’;
```

复制代码



![img](https://static001.infoq.cn/resource/image/26/b7/26f28c14c61d9dc0351a58d0c7e58cb7.png)



#### 7）order by

正常（索引参与了排序），没有违反最左匹配原则。



```
explain select * from user where name = 'zhangsan' and age = 20 order by age,pos;
```

复制代码



![img](https://static001.infoq.cn/resource/image/65/6d/6512e5fe3904d349a7d02a86ff9c906d.png)



违反最左前缀法则，导致额外的文件排序（会降低性能）。



```
explain select name,age from user where name = 'zhangsan' order by pos;
```

复制代码



![img](https://static001.infoq.cn/resource/image/38/e0/3836bf179a86dad9a1b38ff951592be0.png)



#### 8）group by

正常（索引参与了排序）。



```
explain select name,age from user where name = 'zhangsan' group by age;
```

复制代码



违反最左前缀法则，导致产生临时表（会降低性能）。



```
explain select name,age from user where name = 'zhangsan' group by pos,age;
```

复制代码



![img](https://static001.infoq.cn/resource/image/23/49/23ea69d6db51a02addf75516fd075249.png)



## 五、总结

- 了解一条查询语句是如何执行的，发现建立索引是一种可以高效查找的数据结构。
- 了解了索引的各种分类情况，聚集索引和非聚集索引的区别，如何创建各种索引。
- 通过需求一步步分析出为什么 MySQL 要选 b+tree 作为索引的数据结构，对比了 btree 和 b+tree 的区别、 MyISAM 和 innodb 中索引的区别。
- 了解了索引会失效的多种情况，比较重要的最左匹配原则，相应地我们可以在建索引的时候做一些优化。

# 数据库三大范式

在设计与操作维护数据库时，最关键的问题就是要确保数据能够正确地分布到数据库的表中。使用正确的数据结构，不仅有助于对数据库进行相应的存取操作，还可以极大地简化应用程序中的其他内容(查询、窗体、报表、代码等)，按照“数据库规范化”对表进行设计，其目的就是减少数据库中的数据冗余，以增加数据的一致性。



泛化时在识别数据库中的一个数据元素、关系以及定义所需的表和各表中的项目这些初始工作之后的一个细化的过程。常见的范式有1NF、2NF、3NF、BCNF以及4NF。下面对这几种常见的范式进行简要分析。



1、1NF(第一范式)

第一范式是指数据库表中的每一列都是不可分割的基本数据项，同一列中不能有多个值，即实体中的某个属性不能有多个值或者不能有重复的属性。



如果出现重复的属性，就可能需要定义一个新的实体，新的实体由重复的属性构成，新实体与原实体之间为一对多关系。第一范式的模式要求属性值不可再分裂成更小部分，即属性项不能是属性组合或是由一组属性构成。



简而言之，第一范式就是无重复的列。例如，由“职工号”“姓名”“电话号码”组成的表(一个人可能有一部办公电话和一部移动电话)，这时将其规范化为1NF可以将电话号码分为“办公电话”和“移动电话”两个属性，即职工(职工号，姓名，办公电话，移动电话)。



2、2NF(第二范式)

第二范式(2NF)是在第一范式(1NF)的基础上建立起来的，即满足第二范式(2NF)必须先满足第一范式(1NF)。**第二范式(2NF)要求数据库表中的每个实例或行必须可以被唯一地区分。**为实现区分通常需要为表加上一个列，以存储各个实例的唯一标识。



如果关系模型R为第一范式，并且R中的每一个非主属性完全函数依赖于R的某个候选键，则称R为第二范式模式(如果A是关系模式R的候选键的一个属性，则称A是R的主属性，否则称A是R的非主属性)。



例如，在选课关系表(学号，课程号，成绩，学分)，关键字为组合关键字(学号，课程号)，但由于非主属性学分仅依赖于课程号，对关键字(学号，课程号)只是部分依赖，而不是完全依赖，因此此种方式会导致数据冗余以及更新异常等问题，解决办法是将其分为两个关系模式：学生表(学号，课程号，分数)和课程表(课程号，学分)，新关系通过学生表中的外关键字课程号联系，在需要时进行连接。



3、3NF(第三范式)

如果关系模型R是第二范式，且每个非主属性都不传递依赖于R的候选键，则称R是第三范式的模式。



以学生表(学号，姓名，课程号，成绩)为例，其中学生姓名无重名，所以该表有两个候选码(学号，课程号)和(姓名，课程号)，故存在函数依赖：学号——>姓名，(学号，课程号)——>成绩，唯一的非主属性成绩对码不存在部分依赖，也不存在传递依赖，所以属性属于第三范式。



4、BCNF(BC范式)

它构建在第三范式的基础上，如果关系模型R是第一范式，且每个属性都不传递依赖于R的候选键，那么称R为BCNF的模式。



假设仓库管理关系表(仓库号，存储物品号，管理员号，数量)，满足一个管理员只在一个仓库工作；一个仓库可以存储多种物品，则存在如下关系：

(仓库号，存储物品号)——>(管理员号，数量)

(管理员号，存储物品号)——>(仓库号，数量)

所以，(仓库号，存储物品号)和(管理员号，存储物品号)都是仓库管理关系表的候选码，表中唯一非关键字段为数量，它是符合第三范式的。但是，由于存在如下决定关系：

(仓库号)——>(管理员号)

(管理员号)——>(仓库号)

即存在关键字段决定关键字段的情况，因此其不符合BCNF。把仓库管理关系表分解为两个关系表仓库管理表(仓库号，管理员号)和仓库表(仓库号，存储物品号，数量)，这样这个数据库表是符合BCNF的，并消除了删除异常、插入异常和更新异常。



5、4NF(第四范式)

设R是一个关系模型，D是R上的多值依赖集合。如果D中存在凡多值依赖X->Y时，X必是R的超键，那么称R是第四范式的模式。



例如，职工表(职工编号，职工孩子姓名，职工选修课程)，在这个表中，同一个职工可能会有多个职工孩子姓名，同样，同一个职工也可能会有多个职工选修课程，即这里存在着多值事实，不符合第四范式。如果要符合第四范式，只需要将上表分为两个表，使它们只有一个多值事实，例如职工表一(职工编号，职工孩子姓名)，职工表二(职工编号，职工选修课程)，两个表都只有一个多值事实，所以符合第四范式。



拓展：各范式的关系图如下所示：

————————————————
版权声明：本文为CSDN博主「Yes_JiangShuai」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/Dove_Knowledge/article/details/71434960

# 联合索引(a,b,c)

用到索引的有a,ab,abc,ac 因为优化器会自动调整and前后的顺序，所以ba,cba,bca,ca都会用到索引，其他的都不会用到该索引。**ac这一组仅仅是a用到索引。**

组合索引 有“最左前缀”原则，遇到范围查询(>、<、between、like)就会停止匹配。为什么是“最左匹配”原则，可以通过数据结构来看：

联合索引是一颗b+树(a,b)



![img](https:////upload-images.jianshu.io/upload_images/12361419-95352fd8827cdf4d.png?imageMogr2/auto-orient/strip|imageView2/2/w/942/format/webp)

联合索引（a,b）.png



a按顺序排列，b在a确定的情况下按顺序排列。所以必须基于a来查找后面的b字段，否则b就是无序的，就用不到索引了。

作者：菊地尤里
链接：https://www.jianshu.com/p/dc0ca4e4e3e7
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

对于复合索引：Mysql从左到右的使用索引中的字段，一个查询可以只使用索引中的一部份，但只能是最左侧部分。例如索引是key index （a,b,c）。 可以支持a | a,b| a,b,c 3种组合进行查找，但不支持 b,c进行查找 .当最左侧字段是常量引用时，索引就十分有效。

以下是一些例子：

(1) select * from myTest where a=3 and b=5 and c=4; ---- abc顺序
abc三个索引都在where条件里面用到了，而且都发挥了作用

(2) select * from myTest where c=4 and b=6 and a=3;
where里面的条件顺序在查询之前会被mysql自动优化，效果跟上一句一样

(3) select * from myTest where a=3 and c=7;
建立了联合索引a,b,c，相当于建立了四个索引 (a,b,c),(a,b),(a,c),(a) ，这样a,c就会命中，而且还有一种情况是，假如建立了（a,b,c）联合索引，查询的时候是where c = 1 and b = 2 and a= 3，即使这样，数据库底层也会优化，从而命中a,b,c三个索引

(4) select * from myTest where a=3 and b>7 and c=3; ---- b范围值，断点，阻塞了c的索引
a用到了，b也用到了，c没有用到，这个地方b是范围值，也算断点，只不过自身用到了索引

(5) select * from myTest where b=3 and c=4; — 联合索引必须按照顺序使用，并且需要全部使用
因为a索引没有使用，所以这里 bc都没有用上索引效果

(6) select * from myTest where a>4 and b=7 and c=9;
a用到了 b没有使用，c没有使用（a用了范围所以，相当于断点，之后的b，c都没有用到索引）

(7) select * from myTest where a=3 order by b;
a用到了索引，b在结果排序中也用到了索引的效果，a下面任意一段的b是排好序的

(8) select * from myTest where a=3 order by c;
a用到了索引，但是这个地方c没有发挥排序效果，因为中间断点了，使用 explain 可以看到 filesort

(9) select * from mytable where b=3 order by a;
b没有用到索引，排序中a也没有发挥索引效果

以下条件会导致索引失效：
1.不在索引列上做任何操作（计算、函数、（自动or手动）类型转换），会导致索引失效而转向全表扫描
2.存储引擎不能使用索引范围条件右边的列（例如 只用到b ， c）
3.尽量使用覆盖索引（只访问索引的查询（索引列和查询列一致）），减少select ***
4.mysql在使用不等于（！=或者<>）的时候**无法使用索引会导致全表扫描

5.is null,is not null也无法使用索引

6.ike以通配符开头（’%abc…’）mysql索引失效会变成全表扫描的操作。问题：解决like‘%字符串%’时索引不被使用的方法

7.字符串不加单引号索引失效

建议：

对于单键索引，尽量选择针对当前query过滤性更好的索引

在选择组合索引的时候，当前Query中过滤性最好的字段在索引字段顺序中，位置越靠前越好。

. 在选择组合索引的时候，尽量选择可以能够包含当前query中的where子句中更多字段的索引

. 在选择组合索引的时候，尽量选择可以能够包含当前query中的where子句中更多字段的索引

尽可能通过分析统计信息和调整query的写法来达到选择合适索引的目的
————————————————
版权声明：本文为CSDN博主「3Nero3」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_42630887/article/details/97113323

# Sql执行很慢的原因

说实话，这个问题可以涉及到 MySQL 的很多核心知识，可以扯出一大堆，就像要考你计算机网络的知识时，问你“输入URL回车之后，究竟发生了什么”一样，看看你能说出多少了。

之前腾讯面试的实话，也问到这个问题了，不过答的很不好，之前没去想过相关原因，导致一时之间扯不出来。所以今天，我带大家来详细扯一下有哪些原因，相信你看完之后一定会有所收获，不然你打我。

### 开始装逼：分类讨论

一条 SQL 语句执行的很慢，那是每次执行都很慢呢？还是大多数情况下是正常的，偶尔出现很慢呢？所以我觉得，我们还得分以下两种情况来讨论。

1、大多数情况是正常的，只是偶尔会出现很慢的情况。

2、在数据量不变的情况下，这条SQL语句一直以来都执行的很慢。

针对这两种情况，我们来分析下可能是哪些原因导致的。

## 针对偶尔很慢的情况

一条 SQL 大多数情况正常，偶尔才能出现很慢的情况，针对这种情况，我觉得这条SQL语句的书写本身是没什么问题的，而是其他原因导致的，那会是什么原因呢？

### 数据库在刷新脏页我也无奈啊

当我们要往数据库插入一条数据、或者要更新一条数据的时候，我们知道数据库会在**内存**中把对应字段的数据更新了，但是更新之后，这些更新的字段并不会马上同步持久化到**磁盘**中去，而是把这些更新的记录写入到 redo log 日记中去，等到空闲的时候，在通过 redo log 里的日记把最新的数据同步到**磁盘**中去。

不过，redo log 里的容量是有限的，如果数据库一直很忙，更新又很频繁，这个时候 redo log 很快就会被写满了，这个时候就没办法等到空闲的时候再把数据同步到磁盘的，只能暂停其他操作，全身心来把数据同步到磁盘中去的，**而这个时候，就会导致我们平时正常的SQL语句突然执行的很慢，所以说，数据库在在同步数据到磁盘的时候，就有可能导致我们的SQL语句执行的很慢了。**

### 拿不到锁我能怎么办

这个就比较容易想到了，我们要执行的这条语句，刚好这条语句涉及到的**表**，别人在用，并且加锁了，我们拿不到锁，只能慢慢等待别人释放锁了。或者，表没有加锁，但要使用到的某个一行被加锁了，这个时候，我也没办法啊。

如果要判断是否真的在等待锁，我们可以用 **show processlist**这个命令来查看当前的状态哦，这里我要提醒一下，有些命令最好记录一下，反正，我被问了好几个命令，都不知道怎么写，呵呵。

下来我们来访分析下第二种情况，我觉得第二种情况的分析才是最重要的

## 针对一直都这么慢的情况

如果在数据量一样大的情况下，这条 SQL 语句每次都执行的这么慢，那就就要好好考虑下你的 SQL 书写了，下面我们来分析下哪些原因会导致我们的 SQL 语句执行的很不理想。

我们先来假设我们有一个表，表里有下面两个字段,分别是主键 id，和两个普通字段 c 和 d。

```
mysql> CREATE TABLE `t` (
  `id` int(11) NOT NULL,
  `c` int(11) DEFAULT NULL,
  `d` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB;
```

### 扎心了，没用到索引

没有用上索引，我觉得这个原因是很多人都能想到的，例如你要查询这条语句

```
select * from t where 100 <c and c < 100000;
```

**字段没有索引**

刚好你的 c 字段上没有索引，那么抱歉，只能走全表扫描了，你就体验不会索引带来的乐趣了，所以，这回导致这条查询语句很慢。

**字段有索引，但却没有用索引**

好吧，这个时候你给 c 这个字段加上了索引，然后又查询了一条语句

```
select * from t where c - 1 = 1000;
```

我想问大家一个问题，这样子在查询的时候会用索引查询吗？

答是不会，如果我们在字段的左边做了运算，那么很抱歉，在查询的时候，就不会用上索引了，所以呢，大家要注意这种**字段上有索引，但由于自己的疏忽，导致系统没有使用索引**的情况了。

正确的查询应该如下

```
select * from t where c = 1000 + 1;
```

有人可能会说，右边有运算就能用上索引？难道数据库就不会自动帮我们优化一下，自动把 c - 1=1000 自动转换为 c = 1000+1。

不好意思，确实不会帮你，所以，你要注意了。

**函数操作导致没有用上索引**

如果我们在查询的时候，对字段进行了函数操作，也是会导致没有用上索引的，例如

```
select * from t where pow(c,2) = 1000;
```

这里我只是做一个例子，假设函数 pow 是求 c 的 n 次方，实际上可能并没有 pow(c,2)这个函数。其实这个和上面在左边做运算也是很类似的。

所以呢，一条语句执行都很慢的时候，可能是该语句没有用上索引了，不过具体是啥原因导致没有用上索引的呢，你就要会分析了，我上面列举的三个原因，应该是出现的比较多的吧。

### 呵呵，数据库自己选错索引了

我们在进行查询操作的时候，例如

```
select * from t where 100 < c and c < 100000;
```

我们知道，主键索引和非主键索引是有区别的，主键索引存放的值是**整行字段的数据**，而非主键索引上存放的值不是整行字段的数据，而且存放**主键字段的值**。不大懂的可以看我这篇文章：[面试小知识：MySQL索引相关](https://mp.weixin.qq.com/s/RemJcqPIvLArmfWIhoaZ1g) 里面有说到主键索引和非主键索引的区别

也就是说，我们如果走 c 这个字段的索引的话，最后会查询到对应主键的值，然后，再根据主键的值走主键索引，查询到整行数据返回。

好吧扯了这么多，其实我就是想告诉你，就算你在 c 字段上有索引，系统也并不一定会走 c 这个字段上的索引，而是有可能会直接扫描扫描全表，找出所有符合 100 < c and c < 100000 的数据。

**为什么会这样呢？**

其实是这样的，系统在执行这条语句的时候，会进行预测：究竟是走 c 索引扫描的行数少，还是直接扫描全表扫描的行数少呢？显然，扫描行数越少当然越好了，因为扫描行数越少，意味着I/O操作的次数越少。

如果是扫描全表的话，那么扫描的次数就是这个表的总行数了，假设为 n；而如果走索引 c 的话，我们通过索引 c 找到主键之后，还得再通过主键索引来找我们整行的数据，也就是说，需要走两次索引。而且，我们也不知道符合 100 c < and c < 10000 这个条件的数据有多少行，万一这个表是全部数据都符合呢？这个时候意味着，走 c 索引不仅扫描的行数是 n，同时还得每行数据走两次索引。

**所以呢，系统是有可能走全表扫描而不走索引的。那系统是怎么判断呢？**

判断来源于系统的预测，也就是说，如果要走 c 字段索引的话，系统会预测走 c 字段索引大概需要扫描多少行。如果预测到要扫描的行数很多，它可能就不走索引而直接扫描全表了。

那么问题来了，**系统是怎么预测判断的呢？**这里我给你讲下系统是怎么判断的吧，虽然这个时候我已经写到脖子有点酸了。

系统是通过**索引的区分度**来判断的，一个索引上不同的值越多，意味着出现相同数值的索引越少，意味着索引的区分度越高。我们也把区分度称之为**基数**，即区分度越高，基数越大。所以呢，基数越大，意味着符合 100 < c and c < 10000 这个条件的行数越少。

所以呢，一个索引的基数越大，意味着走索引查询越有优势。

**那么问题来了，怎么知道这个索引的基数呢？**

系统当然是不会遍历全部来获得一个索引的基数的，代价太大了，索引系统是通过遍历部分数据，也就是通过**采样**的方式，来预测索引的基数的。

**扯了这么多，重点的来了**，居然是采样，那就有可能出现**失误**的情况，也就是说，c 这个索引的基数实际上是很大的，但是采样的时候，却很不幸，把这个索引的基数预测成很小。例如你采样的那一部分数据刚好基数很小，然后就误以为索引的基数很小。**然后就呵呵，系统就不走 c 索引了，直接走全部扫描了**。

所以呢，说了这么多，得出结论：**由于统计的失误，导致系统没有走索引，而是走了全表扫描**，而这，也是导致我们 SQL 语句执行的很慢的原因。

> 这里我声明一下，系统判断是否走索引，扫描行数的预测其实只是原因之一，这条查询语句是否需要使用使用临时表、是否需要排序等也是会影响系统的选择的。

不过呢，我们有时候也可以通过强制走索引的方式来查询，例如

```
select * from t force index(a) where c < 100 and c < 100000;
```

我们也可以通过

```
show index from t;
```

来查询索引的基数和实际是否符合，如果和实际很不符合的话，我们可以重新来统计索引的基数，可以用这条命令

```
analyze table t;
```

来重新统计分析。

**既然会预测错索引的基数，这也意味着，当我们的查询语句有多个索引的时候，系统有可能也会选错索引哦**，这也可能是 SQL 执行的很慢的一个原因。

好吧，就先扯这么多了，你到时候能扯出这么多，我觉得已经很棒了，下面做一个总结。

### 总结

以上是我的总结与理解，最后一个部分，我怕很多人不大懂**数据库居然会选错索引**，所以我详细解释了一下，下面我对以上做一个总结。

一个 SQL 执行的很慢，我们要分两种情况讨论：

1、大多数情况下很正常，偶尔很慢，则有如下原因

(1)、数据库在刷新脏页，例如 redo log 写满了需要同步到磁盘。

(2)、执行的时候，遇到锁，如表锁、行锁。

2、这条 SQL 语句一直执行的很慢，则有如下原因。

(1)、没有用上索引：例如该字段没有索引；由于对字段进行运算、函数操作导致无法用索引。

(2)、数据库选错了索引。

大家如果有补充的，也是可以留言区补充一波哦。

# MVCC

https://www.cnblogs.com/myseries/p/10930910.html#:~:text=MVCC%E5%85%A8%E7%A7%B0%E6%98%AF%EF%BC%9A%20Multiversion,concurrency%20control%20%EF%BC%8C%E5%A4%9A%E7%89%88%E6%9C%AC%E5%B9%B6%E5%8F%91%E6%8E%A7%E5%88%B6%EF%BC%8C%E6%8F%90%E4%BE%9B%E5%B9%B6%E5%8F%91%E8%AE%BF%E9%97%AE%E6%95%B0%E6%8D%AE%E5%BA%93%E6%97%B6%EF%BC%8C%E5%AF%B9%E4%BA%8B%E5%8A%A1%E5%86%85%E8%AF%BB%E5%8F%96%E7%9A%84%E5%88%B0%E7%9A%84%E5%86%85%E5%AD%98%E5%81%9A%E5%A4%84%E7%90%86%EF%BC%8C%E7%94%A8%E6%9D%A5%E9%81%BF%E5%85%8D%E5%86%99%E6%93%8D%E4%BD%9C%E5%A0%B5%E5%A1%9E%E8%AF%BB%E6%93%8D%E4%BD%9C%E7%9A%84%E5%B9%B6%E5%8F%91%E9%97%AE%E9%A2%98%E3%80%82

https://www.php.cn/mysql-tutorials-460111.html

https://zhuanlan.zhihu.com/p/66791480

# MySQL大表优化

当MySQL单表记录数过大时，增删改查性能都会急剧下降，可以参考以下步骤来优化：

### 单表优化

除非单表数据未来会一直不断上涨，否则不要一开始就考虑拆分，拆分会带来逻辑、部署、运维的各种复杂度，一般以整型值为主的表在`千万级`以下，字符串为主的表在`五百万`以下是没有太大问题的。而事实上很多时候MySQL单表的性能依然有不少优化空间，甚至能正常支撑千万级以上的数据量：

#### 字段

- 尽量使用`TINYINT`、`SMALLINT`、`MEDIUM_INT`作为整数类型而非`INT`，如果非负则加上`UNSIGNED`
- `VARCHAR`的长度只分配真正需要的空间
- 使用枚举或整数代替字符串类型
- 尽量使用`TIMESTAMP`而非`DATETIME`，
- 单表不要有太多字段，建议在20以内
- 避免使用NULL字段，很难查询优化且占用额外索引空间
- 用整型来存IP

#### 索引

- 索引并不是越多越好，要根据查询有针对性的创建，考虑在`WHERE`和`ORDER BY`命令上涉及的列建立索引，可根据`EXPLAIN`来查看是否用了索引还是全表扫描
- 应尽量避免在`WHERE`子句中对字段进行`NULL`值判断，否则将导致引擎放弃使用索引而进行全表扫描
- 值分布很稀少的字段不适合建索引，例如"性别"这种只有两三个值的字段
- 字符字段只建前缀索引
- 字符字段最好不要做主键
- 不用外键，由程序保证约束
- 尽量不用`UNIQUE`，由程序保证约束
- 使用多列索引时主意顺序和查询条件保持一致，同时删除不必要的单列索引

#### 查询SQL

- 可通过开启慢查询日志来找出较慢的SQL
- 不做列运算：`SELECT id WHERE age + 1 = 10`，任何对列的操作都将导致表扫描，它包括数据库教程函数、计算表达式等等，查询时要尽可能将操作移至等号右边
- sql语句尽可能简单：一条sql只能在一个cpu运算；大语句拆小语句，减少锁时间；一条大sql可以堵死整个库
- 不用`SELECT *`
- `OR`改写成`IN`：`OR`的效率是n级别，`IN`的效率是log(n)级别，in的个数建议控制在200以内
- 不用函数和触发器，在应用程序实现
- 避免`%xxx`式查询
- 少用`JOIN`
- 使用同类型进行比较，比如用`'123'`和`'123'`比，`123`和`123`比
- 尽量避免在`WHERE`子句中使用!=或<>操作符，否则将引擎放弃使用索引而进行全表扫描
- 对于连续数值，使用`BETWEEN`不用`IN`：`SELECT id FROM t WHERE num BETWEEN 1 AND 5`
- 列表数据不要拿全表，要使用`LIMIT`来分页，每页数量也不要太大

#### 引擎

目前广泛使用的是MyISAM和InnoDB两种引擎：

##### MyISAM

MyISAM引擎是MySQL 5.1及之前版本的默认引擎，它的特点是：

- 不支持行锁，读取时对需要读到的所有表加锁，写入时则对表加排它锁
- 不支持事务
- 不支持外键
- 不支持崩溃后的安全恢复
- 在表有读取查询的同时，支持往表中插入新纪录
- 支持`BLOB`和`TEXT`的前500个字符索引，支持全文索引
- 支持延迟更新索引，极大提升写入性能
- 对于不会进行修改的表，支持压缩表，极大减少磁盘空间占用

##### InnoDB

InnoDB在MySQL 5.5后成为默认索引，它的特点是：

- 支持行锁，采用MVCC来支持高并发
- 支持事务
- 支持外键
- 支持崩溃后的安全恢复
- 不支持全文索引

总体来讲，MyISAM适合`SELECT`密集型的表，而InnoDB适合`INSERT`和`UPDATE`密集型的表

#### 系统调优参数

可以使用下面几个工具来做基准测试：

- [sysbench](https://github.com/akopytov/sysbench)：一个模块化，跨平台以及多线程的性能测试工具
- [iibench-mysql](https://github.com/tmcallaghan/iibench-mysql)：基于 Java 的 MySQL/Percona/MariaDB 索引进行插入性能测试工具
- [tpcc-mysql](https://github.com/Percona-Lab/tpcc-mysql)：Percona开发的TPC-C测试工具

具体的调优参数内容较多，具体可参考官方文档，这里介绍一些比较重要的参数：

- back_log：back_log值指出在MySQL暂时停止回答新请求之前的短时间内多少个请求可以被存在堆栈中。也就是说，如果MySql的连接数据达到max_connections时，新来的请求将会被存在堆栈中，以等待某一连接释放资源，该堆栈的数量即back_log，如果等待连接的数量超过back_log，将不被授予连接资源。可以从默认的50升至500
- wait_timeout：数据库连接闲置时间，闲置连接会占用内存资源。可以从默认的8小时减到半小时
- max_user_connection: 最大连接数，默认为0无上限，最好设一个合理上限
- thread_concurrency：并发线程数，设为CPU核数的两倍
- skip_name_resolve：禁止对外部连接进行DNS解析，消除DNS解析时间，但需要所有远程主机用IP访问
- key_buffer_size：索引块的缓存大小，增加会提升索引处理速度，对MyISAM表性能影响最大。对于内存4G左右，可设为256M或384M，通过查询`show status like 'key_read%'`，保证`key_reads / key_read_requests`在0.1%以下最好
- innodb_buffer_pool_size：缓存数据块和索引块，对InnoDB表性能影响最大。通过查询`show status like 'Innodb_buffer_pool_read%'`，保证` (Innodb_buffer_pool_read_requests – Innodb_buffer_pool_reads) / Innodb_buffer_pool_read_requests`越高越好
- innodb_additional_mem_pool_size：InnoDB存储引擎用来存放数据字典信息以及一些内部数据结构的内存空间大小，当数据库对象非常多的时候，适当调整该参数的大小以确保所有数据都能存放在内存中提高访问效率，当过小的时候，MySQL会记录Warning信息到数据库的错误日志中，这时就需要该调整这个参数大小
- innodb_log_buffer_size：InnoDB存储引擎的事务日志所使用的缓冲区，一般来说不建议超过32MB
- query_cache_size：缓存MySQL中的ResultSet，也就是一条SQL语句执行的结果集，所以仅仅只能针对select语句。当某个表的数据有任何任何变化，都会导致所有引用了该表的select语句在Query Cache中的缓存数据失效。所以，当我们的数据变化非常频繁的情况下，使用Query Cache可能会得不偿失。根据命中率`(Qcache_hits/(Qcache_hits+Qcache_inserts)*100))`进行调整，一般不建议太大，256MB可能已经差不多了，大型的配置型静态数据可适当调大.
  可以通过命令`show status like 'Qcache_%'`查看目前系统Query catch使用大小
- read_buffer_size：MySql读入缓冲区大小。对表进行顺序扫描的请求将分配一个读入缓冲区，MySql会为它分配一段内存缓冲区。如果对表的顺序扫描请求非常频繁，可以通过增加该变量值以及内存缓冲区大小提高其性能
- sort_buffer_size：MySql执行排序使用的缓冲大小。如果想要增加`ORDER BY`的速度，首先看是否可以让MySQL使用索引而不是额外的排序阶段。如果不能，可以尝试增加sort_buffer_size变量的大小
- read_rnd_buffer_size：MySql的随机读缓冲区大小。当按任意顺序读取行时(例如，按照排序顺序)，将分配一个随机读缓存区。进行排序查询时，MySql会首先扫描一遍该缓冲，以避免磁盘搜索，提高查询速度，如果需要排序大量数据，可适当调高该值。但MySql会为每个客户连接发放该缓冲空间，所以应尽量适当设置该值，以避免内存开销过大。
- record_buffer：每个进行一个顺序扫描的线程为其扫描的每张表分配这个大小的一个缓冲区。如果你做很多顺序扫描，可能想要增加该值
- thread_cache_size：保存当前没有与连接关联但是准备为后面新的连接服务的线程，可以快速响应连接的线程请求而无需创建新的
- table_cache：类似于thread_cache_size，但用来缓存表文件，对InnoDB效果不大，主要用于MyISAM

#### 升级硬件

Scale up，这个不多说了，根据MySQL是CPU密集型还是I/O密集型，通过提升CPU和内存、使用SSD，都能显著提升MySQL性能

### 读写分离

也是目前常用的优化，从库读主库写，一般不要采用双主或多主引入很多复杂性，尽量采用文中的其他方案来提高性能。同时目前很多拆分的解决方案同时也兼顾考虑了读写分离

### 缓存

缓存可以发生在这些层次：

- MySQL内部：在系统调优参数介绍了相关设置
- 数据访问层：比如MyBatis针对SQL语句做缓存，而Hibernate可以精确到单个记录，这里缓存的对象主要是持久化对象`Persistence Object`
- 应用服务层：这里可以通过编程手段对缓存做到更精准的控制和更多的实现策略，这里缓存的对象是数据传输对象`Data Transfer Object`
- Web层：针对web页面做缓存
- 浏览器客户端：用户端的缓存

可以根据实际情况在一个层次或多个层次结合加入缓存。这里重点介绍下服务层的缓存实现，目前主要有两种方式：

- 直写式（Write Through）：在数据写入数据库后，同时更新缓存，维持数据库与缓存的一致性。这也是当前大多数应用缓存框架如Spring Cache的工作方式。这种实现非常简单，同步好，但效率一般。
- 回写式（Write Back）：当有数据要写入数据库时，只会更新缓存，然后异步批量的将缓存数据同步到数据库上。这种实现比较复杂，需要较多的应用逻辑，同时可能会产生数据库与缓存的不同步，但效率非常高。

### 表分区

MySQL在5.1版引入的分区是一种简单的水平拆分，用户需要在建表的时候加上分区参数，对应用是透明的无需修改代码

对用户来说，分区表是一个独立的逻辑表，但是底层由多个物理子表组成，实现分区的代码实际上是通过对一组底层表的对象封装，但对SQL层来说是一个完全封装底层的黑盒子。MySQL实现分区的方式也意味着索引也是按照分区的子表定义，没有全局索引

![Alt text]()

用户的SQL语句是需要针对分区表做优化，SQL条件中要带上分区条件的列，从而使查询定位到少量的分区上，否则就会扫描全部分区，可以通过`EXPLAIN PARTITIONS`来查看某条SQL语句会落在那些分区上，从而进行SQL优化，如下图5条记录落在两个分区上：

```
mysql> explain partitions select count(1) from user_partition where id in (1,2,3,4,5);
+----+-------------+----------------+------------+-------+---------------+---------+---------+------+------+--------------------------+
| id | select_type | table          | partitions | type  | possible_keys | key     | key_len | ref  | rows | Extra                    |
+----+-------------+----------------+------------+-------+---------------+---------+---------+------+------+--------------------------+
|  1 | SIMPLE      | user_partition | p1,p4      | range | PRIMARY       | PRIMARY | 8       | NULL |    5 | Using where; Using index |
+----+-------------+----------------+------------+-------+---------------+---------+---------+------+------+--------------------------+
1 row in set (0.00 sec)
```

分区的好处是：

- 可以让单表存储更多的数据
- 分区表的数据更容易维护，可以通过清楚整个分区批量删除大量数据，也可以增加新的分区来支持新插入的数据。另外，还可以对一个独立分区进行优化、检查、修复等操作
- 部分查询能够从查询条件确定只落在少数分区上，速度会很快
- 分区表的数据还可以分布在不同的物理设备上，从而搞笑利用多个硬件设备
- 可以使用分区表赖避免某些特殊瓶颈，例如InnoDB单个索引的互斥访问、ext3文件系统的inode锁竞争
- 可以备份和恢复单个分区

分区的限制和缺点：

- 一个表最多只能有1024个分区
- 如果分区字段中有主键或者唯一索引的列，那么所有主键列和唯一索引列都必须包含进来
- 分区表无法使用外键约束
- NULL值会使分区过滤无效
- 所有分区必须使用相同的存储引擎

分区的类型：

- RANGE分区：基于属于一个给定连续区间的列值，把多行分配给分区
- LIST分区：类似于按RANGE分区，区别在于LIST分区是基于列值匹配一个离散值集合中的某个值来进行选择
- HASH分区：基于用户定义的表达式的返回值来进行选择的分区，该表达式使用将要插入到表中的这些行的列值进行计算。这个函数可以包含MySQL中有效的、产生非负整数值的任何表达式
- KEY分区：类似于按HASH分区，区别在于KEY分区只支持计算一列或多列，且MySQL服务器提供其自身的哈希函数。必须有一列或多列包含整数值

分区适合的场景有：

- 最适合的场景数据的时间序列性比较强，则可以按时间来分区，如下所示：

```
CREATE TABLE members (
    firstname VARCHAR(25) NOT NULL,
    lastname VARCHAR(25) NOT NULL,
    username VARCHAR(16) NOT NULL,
    email VARCHAR(35),
    joined DATE NOT NULL
)
PARTITION BY RANGE( YEAR(joined) ) (
    PARTITION p0 VALUES LESS THAN (1960),
    PARTITION p1 VALUES LESS THAN (1970),
    PARTITION p2 VALUES LESS THAN (1980),
    PARTITION p3 VALUES LESS THAN (1990),
    PARTITION p4 VALUES LESS THAN MAXVALUE
);
```

查询时加上时间范围条件效率会非常高，同时对于不需要的历史数据能很容的批量删除。

- 如果数据有明显的热点，而且除了这部分数据，其他数据很少被访问到，那么可以将热点数据单独放在一个分区，让这个分区的数据能够有机会都缓存在内存中，查询时只访问一个很小的分区表，能够有效使用索引和缓存

另外MySQL有一种早期的简单的分区实现 - 合并表（merge table），限制较多且缺乏优化，不建议使用，应该用新的分区机制来替代

### 垂直拆分

垂直分库是根据数据库里面的数据表的相关性进行拆分，比如：一个数据库里面既存在用户数据，又存在订单数据，那么垂直拆分可以把用户数据放到用户库、把订单数据放到订单库。垂直分表是对数据表进行垂直拆分的一种方式，常见的是把一个多字段的大表按常用字段和非常用字段进行拆分，每个表里面的数据记录数一般情况下是相同的，只是字段不一样，使用主键关联

比如原始的用户表是：

![Alt text](https://segmentfault.com/img/remote/1460000006158196)

垂直拆分后是：

![Alt text](https://segmentfault.com/img/remote/1460000006158199)

垂直拆分的优点是：

- 可以使得行数据变小，一个数据块(Block)就能存放更多的数据，在查询时就会减少I/O次数(每次查询时读取的Block 就少)
- 可以达到最大化利用Cache的目的，具体在垂直拆分的时候可以将不常变的字段放一起，将经常改变的放一起
- 数据维护简单

缺点是：

- 主键出现冗余，需要管理冗余列
- 会引起表连接JOIN操作（增加CPU开销）可以通过在业务服务器上进行join来减少数据库压力
- 依然存在单表数据量过大的问题（需要水平拆分）
- 事务处理复杂

### 水平拆分

#### 概述

水平拆分是通过某种策略将数据分片来存储，分库内分表和分库两部分，每片数据会分散到不同的MySQL表或库，达到分布式的效果，能够支持非常大的数据量。前面的表分区本质上也是一种特殊的库内分表

库内分表，仅仅是单纯的解决了单一表数据过大的问题，由于没有把表的数据分布到不同的机器上，因此对于减轻MySQL服务器的压力来说，并没有太大的作用，大家还是竞争同一个物理机上的IO、CPU、网络，这个就要通过分库来解决

前面垂直拆分的用户表如果进行水平拆分，结果是：

![Alt text](https://segmentfault.com/img/remote/1460000006158207)

实际情况中往往会是垂直拆分和水平拆分的结合，即将`Users_A_M`和`Users_N_Z`再拆成`Users`和`UserExtras`，这样一共四张表

水平拆分的优点是:

- 不存在单库大数据和高并发的性能瓶颈
- 应用端改造较少
- 提高了系统的稳定性和负载能力

缺点是：

- 分片事务一致性难以解决
- 跨节点Join性能差，逻辑复杂
- 数据多次扩展难度跟维护量极大

#### 分片原则

- 能不分就不分，参考单表优化
- 分片数量尽量少，分片尽量均匀分布在多个数据结点上，因为一个查询SQL跨分片越多，则总体性能越差，虽然要好于所有数据在一个分片的结果，只在必要的时候进行扩容，增加分片数量
- 分片规则需要慎重选择做好提前规划，分片规则的选择，需要考虑数据的增长模式，数据的访问模式，分片关联性问题，以及分片扩容问题，最近的分片策略为范围分片，枚举分片，一致性Hash分片，这几种分片都有利于扩容
- 尽量不要在一个事务中的SQL跨越多个分片，分布式事务一直是个不好处理的问题
- 查询条件尽量优化，尽量避免Select * 的方式，大量数据结果集下，会消耗大量带宽和CPU资源，查询尽量避免返回大量结果集，并且尽量为频繁使用的查询语句建立索引。
- 通过数据冗余和表分区赖降低跨库Join的可能

这里特别强调一下分片规则的选择问题，如果某个表的数据有明显的时间特征，比如订单、交易记录等，则他们通常比较合适用时间范围分片，因为具有时效性的数据，我们往往关注其近期的数据，查询条件中往往带有时间字段进行过滤，比较好的方案是，当前活跃的数据，采用跨度比较短的时间段进行分片，而历史性的数据，则采用比较长的跨度存储。

总体上来说，分片的选择是取决于最频繁的查询SQL的条件，因为不带任何Where语句的查询SQL，会遍历所有的分片，性能相对最差，因此这种SQL越多，对系统的影响越大，所以我们要尽量避免这种SQL的产生。

#### 解决方案

由于水平拆分牵涉的逻辑比较复杂，当前也有了不少比较成熟的解决方案。这些方案分为两大类：客户端架构和代理架构。

##### 客户端架构

通过修改数据访问层，如JDBC、Data Source、MyBatis，通过配置来管理多个数据源，直连数据库，并在模块内完成数据的分片整合，一般以Jar包的方式呈现

这是一个客户端架构的例子：

![Alt text](https://segmentfault.com/img/remote/1460000006158210)

可以看到分片的实现是和应用服务器在一起的，通过修改Spring JDBC层来实现

客户端架构的优点是：

- 应用直连数据库，降低外围系统依赖所带来的宕机风险
- 集成成本低，无需额外运维的组件

缺点是：

- 限于只能在数据库访问层上做文章，扩展性一般，对于比较复杂的系统可能会力不从心
- 将分片逻辑的压力放在应用服务器上，造成额外风险

##### 代理架构

通过独立的中间件来统一管理所有数据源和数据分片整合，后端数据库集群对前端应用程序透明，需要独立部署和运维代理组件

这是一个代理架构的例子：

![Alt text](https://segmentfault.com/img/remote/1460000006767127)

代理组件为了分流和防止单点，一般以集群形式存在，同时可能需要Zookeeper之类的服务组件来管理

代理架构的优点是：

- 能够处理非常复杂的需求，不受数据库访问层原来实现的限制，扩展性强
- 对于应用服务器透明且没有增加任何额外负载

缺点是：

- 需部署和运维独立的代理中间件，成本高
- 应用需经过代理来连接数据库，网络上多了一跳，性能有损失且有额外风险

##### 各方案比较

|                                                              | 出品方    | 架构模型   | 支持数据库 | 分库 | 分表 | 读写分离 | 外部依赖 | 是否开源   | 实现语言 | 支持语言 | 最后更新 | Github星数 |
| ------------------------------------------------------------ | --------- | ---------- | ---------- | ---- | ---- | -------- | -------- | ---------- | -------- | -------- | -------- | ---------- |
| [MySQL Fabric](https://www.mysql.com/products/enterprise/fabric.html) | MySQL官方 | 代理架构   | MySQL      | 有   | 有   | 有       | 无       | 是         | python   | 无限制   | 4个月前  | 35         |
| [Cobar](https://github.com/alibaba/cobar)                    | 阿里巴巴  | 代理架构   | MySQL      | 有   | 无   | 无       | 无       | 是         | Java     | 无限制   | 两年前   | 1287       |
| [Cobar Client](https://github.com/alibaba/cobarclient)       | 阿里巴巴  | 客户端架构 | MySQL      | 有   | 无   | 无       | 无       | 是         | Java     | Java     | 三年前   | 344        |
| [TDDL](https://github.com/alibaba/tb_tddl)                   | 淘宝      | 客户端架构 | 无限制     | 有   | 有   | 有       | Diamond  | 只开源部分 | Java     | Java     | 未知     | 519        |
| [Atlas](https://github.com/Qihoo360/Atlas)                   | 奇虎360   | 代理架构   | MySQL      | 有   | 有   | 有       | 无       | 是         | C        | 无限制   | 10个月前 | 1941       |
| [Heisenberg](https://github.com/brucexx/heisenberg)          | 百度熊照  | 代理架构   | MySQL      | 有   | 有   | 有       | 无       | 是         | Java     | 无限制   | 2个月前  | 197        |
| [TribeDB](https://github.com/jojoin/TribeDB)                 | 个人      | 代理架构   | MySQL      | 有   | 有   | 有       | 无       | 是         | NodeJS   | 无限制   | 3个月前  | 126        |
| [ShardingJDBC](https://github.com/dangdangdotcom/sharding-jdbc) | 当当      | 客户端架构 | MySQL      | 有   | 有   | 有       | 无       | 是         | Java     | Java     | 当天     | 1144       |
| [Shark](https://github.com/gaoxianglong/shark)               | 个人      | 客户端架构 | MySQL      | 有   | 有   | 无       | 无       | 是         | Java     | Java     | 两天前   | 84         |
| [KingShard](https://github.com/flike/kingshard)              | 个人      | 代理架构   | MySQL      | 有   | 有   | 有       | 无       | 是         | Golang   | 无限制   | 两天前   | 1836       |
| [OneProxy](http://www.onexsoft.com/?page_id=3383)            | 平民软件  | 代理架构   | MySQL      | 有   | 有   | 有       | 无       | 否         | 未知     | 无限制   | 未知     | 未知       |
| [MyCat](http://mycat.io/)                                    | 社区      | 代理架构   | MySQL      | 有   | 有   | 有       | 无       | 是         | Java     | 无限制   | 两天前   | 1270       |
| [Vitess](https://github.com/youtube/vitess)                  | Youtube   | 代理架构   | MySQL      | 有   | 有   | 有       | 无       | 是         | Golang   | 无限制   | 当天     | 3636       |
| [Mixer](https://github.com/siddontang/mixer)                 | 个人      | 代理架构   | MySQL      | 有   | 有   | 无       | 无       | 是         | Golang   | 无限制   | 9个月前  | 472        |
| [JetPants](https://github.com/tumblr/jetpants)               | Tumblr    | 客户端架构 | MySQL      | 有   | 有   | 无       | 无       | 是         | Ruby     | Ruby     | 10个月前 | 957        |
| [HibernateShard](https://github.com/hibernate/hibernate-shards) | Hibernate | 客户端架构 | 无限制     | 有   | 有   | 无       | 无       | 是         | Java     | Java     | 4年前    | 57         |
| [MybatisShard](https://github.com/makersoft/mybatis-shards)  | MakerSoft | 客户端架构 | 无限制     | 有   | 有   | 无       | 无       | 是         | Java     | Java     | 11个月前 | 119        |
| [Gizzard](https://github.com/twitter/gizzard)                | Twitter   | 代理架构   | 无限制     | 有   | 有   | 无       | 无       | 是         | Java     | 无限制   | 3年前    | 2087       |

如此多的方案，如何进行选择？可以按以下思路来考虑：

1. 确定是使用代理架构还是客户端架构。中小型规模或是比较简单的场景倾向于选择客户端架构，复杂场景或大规模系统倾向选择代理架构
2. 具体功能是否满足，比如需要跨节点`ORDER BY`，那么支持该功能的优先考虑
3. 不考虑一年内没有更新的产品，说明开发停滞，甚至无人维护和技术支持
4. 最好按大公司->社区->小公司->个人这样的出品方顺序来选择
5. 选择口碑较好的，比如github星数、使用者数量质量和使用者反馈
6. 开源的优先，往往项目有特殊需求可能需要改动源代码

按照上述思路，推荐以下选择：

- 客户端架构：ShardingJDBC
- 代理架构：MyCat或者Atlas

### 兼容MySQL且可水平扩展的数据库

目前也有一些开源数据库兼容MySQL协议，如：

- [TiDB](https://github.com/pingcap/tidb)
- [Cubrid](http://www.cubrid.org/)

但其工业品质和MySQL尚有差距，且需要较大的运维投入，如果想将原始的MySQL迁移到可水平扩展的新数据库中，可以考虑一些云数据库：

- [阿里云PetaData](https://cn.aliyun.com/product/petadata/?spm=5176.7960203.237031.38.cAzx5r)
- [阿里云OceanBase](https://cn.aliyun.com/product/oceanbase?spm=5176.7960203.237031.40.cAzx5r)
- [腾讯云DCDB](https://www.qcloud.com/product/dcdb_for_tdsql.html)

### NoSQL

在MySQL上做Sharding是一种戴着镣铐的跳舞，事实上很多大表本身对MySQL这种RDBMS的需求并不大，并不要求ACID，可以考虑将这些表迁移到NoSQL，彻底解决水平扩展问题，例如：

- 日志类、监控类、统计类数据
- 非结构化或弱结构化数据
- 对事务要求不强，且无太多关联操作的数据

# MySQL锁机制

一 锁分类（按照锁的粒度分类）
Mysql为了解决并发、数据安全的问题，使用了锁机制。

可以按照锁的粒度把数据库锁分为表级锁和行级锁。

表级锁

Mysql中锁定 粒度最大 的一种锁，对当前操作的整张表加锁，实现简单 ，资源消耗也比较少，加锁快，不会出现死锁 。其锁定粒度最大，触发锁冲突的概率最高，并发度最低，MyISAM和 InnoDB引擎都支持表级锁。

行级锁

Mysql中锁定 粒度最小 的一种锁，只针对当前操作的行进行加锁。 行级锁能大大减少数据库操作的冲突。其加锁粒度最小，并发度高，但加锁的开销也最大，加锁慢，会出现死锁。 InnoDB支持的行级锁，包括如下几种。

**Record Lock: 对索引项加锁，锁定符合条件的行。其他事务不能修改和删除加锁项；**
**Gap Lock: 对索引项之间的“间隙”加锁，锁定记录的范围（对第一条记录前的间隙或最后一条记录后的间隙加锁），不包含索引项本身。其他事务不能在锁范围内插入数据，这样就防止了别的事务新增幻影行。**
**Next-key Lock： 锁定索引项本身和索引范围。即Record Lock和Gap Lock的结合。可解决幻读问题。**
虽然使用行级索具有粒度小、并发度高等特点，但是表级锁有时候也是非常必要的：

事务更新大表中的大部分数据直接使用表级锁效率更高；
事务比较复杂，使用行级索很可能引起死锁导致回滚。
二 锁分类（按照是否可写分类） 
表级锁和行级锁可以进一步划分为共享锁（s）和排他锁（X）。

共享锁（s）

共享锁（Share Locks，简记为S）又被称为读锁，其他用户可以并发读取数据，但任何事务都不能获取数据上的排他锁，直到已释放所有共享锁。

共享锁(S锁)又称为读锁，若事务T对数据对象A加上S锁，则事务T只能读A；其他事务只能再对A加S锁，而不能加X锁，直到T释放A上的S锁。这就保证了其他事务可以读A，但在T释放A上的S锁之前不能对A做任何修改。

排他锁（X）：

排它锁（(Exclusive lock,简记为X锁)）又称为写锁，若事务T对数据对象A加上X锁，则只允许T读取和修改A，其它任何事务都不能再对A加任何类型的锁，直到T释放A上的锁。它防止任何其它事务获取资源上的锁，直到在事务的末尾将资源上的原始锁释放为止。在更新操作(INSERT、UPDATE 或 DELETE)过程中始终应用排它锁。

两者之间的区别：

共享锁（S锁）：如果事务T对数据A加上共享锁后，则其他事务只能对A再加共享锁，不 能加排他锁。获取共享锁的事务只能读数据，不能修改数据。

排他锁（X锁）：如果事务T对数据A加上排他锁后，则其他事务不能再对A加任任何类型的封锁。获取排他锁的事务既能读数据，又能修改数据。

三 另外两个表级锁：IS和IX
当一个事务需要给自己需要的某个资源加锁的时候，如果遇到一个共享锁正锁定着自己需要的资源的时候，自己可以再加一个共享锁，不过不能加排他锁。但是，如果遇到自己需要锁定的资源已经被一个排他锁占有之后，则只能等待该锁定释放资源之后自己才能获取锁定资源并添加自己的锁定。而意向锁的作用就是当一个事务在需要获取资源锁定的时候，如果遇到自己需要的资源已经被排他锁占用的时候，该事务可以需要锁定行的表上面添加一个合适的意向锁。如果自己需要一个共享锁，那么就在表上面添加一个意向共享锁。而如果自己需要的是某行（或者某些行）上面添加一个排他锁的话，则先在表上面添加一个意向排他锁。意向共享锁可以同时并存多个，但是意向排他锁同时只能有一个存在。

InnoDB另外的两个表级锁：

意向共享锁（IS）： 表示事务准备给数据行记入共享锁，事务在一个数据行加共享锁前必须先取得该表的IS锁。

意向排他锁（IX）： 表示事务准备给数据行加入排他锁，事务在一个数据行加排他锁前必须先取得该表的IX锁。

注意：

这里的意向锁是表级锁，表示的是一种意向，仅仅表示事务正在读或写某一行记录，在真正加行锁时才会判断是否冲突。意向锁是InnoDB自动加的，不需要用户干预。
IX，IS是表级锁，不会和行级的X，S锁发生冲突，只会和表级的X，S发生冲突。
InnoDB的锁机制兼容情况如下：


当一个事务请求的锁模式与当前的锁兼容，InnoDB就将请求的锁授予该事务；反之如果请求不兼容，则该事物就等待锁释放。

四 死锁和避免死锁
InnoDB的行级锁是基于索引实现的，如果查询语句为命中任何索引，那么InnoDB会使用表级锁. 此外，InnoDB的行级锁是针对索引加的锁，不针对数据记录，因此即使访问不同行的记录，如果使用了相同的索引键仍然会出现锁冲突，还需要注意的是，在通过

SELECT ...LOCK IN SHARE MODE;
1
或

SELECT ...FOR UPDATE;
1
使用锁的时候，如果表没有定义任何索引，那么InnoDB会创建一个隐藏的聚簇索引并使用这个索引来加记录锁。

此外，不同于MyISAM总是一次性获得所需的全部锁，InnoDB的锁是逐步获得的，当两个事务都需要获得对方持有的锁，导致双方都在等待，这就产生了死锁。 发生死锁后，InnoDB一般都可以检测到，并使一个事务释放锁回退，另一个则可以获取锁完成事务，我们可以采取以上方式避免死锁：

通过表级锁来减少死锁产生的概率；
多个程序尽量约定以相同的顺序访问表（这也是解决并发理论中哲学家就餐问题的一种思路）；
同一个事务尽可能做到一次锁定所需要的所有资源。
五 总结与补充
MyISAM和InnoDB存储引擎使用的锁：

MyISAM采用表级锁(table-level locking)。
InnoDB支持行级锁(row-level locking)和表级锁,默认为行级锁
表级锁和行级锁对比：

表级锁： Mysql中锁定 粒度最大 的一种锁，对当前操作的整张表加锁，实现简单，资源消耗也比较少，加锁快，不会出现死锁。其锁定粒度最大，触发锁冲突的概率最高，并发度最低，MyISAM和 InnoDB引擎都支持表级锁。
行级锁： Mysql中锁定 粒度最小 的一种锁，只针对当前操作的行进行加锁。 行级锁能大大减少数据库操作的冲突。其加锁粒度最小，并发度高，但加锁的开销也最大，加锁慢，会出现死锁。
补充：

页级锁： MySQL中锁定粒度介于行级锁和表级锁中间的一种锁。表级锁速度快，但冲突多，行级冲突少，但速度慢。页级进行了折衷，一次锁定相邻的一组记录。BDB支持页级锁。开销和加锁时间界于表锁和行锁之间，会出现死锁。锁定粒度界于表锁和行锁之间，并发度一般。
————————————————
版权声明：本文为CSDN博主「JavaGuide」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_34337272/article/details/80611486

# 一条SQL语句在MySQL中如何执行

# 一致性

数据库事务的一致性是指：**保证事务只能把数据库从一个有效（正确）的状态“转移”到另一个有效（正确）的状态。**那么，什么是数据库的有效(正确）的状态？满足给这个数据库pred-defined的一些规则的状态都是 valid 的。这些规则有哪些呢，比如说[constraints](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Integrity_constraints), [cascades](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Cascading_rollback),[triggers](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Database_trigger) 及它们的组合等。具体到某个表的某个字段，比如你在定义表的时候，给这个字段的类型是number类型，并且它的值不能小于0，那么你在某个 transaction 中给这个字段插入（更改）为一个 String 值或者是负值是不可以的，这不是一个“合法”的transaction，也就是说它不满足我们给数据库定义的一些规则（约束条件）。



作者：sleep deep
链接：https://www.zhihu.com/question/31346392/answer/569142076
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## 一 MySQL 基础架构分析

### 1.1 MySQL 基本架构概览

下图是 MySQL 的一个简要架构图，从下图你可以很清晰的看到用户的 SQL 语句在 MySQL 内部是如何执行的。

先简单介绍一下下图涉及的一些组件的基本作用帮助大家理解这幅图，在 1.2 节中会详细介绍到这些组件的作用。

•**连接器：** 身份认证和权限相关(登录 MySQL 的时候)。•**查询缓存:** 执行查询语句的时候，会先查询缓存（MySQL 8.0 版本后移除，因为这个功能不太实用）。•**分析器:** 没有命中缓存的话，SQL 语句就会经过分析器，分析器说白了就是要先看你的 SQL 语句要干嘛，再检查你的 SQL 语句语法是否正确。•**优化器：** 按照 MySQL 认为最优的方案去执行。•**执行器:** 执行语句，然后从存储引擎返回数据。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/iaIdQfEric9TzWuuhjqx58LnibzsWR0Pf8x9nVefLe59Q8SBNcZGIGn1VGNFfNUVQyOwQksDoyvIOUJicgzU6ICVLg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

简单来说 MySQL 主要分为 Server 层和存储引擎层：

•**Server 层**：主要包括连接器、查询缓存、分析器、优化器、执行器等，所有跨存储引擎的功能都在这一层实现，比如存储过程、触发器、视图，函数等，还有一个通用的日志模块 binglog 日志模块。•**存储引擎**： 主要负责数据的存储和读取，采用可以替换的插件式架构，支持 InnoDB、MyISAM、Memory 等多个存储引擎，其中 InnoDB 引擎有自有的日志模块 redolog 模块。**现在最常用的存储引擎是 InnoDB，它从 MySQL 5.5.5 版本开始就被当做默认存储引擎了。**

### 1.2 Server 层基本组件介绍

### 1) 连接器

连接器主要和身份认证和权限相关的功能相关，就好比一个级别很高的门卫一样。

主要负责用户登录数据库，进行用户的身份认证，包括校验账户密码，权限等操作，如果用户账户密码已通过，连接器会到权限表中查询该用户的所有权限，之后在这个连接里的权限逻辑判断都是会依赖此时读取到的权限数据，也就是说，后续只要这个连接不断开，即时管理员修改了该用户的权限，该用户也是不受影响的。

### 2) 查询缓存(MySQL 8.0 版本后移除)

查询缓存主要用来缓存我们所执行的 SELECT 语句以及该语句的结果集。

连接建立后，执行查询语句的时候，会先查询缓存，MySQL 会先校验这个 sql 是否执行过，以 Key-Value 的形式缓存在内存中，Key 是查询预计，Value 是结果集。如果缓存 key 被命中，就会直接返回给客户端，如果没有命中，就会执行后续的操作，完成后也会把结果缓存起来，方便下一次调用。当然在真正执行缓存查询的时候还是会校验用户的权限，是否有该表的查询条件。

MySQL 查询不建议使用缓存，因为查询缓存失效在实际业务场景中可能会非常频繁，假如你对一个表更新的话，这个表上的所有的查询缓存都会被清空。对于不经常更新的数据来说，使用缓存还是可以的。

所以，一般在大多数情况下我们都是不推荐去使用查询缓存的。

MySQL 8.0 版本后删除了缓存的功能，官方也是认为该功能在实际的应用场景比较少，所以干脆直接删掉了。

### 3) 分析器

MySQL 没有命中缓存，那么就会进入分析器，分析器主要是用来分析 SQL 语句是来干嘛的，分析器也会分为几步：

**第一步，词法分析**，一条 SQL 语句有多个字符串组成，首先要提取关键字，比如 select，提出查询的表，提出字段名，提出查询条件等等。做完这些操作后，就会进入第二步。

**第二步，语法分析**，主要就是判断你输入的 sql 是否正确，是否符合 MySQL 的语法。

完成这 2 步之后，MySQL 就准备开始执行了，但是如何执行，怎么执行是最好的结果呢？这个时候就需要优化器上场了。

### 4) 优化器

优化器的作用就是它认为的最优的执行方案去执行（有时候可能也不是最优，这篇文章涉及对这部分知识的深入讲解），比如多个索引的时候该如何选择索引，多表查询的时候如何选择关联顺序等。

可以说，经过了优化器之后可以说这个语句具体该如何执行就已经定下来。

### 5) 执行器

当选择了执行方案后，MySQL 就准备开始执行了，首先执行前会校验该用户有没有权限，如果没有权限，就会返回错误信息，如果有权限，就会去调用引擎的接口，返回接口执行的结果。

## 二 语句分析

### 2.1 查询语句

说了以上这么多，那么究竟一条 sql 语句是如何执行的呢？其实我们的 sql 可以分为两种，一种是查询，一种是更新（增加，更新，删除）。我们先分析下查询语句，语句如下：

- 

```
select * from tb_student  A where A.age='18' and A.name=' 张三 ';
```

结合上面的说明，我们分析下这个语句的执行流程：

- 先检查该语句是否有权限，如果没有权限，直接返回错误信息，如果有权限，在 MySQL8.0 版本以前，会先查询缓存，以这条 sql 语句为 key 在内存中查询是否有结果，如果有直接缓存，如果没有，执行下一步。
- 通过分析器进行词法分析，提取 sql 语句的关键元素，比如提取上面这个语句是查询 select，提取需要查询的表名为 tb_student,需要查询所有的列，查询条件是这个表的 id='1'。然后判断这个 sql 语句是否有语法错误，比如关键词是否正确等等，如果检查没问题就执行下一步。
- 接下来就是优化器进行确定执行方案，上面的 sql 语句，可以有两种执行方案：



- 
- 

```
  a.先查询学生表中姓名为“张三”的学生，然后判断是否年龄是 18。  b.先找出学生中年龄 18 岁的学生，然后再查询姓名为“张三”的学生。
```

​    那么优化器根据自己的优化算法进行选择执行效率最好的一个方案（优化器认为，有时候不一定最好）。那么确认了执行计划后就准备开始执行了。

- 

  进行权限校验，如果没有权限就会返回错误信息，如果有权限就会调用数据库引擎接口，返回引擎的执行结果。



### 2.2 更新语句

以上就是一条查询 sql 的执行流程，那么接下来我们看看一条更新语句如何执行的呢？sql 语句如下：

- 

```
update tb_student A set A.age='19' where A.name=' 张三 ';
```

我们来给张三修改下年龄，在实际数据库肯定不会设置年龄这个字段的，不然要被技术负责人打的。其实条语句也基本上会沿着上一个查询的流程走，只不过执行更新的时候肯定要记录日志啦，这就会引入日志模块了，MySQL 自带的日志模块式 **binlog（归档日志）** ，所有的存储引擎都可以使用，我们常用的 InnoDB 引擎还自带了一个日志模块 **redo log（重做日志）**，我们就以 InnoDB 模式下来探讨这个语句的执行流程。流程如下：

•先查询到张三这一条数据，如果有缓存，也是会用到缓存。•然后拿到查询的语句，把 age 改为 19，然后调用引擎 API 接口，写入这一行数据，InnoDB 引擎把数据保存在内存中，同时记录 redo log，此时 redo log 进入 prepare 状态，然后告诉执行器，执行完成了，随时可以提交。•执行器收到通知后记录 binlog，然后调用引擎接口，提交 redo log 为提交状态。•更新完成。

**这里肯定有同学会问，为什么要用两个日志模块，用一个日志模块不行吗?**

这是因为最开始 MySQL 并没与 InnoDB 引擎( InnoDB 引擎是其他公司以插件形式插入 MySQL 的) ，MySQL 自带的引擎是 MyISAM，但是我们知道 redo log 是 InnoDB 引擎特有的，其他存储引擎都没有，这就导致会没有 crash-safe 的能力(crash-safe 的能力即使数据库发生异常重启，之前提交的记录都不会丢失)，binlog 日志只能用来归档。

并不是说只用一个日志模块不可以，只是 InnoDB 引擎就是通过 redo log 来支持事务的。那么，又会有同学问，我用两个日志模块，但是不要这么复杂行不行，为什么 redo log 要引入 prepare 预提交状态？这里我们用反证法来说明下为什么要这么做？

•**先写 redo log 直接提交，然后写 binlog**，假设写完 redo log 后，机器挂了，binlog 日志没有被写入，那么机器重启后，这台机器会通过 redo log 恢复数据，但是这个时候 bingog 并没有记录该数据，后续进行机器备份的时候，就会丢失这一条数据，同时主从同步也会丢失这一条数据。•**先写 binlog，然后写 redo log**，假设写完了 binlog，机器异常重启了，由于没有 redo log，本机是无法恢复这一条记录的，但是 binlog 又有记录，那么和上面同样的道理，就会产生数据不一致的情况。

如果采用 redo log 两阶段提交的方式就不一样了，写完 binglog 后，然后再提交 redo log 就会防止出现上述的问题，从而保证了数据的一致性。那么问题来了，有没有一个极端的情况呢？假设 redo log 处于预提交状态，binglog 也已经写完了，这个时候发生了异常重启会怎么样呢？ 这个就要依赖于 MySQL 的处理机制了，MySQL 的处理过程如下：

•判断 redo log 是否完整，如果判断是完整的，就立即提交。•如果 redo log 只是预提交但不是 commit 状态，这个时候就会去判断 binlog 是否完整，如果完整就提交 redo log, 不完整就回滚事务。

这样就解决了数据一致性的问题。

## 三 总结

•MySQL 主要分为 Server 层和引擎层，Server 层主要包括连接器、查询缓存、分析器、优化器、执行器，同时还有一个日志模块（binlog），这个日志模块所有执行引擎都可以共用,redolog 只有 InnoDB 有。•引擎层是插件式的，目前主要包括，MyISAM,InnoDB,Memory 等。•SQL 等执行过程分为两类，一类对于查询等过程如下：权限校验---》查询缓存---》分析器---》优化器---》权限校验---》执行器---》引擎•对于更新等语句执行流程如下：分析器----》权限校验----》执行器---》引擎---redo log prepare---》binlog---》redo log commit

# row_number窗口函数

## MySQL ROW_NUMBER() 语法

MySQL `ROW_NUMBER()`从8.0版开始引入了功能。这`ROW_NUMBER()`是一个[窗口函数](https://www.begtut.com/mysql/mysql-window-functions.html)或分析函数，它为从1开始应用的每一行分配一个序号。

请注意，如果你使用MySQL版本低于8.0，你可以[效仿的一些功能`ROW_NUMBER()`函数](https://www.begtut.com/mysql/mysql-row_number.html)使用各种技术。

以下显示了`ROW_NUMBER()`函数的语法：

```
ROW_NUMBER() OVER (<partition_definition> <order_definition>) 
```

### partition_definition

`partition_definition`语法如下：

```
PARTITION BY <expression>,[{,<expression>}...] 
```

`PARTITION BY`子句将行分成更小的集合。表达式可以是将在`GROUP BY`子句中使用的任何有效表达式。您可以使用以逗号分隔的多个表达式。

`PARTITION BY`条款是可选项。如果省略它，则整个结果集被视为分区。但是，当您使用`PARTITION BY`子句时，每个分区也可以被视为一个窗口。

### order_definition

的`order_definition`语法如下所示：

```
ORDER BY <expression> [ASC|DESC],[{,<expression>}...] 
```

`ORDER BY`子句的目的是设置行的顺序。此`ORDER BY`子句独立`ORDER BY`于查询的子句。

------

## MySQL ROW_NUMBER() 函数示例

让我们使用示例数据库中的`products`表进行演示：

### 1）为行分配序号

以下语句使用`ROW_NUMBER()`函数为`products`表中的每一行分配一个序号：

```
SELECT 
 ROW_NUMBER() OVER (
 ORDER BY productName
 ) row_num,
    productName,
    msrp
FROM 
 products
ORDER BY 
 productName; 
```

这是输出：

```
+---------+---------------------------------------------+--------+
| row_num | productName                                 | msrp   |
+---------+---------------------------------------------+--------+
|       1 | 18th century schooner                       | 122.89 |
|       2 | 18th Century Vintage Horse Carriage         | 104.72 |
|       3 | 1900s Vintage Bi-Plane                      |  68.51 |
|       4 | 1900s Vintage Tri-Plane                     |  72.45 |
|       5 | 1903 Ford Model A                           | 136.59 |
|       6 | 1904 Buick Runabout                         |  87.77 |
|       7 | 1911 Ford Town Car                          |  60.54 |
|       8 | 1912 Ford Model T Delivery Wagon            |  88.51 |
|       9 | 1913 Ford Model T Speedster                 | 101.31 |
|      10 | 1917 Grand Touring Sedan                    | 170.00 |
|      11 | 1917 Maxwell Touring Car                    |  99.21 |
|      12 | 1926 Ford Fire Engine                       |  60.77 |
|      13 | 1928 British Royal Navy Airplane            | 109.42 |
...
```

### 2）找到每组的前N行

您可以将`ROW_NUMBER()`功能用于查找每个组的前N行的查询，例如，每个销售渠道的前三名销售员工，每个类别的前五名高性能产品。

以下语句查找每个产品系列中库存最高的前三种产品：

```
WITH inventory
AS (SELECT 
       productLine,
       productName,
       quantityInStock,
       ROW_NUMBER() OVER (
          PARTITION BY productLine 
          ORDER BY quantityInStock DESC) row_num
    FROM 
       products
   )
SELECT 
   productLine,
   productName,
   quantityInStock
FROM 
   inventory
WHERE 
   row_num <= 3; 
```

在这个例子中，

- 首先，我们使用`ROW_NUMER()`函数对每个产品系列中的所有产品的库存进行排序，方法是按产品线划分所有产品，并按库存数量按降序排序。结果，每个产品根据其库存数量分配一个等级。并为每个产品系列重置排名。
- 然后，我们只选择等级小于或等于3的产品。

以下显示输出：

```
+------------------+----------------------------------------+-----------------+
| productLine      | productName                            | quantityInStock |
+------------------+----------------------------------------+-----------------+
| Classic Cars     | 1995 Honda Civic                       |            9772 |
| Classic Cars     | 2002 Chevy Corvette                    |            9446 |
| Classic Cars     | 1976 Ford Gran Torino                  |            9127 |
| Motorcycles      | 2002 Suzuki XREO                       |            9997 |
| Motorcycles      | 1982 Ducati 996 R                      |            9241 |
| Motorcycles      | 1969 Harley Davidson Ultimate Chopper  |            7933 |
| Planes           | America West Airlines B757-200         |            9653 |
| Planes           | American Airlines: MD-11S              |            8820 |
| Planes           | ATA: B757-300                          |            7106 |
| Ships            | The USS Constitution Ship              |            7083 |
| Ships            | The Queen Mary                         |            5088 |
| Ships            | 1999 Yamaha Speed Boat                 |            4259 |
| Trains           | 1950's Chicago Surface Lines Streetcar |            8601 |
| Trains           | Collectable Wooden Train               |            6450 |
| Trains           | 1962 City of Detroit Streetcar         |            1645 |
| Trucks and Buses | 1964 Mercedes Tour Bus                 |            8258 |
| Trucks and Buses | 1957 Chevy Pickup                      |            6125 |
| Trucks and Buses | 1980鈥檚 GM Manhattan Express          |            5099 |
| Vintage Cars     | 1932 Model A Ford J-Coupe              |            9354 |
| Vintage Cars     | 1912 Ford Model T Delivery Wagon       |            9173 |
| Vintage Cars     | 1937 Lincoln Berline                   |            8693 |
+------------------+----------------------------------------+-----------------+
21 rows in set (0.03 sec)
```

### 3）删除重复的行

您可以使用`ROW_NUMBER()`它将非唯一行转换为唯一行，然后[删除重复行](https://www.begtut.com/mysql/mysql-delete-duplicate-rows.html)。请考虑以下示例。

首先，创建一个包含一些重复值的表：

```
DROP TABLE IF EXISTS rowNumberDemo;
CREATE TABLE rowNumberDemo (
    id INT,
    name VARCHAR(10) NOT NULL
);
 
INSERT INTO rowNumberDemo(id,name) 
VALUES(1,'A'),
      (2,'B'),
      (3,'B'),
      (4,'C'),
      (5,'C'),
      (6,'C'),
      (7,'D'); 
```

其次，使用`ROW_NUMBER()`函数将行划分为所有列的分区。对于每个唯一的行集，将重新开始行号。

```
SELECT 
    id,
    name,
    ROW_NUMBER() OVER (PARTITION BY name ORDER BY name) AS row_num
FROM rowNumberDemo; 
+------+------+---------+
| id   | name | row_num |
+------+------+---------+
|    1 | A    |       1 |
|    2 | B    |       1 |
|    3 | B    |       2 |
|    4 | C    |       1 |
|    5 | C    |       2 |
|    6 | C    |       3 |
|    7 | D    |       1 |
+------+------+---------+
7 rows in set (0.02 sec)
```

从输出中可以看出，唯一的行是行号等于1的行。

第三，您可以使用[公用表表达式](https://www.begtut.com/mysql/mysql-cte.html)（CTE）返回要删除的重复行和delete语句：

```
WITH dups AS (SELECT 
        id,
        name,
        ROW_NUMBER() OVER(PARTITION BY name ORDER BY name) AS row_num
    FROM rowNumberDemo)
DELETE rowNumberDemo FROM rowNumberDemo INNER JOIN dups ON rowNumberDemo.id = dups.id
WHERE dups.row_num <> 1; 
+------+------+
| id   | name |
+------+------+
|    1 | A    |
|    2 | B    |
|    4 | C    |
|    7 | D    |
+------+------+
4 rows in set (0.01 sec)
```

请注意，MySQL不支持基于CTE的删除，因此，我们必须将原始表与CTE一起作为一种解决方法。

### 4）使用`ROW_NUMBER()`函数分页

因为`ROW_NUMBER()`为结果集中的每一行指定一个唯一的数字，所以可以将其用于分页。

假设您需要显示每页包含10个产品的产品列表。要获取第二页的产品，请使用以下查询：

```
SELECT *
FROM 
    (SELECT productName,
         msrp,
         row_number()
        OVER (order by msrp) AS row_num
    FROM products) t
WHERE row_num BETWEEN 11 AND 20; 
```

这是输出：

```
+------------------------------------------+-------+---------+
| productName                              | msrp  | row_num |
+------------------------------------------+-------+---------+
| 1936 Mercedes-Benz 500K Special Roadster | 53.91 |      11 |
| 1954 Greyhound Scenicruiser              | 54.11 |      12 |
| Pont Yacht                               | 54.60 |      13 |
| 1970 Dodge Coronet                       | 57.80 |      14 |
| 1962 City of Detroit Streetcar           | 58.58 |      15 |
| 1911 Ford Town Car                       | 60.54 |      16 |
| 1936 Harley Davidson El Knucklehead      | 60.57 |      17 |
| 1926 Ford Fire Engine                    | 60.77 |      18 |
| 1971 Alpine Renault 1600s                | 61.23 |      19 |
| 1950's Chicago Surface Lines Streetcar   | 62.14 |      20 |
+------------------------------------------+-------+---------+
10 rows in set (0.02 sec)
```

在本教程中，您学习了如何使用MySQL `ROW_NUMBER()`函数为结果集中的每一行生成序列号。

# partition by和group by区别

over partition by与group by是都是分组统计的函数。

区别
1. over partition by 其中partition by 只是over一个子句参数，作用就是分组。over 子句可以与聚合函数结合使用(max、min、sum、avg、count等).下面我们看一个例子

-- 创建表并插入数据
CREATE TABLE Employee
(
  ID int identity(1,1),
  EmpName varchar(20),
  EmpSalary varchar(10),
  EmpDepartment varchar(20) 
);

INSERT INTO Employee
SELECT '张三','5000','开发部' UNION ALL
SELECT '李四','2000','销售部'  UNION ALL
SELECT '王麻子','2500','销售部' UNION ALL
SELECT '张三表叔','8000','开发部' UNION ALL
SELECT '李四表叔','5000','开发部' UNION ALL
SELECT '王麻子表叔','5000','销售部'

-- 现在使用来对 部门进行分组,使用over partition by分组,按照工资排序(使用ROW_NUMBER()函数来生成一列)
SELECT * ,ROW_NUMBER() OVER(PARTITION BY EmpDepartment ORDER BY EmpSalary) EmpNumber FROM Employee

-- 执行结果如下:

ID          EmpName              EmpSalary  EmpDepartment        EmpNumber
--------- -------------------- ---------- -------------------- --------------------
1           张三                   5000       开发部                  1
5           李四表叔                 5000       开发部                  2
4           张三表叔                 8000       开发部                  3
6           王麻子表叔                5000       销售部                   1
2           李四                   2000       销售部                  1
3           王麻子                  2500       销售部                  2


 2. 假如现在 使用  over partition by 分组，并取得最高工资的员工信息，我们看看会有什么问题

SELECT * ,MAX(EmpSalary) OVER(PARTITION BY EmpDepartment) MaxSalary FROM Employee

-- 执行结果

ID          EmpName              EmpSalary  EmpDepartment        MaxSalary
----------- -------------------- ---------- -------------------- ----------
1           张三                   5000       开发部                  8000
4           张三表叔                 8000       开发部                  8000
5           李四表叔                 5000       开发部                  8000
6           王麻子表叔                5000       销售部                  5000
2           李四                   2000       销售部                  5000
3           王麻子                  2500       销售部                  5000
(6 行受影响)


查看执行结果不难发现，最大工资是获取到了，可是不需要的列也显示出来了。

使用Group by来分组就没有（相信可能使用over partition by 也可以实现如group by 这样的分组）

SELECT EmpDepartment,MAX(EmpSalary) FROM Employee 
GROUP BY EmpDepartment

-- 执行结果
EmpDepartment        
-------------------- ----------
开发部                  8000
销售部                  5000

(2 行受影响)

group by是对检索结果的保留行进行单纯分组，一般和聚合函数一起使用例如max、min、sum、avg、count等一块用。 (官方参考文档：http://technet.microsoft.com/zh-cn/library/ms177673.aspx)

partition by虽然也具有分组功能，但同时也具有其他的高级功能。(可以参考官方文档：http://technet.microsoft.com/zh-cn/library/ms189461.aspx)
————————————————
版权声明：本文为CSDN博主「Tragedy」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/gyc1105/article/details/8074545

# with as 子句

一直以来很少在SQL中使用过with  as  的用法，现在打算记录这条语句的使用方法。

 WITH AS短语，也叫做子查询部分（subquery factoring），是用来定义一个SQL片断，该SQL片断会被整个SQL语句所用到。这个语句算是公用表表达式（CTE）。

  比如

 with A as (select * from class)

    select *from A  

这个语句的意思就是，先执行select * from class   得到一个结果，将这个结果记录为A  ，在执行select *from A  语句。A 表只是一个别名。

也就是将重复用到的大批量 的SQL语句，放到with  as 中，加一个别名，在后面用到的时候就可以直接用。

对于大批量的SQL数据，起到优化的作用。

二、with的相关总结（摘录别人博客）
1.使用with子句可以让子查询重用相同的with查询块,通过select调用（with子句只能被select查询块引用），一般在with查询用到多次情况下。在引用的select语句之前定义,同级只能定义with关键字只能使用一次,多个用逗号分割。


2.with子句的返回结果存到用户的临时表空间中，只做一次查询，反复使用,提高效率。


3.在同级select前有多个查询定义的时候，第1个用with，后面的不用with，并且用逗号隔开。


4.最后一个with 子句与下面的查询之间不能有逗号，只通过右括号分割,with 子句的查询必须用括号括起来


5.如果定义了with子句，而在查询中不使用，那么会报ora-32035 错误：未引用在with子句中定义的查询名。（至少一个with查询的name未被引用，解决方法是移除未被引用的with查询），注意：只要后面有引用的就可以，不一定非要在主查询中引用，比如后面的with查询也引用了，也是可以的。

6.前面的with子句定义的查询在后面的with子句中可以使用。但是一个with子句内部不能嵌套with子句。


7.当一个查询块名字和一个表名或其他的对象相同时，解析器从内向外搜索，优先使用子查询块名字。

8.with查询的结果列有别名，引用的时候必须使用别名或*。
————————————————
版权声明：本文为CSDN博主「我是一只小小小菜鸟」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/baidu_30527569/article/details/48680745

# Sql四大排名函数详解

排名函数是Sql Server2005新增的功能，下面简单介绍一下他们各自的用法和区别。我们新建一张Order表并添加一些初始数据方便我们查看效果。

 

![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
CREATE TABLE [dbo].[Order](
    [ID] [int] IDENTITY(1,1) NOT NULL,
    [UserId] [int] NOT NULL,
    [TotalPrice] [int] NOT NULL,
    [SubTime] [datetime] NOT NULL,
 CONSTRAINT [PK_Order] PRIMARY KEY CLUSTERED 
(
    [ID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
SET IDENTITY_INSERT [dbo].[Order] ON 

GO
INSERT [dbo].[Order] ([ID], [UserId], [TotalPrice], [SubTime]) VALUES (1, 1, 100, CAST(0x0000A419011D32AF AS DateTime))
GO
INSERT [dbo].[Order] ([ID], [UserId], [TotalPrice], [SubTime]) VALUES (2, 2, 500, CAST(0x0000A419011D40BA AS DateTime))
GO
INSERT [dbo].[Order] ([ID], [UserId], [TotalPrice], [SubTime]) VALUES (3, 3, 300, CAST(0x0000A419011D4641 AS DateTime))
GO
INSERT [dbo].[Order] ([ID], [UserId], [TotalPrice], [SubTime]) VALUES (4, 2, 1000, CAST(0x0000A419011D4B72 AS DateTime))
GO
INSERT [dbo].[Order] ([ID], [UserId], [TotalPrice], [SubTime]) VALUES (5, 1, 520, CAST(0x0000A419011D50F3 AS DateTime))
GO
INSERT [dbo].[Order] ([ID], [UserId], [TotalPrice], [SubTime]) VALUES (6, 2, 2000, CAST(0x0000A419011E50C9 AS DateTime))
GO
SET IDENTITY_INSERT [dbo].[Order] OFF
GO
ALTER TABLE [dbo].[Order] ADD  CONSTRAINT [DF_Order_SubTime]  DEFAULT (getdate()) FOR [SubTime]
GO
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

 

　　附上表结构和初始数据图：

　　![表结构和初始数据-晓菜鸟](https://images.cnblogs.com/cnblogs_com/52XF/642690/o_data.png)

 

### 一、ROW_NUMBER

　　row_number的用途的非常广泛，排序最好用他，一般可以用来实现web程序的分页，他会为查询出来的每一行记录生成一个序号，依次排序且不会重复，注意使用row_number函数时必须要用over子句选择对某一列进行排序才能生成序号。row_number用法实例:

 

```
select ROW_NUMBER() OVER(order by [SubTime] desc) as row_num,* from [Order]
```

 

　　查询结果如下图所示：

　　![row_number查询结果-晓菜鸟](https://images.cnblogs.com/cnblogs_com/52XF/642690/o_row_number.png)

　　图中的row_num列就是row_number函数生成的序号列，其基本原理是先使用over子句中的排序语句对记录进行排序，然后按照这个顺序生成序号。over子句中的order by子句与SQL语句中的order by子句没有任何关系，这两处的order by 可以完全不同，如以下sql，over子句中根据SubTime降序排列，Sql语句中则按TotalPrice降序排列。

```
select ROW_NUMBER() OVER(order by [SubTime] desc) as row_num,* from [Order] order by [TotalPrice] desc
```

　　查询结果如下图所示：

　　![over子句和sql语句中的order by 可完全不同-晓菜鸟](https://images.cnblogs.com/cnblogs_com/52XF/642690/o_row_number_2.png)

　　利用row_number可以实现web程序的分页，我们来查询指定范围的表数据。例：根据订单提交时间倒序排列获取第三至第五条数据。

```
with orderSection as
(
    select ROW_NUMBER() OVER(order by [SubTime] desc) rownum,* from [Order]
)
select * from [orderSection] where rownum between 3 and 5 order by [SubTime] desc
```

　　查询结果如下图所示：

　　![利用row_number实现分页-晓菜鸟](https://images.cnblogs.com/cnblogs_com/52XF/642690/o_row_number_3.1.png)

　　**注意：在使用row_number实现分页时需要特别注意一点，over子句中的order by 要与Sql排序记录中的order by 保持一致，否则得到的序号可能不是连续的。**下面我们写一个例子来证实这一点，将上面Sql语句中的排序字段由SubTime改为TotalPrice。**另外提一下，对于带有子查询和CTE的查询，子查询和CTE查询有序并不代表整个查询有序，除非显示指定了order by。**

```
with orderSection as
(
    select ROW_NUMBER() OVER(order by [SubTime] desc) rownum,* from [Order]
)
select * from [orderSection] where rownum between 3 and 5 order by [TotalPrice] desc
```

　　查询结果如下图所示：

　　![over子句中的order by 与sql排序的order by 不一致-晓菜鸟](https://images.cnblogs.com/cnblogs_com/52XF/642690/o_row_number_3.2.png)

　　

### 二、RANK

　　rank函数用于返回结果集的分区内每行的排名， 行的排名是相关行之前的排名数加一。简单来说rank函数就是对查询出来的记录进行排名，与row_number函数不同的是，rank函数考虑到了over子句中排序字段值相同的情况，如果使用rank函数来生成序号，over子句中排序字段值相同的序号是一样的，后面字段值不相同的序号将跳过相同的排名号排下一个，也就是相关行之前的排名数加一，可以理解为根据当前的记录数生成序号，后面的记录依此类推。可能我描述的比较苍白，理解起来也比较吃力，我们直接上代码，rank函数的使用方法与row_number函数完全相同。

```
select RANK() OVER(order by [UserId]) as rank,* from [Order] 
```

　　查询结果如下图所示：

　　![使用rank函数排名-晓菜鸟](https://images.cnblogs.com/cnblogs_com/52XF/642690/o_rank_1.png)

　　由上图可以看出，rank函数在进行排名时，同一组的序号是一样的，而后面的则是根据当前的记录数依次类推，图中第一、二条记录的用户Id相同，所以他们的序号是一样的，第三条记录的序号则是3。　　

 

### 三、DENSE_RANK

　　dense_rank函数的功能与rank函数类似，dense_rank函数在生成序号时是连续的，而rank函数生成的序号有可能不连续。dense_rank函数出现相同排名时，将不跳过相同排名号，rank值紧接上一次的rank值。在各个分组内，rank()是跳跃排序，有两个第一名时接下来就是第四名，dense_rank()是连续排序，有两个第一名时仍然跟着第二名。将上面的Sql语句改由dense_rank函数来实现。

```
select DENSE_RANK() OVER(order by [UserId]) as den_rank,* from [Order]
```

　　查询结果如下图所示：

　　![使用dense_rank函数排名-晓菜鸟](https://images.cnblogs.com/cnblogs_com/52XF/642690/o_dense_rank.png)

　　图中第一、二条记录的用户Id相同，所以他们的序号是一样的，第三条记录的序号紧接上一个的序号，所以为2不为3，后面的依此类推。

### 四、NTILE

　　ntile函数可以对序号进行分组处理，将有序分区中的行分发到指定数目的组中。 各个组有编号，编号从一开始。 对于每一个行，ntile 将返回此行所属的组的编号。这就相当于将查询出来的记录集放到指定长度的数组中，每一个数组元素存放一定数量的记录。ntile函数为每条记录生成的序号就是这条记录所有的数组元素的索引（从1开始）。也可以将每一个分配记录的数组元素称为“桶”。ntile函数有一个参数，用来指定桶数。下面的SQL语句使用ntile函数对Order表进行了装桶处理：

```
select NTILE(4) OVER(order by [SubTime] desc) as ntile,* from [Order]
```

　　查询结果如下图所示：

　　![使用ntile排名函数-晓菜鸟](https://images.cnblogs.com/cnblogs_com/52XF/642690/o_ntile.png)

　　Order表的总记录数是6条，而上面的Sql语句ntile函数指定的组数是4，那么Sql Server2005是怎么来决定每一组应该分多少条记录呢？这里我们就需要了解ntile函数的分组依据（约定）。

　　ntile函数的分组依据（约定）：

　　**1、每组的记录数不能大于它上一组的记录数，即编号小的桶放的记录数不能小于编号大的桶。****也就是说，第1组中的记录数只能大于等于第2组及以后各组中的记录数。**

　　**2、所有组中的记录数要么都相同，要么从某一个记录较少的组（命名为X）开始后面所有组的记录数都与该组（X组）的记录数相同。也就是说，如果有个组，前三组的记录数都是9，而第四组的记录数是8，那么第五组和第六组的记录数也必须是8。**

　　这里对约定2进行详细说明一下，以便于更好的理解。

　　首先系统会去检查能不能对所有满足条件的记录进行平均分组，若能则直接平均分配就完成分组了；若不能，则会先分出一个组，这个组分多少条记录呢？就是 （总记录数/总组数）+1 条，之所以分配 （总记录数/总组数）+1 条是因为当不能进行平均分组时，总记录数%总组数肯定是有余的，又因为分组约定1，所以先分出去的组需要+1条。

　　分完之后系统会继续去比较余下的记录数和未分配的组数能不能进行平均分配，若能，则平均分配余下的记录；若不能，则再分出去一组，这个组的记录数也是（总记录数/总组数）+1条。

　　然后系统继续去比较余下的记录数和未分配的组数能不能进行平均分配，若能，则平均分配余下的记录；若还是不能，则再分配出去一组，继续比较余下的......这样一直进行下去，直至分组完成。

　　举个例子，将51条记录分配成5组，51%5==1不能平均分配，则先分出去一组（51/5）+1=11条记录，然后比较余下的 51-11=40 条记录能否平均分配给未分配的4组，能平均分配，则剩下的4组，每组各40/4=10 条记录，分配完成，分配结果为：11，10，10，10，10，[晓菜鸟](http://www.cnblogs.com/52XF/)我开始就错误的以为他会分配成 11，11，11，11，7。

　　根据上面的两个约定，可以得出如下的算法：

 

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
//mod表示取余，div表示取整.
if(记录总数 mod 桶数==0)
{
　　recordCount=记录总数 div 桶数;
　　//将每桶的记录数都设为recordCount.
}
else
{
　　recordCount1=记录总数 div 桶数+1;
　　int n=1;//n表示桶中记录数为recordCount1的最大桶数.
　　m=recordCount1*n;
　　while(((记录总数-m)　mod　(桶数-　n))　!=0)
　　{
　　　　n++;
　　　　m=recordCount1*n;
　　}
　　recordCount2=(记录总数-m) div　(桶数-n);
　　//将前n个桶的记录数设为recordCount1.
　　//将n+1个至后面所有桶的记录数设为recordCount2.
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) NTILE()函数算法实现代码

　　

　　根据上面的算法，如果总记录数为59，总组数为5，则 n=4 , recordCount1=12 , recordCount2=11，分组结果为 ：12，12，12，12，11。

　　如果总记录数为53，总组数为5，则 n=3 , recordCount1=11 , recordCount2=10，分组结果为：11，11，11，10，10。

　　就拿上面的例子来说，总记录数为6，总组数为4，通过算法得到 n=2 , recordCount1=2 , recordCount2=1，分组结果为：2，2，1，1。

 

```
select ntile,COUNT([ID]) recordCount from 
(
    select NTILE(4) OVER(order by [SubTime] desc) as ntile,* from [Order]
) as t
group by t.ntile
```

 

　　运行Sql，分组结果如图：

　　![使用ntilt()函数分组-晓菜鸟](https://images.cnblogs.com/cnblogs_com/52XF/642690/o_ntile_2.png)

　　比对算法与Sql Server的分组结果是一致的，说明算法没错。:)

 

**总结：**

在使用排名函数的时候需要注意以下三点：

　　1、排名函数必须有 OVER 子句。

　　2、排名函数必须有包含 ORDER BY 的 OVER 子句。

　　3、分组内从1开始排序。

 

感谢：

　　在博文的最后我要感谢园友 [海岸线](http://www.cnblogs.com/xhyang110/)，他写的 [SQL2005四个排名函数（row_number、rank、dense_rank和ntile）的比较](http://www.cnblogs.com/xhyang110/archive/2009/10/27/1590448.html) 对我帮助很大，非常感谢！

# 线程池

# 前言 

谈到java的线程池最熟悉的莫过于 `ExecutorService`接口了，jdk1.5新增的`java.util.concurrent`包下的这个api，大大的简化了多线程代码的开发。而不论你用`FixedThreadPool`还是`CachedThreadPool`其背后实现都是`ThreadPoolExecutor`。`ThreadPoolExecutor`是一个典型的缓存池化设计的产物，因为池子有大小，当池子体积不够承载时，就涉及到拒绝策略。JDK中已经预设了4种线程池拒绝策略，下面结合场景详细聊聊这些策略的使用场景，以及我们还能扩展哪些拒绝策略。

# 池化设计思想

池话设计应该不是一个新名词。我们常见的如java线程池、jdbc连接池、redis连接池等就是这类设计的代表实现。这种设计会初始预设资源，解决的问题就是抵消每次获取资源的消耗，如创建线程的开销，获取远程连接的开销等。就好比你去食堂打饭，打饭的大妈会先把饭盛好几份放那里，你来了就直接拿着饭盒加菜即可，不用再临时又盛饭又打菜，效率就高了。除了初始化资源，池化设计还包括如下这些特征：池子的初始值、池子的活跃值、池子的最大值等，这些特征可以直接映射到java线程池和数据库连接池的成员属性中。

# 线程池触发拒绝策略的时机

和数据源连接池不一样，线程池除了初始大小和池子最大值，还多了一个阻塞队列来缓冲。数据源连接池一般请求的连接数超过连接池的最大值的时候就会触发拒绝策略，策略一般是阻塞等待设置的时间或者直接抛异常。而线程池的触发时机如下图：

![图片](https://mmbiz.qpic.cn/mmbiz_png/iaIdQfEric9TwJWhf9QLJ2lGPx63FcQtTJPwT472sNfBOH2DePkvKicLg5H7JibwcV10PsFDlREbq5IndYnaN6nsOw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)img

如图，想要了解线程池什么时候触发拒绝粗略，需要明确上面三个参数的具体含义，是这三个参数总体协调的结果，而不是简单的超过最大线程数就会触发线程拒绝粗略，当提交的任务数大于corePoolSize时，会优先放到队列缓冲区，只有填满了缓冲区后，才会判断当前运行的任务是否大于maxPoolSize，小于时会新建线程处理。大于时就触发了拒绝策略，总结就是：当前提交任务数大于（maxPoolSize + queueCapacity）时就会触发线程池的拒绝策略了。

# JDK内置4种线程池拒绝策略

# 拒绝策略接口定义

在分析JDK自带的线程池拒绝策略前，先看下JDK定义的 拒绝策略接口，如下：

```
publicinterface RejectedExecutionHandler {
    void rejectedExecution(Runnable r, ThreadPoolExecutor executor);
}
```

接口定义很明确，当触发拒绝策略时，线程池会调用你设置的具体的策略，将当前提交的任务以及线程池实例本身传递给你处理，具体作何处理，不同场景会有不同的考虑，下面看JDK为我们内置了哪些实现：

# CallerRunsPolicy（调用者运行策略）

```
publicstaticclass CallerRunsPolicy implements RejectedExecutionHandler {

        public CallerRunsPolicy() { }

        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
            if (!e.isShutdown()) {
                r.run();
            }
        }
    }
```

功能：当触发拒绝策略时，只要线程池没有关闭，就由提交任务的当前线程处理。

使用场景：一般在不允许失败的、对性能要求不高、并发量较小的场景下使用，因为线程池一般情况下不会关闭，也就是提交的任务一定会被运行，但是由于是调用者线程自己执行的，当多次提交任务时，就会阻塞后续任务执行，性能和效率自然就慢了。

# AbortPolicy（中止策略）

```
publicstaticclass AbortPolicy implements RejectedExecutionHandler {

        public AbortPolicy() { }

        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
            thrownew RejectedExecutionException("Task " + r.toString() +
                                                 " rejected from " +
                                                 e.toString());
        }
    }
```

功能：当触发拒绝策略时，直接抛出拒绝执行的异常，中止策略的意思也就是打断当前执行流程

使用场景：这个就没有特殊的场景了，但是一点要正确处理抛出的异常。ThreadPoolExecutor中默认的策略就是AbortPolicy，ExecutorService接口的系列ThreadPoolExecutor因为都没有显示的设置拒绝策略，所以默认的都是这个。但是请注意，ExecutorService中的线程池实例队列都是无界的，也就是说把内存撑爆了都不会触发拒绝策略。当自己自定义线程池实例时，使用这个策略一定要处理好触发策略时抛的异常，因为他会打断当前的执行流程。

# DiscardPolicy（丢弃策略）

```
publicstaticclass DiscardPolicy implements RejectedExecutionHandler {

        public DiscardPolicy() { }

        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
        }
    }
```

功能：如果线程池未关闭，就弹出队列头部的元素，然后尝试执行

使用场景：这个策略还是会丢弃任务，丢弃时也是毫无声息，但是特点是丢弃的是老的未执行的任务，而且是待执行优先级较高的任务。基于这个特性，我能想到的场景就是，发布消息，和修改消息，当消息发布出去后，还未执行，此时更新的消息又来了，这个时候未执行的消息的版本比现在提交的消息版本要低就可以被丢弃了。因为队列中还有可能存在消息版本更低的消息会排队执行，所以在真正处理消息的时候一定要做好消息的版本比较

# 第三方实现的拒绝策略

# dubbo中的线程拒绝策略

```
publicclass AbortPolicyWithReport extends ThreadPoolExecutor.AbortPolicy {

    protectedstaticfinal Logger logger = LoggerFactory.getLogger(AbortPolicyWithReport.class);

    privatefinal String threadName;

    privatefinal URL url;

    privatestaticvolatilelong lastPrintTime = 0;

    privatestatic Semaphore guard = new Semaphore(1);

    public AbortPolicyWithReport(String threadName, URL url) {
        this.threadName = threadName;
        this.url = url;
    }

    @Override
    public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
        String msg = String.format("Thread pool is EXHAUSTED!" +
                        " Thread Name: %s, Pool Size: %d (active: %d, core: %d, max: %d, largest: %d), Task: %d (completed: %d)," +
                        " Executor status:(isShutdown:%s, isTerminated:%s, isTerminating:%s), in %s://%s:%d!",
                threadName, e.getPoolSize(), e.getActiveCount(), e.getCorePoolSize(), e.getMaximumPoolSize(), e.getLargestPoolSize(),
                e.getTaskCount(), e.getCompletedTaskCount(), e.isShutdown(), e.isTerminated(), e.isTerminating(),
                url.getProtocol(), url.getIp(), url.getPort());
        logger.warn(msg);
        dumpJStack();
        thrownew RejectedExecutionException(msg);
    }

    private void dumpJStack() {
       //省略实现
    }
}
```

可以看到，当dubbo的工作线程触发了线程拒绝后，主要做了三个事情，原则就是尽量让使用者清楚触发线程拒绝策略的真实原因

- 输出了一条警告级别的日志，日志内容为线程池的详细设置参数，以及线程池当前的状态，还有当前拒绝任务的一些详细信息。可以说，这条日志，使用dubbo的有过生产运维经验的或多或少是见过的，这个日志简直就是日志打印的典范，其他的日志打印的典范还有spring。得益于这么详细的日志，可以很容易定位到问题所在
- 输出当前线程堆栈详情，这个太有用了，当你通过上面的日志信息还不能定位问题时，案发现场的dump线程上下文信息就是你发现问题的救命稻草。
- 继续抛出拒绝执行异常，使本次任务失败，这个继承了JDK默认拒绝策略的特性

# Netty中的线程池拒绝策略

```
privatestaticfinalclass NewThreadRunsPolicy implements RejectedExecutionHandler {
        NewThreadRunsPolicy() {
            super();
        }

        public void rejectedExecution(Runnable r, ThreadPoolExecutor executor) {
            try {
                final Thread t = new Thread(r, "Temporary task executor");
                t.start();
            } catch (Throwable e) {
                thrownew RejectedExecutionException(
                        "Failed to start a new thread", e);
            }
        }
    }
```

Netty中的实现很像JDK中的CallerRunsPolicy，舍不得丢弃任务。不同的是，CallerRunsPolicy是直接在调用者线程执行的任务。而 Netty是新建了一个线程来处理的。所以，Netty的实现相较于调用者执行策略的使用面就可以扩展到支持高效率高性能的场景了。但是也要注意一点，Netty的实现里，在创建线程时未做任何的判断约束，也就是说只要系统还有资源就会创建新的线程来处理，直到new不出新的线程了，才会抛创建线程失败的异常

# ActiveMq中的线程池拒绝策略

```
new RejectedExecutionHandler() {
                @Override
                public void rejectedExecution(final Runnable r, final ThreadPoolExecutor executor) {
                    try {
                        executor.getQueue().offer(r, 60, TimeUnit.SECONDS);
                    } catch (InterruptedException e) {
                        thrownew RejectedExecutionException("Interrupted waiting for BrokerService.worker");
                    }

                    thrownew RejectedExecutionException("Timed Out while attempting to enqueue Task.");
                }
            });
```

ActiveMq中的策略属于最大努力执行任务型，当触发拒绝策略时，在尝试一分钟的时间重新将任务塞进任务队列，当一分钟超时还没成功时，就抛出异常

# pinpoint中的线程池拒绝策略

```
publicclass RejectedExecutionHandlerChain implements RejectedExecutionHandler {
    privatefinal RejectedExecutionHandler[] handlerChain;

    public static RejectedExecutionHandler build(List<RejectedExecutionHandler> chain) {
        Objects.requireNonNull(chain, "handlerChain must not be null");
        RejectedExecutionHandler[] handlerChain = chain.toArray(new RejectedExecutionHandler[0]);
        returnnew RejectedExecutionHandlerChain(handlerChain);
    }

    private RejectedExecutionHandlerChain(RejectedExecutionHandler[] handlerChain) {
        this.handlerChain = Objects.requireNonNull(handlerChain, "handlerChain must not be null");
    }

    @Override
    public void rejectedExecution(Runnable r, ThreadPoolExecutor executor) {
        for (RejectedExecutionHandler rejectedExecutionHandler : handlerChain) {
            rejectedExecutionHandler.rejectedExecution(r, executor);
        }
    }
}
```

pinpoint的拒绝策略实现很有特点，和其他的实现都不同。他定义了一个拒绝策略链，包装了一个拒绝策略列表，当触发拒绝策略时，会将策略链中的rejectedExecution依次执行一遍

# 结语

前文从线程池设计思想，以及线程池触发拒绝策略的时机引出java线程池拒绝策略接口的定义。并辅以JDK内置4种以及四个第三方开源软件的拒绝策略定义描述了线程池拒绝策略实现的各种思路和使用场景。希望阅读此文后能让你对java线程池拒绝策略有更加深刻的认识，能够根据不同的使用场景更加灵活的应用。

# 线程池知识补充

# **前言**

前几天小强去阿里巴巴面试Java岗，止步于二面。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834329606_C01349705DA3A591200E2F12CF7D7AAE)

 

他和我诉苦自己被虐得多惨多惨，特别是深挖线程和线程池的时候，居然被问到不知道如何作答。

对于他的遭遇，结合他过了一面的那个嘚瑟样，我深表同情（加大力度）！

好了，不开玩笑了，在和小强的面试题中，我选取了几个比较典型的线程和线程池的问题。

> Java中的线程和操作系统的线程有什么关系？ 调用start方法是如何执行run方法的？ 线程池提交任务有哪几种方式？分别有什么区别？ 谈谈你对阻塞队列的理解。 常见的线程池有哪些？为什么阿里不允许使用 Executors 去创建线程池？ 线程池任务调度的流程大致讲一下。 线程池里面的线程执行异常了会怎么样？ 核心线程和非核心线程是如何区分的？

想要答对这些问题，并不是很难，但是想要答好，我觉得是非常考验个人功底的。

为了弄清这些问题，我连夜加急，采访了“线程”，下面是线程的自述。

# **我是谁**

我是一个线程，一个底层的打工人。

总有人把我和进程搞混，但其实我和进程的区别很大。

进程是程序的一次执行，CPU的资源都是分发给进程而不是分发给我们线程，进程是资源分配的最小单位，一个进程可以包含很多向我这样的线程。

我们线程是CPU调度执行的最小单位，真正的打工人。

# **Java中的线程**

在Java里面，我的名字叫做java.lang.Thread。

需要注意的是，调用run方法和执行一个普通方法没有区别。想要真正的创建一个线程并启动，需要调用我的start方法。

有一点我必须告诉你，就是我也是有小弟的。

在JVM里面，我有一个JavaThread的小弟，他帮我联系操作系统的osthread线程。

调用我的start方法之后，具体的执行流程是这样的：

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834329894_4CA2C2D6ACCEC810A92AE071CE601CDB)

 

当然了，这个过程省略了很多细节，不过很明确的是，我和内核线程是一一对应的。

调度我就相当于调度内核线程，而调度内核线程需要在用户态和内核态之间切换，这个过程开销是非常大的。

所以，创建我成本是很高的，一定要慎重。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834330184_6FD42A830AF0C6415F2AC795AA798B90)

 

# **线程池**

和你们人类一样，我也有着精彩的一生，也会经历出生（创建）、奋斗（Running）、死亡（销毁）等过程，今天我主要和你讲述的是我打工奋斗的生活。

原来我是打零工的，有人需要我的时候就创建一个我，等我完成工作就把我销毁。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834330430_4B7DB0AB43829F51936FF53D1D25CE2D)

 

上面也提到过，我和内核线程是一对一的，创建和销毁的过程是非常消耗资源的，所以这样的成本非常高。

于是，有人就想了一个办法，开了一个公司，也就是你们说的线程池。

线程池公司统一管理调度我们线程。我们在线程池里面重复着等待工作——完成工作的步骤。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834330693_1BA531B233EA35B6A084A1ED61AB7437)

 

这样我就可以日复一日年复一年的重复打工了，这种提供了减少对象数量从而改善应用所需的对象结构的方式的模式，被你们人类叫做“享元模式”。

线程池公司有很多种，但都离不开这几个主要指标：

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834330900_32A13A5C5FEECA0D01DF5FCB6B7B7B8C)

 

- corePoolSize：公司正式员工人数。
- maximumPoolSize：正式工+临时工最大数量。
- keepAliveTime：临时工多久没做事情会被开除。
- unit：临时工没做事情会被开除的时间单位。
- workQueue：公司业务接收部门。
- threadFactory：行政部，负责招聘培训员工的。
- handler：业务部接收业务到达上限了的处理方式。

# **阻塞队列**

线程池中的workQueue是一个阻塞队列，用于存放线程池未能及时处理执行的任务。

它的存在既解决了任务的提交与执行，又能起到一个缓冲的作用。

阻塞队列有很多，下面我带你了解一下常见的阻塞队列。

# **ArrayBlockingQueue**

基于数组实现的有界阻塞队列，创建的时候需要指定容量。此类型的队列按照FIFO（先进先出）的规则对元素进行排序。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834331275_1AE1E0EF02F1D828680E8B1399F52332)

 

# **LinkedBlockingQueue**

基于链表实现阻塞队列，默认大小为Integer.MAX_VALUE。按照FIFO（先进先出）的规则对元素进行排序

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834331462_56753BE85EF2C796E1E6E1F29C80B98E)

 

# **SynchronousQueue**

一个不存储元素的阻塞队列。每一个put操作必须阻塞等待其他线程的take操作，take操作也必须等待其他线程的put操作。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834332097_082EF5F606CBA2039336B1AE59F0EDC8)

 

# **PriorityBlockingQueue**

一个基于数组利用堆结构实现优先级效果的无界队列，默认自然序排序，也可以自己实现compareTo方法自定义排序规则。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834332571_5F2CD93D860B9DD02A68A3E0435C11F9)

 

# **DelayedWorkQueue**

一个实现了优先级队列功能且实现了延迟获取的无界队列，在创建元素时，可以指定多久多久才能在队列中获取当前元素。只有延时期满了后才能从队列中获取元素。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834332890_7628077DED6B5CF1BA2525A4B085530D)

 

# **拒绝策略**

当任务队列满了之后，如果还有任务提交过来，会触发拒绝策略，常见的拒绝策略有：

- AbortPolicy：丢弃任务并抛出异常，默认该方式。
- CallerRunsPolicy：由调用线程自己处理该任务。谁调用，谁处理。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834333145_0DC476A393A43FE5ED32BA75D6387A09)

 

- DiscardPolicy：丢弃任务，但是不抛出异常。
- DiscardOldestPolicy：抛弃任务队列中最旧的任务也就是最先加入队列的，再把这个新任务添加进去。先从任务队列中弹出最先加入的任务，空出一个位置，然后再次执行execute方法把任务加入队列。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834333385_59E7E31EA843FB74025CACABE112A55E)

 

当然，除了以上这几种拒绝策略，你也可以根据实际的业务场景和业务需求去自定义拒绝策略，只需要实现RejectedExecutionHander接口，自定义里面的rejectedExecution方法。

# **运行流程**

我们每个线程会被包装成Worker，线程池里面有一个HashSet存放Worker。

当有任务提交过来之后：

1. 首先检测线程池运行状态，如果不是RUNNING，则直接拒绝，线程池要保证在RUNNING的状态下执行任务。
2. 如果线程池中Worker的数量小于核心线程数，就会去创建一个新的线程，也就是招聘一个正式工让他执行任务。
3. 如果Worker的数量大于或者等于核心线程数，就会把任务放到阻塞任务队列里面。
4. 如果任务队列满了还有任务过来，如果临时工名额没有满（workerCount < maximumPoolSize），就去招聘临时工让临时工执行任务。如果临时工名额都满了，触发任务拒绝策略。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834333652_4D110650026EBDD89E6320C8C3F865D4)

 

总结而言，就是核心线程能干的事情尽量不去创建非核心线程，这是线程池很关键的一点。

```
new` `ThreadPoolExecutor(``4``, ``8``, 0L, TimeUnit.MILLISECONDS, ``new` `ArrayBlockingQueue<Runnable>(``4``));
```

以这个线程池为例，下面是他的任务提交和执行流程：

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834333962_E973222A750FC9F2A1DC34874A9974E9)

 

# **有哪些线程池**

我有过四段工作经历，每段经历都有着精彩的故事。

# **SingleThreadExecutor**

SingleThreadExecutor是我加入的第一家线程池，这是一家创业公司，整个线程池就只有我一个线程。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834334631_4F42152A54EEABFF29F5A5925636EEC5)

 

所有的任务都由我干，而且任务队列是一个无界队列。就是说，打工的线程只有我一个，但是需求任务可以是无限多。

在需求任务很多的时候，经常出现任务处理不过来的情况，导致任务堆积，出现OOM。

但因为所有的活都是我干，没有繁琐的沟通成本，不需要处理线程同步的问题，这算是这种线程池的一个优点吧。

这种线程池适用于并发量不大且需要任务顺序执行的场景。

# **FixedThreadPool**

后来公司倒闭了，我又加入了一个叫FixedThreadPool的线程池。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834335277_D59A8AD5ACB15C7DE67A433830375423)

 

FixedThreadPool和SingleThreadExecutor唯一不同的地方就是核心线程的数量，FixedThreadPool可以招收很多的打工线程。

在这里，我不再是孤军奋斗了，我有了一群共同打拼的小伙伴，大家一起完成任务，一起承担压力。

可这种线程池还是存在一个问题——任务队列是无界的，需求任务过多的话，还是会造成OOM。

这种线程池线程数固定，且不被回收，线程与线程池的生命周期同步的线程池，适用于任务量比较固定但耗时长的任务。

# **CachedThreadPool**

后来，为了离家更近，我离职了。加入了一家叫CachedThreadPool的线程池，进去之后，却发现这是一家外包公司。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834335830_110732630DBB29A62A3DA580987CED81)

 

这种线程池里面没有一个核心线程（正式工），一有需求就去招聘一个非核心线程（临时工）。

如果一个线程任务干完了之后，60秒之后没有新的任务就会被辞退。

这种线程池的任务队列采用的是SynchronousQueue，这个队列是无法插入任务的，一有任务就创建一个线程执行，如果并发高且任务耗时长，创建太多线程也是可能导致OOM的。所以CachedThreadPool比较适合任务量大但耗时少的任务。

# **ScheduleThreadPool**

经历了外面的风风雨雨，我觉得还是找份固定的工作比较可靠，于是我加入了一家叫做ScheduleThreadPool的国企。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834336090_5EFF88A5F3DA71797FD60123B2B6A4F8)

 

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834336509_B6ADF38B14BE9CD7F65CA076CE325F75)

 

在这里，工作比较的轻松，多数情况下，我只需要在固定的时间干固定的活。

任务忙不过来的时候，公司也会招聘一些临时工帮忙处理，临时工干完活就会被辞退。

综合来说，这类线程池适用于执行定时任务和具体固定周期的重复任务。由于采用的任务队列是DelayedWorkQueue无界队列，所以也是有OOM的风险的。

# **总结**

好了，关于线程的故事就告一段落了。关于线程池的应用实践，我们下次再聊。

文章开头的面试题在大部分在文中都能找到答案，对于没有提到的，这里做一个补充：

# **1. 线程池提交任务有哪几种方式？分别有什么区别？**

有execute和submit两种方式

- execute只能提交Runnable类型的任务，无返回值。submit既可以提交Runnable类型的任务，也可以提交Callable类型的任务，会有一个类型为Future的返回值，但当任务类型为Runnable时，返回值为null。
- execute在执行任务时，如果遇到异常会直接抛出，而submit不会直接抛出，只有在使用Future的get方法获取返回值时，才会抛出异常。

# **2. 线程池里面的线程执行异常了会怎么样？**

如果一个线程执行任务的过程中出现异常，那么这个线程对应的Worker会被移出线程池，该线程也会被销毁回收。

同时会通过指定的线程工厂创建一个线程，并封装成Worker放入线程池代替移除的Worker。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834336779_29E38252F39C493DF5BCC0D70C401BD3)

 

# **3. 核心线程能被回收吗？**

核心线程默认不会被回收。但是可以调用allowCoreThreadTimeOut让核心线程可以被回收。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834337034_61426E695C93953A64C5378FC6A85D61)

 

需要注意的是，调用这个方法的线程池必须将keepAliveTime设置为大于0，否则会抛出异常。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834337274_3D1AC650858ACB873FD8DC41CE09E059)

 

# **4. 核心线程和非核心线程是如何区分的？**

核心线程和非核心线程是一个抽象概念，只是用于更好的表述线程池的运行逻辑，实际上都对应操作系统的osThread，都是重量级线程。

在新增Worker的时候，通过一个boolean表达是核心线程还是非核心线程，本质上两者没有什么不同。

![img](https://uploadfiles.nowcoder.com/images/20210408/858838406_1617834337522_C3B1CFD3D8CDAB880D56364F8563EEBD)

 

# **5. 为什么阿里不允许使用 Executors 去创建线程池？**

FixedThreadPool 和 SingleThreadPool：允许的请求队列长度为 Integer.MAX_VALUE，可能会堆积大量的请求，从而导致 OOM。

CachedThreadPool：允许的创建线程数量为 Integer.MAX_VALUE，可能会创建大量的线程，从而导致 OOM。

总结来说就是，使用Executors创建线程池会容易忽视线程池的一些属性，使用不当容易引起资源耗尽。

# **写在最后**

> 这个世界上或许没有线程，又或许人人都是线程。 无畏年少青春，迎风潇洒前行，做一个努力奋斗的线程，希望他日回首望去，不是一片黑暗，而是漫天星光。

好了，今天的文章就到这里了。

最后，感谢你的阅读！

> 原文链接：https://www.cnblogs.com/coderw/p/14358493.html

# redis面试题

### redis 和 memcached 有啥区别？

#### redis 支持复杂的数据结构

redis 相比 memcached 来说，拥有更多的数据结构，能支持更丰富的数据操作。如果需要缓存能够支持更复杂的结构和操作， redis 会是不错的选择。

#### redis 原生支持集群模式

在 redis3.x 版本中，便能支持 cluster 模式，而 memcached 没有原生的集群模式，需要依靠客户端来实现往集群中分片写入数据。

#### 性能对比

由于 redis 只使用单核，而 memcached 可以使用多核，所以平均每一个核上 redis 在存储小数据时比 memcached 性能更高。而在 100k 以上的数据中，memcached 性能要高于 redis，虽然 redis 最近也在存储大数据的性能上进行优化，但是比起 memcached，还是稍有逊色。

### redis 的线程模型

redis 内部使用文件事件处理器 `file event handler`，这个文件事件处理器是单线程的，所以 redis 才叫做单线程的模型。它采用 IO 多路复用机制同时监听多个 socket，根据 socket 上的事件来选择对应的事件处理器进行处理。

文件事件处理器的结构包含 4 个部分：

- 多个 socket
- IO 多路复用程序
- 文件事件分派器
- 事件处理器（连接应答处理器、命令请求处理器、命令回复处理器）

多个 socket 可能会并发产生不同的操作，每个操作对应不同的文件事件，但是 IO 多路复用程序会监听多个 socket，会将 socket 产生的事件放入队列中排队，事件分派器每次从队列中取出一个事件，把该事件交给对应的事件处理器进行处理。

来看客户端与 redis 的一次通信过程：

![redis-single-thread-model](https://www.javazhiyin.com/wp-content/uploads/2018/12/redis-single-thread-model.png)

客户端 socket01 向 redis 的 server socket 请求建立连接，此时 server socket 会产生一个 `AE_READABLE` 事件，IO 多路复用程序监听到 server socket 产生的事件后，将该事件压入队列中。文件事件分派器从队列中获取该事件，交给`连接应答处理器`。连接应答处理器会创建一个能与客户端通信的 socket01，并将该 socket01 的 `AE_READABLE` 事件与命令请求处理器关联。

假设此时客户端发送了一个 `set key value` 请求，此时 redis 中的 socket01 会产生 `AE_READABLE` 事件，IO 多路复用程序将事件压入队列，此时事件分派器从队列中获取到该事件，由于前面 socket01 的 `AE_READABLE` 事件已经与命令请求处理器关联，因此事件分派器将事件交给命令请求处理器来处理。命令请求处理器读取 socket01 的 `key value` 并在自己内存中完成 `key value` 的设置。操作完成后，它会将 socket01 的 `AE_WRITABLE` 事件与命令回复处理器关联。

如果此时客户端准备好接收返回结果了，那么 redis 中的 socket01 会产生一个 `AE_WRITABLE` 事件，同样压入队列中，事件分派器找到相关联的命令回复处理器，由命令回复处理器对 socket01 输入本次操作的一个结果，比如 `ok`，之后解除 socket01 的 `AE_WRITABLE` 事件与命令回复处理器的关联。

这样便完成了一次通信。

### 为啥 redis 单线程模型也能效率这么高？

- 纯内存操作
- 核心是基于非阻塞的 IO 多路复用机制
- 单线程反而避免了多线程的频繁上下文切换问题

# Redis中的事务

[MULTI](http://www.redis.cn/commands/multi.html) 、 [EXEC](http://www.redis.cn/commands/exec.html) 、 [DISCARD](http://www.redis.cn/commands/discard.html) 和 [WATCH](http://www.redis.cn/commands/watch.html) 是 Redis 事务相关的命令。事务可以一次执行多个命令， 并且带有以下两个重要的保证：

- 事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。
- 事务是一个原子操作：事务中的命令要么全部被执行，要么全部都不执行。

[EXEC](http://www.redis.cn/commands/exec.html) 命令负责触发并执行事务中的所有命令：

- 如果客户端在使用 [MULTI](http://www.redis.cn/commands/multi.html) 开启了一个事务之后，却因为断线而没有成功执行 [EXEC](http://www.redis.cn/commands/exec.html) ，那么事务中的所有命令都不会被执行。
- 另一方面，如果客户端成功在开启事务之后执行 [EXEC](http://www.redis.cn/commands/exec.html) ，那么事务中的所有命令都会被执行。

当使用 AOF 方式做持久化的时候， Redis 会使用单个 write(2) 命令将事务写入到磁盘中。

然而，如果 Redis 服务器因为某些原因被管理员杀死，或者遇上某种硬件故障，那么可能只有部分事务命令会被成功写入到磁盘中。

如果 Redis 在重新启动时发现 AOF 文件出了这样的问题，那么它会退出，并汇报一个错误。

使用`redis-check-aof`程序可以修复这一问题：它会移除 AOF 文件中不完整事务的信息，确保服务器可以顺利启动。

从 2.2 版本开始，Redis 还可以通过乐观锁（optimistic lock）实现 CAS （check-and-set）操作，具体信息请参考文档的后半部分。

## 用法

[MULTI](http://www.redis.cn/commands/multi.html) 命令用于开启一个事务，它总是返回 `OK` 。 [MULTI](http://www.redis.cn/commands/multi.html) 执行之后， 客户端可以继续向服务器发送任意多条命令， 这些命令不会立即被执行， 而是被放到一个队列中， 当 [EXEC](http://www.redis.cn/commands/exec.html)命令被调用时， 所有队列中的命令才会被执行。

另一方面， 通过调用 [DISCARD](http://www.redis.cn/commands/discard.html) ， 客户端可以清空事务队列， 并放弃执行事务。

以下是一个事务例子， 它原子地增加了 `foo` 和 `bar` 两个键的值：

```
> MULTI
OK
> INCR foo
QUEUED
> INCR bar
QUEUED
> EXEC
1) (integer) 1
2) (integer) 1
```

[EXEC](http://www.redis.cn/commands/exec.html) 命令的回复是一个数组， 数组中的每个元素都是执行事务中的命令所产生的回复。 其中， 回复元素的先后顺序和命令发送的先后顺序一致。

当客户端处于事务状态时， 所有传入的命令都会返回一个内容为 `QUEUED` 的状态回复（status reply）， 这些被入队的命令将在 EXEC 命令被调用时执行。

## 事务中的错误

使用事务时可能会遇上以下两种错误：

- 事务在执行 [EXEC](http://www.redis.cn/commands/exec.html) 之前，入队的命令可能会出错。比如说，命令可能会产生语法错误（参数数量错误，参数名错误，等等），或者其他更严重的错误，比如内存不足（如果服务器使用 `maxmemory` 设置了最大内存限制的话）。
- 命令可能在 [EXEC](http://www.redis.cn/commands/exec.html) 调用之后失败。举个例子，事务中的命令可能处理了错误类型的键，比如将列表命令用在了字符串键上面，诸如此类。

对于发生在 [EXEC](http://www.redis.cn/commands/exec.html) 执行之前的错误，客户端以前的做法是检查命令入队所得的返回值：如果命令入队时返回 `QUEUED` ，那么入队成功；否则，就是入队失败。如果有命令在入队时失败，那么大部分客户端都会停止并取消这个事务。

不过，从 Redis 2.6.5 开始，服务器会对命令入队失败的情况进行记录，并在客户端调用 [EXEC](http://www.redis.cn/commands/exec.html) 命令时，拒绝执行并自动放弃这个事务。

在 Redis 2.6.5 以前， Redis 只执行事务中那些入队成功的命令，而忽略那些入队失败的命令。 而新的处理方式则使得在流水线（pipeline）中包含事务变得简单，因为发送事务和读取事务的回复都只需要和服务器进行一次通讯。

至于那些在 [EXEC](http://www.redis.cn/commands/exec.html) 命令执行之后所产生的错误， 并没有对它们进行特别处理： 即使事务中有某个/某些命令在执行时产生了错误， 事务中的其他命令仍然会继续执行。

从协议的角度来看这个问题，会更容易理解一些。 以下例子中， [LPOP](http://www.redis.cn/commands/lpop.html) 命令的执行将出错， 尽管调用它的语法是正确的：

```
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
MULTI
+OK
SET a 3
abc
+QUEUED
LPOP a
+QUEUED
EXEC
*2
+OK
-ERR Operation against a key holding the wrong kind of value
```

[EXEC](http://www.redis.cn/commands/exec.html) 返回两条[bulk-string-reply](http://www.redis.cn/topics/protocol.html#bulk-string-reply)： 第一条是 `OK` ，而第二条是 `-ERR` 。 至于怎样用合适的方法来表示事务中的错误， 则是由客户端自己决定的。

最重要的是记住这样一条， 即使事务中有某条/某些命令执行失败了， 事务队列中的其他命令仍然会继续执行 —— Redis 不会停止执行事务中的命令。

以下例子展示的是另一种情况， 当命令在入队时产生错误， 错误会立即被返回给客户端：

```
MULTI
+OK
INCR a b c
-ERR wrong number of arguments for 'incr' command
```

因为调用 [INCR](http://www.redis.cn/commands/incr.html) 命令的参数格式不正确， 所以这个 [INCR](http://www.redis.cn/commands/incr.html) 命令入队失败。

## 为什么 Redis 不支持回滚（roll back）

如果你有使用关系式数据库的经验， 那么 “Redis 在事务失败时不进行回滚，而是继续执行余下的命令”这种做法可能会让你觉得有点奇怪。

以下是这种做法的优点：

- Redis 命令只会因为错误的语法而失败（并且这些问题不能在入队时发现），或是命令用在了错误类型的键上面：这也就是说，从实用性的角度来说，失败的命令是由编程错误造成的，而这些错误应该在开发的过程中被发现，而不应该出现在生产环境中。
- 因为不需要对回滚进行支持，所以 Redis 的内部可以保持简单且快速。

有种观点认为 Redis 处理事务的做法会产生 bug ， 然而需要注意的是， 在通常情况下， 回滚并不能解决编程错误带来的问题。 举个例子， 如果你本来想通过 [INCR](http://www.redis.cn/commands/incr.html) 命令将键的值加上 1 ， 却不小心加上了 2 ， 又或者对错误类型的键执行了 [INCR](http://www.redis.cn/commands/incr.html) ， 回滚是没有办法处理这些情况的。

## 放弃事务

当执行 [DISCARD](http://www.redis.cn/commands/discard.html) 命令时， 事务会被放弃， 事务队列会被清空， 并且客户端会从事务状态中退出：

```
> SET foo 1
OK
> MULTI
OK
> INCR foo
QUEUED
> DISCARD
OK
> GET foo
"1"
```

## 使用 check-and-set 操作实现乐观锁

[WATCH](http://www.redis.cn/commands/watch.html) 命令可以为 Redis 事务提供 check-and-set （CAS）行为。

被 [WATCH](http://www.redis.cn/commands/watch.html) 的键会被监视，并会发觉这些键是否被改动过了。 如果有至少一个被监视的键在 [EXEC](http://www.redis.cn/commands/exec.html) 执行之前被修改了， 那么整个事务都会被取消， [EXEC](http://www.redis.cn/commands/exec.html) 返回[nil-reply](http://www.redis.cn/topics/protocol.html#nil-reply)来表示事务已经失败。

举个例子， 假设我们需要原子性地为某个值进行增 1 操作（假设 [INCR](http://www.redis.cn/commands/incr.html) 不存在）。

首先我们可能会这样做：

```
val = GET mykey
val = val + 1
SET mykey $val
```

上面的这个实现在只有一个客户端的时候可以执行得很好。 但是， 当多个客户端同时对同一个键进行这样的操作时， 就会产生竞争条件。举个例子， 如果客户端 A 和 B 都读取了键原来的值， 比如 10 ， 那么两个客户端都会将键的值设为 11 ， 但正确的结果应该是 12 才对。

有了 [WATCH](http://www.redis.cn/commands/watch.html) ， 我们就可以轻松地解决这类问题了：

```
WATCH mykey
val = GET mykey
val = val + 1
MULTI
SET mykey $val
EXEC
```

使用上面的代码， 如果在 [WATCH](http://www.redis.cn/commands/watch.html) 执行之后， [EXEC](http://www.redis.cn/commands/exec.html) 执行之前， 有其他客户端修改了 `mykey` 的值， 那么当前客户端的事务就会失败。 程序需要做的， 就是不断重试这个操作， 直到没有发生碰撞为止。

这种形式的锁被称作乐观锁， 它是一种非常强大的锁机制。 并且因为大多数情况下， 不同的客户端会访问不同的键， 碰撞的情况一般都很少， 所以通常并不需要进行重试。

## 了解 `WATCH`

[WATCH](http://www.redis.cn/commands/watch.html) 使得 [EXEC](http://www.redis.cn/commands/exec.html) 命令需要有条件地执行： 事务只能在所有被监视键都没有被修改的前提下执行， 如果这个前提不能满足的话，事务就不会被执行。 [了解更多->](http://code.google.com/p/redis/issues/detail?id=270)

[WATCH](http://www.redis.cn/commands/watch.html) 命令可以被调用多次。 对键的监视从 [WATCH](http://www.redis.cn/commands/watch.html) 执行之后开始生效， 直到调用 [EXEC](http://www.redis.cn/commands/exec.html) 为止。

用户还可以在单个 [WATCH](http://www.redis.cn/commands/watch.html) 命令中监视任意多个键， 就像这样：

```
redis> WATCH key1 key2 key3
OK
```

当 [EXEC](http://www.redis.cn/commands/exec.html) 被调用时， 不管事务是否成功执行， 对所有键的监视都会被取消。

另外， 当客户端断开连接时， 该客户端对键的监视也会被取消。

使用无参数的 [UNWATCH](http://www.redis.cn/commands/unwatch.html) 命令可以手动取消对所有键的监视。 对于一些需要改动多个键的事务， 有时候程序需要同时对多个键进行加锁， 然后检查这些键的当前值是否符合程序的要求。 当值达不到要求时， 就可以使用 [UNWATCH](http://www.redis.cn/commands/unwatch.html) 命令来取消目前对键的监视， 中途放弃这个事务， 并等待事务的下次尝试。

### 使用 WATCH 实现 ZPOP

[WATCH](http://www.redis.cn/commands/watch.html) 可以用于创建 Redis 没有内置的原子操作。举个例子， 以下代码实现了原创的 [ZPOP](http://www.redis.cn/commands/zpop.html) 命令， 它可以原子地弹出有序集合中分值（score）最小的元素：

```
WATCH zset
element = ZRANGE zset 0 0
MULTI
ZREM zset element
EXEC
```

程序只要重复执行这段代码， 直到 [EXEC](http://www.redis.cn/commands/exec.html) 的返回值不是[nil-reply](http://www.redis.cn/topics/protocol.html#nil-reply)回复即可。

## Redis 脚本和事务

从定义上来说， Redis 中的脚本本身就是一种事务， 所以任何在事务里可以完成的事， 在脚本里面也能完成。 并且一般来说， 使用脚本要来得更简单，并且速度更快。

因为脚本功能是 Redis 2.6 才引入的， 而事务功能则更早之前就存在了， 所以 Redis 才会同时存在两种处理事务的方法。

不过我们并不打算在短时间内就移除事务功能， 因为事务提供了一种即使不使用脚本， 也可以避免竞争条件的方法， 而且事务本身的实现并不复杂。

不过在不远的将来， 可能所有用户都会只使用脚本来实现事务也说不定。 如果真的发生这种情况的话， 那么我们将废弃并最终移除事务功能。

# Redis中的分布式锁

## 1 背景



我们日常在电商网站购物时经常会遇到一些高并发的场景，例如电商 App 上经常出现的秒杀活动、限量优惠券抢购，还有我们去哪儿网的火车票抢票系统等，这些场景有一个共同特点就是访问量激增，虽然在系统设计时会通过限流、异步、排队等方式优化，但整体的并发还是平时的数倍以上，为了避免并发问题，防止库存超卖，给用户提供一个良好的购物体验，这些系统中都会用到锁的机制。



对于单进程的并发场景，可以使用编程语言及相应的类库提供的锁，如 Java 中的 synchronized 语法以及 ReentrantLock 类等，避免并发问题。



![img](https://static001.infoq.cn/resource/image/e8/52/e8cab0d04fyya53f65127de9fa178e52.png)



如果在分布式场景中，实现不同客户端的线程对代码和资源的同步访问，保证在多线程下处理共享数据的安全性，就需要用到分布式锁技术。



![img](https://static001.infoq.cn/resource/image/f1/40/f161d9e44f7eea10bdeb512c1b127640.png)



那么何为分布式锁呢？分布式锁是控制分布式系统或不同系统之间共同访问共享资源的一种锁实现，如果不同的系统或同一个系统的不同主机之间共享了某个资源时，往往需要互斥来防止彼此干扰保证一致性。



一个相对安全的分布式锁，一般需要具备以下特征：



- 互斥性。互斥是锁的基本特征，同一时刻锁只能被一个线程持有，执行临界区操作。
- 超时释放。通过超时释放，可以避免死锁，防止不必要的线程等待和资源浪费，类似于 MySQL 的 InnoDB 引擎中的 innodblockwait_timeout 参数配置。
- 可重入性。一个线程在持有锁的情况可以对其再次请求加锁，防止锁在线程执行完临界区操作之前释放。
- 高性能和高可用。加锁和释放锁的过程性能开销要尽可能的低，同时也要保证高可用，防止分布式锁意外失效。



可以看出实现分布式锁，并不是锁住资源就可以了，还需要满足一些额外的特征，避免出现死锁、锁失效等问题。



## 2 分布式锁的实现方式



目前实现分布式锁的方式有很多，常见的主要有：



- **Memcached 分布式锁**
- 利用 Memcached 的 add 命令。此命令是原子性操作，只有在 key 不存在的情况下，才能 add 成功，也就意味着线程得到了锁。
- **Zookeeper 分布式锁**
- 利用 Zookeeper 的顺序临时节点，来实现分布式锁和等待队列。ZooKeeper 作为一个专门为分布式应用提供方案的框架，它提供了一些非常好的特性，如 ephemeral 类型的 znode 自动删除的功能，同时 ZooKeeper 还提供 watch 机制，可以让分布式锁在客户端用起来就像一个本地的锁一样：加锁失败就阻塞住，直到获取到锁为止。
- **Chubby**
- Google 公司实现的粗粒度分布式锁服务，有点类似于 ZooKeeper，但也存在很多差异。Chubby 通过 sequencer 机制解决了请求延迟造成的锁失效的问题。
- **Redis 分布式锁**
- 基于 Redis 单机实现的分布式锁，其方式和 Memcached 的实现方式类似，利用 Redis 的 SETNX 命令，此命令同样是原子性操作，只有在 key 不存在的情况下，才能 set 成功。而基于 Redis 多机实现的分布式锁 Redlock，是 Redis 的作者 antirez 为了规范 Redis 分布式锁的实现，提出的一个更安全有效的实现机制。



本文主要讨论分析基于 Redis 的分布式锁的几种实现方式以及存在的问题。



## 3 Redis 分布式锁



使用 Redis 作为分布式锁，本质上要实现的目标就是一个进程在 Redis 里面占据了仅有的一个“茅坑”，当别的进程也想来占坑时，发现已经有人蹲在那里了，就只好放弃或者等待稍后再试。



目前基于 Redis 实现分布式锁主要有两大类，一类是基于单机，另一类是基于 Redis 多机，不管是哪种实现方式，均需要实现加锁、解锁、锁超时这三个分布式锁的核心要素。



### 3.1 基于 Redis 单机实现的分布式锁



**3.1.1 使用 SETNX 指令**



最简单的加锁方式就是直接使用 Redis 的 SETNX 指令，该指令只在 key 不存在的情况下，将 key 的值设置为 value，若 key 已经存在，则 SETNX 命令不做任何动作。key 是锁的唯一标识，可以按照业务需要锁定的资源来命名。



比如在某商城的秒杀活动中对某一商品加锁，那么 key 可以设置为  lock_resource_id ，value 可以设置为任意值，在资源使用完成后，使用 DEL 删除该 key 对锁进行释放，整个过程如下：



![img](https://static001.infoq.cn/resource/image/e6/6c/e64ce358de2f0bdab6e1dee4d2a5b16c.png)



很显然，这种获取锁的方式很简单，但也存在一个问题，就是我们上面提到的分布式锁三个核心要素之一的锁超时问题，即如果获得锁的进程在业务逻辑处理过程中出现了异常，可能会导致 DEL 指令一直无法执行，导致锁无法释放，该资源将会永远被锁住。



![img](https://static001.infoq.cn/resource/image/33/1e/33d85bcdcf1c3858ff918134ae07e61e.png)



所以，在使用 SETNX 拿到锁以后，必须给 key 设置一个过期时间，以保证即使没有被显式释放，在获取锁达到一定时间后也要自动释放，防止资源被长时间独占。由于 SETNX 不支持设置过期时间，所以需要额外的 EXPIRE 指令，整个过程如下：



![img](https://static001.infoq.cn/resource/image/31/6b/3183a3c30af87a67999097795256aa6b.png)



这样实现的分布式锁仍然存在一个严重的问题，由于 SETNX 和 EXPIRE 这两个操作是非原子性的， 如果进程在执行 SETNX 和 EXPIRE 之间发生异常，SETNX 执行成功，但 EXPIRE 没有执行，导致这把锁变得“长生不老”，这种情况就可能出现前文提到的锁超时问题，其他进程无法正常获取锁。



![img](https://static001.infoq.cn/resource/image/8c/c3/8c5cda6ae509c4a2d77bfcb9077a74c3.png)



**3.1.2 使用 SET 扩展指令**



为了解决 SETNX 和 EXPIRE 两个操作非原子性的问题，可以使用 Redis 的 SET 指令的扩展参数，使得 SETNX 和 EXPIRE 这两个操作可以原子执行，整个过程如下：



![img](https://static001.infoq.cn/resource/image/9d/f7/9dd36143ac259429f9e6ef0ebebaf9f7.png)



在这个 SET 指令中：



- NX 表示只有当 lock_resource_id 对应的 key 值不存在的时候才能 SET 成功。保证了只有第一个请求的客户端才能获得锁，而其它客户端在锁被释放之前都无法获得锁。
- EX 10 表示这个锁 10 秒钟后会自动过期，业务可以根据实际情况设置这个时间的大小。



但是这种方式仍然不能彻底解决分布式锁超时问题：



- 锁被提前释放。假如线程 A 在加锁和释放锁之间的逻辑执行的时间过长（或者线程 A 执行过程中被堵塞），以至于超出了锁的过期时间后进行了释放，但线程 A 在临界区的逻辑还没有执行完，那么这时候线程 B 就可以提前重新获取这把锁，导致临界区代码不能严格的串行执行。
- 锁被误删。假如以上情形中的线程 A 执行完后，它并不知道此时的锁持有者是线程 B，线程 A 会继续执行 DEL 指令来释放锁，如果线程 B 在临界区的逻辑还没有执行完，线程 A 实际上释放了线程 B 的锁。



为了避免以上情况，建议不要在执行时间过长的场景中使用 Redis 分布式锁，同时一个比较安全的做法是在执行 DEL 释放锁之前对锁进行判断，验证当前锁的持有者是否是自己。



具体实现就是在加锁时将 value 设置为一个唯一的随机数（或者线程 ID ），释放锁时先判断随机数是否一致，然后再执行释放操作，确保不会错误地释放其它线程持有的锁，除非是锁过期了被服务器自动释放，整个过程如下：



![img](https://static001.infoq.cn/resource/image/2d/88/2dfe6d82fefe6a5d0c2e5d98492b7888.png)



但判断 value 和删除 key 是两个独立的操作，并不是原子性的，所以这个地方需要使用 Lua 脚本进行处理，因为 Lua 脚本可以保证连续多个指令的原子性执行。



![img](https://static001.infoq.cn/resource/image/ec/5e/ecd616659beb01457a033c61a0f5b65e.png)



![img](https://static001.infoq.cn/resource/image/66/d6/660c1c540c835f6c8c6e77601db894d6.png)



基于 Redis 单节点的分布式锁基本完成了，但是这并不是一个完美的方案，只是相对完全一点，因为它并没有完全解决当前线程执行超时锁被提前释放后，其它线程乘虚而入的问题。



**3.1.3 使用 Redisson 的分布式锁**



怎么能解决锁被提前释放这个问题呢？



可以利用锁的可重入特性，让获得锁的线程开启一个定时器的守护线程，每 expireTime/3 执行一次，去检查该线程的锁是否存在，如果存在则对锁的过期时间重新设置为 expireTime，即利用守护线程对锁进行“续命”，防止锁由于过期提前释放。



当然业务要实现这个守护进程的逻辑还是比较复杂的，可能还会出现一些未知的问题。



目前互联网公司在生产环境用的比较广泛的开源框架 Redisson 很好地解决了这个问题，非常的简便易用，且支持 Redis 单实例、Redis M-S、Redis Sentinel、Redis Cluster 等多种部署架构。



感兴趣的朋友可以查阅下官方文档或者源码：[*https://github.com/redisson/redisson/wiki*](https://github.com/redisson/redisson/wiki)



其实现原理如图所示（图中以 Redis 集群为例）：



![img](https://static001.infoq.cn/resource/image/15/87/1522bfd21c92cddc8072934f336e0787.png)



### 3.2 基于 Redis 多机实现的分布式锁 Redlock



以上几种基于 Redis 单机实现的分布式锁其实都存在一个问题，就是加锁时只作用在一个 Redis 节点上，即使 Redis 通过 Sentinel 保证了高可用，但由于 Redis 的复制是异步的，Master 节点获取到锁后在未完成数据同步的情况下发生故障转移，此时其他客户端上的线程依然可以获取到锁，因此会丧失锁的安全性。



整个过程如下：



- 客户端 A 从 Master 节点获取锁。
- Master 节点出现故障，主从复制过程中，锁对应的 key 没有同步到 Slave 节点。
- Slave 升 级为 Master 节点，但此时的 Master 中没有锁数据。
- 客户端 B 请求新的 Master 节点，并获取到了对应同一个资源的锁。
- 出现多个客户端同时持有同一个资源的锁，不满足锁的互斥性。



正因为如此，在 Redis 的分布式环境中，Redis 的作者 antirez 提供了 RedLock 的算法来实现一个分布式锁，该算法大概是这样的：



假设有 N（N>=5）个 Redis 节点，这些节点完全互相独立，不存在主从复制或者其他集群协调机制，确保在这 N 个节点上使用与在 Redis 单实例下相同的方法获取和释放锁。



获取锁的过程，客户端应执行如下操作：



- 获取当前 Unix 时间，以毫秒为单位。
- 按顺序依次尝试从 5 个实例使用相同的 key 和具有唯一性的 value（例如 UUID）获取锁。当向 Redis 请求获取锁时，客户端应该设置一个网络连接和响应超时时间，这个超时时间应该小于锁的失效时间。例如锁自动失效时间为 10 秒，则超时时间应该在 5-50 毫秒之间。这样可以避免服务器端 Redis 已经挂掉的情况下，客户端还在一直等待响应结果。如果服务器端没有在规定时间内响应，客户端应该尽快尝试去另外一个 Redis 实例请求获取锁。
- 客户端使用当前时间减去开始获取锁时间（步骤 1 记录的时间）就得到获取锁使用的时间。当且仅当从大多数（N/2+1，这里是 3 个节点）的 Redis 节点都取到锁，并且使用的时间小于锁失效时间时，锁才算获取成功。
- 如果取到了锁，key 的真正有效时间等于有效时间减去获取锁所使用的时间（步骤 3 计算的结果）。
- 如果因为某些原因，获取锁失败（没有在至少 N/2+1 个 Redis 实例取到锁或者取锁时间已经超过了有效时间），客户端应该在所有的 Redis 实例上进行解锁（使用 Redis Lua 脚本）。



释放锁的过程相对比较简单：客户端向所有 Redis 节点发起释放锁的操作，包括加锁失败的节点，也需要执行释放锁的操作，antirez 在算法描述中特别强调这一点，这是为什么呢？



原因是可能存在某个节点加锁成功后返回客户端的响应包丢失了，这种情况在异步通信模型中是有可能发生的：客户端向服务器通信是正常的，但反方向却是有问题的。虽然对客户端而言，由于响应超时导致加锁失败，但是对 Redis 节点而言，SET 指令执行成功，意味着加锁成功。因此，释放锁的时候，客户端也应该对当时获取锁失败的那些 Redis 节点同样发起请求。



除此之外，为了避免 Redis 节点发生崩溃重启后造成锁丢失，从而影响锁的安全性，antirez 还提出了延时重启的概念，即一个节点崩溃后不要立即重启，而是等待一段时间后再进行重启，这段时间应该大于锁的有效时间。



关于 Redlock 的更深层次的学习，感兴趣的朋友可以查阅下官方文档，https://redis.io/topics/distlock



## 4 总结



分布式系统设计是实现复杂性和收益的平衡，既要尽可能地安全可靠，也要避免过度设计。Redlock 确实能够提供更安全的分布式锁，但也是有代价的，需要更多的 Redis 节点。在实际业务中，一般使用基于单点的 Redis 实现分布式锁就可以满足绝大部分的需求，偶尔出现数据不一致的情况，可通过人工介入回补数据进行解决，正所谓“技术不够，人工来凑”！。



![img](https://static001.infoq.cn/resource/image/d4/4e/d451355e47a750b127f43ca99506854e.jpg)

# Redis单线程工作原理

## 一、redis 的线程模型[#](https://www.cnblogs.com/mrmirror/p/13587311.html#2557712488)

------

> redis 内部使用**文件事件处理器 file event handler**，它是单线程的，所以redis才叫做单线程模型。它采用**IO多路复用机制**同时监听多个 socket，将产生事件的 socket 压入内存队列中，事件分派器根据 socket 上的事件类型来选择对应的事件处理器进行处理。

##### 文件事件处理器的结构：

- 多个 socket
- IO 多路复用程序
- 文件事件分派器
- 事件处理器（连接应答处理器、命令请求处理器、命令回复处理器）

##### 线程模型

多个 socket 可能会并发产生不同的操作，每个操作对应不同的文件事件，但是 IO多路复用程序会监听多个 socket，会将产生事件的 socket 放入队列中排队，事件分派器每次从队列中取出一个 socket，根据 socket 的事件类型交给对应的事件处理器进行处理。
[![img](https://img2020.cnblogs.com/blog/2136379/202008/2136379-20200830233359116-1537526582.png)](https://img2020.cnblogs.com/blog/2136379/202008/2136379-20200830233359116-1537526582.png)


 

## 二、一次客户端与redis的完整通信过程[#](https://www.cnblogs.com/mrmirror/p/13587311.html#3036043557)

------

##### 建立连接

1. 首先，redis 服务端进程初始化的时候，会将 server socket 的 AE_READABLE 事件与连接应答处理器关联。
2. 客户端 socket01 向 redis 进程的 server socket 请求建立连接，此时 server socket 会产生一个 AE_READABLE 事件，IO 多路复用程序监听到 server socket 产生的事件后，将该 socket 压入队列中。
3. 文件事件分派器从队列中获取 socket，交给连接应答处理器。
4. 连接应答处理器会创建一个能与客户端通信的 socket01，并将该 socket01 的 AE_READABLE 事件与命令请求处理器关联。

##### 执行一个set请求

1. 客户端发送了一个 set key value 请求，此时 redis 中的 socket01 会产生 AE_READABLE 事件，IO 多路复用程序将 socket01 压入队列，
2. 此时事件分派器从队列中获取到 socket01 产生的 AE_READABLE 事件，由于前面 socket01 的 AE_READABLE 事件已经与命令请求处理器关联，
3. 因此事件分派器将事件交给命令请求处理器来处理。命令请求处理器读取 socket01 的 key value 并在自己内存中完成 key value 的设置。
4. 操作完成后，它会将 socket01 的 AE_WRITABLE 事件与命令回复处理器关联。
5. 如果此时客户端准备好接收返回结果了，那么 redis 中的 socket01 会产生一个 AE_WRITABLE 事件，同样压入队列中，
6. 事件分派器找到相关联的命令回复处理器，由命令回复处理器对 socket01 输入本次操作的一个结果，比如 ok，之后解除 socket01 的 AE_WRITABLE 事件与命令回复处理器的关联。

[![img](https://img2020.cnblogs.com/blog/2136379/202008/2136379-20200830233422768-1205881078.png)](https://img2020.cnblogs.com/blog/2136379/202008/2136379-20200830233422768-1205881078.png)


 

## 三、redis为什么效率这么高？[#](https://www.cnblogs.com/mrmirror/p/13587311.html#3906638360)

------

- 纯内存操作。
- 核心是基于非阻塞的 IO 多路复用机制。
- C 语言实现，语言更接近操作系统，执行速度相对会更快。
- 单线程反而避免了多线程的频繁上下文切换问题，预防了多线程可能产生的竞争问题。

# 布隆过滤器

海量数据处理以及缓存穿透这两个场景让我认识了 布隆过滤器 ，我查阅了一些资料来了解它，但是很多现成资料并不满足我的需求，所以就决定自己总结一篇关于布隆过滤器的文章。希望通过这篇文章让更多人了解布隆过滤器，并且会实际去使用它！

下面我们将分为几个方面来介绍布隆过滤器：

1. 什么是布隆过滤器？
2. 布隆过滤器的原理介绍。
3. 布隆过滤器使用场景。
4. 通过 Java 编程手动实现布隆过滤器。
5. 利用Google开源的Guava中自带的布隆过滤器。
6. Redis 中的布隆过滤器。

### 1.什么是布隆过滤器？

首先，我们需要了解布隆过滤器的概念。

布隆过滤器（Bloom Filter）是一个叫做 Bloom 的老哥于1970年提出的。我们可以把它看作由二进制向量（或者说位数组）和一系列随机映射函数（哈希函数）两部分组成的数据结构。相比于我们平时常用的的 List、Map 、Set 等数据结构，它占用空间更少并且效率更高，但是缺点是其返回的结果是概率性的，而不是非常准确的。理论情况下添加到集合中的元素越多，误报的可能性就越大。并且，存放在布隆过滤器的数据不容易删除。

[![布隆过滤器示意图](https://camo.githubusercontent.com/cda39808ba1cbc106cdd931845f84dd26b76d7c14d84852fe83d0bfdbcbec64b/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d31312f2545352542382538332545392539412538362545382542462538372545362542422541342545352539392541382d6269742545362539352542302545372542422538342e706e67)](https://camo.githubusercontent.com/cda39808ba1cbc106cdd931845f84dd26b76d7c14d84852fe83d0bfdbcbec64b/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d31312f2545352542382538332545392539412538362545382542462538372545362542422541342545352539392541382d6269742545362539352542302545372542422538342e706e67)

位数组中的每个元素都只占用 1 bit ，并且每个元素只能是 0 或者 1。这样申请一个 100w 个元素的位数组只占用 1000000Bit / 8 = 125000 Byte = 125000/1024 kb ≈ 122kb 的空间。

总结：**一个名叫 Bloom 的人提出了一种来检索元素是否在给定大集合中的数据结构，这种数据结构是高效且性能很好的，但缺点是具有一定的错误识别率和删除难度。并且，理论情况下，添加到集合中的元素越多，误报的可能性就越大。**

### 2.布隆过滤器的原理介绍

**当一个元素加入布隆过滤器中的时候，会进行如下操作：**

1. 使用布隆过滤器中的哈希函数对元素值进行计算，得到哈希值（有几个哈希函数得到几个哈希值）。
2. 根据得到的哈希值，在位数组中把对应下标的值置为 1。

**当我们需要判断一个元素是否存在于布隆过滤器的时候，会进行如下操作：**

1. 对给定元素再次进行相同的哈希计算；
2. 得到值之后判断位数组中的每个元素是否都为 1，如果值都为 1，那么说明这个值在布隆过滤器中，如果存在一个值不为 1，说明该元素不在布隆过滤器中。

举个简单的例子：

[![布隆过滤器hash计算](https://camo.githubusercontent.com/bff9320e4e3bb9f59b072e1c40419c1f3c5122b3e12c4781733014f00318fad6/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d31312f2545352542382538332545392539412538362545382542462538372545362542422541342545352539392541382d686173682545382542462539302545372541452539372e706e67)](https://camo.githubusercontent.com/bff9320e4e3bb9f59b072e1c40419c1f3c5122b3e12c4781733014f00318fad6/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d31312f2545352542382538332545392539412538362545382542462538372545362542422541342545352539392541382d686173682545382542462539302545372541452539372e706e67)

如图所示，当字符串存储要加入到布隆过滤器中时，该字符串首先由多个哈希函数生成不同的哈希值，然后在对应的位数组的下表的元素设置为 1（当位数组初始化时 ，所有位置均为0）。当第二次存储相同字符串时，因为先前的对应位置已设置为 1，所以很容易知道此值已经存在（去重非常方便）。

如果我们需要判断某个字符串是否在布隆过滤器中时，只需要对给定字符串再次进行相同的哈希计算，得到值之后判断位数组中的每个元素是否都为 1，如果值都为 1，那么说明这个值在布隆过滤器中，如果存在一个值不为 1，说明该元素不在布隆过滤器中。

**不同的字符串可能哈希出来的位置相同，这种情况我们可以适当增加位数组大小或者调整我们的哈希函数。**

综上，我们可以得出：**布隆过滤器说某个元素存在，小概率会误判。布隆过滤器说某个元素不在，那么这个元素一定不在。**

### 3.布隆过滤器使用场景

1. 判断给定数据是否存在：比如判断一个数字是否存在于包含大量数字的数字集中（数字集很大，5亿以上！）、 防止缓存穿透（判断请求的数据是否有效避免直接绕过缓存请求数据库）等等、邮箱的垃圾邮件过滤、黑名单功能等等。
2. 去重：比如爬给定网址的时候对已经爬取过的 URL 去重。

### 4.通过 Java 编程手动实现布隆过滤器

我们上面已经说了布隆过滤器的原理，知道了布隆过滤器的原理之后就可以自己手动实现一个了。

如果你想要手动实现一个的话，你需要：

1. 一个合适大小的位数组保存数据
2. 几个不同的哈希函数
3. 添加元素到位数组（布隆过滤器）的方法实现
4. 判断给定元素是否存在于位数组（布隆过滤器）的方法实现。

下面给出一个我觉得写的还算不错的代码（参考网上已有代码改进得到，对于所有类型对象皆适用）：

```
import java.util.BitSet;

public class MyBloomFilter {

    /**
     * 位数组的大小
     */
    private static final int DEFAULT_SIZE = 2 << 24;
    /**
     * 通过这个数组可以创建 6 个不同的哈希函数
     */
    private static final int[] SEEDS = new int[]{3, 13, 46, 71, 91, 134};

    /**
     * 位数组。数组中的元素只能是 0 或者 1
     */
    private BitSet bits = new BitSet(DEFAULT_SIZE);

    /**
     * 存放包含 hash 函数的类的数组
     */
    private SimpleHash[] func = new SimpleHash[SEEDS.length];

    /**
     * 初始化多个包含 hash 函数的类的数组，每个类中的 hash 函数都不一样
     */
    public MyBloomFilter() {
        // 初始化多个不同的 Hash 函数
        for (int i = 0; i < SEEDS.length; i++) {
            func[i] = new SimpleHash(DEFAULT_SIZE, SEEDS[i]);
        }
    }

    /**
     * 添加元素到位数组
     */
    public void add(Object value) {
        for (SimpleHash f : func) {
            bits.set(f.hash(value), true);
        }
    }

    /**
     * 判断指定元素是否存在于位数组
     */
    public boolean contains(Object value) {
        boolean ret = true;
        for (SimpleHash f : func) {
            ret = ret && bits.get(f.hash(value));
        }
        return ret;
    }

    /**
     * 静态内部类。用于 hash 操作！
     */
    public static class SimpleHash {

        private int cap;
        private int seed;

        public SimpleHash(int cap, int seed) {
            this.cap = cap;
            this.seed = seed;
        }

        /**
         * 计算 hash 值
         */
        public int hash(Object value) {
            int h;
            return (value == null) ? 0 : Math.abs(seed * (cap - 1) & ((h = value.hashCode()) ^ (h >>> 16)));
        }

    }
}
```

测试：

```
        String value1 = "https://javaguide.cn/";
        String value2 = "https://github.com/Snailclimb";
        MyBloomFilter filter = new MyBloomFilter();
        System.out.println(filter.contains(value1));
        System.out.println(filter.contains(value2));
        filter.add(value1);
        filter.add(value2);
        System.out.println(filter.contains(value1));
        System.out.println(filter.contains(value2));
```

Output:

```
false
false
true
true
```

测试：

```
        Integer value1 = 13423;
        Integer value2 = 22131;
        MyBloomFilter filter = new MyBloomFilter();
        System.out.println(filter.contains(value1));
        System.out.println(filter.contains(value2));
        filter.add(value1);
        filter.add(value2);
        System.out.println(filter.contains(value1));
        System.out.println(filter.contains(value2));
```

Output:

```
false
false
true
true
```

### 5.利用Google开源的 Guava中自带的布隆过滤器

自己实现的目的主要是为了让自己搞懂布隆过滤器的原理，Guava 中布隆过滤器的实现算是比较权威的，所以实际项目中我们不需要手动实现一个布隆过滤器。

首先我们需要在项目中引入 Guava 的依赖：

```
        <dependency>
            <groupId>com.google.guava</groupId>
            <artifactId>guava</artifactId>
            <version>28.0-jre</version>
        </dependency>
```

实际使用如下：

我们创建了一个最多存放 最多 1500个整数的布隆过滤器，并且我们可以容忍误判的概率为百分之（0.01）

```
        // 创建布隆过滤器对象
        BloomFilter<Integer> filter = BloomFilter.create(
                Funnels.integerFunnel(),
                1500,
                0.01);
        // 判断指定元素是否存在
        System.out.println(filter.mightContain(1));
        System.out.println(filter.mightContain(2));
        // 将元素添加进布隆过滤器
        filter.put(1);
        filter.put(2);
        System.out.println(filter.mightContain(1));
        System.out.println(filter.mightContain(2));
```

在我们的示例中，当`mightContain（）` 方法返回*true*时，我们可以99％确定该元素在过滤器中，当过滤器返回*false*时，我们可以100％确定该元素不存在于过滤器中。

**Guava 提供的布隆过滤器的实现还是很不错的（想要详细了解的可以看一下它的源码实现），但是它有一个重大的缺陷就是只能单机使用（另外，容量扩展也不容易），而现在互联网一般都是分布式的场景。为了解决这个问题，我们就需要用到 Redis 中的布隆过滤器了。**

### 6.Redis 中的布隆过滤器

#### 6.1介绍

Redis v4.0 之后有了 Module（模块/插件） 功能，Redis Modules 让 Redis 可以使用外部模块扩展其功能 。布隆过滤器就是其中的 Module。详情可以查看 Redis 官方对 Redis Modules 的介绍 ：https://redis.io/modules

另外，官网推荐了一个 RedisBloom 作为 Redis 布隆过滤器的 Module,地址：https://github.com/RedisBloom/RedisBloom. 其他还有：

- redis-lua-scaling-bloom-filter （lua 脚本实现）：https://github.com/erikdubbelboer/redis-lua-scaling-bloom-filter
- pyreBloom（Python中的快速Redis 布隆过滤器） ：https://github.com/seomoz/pyreBloom
- ......

RedisBloom 提供了多种语言的客户端支持，包括：Python、Java、JavaScript 和 PHP。

#### 6.2使用Docker安装

如果我们需要体验 Redis 中的布隆过滤器非常简单，通过 Docker 就可以了！我们直接在 Google 搜索**docker redis bloomfilter** 然后在排除广告的第一条搜素结果就找到了我们想要的答案（这是我平常解决问题的一种方式，分享一下），具体地址：https://hub.docker.com/r/redislabs/rebloom/ （介绍的很详细 ）。

**具体操作如下：**

```
➜  ~ docker run -p 6379:6379 --name redis-redisbloom redislabs/rebloom:latest
➜  ~ docker exec -it redis-redisbloom bash
root@21396d02c252:/data# redis-cli
127.0.0.1:6379> 
```

#### 6.3常用命令一览

> 注意： key:布隆过滤器的名称，item : 添加的元素。

1. **`BF.ADD `**：将元素添加到布隆过滤器中，如果该过滤器尚不存在，则创建该过滤器。格式：`BF.ADD {key} {item}`。
2. **`BF.MADD `**: 将一个或多个元素添加到“布隆过滤器”中，并创建一个尚不存在的过滤器。该命令的操作方式`BF.ADD`与之相同，只不过它允许多个输入并返回多个值。格式：`BF.MADD {key} {item} [item ...]` 。
3. **`BF.EXISTS` ** : 确定元素是否在布隆过滤器中存在。格式：`BF.EXISTS {key} {item}`。
4. **`BF.MEXISTS`** ： 确定一个或者多个元素是否在布隆过滤器中存在格式：`BF.MEXISTS {key} {item} [item ...]`。

另外，`BF.RESERVE` 命令需要单独介绍一下：

这个命令的格式如下：

`BF.RESERVE {key} {error_rate} {capacity} [EXPANSION expansion] `。

下面简单介绍一下每个参数的具体含义：

1. key：布隆过滤器的名称
2. error_rate :误报的期望概率。这应该是介于0到1之间的十进制值。例如，对于期望的误报率0.1％（1000中为1），error_rate应该设置为0.001。该数字越接近零，则每个项目的内存消耗越大，并且每个操作的CPU使用率越高。
3. capacity: 过滤器的容量。当实际存储的元素个数超过这个值之后，性能将开始下降。实际的降级将取决于超出限制的程度。随着过滤器元素数量呈指数增长，性能将线性下降。

可选参数：

- expansion：如果创建了一个新的子过滤器，则其大小将是当前过滤器的大小乘以`expansion`。默认扩展值为2。这意味着每个后续子过滤器将是前一个子过滤器的两倍。

#### 6.4实际使用

```
127.0.0.1:6379> BF.ADD myFilter java
(integer) 1
127.0.0.1:6379> BF.ADD myFilter javaguide
(integer) 1
127.0.0.1:6379> BF.EXISTS myFilter java
(integer) 1
127.0.0.1:6379> BF.EXISTS myFilter javaguide
(integer) 1
127.0.0.1:6379> BF.EXISTS myFilter github
(integer) 0
```

# Redis的缓存雪崩、缓存击穿、缓存穿透与缓存预热、缓存降级
一、缓存雪崩：
1、什么是缓存雪崩：

如果缓在某一个时刻出现大规模的key失效，那么就会导致大量的请求打在了数据库上面，导致数据库压力巨大，如果在高并发的情况下，可能瞬间就会导致数据库宕机。这时候如果运维马上又重启数据库，马上又会有新的流量把数据库打死。这就是缓存雪崩。

2、问题分析：

造成缓存雪崩的关键在于同一时间的大规模的key失效，为什么会出现这个问题，主要有两种可能：第一种是Redis宕机，第二种可能就是采用了相同的过期时间。搞清楚原因之后，那么有什么解决方案呢？

3、解决方案：

（1）事前：

① 均匀过期：设置不同的过期时间，让缓存失效的时间尽量均匀，避免相同的过期时间导致缓存雪崩，造成大量数据库的访问。

② 分级缓存：第一级缓存失效的基础上，访问二级缓存，每一级缓存的失效时间都不同。

③ 热点数据缓存永远不过期。

永不过期实际包含两层意思：

物理不过期，针对热点key不设置过期时间

逻辑过期，把过期时间存在key对应的value里，如果发现要过期了，通过一个后台的异步线程进行缓存的构建

④ 保证Redis缓存的高可用，防止Redis宕机导致缓存雪崩的问题。可以使用 主从+ 哨兵，Redis集群来避免 Redis 全盘崩溃的情况。

（2）事中：

① 互斥锁：在缓存失效后，通过互斥锁或者队列来控制读数据写缓存的线程数量，比如某个key只允许一个线程查询数据和写缓存，其他线程等待。这种方式会阻塞其他的线程，此时系统的吞吐量会下降

② 使用熔断机制，限流降级。当流量达到一定的阈值，直接返回“系统拥挤”之类的提示，防止过多的请求打在数据库上将数据库击垮，至少能保证一部分用户是可以正常使用，其他用户多刷新几次也能得到结果。

（3）事后：

① 开启Redis持久化机制，尽快恢复缓存数据，一旦重启，就能从磁盘上自动加载数据恢复内存中的数据。

 

二、缓存击穿：
1、什么是缓存击穿：

缓存击穿跟缓存雪崩有点类似，缓存雪崩是大规模的key失效，而缓存击穿是某个热点的key失效，大并发集中对其进行请求，就会造成大量请求读缓存没读到数据，从而导致高并发访问数据库，引起数据库压力剧增。这种现象就叫做缓存击穿。

2、问题分析：

关键在于某个热点的key失效了，导致大并发集中打在数据库上。所以要从两个方面解决，第一是否可以考虑热点key不设置过期时间，第二是否可以考虑降低打在数据库上的请求数量。

3、解决方案：

（1）在缓存失效后，通过互斥锁或者队列来控制读数据写缓存的线程数量，比如某个key只允许一个线程查询数据和写缓存，其他线程等待。这种方式会阻塞其他的线程，此时系统的吞吐量会下降

（2）热点数据缓存永远不过期。

永不过期实际包含两层意思：

物理不过期，针对热点key不设置过期时间

逻辑过期，把过期时间存在key对应的value里，如果发现要过期了，通过一个后台的异步线程进行缓存的构建

 

三、缓存穿透：
1、什么是缓存穿透：

缓存穿透是指用户请求的数据在缓存中不存在即没有命中，同时在数据库中也不存在，导致用户每次请求该数据都要去数据库中查询一遍。如果有恶意攻击者不断请求系统中不存在的数据，会导致短时间大量请求落在数据库上，造成数据库压力过大，甚至导致数据库承受不住而宕机崩溃。

2、问题分析：

缓存穿透的关键在于在Redis中查不到key值，它和缓存击穿的根本区别在于传进来的key在Redis中是不存在的。假如有黑客传进大量的不存在的key，那么大量的请求打在数据库上是很致命的问题，所以在日常开发中要对参数做好校验，一些非法的参数，不可能存在的key就直接返回错误提示。



3、解决方法：

（1）将无效的key存放进Redis中：

当出现Redis查不到数据，数据库也查不到数据的情况，我们就把这个key保存到Redis中，设置value="null"，并设置其过期时间极短，后面再出现查询这个key的请求的时候，直接返回null，就不需要再查询数据库了。但这种处理方式是有问题的，假如传进来的这个不存在的Key值每次都是随机的，那存进Redis也没有意义。

（2）使用布隆过滤器：

如果布隆过滤器判定某个 key 不存在布隆过滤器中，那么就一定不存在，如果判定某个 key 存在，那么很大可能是存在(存在一定的误判率)。于是我们可以在缓存之前再加一个布隆过滤器，将数据库中的所有key都存储在布隆过滤器中，在查询Redis前先去布隆过滤器查询 key 是否存在，如果不存在就直接返回，不让其访问数据库，从而避免了对底层存储系统的查询压力。



如何选择：针对一些恶意攻击，攻击带过来的大量key是随机，那么我们采用第一种方案就会缓存大量不存在key的数据。那么这种方案就不合适了，我们可以先对使用布隆过滤器方案进行过滤掉这些key。所以，针对这种key异常多、请求重复率比较低的数据，优先使用第二种方案直接过滤掉。而对于空数据的key有限的，重复率比较高的，则可优先采用第一种方式进行缓存。

 

四、缓存预热：
1、什么是缓存预热：

缓存预热是指系统上线后，提前将相关的缓存数据加载到缓存系统。避免在用户请求的时候，先查询数据库，然后再将数据缓存的问题，用户直接查询事先被预热的缓存数据。

如果不进行预热，那么Redis初始状态数据为空，系统上线初期，对于高并发的流量，都会访问到数据库中， 对数据库造成流量的压力。

2、缓存预热解决方案：

（1）数据量不大的时候，工程启动的时候进行加载缓存动作；

（2）数据量大的时候，设置一个定时任务脚本，进行缓存的刷新；

（3）数据量太大的时候，优先保证热点数据进行提前加载到缓存。

 

五、缓存降级：
缓存降级是指缓存失效或缓存服务器挂掉的情况下，不去访问数据库，直接返回默认数据或访问服务的内存数据。降级一般是有损的操作，所以尽量减少降级对于业务的影响程度。

在项目实战中通常会将部分热点数据缓存到服务的内存中，这样一旦缓存出现异常，可以直接使用服务的内存数据，从而避免数据库遭受巨大压力。
————————————————

# Redis主从复制，哨兵模式

在开发测试环境中，我们一般搭建Redis的单实例来应对开发测试需求，但是在生产环境，如果对可用性、可靠性要求较高，则需要引入Redis的集群方案。虽然现在各大云平台有提供缓存服务可以直接使用，但了解一下其背后的实现与原理总还是有些必要（比如面试）， 本文就一起来学习一下Redis的几种集群方案。

Redis支持三种集群方案

- 主从复制模式
- Sentinel（哨兵）模式
- Cluster模式

## 主从复制模式

### 1. 基本原理

主从复制模式中包含一个主数据库实例（master）与一个或多个从数据库实例（slave），如下图



![redis-master-slave](https://user-gold-cdn.xitu.io/2020/3/19/170f23858652a465?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



客户端可对主数据库进行读写操作，对从数据库进行读操作，主数据库写入的数据会实时自动同步给从数据库。

具体工作机制为：

1. slave启动后，向master发送SYNC命令，master接收到SYNC命令后通过bgsave保存快照（即上文所介绍的RDB持久化），并使用缓冲区记录保存快照这段时间内执行的写命令
2. master将保存的快照文件发送给slave，并继续记录执行的写命令
3. slave接收到快照文件后，加载快照文件，载入数据
4. master快照发送完后开始向slave发送缓冲区的写命令，slave接收命令并执行，完成复制初始化
5. 此后master每次执行一个写命令都会同步发送给slave，保持master与slave之间数据的一致性

### 2. 部署示例

本示例基于Redis 5.0.3版。

redis.conf的主要配置

```
###网络相关###
# bind 127.0.0.1 # 绑定监听的网卡IP，注释掉或配置成0.0.0.0可使任意IP均可访问
protected-mode no # 关闭保护模式，使用密码访问
port 6379  # 设置监听端口，建议生产环境均使用自定义端口
timeout 30 # 客户端连接空闲多久后断开连接，单位秒，0表示禁用

###通用配置###
daemonize yes # 在后台运行
pidfile /var/run/redis_6379.pid  # pid进程文件名
logfile /usr/local/redis/logs/redis.log # 日志文件的位置

###RDB持久化配置###
save 900 1 # 900s内至少一次写操作则执行bgsave进行RDB持久化
save 300 10
save 60 10000 
# 如果禁用RDB持久化，可在这里添加 save ""
rdbcompression yes #是否对RDB文件进行压缩，建议设置为no，以（磁盘）空间换（CPU）时间
dbfilename dump.rdb # RDB文件名称
dir /usr/local/redis/datas # RDB文件保存路径，AOF文件也保存在这里

###AOF配置###
appendonly yes # 默认值是no，表示不使用AOF增量持久化的方式，使用RDB全量持久化的方式
appendfsync everysec # 可选值 always， everysec，no，建议设置为everysec

###设置密码###
requirepass 123456 # 设置复杂一点的密码
复制代码
```

部署主从复制模式只需稍微调整slave的配置，在redis.conf中添加

```
replicaof 127.0.0.1 6379 # master的ip，port
masterauth 123456 # master的密码
replica-serve-stale-data no # 如果slave无法与master同步，设置成slave不可读，方便监控脚本发现问题
复制代码
```

本示例在单台服务器上配置master端口6379，两个slave端口分别为7001,7002，启动master，再启动两个slave

```
[root@dev-server-1 master-slave]# redis-server master.conf
[root@dev-server-1 master-slave]# redis-server slave1.conf
[root@dev-server-1 master-slave]# redis-server slave2.conf
复制代码
```

进入master数据库，写入一个数据，再进入一个slave数据库，立即便可访问刚才写入master数据库的数据。如下所示

```
[root@dev-server-1 master-slave]# redis-cli 
127.0.0.1:6379> auth 123456
OK
127.0.0.1:6379> set site blog.jboost
OK
127.0.0.1:6379> get site
"blog.jboost"
127.0.0.1:6379> info replication
# Replication
role:master
connected_slaves:2
slave0:ip=127.0.0.1,port=7001,state=online,offset=13364738,lag=1
slave1:ip=127.0.0.1,port=7002,state=online,offset=13364738,lag=0
...
127.0.0.1:6379> exit

[root@dev-server-1 master-slave]# redis-cli -p 7001
127.0.0.1:7001> auth 123456
OK
127.0.0.1:7001> get site
"blog.jboost"
复制代码
```

执行`info replication`命令可以查看连接该数据库的其它库的信息，如上可看到有两个slave连接到master

### 3. 主从复制的优缺点

优点：

1. master能自动将数据同步到slave，可以进行读写分离，分担master的读压力
2. master、slave之间的同步是以非阻塞的方式进行的，同步期间，客户端仍然可以提交查询或更新请求

缺点：

1. 不具备自动容错与恢复功能，master或slave的宕机都可能导致客户端请求失败，需要等待机器重启或手动切换客户端IP才能恢复
2. master宕机，如果宕机前数据没有同步完，则切换IP后会存在数据不一致的问题
3. 难以支持在线扩容，Redis的容量受限于单机配置

## Sentinel（哨兵）模式

### 1. 基本原理

哨兵模式基于主从复制模式，只是引入了哨兵来监控与自动处理故障。如图



![redis-sentinel](https://user-gold-cdn.xitu.io/2020/3/19/170f23893704295e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



哨兵顾名思义，就是来为Redis集群站哨的，一旦发现问题能做出相应的应对处理。其功能包括

1. 监控master、slave是否正常运行
2. 当master出现故障时，能自动将一个slave转换为master（大哥挂了，选一个小弟上位）
3. 多个哨兵可以监控同一个Redis，哨兵之间也会自动监控

哨兵模式的具体工作机制：

在配置文件中通过 `sentinel monitor <master-name> <ip> <redis-port> <quorum>` 来定位master的IP、端口，一个哨兵可以监控多个master数据库，只需要提供多个该配置项即可。哨兵启动后，会与要监控的master建立两条连接：

1. 一条连接用来订阅master的`_sentinel_:hello`频道与获取其他监控该master的哨兵节点信息
2. 另一条连接定期向master发送INFO等命令获取master本身的信息

与master建立连接后，哨兵会执行三个操作：

1. 定期（一般10s一次，当master被标记为主观下线时，改为1s一次）向master和slave发送INFO命令
2. 定期向master和slave的`_sentinel_:hello`频道发送自己的信息
3. 定期（1s一次）向master、slave和其他哨兵发送PING命令

发送INFO命令可以获取当前数据库的相关信息从而实现新节点的自动发现。所以说哨兵只需要配置master数据库信息就可以自动发现其slave信息。获取到slave信息后，哨兵也会与slave建立两条连接执行监控。通过INFO命令，哨兵可以获取主从数据库的最新信息，并进行相应的操作，比如角色变更等。

接下来哨兵向主从数据库的_sentinel_:hello频道发送信息与同样监控这些数据库的哨兵共享自己的信息，发送内容为哨兵的ip端口、运行id、配置版本、master名字、master的ip端口还有master的配置版本。这些信息有以下用处：

1. 其他哨兵可以通过该信息判断发送者是否是新发现的哨兵，如果是的话会创建一个到该哨兵的连接用于发送PING命令。
2. 其他哨兵通过该信息可以判断master的版本，如果该版本高于直接记录的版本，将会更新
3. 当实现了自动发现slave和其他哨兵节点后，哨兵就可以通过定期发送PING命令定时监控这些数据库和节点有没有停止服务。

如果被PING的数据库或者节点超时（通过 `sentinel down-after-milliseconds master-name milliseconds` 配置）未回复，哨兵认为其主观下线（sdown，s就是Subjectively —— 主观地）。如果下线的是master，哨兵会向其它哨兵发送命令询问它们是否也认为该master主观下线，如果达到一定数目（即配置文件中的quorum）投票，哨兵会认为该master已经客观下线（odown，o就是Objectively —— 客观地），并选举领头的哨兵节点对主从系统发起故障恢复。若没有足够的sentinel进程同意master下线，master的客观下线状态会被移除，若master重新向sentinel进程发送的PING命令返回有效回复，master的主观下线状态就会被移除

哨兵认为master客观下线后，故障恢复的操作需要由选举的领头哨兵来执行，选举采用Raft算法：

1. 发现master下线的哨兵节点（我们称他为A）向每个哨兵发送命令，要求对方选自己为领头哨兵
2. 如果目标哨兵节点没有选过其他人，则会同意选举A为领头哨兵
3. 如果有超过一半的哨兵同意选举A为领头，则A当选
4. 如果有多个哨兵节点同时参选领头，此时有可能存在一轮投票无竞选者胜出，此时每个参选的节点等待一个随机时间后再次发起参选请求，进行下一轮投票竞选，直至选举出领头哨兵

选出领头哨兵后，领头者开始对系统进行故障恢复，从出现故障的master的从数据库中挑选一个来当选新的master,选择规则如下：

1. 所有在线的slave中选择优先级最高的，优先级可以通过slave-priority配置
2. 如果有多个最高优先级的slave，则选取复制偏移量最大（即复制越完整）的当选
3. 如果以上条件都一样，选取id最小的slave

挑选出需要继任的slave后，领头哨兵向该数据库发送命令使其升格为master，然后再向其他slave发送命令接受新的master，最后更新数据。将已经停止的旧的master更新为新的master的从数据库，使其恢复服务后以slave的身份继续运行。

### 2. 部署演示

本示例基于Redis 5.0.3版。

哨兵模式基于前文的主从复制模式。哨兵的配置文件为sentinel.conf，在文件中添加

```
sentinel monitor mymaster 127.0.0.1 6379 1 # mymaster定义一个master数据库的名称，后面是master的ip， port，1表示至少需要一个Sentinel进程同意才能将master判断为失效，如果不满足这个条件，则自动故障转移（failover）不会执行
sentinel auth-pass mymaster 123456 # master的密码

sentinel down-after-milliseconds mymaster 5000 # 5s未回复PING，则认为master主观下线，默认为30s
sentinel parallel-syncs mymaster 2  # 指定在执行故障转移时，最多可以有多少个slave实例在同步新的master实例，在slave实例较多的情况下这个数字越小，同步的时间越长，完成故障转移所需的时间就越长
sentinel failover-timeout mymaster 300000 # 如果在该时间（ms）内未能完成故障转移操作，则认为故障转移失败，生产环境需要根据数据量设置该值
复制代码
```

> 一个哨兵可以监控多个master数据库，只需按上述配置添加多套

分别以26379,36379,46379端口启动三个sentinel

```
[root@dev-server-1 sentinel]# redis-server sentinel1.conf --sentinel
[root@dev-server-1 sentinel]# redis-server sentinel2.conf --sentinel
[root@dev-server-1 sentinel]# redis-server sentinel3.conf --sentinel
复制代码
```

也可以使用`redis-sentinel sentinel1.conf` 命令启动。此时集群包含一个master、两个slave、三个sentinel，如图，



![redis-cluster-instance](https://user-gold-cdn.xitu.io/2020/3/19/170f238a78f75da5?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



我们来模拟master挂掉的场景，执行 `kill -9 3017` 将master进程干掉，进入slave中执行 `info replication`查看，

```
[root@dev-server-1 sentinel]# redis-cli -p 7001
127.0.0.1:7001> auth 123456
OK
127.0.0.1:7001> info replication
# Replication
role:slave
master_host:127.0.0.1
master_port:7002
master_link_status:up
master_last_io_seconds_ago:1
master_sync_in_progress:0
# 省略
127.0.0.1:7001> exit
[root@dev-server-1 sentinel]# redis-cli -p 7002
127.0.0.1:7002> auth 123456
OK
127.0.0.1:7002> info replication
# Replication
role:master
connected_slaves:1
slave0:ip=127.0.0.1,port=7001,state=online,offset=13642721,lag=1
# 省略
复制代码
```

可以看到slave 7002已经成功上位晋升为master（role：master），接收一个slave 7001的连接。此时查看slave2.conf配置文件，发现`replicaof`的配置已经被移除了，slave1.conf的配置文件里`replicaof 127.0.0.1 6379` 被改为 `replicaof 127.0.0.1 7002`。重新启动master，也可以看到master.conf配置文件中添加了`replicaof 127.0.0.1 7002`的配置项，可见大哥（master）下位后，再出来混就只能当当小弟（slave）了，三十年河东三十年河西。

### 3. 哨兵模式的优缺点

优点：

1. 哨兵模式基于主从复制模式，所以主从复制模式有的优点，哨兵模式也有
2. 哨兵模式下，master挂掉可以自动进行切换，系统可用性更高

缺点：

1. 同样也继承了主从模式难以在线扩容的缺点，Redis的容量受限于单机配置
2. 需要额外的资源来启动sentinel进程，实现相对复杂一点，同时slave节点作为备份节点不提供服务

## Cluster模式

### 1. 基本原理

哨兵模式解决了主从复制不能自动故障转移，达不到高可用的问题，但还是存在难以在线扩容，Redis容量受限于单机配置的问题。Cluster模式实现了Redis的分布式存储，即每台节点存储不同的内容，来解决在线扩容的问题。如图



![redis-cluster](https://user-gold-cdn.xitu.io/2020/3/19/170f238bca333f96?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



Cluster采用无中心结构,它的特点如下：

1. 所有的redis节点彼此互联(PING-PONG机制),内部使用二进制协议优化传输速度和带宽
2. 节点的fail是通过集群中超过半数的节点检测失效时才生效
3. 客户端与redis节点直连,不需要中间代理层.客户端不需要连接集群所有节点,连接集群中任何一个可用节点即可

Cluster模式的具体工作机制：

1. 在Redis的每个节点上，都有一个插槽（slot），取值范围为0-16383
2. 当我们存取key的时候，Redis会根据CRC16的算法得出一个结果，然后把结果对16384求余数，这样每个key都会对应一个编号在0-16383之间的哈希槽，通过这个值，去找到对应的插槽所对应的节点，然后直接自动跳转到这个对应的节点上进行存取操作
3. 为了保证高可用，Cluster模式也引入主从复制模式，一个主节点对应一个或者多个从节点，当主节点宕机的时候，就会启用从节点
4. 当其它主节点ping一个主节点A时，如果半数以上的主节点与A通信超时，那么认为主节点A宕机了。如果主节点A和它的从节点都宕机了，那么该集群就无法再提供服务了

Cluster模式集群节点最小配置6个节点(3主3从，因为需要半数以上)，其中主节点提供读写操作，从节点作为备用节点，不提供请求，只作为故障转移使用。

### 2. 部署演示

本示例基于Redis 5.0.3版。

Cluster模式的部署比较简单，首先在redis.conf中

```
port 7100 # 本示例6个节点端口分别为7100,7200,7300,7400,7500,7600 
daemonize yes # r后台运行 
pidfile /var/run/redis_7100.pid # pidfile文件对应7100,7200,7300,7400,7500,7600 
cluster-enabled yes # 开启集群模式 
masterauth passw0rd # 如果设置了密码，需要指定master密码
cluster-config-file nodes_7100.conf # 集群的配置文件，同样对应7100,7200等六个节点
cluster-node-timeout 15000 # 请求超时 默认15秒，可自行设置 
复制代码
```

分别以端口7100,7200,7300,7400,7500,7600 启动六个实例(如果是每个服务器一个实例则配置可一样)

```
[root@dev-server-1 cluster]# redis-server redis_7100.conf
[root@dev-server-1 cluster]# redis-server redis_7200.conf
...
复制代码
```

然后通过命令将这个6个实例组成一个3主节点3从节点的集群，

```
redis-cli --cluster create --cluster-replicas 1 127.0.0.1:7100 127.0.0.1:7200 127.0.0.1:7300 127.0.0.1:7400 127.0.0.1:7500 127.0.0.1:7600 -a passw0rd
复制代码
```

执行结果如图



![redis-cluster-deploy](https://user-gold-cdn.xitu.io/2020/3/19/170f238d683bd472?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



可以看到 7100， 7200， 7300 作为3个主节点，分配的slot分别为 0-5460， 5461-10922， 10923-16383， 7600作为7100的slave， 7500作为7300的slave，7400作为7200的slave。

我们连接7100设置一个值

```
[root@dev-server-1 cluster]# redis-cli -p 7100 -c -a passw0rd
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
127.0.0.1:7100> set site blog.jboost
-> Redirected to slot [9421] located at 127.0.0.1:7200
OK
127.0.0.1:7200> get site
"blog.jboost"
127.0.0.1:7200>
复制代码
```

注意添加 -c 参数表示以集群模式，否则报 `(error) MOVED 9421 127.0.0.1:7200` 错误， 以 -a 参数指定密码，否则报`(error) NOAUTH Authentication required`错误。

从上面命令看到key为site算出的slot为9421，落在7200节点上，所以有`Redirected to slot [9421] located at 127.0.0.1:7200`，集群会自动进行跳转。因此客户端可以连接任何一个节点来进行数据的存取。

通过`cluster nodes`可查看集群的节点信息

```
127.0.0.1:7200> cluster nodes
eb28aaf090ed1b6b05033335e3d90a202b422d6c 127.0.0.1:7500@17500 slave c1047de2a1b5d5fa4666d554376ca8960895a955 0 1584165266071 5 connected
4cc0463878ae00e5dcf0b36c4345182e021932bc 127.0.0.1:7400@17400 slave 5544aa5ff20f14c4c3665476de6e537d76316b4a 0 1584165267074 4 connected
dbbb6420d64db22f35a9b6fa460b0878c172a2fb 127.0.0.1:7100@17100 master - 0 1584165266000 1 connected 0-5460
d4b434f5829e73e7e779147e905eea6247ffa5a2 127.0.0.1:7600@17600 slave dbbb6420d64db22f35a9b6fa460b0878c172a2fb 0 1584165265000 6 connected
5544aa5ff20f14c4c3665476de6e537d76316b4a 127.0.0.1:7200@17200 myself,master - 0 1584165267000 2 connected 5461-10922
c1047de2a1b5d5fa4666d554376ca8960895a955 127.0.0.1:7300@17300 master - 0 1584165268076 3 connected 10923-16383
复制代码
```

我们将7200通过 `kill -9 pid`杀死进程来验证集群的高可用，重新进入集群执行`cluster nodes`可以看到7200 fail了，但是7400成了master，重新启动7200，可以看到此时7200已经变成了slave。

### 3. Cluster模式的优缺点

优点：

1. 无中心架构，数据按照slot分布在多个节点。
2. 集群中的每个节点都是平等的关系，每个节点都保存各自的数据和整个集群的状态。每个节点都和其他所有节点连接，而且这些连接保持活跃，这样就保证了我们只需要连接集群中的任意一个节点，就可以获取到其他节点的数据。
3. 可线性扩展到1000多个节点，节点可动态添加或删除
4. 能够实现自动故障转移，节点之间通过gossip协议交换状态信息，用投票机制完成slave到master的角色转换

缺点：

1. 客户端实现复杂，驱动要求实现Smart Client，缓存slots mapping信息并及时更新，提高了开发难度。目前仅JedisCluster相对成熟，异常处理还不完善，比如常见的“max redirect exception”
2. 节点会因为某些原因发生阻塞（阻塞时间大于 cluster-node-timeout）被判断下线，这种failover是没有必要的
3. 数据通过异步复制，不保证数据的强一致性
4. slave充当“冷备”，不能缓解读压力
5. 批量操作限制，目前只支持具有相同slot值的key执行批量操作，对mset、mget、sunion等操作支持不友好
6. key事务操作支持有线，只支持多key在同一节点的事务操作，多key分布不同节点时无法使用事务功能
7. 不支持多数据库空间，单机redis可以支持16个db，集群模式下只能使用一个，即db 0

Redis Cluster模式不建议使用pipeline和multi-keys操作，减少max redirect产生的场景。

## 总结

本文介绍了Redis集群方案的三种模式，其中主从复制模式能实现读写分离，但是不能自动故障转移；哨兵模式基于主从复制模式，能实现自动故障转移，达到高可用，但与主从复制模式一样，不能在线扩容，容量受限于单机的配置；Cluster模式通过无中心化架构，实现分布式存储，可进行线性扩展，也能高可用，但对于像批量操作、事务操作等的支持性不够好。三种模式各有优缺点，可根据实际场景进行选择。

## Redis 主从复制、哨兵和集群三者区别

**主从复制是为了数据备份**，**哨兵是为1·了高可用**，Redis主服务器挂了哨兵可以切换，**集群则是因为单实例能力有限，搞多个分散压力**，简短总结如下：

主从模式：**备份数据、负载均衡**，一个Master可以有多个Slaves。

sentinel发现master挂了后，就会从slave中重新选举一个master。

cluster是为了解决单机Redis容量有限的问题，将数据按一定的规则分配到多台机器。

**sentinel着眼于高可用，Cluster提高并发量。**

**1. 主从模式：读写分离，备份，一个Master可以有多个Slaves。**

**2. 哨兵sentinel：监控，自动转移，哨兵发现主服务器挂了后，就会从slave中重新选举一个主服务器。**

**3. 集群：为了解决单机Redis容量有限的问题，将数据按一定的规则分配到多台机器，内存/QPS不受限于单机，可受益于分布式集群高扩展性。**



作者：半路雨歌
链接：https://juejin.cn/post/6844904097116585991
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

# Redis并发竞争锁

**01. 并发竞争的由来**

**1.Redis高并发的问题**

Redis缓存的高性能有目共睹，应用的场景也是非常广泛，但是在高并发的场景下，也会出现问题：缓存击穿、缓存雪崩、缓存和数据一致性，以及今天要谈到的缓存并发竞争。

这里的并发指的是多个redis的client同时set key引起的并发问题。

**2.出现并发设置Key的原因**

Redis是一种单线程机制的nosql数据库，基于key-value，数据可持久化落盘。由于单线程所以Redis本身并没有锁的概念，多个客户端连接并不存在竞争关系，但是利用jedis等客户端对Redis进行并发访问时会出现问题。

比如：同时有多个子系统去set一个key。这个时候要注意什么呢？

**3.举一个例子**

多客户端同时并发写一个key，一个key的值是1，本来按顺序修改为2,3,4，最后是4，但是顺序变成了4,3,2，最后变成了2。

![img](https://user-gold-cdn.xitu.io/2019/5/19/16aceb58b476a1ea?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

如何解决redis的并发竞争key问题呢？下面给到**2个Redis并发竞争的解决方案。**

![img](https://user-gold-cdn.xitu.io/2019/5/19/16aceb58b5ea14a1?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**02. 第一种方案：分布式锁+时间戳**

**1.整体技术方案**

这种情况，主要是准备一个分布式锁，大家去抢锁，抢到锁就做set操作。
 加锁的目的实际上就是把并行读写改成串行读写的方式，从而来避免资源竞争。

**2.Redis分布式锁的实现**

主要用到的redis函数是setnx()

**用SETNX实现分布式锁**

利用SETNX非常简单地实现分布式锁。例如：某客户端要获得一个名字youzhi的锁，客户端使用下面的命令进行获取：
 SETNX lock.youzhi<current Unix time + lock timeout + 1>

- 如返回1，则该客户端获得锁，把lock.youzhi的键值设置为时间值表示该键已被锁定，该客户端最后可以通过DEL lock.foo来释放该锁。
- 如返回0，表明该锁已被其他客户端取得，这时我们可以先返回或进行重试等对方完成或等待锁超时。

**2.时间戳**

由于上面举的例子，要求key的操作需要顺序执行，所以需要保存一个时间戳判断set顺序。

```
系统A key 1 {ValueA 7:00}
系统B key 1 { ValueB 7:05}
复制代码
```

假设系统B先抢到锁，将key1设置为{ValueB 7:05}。接下来系统A抢到锁，发现自己的key1的时间戳早于缓存中的时间戳（7:00<7:05），那就不做set操作了。

**3.什么是分布式锁**

因为传统的加锁的做法（如java的synchronized和Lock）这里没用，只适合单点。因为这是分布式环境，需要的是分布式锁。
 当然，分布式锁可以基于很多种方式实现，比如zookeeper、redis等，不管哪种方式实现，基本原理是不变的：用一个状态值表示锁，对锁的占用和释放通过状态值来标识。

**03. 第二种方案：利用消息队列**

在**并发量过大的情况下,可以通过消息中间件进行处理,把并行读写进行串行化。**
 把Redis.set操作放在队列中使其串行化,必须的一个一个执行。

这种方式在一些高并发的场景中算是一种通用的解决方案。

# 缓存数据库保证双写一致

一般来说，如果允许缓存可以稍微的跟数据库偶尔有不一致的情况，也就是说如果你的系统**不是严格要求** “缓存+数据库” 必须保持一致性的话，最好不要做这个方案，即：**读请求和写请求串行化**，串到一个**内存队列**里去。

串行化可以保证一定不会出现不一致的情况，但是它也会导致系统的吞吐量大幅度降低，用比正常情况下多几倍的机器去支撑线上的一个请求。

### Cache Aside Pattern

最经典的缓存+数据库读写的模式，就是 Cache Aside Pattern。

- 读的时候，先读缓存，缓存没有的话，就读数据库，然后取出数据后放入缓存，同时返回响应。
- 更新的时候，**先更新数据库，然后再删除缓存**。

**为什么是删除缓存，而不是更新缓存？**

原因很简单，很多时候，在复杂点的缓存场景，缓存不单单是数据库中直接取出来的值。

比如可能更新了某个表的一个字段，然后其对应的缓存，是需要查询另外两个表的数据并进行运算，才能计算出缓存最新的值的。

另外更新缓存的代价有时候是很高的。是不是说，每次修改数据库的时候，都一定要将其对应的缓存更新一份？也许有的场景是这样，但是对于**比较复杂的缓存数据计算的场景**，就不是这样了。如果你频繁修改一个缓存涉及的多个表，缓存也频繁更新。但是问题在于，**这个缓存到底会不会被频繁访问到？**

举个栗子，一个缓存涉及的表的字段，在 1 分钟内就修改了 20 次，或者是 100 次，那么缓存更新 20 次、100 次；但是这个缓存在 1 分钟内只被读取了 1 次，有**大量的冷数据**。实际上，如果你只是删除缓存的话，那么在 1 分钟内，这个缓存不过就重新计算一次而已，开销大幅度降低。**用到缓存才去算缓存。**

其实删除缓存，而不是更新缓存，就是一个 lazy 计算的思想，不要每次都重新做复杂的计算，不管它会不会用到，而是让它到需要被使用的时候再重新计算。像 mybatis，hibernate，都有懒加载思想。查询一个部门，部门带了一个员工的 list，没有必要说每次查询部门，都把里面的 1000 个员工的数据也同时查出来啊。80% 的情况，查这个部门，就只是要访问这个部门的信息就可以了。先查部门，同时要访问里面的员工，那么这个时候只有在你要访问里面的员工的时候，才会去数据库里面查询 1000 个员工。

### 最初级的缓存不一致问题及解决方案

问题：先更新数据库，再删除缓存。如果删除缓存失败了，那么会导致数据库中是新数据，缓存中是旧数据，数据就出现了不一致。

[![redis-junior-inconsistent](https://github.com/doocs/advanced-java/raw/main/docs/high-concurrency/images/redis-junior-inconsistent.png)](https://github.com/doocs/advanced-java/blob/main/docs/high-concurrency/images/redis-junior-inconsistent.png)

**解决思路：先删除缓存，再更新数据库。如果数据库更新失败了，那么数据库中是旧数据，缓存中是空的，那么数据不会不一致。因为读的时候缓存没有，所以去读了数据库中的旧数据，然后更新到缓存中。**

### 比较复杂的数据不一致问题分析

数据发生了变更，先删除了缓存，然后要去修改数据库，此时还没修改。一个请求过来，去读缓存，发现缓存空了，去查询数据库，**查到了修改前的旧数据**，放到了缓存中。随后数据变更的程序完成了数据库的修改。完了，数据库和缓存中的数据不一样了...

**为什么上亿流量高并发场景下，缓存会出现这个问题？**

只有在对一个数据在并发的进行读写的时候，才可能会出现这种问题。其实如果说你的并发量很低的话，特别是读并发很低，每天访问量就 1 万次，那么很少的情况下，会出现刚才描述的那种不一致的场景。但是问题是，如果每天的是上亿的流量，每秒并发读是几万，每秒只要有数据更新的请求，就**可能会出现上述的数据库+缓存不一致的情况**。

**解决方案如下：**

更新数据的时候，根据**数据的唯一标识**，将操作路由之后，发送到一个 jvm 内部队列中。读取数据的时候，如果发现数据不在缓存中，那么将重新执行“读取数据+更新缓存”的操作，根据唯一标识路由之后，也发送到同一个 jvm 内部队列中。

一个队列对应一个工作线程，每个工作线程**串行**拿到对应的操作，然后一条一条的执行。这样的话，一个数据变更的操作，先删除缓存，然后再去更新数据库，但是还没完成更新。此时如果一个读请求过来，没有读到缓存，那么可以先将缓存更新的请求发送到队列中，此时会在队列中积压，然后同步等待缓存更新完成。

这里有一个**优化点**，一个队列中，其实**多个更新缓存请求串在一起是没意义的**，因此可以做过滤，如果发现队列中已经有一个更新缓存的请求了，那么就不用再放个更新请求操作进去了，直接等待前面的更新操作请求完成即可。

待那个队列对应的工作线程完成了上一个操作的数据库的修改之后，才会去执行下一个操作，也就是缓存更新的操作，此时会从数据库中读取最新的值，然后写入缓存中。

如果请求还在等待时间范围内，不断轮询发现可以取到值了，那么就直接返回；如果请求等待的时间超过一定时长，那么这一次直接从数据库中读取当前的旧值。

# Spring中IOC和AOP

一.控制反转与依赖注入

例子：现在有一个类A，现在B,C,D需要引用这个类的实例化对象。

假如不用IoC容器依赖注入，我们的思路是在B,C,D中分别创造一个A新的实例化对象a，a，a，也能获得我们想要的对象，但是会有几个问题。

1.重复代码变多。这点很容易理解

2.对象太多，管理起来比较麻烦，也比较占内存。

3.最重要的一点，代码之间耦合度较高，不利于维护。这点听起来可能很难理解。假如A类中有2个构造方法，1个是无参构造，1个是全参构造，之前在B,C,D中用的对象a都是由空参构造产生的。现在由于业务发生改变，需要的a对象也全要改变成全参构造的对象。想要完成这个改变，需要在B,C,D中创建a对象时全部改为全参构造，假如不改，那么程序一定无法完成业务。这点相信大家一定知道。

那么来看一下spring的IoC是如何完成这一步的？

我们只需要把放入容器中的A类的实例化对象a，改为全参构造产生的a，在B,C,D类中什么都不需要改。程序运行的时候，会动态地从IoC容器中获取a对象且别注入B,C,D中，也就是只需要修改放入容器的a对象即可。这就达到了降低耦合性，便于维护的效果。

二.面向切面

同样的道理，假如A类有方法a，B,C,D类依赖方法a。如果不使用面向切面，那么常用的做法是在B,C,D中分别创建A的实例化对象，然后调用a方法。

同样的，代码间的耦合度太高，重复代码增多，且不利于维护。假如此时a方法的名字发生改变，或者a方法不再适用，改为b方法，那么A,B,C,D中每个类都需要更改。

而使用spring的面向切面，则只需要更改A类中的a方法即可。B,C,D中没有a方法，只是在程序运行的时候，动态地加载A中的a方法。只要A中的数据变化，B,C,D就会自动一起发生改变。维护起来大大方便，同时也减少了大量的重复代码。

# Spring的单例Bean的线程安全问题

1、有状态的bean与无状态的bean
有状态bean：每个用户有自己特有的一个实例，在用户的生存期内，bean保存了用户的信息，即有状态；一旦用户灭亡（调用结束或实例结束），bean的生命期也告结束。即每个用户最初都会得到一个初始的bean。

无状态bean：bean一旦实例化就被加进会话池中，各个用户都可以共用。即使用户已经消亡，bean的生命期也不一定结束，它可能依然存在于会话池中，供其他用户调用。由于没有特定的用户，那么也就不能保持某一用户的状态，所以叫无状态bean。但无状态会话bean 并非没有状态，如果它有自己的属性（变量）。

有状态就是有数据存储功能。有状态对象(Stateful Bean)，就是有实例变量的对象 ，可以保存数据，是非线程安全的。在不同方法调用间不保留任何状态。

无状态就是一次操作不能保存数据。无状态对象(Stateless Bean)，就是没有实例变量的对象 ，不能保存数据是不变类，是线程安全的。

在Spring的Bean配置中，存在这样两种情况：

<bean id="testManager" class="com.sw.TestManagerImpl" scope="singleton" />  
<bean id="testManager" class="com.sw.TestManagerImpl" scope="prototype" /> 
1
2
当然，scope的值不止这两种，还包括了request、session 等。但用的最多的还是singleton单态与prototype多态。

singleton表示该bean全局只有一个实例，Spring中bean的scope默认也是singleton。

prototype表示该bean在每次被注入的时候，都要重新创建一个实例，这种情况适用于有状态的Bean。

下面是一个有状态的Bean示例

```java
public class TestManagerImpl implements TestManager {  
	private User user;
	public void deleteUser(User e) throws Exception {  
        user = e;	//1
        prepareData(e);
    }

    public void prepareData(User e) throws Exception {  
    	user = getUserByID(e.getId()); 	//2
    	//使用user.getId();				//3
    }

}
```


如果该Bean配置为singleton，会出现什么样的状况呢？
如果有2个用户访问，都调用到了该Bean，假定为user1、user2。

当user1调用到程序中的步骤1的时候，该Bean的私有变量user被赋值为user1，当user1的程序走到步骤2的时候，该Bean的私有变量user被重新赋值为user1_create，理想的状况，当user1走到3步骤的时候，私有变量user应该为user1_create;但如果在user1调用到3步骤之前，user2开始运行到了步骤1了，由于单态的资源共享，则私有变量user被修改为user2；这种情况下，user1的步骤3用到的user.getId()实际用到是user2的对象。

即将有状态的bean配置成singleton会造成资源混乱问题（线程安全问题），而如果是prototype的话，就不会出现资源共享的问题，即不会出现线程安全的问题。

注：如果你的代码所在的进程中有多个线程在同时运行，而这些线程可能会同时运行这段代码。**如果每次运行结果和单线程运行的结果是一样的，而且其他的变量的值也和预期的是一样的，那么代码就是线程安全的。**

通过上面分析，大家已经对有状态和无状态有了一定的理解。无状态的Bean适合用不变模式，技术就是单例模式，这样可以共享实例，提高性能。有状态的Bean，多线程环境下不安全，那么适合用Prototype原型模式（解决多线程问题），每次对bean的请求都会创建一个新的bean实例。

2、Spring中的单例
Spring中的单例与设计模式里面的单例略有不同，设计模式的单例是在整个应用中只有一个实例，而Spring中的单例是在一个IOC容器中就只有一个实例。

大多数时候客户端都在访问我们应用中的业务对象，为了减少并发控制，在这个时候我们不应该在业务对象中设置那些容易造成出错的成员变量。在并发访问时候，这些成员变量将会是并发线程中的共享对象，那么这个时候就会出现意外情况。

成员变量的解决方式：

方法的参数局部变量（在方法中new）
使用Threadlocal
设置bean的scope=prototype
3、Spring使用ThreadLocal解决线程安全问题案例
Spring作为一个IOC容器，帮助我们管理了许许多多的bean。但其实，Spring并没有保证这些对象的线程安全，需要由开发者自己编写解决线程安全问题的代码。

在使用Spring时，很多人可能对Spring中为什么DAO和Service对象采用单实例方式很迷惑，这些读者是这么认为的。

DAO对象必须包含一个数据库的连接Connection，而这个Connection不是线程安全的，所以每个DAO都要包含一个不同的Connection对象实例，这样一来DAO对象就不能是单实例的了。

上述观点对了一半。对的是“每个DAO都要包含一个不同的Connection对象实例”这句话，错的是“DAO对象就不能是单实例”。其实Spring在实现Service和DAO对象时，使用了ThreadLocal这个类，这个是一切的核心！

ThreadLocal

每个线程中都有一个自己的ThreadLocalMap类对象，可以将线程自己的对象保持到其中，各管各的，线程可以正确的访问到自己的对象。
将一个共用的ThreadLocal静态实例作为key，将不同对象的引用保存到不同线程的ThreadLocalMap中，然后在线程执行的各处通过这个静态ThreadLocal实例的get()方法取得自己线程保存的那个对象，避免了将这个对象作为参数传递的麻烦。
一般的Web应用划分为展现层、服务层和持久层三个层次，在不同的层中编写对应的逻辑，下层通过接口向上层开放功能调用。在一般情况下，从接收请求到返回响应所经过的所有程序调用都同属于一个线程。这样你就可以根据需要，将一些非线程安全的变量以ThreadLocal存放，在同一次请求响应的调用线程中，所有关联的对象引用到的都是同一个变量。

下面的实例能够体现Spring对有状态Bean的改造思路：

```java
public class TopicDao {
	//①一个非线程安全的变量
　　private Connection conn;
	//②引用非线程安全变量
　　public void addTopic() {
		Statement stat = conn.createStatement();
　　}
}
```


由于①处的conn是成员变量，因为addTopic()方法是非线程安全的，必须在使用时创建一个新TopicDao实例（非singleton）。下面使用ThreadLocal对conn这个非线程安全的状态进行改造：

```java
public class TopicDao {  
    //①使用ThreadLocal保存Connection变量  
    private static ThreadLocal <Connection> connThreadLocal = new ThreadLocal<Connection>();  
    public static Connection getConnection() {  
       // ②如果connThreadLocal没有本线程对应的Connection创建一个新的Connection，  
       // 并将其保存到线程本地变量中。  
       if (connThreadLocal.get() == null) {  
           Connection conn = getConnection();  
           connThreadLocal.set(conn);  
           return conn;  
       }
       // ③直接返回线程本地变量
       return connThreadLocal.get();  
    }

    public void addTopic() {  
       // ④从ThreadLocal中获取线程对应的Connection  
       try {
           Statement stat = getConnection().createStatement();  
       } catch (SQLException e) {  
           e.printStackTrace();  
       }
    }

}
```


不同的线程在使用TopicDao时，先判断connThreadLocal是否是null，如果是null，则说明当前线程还没有对应的Connection对象，这时创建一个Connection对象并添加到本地线程变量中；如果不为null，则说明当前的线程已经拥有了Connection对象，直接使用就可以了。这样，就保证了不同的线程使用线程相关的Connection，而不会使用其它线程的Connection。因此，这个TopicDao就可以做到singleton共享了。

Spring中DAO和Service都是以单实例的bean形式存在，Spring通过ThreadLocal类将有状态的变量（例如数据库连接Connection）本地线程化，从而做到多线程状况下的安全。在一次请求响应的处理线程中， 该线程贯通展示、服务、数据持久化三层，通过ThreadLocal使得所有关联的对象引用到的都是同一个变量。

当然，这个例子本身很粗糙，将Connection的ThreadLocal直接放在DAO只能做到本DAO的多个方法共享Connection时不发生线程安全问题，但无法和其它DAO共用同一个Connection，要做到同一事务多DAO共享同一Connection，必须在一个共同的外部类使用ThreadLocal保存Connection。

Web应用划分为展现层、服务层和持久层，controller中引入xxxService作为成员变量，xxxService中又引入xxxDao作为成员变量，这些对象都是单例而且会被多个线程并发访问，可我们访问的是它们里面的方法，这些类里面通常不会含有成员变量，dao实例是在MyBatis等ORM框架里面封装好的，已经被测试，不会出现线程同步问题了。所以出问题的地方就是我们自己系统里面的业务对象，所以我们一定要注意自定义的业务对象里面千万不能出现独立成员变量，否则会有线程安全问题。

通常我们在应用中的业务对象如下例子，controller中拥有成员变量list和paperService。

```java
public class TestPaperController extends BaseController {

	private static final int list = 0;
	
	@Autowired
	@Qualifier("papersService")
	private TestPaperService papersService ;
	
	public Page queryPaper(int pageSize, int page,TestPaper paper) throws EicException {
	  RowSelection localRowSelection = getRowSelection(pageSize, page);
	  List<TestPaper> paperList = papersService.queryPaper(paper,localRowSelection);
	  Page localPage = new Page(page, localRowSelection.getTotalRows(), paperList);
	  return localPage;
	}

}
```


service里面又引入了成员变量ibatisEntityDao

```java
@SuppressWarnings("unchecked")
@Service("papersService")
@Transactional(rollbackFor = {Exception.class})
public class TestPaperServiceImpl implements TestPaperService {

	@Autowired
	@Qualifier("ibatisEntityDao")
	private IbatisEntityDao ibatisEntityDao;
	
	private static final String NAMESPACE_TESTPAPER = "com.its.exam.testpaper.model.TestPaper";
	
	private static final String BO_NAME[] = { "试卷仓库" };
	
	private static final String BO_NAME2[] = { "试卷配置试题" };
	
	private static final String BO_NAME1[] = { "试卷试题类型" };
	
	private static final String NAMESPACE_TESTQUESTION="com.its.exam.testpaper.model.TestQuestion";
	
	public List<TestPaper> queryPaper(TestPaper paper,RowSelection paramRowSelection) throws EicException {
	  try {
	   return (List<TestPaper>) ibatisEntityDao.queryForListWithPage(NAMESPACE_TESTPAPER, "queryPaper", paper,paramRowSelection);
	  } catch (Exception e) {
	   e.printStackTrace();
	   throw new EicException(e, "eic", "0001", BO_NAME);
	  }
	}

}
```


由上面例子可以看出，虽然我们这个应用里面含有成员变量，但是并不会出现线程同步方面的问题，controller里面的成员变量papersService被注入后，是为了访问该service类的方法，papersService里面注入的成员变量ibatisEntityDao是ORM框架封装好的，其线程同步问题已解决。

3、ThreadLocal与线程同步机制的比较
ThreadLocal和线程同步机制相比有什么优势呢？ThreadLocal和线程同步机制都是为了解决多线程中相同变量的访问冲突问题。

在同步机制中，通过对象的锁机制保证同一时间只有一个线程访问变量。这时该变量是多个线程共享的，使用同步机制要求程序慎密地分析什么时候对变量进行读写，什么时候需要锁定某个对象，什么时候释放对象锁等繁杂的问题，程序设计和编写难度相对较大。

而ThreadLocal则从另一个角度来解决多线程的并发访问。ThreadLocal会为每一个线程提供一个独立的变量副本，从而隔离了多个线程对数据的访问冲突。因为每一个线程都拥有自己的变量副本，从而也就没有必要对该变量进行同步了。ThreadLocal提供了线程安全的共享对象，在编写多线程代码时，可以把不安全的变量封装进ThreadLocal。

由于ThreadLocal中可以持有任何类型的对象，低版本JDK所提供的get()返回的是Object对象，需要强制类型转换。但JDK 5.0通过泛型很好的解决了这个问题，在一定程度地简化ThreadLocal的使用。

**概括起来说，对于多线程资源共享的问题，同步机制采用了以时间换空间”的方式，而ThreadLocal采用了以空间换时间的方式。前者仅提供一份变量，让不同的线程排队访问，而后者为每一个线程都提供了一份变量，因此可以同时访问而互不影响。**

ThreadLocal是解决线程安全问题一个很好的思路，它通过为每个线程提供一个独立的变量副本解决了变量并发访问的冲突问题。在很多情况下，ThreadLocal比直接使用synchronized同步机制解决线程安全问题更简单，更方便，且结果程序拥有更高的并发性。

下面总结一下：

　　1、在@Controller/@Service等容器中，默认情况下，scope值是单例-singleton的，也是线程不安全的。
　　2、尽量不要在@Controller/@Service等容器中定义静态变量，不论是单例(singleton)还是多实例(prototype)他都是线程不安全的。
　　3、默认注入的Bean对象，在不设置scope的时候他也是线程不安全的。
　　4、一定要定义变量的话，用ThreadLocal来封装，这个是线程安全的。

————————————————
版权声明：本文为CSDN博主「fuzhongmin05」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/fuzhongmin05/article/details/100849867

# Spring中Bean的生命周期

Spring Bean的生命周期是Spring面试热点问题。这个问题即考察对Spring的微观了解，又考察对Spring的宏观认识，想要答好并不容易！本文希望能够从源码角度入手，帮助面试者彻底搞定Spring Bean的生命周期。

### 只有四个！

是的，Spring Bean的生命周期只有这四个阶段。把这四个阶段和每个阶段对应的扩展点糅合在一起虽然没有问题，但是这样非常凌乱，难以记忆。要彻底搞清楚Spring的生命周期，首先要把这四个阶段牢牢记住。实例化和属性赋值对应构造方法和setter方法的注入，初始化和销毁是用户能自定义扩展的两个阶段。在这四步之间穿插的各种扩展点，稍后会讲。

1. 实例化 Instantiation
2. 属性赋值 Populate
3. 初始化 Initialization
4. 销毁 Destruction

实例化 -> 属性赋值 -> 初始化 -> 销毁

主要逻辑都在doCreate()方法中，逻辑很清晰，就是顺序调用以下三个方法，这三个方法与三个生命周期阶段一一对应，非常重要，在后续扩展接口分析中也会涉及。

1. createBeanInstance() -> 实例化
2. populateBean() -> 属性赋值
3. initializeBean() -> 初始化

源码如下，能证明实例化，属性赋值和初始化这三个生命周期的存在。关于本文的Spring源码都将忽略无关部分，便于理解：



```dart
// 忽略了无关代码
protected Object doCreateBean(final String beanName, final RootBeanDefinition mbd, final @Nullable Object[] args)
      throws BeanCreationException {

   // Instantiate the bean.
   BeanWrapper instanceWrapper = null;
   if (instanceWrapper == null) {
       // 实例化阶段！
      instanceWrapper = createBeanInstance(beanName, mbd, args);
   }

   // Initialize the bean instance.
   Object exposedObject = bean;
   try {
       // 属性赋值阶段！
      populateBean(beanName, mbd, instanceWrapper);
       // 初始化阶段！
      exposedObject = initializeBean(beanName, exposedObject, mbd);
   }

   
   }
```

至于销毁，是在容器关闭时调用的，详见`ConfigurableApplicationContext#close()`

### 常用扩展点

Spring生命周期相关的常用扩展点非常多，所以问题不是不知道，而是记不住或者记不牢。其实记不住的根本原因还是不够了解，这里通过源码+分类的方式帮大家记忆。

#### 第一大类：影响多个Bean的接口

实现了这些接口的Bean会切入到多个Bean的生命周期中。正因为如此，这些接口的功能非常强大，Spring内部扩展也经常使用这些接口，例如自动注入以及AOP的实现都和他们有关。

- BeanPostProcessor
- InstantiationAwareBeanPostProcessor

这两兄弟可能是Spring扩展中**最重要**的两个接口！InstantiationAwareBeanPostProcessor作用于**实例化**阶段的前后，BeanPostProcessor作用于**初始化**阶段的前后。正好和第一、第三个生命周期阶段对应。通过图能更好理解：

![img](https:////upload-images.jianshu.io/upload_images/4558491-dc3eebbd1d6c65f4.png?imageMogr2/auto-orient/strip|imageView2/2/w/823/format/webp)

未命名文件 (1).png



InstantiationAwareBeanPostProcessor实际上继承了BeanPostProcessor接口，严格意义上来看他们不是两兄弟，而是两父子。但是从生命周期角度我们重点关注其特有的对实例化阶段的影响，图中省略了从BeanPostProcessor继承的方法。



```dart
InstantiationAwareBeanPostProcessor extends BeanPostProcessor
```

##### InstantiationAwareBeanPostProcessor源码分析：

- postProcessBeforeInstantiation调用点，忽略无关代码：



```tsx
@Override
    protected Object createBean(String beanName, RootBeanDefinition mbd, @Nullable Object[] args)
            throws BeanCreationException {

        try {
            // Give BeanPostProcessors a chance to return a proxy instead of the target bean instance.
            // postProcessBeforeInstantiation方法调用点，这里就不跟进了，
            // 有兴趣的同学可以自己看下，就是for循环调用所有的InstantiationAwareBeanPostProcessor
            Object bean = resolveBeforeInstantiation(beanName, mbdToUse);
            if (bean != null) {
                return bean;
            }
        }
        
        try {   
            // 上文提到的doCreateBean方法，可以看到
            // postProcessBeforeInstantiation方法在创建Bean之前调用
            Object beanInstance = doCreateBean(beanName, mbdToUse, args);
            if (logger.isTraceEnabled()) {
                logger.trace("Finished creating instance of bean '" + beanName + "'");
            }
            return beanInstance;
        }
        
    }
```

可以看到，postProcessBeforeInstantiation在doCreateBean之前调用，也就是在bean实例化之前调用的，英文源码注释解释道该方法的返回值会替换原本的Bean作为代理，这也是Aop等功能实现的关键点。

- postProcessAfterInstantiation调用点，忽略无关代码：



```tsx
protected void populateBean(String beanName, RootBeanDefinition mbd, @Nullable BeanWrapper bw) {

   // Give any InstantiationAwareBeanPostProcessors the opportunity to modify the
   // state of the bean before properties are set. This can be used, for example,
   // to support styles of field injection.
   boolean continueWithPropertyPopulation = true;
    // InstantiationAwareBeanPostProcessor#postProcessAfterInstantiation()
    // 方法作为属性赋值的前置检查条件，在属性赋值之前执行，能够影响是否进行属性赋值！
   if (!mbd.isSynthetic() && hasInstantiationAwareBeanPostProcessors()) {
      for (BeanPostProcessor bp : getBeanPostProcessors()) {
         if (bp instanceof InstantiationAwareBeanPostProcessor) {
            InstantiationAwareBeanPostProcessor ibp = (InstantiationAwareBeanPostProcessor) bp;
            if (!ibp.postProcessAfterInstantiation(bw.getWrappedInstance(), beanName)) {
               continueWithPropertyPopulation = false;
               break;
            }
         }
      }
   }

   // 忽略后续的属性赋值操作代码
}
```

可以看到该方法在属性赋值方法内，但是在真正执行赋值操作之前。其返回值为boolean，返回false时可以阻断属性赋值阶段（`continueWithPropertyPopulation = false;`）。

关于BeanPostProcessor执行阶段的源码穿插在下文Aware接口的调用时机分析中，因为部分Aware功能的就是通过他实现的!只需要先记住BeanPostProcessor在初始化前后调用就可以了。

#### 第二大类：只调用一次的接口

这一大类接口的特点是功能丰富，常用于用户自定义扩展。
 第二大类中又可以分为两类：

1. Aware类型的接口
2. 生命周期接口

##### 无所不知的Aware

Aware类型的接口的作用就是让我们能够拿到Spring容器中的一些资源。基本都能够见名知意，Aware之前的名字就是可以拿到什么资源，例如`BeanNameAware`可以拿到BeanName，以此类推。调用时机需要注意：所有的Aware方法都是在初始化阶段之前调用的！
 Aware接口众多，这里同样通过分类的方式帮助大家记忆。
 Aware接口具体可以分为两组，至于为什么这么分，详见下面的源码分析。如下排列顺序同样也是Aware接口的执行顺序，能够见名知意的接口不再解释。

```
Aware Group1
```

1. BeanNameAware
2. BeanClassLoaderAware
3. BeanFactoryAware

```
Aware Group2
```

1. EnvironmentAware
2. EmbeddedValueResolverAware 这个知道的人可能不多，实现该接口能够获取Spring EL解析器，用户的自定义注解需要支持spel表达式的时候可以使用，非常方便。
3. ApplicationContextAware(ResourceLoaderAware\ApplicationEventPublisherAware\MessageSourceAware) 这几个接口可能让人有点懵，实际上这几个接口可以一起记，其返回值实质上都是当前的ApplicationContext对象，因为ApplicationContext是一个复合接口，如下：



```kotlin
public interface ApplicationContext extends EnvironmentCapable, ListableBeanFactory, HierarchicalBeanFactory,
        MessageSource, ApplicationEventPublisher, ResourcePatternResolver {}
```

这里涉及到另一道面试题，ApplicationContext和BeanFactory的区别，可以从ApplicationContext继承的这几个接口入手，除去BeanFactory相关的两个接口就是ApplicationContext独有的功能，这里不详细说明。

##### Aware调用时机源码分析

详情如下，忽略了部分无关代码。代码位置就是我们上文提到的initializeBean方法详情，这也说明了Aware都是在初始化阶段之前调用的！



```dart
    // 见名知意，初始化阶段调用的方法
    protected Object initializeBean(final String beanName, final Object bean, @Nullable RootBeanDefinition mbd) {

        // 这里调用的是Group1中的三个Bean开头的Aware
        invokeAwareMethods(beanName, bean);

        Object wrappedBean = bean;
        
        // 这里调用的是Group2中的几个Aware，
        // 而实质上这里就是前面所说的BeanPostProcessor的调用点！
        // 也就是说与Group1中的Aware不同，这里是通过BeanPostProcessor（ApplicationContextAwareProcessor）实现的。
        wrappedBean = applyBeanPostProcessorsBeforeInitialization(wrappedBean, beanName);
        // 下文即将介绍的InitializingBean调用点
        invokeInitMethods(beanName, wrappedBean, mbd);
        // BeanPostProcessor的另一个调用点
        wrappedBean = applyBeanPostProcessorsAfterInitialization(wrappedBean, beanName);

        return wrappedBean;
    }
```

可以看到并不是所有的Aware接口都使用同样的方式调用。Bean××Aware都是在代码中直接调用的，而ApplicationContext相关的Aware都是通过BeanPostProcessor#postProcessBeforeInitialization()实现的。感兴趣的可以自己看一下ApplicationContextAwareProcessor这个类的源码，就是判断当前创建的Bean是否实现了相关的Aware方法，如果实现了会调用回调方法将资源传递给Bean。
 至于Spring为什么这么实现，应该没什么特殊的考量。也许和Spring的版本升级有关。基于对修改关闭，对扩展开放的原则，Spring对一些新的Aware采用了扩展的方式添加。

BeanPostProcessor的调用时机也能在这里体现，包围住invokeInitMethods方法，也就说明了在初始化阶段的前后执行。

关于Aware接口的执行顺序，其实只需要记住第一组在第二组执行之前就行了。每组中各个Aware方法的调用顺序其实没有必要记，有需要的时候点进源码一看便知。

##### 简单的两个生命周期接口

至于剩下的两个生命周期接口就很简单了，实例化和属性赋值都是Spring帮助我们做的，能够自己实现的有初始化和销毁两个生命周期阶段。

1. InitializingBean 对应生命周期的初始化阶段，在上面源码的`invokeInitMethods(beanName, wrappedBean, mbd);`方法中调用。
    有一点需要注意，因为Aware方法都是执行在初始化方法之前，所以可以在初始化方法中放心大胆的使用Aware接口获取的资源，这也是我们自定义扩展Spring的常用方式。
    除了实现InitializingBean接口之外还能通过注解或者xml配置的方式指定初始化方法，至于这几种定义方式的调用顺序其实没有必要记。因为这几个方法对应的都是同一个生命周期，只是实现方式不同，我们一般只采用其中一种方式。
2. DisposableBean 类似于InitializingBean，对应生命周期的销毁阶段，以ConfigurableApplicationContext#close()方法作为入口，实现是通过循环取所有实现了DisposableBean接口的Bean然后调用其destroy()方法 。感兴趣的可以自行跟一下源码。

### 扩展阅读: BeanPostProcessor 注册时机与执行顺序

##### 注册时机

我们知道BeanPostProcessor也会注册为Bean，那么Spring是如何保证BeanPostProcessor在我们的业务Bean之前初始化完成呢？
 请看我们熟悉的refresh()方法的源码，省略部分无关代码：



```java
@Override
    public void refresh() throws BeansException, IllegalStateException {
        synchronized (this.startupShutdownMonitor) {

            try {
                // Allows post-processing of the bean factory in context subclasses.
                postProcessBeanFactory(beanFactory);

                // Invoke factory processors registered as beans in the context.
                invokeBeanFactoryPostProcessors(beanFactory);

                // Register bean processors that intercept bean creation.
                // 所有BeanPostProcesser初始化的调用点
                registerBeanPostProcessors(beanFactory);

                // Initialize message source for this context.
                initMessageSource();

                // Initialize event multicaster for this context.
                initApplicationEventMulticaster();

                // Initialize other special beans in specific context subclasses.
                onRefresh();

                // Check for listener beans and register them.
                registerListeners();

                // Instantiate all remaining (non-lazy-init) singletons.
                // 所有单例非懒加载Bean的调用点
                finishBeanFactoryInitialization(beanFactory);

                // Last step: publish corresponding event.
                finishRefresh();
            }

    }
```

可以看出，Spring是先执行registerBeanPostProcessors()进行BeanPostProcessors的注册，然后再执行finishBeanFactoryInitialization初始化我们的单例非懒加载的Bean。

##### 执行顺序

BeanPostProcessor有很多个，而且每个BeanPostProcessor都影响多个Bean，其执行顺序至关重要，必须能够控制其执行顺序才行。关于执行顺序这里需要引入两个排序相关的接口：PriorityOrdered、Ordered

- PriorityOrdered是一等公民，首先被执行，PriorityOrdered公民之间通过接口返回值排序
- Ordered是二等公民，然后执行，Ordered公民之间通过接口返回值排序
- 都没有实现是三等公民，最后执行

在以下源码中，可以很清晰的看到Spring注册各种类型BeanPostProcessor的逻辑，根据实现不同排序接口进行分组。优先级高的先加入，优先级低的后加入。



```kotlin
// First, invoke the BeanDefinitionRegistryPostProcessors that implement PriorityOrdered.
// 首先，加入实现了PriorityOrdered接口的BeanPostProcessors，顺便根据PriorityOrdered排了序
            String[] postProcessorNames =
                    beanFactory.getBeanNamesForType(BeanDefinitionRegistryPostProcessor.class, true, false);
            for (String ppName : postProcessorNames) {
                if (beanFactory.isTypeMatch(ppName, PriorityOrdered.class)) {
                    currentRegistryProcessors.add(beanFactory.getBean(ppName, BeanDefinitionRegistryPostProcessor.class));
                    processedBeans.add(ppName);
                }
            }
            sortPostProcessors(currentRegistryProcessors, beanFactory);
            registryProcessors.addAll(currentRegistryProcessors);
            invokeBeanDefinitionRegistryPostProcessors(currentRegistryProcessors, registry);
            currentRegistryProcessors.clear();

            // Next, invoke the BeanDefinitionRegistryPostProcessors that implement Ordered.
// 然后，加入实现了Ordered接口的BeanPostProcessors，顺便根据Ordered排了序
            postProcessorNames = beanFactory.getBeanNamesForType(BeanDefinitionRegistryPostProcessor.class, true, false);
            for (String ppName : postProcessorNames) {
                if (!processedBeans.contains(ppName) && beanFactory.isTypeMatch(ppName, Ordered.class)) {
                    currentRegistryProcessors.add(beanFactory.getBean(ppName, BeanDefinitionRegistryPostProcessor.class));
                    processedBeans.add(ppName);
                }
            }
            sortPostProcessors(currentRegistryProcessors, beanFactory);
            registryProcessors.addAll(currentRegistryProcessors);
            invokeBeanDefinitionRegistryPostProcessors(currentRegistryProcessors, registry);
            currentRegistryProcessors.clear();

            // Finally, invoke all other BeanDefinitionRegistryPostProcessors until no further ones appear.
// 最后加入其他常规的BeanPostProcessors
            boolean reiterate = true;
            while (reiterate) {
                reiterate = false;
                postProcessorNames = beanFactory.getBeanNamesForType(BeanDefinitionRegistryPostProcessor.class, true, false);
                for (String ppName : postProcessorNames) {
                    if (!processedBeans.contains(ppName)) {
                        currentRegistryProcessors.add(beanFactory.getBean(ppName, BeanDefinitionRegistryPostProcessor.class));
                        processedBeans.add(ppName);
                        reiterate = true;
                    }
                }
                sortPostProcessors(currentRegistryProcessors, beanFactory);
                registryProcessors.addAll(currentRegistryProcessors);
                invokeBeanDefinitionRegistryPostProcessors(currentRegistryProcessors, registry);
                currentRegistryProcessors.clear();
            }
```

根据排序接口返回值排序，默认升序排序，返回值越低优先级越高。



```dart
    /**
     * Useful constant for the highest precedence value.
     * @see java.lang.Integer#MIN_VALUE
     */
    int HIGHEST_PRECEDENCE = Integer.MIN_VALUE;

    /**
     * Useful constant for the lowest precedence value.
     * @see java.lang.Integer#MAX_VALUE
     */
    int LOWEST_PRECEDENCE = Integer.MAX_VALUE;
```

PriorityOrdered、Ordered接口作为Spring整个框架通用的排序接口，在Spring中应用广泛，也是非常重要的接口。

### 总结

Spring Bean的生命周期分为`四个阶段`和`多个扩展点`。扩展点又可以分为`影响多个Bean`和`影响单个Bean`。整理如下：
 四个阶段

- 实例化 Instantiation
- 属性赋值 Populate
- 初始化 Initialization
- 销毁 Destruction

多个扩展点

- 影响多个Bean
  - BeanPostProcessor
  - InstantiationAwareBeanPostProcessor
- 影响单个Bean
  - Aware
    - Aware Group1
      - BeanNameAware
      - BeanClassLoaderAware
      - BeanFactoryAware
    - Aware Group2
      - EnvironmentAware
      - EmbeddedValueResolverAware
      - ApplicationContextAware(ResourceLoaderAware\ApplicationEventPublisherAware\MessageSourceAware)
  - 生命周期
    - InitializingBean
    - DisposableBean

至此，Spring Bean的生命周期介绍完毕，由于作者水平有限难免有疏漏，欢迎留言纠错。



作者：sunshujie1990
链接：https://www.jianshu.com/p/1dec08d290c1
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

# SpringMVC常见面试题

1、什么是Spring MVC ？简单介绍下你对springMVC的理解?

Spring MVC是一个基于Java的实现了MVC设计模式的请求驱动类型的轻量级Web框架，通过把Model，View，Controller分离，将web层进行职责解耦，把复杂的web应用分成逻辑清晰的几部分，简化开发，减少出错，方便组内开发人员之间的配合。

 

2、SpringMVC的流程？

![img](https://img-blog.csdn.net/20180708224853769?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2E3NDUyMzM3MDA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

（1）用户发送请求至前端控制器DispatcherServlet；
（2）DispatcherServlet收到请求后，调用HandlerMapping处理器映射器，请求获取Handler；
（3）处理器映射器根据请求url找到具体的处理器Handler，生成处理器对象及处理器拦截器(如果有则生成)，一并返回给DispatcherServlet；
（4）DispatcherServlet 调用 HandlerAdapter处理器适配器，请求执行Handler；
（5）HandlerAdapter 经过适配调用 具体处理器进行处理业务逻辑；
（6）Handler执行完成返回ModelAndView；
（7）HandlerAdapter将Handler执行结果ModelAndView返回给DispatcherServlet；
（8）DispatcherServlet将ModelAndView传给ViewResolver视图解析器进行解析；
（9）ViewResolver解析后返回具体View；
（10）DispatcherServlet对View进行渲染视图（即将模型数据填充至视图中）
（11）DispatcherServlet响应用户。

前端控制器 DispatcherServlet：接收请求、响应结果，相当于转发器，有了DispatcherServlet 就减少了其它组件之间的耦合度。
处理器映射器 HandlerMapping：根据请求的URL来查找Handler
处理器适配器 HandlerAdapter：负责执行Handler
处理器 Handler：处理器，需要程序员开发
视图解析器 ViewResolver：进行视图的解析，根据视图逻辑名将ModelAndView解析成真正的视图（view）
视图View：View是一个接口， 它的实现类支持不同的视图类型，如jsp，freemarker，pdf等等


3、Springmvc的优点:

（1）可以支持各种视图技术，而不仅仅局限于JSP；

（2）与Spring框架集成（如IoC容器、AOP等）；

（3）清晰的角色分配：前端控制器(dispatcherServlet) ，请求到处理器映射（handlerMapping)，处理器适配器（HandlerAdapter)，视图解析器（ViewResolver）。

（4） 支持各种请求资源的映射策略。

 

4、SpringMVC怎么样设定重定向和转发的？

（1）转发：在返回值前面加"forward:"，譬如"forward:user.do?name=method4"

（2）重定向：在返回值前面加"redirect:"，譬如"redirect:http://www.baidu.com"

 

5、 SpringMVC常用的注解有哪些？

@RequestMapping：用于处理请求 url 映射的注解，可用于类或方法上。用于类上，则表示类中的所有响应请求的方法都是以该地址作为父路径。

@RequestBody：注解实现接收http请求的json数据，将json转换为java对象。

@ResponseBody：注解实现将conreoller方法返回对象转化为json对象响应给客户。

 

6、SpingMvc中的控制器的注解一般用哪个？有没有别的注解可以替代？

答：一般用@Controller注解，也可以使用@RestController，@RestController注解相当于@ResponseBody ＋ @Controller，表示是表现层，除此之外，一般不用别的注解代替。

 

7、springMVC和struts2的区别有哪些?

（1）springmvc的入口是一个servlet即前端控制器（DispatchServlet），而struts2入口是一个filter过虑器（StrutsPrepareAndExecuteFilter）。

（2）springmvc是基于方法开发(一个url对应一个方法)，请求参数传递到方法的形参，可以设计为单例或多例(建议单例)，struts2是基于类开发，传递参数是通过类的属性，只能设计为多例。

（3）Struts采用值栈存储请求和响应的数据，通过OGNL存取数据，springmvc通过参数解析器是将request请求内容解析，并给方法形参赋值，将数据和视图封装成ModelAndView对象，最后又将ModelAndView中的模型数据通过reques域传输到页面。Jsp视图解析器默认使用jstl。

 

8、如何解决POST请求中文乱码问题，GET的又如何处理呢？

（1）解决post请求乱码问题：在web.xml中配置一个CharacterEncodingFilter过滤器，设置成utf-8；

```xml
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
</filter>

<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```


（2）get请求中文参数出现乱码解决方法有两个：

①修改tomcat配置文件添加编码与工程编码一致，如下：

`<ConnectorURIEncoding="utf-8" connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443"/>`
 ②另外一种方法对参数进行重新编码：

`String userName = new String(request.getParamter("userName").getBytes("ISO8859-1"),"utf-8")`
ISO8859-1是tomcat默认编码，需要将tomcat编码后的内容按utf-8编码。

 

9、SpringMvc里面拦截器是怎么写的：

有两种写法，一种是实现HandlerInterceptor接口，另外一种是继承适配器类，接着在接口方法当中，实现处理逻辑；然后在SpringMvc的配置文件中配置拦截器即可：

```xml
<!-- 配置SpringMvc的拦截器 -->
<mvc:interceptors>
    <!-- 配置一个拦截器的Bean就可以了 默认是对所有请求都拦截 -->
    <bean id="myInterceptor" class="com.zwp.action.MyHandlerInterceptor"></bean>

    <!-- 只针对部分请求拦截 -->
    <mvc:interceptor>
       <mvc:mapping path="/modelMap.do" />
       <bean class="com.zwp.action.MyHandlerInterceptorAdapter" />
    </mvc:interceptor>

</mvc:interceptors>
```


10、注解原理：

注解本质是一个继承了Annotation的特殊接口，其具体实现类是JDK动态代理生成的代理类。我们通过反射获取注解时，返回的也是Java运行时生成的动态代理对象。通过代理对象调用自定义注解的方法，会最终调用AnnotationInvocationHandler的invoke方法，该方法会从memberValues这个Map中查询出对应的值，而memberValues的来源是Java常量池。

 

11、SpringMvc怎么和AJAX相互调用的？

通过Jackson框架就可以把Java里面的对象直接转化成Js可以识别的Json对象。具体步骤如下 ：

（1）加入Jackson.jar

（2）在配置文件中配置json的映射

（3）在接受Ajax方法里面可以直接返回Object、List等，但方法前面要加上@ResponseBody注解。

12、Spring MVC的异常处理 ？

答：可以将异常抛给Spring框架，由Spring框架来处理；我们只需要配置简单的异常处理器，在异常处理器中添视图页面即可。

13、SpringMvc的控制器是不是单例模式？如果是，有什么问题？怎么解决？

答：是单例模式，在多线程访问的时候有线程安全问题，解决方案是在控制器里面不能写可变状态量，如果需要使用这些可变状态，可以使用ThreadLocal机制解决，为每个线程单独生成一份变量副本，独立操作，互不影响。

14、如果在拦截请求中，我想拦截get方式提交的方法，怎么配置？

答：可以在@RequestMapping注解里面加上method=RequestMethod.GET。

16、怎样在方法里面得到Request，或者Session？

答：直接在方法的形参中声明request，SpringMvc就自动把request对象传入。

16、如果想在拦截的方法里面得到从前台传入的参数，怎么得到？

答：直接在形参里面声明这个参数就可以，但必须名字和传过来的参数一样。

17、如果前台有很多个参数传入，并且这些参数都是一个对象的，那么怎么样快速得到这个对象？

答：直接在方法中声明这个对象，SpringMvc就自动会把属性赋值到这个对象里面。

18、SpringMvc中函数的返回值是什么？

答：返回值可以有很多类型，有String，ModelAndView。ModelAndView类把视图和数据都合并的一起的，但一般用String比较好。

19、SpringMvc用什么对象从后台向前台传递数据的？

答：通过ModelMap对象，可以在这个对象里面调用put方法，把对象加到里面，前端就可以通过el表达式拿到。

20、怎么样把ModelMap里面的数据放入Session里面？

答：可以在类上面加上@SessionAttributes注解，里面包含的字符串就是要放入session里面的key。
————————————————
版权声明：本文为CSDN博主「张维鹏」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/a745233700/article/details/80963758

# Spring中用到的设计模式

JDK 中用到了那些设计模式?Spring 中用到了那些设计模式?这两个问题，在面试中比较常见。我在网上搜索了一下关于 Spring 中设计模式的讲解几乎都是千篇一律，而且大部分都年代久远。所以，花了几天时间自己总结了一下，由于我的个人能力有限，文中如有任何错误各位都可以指出。另外，文章篇幅有限，对于设计模式以及一些源码的解读我只是一笔带过，这篇文章的主要目的是回顾一下 Spring 中的常见的设计模式。

Design Patterns(设计模式) 表示面向对象软件开发中最好的计算机编程实践。 Spring 框架中广泛使用了不同类型的设计模式，下面我们来看看到底有哪些设计模式?

## 控制反转(IoC)和依赖注入(DI)

**IoC(Inversion of Control,控制翻转)** 是Spring 中一个非常非常重要的概念，它不是什么技术，而是一种解耦的设计思想。它的主要目的是借助于“第三方”(Spring 中的 IOC 容器) 实现具有依赖关系的对象之间的解耦(IOC容易管理对象，你只管使用即可)，从而降低代码之间的耦合度。**IOC 是一个原则，而不是一个模式，以下模式（但不限于）实现了IoC原则。**

![图片](https://mmbiz.qpic.cn/mmbiz_png/iaIdQfEric9TwBuibJ4N5OTyAvJibFj8b7zhiaTcnFwmnfLqQQWwlWv2uNMZyiabexkUSuW24WAWAuL6cvzguu8JyYzw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)ioc-patterns

**Spring IOC 容器就像是一个工厂一样，当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可，完全不用考虑对象是如何被创建出来的。** IOC 容器负责创建对象，将对象连接在一起，配置这些对象，并从创建中处理这些对象的整个生命周期，直到它们被完全销毁。

在实际项目中一个 Service 类如果有几百甚至上千个类作为它的底层，我们需要实例化这个 Service，你可能要每次都要搞清这个 Service 所有底层类的构造函数，这可能会把人逼疯。如果利用 IOC 的话，你只需要配置好，然后在需要的地方引用就行了，这大大增加了项目的可维护性且降低了开发难度。关于Spring IOC 的理解，推荐看这一下知乎的一个回答：https://www.zhihu.com/question/23277575/answer/169698662 ，非常不错。

**控制翻转怎么理解呢?** 举个例子："对象a 依赖了对象 b，当对象 a 需要使用 对象 b的时候必须自己去创建。但是当系统引入了 IOC 容器后， 对象a 和对象 b 之前就失去了直接的联系。这个时候，当对象 a 需要使用 对象 b的时候， 我们可以指定 IOC 容器去创建一个对象b注入到对象 a 中"。 对象 a 获得依赖对象 b 的过程,由主动行为变为了被动行为，控制权翻转，这就是控制反转名字的由来。

**DI(Dependecy Inject,依赖注入)是实现控制反转的一种设计模式，依赖注入就是将实例变量传入到一个对象中去。**

## 工厂设计模式

Spring使用工厂模式可以通过 `BeanFactory` 或 `ApplicationContext` 创建 bean 对象。

**两者对比：**

- `BeanFactory` ：延迟注入(使用到某个 bean 的时候才会注入),相比于`BeanFactory`来说会占用更少的内存，程序启动速度更快。
- `ApplicationContext` ：容器启动的时候，不管你用没用到，一次性创建所有 bean 。`BeanFactory` 仅提供了最基本的依赖注入支持，`ApplicationContext` 扩展了 `BeanFactory` ,除了有`BeanFactory`的功能还有额外更多功能，所以一般开发人员使用`ApplicationContext`会更多。

ApplicationContext的三个实现类：

1. `ClassPathXmlApplication`：把上下文文件当成类路径资源。
2. `FileSystemXmlApplication`：从文件系统中的 XML 文件载入上下文定义信息。
3. `XmlWebApplicationContext`：从Web系统中的XML文件载入上下文定义信息。

Example:

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.FileSystemXmlApplicationContext;

public class App {
    public static void main(String[] args) {
        ApplicationContext context = new FileSystemXmlApplicationContext(
                "C:/work/IOC Containers/springframework.applicationcontext/src/main/resources/bean-factory-config.xml");

        HelloApplicationContext obj = (HelloApplicationContext) context.getBean("helloApplicationContext");
        obj.getMsg();
    }
}
```

## 单例设计模式

在我们的系统中，有一些对象其实我们只需要一个，比如说：线程池、缓存、对话框、注册表、日志对象、充当打印机、显卡等设备驱动程序的对象。事实上，这一类对象只能有一个实例，如果制造出多个实例就可能会导致一些问题的产生，比如：程序的行为异常、资源使用过量、或者不一致性的结果。

**使用单例模式的好处:**

- 对于频繁使用的对象，可以省略创建对象所花费的时间，这对于那些重量级对象而言，是非常可观的一笔系统开销；
- 由于 new 操作的次数减少，因而对系统内存的使用频率也会降低，这将减轻 GC 压力，缩短 GC 停顿时间。

**Spring 中 bean 的默认作用域就是 singleton(单例)的。** 除了 singleton 作用域，Spring 中 bean 还有下面几种作用域：

- prototype : 每次请求都会创建一个新的 bean 实例。
- request : 每一次HTTP请求都会产生一个新的bean，该bean仅在当前HTTP request内有效。
- session : 每一次HTTP请求都会产生一个新的 bean，该bean仅在当前 HTTP session 内有效。
- global-session： 全局session作用域，仅仅在基于portlet的web应用中才有意义，Spring5已经没有了。Portlet是能够生成语义代码(例如：HTML)片段的小型Java Web插件。它们基于portlet容器，可以像servlet一样处理HTTP请求。但是，与 servlet 不同，每个 portlet 都有不同的会话

**Spring 实现单例的方式：**

- xml:<bean id="userService" class="top.snailclimb.UserService" scope="singleton"/>``
- 注解：`@Scope(value = "singleton")`

Spring 通过 `ConcurrentHashMap` 实现单例注册表的特殊方式实现单例模式。Spring 实现单例的核心代码如下：

```
// 通过 ConcurrentHashMap（线程安全） 实现单例注册表
private final Map<String, Object> singletonObjects = new ConcurrentHashMap<String, Object>(64);

public Object getSingleton(String beanName, ObjectFactory<?> singletonFactory) {
        Assert.notNull(beanName, "'beanName' must not be null");
        synchronized (this.singletonObjects) {
            // 检查缓存中是否存在实例  
            Object singletonObject = this.singletonObjects.get(beanName);
            if (singletonObject == null) {
                //...省略了很多代码
                try {
                    singletonObject = singletonFactory.getObject();
                }
                //...省略了很多代码
                // 如果实例对象在不存在，我们注册到单例注册表中。
                addSingleton(beanName, singletonObject);
            }
            return (singletonObject != NULL_OBJECT ? singletonObject : null);
        }
    }
    //将对象添加到单例注册表
    protected void addSingleton(String beanName, Object singletonObject) {
            synchronized (this.singletonObjects) {
                this.singletonObjects.put(beanName, (singletonObject != null ? singletonObject : NULL_OBJECT));

            }
        }
}
```

## 代理设计模式

### 代理模式在 AOP 中的应用

AOP(Aspect-Oriented Programming:面向切面编程)能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。

**Spring AOP 就是基于动态代理的**，如果要代理的对象，实现了某个接口，那么Spring AOP会使用**JDK Proxy**，去创建代理对象，而对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候Spring AOP会使用**Cglib** ，这时候Spring AOP会使用 **Cglib** 生成一个被代理对象的子类来作为代理，如下图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/iaIdQfEric9TwBuibJ4N5OTyAvJibFj8b7zhPn9m0PpOfqr7exaXPcBL5qJJDiaFibZxVprZjicTJxialjXKjicQrxnOcEA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)SpringAOPProcess

当然你也可以使用 AspectJ ,Spring AOP 已经集成了AspectJ ，AspectJ 应该算的上是 Java 生态系统中最完整的 AOP 框架了。

使用 AOP 之后我们可以把一些通用功能抽象出来，在需要用到的地方直接使用即可，这样大大简化了代码量。我们需要增加新功能时也方便，这样也提高了系统扩展性。日志功能、事务管理等等场景都用到了 AOP 。

### Spring AOP 和 AspectJ AOP 有什么区别?

**Spring AOP 属于运行时增强，而 AspectJ 是编译时增强。** Spring AOP 基于代理(Proxying)，而 AspectJ 基于字节码操作(Bytecode Manipulation)。

Spring AOP 已经集成了 AspectJ ，AspectJ 应该算的上是 Java 生态系统中最完整的 AOP 框架了。AspectJ 相比于 Spring AOP 功能更加强大，但是 Spring AOP 相对来说更简单，

如果我们的切面比较少，那么两者性能差异不大。但是，当切面太多的话，最好选择 AspectJ ，它比Spring AOP 快很多。

## 模板方法

模板方法模式是一种行为设计模式，它定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。 模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤的实现方式。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/iaIdQfEric9TwBuibJ4N5OTyAvJibFj8b7zhD3EHPJsnpY1M86vfnQoqTMCfbPGb5ianFdlBRm6jiahkFTQ8dAfgt10w/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)模板方法UML图

```
public abstract class Template {
    //这是我们的模板方法
    public final void TemplateMethod(){
        PrimitiveOperation1();  
        PrimitiveOperation2();
        PrimitiveOperation3();
    }

    protected void  PrimitiveOperation1(){
        //当前类实现
    }

    //被子类实现的方法
    protected abstract void PrimitiveOperation2();
    protected abstract void PrimitiveOperation3();

}
public class TemplateImpl extends Template {

    @Override
    public void PrimitiveOperation2() {
        //当前类实现
    }

    @Override
    public void PrimitiveOperation3() {
        //当前类实现
    }
}
```

Spring 中 `jdbcTemplate`、`hibernateTemplate` 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。一般情况下，我们都是使用继承的方式来实现模板模式，但是 Spring 并没有使用这种方式，而是使用Callback 模式与模板方法模式配合，既达到了代码复用的效果，同时增加了灵活性。

## 观察者模式

观察者模式是一种对象行为型模式。它表示的是一种对象与对象之间具有依赖关系，当一个对象发生改变的时候，这个对象所依赖的对象也会做出反应。Spring 事件驱动模型就是观察者模式很经典的一个应用。Spring 事件驱动模型非常有用，在很多场景都可以解耦我们的代码。比如我们每次添加商品的时候都需要重新更新商品索引，这个时候就可以利用观察者模式来解决这个问题。

### Spring 事件驱动模型中的三种角色

#### 事件角色

`ApplicationEvent` (`org.springframework.context`包下)充当事件的角色,这是一个抽象类，它继承了`java.util.EventObject`并实现了 `java.io.Serializable`接口。

Spring 中默认存在以下事件，他们都是对 `ApplicationContextEvent` 的实现(继承自`ApplicationContextEvent`)：

- `ContextStartedEvent`：`ApplicationContext` 启动后触发的事件;
- `ContextStoppedEvent`：`ApplicationContext` 停止后触发的事件;
- `ContextRefreshedEvent`：`ApplicationContext` 初始化或刷新完成后触发的事件;
- `ContextClosedEvent`：`ApplicationContext` 关闭后触发的事件。

![图片](https://mmbiz.qpic.cn/mmbiz_png/iaIdQfEric9TwBuibJ4N5OTyAvJibFj8b7zh0S96ibUaNiaUI0B1ziapCqiaelPPic1djtAHzRT7ZhKXgJiasGfxAChiaiaKLg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)ApplicationEvent-Subclass

#### 事件监听者角色

`ApplicationListener` 充当了事件监听者角色，它是一个接口，里面只定义了一个 `onApplicationEvent（）`方法来处理`ApplicationEvent`。`ApplicationListener`接口类源码如下，可以看出接口定义看出接口中的事件只要实现了 `ApplicationEvent`就可以了。所以，在 Spring中我们只要实现 `ApplicationListener` 接口实现 `onApplicationEvent()` 方法即可完成监听事件

```
package org.springframework.context;
import java.util.EventListener;
@FunctionalInterface
public interface ApplicationListener<E extends ApplicationEvent> extends EventListener {
    void onApplicationEvent(E var1);
}
```

#### 事件发布者角色

`ApplicationEventPublisher` 充当了事件的发布者，它也是一个接口。

```
@FunctionalInterface
public interface ApplicationEventPublisher {
    default void publishEvent(ApplicationEvent event) {
        this.publishEvent((Object)event);
    }

    void publishEvent(Object var1);
}
```

`ApplicationEventPublisher` 接口的`publishEvent（）`这个方法在`AbstractApplicationContext`类中被实现，阅读这个方法的实现，你会发现实际上事件真正是通过`ApplicationEventMulticaster`来广播出去的。具体内容过多，就不在这里分析了，后面可能会单独写一篇文章提到。

### Spring 的事件流程总结

1. 定义一个事件: 实现一个继承自 `ApplicationEvent`，并且写相应的构造函数；
2. 定义一个事件监听者：实现 `ApplicationListener` 接口，重写 `onApplicationEvent()` 方法；
3. 使用事件发布者发布消息: 可以通过 `ApplicationEventPublisher` 的 `publishEvent()` 方法发布消息。

Example:

```
// 定义一个事件,继承自ApplicationEvent并且写相应的构造函数
public class DemoEvent extends ApplicationEvent{
    private static final long serialVersionUID = 1L;

    private String message;

    public DemoEvent(Object source,String message){
        super(source);
        this.message = message;
    }

    public String getMessage() {
         return message;
          }


// 定义一个事件监听者,实现ApplicationListener接口，重写 onApplicationEvent() 方法；
@Component
public class DemoListener implements ApplicationListener<DemoEvent>{

    //使用onApplicationEvent接收消息
    @Override
    public void onApplicationEvent(DemoEvent event) {
        String msg = event.getMessage();
        System.out.println("接收到的信息是："+msg);
    }

}
// 发布事件，可以通过ApplicationEventPublisher  的 publishEvent() 方法发布消息。
@Component
public class DemoPublisher {

    @Autowired
    ApplicationContext applicationContext;

    public void publish(String message){
        //发布事件
        applicationContext.publishEvent(new DemoEvent(this, message));
    }
}
```

当调用 `DemoPublisher` 的 `publish()` 方法的时候，比如 `demoPublisher.publish("你好")` ，控制台就会打印出:`接收到的信息是：你好` 。

## 适配器模式

适配器模式(Adapter Pattern) 将一个接口转换成客户希望的另一个接口，适配器模式使接口不兼容的那些类可以一起工作，其别名为包装器(Wrapper)。

### spring AOP中的适配器模式

我们知道 Spring AOP 的实现是基于代理模式，但是 Spring AOP 的增强或通知(Advice)使用到了适配器模式，与之相关的接口是`AdvisorAdapter` 。Advice 常用的类型有：`BeforeAdvice`（目标方法调用前,前置通知）、`AfterAdvice`（目标方法调用后,后置通知）、`AfterReturningAdvice`(目标方法执行结束后，return之前)等等。每个类型Advice（通知）都有对应的拦截器:`MethodBeforeAdviceInterceptor`、`AfterReturningAdviceAdapter`、`AfterReturningAdviceInterceptor`。Spring预定义的通知要通过对应的适配器，适配成 `MethodInterceptor`接口(方法拦截器)类型的对象（如：`MethodBeforeAdviceInterceptor` 负责适配 `MethodBeforeAdvice`）。

### spring MVC中的适配器模式

在Spring MVC中，`DispatcherServlet` 根据请求信息调用 `HandlerMapping`，解析请求对应的 `Handler`。解析到对应的 `Handler`（也就是我们平常说的 `Controller` 控制器）后，开始由`HandlerAdapter` 适配器处理。`HandlerAdapter` 作为期望接口，具体的适配器实现类用于对目标类进行适配，`Controller` 作为需要适配的类。

**为什么要在 Spring MVC 中使用适配器模式？** Spring MVC 中的 `Controller` 种类众多，不同类型的 `Controller` 通过不同的方法来对请求进行处理。如果不利用适配器模式的话，`DispatcherServlet` 直接获取对应类型的 `Controller`，需要的自行来判断，像下面这段代码一样：

```
if(mappedHandler.getHandler() instanceof MultiActionController){  
   ((MultiActionController)mappedHandler.getHandler()).xxx  
}else if(mappedHandler.getHandler() instanceof XXX){  
    ...  
}else if(...){  
   ...  
}  
```

假如我们再增加一个 `Controller`类型就要在上面代码中再加入一行 判断语句，这种形式就使得程序难以维护，也违反了设计模式中的开闭原则 – 对扩展开放，对修改关闭。

## 装饰者模式

装饰者模式可以动态地给对象添加一些额外的属性或行为。相比于使用继承，装饰者模式更加灵活。简单点儿说就是当我们需要修改原有的功能，但我们又不愿直接去修改原有的代码时，设计一个Decorator套在原有代码外面。其实在 JDK 中就有很多地方用到了装饰者模式，比如 `InputStream`家族，`InputStream` 类下有 `FileInputStream` (读取文件)、`BufferedInputStream` (增加缓存,使读取文件速度大大提升)等子类都在不修改`InputStream` 代码的情况下扩展了它的功能。

![图片](https://mmbiz.qpic.cn/mmbiz_png/iaIdQfEric9TwBuibJ4N5OTyAvJibFj8b7zh3apBemUibszLADBA4wIW10sYZcPXiaBrSaJUTPjVY5EY03Pu65eeDdNA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)装饰者模式示意图

Spring 中配置 DataSource 的时候，DataSource 可能是不同的数据库和数据源。我们能否根据客户的需求在少修改原有类的代码下动态切换不同的数据源？这个时候就要用到装饰者模式(这一点我自己还没太理解具体原理)。Spring 中用到的包装器模式在类名上含有 `Wrapper`或者 `Decorator`。这些类基本上都是动态地给一个对象添加一些额外的职责

## 总结

Spring 框架中用到了哪些设计模式：

- **工厂设计模式** : Spring使用工厂模式通过 `BeanFactory`、`ApplicationContext` 创建 bean 对象。
- **代理设计模式** : Spring AOP 功能的实现。
- **单例设计模式** : Spring 中的 Bean 默认都是单例的。
- **模板方法模式** : Spring 中 `jdbcTemplate`、`hibernateTemplate` 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。
- **包装器设计模式** : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。
- **观察者模式:** Spring 事件驱动模型就是观察者模式很经典的一个应用。
- **适配器模式** :Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配`Controller`。
- ……



# Spring中的事务传播行为

### 一、什么是事务的传播？

简单的理解就是多个事务方法相互调用时,事务如何在这些方法间传播。

> 举个栗子，方法A是一个事务的方法，方法A执行过程中调用了方法B，那么方法B有无事务以及方法B对事务的要求不同都会对方法A的事务具体执行造成影响，同时方法A的事务对方法B的事务执行也有影响，这种影响具体是什么就由两个方法所定义的事务传播类型所决定。

### 二、Spring事务传播类型枚举Propagation介绍

在Spring中对于事务的传播行为定义了七种类型分别是：**REQUIRED、SUPPORTS、MANDATORY、REQUIRES_NEW、NOT_SUPPORTED、NEVER、NESTED**。

在Spring源码中这七种类型被定义为了枚举。源码在org.springframework.transaction.annotation包下的Propagation，源码中注释很多，对传播行为的七种类型的不同含义都有解释，后文中锤子我也会给大家分析，我在这里就不贴所有的源码，只把这个类上的注解贴一下，翻译一下就是：*表示与TransactionDefinition接口相对应的用于@Transactional注解的事务传播行为的枚举。*

也就是说枚举类Propagation是为了结合@Transactional注解使用而设计的，这个枚举里面定义的事务传播行为类型与TransactionDefinition中定义的事务传播行为类型是对应的，所以在使用@Transactional注解时我们就要使用Propagation枚举类来指定传播行为类型，而不直接使用TransactionDefinition接口里定义的属性。

在TransactionDefinition接口中定义了Spring事务的一些属性，不仅包括事务传播特性类型，还包括了事务的隔离级别类型（事务的隔离级别后面文章会详细讲解），更多详细信息，大家可以打开源码自己翻译一下里面的注释

```java
package org.springframework.transaction.annotation;

import org.springframework.transaction.TransactionDefinition;

/**
 * Enumeration that represents transaction propagation behaviors for use
 * with the {@link Transactional} annotation, corresponding to the
 * {@link TransactionDefinition} interface.
 *
 * @author Colin Sampaleanu
 * @author Juergen Hoeller
 * @since 1.2
 */
public enum Propagation {
    ...
}
```

### 三、七种事务传播行为详解与示例

在介绍七种事务传播行为前，我们先设计一个场景，帮助大家理解，场景描述如下

> 现有两个方法A和B，方法A执行会在数据库ATable插入一条数据，方法B执行会在数据库BTable插入一条数据，伪代码如下:

```java
//将传入参数a存入ATable
pubilc void A(a){
    insertIntoATable(a);    
}
//将传入参数b存入BTable
public void B(b){
    insertIntoBTable(b);
}
```

接下来，我们看看在如下场景下，没有事务，情况会怎样

```java
public void testMain(){
    A(a1);  //调用A入参a1
    testB();    //调用testB
}

public void testB(){
    B(b1);  //调用B入参b1
    throw Exception;     //发生异常抛出
    B(b2);  //调用B入参b2
}
```

在这里要做一个重要提示：**Spring中事务的默认实现使用的是AOP，也就是代理的方式，如果大家在使用代码测试时，同一个Service类中的方法相互调用需要使用注入的对象来调用，不要直接使用this.方法名来调用，this.方法名调用是对象内部方法调用，不会通过Spring代理，也就是事务不会起作用**

以上伪代码描述的一个场景，方法testMain和testB都没有事务，执行testMain方法，那么结果会怎么样呢？

相信大家都知道了，就是a1数据成功存入ATable表，b1数据成功存入BTable表，而在抛出异常后b2数据存储就不会执行，也就是b2数据不会存入数据库，这就是没有事务的场景。

可想而知，在上一篇文章（认识事务）中举例的转账操作，如果在某一步发生异常，且没有事务，那么钱是不是就凭空消失了，所以事务在数据库操作中的重要性可想而知。接下我们就开始理解七种不同事务传播类型的含义

### **REQUIRED(Spring默认的事务传播类型)**

**如果当前没有事务，则自己新建一个事务，如果当前存在事务，则加入这个事务**

源码说明如下：

```java
/**
     * Support a current transaction, create a new one if none exists.
     * Analogous to EJB transaction attribute of the same name.
     * <p>This is the default setting of a transaction annotation.
     */
    REQUIRED(TransactionDefinition.PROPAGATION_REQUIRED),
```

*(示例1)*根据场景举栗子,我们在testMain和testB上声明事务，设置传播行为REQUIRED，伪代码如下：

```java
@Transactional(propagation = Propagation.REQUIRED)
public void testMain(){
    A(a1);  //调用A入参a1
    testB();    //调用testB
}
@Transactional(propagation = Propagation.REQUIRED)
public void testB(){
    B(b1);  //调用B入参b1
    throw Exception;     //发生异常抛出
    B(b2);  //调用B入参b2
}
```

该场景下执行testMain方法结果如何呢？

数据库没有插入新的数据，数据库还是保持着执行testMain方法之前的状态，没有发生改变。testMain上声明了事务，在执行testB方法时就加入了testMain的事务（**当前存在事务，则加入这个事务**），在执行testB方法抛出异常后事务会发生回滚，又testMain和testB使用的同一个事务，所以事务回滚后testMain和testB中的操作都会回滚，也就使得数据库仍然保持初始状态

*(示例2)*根据场景再举一个栗子,我们只在testB上声明事务，设置传播行为REQUIRED，伪代码如下：

```java
public void testMain(){
    A(a1);  //调用A入参a1
    testB();    //调用testB
}
@Transactional(propagation = Propagation.REQUIRED)
public void testB(){
    B(b1);  //调用B入参b1
    throw Exception;     //发生异常抛出
    B(b2);  //调用B入参b2
}
```

这时的执行结果又如何呢？

数据a1存储成功，数据b1和b2没有存储。由于testMain没有声明事务，testB有声明事务且传播行为是REQUIRED，所以在执行testB时会自己新建一个事务（**如果当前没有事务，则自己新建一个事务**），testB抛出异常则只有testB中的操作发生了回滚，也就是b1的存储会发生回滚，但a1数据不会回滚，所以最终a1数据存储成功，b1和b2数据没有存储

### **SUPPORTS**

**当前存在事务，则加入当前事务，如果当前没有事务，就以非事务方法执行**

源码注释如下(太长省略了一部分)，其中里面有一个提醒翻译一下就是：“对于具有事务同步的事务管理器，SUPPORTS与完全没有事务稍有不同，因为它定义了可能应用同步的事务范围”。这个是与事务同步管理器相关的一个注意项，这里不过多讨论。

```java
/**
     * Support a current transaction, execute non-transactionally if none exists.
     * Analogous to EJB transaction attribute of the same name.
     * <p>Note: For transaction managers with transaction synchronization,
     * {@code SUPPORTS} is slightly different from no transaction at all,
     * as it defines a transaction scope that synchronization will apply for.
     ...
     */
    SUPPORTS(TransactionDefinition.PROPAGATION_SUPPORTS),
```

*(示例3)*根据场景举栗子，我们只在testB上声明事务，设置传播行为SUPPORTS，伪代码如下：

```java
public void testMain(){
    A(a1);  //调用A入参a1
    testB();    //调用testB
}
@Transactional(propagation = Propagation.SUPPORTS)
public void testB(){
    B(b1);  //调用B入参b1
    throw Exception;     //发生异常抛出
    B(b2);  //调用B入参b2
}
```

这种情况下，执行testMain的最终结果就是，a1，b1存入数据库，b2没有存入数据库。由于testMain没有声明事务，且testB的事务传播行为是SUPPORTS，所以执行testB时就是没有事务的（**如果当前没有事务，就以非事务方法执行**），则在testB抛出异常时也不会发生回滚，所以最终结果就是a1和b1存储成功，b2没有存储。

那么当我们在testMain上声明事务且使用REQUIRED传播方式的时候，这个时候执行testB就满足**当前存在事务，则加入当前事务**，在testB抛出异常时事务就会回滚，最终结果就是a1，b1和b2都不会存储到数据库

### **MANDATORY**

**当前存在事务，则加入当前事务，如果当前事务不存在，则抛出异常。**

源码注释如下：

```java
/**
     * Support a current transaction, throw an exception if none exists.
     * Analogous to EJB transaction attribute of the same name.
     */
    MANDATORY(TransactionDefinition.PROPAGATION_MANDATORY),
```

*(示例4)*场景举栗子，我们只在testB上声明事务，设置传播行为MANDATORY，伪代码如下：

```java
public void testMain(){
    A(a1);  //调用A入参a1
    testB();    //调用testB
}
@Transactional(propagation = Propagation.MANDATORY)
public void testB(){
    B(b1);  //调用B入参b1
    throw Exception;     //发生异常抛出
    B(b2);  //调用B入参b2
}
```

这种情形的执行结果就是a1存储成功，而b1和b2没有存储。b1和b2没有存储，并不是事务回滚的原因，而是因为testMain方法没有声明事务，在去执行testB方法时就直接抛出事务要求的异常（**如果当前事务不存在，则抛出异常**），所以testB方法里的内容就没有执行。

那么如果在testMain方法进行事务声明，并且设置为REQUIRED，则执行testB时就会使用testMain已经开启的事务，遇到异常就正常的回滚了。

### **REQUIRES_NEW**

**创建一个新事务，如果存在当前事务，则挂起该事务。**

可以理解为设置事务传播类型为REQUIRES_NEW的方法，在执行时，不论当前是否存在事务，总是会新建一个事务。

源码注释如下

```java
/**
     * Create a new transaction, and suspend the current transaction if one exists.
     ...
     */
    REQUIRES_NEW(TransactionDefinition.PROPAGATION_REQUIRES_NEW),
```

*(示例5)*场景举栗子，为了说明设置REQUIRES_NEW的方法会开启新事务，我们把异常发生的位置换到了testMain，然后给testMain声明事务，传播类型设置为REQUIRED，testB也声明事务，设置传播类型为REQUIRES_NEW，伪代码如下

```java
@Transactional(propagation = Propagation.REQUIRED)
public void testMain(){
    A(a1);  //调用A入参a1
    testB();    //调用testB
    throw Exception;     //发生异常抛出
}
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void testB(){
    B(b1);  //调用B入参b1
    B(b2);  //调用B入参b2
}
```

这种情形的执行结果就是a1没有存储，而b1和b2存储成功，因为testB的事务传播设置为REQUIRES_NEW,所以在执行testB时会开启一个新的事务，testMain中发生的异常时在testMain所开启的事务中，所以这个异常不会影响testB的事务提交，testMain中的事务会发生回滚，所以最终a1就没有存储，而b1和b2就存储成功了。

与这个场景对比的一个场景就是testMain和testB都设置为REQUIRED，那么上面的代码执行结果就是所有数据都不会存储，因为testMain和testMain是在同一个事务下的，所以事务发生回滚时，所有的数据都会回滚

### **NOT_SUPPORTED**

**始终以非事务方式执行,如果当前存在事务，则挂起当前事务**

可以理解为设置事务传播类型为NOT_SUPPORTED的方法，在执行时，不论当前是否存在事务，都会以非事务的方式运行。

源码说明如下

```java
/**
     * Execute non-transactionally, suspend the current transaction if one exists.
     ...
     */
    NOT_SUPPORTED(TransactionDefinition.PROPAGATION_NOT_SUPPORTED),
```

*(示例6)*场景举栗子，testMain传播类型设置为REQUIRED，testB传播类型设置为NOT_SUPPORTED，且异常抛出位置在testB中，伪代码如下

```java
@Transactional(propagation = Propagation.REQUIRED)
public void testMain(){
    A(a1);  //调用A入参a1
    testB();    //调用testB
}
@Transactional(propagation = Propagation.NOT_SUPPORTED)
public void testB(){
    B(b1);  //调用B入参b1
    throw Exception;     //发生异常抛出
    B(b2);  //调用B入参b2
}
```

该场景的执行结果就是a1和b2没有存储，而b1存储成功。testMain有事务，而testB不使用事务，所以执行中testB的存储b1成功，然后抛出异常，此时testMain检测到异常事务发生回滚，但是由于testB不在事务中，所以只有testMain的存储a1发生了回滚，最终只有b1存储成功，而a1和b1都没有存储

### **NEVER**

**不使用事务，如果当前事务存在，则抛出异常**

很容易理解，就是我这个方法不使用事务，并且调用我的方法也不允许有事务，如果调用我的方法有事务则我直接抛出异常。

源码注释如下：

```java
/**
     * Execute non-transactionally, throw an exception if a transaction exists.
     * Analogous to EJB transaction attribute of the same name.
     */
    NEVER(TransactionDefinition.PROPAGATION_NEVER),
```

*(示例7)*场景举栗子，testMain设置传播类型为REQUIRED，testB传播类型设置为NEVER，并且把testB中的抛出异常代码去掉，则伪代码如下

```java
@Transactional(propagation = Propagation.REQUIRED)
public void testMain(){
    A(a1);  //调用A入参a1
    testB();    //调用testB
}
@Transactional(propagation = Propagation.NEVER)
public void testB(){
    B(b1);  //调用B入参b1
    B(b2);  //调用B入参b2
}
```

该场景执行，直接抛出事务异常，且不会有数据存储到数据库。由于testMain事务传播类型为REQUIRED，所以testMain是运行在事务中，而testB事务传播类型为NEVER，所以testB不会执行而是直接抛出事务异常，此时testMain检测到异常就发生了回滚，所以最终数据库不会有数据存入。

### **NESTED**

**如果当前事务存在，则在嵌套事务中执行，否则REQUIRED的操作一样（开启一个事务）**

这里需要注意两点：

- 和REQUIRES_NEW的区别

> REQUIRES_NEW是新建一个事务并且新开启的这个事务与原有事务无关，而NESTED则是当前存在事务时（我们把当前事务称之为父事务）会开启一个嵌套事务（称之为一个子事务）。
> 在NESTED情况下父事务回滚时，子事务也会回滚，而在REQUIRES_NEW情况下，原有事务回滚，不会影响新开启的事务。

- 和REQUIRED的区别

> REQUIRED情况下，调用方存在事务时，则被调用方和调用方使用同一事务，那么被调用方出现异常时，由于共用一个事务，所以无论调用方是否catch其异常，事务都会回滚
> 而在NESTED情况下，被调用方发生异常时，调用方可以catch其异常，这样只有子事务回滚，父事务不受影响

*(示例8)*场景举栗子，testMain设置为REQUIRED，testB设置为NESTED，且异常发生在testMain中，伪代码如下

```java
@Transactional(propagation = Propagation.REQUIRED)
public void testMain(){
    A(a1);  //调用A入参a1
    testB();    //调用testB
    throw Exception;     //发生异常抛出
}
@Transactional(propagation = Propagation.NESTED)
public void testB(){
    B(b1);  //调用B入参b1
    B(b2);  //调用B入参b2
}
```

该场景下，所有数据都不会存入数据库，因为在testMain发生异常时，父事务回滚则子事务也跟着回滚了，可以与*(示例5)*比较看一下，就找出了与REQUIRES_NEW的不同

*(示例9)*场景举栗子，testMain设置为REQUIRED，testB设置为NESTED，且异常发生在testB中，伪代码如下

```java
@Transactional(propagation = Propagation.REQUIRED)
public void testMain(){
    A(a1);  //调用A入参a1
    try{
        testB();    //调用testB
    }catch（Exception e){

    }
    A(a2);
}
@Transactional(propagation = Propagation.NESTED)
public void testB(){
    B(b1);  //调用B入参b1
    throw Exception;     //发生异常抛出
    B(b2);  //调用B入参b2
}
```

这种场景下，结果是a1,a2存储成功，b1和b2存储失败，因为调用方catch了被调方的异常，所以只有子事务回滚了。

同样的代码，如果我们把testB的传播类型改为REQUIRED，结果也就变成了：没有数据存储成功。就算在调用方catch了异常，整个事务还是会回滚，因为，调用方和被调方共用的同一个事务

# Java中拦截器和过滤器

过滤器（Filter）

Servlet中的过滤器Filter是实现了javax.servlet.Filter接口的服务器端程序，主要的用途是设置字符集、控制权限、控制转向、做一些业务逻辑判断等。其工作原理是，只要你在web.xml文件配置好要拦截的客户端请求，它都会帮你拦截到请求，此时你就可以对请求或响应(Request、Response)统一设置编码，简化操作；同时还可进行逻辑判断，如用户是否已经登陆、有没有权限访问该页面等等工作。它是随你的web应用启动而启动的，只初始化一次，以后就可以拦截相关请求，只有当你的web应用停止或重新部署的时候才销毁。

Filter可以认为是Servlet的一种“加强版”，它主要用于对用户请求进行预处理，也可以对HttpServletResponse进行后处理，是个典型的处理链。Filter也可以对用户请求生成响应，这一点与Servlet相同，但实际上很少会使用Filter向用户请求生成响应。使用Filter完整的流程是：Filter对用户请求进行预处理，接着将请求交给Servlet进行处理并生成响应，最后Filter再对服务器响应进行后处理。

   Filter有如下几个用处。

- 在HttpServletRequest到达Servlet之前，拦截客户的HttpServletRequest。
- 根据需要检查HttpServletRequest，也可以修改HttpServletRequest头和数据。
- 在HttpServletResponse到达客户端之前，拦截HttpServletResponse。
- 根据需要检查HttpServletResponse，也可以修改HttpServletResponse头和数据。

   Filter有如下几个种类。

- 用户授权的Filter：Filter负责检查用户请求，根据请求过滤用户非法请求。
- 日志Filter：详细记录某些特殊的用户请求。
- 负责解码的Filter:包括对非标准编码的请求解码。
- 能改变XML内容的XSLT Filter等。
- Filter可以负责拦截多个请求或响应；一个请求或响应也可以被多个Filter拦截。

   创建一个Filter只需两个步骤

1. 创建Filter处理类
2. web.xml文件中配置Filter

  创建Filter必须实现javax.servlet.Filter接口，在该接口中定义了如下三个方法。

- void init(FilterConfig config):用于完成Filter的初始化。
- void destory():用于Filter销毁前，完成某些资源的回收。
- void doFilter(ServletRequest request,ServletResponse response,FilterChain chain):实现过滤功能，该方法就是对每个请求及响应增加的额外处理。该方法可以实现对用户请求进行预处理(ServletRequest request)，也可实现对服务器响应进行后处理(ServletResponse response)—它们的分界线为是否调用了chain.doFilter(),执行该方法之前，即对用户请求进行预处理；执行该方法之后，即对服务器响应进行后处理。

##  拦截器（Interceptor）

拦截器是在面向切面编程中应用的，就是在你的service或者一个方法前调用一个方法，或者在方法后调用一个方法。是基于JAVA的反射机制。拦截器不是在web.xml，比如struts在struts.xml中配置。

拦截器，在AOP(Aspect-Oriented Programming)中用于在某个方法或字段被访问之前，进行拦截，然后在之前或之后加入某些操作。拦截是AOP的一种实现策略。

   在WebWork的中文文档的解释为—拦截器是动态拦截Action调用的对象。它提供了一种机制使开发者可以定义在一个Action执行的前后执行的代码，也可以在一个Action执行前阻止其执行。同时也提供了一种可以提取Action中可重用的部分的方式。

   拦截器将Action共用的行为独立出来，在Action执行前后执行。这也就是我们所说的AOP，它是分散关注的编程方法，它将通用需求功能从不相关类之中分离出来；同时，能够共享一个行为，一旦行为发生变化，不必修改很多类，只要修改这个行为就可以。

   拦截器将很多功能从我们的Action中独立出来，大量减少了我们Action的代码，独立出来的行为就有很好的重用性。

   当你提交对Action(默认是.action结尾的url)的请求时，ServletDispatcher会根据你的请求，去调度并执行相应的Action。在Action执行之前，调用被Interceptor截取，Interceptor在Action执行前后执行。

   SpringMVC 中的Interceptor 拦截请求是通过HandlerInterceptor 来实现的。在SpringMVC 中定义一个Interceptor 非常简单，主要有两种方式，第一种方式是要定义的Interceptor类要实现了Spring 的HandlerInterceptor 接口，或者是这个类继承实现了HandlerInterceptor 接口的类，比如Spring 已经提供的实现了HandlerInterceptor 接口的抽象类HandlerInterceptorAdapter ；第二种方式是实现Spring的WebRequestInterceptor接口，或者是继承实现了WebRequestInterceptor的类。

  （1 ）preHandle (HttpServletRequest request, HttpServletResponse response, Object handle) 方法，顾名思义，该方法将在请求处理之前进行调用。SpringMVC 中的Interceptor 是链式的调用的，在一个应用中或者说是在一个请求中可以同时存在多个Interceptor 。每个Interceptor 的调用会依据它的声明顺序依次执行，而且最先执行的都是Interceptor 中的preHandle 方法，所以可以在这个方法中进行一些前置初始化操作或者是对当前请求的一个预处理，也可以在这个方法中进行一些判断来决定请求是否要继续进行下去。该方法的返回值是布尔值Boolean类型的，当它返回为false 时，表示请求结束，后续的Interceptor 和Controller 都不会再执行；当返回值为true 时就会继续调用下一个Interceptor 的preHandle 方法，如果已经是最后一个Interceptor 的时候就会是调用当前请求的Controller 方法。

  （2 ）postHandle (HttpServletRequest request, HttpServletResponse response, Object handle, ModelAndView modelAndView) 方法，由preHandle 方法的解释我们知道这个方法包括后面要说到的afterCompletion 方法都只能是在当前所属的Interceptor 的preHandle 方法的返回值为true 时才能被调用。postHandle 方法，顾名思义就是在当前请求进行处理之后，也就是Controller 方法调用之后执行，但是它会在DispatcherServlet 进行视图返回渲染之前被调用，所以我们可以在这个方法中对Controller 处理之后的ModelAndView 对象进行操作。postHandle 方法被调用的方向跟preHandle 是相反的，也就是说先声明的Interceptor 的postHandle 方法反而会后执行，这和Struts2 里面的Interceptor 的执行过程有点类型。Struts2 里面的Interceptor 的执行过程也是链式的，只是在Struts2 里面需要手动调用ActionInvocation 的invoke 方法来触发对下一个Interceptor 或者是Action 的调用，然后每一个Interceptor 中在invoke 方法调用之前的内容都是按照声明顺序执行的，而invoke 方法之后的内容就是反向的。

  （3 ）afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handle, Exception ex) 方法，该方法也是需要当前对应的Interceptor 的preHandle 方法的返回值为true 时才会执行。顾名思义，该方法将在整个请求结束之后，也就是在DispatcherServlet 渲染了对应的视图之后执行。这个方法的主要作用是用于进行资源清理工作的。

##  拦截器（Interceptor）和过滤器（Filter）的区别

Spring的Interceptor(拦截器)与Servlet的Filter有相似之处，比如二者都是AOP编程思想的体现，都能实现权限检查、日志记录等。不同的是：

| Filter                                                       | Interceptor                                                  | Summary                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Filter 接口定义在 javax.servlet 包中                         | 接口 HandlerInterceptor 定义在org.springframework.web.servlet 包中 |                                                              |
| Filter 定义在 web.xml 中                                     |                                                              |                                                              |
| Filter在只在 Servlet 前后起作用。Filters 通常将 请求和响应（request/response） 当做黑盒子，Filter 通常不考虑servlet 的实现。 | 拦截器能够深入到方法前后、异常抛出前后等，因此拦截器的使用具有更大的弹性。允许用户介入（hook into）请求的生命周期，在请求过程中获取信息，Interceptor 通常和请求更加耦合。 | 在Spring构架的程序中，要优先使用拦截器。几乎所有 Filter 能够做的事情， interceptor 都能够轻松的实现[ ](http://einverne.github.io/post/2017/08/spring-interceptor-vs-filter.html#fn:top) |
| Filter 是 Servlet 规范规定的。                               | 而拦截器既可以用于Web程序，也可以用于Application、Swing程序中。 | 使用范围不同                                                 |
| Filter 是在 Servlet 规范中定义的，是 Servlet 容器支持的。    | 而拦截器是在 Spring容器内的，是Spring框架支持的。            | 规范不同                                                     |
| Filter 不能够使用 Spring 容器资源                            | 拦截器是一个Spring的组件，归Spring管理，配置在Spring文件中，因此能使用Spring里的任何资源、对象，例如 Service对象、数据源、事务管理等，通过IoC注入到拦截器即可 | Spring 中使用 interceptor 更容易                             |
| Filter 是被 Server(like Tomcat) 调用                         | Interceptor 是被 Spring 调用                                 | 因此 Filter 总是优先于 Interceptor 执行                      |

##  拦截器（Interceptor）和过滤器（Filter）的执行顺序

```
过滤前-拦截前-Action处理-拦截后-过滤后
```

## 拦截器（Interceptor）使用

interceptor 的执行顺序大致为：

1. 请求到达 DispatcherServlet
2. DispatcherServlet 发送至 Interceptor ，执行 preHandle
3. 请求达到 Controller
4. 请求结束后，postHandle 执行

Spring 中主要通过 HandlerInterceptor 接口来实现请求的拦截，实现 HandlerInterceptor 接口需要实现下面三个方法：

- preHandle() – 在handler执行之前，返回 boolean 值，true 表示继续执行，false 为停止执行并返回。
- postHandle() – 在handler执行之后, 可以在返回之前对返回的结果进行修改
- afterCompletion() – 在请求完全结束后调用，可以用来统计请求耗时等等

统计请求耗时

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

```
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.log4j.Logger;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;

public class ExecuteTimeInterceptor extends HandlerInterceptorAdapter{

    private static final Logger logger = Logger.getLogger(ExecuteTimeInterceptor.class);

    //before the actual handler will be executed
    public boolean preHandle(HttpServletRequest request,
        HttpServletResponse response, Object handler)
        throws Exception {

        long startTime = System.currentTimeMillis();
        request.setAttribute("startTime", startTime);

        return true;
    }

    //after the handler is executed
    public void postHandle(
        HttpServletRequest request, HttpServletResponse response,
        Object handler, ModelAndView modelAndView)
        throws Exception {

        long startTime = (Long)request.getAttribute("startTime");

        long endTime = System.currentTimeMillis();

        long executeTime = endTime - startTime;

        //modified the exisitng modelAndView
        modelAndView.addObject("executeTime",executeTime);

        //log it
        if(logger.isDebugEnabled()){
           logger.debug("[" + handler + "] executeTime : " + executeTime + "ms");
        }
    }
} 
```

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

使用mvc:interceptors标签来声明需要加入到SpringMVC拦截器链中的拦截器

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

```
<mvc:interceptors>  
<!-- 使用bean定义一个Interceptor，直接定义在mvc:interceptors根下面的Interceptor将拦截所有的请求 -->  
<bean class="com.company.app.web.interceptor.AllInterceptor"/>  
    <mvc:interceptor>  
         <mvc:mapping path="/**"/>  
         <mvc:exclude-mapping path="/parent/**"/>  
         <bean class="com.company.authorization.interceptor.SecurityInterceptor" />  
    </mvc:interceptor>  
    <mvc:interceptor>  
         <mvc:mapping path="/parent/**"/>  
         <bean class="com.company.authorization.interceptor.SecuritySystemInterceptor" />  
    </mvc:interceptor>  
</mvc:interceptors>   
```

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

可以利用mvc:interceptors标签声明一系列的拦截器，然后它们就可以形成一个拦截器链，拦截器的执行顺序是按声明的先后顺序执行的，先声明的拦截器中的preHandle方法会先执行，然而它的postHandle方法和afterCompletion方法却会后执行。

在mvc:interceptors标签下声明interceptor主要有两种方式：

- 直接定义一个Interceptor实现类的bean对象。使用这种方式声明的Interceptor拦截器将会对所有的请求进行拦截。
- 使用mvc:interceptor标签进行声明。使用这种方式进行声明的Interceptor可以通过mvc:mapping子标签来定义需要进行拦截的请求路径。

经过上述两步之后，定义的拦截器就会发生作用对特定的请求进行拦截了。

## 过滤器（Filter）使用

Servlet 的 Filter 接口需要实现如下方法：

- `void init(FilterConfig paramFilterConfig)` – 当容器初始化 Filter 时调用，该方法在 Filter 的生命周期只会被调用一次，一般在该方法中初始化一些资源，FilterConfig 是容器提供给 Filter 的初始化参数，在该方法中可以抛出 ServletException 。init 方法必须执行成功，否则 Filter 可能不起作用，出现以下两种情况时，web 容器中 Filter 可能无效： 1）抛出 ServletException 2）超过 web 容器定义的执行时间。

- `doFilter(ServletRequest paramServletRequest, ServletResponse paramServletResponse, FilterChain paramFilterChain)` – Web 容器每一次请求都会调用该方法。该方法将容器的请求和响应作为参数传递进来， FilterChain 用来调用下一个 Filter。

- `void destroy()` – 当容器销毁 Filter 实例时调用该方法，可以在方法中销毁资源，该方法在 Filter 的生命周期只会被调用一次。

  FrequencyLimitFilter com.company.filter.FrequencyLimitFilter FrequencyLimitFilter /login/*

## 拦截器（Interceptor）和过滤器（Filter）的一些用途

- Authentication Filters
- Logging and Auditing Filters
- Image conversion Filters
- Data compression Filters
- Encryption Filters
- Tokenizing Filters
- Filters that trigger resource access events
- XSL/T filters
- Mime-type chain Filter

Request Filters 可以:

- 执行安全检查 perform security checks
- 格式化请求头和主体 reformat request headers or bodies
- 审查或者记录日志 audit or log requests
- 根据请求内容授权或者限制用户访问 Authentication-Blocking requests based on user identity.
- 根据请求频率限制用户访问

Response Filters 可以:

- 压缩响应内容,比如让下载的内容更小 Compress the response stream
- 追加或者修改响应 append or alter the response stream
- 创建或者整体修改响应 create a different response altogether
- 根据地方不同修改响应内容 Localization-Targeting the request and response to a particular locale.

## demo

过滤器（Filter）：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

```
    <filter>
        <description>字符集过滤器</description>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <description>字符集编码</description>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

##  总结

1.过滤器：所谓过滤器顾名思义是用来过滤的，在java web中，你传入的request,response提前过滤掉一些信息，或者提前设置一些参数，然后再传入servlet或者struts的action进行业务逻辑，比如过滤掉非法url（不是login.do的地址请求，如果用户没有登陆都过滤掉）,或者在传入servlet或者struts的action前统一设置字符集，或者去除掉一些非法字符（聊天室经常用到的，一些骂人的话）。filter 流程是线性的， url传来之后，检查之后，可保持原来的流程继续向下执行，被下一个filter, servlet接收等.

2.java的拦截器 主要是用在插件上，扩展件上比如 hibernate spring struts2等 有点类似面向切片的技术，在用之前先要在配置文件即xml文件里声明一段的那个东西。

##  参考资料

http://blog.csdn.net/heyeqingquan/article/details/71482169

http://einverne.github.io/post/2017/08/spring-interceptor-vs-filter.html

http://blog.csdn.net/xiaodanjava/article/details/32125687

# Mybatis的执行流程

我们都知道MyBtis是对JDBC的简易封装，它的出现某种程度了是为了消除所有的JDBC代码和参数的手工设置以及结果集的封装问题；不管怎样，JDBC的那一套还是不会变的，只是做了抽象、封装、归类等；所以想要理解MyBatis的执行流程，那就不得不先回顾一下JDBC的执行流程。



## **JDBC执行六步走**

1. **注册驱动**
2. **获取Connection连接**
3. **执行预编译**
4. **执行SQL**
5. **封装结果集**
6. **释放资源**

以上就是JDBC操作数据的流程步骤，然后我看下ＭyBatis的执行流程图。



![img](https://pic4.zhimg.com/80/v2-11c9b17f5f79909cf4cbae580ba63493_720w.jpg)





### **MyBatis执行八步走**

上面流程就是MyBatis内部核心流程，咱们来一步步解释下，根据图中步骤，我们可以将这个执行流程分成了8个步骤。

1、读取MyBatis的核心配置文件。mybatis-config.xml为MyBatis的全局配置文件，用于配置数据库连接、属性、类型别名、类型处理器、插件、环境配置、映射器（mapper.xml）等信息，这个过程中有一个比较重要的部分就是映射文件其实是配在这里的；这个核心配置文件最终会被封装成一个Configuration对象

2、加载映射文件。映射文件即SQL映射文件，该文件中配置了操作数据库的SQL语句，映射文件是在mybatis-config.xml中加载；可以加载多个映射文件。常见的配置的方式有两种，一种是package扫描包，一种是mapper找到配置文件的位置。

```java
<!-- 使用包路径，扫描包下所有的接口，这种方式比较方便 --> 

<package name="com.mybatis.demo"/> 
<!-- resource:使用相对路径的资源引用-->
<!-- url:使用绝对类路径的资源引用-->
<!-- class:使用映射器接口实现类的完全限定类名-->
<mapper resource="xxx.xml"/>
```



3、构造会话工厂获取SqlSessionFactory。这个过程其实是用建造者设计模式使用SqlSessionFactoryBuilder对象构建的，SqlSessionFactory的最佳作用域是应用作用域。

```text
//2. 创建SqlSessionFactory对象实际创建的是DefaultSqlSessionFactory对象
SqlSessionFactory builder = new	SqlSessionFactoryBuilder().build(inputStream);
```



4、创建会话对象SqlSession。由会话工厂创建SqlSession对象，对象中包含了执行SQL语句的所有方法，每个线程都应该有它自己的 SqlSession 实例。SqlSession的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。

```text
//3. 创建SqlSession对象实际创建的是DefaultSqlSession对象
  SqlSession sqlSession = builder.openSession();
```



5、Executor执行器。是MyBatis的核心，负责SQL语句的生成和查询缓存的维护，它将根据SqlSession传递的参数动态地生成需要执行的SQL语句，同时负责查询缓存的维护

- SimpleExecutor -- SIMPLE 就是普通的执行器。
- ReuseExecutor-执行器会重用预处理语句（PreparedStatements）
- BatchExecutor --它是批处理执行器



6、MappedStatement对象。MappedStatement是对解析的SQL的语句封装，一个MappedStatement代表了一个sql语句标签，如下：

```text
<!--一个动态sql标签就是一个`MappedStatement`对象-->
<select	id="selectUserList"	resultType="com.mybatis.User"> 
  select * from t_user
</select>
```



7、输入参数映射。输入参数类型可以是基本数据类型，也可以是Map、List、POJO类型复杂数据类型，这个过程类似于JDBC的预编译处理参数的过程，有两个属性 parameterType和parameterMap

8、封装结果集。可以封装成多种类型可以是基本数据类型，也可以是Map、List、POJO类型复杂数据类型。封装结果集的过程就和JDBC封装结果集是一样的。也有两个常用的属性resultType和resultMap。

我们再来看一下这个完整的执行步骤，代码如下：

```text
/**
*	Mybatis测试
*/
public class	MybatisTest {
public	static	void	main(String[]args) throws	Exception	{
 //	1.加载配置文件
 InputStream	inputStream =	Resources.getResourceAsStream("mybatis-config.xml");
 //2. 创建SqlSessionFactory对象实际创建的是DefaultSqlSessionFactory对象
SqlSessionFactory builder = new	SqlSessionFactoryBuilder().build(inputStream);
 //3. 创建SqlSession对象实际创建的是DefaultSqlSession对象
  SqlSession sqlSession = builder.openSession();
 //4. 创建代理对象
  UserMapper mapper = sqlSession.getMapper(UserMapper.class);
 //5. 执行查询语句
 List<User>	users = mapper.selectUserList();
 //6. 释放资源
  sqlSession.close();
  inputStream.close();
}
}
```

通过分析Mybatis的执行流程，我们可以发现它和JDBC基本大同小异，比较明显的地方就是：

1. 注册驱动获取链接的部分都抽取到了核心配置文件mybatis-config.xml中。
2. sql语句抽取到了映射文件mapper.xml中。

至于其他的部分，如执行sql预编译、执行查询、封装结果集等都是抽取到了其他的类中来完成这些操作。通过对JDBC执行步骤来对比分析MyBatis的执行的流程，总体上来看它们的执行步骤基本是一样的，所以大家是不是觉得MyBatis这个框架其实也挺简单的，总结下其实就是：

- **加载解析配置文件（核心配置文件和映射文件）**
- **处理参数**
- **执行查询**
- **封装结果集**

那如果去参考 Mybatis，我们来看看它的几个环节是如何设计的：



![img](https://static001.geekbang.org/infoq/07/07bfe66f7b8da2926a33debf355b9239.jpeg?x-oss-process=image/resize,p_80/auto-orient,1)

# Mybatis缓存

1      查询缓存


1.1  什么是查询缓存
mybatis提供查询缓存，用于减轻数据压力，提高数据库性能。

mybaits提供一级缓存，和二级缓存。

 

一级缓存


一级缓存是SqlSession级别的缓存。在操作数据库时需要构造 sqlSession对象，在对象中有一个(内存区域)数据结构（HashMap）用于存储缓存数据。不同的sqlSession之间的缓存数据区域（HashMap）是互相不影响的。

一级缓存的作用域是同一个SqlSession，在同一个sqlSession中两次执行相同的sql语句，第一次执行完毕会将数据库中查询的数据写到缓存（内存），第二次会从缓存中获取数据将不再从数据库查询，从而提高查询效率。当一个sqlSession结束后该sqlSession中的一级缓存也就不存在了。Mybatis默认开启一级缓存。

一级缓存只是相对于同一个SqlSession而言。所以在参数和SQL完全一样的情况下，我们使用同一个SqlSession对象调用一个Mapper方法，往往只执行一次SQL，因为使用SelSession第一次查询后，MyBatis会将其放在缓存中，以后再查询的时候，如果没有声明需要刷新，并且缓存没有超时的情况下，SqlSession都会取出当前缓存的数据，而不会再次发送SQL到数据库。

1、一级缓存的生命周期有多长？
　　a、MyBatis在开启一个数据库会话时，会 创建一个新的SqlSession对象，SqlSession对象中会有一个新的Executor对象。Executor对象中持有一个新的PerpetualCache对象；当会话结束时，SqlSession对象及其内部的Executor对象还有PerpetualCache对象也一并释放掉。

　　b、如果SqlSession调用了close()方法，会释放掉一级缓存PerpetualCache对象，一级缓存将不可用。

　　c、如果SqlSession调用了clearCache()，会清空PerpetualCache对象中的数据，但是该对象仍可使用。

　　d、SqlSession中执行了任何一个update操作(update()、delete()、insert()) ，都会清空PerpetualCache对象的数据，但是该对象可以继续使用

 2、怎么判断某两次查询是完全相同的查询？
　　mybatis认为，对于两次查询，如果以下条件都完全一样，那么就认为它们是完全相同的两次查询。

　　2.1 传入的statementId

　　2.2 查询时要求的结果集中的结果范围

　　2.3. 这次查询所产生的最终要传递给JDBC java.sql.Preparedstatement的Sql语句字符串（boundSql.getSql() ）

　　2.4 传递给java.sql.Statement要设置的参数值

 

二级缓存
MyBatis的二级缓存是Application级别的缓存，它可以提高对数据库查询的效率，以提高应用的性能。

　　MyBatis的缓存机制整体设计以及二级缓存的工作模式



二级缓存是mapper级别的缓存，多个SqlSession去操作同一个Mapper的sql语句，多个SqlSession去操作数据库得到数据会存在二级缓存区域，多个SqlSession可以共用二级缓存，二级缓存是跨SqlSession的。

    二级缓存是多个SqlSession共享的，其作用域是mapper的同一个namespace，不同的sqlSession两次执行相同namespace下的sql语句且向sql中传递参数也相同即最终执行相同的sql语句，第一次执行完毕会将数据库中查询的数据写到缓存（内存），第二次会从缓存中获取数据将不再从数据库查询，从而提高查询效率。Mybatis默认没有开启二级缓存需要在setting全局参数中配置开启二级缓存。

sqlSessionFactory层面上的二级缓存默认是不开启的，二级缓存的开启需要进行配置，实现二级缓存的时候，MyBatis要求返回的POJO必须是可序列化的。 也就是要求实现Serializable接口，配置方法很简单，只需要在映射XML文件配置就可以开启缓存了<cache/>，如果我们配置了二级缓存就意味着：

映射语句文件中的所有select语句将会被缓存。
映射语句文件中的所欲insert、update和delete语句会刷新缓存。
缓存会使用默认的Least Recently Used（LRU，最近最少使用的）算法来收回。
根据时间表，比如No Flush Interval,（CNFI没有刷新间隔），缓存不会以任何时间顺序来刷新。
缓存会存储列表集合或对象(无论查询方法返回什么)的1024个引用
缓存会被视为是read/write(可读/可写)的缓存，意味着对象检索不是共享的，而且可以安全的被调用者修改，不干扰其他调用者或线程所做的潜在修改。
如果缓存中有数据就不用从数据库中获取，大大提高系统性能。

1.2  一级缓存
1.2.1    一级缓存工作原理
下图是根据id查询用户的一级缓存图解

 

第一次发起查询用户id为1的用户信息，先去找缓存中是否有id为1的用户信息，如果没有，从数据库查询用户信息。

得到用户信息，将用户信息存储到一级缓存中。

 

如果sqlSession去执行commit操作（执行插入、更新、删除），清空SqlSession中的一级缓存，这样做的目的为了让缓存中存储的是最新的信息，避免脏读。

 

第二次发起查询用户id为1的用户信息，先去找缓存中是否有id为1的用户信息，缓存中有，直接从缓存中获取用户信息。

1.2.2    一级缓存测试
mybatis默认支持一级缓存，不需要在配置文件去配置。

 

按照上边一级缓存原理步骤去测试。


@Test

   public void testCache1() throws Exception{

      SqlSessionsqlSession = sqlSessionFactory.openSession();//创建代理对象
     
      UserMapperuserMapper = sqlSession.getMapper(UserMapper.class);


​     

      //下边查询使用一个SqlSession
     
      //第一次发起请求，查询id为1的用户
     
      Useruser1 = userMapper.findUserById(1);
     
      System.out.println(user1);


​     

//    如果sqlSession去执行commit操作（执行插入、更新、删除），清空SqlSession中的一级缓存，这样做的目的为了让缓存中存储的是最新的信息，避免脏读。

​     

      //更新user1的信息
     
      user1.setUsername("测试用户22");
     
      userMapper.updateUser(user1);
     
      //执行commit操作去清空缓存
     
      sqlSession.commit();


​     

      //第二次发起请求，查询id为1的用户
     
      Useruser2 = userMapper.findUserById(1);
     
      System.out.println(user2);


​     

      sqlSession.close();


​     

   }
1.2.3    一级缓存应用
正式开发，是将mybatis和spring进行整合开发，事务控制在service中。

一个service方法中包括很多mapper方法调用。

service{

         //开始执行时，开启事务，创建SqlSession对象
     
         //第一次调用mapper的方法findUserById(1)


​        

         //第二次调用mapper的方法findUserById(1)，从一级缓存中取数据


​        

//aop控制 只要方法结束，sqlSession关闭 sqlsession关闭后就销毁数据结构，清空缓存

         Service结束sqlsession关闭

}
如果是执行两次service调用查询相同的用户信息，不走一级缓存，因为Service方法结束，sqlSession就关闭，一级缓存就清空。

1.3  二级缓存
1.3.1    原理




首先开启mybatis的二级缓存。

 

sqlSession1去查询用户id为1的用户信息，查询到用户信息会将查询数据存储到二级缓存中。

 

如果SqlSession3去执行相同 mapper下sql，执行commit提交，清空该 mapper下的二级缓存区域的数据。

 

sqlSession2去查询用户id为1的用户信息，去缓存中找是否存在数据，如果存在直接从缓存中取出数据。

 

二级缓存与一级缓存区别，二级缓存的范围更大，多个sqlSession可以共享一个UserMapper的二级缓存区域。数据类型仍然为HashMap

UserMapper有一个二级缓存区域（按namespace分，如果namespace相同则使用同一个相同的二级缓存区），其它mapper也有自己的二级缓存区域（按namespace分）。

每一个namespace的mapper都有一个二缓存区域，两个mapper的namespace如果相同，这两个mapper执行sql查询到数据将存在相同的二级缓存区域中。

1.3.2    开启二级缓存
mybaits的二级缓存是mapper范围级别，除了在SqlMapConfig.xml设置二级缓存的总开关，还要在具体的mapper.xml中开启二级缓存。

 

在核心配置文件SqlMapConfig.xml中加入

<setting name="cacheEnabled"value="true"/>

<!-- 全局配置参数，需要时再设置 -->

    <settings>
     
       <!-- 开启二级缓存  默认值为true -->
     
    <setting name="cacheEnabled" value="true"/>
     
    </settings>

 












描述

允许值

默认值

cacheEnabled

对在此配置文件下的所有cache 进行全局性开/关设置。

true false

true

 

在UserMapper.xml中开启二缓存，UserMapper.xml下的sql执行完成会存储到它的缓存区域（HashMap）。

<mapper namespace="cn.hpu.mybatis.mapper.UserMapper">

<!-- 开启本mapper namespace下的二级缓存 -->

<cache></cache>

1.3.3    调用pojo类实现序列化接口

public class Userimplements Serializable {

    //Serializable实现序列化，为了将来反序列化

二级缓存需要查询结果映射的pojo对象实现java.io.Serializable接口实现序列化和反序列化操作，注意如果存在父类、成员pojo都需要实现序列化接口。

pojo类实现序列化接口是为了将缓存数据取出执行反序列化操作，因为二级缓存数据存储介质多种多样，不一定在内存有可能是硬盘或者远程服务器。

1.3.4    测试方法
// 二级缓存测试

   @Test

   public void testCache2() throws Exception {

      SqlSessionsqlSession1 = sqlSessionFactory.openSession();
     
      SqlSessionsqlSession2 = sqlSessionFactory.openSession();
     
      SqlSessionsqlSession3 = sqlSessionFactory.openSession();
     
      // 创建代理对象
     
      UserMapperuserMapper1 = sqlSession1.getMapper(UserMapper.class);
     
      // 第一次发起请求，查询id为1的用户
     
      Useruser1 = userMapper1.findUserById(1);
     
      System.out.println(user1);


​     

      //这里执行关闭操作，将sqlsession中的数据写到二级缓存区域
     
      sqlSession1.close();


​     

      //使用sqlSession3执行commit()操作
     
      UserMapperuserMapper3 = sqlSession3.getMapper(UserMapper.class);
     
      Useruser  = userMapper3.findUserById(1);
     
      user.setUsername("张明明");
     
      userMapper3.updateUser(user);
     
      //执行提交，清空UserMapper下边的二级缓存
     
      sqlSession3.commit();
     
      sqlSession3.close();


​     

      UserMapperuserMapper2 = sqlSession2.getMapper(UserMapper.class);
     
      // 第二次发起请求，查询id为1的用户
     
      Useruser2 = userMapper2.findUserById(1);
     
      System.out.println(user2);

 












      sqlSession2.close();

   }
1.3.5  useCache配置禁用二级缓存
在statement中设置useCache=false可以禁用当前select语句的二级缓存，即每次查询都会发出sql去查询，默认情况是true，即该sql使用二级缓存。

<selectid="findOrderListResultMap" resultMap="ordersUserMap" useCache="false">
总结：针对每次查询都需要最新的数据sql，要设置成useCache=false，禁用二级缓存。 

1.3.6    mybatis刷新缓存（就是清空缓存）
在mapper的同一个namespace中，如果有其它insert、update、delete操作数据后需要刷新缓存，如果不执行刷新缓存会出现脏读。

 设置statement配置中的flushCache="true" 属性，默认情况下为true即刷新缓存，如果改成false则不会刷新。使用缓存时如果手动修改数据库表中的查询数据会出现脏读。

如下：

<insertid="insertUser" parameterType="cn.itcast.mybatis.po.User" flushCache="true">


总结：一般下执行完commit操作都需要刷新缓存，flushCache=true表示刷新缓存默认情况下为true,我们不用去设置它，这样可以避免数据库脏读。

1.3.7  Mybatis Cache参数
flushInterval（刷新间隔）可以被设置为任意的正整数，而且它们代表一个合理的毫秒形式的时间段。默认情况是不设置，也就是没有刷新间隔，缓存仅仅调用语句时刷新。

size（引用数目）可以被设置为任意正整数，要记住你缓存的对象数目和你运行环境的可用内存资源数目。默认值是1024。

readOnly（只读）属性可以被设置为true或false。只读的缓存会给所有调用者返回缓存对象的相同实例。因此这些对象不能被修改。这提供了很重要的性能优势。可读写的缓存会返回缓存对象的拷贝（通过序列化）。这会慢一些，但是安全，因此默认是false。

如下例子：

<cache  eviction="FIFO" flushInterval="60000"  size="512" readOnly="true"/>
这个更高级的配置创建了一个 FIFO 缓存,并每隔 60 秒刷新,存数结果对象或列表的 512 个引用,而且返回的对象被认为是只读的,因此在不同线程中的调用者之间修改它们会导致冲突。可用的收回策略有, 默认的是 LRU:

1.      LRU – 最近最少使用的:移除最长时间不被使用的对象。

2.      FIFO – 先进先出:按对象进入缓存的顺序来移除它们。

3.      SOFT – 软引用:移除基于垃圾回收器状态和软引用规则的对象。

4.      WEAK – 弱引用:更积极地移除基于垃圾收集器状态和弱引用规则的对象。

1.4  mybatis整合ehcache
ehcache是一个分布式缓存框架。

EhCache 是一个纯Java的进程内缓存框架，是一种广泛使用的开源Java分布式缓存，具有快速、精干等特点，是Hibernate中默认的CacheProvider。

1.4.1    分布缓存
我们系统为了提高系统并发，性能、一般对系统进行分布式部署（集群部署方式）



 

不使用分布缓存，缓存的数据在各各服务单独存储，不方便系统开发。所以要使用分布式缓存对缓存数据进行集中管理。

mybatis无法实现分布式缓存，需要和其它分布式缓存框架进行整合。

1.4.2    整合方法(掌握无论整合谁，首先想到改type接口)
mybatis提供了一个cache接口，如果要实现自己的缓存逻辑，实现cache接口开发即可。

mybatis和ehcache整合，mybatis和ehcache整合包中提供了一个cache接口的实现类。

1.4.3    第一步加入ehcache包




1.4.4    整合ehcache
配置mapper中cache中的type为ehcache对cache接口的实现类型。

<mapper namespace="cn.hpu.mybatis.mapper.UserMapper">

<!-- 开启本mapper namespace下的二级缓存
   type：指定cache接口实现类，mybatis默认使用PerpetualCache
   要和eache整合，需要配置type为ehcahe实现cache接口的类型
 -->

<cache type="org.mybatis.caches.ehcache.EhcacheCache">

</cache>


可以根据需求调整缓存参数：

<cache type="org.mybatis.caches.ehcache.EhcacheCache">

        <property name="timeToIdleSeconds" value="3600"/>
     
        <property name="timeToLiveSeconds" value="3600"/>
     
        <!-- 同ehcache参数maxElementsInMemory-->
     
       <property name="maxEntriesLocalHeap"value="1000"/>
     
       <!-- 同ehcache参数maxElementsOnDisk -->
     
        <property name="maxEntriesLocalDisk" value="10000000"/>
     
        <property name="memoryStoreEvictionPolicy" value="LRU"/>
     
    </cache>
1.4.5    加入ehcache的配置文件
在classpath下配置ehcache.xml

<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

    xsi:noNamespaceSchemaLocation="../config/ehcache.xsd">
     
    <diskStore path="F:\develop\ehcache"/>
     
    <defaultCache

maxElementsInMemory="1000"

       maxElementsOnDisk="10000000"
     
       eternal="false"
     
       overflowToDisk="false"
     
       timeToIdleSeconds="120"
     
       timeToLiveSeconds="120"
     
       diskExpiryThreadIntervalSeconds="120"
     
       memoryStoreEvictionPolicy="LRU">
     
    </defaultCache>

</ehcache>
属性说明：

 diskStore：指定数据在磁盘中的存储位置。

 defaultCache：当借助CacheManager.add("demoCache")创建Cache时，EhCache便会采用<defalutCache/>指定的的管理策略

以下属性是必须的：

maxElementsInMemory - 在内存中缓存的element的最大数目

maxElementsOnDisk - 在磁盘上缓存的element的最大数目，若是0表示无穷大

 eternal - 设定缓存的elements是否永远不过期。如果为true，则缓存的数据始终有效，如果为false那么还要根据timeToIdleSeconds，timeToLiveSeconds判断

 overflowToDisk- 设定当内存缓存溢出的时候是否将过期的element缓存到磁盘上

以下属性是可选的：

timeToIdleSeconds - 当缓存在EhCache中的数据前后两次访问的时间超过timeToIdleSeconds的属性取值时，这些数据便会删除，默认值是0,也就是可闲置时间无穷大

timeToLiveSeconds - 缓存element的有效生命期，默认是0.,也就是element存活时间无穷大

       diskSpoolBufferSizeMB 这个参数设置DiskStore(磁盘缓存)的缓存区大小.默认是30MB.每个Cache都应该有自己的一个缓冲区.

diskPersistent在VM重启的时候是否启用磁盘保存EhCache中的数据，默认是false。

diskExpiryThreadIntervalSeconds - 磁盘缓存的清理线程运行间隔，默认是120秒。每个120s，相应的线程会进行一次EhCache中数据的清理工作

memoryStoreEvictionPolicy - 当内存缓存达到最大，有新的element加入的时候，移除缓存中element的策略。默认是LRU（最近最少使用），可选的有LFU（最不常使用）和FIFO（先进先出）

1.5  二级应用场景
对于访问多的查询请求且用户对查询结果实时性要求不高，此时可采用mybatis二级缓存技术降低数据库访问量，提高访问速度，业务场景比如：耗时较高的统计分析sql、电话账单查询sql等。

         实现方法如下：通过设置刷新间隔时间，由mybatis每隔一段时间自动清空缓存，根据数据变化频率设置缓存刷新间隔flushInterval，比如设置为30分钟、60分钟、24小时等，根据需求而定。

1.6  二级缓存局限性
         mybatis二级缓存对细粒度的数据级别的缓存实现不好，对同时缓存较多条数据的缓存，比如如下需求：对商品信息进行缓存，由于商品信息查询访问量大，但是要求用户每次都能查询最新的商品信息，此时如果使用mybatis的二级缓存就无法实现当一个商品变化时只刷新该商品的缓存信息而不刷新其它商品的信息，因为mybaits的二级缓存区域以mapper为单位划分，当一个商品信息变化会将所有商品信息的缓存数据全部清空。解决此类问题需要在业务层根据需求对数据有针对性缓存。需要使用三级缓存
————————————————
版权声明：本文为CSDN博主「双斜杠少年」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/u012373815/article/details/47069223

# 为什么Mapper类不需要实现

https://blog.csdn.net/puhaiyang/article/details/77418012

# Mybatis中处理器

https://blog.csdn.net/a1032722788/article/details/115678866

# JWT详解

JSON Web Token（缩写 JWT）是目前最流行的跨域认证解决方案，本文介绍它的原理和用法。

![img](https://www.wangbase.com/blogimg/asset/201807/bg2018072301.jpg)

## 一、跨域认证的问题

互联网服务离不开用户认证。一般流程是下面这样。

> 1、用户向服务器发送用户名和密码。
>
> 2、服务器验证通过后，在当前对话（session）里面保存相关数据，比如用户角色、登录时间等等。
>
> 3、服务器向用户返回一个 session_id，写入用户的 Cookie。
>
> 4、用户随后的每一次请求，都会通过 Cookie，将 session_id 传回服务器。
>
> 5、服务器收到 session_id，找到前期保存的数据，由此得知用户的身份。

这种模式的问题在于，扩展性（scaling）不好。单机当然没有问题，如果是服务器集群，或者是跨域的服务导向架构，就要求 session 数据共享，每台服务器都能够读取 session。

举例来说，A 网站和 B 网站是同一家公司的关联服务。现在要求，用户只要在其中一个网站登录，再访问另一个网站就会自动登录，请问怎么实现？

一种解决方案是 session 数据持久化，写入数据库或别的持久层。各种服务收到请求后，都向持久层请求数据。这种方案的优点是架构清晰，缺点是工程量比较大。另外，持久层万一挂了，就会单点失败。

另一种方案是服务器索性不保存 session 数据了，所有数据都保存在客户端，每次请求都发回服务器。JWT 就是这种方案的一个代表。

## 二、JWT 的原理

JWT 的原理是，服务器认证以后，生成一个 JSON 对象，发回给用户，就像下面这样。

> ```javascript
> {
>   "姓名": "张三",
>   "角色": "管理员",
>   "到期时间": "2018年7月1日0点0分"
> }
> ```

以后，用户与服务端通信的时候，都要发回这个 JSON 对象。服务器完全只靠这个对象认定用户身份。为了防止用户篡改数据，服务器在生成这个对象的时候，会加上签名（详见后文）。

服务器就不保存任何 session 数据了，也就是说，服务器变成无状态了，从而比较容易实现扩展。

## 三、JWT 的数据结构

实际的 JWT 大概就像下面这样。

![img](https://www.wangbase.com/blogimg/asset/201807/bg2018072304.jpg)

它是一个很长的字符串，中间用点（`.`）分隔成三个部分。注意，JWT 内部是没有换行的，这里只是为了便于展示，将它写成了几行。

JWT 的三个部分依次如下。

> - Header（头部）
> - Payload（负载）
> - Signature（签名）

写成一行，就是下面的样子。

> ```javascript
> Header.Payload.Signature
> ```

![img](https://www.wangbase.com/blogimg/asset/201807/bg2018072303.jpg)

下面依次介绍这三个部分。

### 3.1 Header

Header 部分是一个 JSON 对象，描述 JWT 的元数据，通常是下面的样子。

> ```javascript
> {
>   "alg": "HS256",
>   "typ": "JWT"
> }
> ```

上面代码中，`alg`属性表示签名的算法（algorithm），默认是 HMAC SHA256（写成 HS256）；`typ`属性表示这个令牌（token）的类型（type），JWT 令牌统一写为`JWT`。

最后，将上面的 JSON 对象使用 Base64URL 算法（详见后文）转成字符串。

### 3.2 Payload

Payload 部分也是一个 JSON 对象，用来存放实际需要传递的数据。JWT 规定了7个官方字段，供选用。

> - iss (issuer)：签发人
> - exp (expiration time)：过期时间
> - sub (subject)：主题
> - aud (audience)：受众
> - nbf (Not Before)：生效时间
> - iat (Issued At)：签发时间
> - jti (JWT ID)：编号

除了官方字段，你还可以在这个部分定义私有字段，下面就是一个例子。

> ```javascript
> {
>   "sub": "1234567890",
>   "name": "John Doe",
>   "admin": true
> }
> ```

注意，JWT 默认是不加密的，任何人都可以读到，所以不要把秘密信息放在这个部分。

这个 JSON 对象也要使用 Base64URL 算法转成字符串。

### 3.3 Signature

Signature 部分是对前两部分的签名，防止数据篡改。

首先，需要指定一个密钥（secret）。这个密钥只有服务器才知道，不能泄露给用户。然后，使用 Header 里面指定的签名算法（默认是 HMAC SHA256），按照下面的公式产生签名。

> ```javascript
> HMACSHA256(
>   base64UrlEncode(header) + "." +
>   base64UrlEncode(payload),
>   secret)
> ```

算出签名以后，把 Header、Payload、Signature 三个部分拼成一个字符串，每个部分之间用"点"（`.`）分隔，就可以返回给用户。

### 3.4 Base64URL

前面提到，Header 和 Payload 串型化的算法是 Base64URL。这个算法跟 Base64 算法基本类似，但有一些小的不同。

JWT 作为一个令牌（token），有些场合可能会放到 URL（比如 api.example.com/?token=xxx）。Base64 有三个字符`+`、`/`和`=`，在 URL 里面有特殊含义，所以要被替换掉：`=`被省略、`+`替换成`-`，`/`替换成`_` 。这就是 Base64URL 算法。

## 四、JWT 的使用方式

客户端收到服务器返回的 JWT，可以储存在 Cookie 里面，也可以储存在 localStorage。

此后，客户端每次与服务器通信，都要带上这个 JWT。你可以把它放在 Cookie 里面自动发送，但是这样不能跨域，所以更好的做法是放在 HTTP 请求的头信息`Authorization`字段里面。

> ```javascript
> Authorization: Bearer <token>
> ```

另一种做法是，跨域的时候，JWT 就放在 POST 请求的数据体里面。

## 五、JWT 的几个特点

（1）JWT 默认是不加密，但也是可以加密的。生成原始 Token 以后，可以用密钥再加密一次。

（2）JWT 不加密的情况下，不能将秘密数据写入 JWT。

（3）JWT 不仅可以用于认证，也可以用于交换信息。有效使用 JWT，可以降低服务器查询数据库的次数。

（4）JWT 的最大缺点是，由于服务器不保存 session 状态，因此无法在使用过程中废止某个 token，或者更改 token 的权限。也就是说，一旦 JWT 签发了，在到期之前就会始终有效，除非服务器部署额外的逻辑。

（5）JWT 本身包含了认证信息，一旦泄露，任何人都可以获得该令牌的所有权限。为了减少盗用，JWT 的有效期应该设置得比较短。对于一些比较重要的权限，使用时应该再次对用户进行认证。

（6）为了减少盗用，JWT 不应该使用 HTTP 协议明码传输，要使用 HTTPS 协议传输。

## 六、参考链接

- [Introduction to JSON Web Tokens](https://jwt.io/introduction/)， by Auth0
- [Sessionless Authentication using JWTs (with Node + Express + Passport JS)](https://medium.com/@bryanmanuele/sessionless-authentication-withe-jwts-with-node-express-passport-js-69b059e4b22c), by Bryan Manuele
- [Learn how to use JSON Web Tokens](https://github.com/dwyl/learn-json-web-tokens/blob/master/README.md), by dwyl

# JWT防止CSRF攻击

JWT只是一个身份验证的凭证，和你用去防范CSRF并不矛盾，你完全可以在JWT之上加上防范CSRF的措施，比如检查Referer字段和添加校验token（即CSRF Token）。

### 检查Referer字段

> HTTP头中有一个Referer字段，这个字段用以标明请求来源于哪个地址。在处理敏感数据请求时，通常来说，Referer字段应和请求的地址位于同一域名下。以上文银行操作为例，Referer字段地址通常应该是转账按钮所在的网页地址，应该也位于www.examplebank.com之下。而如果是CSRF攻击传来的请求，Referer字段会是包含恶意网址的地址，不会位于www.examplebank.com之下，这时候服务器就能识别出恶意的访问。
>
> 这种办法简单易行，工作量低，仅需要在关键访问处增加一步校验。但这种办法也有其局限性，因其完全依赖浏览器发送正确的Referer字段。虽然http协议对此字段的内容有明确的规定，但并无法保证来访的浏览器的具体实现，亦无法保证浏览器没有安全漏洞影响到此字段。并且也存在攻击者攻击某些浏览器，篡改其Referer字段的可能。

### 添加校验token

> 由于CSRF的本质在于攻击者欺骗用户去访问自己设置的地址，所以如果要求在访问敏感数据请求时，要求用户浏览器提供不保存在cookie中，并且攻击者无法伪造的数据作为校验，那么攻击者就无法再执行CSRF攻击。这种数据通常是表单中的一个数据项。服务器将其生成并附加在表单中，其内容是一个伪乱数。当客户端通过表单提交请求时，这个伪乱数也一并提交上去以供校验。正常的访问时，客户端浏览器能够正确得到并传回这个伪乱数，而通过CSRF传来的欺骗性攻击中，攻击者无从事先得知这个伪乱数的值，服务器端就会因为校验token的值为空或者错误，拒绝这个可疑请求。

CSRF攻击的大前提是***攻击者获取不到用户的cookie\***，但是通过恶意链接诱使用户通过攻击者的链接提交请求给相关接口，cookie的值是默认携带的，而jwt的值***需要通过前端页面的js代码\***提取出并主动提交给后端进行验证，而攻击者获取不到用户的jwt，更无法通过js的方式将它取出来，因此jwt可以防止CSRF攻击。
简单来说，CSRF攻击意味着请求没有来自正确的前端页面，而jwt需要前端***主动\***将它取出。

# 二叉堆

堆排序(Heapsort)是指利用堆积树（堆）这种数据结构所设计的一种排序算法，它是选择排序的一种。可以利用数组的特点快速定位指定索引的元素。堆分为大根堆和小根堆，是完全二叉树。大根堆的要求是每个节点的值都不大于其父节点的值，即A[PARENT[i]] >= A[i]。在数组的非降序排序中，需要使用的就是大根堆，因为根据大根堆的要求可知，最大的值一定在堆顶。

**最小堆**

最小堆是一棵完全二叉树，非叶子结点的值不大于左孩子和右孩子的值。最小堆示例如下：

![img](https://pic3.zhimg.com/80/v2-cc07c80c175f03490953dfcb109a09be_720w.jpg)

**最小堆的构建**

从末尾节点的父节点的这棵树开始调整，根据小根堆的性质，越小的数据往上移动，注意，被调整的节点还有子节点的情况，需要递归进行调整。

![img](https://pic1.zhimg.com/80/v2-882c0c10e0315d820de4e6766951cb0c_720w.jpg)

![img](https://pic3.zhimg.com/80/v2-203b6807a37dc791b0f10fccbc431466_720w.jpg)

![img](https://pic3.zhimg.com/80/v2-f6f6457dfcce074a130daeb69d24b27a_720w.jpg)

![img](https://pic1.zhimg.com/80/v2-90c4c1eb1965b7d5260957069c2b4420_720w.jpg)

2和3交换之后，3所处的节点还有子节点，需要递归检验3所在树是否也符合小根堆的性质

![img](https://pic3.zhimg.com/80/v2-b3a5cfe7e5c6c9901b1703020204c9be_720w.jpg)

注意：此时9和1发生交换之后，9所处的节点具有子节点，递归调整9所处的树

![img](https://pic3.zhimg.com/80/v2-5dfcd9cff3c4f26889f040d610e4744e_720w.jpg)

**堆的存储**

一般都用数组来表示堆，i结点的父结点下标就为(i–1)/2。它的左右子结点下标分别为2 * i + 1和2 * i + 2。如第0个结点左右子结点下标分别为1和2。

![img](https://pic2.zhimg.com/80/v2-d14b55921b7a2c8a5ab78365a327a1d9_720w.jpg)

**堆的插入**

插入一个元素：新元素被加入到heap的末尾，然后更新树以恢复堆的次序。

每次插入都是将新数据放在数组最后。可以发现从这个新数据的父结点到根结点必然为一个有序的数列，现在的任务是将这个新数据插入到这个有序数据中——这就类似于直接插入排序中将一个数据并入到有序区间中。需要从下往上，与父节点的关键码进行比较，对调。

**堆的删除**

按定义，堆中每次都删除第0个数据。为了便于重建堆，**实际的操作是将最后一个数据的值赋给根结点，堆的元素个数-1，然后再从根结点开始进行一次从上向下的调整。**调整时先在左右儿子结点中找最小的，如果父结点比这个最小的子结点还小说明不需要调整了，反之将父结点和它交换后再考虑后面的结点。相当于从根结点将一个数据的“下沉”过程。

![img](https://pic1.zhimg.com/80/v2-9305a490e3b136e12dc633083ab8e4e8_720w.jpg)

**搜索性能**：二叉排序树是为了实现动态查找而涉及的数据结构，它是面向查找操作的，在二叉排序树中查找一个节点的平均时间复杂度是O(logn)；在堆中搜索并不是第一优先级，**堆是为了实现排序而实现的一种数据结构**，它不是面向查找操作的，因为在堆中查找一个节点需要进行遍历，其平均时间复杂度是O(n)。

Leecode题目: 滑动窗口最大值239

思路与算法

对于「最大值」，我们可以想到一种非常合适的数据结构，那就是优先队列（堆），其中的大根堆可以帮助我们实时维护一系列元素中的最大值。

> 对于本题而言，初始时，我们将数组 nums 的前 k 个元素放入优先队列中。每当我们向右移动窗口时，我们就可以把一个新的元素放入优先队列中，此时堆顶的元素就是堆中所有元素的最大值。**然而这个最大值可能并不在滑动窗口中，在这种情况下，这个值在数组 nums 中的位置出现在滑动窗口左边界的左侧。**因此，当我们后续继续向右移动窗口时，这个值就永远不可能出现在滑动窗口中了，我们可以将其永久地从优先队列中移除。
> 我们不断地移除堆顶的元素，直到其确实出现在滑动窗口中。此时，堆顶元素就是滑动窗口中的最大值。为了方便判断堆顶元素与滑动窗口的位置关系，我们可以在优先队列中存储二元组 (num,index)，表示元素num 在数组中的下标为 index。

[力扣leetcode-cn.com](https://link.zhihu.com/?target=https%3A//leetcode-cn.com/problems/sliding-window-maximum/)

```python3
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        # length=len(nums)
        # if k>=length:
        #     return [max(nums)]
        # res=[]
        # temp=[0]*k
        # for i in range(length-k+1):
        #     temp=nums[i:i+k]
        #     res.append(max(temp))
        # return res

        n = len(nums)
        # 注意 Python 默认的优先队列是小根堆
        q = [(-nums[i], i) for i in range(k)]
        heapq.heapify(q)

        ans = [-q[0][0]]
        for i in range(k, n):
            heapq.heappush(q, (-nums[i], i))
            while q[0][1] <= i - k:
                heapq.heappop(q)
            ans.append(-q[0][0])
        
        return ans
```

# 面经总结

枚举类型比较 Iterable接口

作者：想要变大的杰杰
链接：https://www.nowcoder.com/discuss/440137?type=2
来源：牛客网

# 字节跳动实习面试

## 5.15 一面（3点开始，80分钟）

1.自我介绍，聊了聊学校近况

2.Java 集合框架，看了哪些[源码]()，arraylist、linkedlist原理，让你实现一个 hashmap 机会如何设计（没让手写😂）

> ArrayList
>
> 先说说Arraylist，**Arraylist是基于动态数组实现的，所以查找速度快**，但是增删操作的速度会比较慢，但是为什么会这样？我解释一下动态数组，基本就可以明白这个问题了。
>
> 先说说静态数组是怎么来存储数据的，当我们使用new来创建一个数组，实际上是在堆上申请了一段连续的大内存，我们知道我们在java中创建数组的时候，会给他一个固定的大小，不能适应数据的动态增删，那么这个时候就有了所谓的动态数组，**动态数组的本质就是，当数据超出当前数组的内存范围，会先试着比较数据长度和数组长度的1.5倍的大小，如果1.5倍大于元素长度(小于的话，则创建一个长度为数据长度的数组)，那么直接通过Arrays.copyOf得到一个新长度的数组，把原数据拷贝过去，释放掉旧的数组， 这个时候内存有很多是没有使用的，这个时候就可以执行增删操作**。
>
> 在动态数组中，存储数据用的是一段连续的大内存，所以如果我们要在某一个位置添加或者删除一个元素，剩下的每个元素都要相应地往前或往后移动。如果该动态数组中的元素很多，那么，每当我们添加或删除一个元素后，需要移动的元素就非常多，因此，效率就比较低。而查找的时候，由于每个元素占用内存相同，可以通过下标迅速访问数组中任何元素。
>
> 这就是为什么ArrayList的查找效率高，而增删操作的效率低了。
>
> LinkedList
>
> **LinkedList是基于双向链表的数据结构实现的，链表是可以占用一段不连续的内存空间的**，我盗用一下别人的图来说明一下，双向链表有前驱节点和后驱节点，里面存储的是上一个元素和后一个元素所在的位置，中间的黄色部分就是业务数据了，当我们需要执行插入的任务，比如第一个元素和第二个元素之间，只需要改变他们的前驱节点和后驱节点的指向就可以了，不要像动态数组那么麻烦，删除也是同样的操作，但是因为是不连续的内存空间，当需要执行查找，需要从第一个元素开始查找，直到找到我们需要的数据。
>
> ![img](https://img-blog.csdn.net/20180105104402234?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM4NDIyMjc2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
>
> 这就是为什么LinkedList查找效率低，增删效率更高。
>
> 上面的分析也能看出，其实LinkedList更加节省内存空间，而ArrayList需要预留部分空间出来增加数据。
>
> 而我自己也有疑问，那么在末尾添加数据，谁的效率更高?我倾向于Arraylist，但是又不明白为什么，等我查清楚了，再来加吧..
> ————————————————
> 版权声明：本文为CSDN博主「花儿与代码」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/qq_35960638/article/details/81105746
>
> # 1. 前言
>
> HashMap作为java数据结构中重要的一员，同时拥有高效的查询和插入的优点。近期在做项目的时候我也是领略到了hashmap的强大之处，hashmap为前后台数据的交互提供了非常高的可扩展性和灵活性。键值对的数据结构也天然对json字符串的转换提供良好的支持。这篇文章主要分析HashMap的原理，以及如何自己实现一个HashMap。
>
> # 2. HashMap的原理
>
> ## 2.1 什么是HashMap？
>
> 基于哈希表的 Map 接口的实现。此实现提供所有可选的映射操作，并允许使用 null 值和 null 键。（除了非同步和允许使用 null 之外，HashMap 类与 Hashtable 大致相同。）此类不保证映射的顺序，特别是它不保证该顺序恒久不变 ——摘自百度百科。
>
> ## 2.2 什么是Hash
>
> Hash是一种算法，能将一个任意长度的二进制值通过一个映射关系转换成一个固定长度的二进制值。在HashMap中，哈希算法主要是用于根据key的值算出存放数组的index值。HashMap中的存储都是根据key的哈希值有关的。
> 哈希算法又被称为散列的过程，那么什么是散列呢？散列即把数据均匀的存放到数组中的各个位置，从而尽量避免出现多个数据存放在一块区间内。
>
> 优秀的哈希算法应该具备以下两点：
>
> - 保证散列值非常均匀
> - 保证冲突极少出现
>
> 哈希函数有很多种，在这里我以除留取余法（取模）为例，首先定义一个数组的长度，假设为16。那么此时的索引为key摸于m，m的取值规则是比数组长度小的最大质数。在这个情况下m为13。由于这种算法会导致这样的情况出现，即不同的key经过哈希运算之后得到了一样的index，如key为2和15的index值都为2，那么此时就需要我们处理冲突了。
>
> Ps. JDK中的hash算法远比这个复杂，在这里我仅仅只是举一个例子方便读者理解。
>
> ## 2.3 处理冲突
>
> - 线性探测法
>   当冲突产生时，查找下一个索引是否被占用，如果没有，则把数据存到该索引上。
> - 链表形式
>   由于在HashMap中，单个数据是以entry的形式存储的，而entry中包含了key，value和next指针。那么当冲突产生时，我们就把原先存放到这个位置的数据取出来，然后在这个位置存放新的数据，并且把新数据的next指针设为原数据，也就是说链表头位置的数据永远是最新的数据。
>
> # 3. 实现自己的HashMap
>
> ## 3.1 Map接口
>
> HashMap的顶层接口是Map，那么我们自己实现的Map也需要一个接口，在这里我定义接口的名称为MyMap。这个接口中应该含有的方法包括：
>
> - put(K k,V v)
> - get(K k)
> - size()
>
> 和一个内部接口
>
> - Entry
>
> 这个内部接口中包含两个方法
>
> - getKey()
> - getValue()
>
> 代码如下
>
> ```
> public interface MyMap<K,V> {
>     /**
>      * put方法
>      * @param k
>      * @param v
>      * @return
>      */
>     V put(K k, V v);
>     /**
>      * get方法
>      * @param k
>      * @return
>      */
>     V get(K k);
> 
>     int size();
>     /**
>      * Entry内部接口
>      * @param <K>
>      * @param <V>
>      */
>     interface Entry<K, V>{
>         /**
>          * 根据entry对象获取key值
>          * @return
>          */
>         K getKey();
>         /**
>          * 根据entry对象获取value值
>          * @return
>          */
>         V getValue();
> 
>     }
> }
> ```
>
> 
>
> ## 3.2 内部类Entry的实现
>
> 接口设计完毕之后，我们需要创建一个类来实现这个接口的方法。我创建了一个名为MyHashMap的类实现MyMap接口。
>
> 这里我们需要一个内部类来实现MyMap的内部接口，内部类的实例对象即数组中存储的entry对象，所以我们需要定义三个成员变量，分别是K，V和Next。next的类型就是entry本身，因为它指向的是下一个entry对象。
>
> 内部类代码如下
>
> ```
> class Entry<K, V> implements  MyMap.Entry<K, V> {
> 
>         K k;
>         
>         V v;
>         
>         Entry<K, V> next;
>         
>         public Entry(K k, V v, Entry next) {
>             this.k = k;
>             this.v = v;
>             this.next = next;
>         }
> 
>         @Override
>         public K getKey() {
>             return k;
>         }
> 
>         @Override
>         public V getValue() {
>             return v;
>         }
>     }
> ```
>
> 
>
> ## 3.3 定义成员变量
>
> HashMap中含有以下几个成员变量：
>
> - 默认数组长度
> - 默认负载因子
> - Entry数组
> - HashMap的大小
>
> ```
> //默认数组大小，初始大小为16
> private static int defaultLength = 16;
> //默认负载因子，为0.75
> private static double defaultLoader = 0.75;
> //Entry数组
> private Entry<K, V>[] table = null;
> //HashMap的大小
> private int size = 0;
> ```
>
> ## 3.4 定义构造方法
>
> 在HashMap中默认数组长度和默认负载因子都是可以自定义的，那么我们定义一个可以自定义数组长度和负载因子的构造方法。
>
> ```
>  /**
>  * 自定义默认长度和负载因子
>  * @param length
>  * @param loader
>  */
> public MyHashMap(int length, double loader) {
>     defaultLength = length;
>     defaultLoader = loader;
>     //初始化数组
>     table = new Entry[defaultLength];
> }
> ```
>
> 
>
> 如果不指定数组长度和负载因子则使用默认值
>
> ```
> /**
>  * 使用默认值
>  */
> public MyHashMap() {
>     this(defaultLength, defaultLoader);
> }
> ```
>
> 
>
> ## 3.5定义哈希函数
>
> 上文已经提过了，哈希函数我们使用除留取余法。定义一个整型m，m的取值应该是一个比数组长度小的最大质数，为了简化算法我取数组的长度作为m的值。以key的哈希值模于m，得到index的值并且返回。
>
> ```
>  /**
>  * 自定义哈希算法
>  * 根据key的哈希值得到一个index索引，即存放到数组中的下标
>  * @param k
>  * @return
>  */
> private int getKey(K k) {
>     int m = defaultLength;
>     int index = k.hashCode() % m;
>     return index >= 0 ? index : -index;
> }
> ```
>
> 最后返回的时候用了一个三元运算符，是为了要确保index的值必须是一个正数。
>
> ## 3.6 实现put方法
>
> 首先，我们需要通过哈希算法得到数组的下标，然后把一个包含键值对以及next指针的entry对象存到该位置中。
>
> 在存入数组之前我们需要判断当前索引中是否已经存在数据。根据不同情况，做出不同的存储处理，代码中的注释有详细的解释。
>
> ```
> public V put(K k, V v) {
>         //根据key和哈希算法算出数组下标
>         int index = getKey(k);
>         Entry<K, V> entry = table[index];
>         //判断entry是否为空
>         if (entry == null) {
>             /*
>             * 如果entry为空，则代表当前位置没有数据。
>             * new一个entry对象，内部存放key，value。
>             * 此时next指针没有值，因为这个位置上只有一个entry对象
>             * */
>             table[index] = new Entry(k, v, null);
>             //map的大小加1
>             size++;
>         } else {
>             /*
>             * 如果entry不为空，则代表当前位置已经有数据了
>             * new一个entry对象，内部存放key，value。
>             * 把next指针设置为原本的entry对象并且把数组中的数据替换为新的entry对象
>             * */
>             table[index] = new Entry<K, V>(k, v, entry);
>         }
>         return table[index].getValue();
>     }
> ```
>
> ## 3.7 实现HashMap的扩容
>
> HashMap的扩容机制是：当map的大小大于默认长度*默认负载因子，那么数组的长度会翻倍，数组中的数据会重新散列然后再存放。那么在原先的put方法中，需要先判断是否达到扩容的标准在进行执行下面的代码。如果达到扩容的标准则需要调用扩容的方法。代码如下：
>
> ```
> //数组的扩容
> private void expand() {
>     //创建一个大小是原来两倍的entry数组
>     Entry<K, V>[] newTable = new Entry[2 * defaultLength];
>     //重新散列
>     rehash(newTable);
> }
> //重新散列的过程
> private void rehash(Entry<K,V>[] newTable) {
>     //创建一个list用于装载HashMap中所有的entry对象
>     List<Entry<K, V>> list = new ArrayList<Entry<K, V>>();
> 
>     //遍历整个数组
>     for(int i=0; i<table.length;i++) {
>         //如果数组中的某个位置没有数据，则跳过
>         if (table[i] == null) {
>             continue;
>         }
>         //通过递归的方式将所有的entry对象装载到list中
>         findEntryByNext(table[i],list);
>         if (list.size() > 0) {
>             //把size重置
>             size = 0;
>             //把默认长度设置为原来的两倍
>             defaultLength = 2 * defaultLength;
> 
>             table = newTable;
>             for (Entry<K, V> entry : list) {
>                 if (entry.next != null) {
>                     //把所有entry的next指针置空
>                     entry.next = null;
>                 }
>                 //对新table进行散列
>                 put(entry.getKey(), entry.getValue());
>             }
>         }
>     }
> }
> 
> private void findEntryByNext(Entry<K, V> entry,List<Entry<K, V>> list ) {
>     if (entry != null && entry.next != null) {
>         list.add(entry);
>         //递归调用
>         findEntryByNext(entry.next,list);
>     }else {
>         list.add(entry);
>     }
> }
> ```
>
> 
>
> 因为在put方法中加上了对扩容标准的判断，原先的put方法需要修改
>
> ```
> public V put(K k, V v) {
>     //判断size是否达到扩容的标准
>     if (size >= defaultLength * defaultLoader) {
>         expand();
>     }
> 
>     //根据key和哈希算法算出数组下标
>     int index = getKey(k);
>     Entry<K, V> entry = table[index];
>     //判断entry是否为空
>     if (entry == null) {
>         /*
>         * 如果entry为空，则代表当前位置没有数据。
>         * new一个entry对象，内部存放key，value。
>         * 此时next指针没有值，因为这个位置上只有一个entry对象
>         * */
>         table[index] = new Entry(k, v, null);
>         size++;
> 
>     } else {
>         /*
>         * 如果entry不为空，则代表当前位置已经有数据了
>         * new一个entry对象，内部存放key，value。
>         * 把next指针设置为原本的entry对象并且把数组中的数据替换为新的entry对象
>         * */
>         table[index] = new Entry<K, V>(k, v, entry);
>     }
>     return table[index].getValue();
> }
> ```
>
> 
>
> ## 3.8 实现get方法
>
> 上文提到了，由于hash算法可能会导致相同的索引中包含了不同的entry对象，我们需要通过对比key值的方式来找到我们真正要的那个entry对象。代码如下：
>
> ```
> public V get(K k) {
>     //获取此key对应的entry对象所存放的索引index
>     int index = getKey(k);
>     //非空判断
>     if (table[index] == null) {
>         return null;
>     }
>     //调用方法找到真正的value值并返回。
>     return findValueByEqualKey(k,table[index]);
> }
> 
>  /**
>  *
>  * 通过递归比较key值的方式找到真正我们要找的value值
>  * @param k
>  * @param entry
>  * @return
>  */
> public V findValueByEqualKey(K k , Entry<K,V> entry) {
> 
>     /*
>     * 如果传进来的key等于这个entry的key值，说明这个就是我们要找的entry对象
>     * 那么直接返回这个entry的value
>     * */
>     if (k == entry.getKey() || k.equals(entry.getKey())) {
>         return entry.getValue();
>     } else {
>         /*
>         * 如果不相等，说明这个不是我们要找的entry对象，
>         * 通过递归的方式去比较它的next指针中的entry的key值，来找到真正的entry对象
>         * */
>         if (entry.next != null) {
>             return findValueByEqualKey(k, entry.next);
>         }
> 
>     }
>     return entry.getValue();
> }
> ```
>
> 
>
> 由于size方法非常简单，直接返回size即可，我直接贴一下代码
>
> ```
> //返回HashMap的大小
> public int size() {
>     return size;
> }
> ```
>
> 
>
> 到了这里，HashMap的基本功能已经都写完了，包括put方法、get方法和扩容机制。那么下面我们来测试一下看看自己写的map能否实现功能。
>
> # 4. 测试MyHashMap
>
> 写一个测试类，对比一下我们的HashMap和jdk自带的HashMap，看看输出是否一样。
>
> 测试类代码
>
> ```
> /**
>  * @Author: Yiheng Chen
>  * @Description: Map测试类
>  * @Date: Created in 4:33 2017/9/1
>  * @Modified by:
>  */
> public class MyMapTest {
>     public static void main(String[] args) {
> 
>         MyHashMap<String,String > map = new MyHashMap();
> 
>         Long t1 = System.currentTimeMillis();
>         for (int i=0; i<1000;i++) {
>             map.put("key" + i, "value" + i);
>         }
> 
>         for (int i = 0; i < 1000; i++) {
>             System.out.println("key: " + "key" + i +"---"+ "value: " + map.get("key" + i));
>         }
>         Long t2 = System.currentTimeMillis();
> 
>         System.out.println("MyHashMap耗时："+(t2-t1));
>         System.out.println("-------------------HashMap-------------------------" );
> 
>         Map<String,String > hashMap = new HashMap();
> 
>         Long t3 = System.currentTimeMillis();
>         for (int i=0; i<1000;i++) {
>             hashMap.put("key" + i, "value" + i);
>         }
> 
>         for (int i = 0; i < 1000; i++) {
>             System.out.println("key: " + "key" + i + "---"+ "value: " + hashMap.get("key" + i));
>         }
>         Long t4 = System.currentTimeMillis();
>         System.out.println("JDK的HashMap耗时："+(t4-t3));
> 
>     }
> }
> ```
>
> 部分结果的截取：
>
> ```
> key: key980---value: value980
> key: key981---value: value981
> key: key982---value: value982
> key: key983---value: value983
> key: key984---value: value984
> key: key985---value: value985
> key: key986---value: value986
> key: key987---value: value987
> key: key988---value: value988
> key: key989---value: value989
> key: key990---value: value990
> key: key991---value: value991
> key: key992---value: value992
> key: key993---value: value993
> key: key994---value: value994
> key: key995---value: value995
> key: key996---value: value996
> key: key997---value: value997
> key: key998---value: value998
> key: key999---value: value999
> ```
>
> 
>
> 可以发现MyHashMap的输出和JDK的HashMap是完全一样的，那么说明了这个自己实现的HashMap功能已经基本实现。
>
> 然后我又在这个基础上测试了一下耗时，结果如下：
>
> ```
> MyHashMap耗时：30
> -----------------------
> JDK的HashMap耗时：19
> ```
>
> 
>
> 性能上自然是JDK的HashMap更强大，这个毋庸置疑，JDK中的HashMap可以说是最优化的map实现了，我们自己实现的肯定不如人家的。但是这个性能已经是不错的了。
>
> 至此，我们已经完整的实现了一个自己的HashMap，希望这篇文章对你有帮助。

3.线程池的执行过程、核心参数以及常用的几个线程池（感觉每次面试都会问😂）

> 什么是线程池
> 线程池的概念大家应该都很清楚，帮我们重复管理线程，避免创建大量的线程增加开销。
>
> 除了降低开销以外，线程池也可以提高响应速度，了解点 JVM 的同学可能知道，一个对象的创建大概需要经过以下几步：
>
> 检查对应的类是否已经被加载、解析和初始化
> 类加载后，为新生对象分配内存
> 将分配到的内存空间初始为 0
> 对对象进行关键信息的设置，比如对象的哈希码等
> 然后执行 init 方法初始化对象
> 创建一个对象的开销需要经过这么多步，也是需要时间的嘛，那可以复用已经创建好的线程的线程池，自然也在提高响应速度上做了贡献。
>
> 线程池的处理流程
> 创建线程池需要使用 ThreadPoolExecutor 类，它的构造函数参数如下：
>
> public ThreadPoolExecutor(int corePoolSize,    //核心线程的数量
>                           int maximumPoolSize,    //最大线程数量
>                           long keepAliveTime,    //超出核心线程数量以外的线程空余存活时间
>                           TimeUnit unit,    //存活时间的单位
>                           BlockingQueue<Runnable> workQueue,    //保存待执行任务的队列
>                           ThreadFactory threadFactory,    //创建新线程使用的工厂
>                           RejectedExecutionHandler handler // 当任务无法执行时的处理器
>                           ) {...}
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 参数介绍如注释所示，要了解这些参数左右着什么，就需要了解线程池具体的执行方法ThreadPoolExecutor.execute:
>
> public void execute(Runnable command) {
>     if (command == null)
>         throw new NullPointerException();
>
>     int c = ctl.get();
>     //1.当前池中线程比核心数少，新建一个线程执行任务
>     if (workerCountOf(c) < corePoolSize) {   
>         if (addWorker(command, true))
>             return;
>         c = ctl.get();
>     }
>     //2.核心池已满，但任务队列未满，添加到队列中
>     if (isRunning(c) && workQueue.offer(command)) {   
>         int recheck = ctl.get();
>         if (! isRunning(recheck) && remove(command))    //如果这时被关闭了，拒绝任务
>             reject(command);
>         else if (workerCountOf(recheck) == 0)    //如果之前的线程已被销毁完，新建一个线程
>             addWorker(null, false);
>     }
>     //3.核心池已满，队列已满，试着创建一个新线程
>     else if (!addWorker(command, false))
>         reject(command);    //如果创建新线程失败了，说明线程池被关闭或者线程池完全满了，拒绝任务
> }
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9
> 10
> 11
> 12
> 13
> 14
> 15
> 16
> 17
> 18
> 19
> 20
> 21
> 22
> 23
> 可以看到，线程池处理一个任务主要分三步处理，代码注释里已经介绍了，我再用通俗易懂的例子解释一下：
>
> （线程比作员工，线程池比作一个团队，核心池比作团队中核心团队员工数，核心池外的比作外包员工）
>
> 有了新需求，先看核心员工数量超没超出最大核心员工数，还有名额的话就新招一个核心员工来做
> 需要获取全局锁
> 核心员工已经最多了，HR 不给批 HC 了，那这个需求只好攒着，放到待完成任务列表吧
> 如果列表已经堆满了，核心员工基本没机会搞完这么多任务了，那就找个外包吧
> 需要获取全局锁
> 如果核心员工 + 外包员工的数量已经是团队最多能承受人数了，没办法，这个需求接不了了
> 结合这张图，这回流程你明白了吗？
>
> 
>
> 由于 1 和 3 新建线程时需要获取全局锁，这将严重影响性能。因此 ThreadPoolExecutor 这样的处理流程是为了在执行 execute() 方法时尽量少地执行 1 和 3，多执行 2。
>
> 在 ThreadPoolExecutor 完成预热后（当前线程数不少于核心线程数），几乎所有的 execute() 都是在执行步骤 2。
>
> 前面提到的 ThreadPoolExecutor 构造函数的参数，分别影响以下内容：
>
> corePoolSize：核心线程池数量
> 在线程数少于核心数量时，有新任务进来就新建一个线程，即使有的线程没事干
> 等超出核心数量后，就不会新建线程了，空闲的线程就得去任务队列里取任务执行了
> maximumPoolSize：最大线程数量
> 包括核心线程池数量 + 核心以外的数量
> 如果任务队列满了，并且池中线程数小于最大线程数，会再创建新的线程执行任务
> keepAliveTime：核心池以外的线程存活时间，即没有任务的外包的存活时间
> 如果给线程池设置 allowCoreThreadTimeOut(true)，则核心线程在空闲时头上也会响起死亡的倒计时
> 如果任务是多而容易执行的，可以调大这个参数，那样线程就可以在存活的时间里有更大可能接受新任务
> workQueue：保存待执行任务的阻塞队列
> 不同的任务类型有不同的选择，下一小节介绍
> threadFactory：每个线程创建的地方
> 可以给线程起个好听的名字，设置个优先级啥的
> handler：饱和策略，大家都很忙，咋办呢，有四种策略
> CallerRunsPolicy：只要线程池没关闭，就直接用调用者所在线程来运行任务
> AbortPolicy：直接抛出 RejectedExecutionException 异常
> DiscardPolicy：悄悄把任务放生，不做了
> DiscardOldestPolicy：把队列里待最久的那个任务扔了，然后再调用 execute() 试试看能行不
> 我们也可以实现自己的 RejectedExecutionHandler 接口自定义策略，比如如记录日志什么的
> 保存待执行任务的阻塞队列
> 当线程池中的核心线程数已满时，任务就要保存到队列中了。
>
> 线程池中使用的队列是 BlockingQueue 接口，常用的实现有如下几种：
>
> ArrayBlockingQueue：基于数组、有界，按 FIFO（先进先出）原则对元素进行排序
> LinkedBlockingQueue：基于链表，按FIFO （先进先出） 排序元素
> 吞吐量通常要高于 ArrayBlockingQueue
> Executors.newFixedThreadPool() 使用了这个队列
> SynchronousQueue：不存储元素的阻塞队列
> 每个插入操作必须等到另一个线程调用移除操作，否则插入操作一直处于阻塞状态
> 吞吐量通常要高于 LinkedBlockingQueue
> Executors.newCachedThreadPool使用了这个队列
> PriorityBlockingQueue：具有优先级的、无限阻塞队列
> 关于阻塞队列的详细介绍请看这 2 篇：
> Java 阻塞队列源码分析（上）
> Java 阻塞队列源码分析（下）
>
> 创建自己的线程池
> 了解上面的内容后，我们就可以创建自己的线程池了。
>
> ①先定义线程池的几个关键属性的值：
>
> private static final int CORE_POOL_SIZE = Runtime.getRuntime().availableProcessors() * 2; // 核心线程数为 CPU 数＊2
> private static final int MAXIMUM_POOL_SIZE = 64;    // 线程池最大线程数
> private static final int KEEP_ALIVE_TIME = 1;    // 保持存活时间 1秒
> 1
> 2
> 3
> 设置核心池的数量为 CPU 数的两倍，一般是 4、8，好点的 16 个线程
> 最大线程数设置为 64
> 空闲线程的存活时间设置为 1 秒
> ②然后根据处理的任务类型选择不同的阻塞队列
>
> 如果是要求高吞吐量的，可以使用 SynchronousQueue 队列；如果对执行顺序有要求，可以使用 PriorityBlockingQueue；如果最大积攒的待做任务有上限，可以使用 LinkedBlockingQueue。
>
> private final BlockingQueue<Runnable> mWorkQueue = new LinkedBlockingQueue<>(128);
> 1
> ③然后创建自己的 ThreadFactory
>
> 在其中为每个线程设置个名称：
>
> private final ThreadFactory DEFAULT_THREAD_FACTORY = new ThreadFactory() {
>     private final AtomicInteger mCount = new AtomicInteger(1);
>
>     public Thread newThread(Runnable r) {
>         Thread thread = new Thread(r, TAG + " #" + mCount.getAndIncrement());
>         thread.setPriority(Thread.NORM_PRIORITY);
>         return thread;
>     }
> };
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9
> ④然后就可以创建线程池了
>
> private ThreadPoolExecutor mExecutor = new ThreadPoolExecutor(CORE_POOL_SIZE, MAXIMUM_POOL_SIZE, KEEP_ALIVE_TIME,
>         TimeUnit.SECONDS, mWorkQueue, DEFAULT_THREAD_FACTORY,
>         new ThreadPoolExecutor.DiscardOldestPolicy());
> 1
> 2
> 3
> 这里我们选择的饱和策略为 DiscardOldestPolicy，你可以可以创建自己的。
>
> ⑤完整代码：
>
> public class ThreadPoolManager {
>     private final String TAG = this.getClass().getSimpleName();
>     private static final int CORE_POOL_SIZE = Runtime.getRuntime().availableProcessors() * 2; // 核心线程数为 CPU数＊2
>     private static final int MAXIMUM_POOL_SIZE = 64;    // 线程队列最大线程数
>     private static final int KEEP_ALIVE_TIME = 1;    // 保持存活时间 1秒
>
>     private final BlockingQueue<Runnable> mWorkQueue = new LinkedBlockingQueue<>(128);
>     
>     private final ThreadFactory DEFAULT_THREAD_FACTORY = new ThreadFactory() {
>         private final AtomicInteger mCount = new AtomicInteger(1);
>     
>         public Thread newThread(Runnable r) {
>             Thread thread = new Thread(r, TAG + " #" + mCount.getAndIncrement());
>             thread.setPriority(Thread.NORM_PRIORITY);
>             return thread;
>         }
>     };
>     
>     private ThreadPoolExecutor mExecutor = new ThreadPoolExecutor(CORE_POOL_SIZE, MAXIMUM_POOL_SIZE, KEEP_ALIVE_TIME,
>             TimeUnit.SECONDS, mWorkQueue, DEFAULT_THREAD_FACTORY,
>             new ThreadPoolExecutor.DiscardOldestPolicy());
>     
>     private static volatile ThreadPoolManager mInstance = new ThreadPoolManager();
>     
>     public static ThreadPoolManager getInstance() {
>         return mInstance;
>     }
>     
>     public void addTask(Runnable runnable) {
>         mExecutor.execute(runnable);
>     }
>     
>     @Deprecated
>     public void shutdownNow() {
>         mExecutor.shutdownNow();
>     }
> }
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9
> 10
> 11
> 12
> 13
> 14
> 15
> 16
> 17
> 18
> 19
> 20
> 21
> 22
> 23
> 24
> 25
> 26
> 27
> 28
> 29
> 30
> 31
> 32
> 33
> 34
> 35
> 36
> 37
> 这样我们就有了自己的线程池。
>
> JDK 提供的线程池及使用场景
> JDK 为我们内置了五种常见线程池的实现，均可以使用 Executors 工厂类创建。
>
> 1.newFixedThreadPool
> public static ExecutorService newFixedThreadPool(int nThreads) {
>     return new ThreadPoolExecutor(nThreads, nThreads,
>                                   0L, TimeUnit.MILLISECONDS,
>                                   new LinkedBlockingQueue<Runnable>());
> }
> 1
> 2
> 3
> 4
> 5
> 不招外包，有固定数量核心成员的正常互联网团队。
>
> 可以看到，FixedThreadPool 的核心线程数和最大线程数都是指定值，也就是说当线程池中的线程数超过核心线程数后，任务都会被放到阻塞队列中。
>
> 此外 keepAliveTime 为 0，也就是多余的空余线程会被立即终止（由于这里没有多余线程，这个参数也没什么意义了）。
>
> 而这里选用的阻塞队列是 LinkedBlockingQueue，使用的是默认容量 Integer.MAX_VALUE，相当于没有上限。
>
> 因此这个线程池执行任务的流程如下：
>
> 线程数少于核心线程数，也就是设置的线程数时，新建线程执行任务
> 线程数等于核心线程数后，将任务加入阻塞队列
> 由于队列容量非常大，可以一直加加加
> 执行完任务的线程反复去队列中取任务执行
> FixedThreadPool 用于负载比较重的服务器，为了资源的合理利用，需要限制当前线程数量。
>
> 2.newSingleThreadExecutor
> public static ExecutorService newSingleThreadExecutor() {
>     return new FinalizableDelegatedExecutorService
>         (new ThreadPoolExecutor(1, 1,
>                                 0L, TimeUnit.MILLISECONDS,
>                                 new LinkedBlockingQueue<Runnable>()));
> }
> 1
> 2
> 3
> 4
> 5
> 6
> 不招外包，只有一个核心成员的创业团队。
>
> 从参数可以看出来，SingleThreadExecutor 相当于特殊的 FixedThreadPool，它的执行流程如下：
>
> 线程池中没有线程时，新建一个线程执行任务
> 有一个线程以后，将任务加入阻塞队列，不停加加加
> 唯一的这一个线程不停地去队列里取任务执行
> 听起来很可怜的样子 - -。
>
> SingleThreadExecutor 用于串行执行任务的场景，每个任务必须按顺序执行，不需要并发执行。
>
> 3.newCachedThreadPool
> public static ExecutorService newCachedThreadPool() {
>     return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
>                                   60L, TimeUnit.SECONDS,
>                                   new SynchronousQueue<Runnable>());
> }
> 1
> 2
> 3
> 4
> 5
> 全部外包，没活最多待 60 秒的外包团队。
>
> 可以看到，CachedThreadPool 没有核心线程，非核心线程数无上限，也就是全部使用外包，但是每个外包空闲的时间只有 60 秒，超过后就会被回收。
>
> CachedThreadPool 使用的队列是 SynchronousQueue，这个队列的作用就是传递任务，并不会保存。
>
> 因此当提交任务的速度大于处理任务的速度时，每次提交一个任务，就会创建一个线程。极端情况下会创建过多的线程，耗尽 CPU 和内存资源。
>
> 它的执行流程如下：
>
> 没有核心线程，直接向 SynchronousQueue 中提交任务
> 如果有空闲线程，就去取出任务执行；如果没有空闲线程，就新建一个
> 执行完任务的线程有 60 秒生存时间，如果在这个时间内可以接到新任务，就可以继续活下去，否则就拜拜
> 由于空闲 60 秒的线程会被终止，长时间保持空闲的 CachedThreadPool 不会占用任何资源。
>
> CachedThreadPool 用于并发执行大量短期的小任务，或者是负载较轻的服务器。
>
> 4.newScheduledThreadPool
> public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize) {
>     return new ScheduledThreadPoolExecutor(corePoolSize);
> }
> public ScheduledThreadPoolExecutor(int corePoolSize) {
>     super(corePoolSize, Integer.MAX_VALUE,
>           DEFAULT_KEEPALIVE_MILLIS, MILLISECONDS,
>           new DelayedWorkQueue());
> }
> private static final long DEFAULT_KEEPALIVE_MILLIS = 10L;
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9
> 定期维护的 2B 业务团队，核心与外包成员都有。
>
> ScheduledThreadPoolExecutor 继承自 ThreadPoolExecutor， 最多线程数为 Integer.MAX_VALUE ，使用 DelayedWorkQueue 作为任务队列。
>
> ScheduledThreadPoolExecutor 添加任务和执行任务的机制与ThreadPoolExecutor 有所不同。
>
> ScheduledThreadPoolExecutor 添加任务提供了另外两个方法：
>
> scheduleAtFixedRate() ：按某种速率周期执行
> scheduleWithFixedDelay()：在某个延迟后执行
> 它俩的代码如下：
>
> public ScheduledFuture<?> scheduleAtFixedRate(Runnable command,
>                                               long initialDelay,
>                                               long period,
>                                               TimeUnit unit) {
>     if (command == null || unit == null)
>         throw new NullPointerException();
>     if (period <= 0L)
>         throw new IllegalArgumentException();
>     ScheduledFutureTask<Void> sft =
>         new ScheduledFutureTask<Void>(command,
>                                       null,
>                                       triggerTime(initialDelay, unit),
>                                       unit.toNanos(period),
>                                       sequencer.getAndIncrement());
>     RunnableScheduledFuture<Void> t = decorateTask(command, sft);
>     sft.outerTask = t;
>     delayedExecute(t);
>     return t;
> }
> public ScheduledFuture<?> scheduleWithFixedDelay(Runnable command,
>                                                  long initialDelay,
>                                                  long delay,
>                                                  TimeUnit unit) {
>     if (command == null || unit == null)
>         throw new NullPointerException();
>     if (delay <= 0L)
>         throw new IllegalArgumentException();
>     ScheduledFutureTask<Void> sft =
>         new ScheduledFutureTask<Void>(command,
>                                       null,
>                                       triggerTime(initialDelay, unit),
>                                       -unit.toNanos(delay),
>                                       sequencer.getAndIncrement());
>     RunnableScheduledFuture<Void> t = decorateTask(command, sft);
>     sft.outerTask = t;
>     delayedExecute(t);
>     return t;
> }
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9
> 10
> 11
> 12
> 13
> 14
> 15
> 16
> 17
> 18
> 19
> 20
> 21
> 22
> 23
> 24
> 25
> 26
> 27
> 28
> 29
> 30
> 31
> 32
> 33
> 34
> 35
> 36
> 37
> 38
> 可以看到，这两种方法都是创建了一个 ScheduledFutureTask 对象，调用 decorateTask() 方法转成 RunnableScheduledFuture 对象，然后添加到队列中。
>
> 看下 ScheduledFutureTask 的主要属性：
>
> private class ScheduledFutureTask<V>
>         extends FutureTask<V> implements RunnableScheduledFuture<V> {
>
>     //添加到队列中的顺序
>     private final long sequenceNumber;
>     //何时执行这个任务
>     private volatile long time;
>     //执行的间隔周期
>     private final long period;
>     //实际被添加到队列中的 task
>     RunnableScheduledFuture<V> outerTask = this;
>     //在 delay queue 中的索引，便于取消时快速查找
>     int heapIndex;
>     //...
> }
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9
> 10
> 11
> 12
> 13
> 14
> 15
> DelayQueue 中封装了一个优先级队列，这个队列会对队列中的 ScheduledFutureTask 进行排序，两个任务的执行 time 不同时，time 小的先执行；否则比较添加到队列中的顺序 sequenceNumber ，先提交的先执行。
>
> ScheduledThreadPoolExecutor 的执行流程如下：
>
> 调用上面两个方法添加一个任务
> 线程池中的线程从 DelayQueue 中取任务
> 然后执行任务
> 具体执行任务的步骤也比较复杂：
>
> 线程从 DelayQueue 中获取 time 大于等于当前时间的 ScheduledFutureTask
> DelayQueue.take()
> 执行完后修改这个 task 的 time 为下次被执行的时间
> 然后再把这个 task 放回队列中
> DelayQueue.add()
> ScheduledThreadPoolExecutor 用于需要多个后台线程执行周期任务，同时需要限制线程数量的场景。
>
> 两种提交任务的方法
> ExecutorService 提供了两种提交任务的方法：
>
> execute()：提交不需要返回值的任务
> submit()：提交需要返回值的任务
> execute
> void execute(Runnable command);
> 1
> execute() 的参数是一个 Runnable，也没有返回值。因此提交后无法判断该任务是否被线程池执行成功。
>
> ExecutorService executor = Executors.newCachedThreadPool();
> executor.execute(new Runnable() {
>     @Override
>     public void run() {
>         //do something
>     }
> });
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> submit
> <T> Future<T> submit(Callable<T> task);
> <T> Future<T> submit(Runnable task, T result);
> Future<?> submit(Runnable task);
> 1
> 2
> 3
> submit() 有三种重载，参数可以是 Callable 也可以是 Runnable。
>
> 同时它会返回一个 Funture 对象，通过它我们可以判断任务是否执行成功。
>
> 获得执行结果调用 Future.get() 方法，这个方法会阻塞当前线程直到任务完成。
>
> 提交一个 Callable 任务时，需要使用 FutureTask 包一层：
>
> FutureTask futureTask = new FutureTask(new Callable<String>() {    //创建 Callable 任务
>     @Override
>     public String call() throws Exception {
>         String result = "";
>         //do something
>         return result;
>     }
> });
> Future<?> submit = executor.submit(futureTask);    //提交到线程池
> try {
>     Object result = submit.get();    //获取结果
> } catch (InterruptedException e) {
>     e.printStackTrace();
> } catch (ExecutionException e) {
>     e.printStackTrace();
> }
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9
> 10
> 11
> 12
> 13
> 14
> 15
> 16
> 关闭线程池
> 线程池即使不执行任务也会占用一些资源，所以在我们要退出任务时最好关闭线程池。
>
> 有两个方法关闭线程池：
>
> 1.`shutdown()
>
>      public void shutdown() {
>         final ReentrantLock mainLock = this.mainLock;
>         mainLock.lock();
>         try {
>             checkShutdownAccess();  //获取权限
>             advanceRunState(SHUTDOWN);  //修改运行状态
>             interruptIdleWorkers();  //遍历停止未开启的线程
>             onShutdown(); // 目前空实现
>         } finally {
>             mainLock.unlock();
>         }
>         tryTerminate();
>     }
>     private void interruptIdleWorkers(boolean onlyOne) {
>         final ReentrantLock mainLock = this.mainLock;
>         mainLock.lock();
>         try {
>             for (Worker w : workers) {  //遍历所有线程
>                 Thread t = w.thread;
>                 //多了一个条件w.tryLock()，表示拿到锁后就中断
>                 if (!t.isInterrupted() && w.tryLock()) {
>                     try {
>                         t.interrupt();
>                     } catch (SecurityException ignore) {
>                     } finally {
>                         w.unlock();
>                     }
>                 }
>                 if (onlyOne)
>                     break;
>             }
>         } finally {
>             mainLock.unlock();
>         }
>     }
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9
> 10
> 11
> 12
> 13
> 14
> 15
> 16
> 17
> 18
> 19
> 20
> 21
> 22
> 23
> 24
> 25
> 26
> 27
> 28
> 29
> 30
> 31
> 32
> 33
> 34
> 35
> 将线程池的状态设置为 SHUTDOWN，然后中断所有 没有执行 的线程，无法再添加线程。
>
> 2.shutdownNow()
>
>     public List<Runnable> shutdownNow() {
>         List<Runnable> tasks;
>         final ReentrantLock mainLock = this.mainLock;
>         mainLock.lock();
>         try {
>             checkShutdownAccess();
>             advanceRunState(STOP);  //修改运行状态
>             interruptWorkers();     //中断所有
>             tasks = drainQueue();
>         } finally {
>             mainLock.unlock();
>         }
>         tryTerminate();
>         return tasks;
>     }
>     
>     private void interruptWorkers() {
>         final ReentrantLock mainLock = this.mainLock;
>         mainLock.lock();
>         try {
>             for (Worker w : workers)  //中断全部线程，不管是否在执行
>                 w.interruptIfStarted();
>         } finally {
>             mainLock.unlock();
>         }
>     }
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9
> 10
> 11
> 12
> 13
> 14
> 15
> 16
> 17
> 18
> 19
> 20
> 21
> 22
> 23
> 24
> 25
> 26
> 将线程池设置为 STOP，然后尝试停止 所有线程，并返回等待执行任务的列表。
>
> 它们的不同点是：shutdown() 只结束未执行的任务；shutdownNow() 结束全部。
>
> 共同点是：都是通过遍历线程池中的线程，逐个调用 Thread.interrup() 来中断线程，所以一些无法响应中断的任务可能永远无法停止（比如 Runnable）。
>
> 如何合理地选择或者配置
> 了解 JDK 提供的几种线程池实现，在实际开发中如何选择呢？
>
> 根据任务类型决定。
> 前面已经介绍了，这里再小节一下：
>
> CachedThreadPool 用于**并发执行大量短期的小任务**，或者是负载较轻的服务器。
> FixedThreadPool 用于**负载比较重的服务器**，为了资源的合理利用，需要限制当前线程数量。
> SingleThreadExecutor 用于**串行执行任务**的场景，每个任务必须按顺序执行，不需要并发执行。
> ScheduledThreadPoolExecutor 用于需要**多个后台线程执行周期任务**，同时需要限制线程数量的场景。
> 自定义线程池时，如果任务是 CPU 密集型（需要进行大量计算、处理），则应该配置尽量少的线程，比如 CPU 个数 + 1，这样可以避免出现每个线程都需要使用很长时间但是有太多线程争抢资源的情况；
> 如果任务是 IO密集型（主要时间都在 I/O，CPU 空闲时间比较多），则应该配置多一些线程，比如 CPU 数的两倍，这样可以更高地压榨 CPU。
>
> 为了错误避免创建过多线程导致系统奔溃，建议使用有界队列。因为它在无法添加更多任务时会拒绝任务，这样可以提前预警，避免影响整个系统。
>
> 执行时间、顺序有要求的话可以选择优先级队列，同时也要保证低优先级的任务有机会被执行。
>
> 总结
> 这篇文章简单介绍了 Java 中线程池的工作原理和一些常见线程池的使用，在实际开发中最好使用线程池来统一管理异步任务，而不是直接 new 一个线程执行任务。
> ————————————————
> 版权声明：本文为CSDN博主「拭心」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/u011240877/article/details/73440993

4.JVM 的相关知识，OOM 如何定位，说几个虚拟机指令以及虚拟机栈可能会发生什么错误，四种引用类型

> 沈老师，有一个Java服务出现了OOM(Out Of Memory)问题，定位了好久不得其法，请问有什么好的思路么?
>
> OOM的问题，印象中之前写过，这里再总结一些相对通用的方案，希望能帮助到Java技术栈的同学。
>
> 某Java服务(假设PID=10765)出现了OOM，最常见的原因为：
>
> - **有可能是内存分配确实过小，而正常业务使用了大量内存**
> - **某一个对象被频繁申请，却没有释放，内存不断泄漏，导致内存耗尽**
> - **某一个资源被频繁申请，系统资源耗尽，例如：不断创建线程，不断发起网络连接**
>
> 画外音：无非“本身资源不够”“申请资源太多”“资源耗尽”几个原因。
>
> 更具体的，可以使用以下工具逐一排查。
>
> **一、确认是不是内存本身就分配过小**
>
> 方法：
>
> ```
> jmap -heap 10765 
> ```
>
> [![img](https://s2.51cto.com/oss/201911/05/ad3e835f1aba7eace1d1e26fa76176d1.jpg-wh_651x-s_3020016654.jpg)](https://s2.51cto.com/oss/201911/05/ad3e835f1aba7eace1d1e26fa76176d1.jpg-wh_651x-s_3020016654.jpg)
>
> 如上图，可以查看新生代，老生代堆内存的分配大小以及使用情况，看是否本身分配过小。
>
> **二、找到最耗内存的对象**
>
> 方法：
>
> ```
> jmap -histo:live 10765 | more 
> ```
>
> [![img](https://s4.51cto.com/oss/201911/05/11e778f139056ecefaeff0e97d3a600b.jpg)](https://s4.51cto.com/oss/201911/05/11e778f139056ecefaeff0e97d3a600b.jpg)
>
> 如上图，输入命令后，会以表格的形式显示存活对象的信息，并按照所占内存大小排序：
>
> - 实例数
> - 所占内存大小
> - 类名
>
> 是不是很直观?对于实例数较多，占用内存大小较多的实例/类，相关的代码就要针对性review了。
>
> 上图中占内存最多的对象是RingBufferLogEvent，共占用内存18M，属于正常使用范围。
>
> 如果发现某类对象占用内存很大(例如几个G)，很可能是类对象创建太多，且一直未释放。例如：
>
> - 申请完资源后，未调用close()或dispose()释放资源
> - 消费者消费速度慢(或停止消费了)，而生产者不断往队列中投递任务，导致队列中任务累积过多
>
> 画外音：线上执行该命令会强制执行一次fgc。另外还可以dump内存进行分析。
>
> **三、确认是否是资源耗尽**
>
> 工具：
>
> - pstree
> - netstat
>
> 查看进程创建的线程数，以及网络连接数，如果资源耗尽，也可能出现OOM。 这里介绍另一种方法，通过
>
> - /proc/${PID}/fd
> - /proc/${PID}/task
>
> 可以分别查看句柄详情和线程数。 例如，某一台线上服务器的sshd进程PID是9339，查看
>
> - ll /proc/9339/fd
> - ll /proc/9339/task
>
> [![img](https://s1.51cto.com/oss/201911/05/3132728842d11bc9ac6912144db65768.jpg)](https://s1.51cto.com/oss/201911/05/3132728842d11bc9ac6912144db65768.jpg)
>
> 如上图，sshd共占用了四个句柄：
>
> - 0 -> 标准输入
> - 1 -> 标准输出
> - 2 -> 标准错误输出
> - 3 -> socket(容易想到是监听端口)
>
> sshd只有一个主线程PID为9339，并没有多线程。
>
> 所以，只要
>
> - ll /proc/${PID}/fd | wc -l
> - ll /proc/${PID}/task | wc -l (效果等同pstree -p | wc -l)
>
> 就能知道进程打开的句柄数和线程数。
>
> # 虚拟机规范中的 StackOverflowError 与 OutOfMemoryError
> 参考 Java 虚拟机规范官方文档：Run-Time Data Areas，可以知道，在如下情况下，会抛出这两种错误：
>
> 当某次线程运行计算时，需要占用的 Java 虚拟机栈（Java Virtual Machine Stack）大小，也就是 Java 线程栈大小，超过规定大小时，抛出 StackOverflowError
> 如果 Java 虚拟机栈大小可以动态扩容，发生扩容时发现内存不足，或者新建Java 虚拟机栈时发现内存不足，抛出 OutOfMemoryError
> 当所需要的堆（heap）内存大小不足时，抛出 OutOfMemoryError
> 当方法区（Method Area）大小不够分配时，抛出 OutOfMemoryError
> 当创建一个类或者接口时，运行时常量区剩余大小不够时，抛出 OutOfMemoryError
> 本地方法栈（Native Method Stack）大小不足时，抛出 StackOverflowError
> 本地方法栈（Native Method Stack）扩容时发现内存不足，或者新建本地方法栈发现内存不足，抛出 OutOfMemoryError
> ————————————————
> 版权声明：本文为CSDN博主「干货满满张哈希」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/zhxdick/article/details/109025250

5.Java 并发，synchronized 性能为什么提高了（锁升级过程），与 Java 的 lock 有什么区别以及使用场景

6.网络，输入 [www.baidu.com](http://www.baidu.com) 都会发生什么

7.http 报文结构，头部都有哪些字段

> # 请求头（Request）：
> Accept：text/html application/xml 告诉服务器客户端浏览器这边可以出里什么数据；
> Accept-Encodeing：gzip 告诉服务器我能支持什么样的压缩格式
> accept-language：告诉服务器浏览器支持的语言
> Cache-control：告诉服务器是否缓存
> Connection:keep-alive 告诉服务器当前保持活跃（与服务器处于链接状态）
> Host：远程服务器的域名
> User-agent：客户端的一些信息，浏览器信息 版本
> referer：当前页面上一个页面地址。一般用于服务器判断是否为同一个域名下的请求
>
> # 返回头（response-header）：
> cache-control:private/no-cache; 私有的不需要缓存/no-cache也不需要缓存
> connection:keep-live; 服务器同意保持连接
> content-Enconding：gzip；除去头的剩余部分压缩返回格式
> content-length:内容长度
> content-type：text/css;返回内容支持格式
> Date： 时间
> server：ngnix 服务器类型
> set-Cookie:服务器向客户端设置cookie 第一次访问服务器会下发cookie当作身份认证信息，第二次访问服务器再把cookie送给服务器，可以当作认证信息
> last-modified: 时间戳 文档的最后改动时间。客户可以通过If-Modified-Since请求头提供一个日期，该请求将被视为一个条件GET，只有改动时间迟于指定时间的文档才会返回，否则返回一个304(Not Modified)状态。Last-Modified也可用setDateHeader方法来设置。
> expires 告诉浏览器把回送的资源缓存多长时间 -1或0则是不缓存
> etag:版本专有的加密指纹。（有的网站不用，并非必须）优先检查etag再检查last-modified的时间戳。向服务器请求带if-none-match,服务器判断是否过期未过期返回304，过期返回200
> // 注释：第一次请求出来的数据先进行缓存协商，是否缓存expires，cache-control 缓存时间，etag，last-modified等
> //注释：多次访问的时候，浏览器先判断是否有缓存，是否过期
> //未过期：直接从缓存中读取。
>
> //http状态码
> //1.指示信息–表示请求已经接受，继续处理
> //2.成功–表示请求已经被成功接受，理解
> //3.重定向–表示完成请求必须进行更进一步操作
> //4.客户端错误–请求有语法错误或者请求无法实现
> //5.服务器端错误–服务器未能实现合法的请求
> ————————————————
> 版权声明：本文为CSDN博主「优秀前端人」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/xiaoxia188/article/details/96116025

8.进程与线程，了解协程吗（大概说了下）

> ## 协程和线程的比较
>
> | 比较项   |                             线程                             |                             协程                             |
> | -------- | :----------------------------------------------------------: | :----------------------------------------------------------: |
> | 占用资源 |                   初始单位为1MB,固定不可变                   |                初始一般为 2KB，可随需要而增大                |
> | 调度所属 |                       由 OS 的内核完成                       |                          由用户完成                          |
> | 切换开销 | 涉及模式切换(从用户态切换到内核态)、16个寄存器、PC、SP...等寄存器的刷新等 |            只有三个寄存器的值修改 - PC / SP / DX.            |
> | 性能问题 |        资源占用太高，频繁创建销毁会带来严重的性能问题        |              资源占用小,不会带来严重的性能问题               |
> | 数据同步 |            需要用锁等机制确保数据的一直性和可见性            | 不需要多线程的锁机制，因为只有一个线程，也不存在同时写变量冲突，在协程中控制共享资源不加锁，只需要判断状态就好了，所以执行效率比多线程高很多。 |

9.死锁了解吗，说一下条件，如何解决

10.让写一下[链表]()实现插入方法（顺序不在这里，忘了在哪了，突然想起来了），查询效率呢，怎么优化

11.写个[算法]()，给一个表达式的字符串（+-*/），算出字符串的结果，没考虑括号说了下括号的思路

12.问问题

面试小哥很厉害，比较有耐心。没回答上来的都给耐心讲解，引导着问问题，由浅入深体验极佳😀

面完让稍等一下，十分钟后二面

## 5.15 二面（45分钟）

1.面试官看着就很厉害，在家办公感觉很忙，上来先问实习的时间以及时长，说最好半年

2.没有自我介绍直接开始，先是网络，TCP 三次握手四次挥手，time_wait 和 close_wait 具体干什么，为什么要三次两次不行吗，有大量连接处于 time_wait 的原因，TCP 是长连接还是短连接

> ![img](https:////upload-images.jianshu.io/upload_images/2526788-a2ae7171d8d1c30c.png?imageMogr2/auto-orient/strip|imageView2/2/w/732/format/webp)
>
> tcp_01.png
>
> 在日常的服务器维护中，会经常用到如下命令。
>  `netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'`
>
> 它会显示例如下面的信息：
>  TIME_WAIT 689
>  CLOSE_WAIT 2
>  FIN_WAIT1 1
>  ESTABLISHED 291
>  SYN_RECV 2
>  LAST_ACK 1
>
> 常用的三个状态是：ESTABLISHED表示正在通信 、TIME_WAIT表示主动关闭、CLOSE_WAIT表示被动关闭。
>
> 如果服务器出现了异常，很大的可能是出现了以下两种情况：
>
> - 服务器保持了大量的TIME_WAIT状态。
> - 服务器保持了大量的CLOSE_WAIT状态。
>
> 我们也都知道Linux系统中分给每个用户的文件句柄数是有限的，而TIME_WAIT和CLOSE_WAIT这两种状态如果一直被保持，那么意味着对应数目的通道(此处应理解为socket，一般一个socket会占用服务器端一个端口，服务器端的端口最大数是65535)一直被占用，一旦达到了上限，则新的请求就无法被处理，接着就是大量Too Many Open Files异常，然后tomcat、nginx、apache崩溃。。。
>
> 下面来讨论这两种状态的处理方法，网络上也有很多资料把这两种情况混为一谈，认为优化内核参数就可以解决，其实这是不恰当的。优化内核参数在一定程度上能解决time_wait过多的问题，但是应对close_wait还得从应用程序本身出发。
>
> ##### 1) 服务器保持了大量的time_wait状态
>
> 这种情况比较常见，一般会出现在爬虫服务器和web服务器(如果没做内核参数优化的话)上，那么这种问题是怎么产生的呢？
>
> 从上图可以看出time_wait是主动关闭连接的一方保持的状态，对于爬虫服务器来说它自身就是客户端，在完成一个爬取任务后就会发起主动关闭连接，从而进入time_wait状态，然后保持这个状态2MSL时间之后，彻底关闭回收资源。这里为什么会保持资源2MSL时间呢？这也是TCP/IP设计者规定的。
>
> **TCP要保证在所有可能的情况下使得所有的数据都能够被正确送达。当你关闭一个socket时，主动关闭一端的socket将进入TIME_WAIT状 态，而被动关闭一方则转入CLOSED状态，这的确能够保证所有的数据都被传输。当一个socket关闭的时候，是通过两端四次握手完成的，当一端调用 close()时，就说明本端没有数据要发送了。这好似看来在握手完成以后，socket就都可以处于初始的CLOSED状态了，其实不然。原因是这样安 排状态有两个问题， 首先，我们没有任何机制保证最后的一个ACK能够正常传输，第二，网络上仍然有可能有残余的数据包(wandering duplicates)，我们也必须能够正常处理。**
>
> TIMEWAIT就是为了解决这两个问题而生的。
>
> - 假设最后的一个ACK丢失，那么被动关闭一方收不到这最后一个ACK则会重发FIN。此时主动关闭一方必须保持一个有效的(time_wait状态下维持)状态信息，以便可以重发ACK。如果主动关闭的socket不维持这种状态而是进入close状态，那么主动关闭的一方在收到被动关闭方重新发送的FIN时则响应给被动方一个RST。被动方收到这个RST后会认为此次回话出错了。所以如果TCP想要完成必要的操作而终止双方的数据流传输，就必须完全正确的传输四次握手的四步，不能有任何的丢失。这就是为什么在socket在关闭后，任然处于time_wait状态的第一个原因。因为他要等待可能出现的错误(被动关闭端没有接收到最后一个ACK)，以便重发ACK。
> - 假设目前连接的通信双方都调用了close(),双方同时进入closed的终结状态，而没有走time_wait状态。则会出现如下问题：
>    假如现在有一个新的连接建立起来，使用的IP地址与之前的端口完全相同，现在建立的一个连接是之前连接的完全复用，我们还假定之前连接中有数据报残存在网络之中，这样的话现在的连接收到的数据有可能是之前连接的报文。为了防止这一点。TCP不允许新的连接复用time_wait状态下的socket。处于time_wait状态的socket在等待2MSL时间后（之所以是两倍的MSL，是由于MSL是一个数据报在网络中单向发出 到认定丢失的时间，即(Maximum Segment Lifetime)报文最长存活时间，一个数据报有可能在发送途中或是其响应过程中成为残余数据报，确认一个数据报及其响应的丢弃需要两倍的MSL），将会转为closed状态。这就意味着，一个成功建立的连接，必须使得之前网络中残余的数据报都丢失了。
>
> 再引用网络中的一段话：
>
> 1. 值得一说的是，基于TCP的http协议，一般(此处为什么说一般呢，因为当你在**keepalive时间内** 主动关闭对服务器端的连接时，那么主动关闭端就是客户端,否则客户端就是被动关闭端。下面的爬虫例子就是这种情况)主动关闭tcp一端的是server端，这样server端就会进入time_wait状态，可想而知，对于访问量大的web服务器，会存在大量的time_wait状态，假如server一秒钟接收1000个请求，那么就会积压240*1000=240000个time_wait状态。(RFC 793中规定MSL为2分钟，实际应用中常用的是30秒，1分钟和2分钟等。)，维持这些状态给服务器端带来巨大的负担。
>     **当然现代操作系统都会用快速的查找算法来管理这些 TIME_WAIT，所以对于新的 TCP连接请求，判断是否hit中一个TIME_WAIT不会太费时间，但是有这么多状态要维护总是不好。**
> 2. HTTP协议1.1版本规定default行为是keep-Alive，也就是会重用tcp连接传输多个request/response。之所以这么做的主要原因是发现了我们上面说的这个问题。
>
> ##### 2) 服务器保持了大量的close_wait状态
>
> time_wait问题可以通过调整内核参数和适当的设置web服务器的keep-Alive值来解决。因为time_wait是自己可控的，要么就是对方连接的异常，要么就是自己没有快速的回收资源，总之不是由于自己程序错误引起的。但是close_wait就不一样了，从上图中我们可以看到服务器保持大量的close_wait只有一种情况，那就是对方发送一个FIN后，程序自己这边没有进一步发送ACK以确认。换句话说就是在对方关闭连接后，程序里没有检测到，或者程序里本身就已经忘了这个时候需要关闭连接，于是这个资源就一直被程序占用着。这个时候快速的解决方法是：
>
> - 关闭正在运行的程序，这个需要视业务情况而定。
> - 尽快的修改程序里的bug，然后测试提交到线上服务器。
>
> 注：
>  直到写这篇文章的时候我才完全弄明白之前工作中遇到的一个问题。程序员写了爬虫(php)运行在采集服务器A上，程序去B服务器上采集资源，但是A服务器很快就发现出现了大量的close_wait状态的连接。后来手动检查才发现这些处于close_wait状态的请求结果都是404，那就说明B服务器上没有要请求的资源。
>
> 下面引用网友分析的结论：
>  服 务器A是一台爬虫服务器，它使用简单的HttpClient去请求资源服务器B上面的apache获取文件资源，正常情况下，如果请求成功，那么在抓取完 资源后，服务器A会主动发出关闭连接的请求，这个时候就是主动关闭连接，服务器A的连接状态我们可以看到是TIME_WAIT。如果一旦发生异常呢？假设 请求的资源服务器B上并不存在，那么这个时候就会由服务器B发出关闭连接的请求，服务器A就是被动的关闭了连接，如果服务器A被动关闭连接之后程序员忘了 让HttpClient释放连接，那就会造成CLOSE_WAIT的状态了。
>
> 
>
> 作者：wangfs
> 链接：https://www.jianshu.com/p/394cafc91d18
> 来源：简书
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
>
> 常用的三个状态是：ESTABLISHED 表示正在通信，TIME_WAIT 表示主动关闭，CLOSE_WAIT 表示被动关闭。
>
> TCP协议规定，对于已经建立的连接，网络双方要进行四次握手才能成功断开连接，如果缺少了其中某个步骤，将会使连接处于假死状态，连接本身占用的资源不会被释放。网络服务器程序要同时管理大量连接，所以很有必要保证无用连接完全断开，否则大量僵死的连接会浪费许多服务器资源。在众多TCP状态中，最值得注意的状态有两个：CLOSE_WAIT和TIME_WAIT。  
>
> **TIME_WAIT** 
>
> TIME_WAIT 是主动关闭链接时形成的，等待2MSL时间，约4分钟。主要是防止最后一个ACK丢失。  由于TIME_WAIT 的时间会非常长，因此server端应尽量减少主动关闭连接
>
> **CLOSE_WAIT**
> CLOSE_WAIT是被动关闭连接是形成的。根据TCP状态机，服务器端收到客户端发送的FIN，则按照TCP实现发送ACK，因此进入CLOSE_WAIT状态。但如果服务器端不执行close()，就不能由CLOSE_WAIT迁移到LAST_ACK，则系统中会存在很多CLOSE_WAIT状态的连接。此时，可能是系统忙于处理读、写操作，而未将已收到FIN的连接，进行close。此时，recv/read已收到FIN的连接socket，会返回0。
>
> **为什么需要 TIME_WAIT 状态？**
> 假设最终的ACK丢失，server将重发FIN，client必须维护TCP状态信息以便可以重发最终的ACK，否则会发送RST，结果server认为发生错误。TCP实现必须可靠地终止连接的两个方向(全双工关闭)，client必须进入 TIME_WAIT 状态，因为client可能面 临重发最终ACK的情形。
>
> **为什么 TIME_WAIT 状态需要保持 2MSL 这么长的时间？**
> 如果 TIME_WAIT 状态保持时间不足够长(比如小于2MSL)，第一个连接就正常终止了。第二个拥有相同相关五元组的连接出现，而第一个连接的重复报文到达，干扰了第二个连接。TCP实现必须防止某个连接的重复报文在连接终止后出现，所以让TIME_WAIT状态保持时间足够长(2MSL)，连接相应方向上的TCP报文要么完全响应完毕，要么被 丢弃。建立第二个连接的时候，不会混淆。
>
>  **TIME_WAIT 和\**CLOSE_WAIT\**状态socket过多**
>
> 如果服务器出了异常，百分之八九十都是下面两种情况：
>
> 1.服务器保持了大量TIME_WAIT状态
>
> 2.服务器保持了大量CLOSE_WAIT状态，简单来说CLOSE_WAIT数目过大是由于被动关闭连接处理不当导致的。
>
> 因为linux分配给一个用户的文件句柄是有限的，而TIME_WAIT和CLOSE_WAIT两种状态如果一直被保持，那么意味着对应数目的通道就一直被占着，而且是“占着茅坑不使劲”，一旦达到句柄数上限，新的请求就无法被处理了，接着就是大量Too Many Open Files异常，Tomcat崩溃。

3.Https 了解吗，说一下整个过程（对称加密，非对称加密），与 http 的不同点

4.进程线程又问了，进程间通信方式（剩下的想不起来）

5.数据库部分知识，手写一个 SQL （子查询 感觉主要看 group by 和 having）

> # 内，左及右连接
>
> 关于inner join 与 left join 之间的区别，以前以为自己搞懂了，今天从前端取参数的时候发现不是预想中的结果，才知道问题出在inner join 上了。
>
> 
>
> 需求是从数据库查数据，在前端以柱形图的形式展现出来，查到的数据按行业分组，显示每个行业的户数及户数占比，涉及到的字段有A表的用户数、总用户数和B表的行业名称。本来是不管查不查的到数据，在X轴都应该显示行业名称的，结果是X、Y轴都没有任何数据显示。问题就是我用错了联结方式。
>
> 一、sql的left join 、right join 、inner join之间的区别
>
> 　　left join(左联接) 返回包括左表中的所有记录和右表中联结字段相等的记录 
> 　　right join(右联接) 返回包括右表中的所有记录和左表中联结字段相等的记录
> 　　inner join(等值连接) 只返回两个表中联结字段相等的行
>
> 举例如下： 
> \--------------------------------------------
> 表A记录如下：
> aID　　　　　aNum
> 1　　　　　a20050111
> 2　　　　　a20050112
> 3　　　　　a20050113
> 4　　　　　a20050114
> 5　　　　　a20050115
>
> 表B记录如下:
> bID　　　　　bName
> 1　　　　　2006032401
> 2　　　　　2006032402
> 3　　　　　2006032403
> 4　　　　　2006032404
> 8　　　　　2006032408
>
> \--------------------------------------------
> 1.left join
> sql语句如下: 
> select * from A
> left join B 
> on A.aID = B.bID
>
> 结果如下:
> aID　　　　　aNum　　　　　bID　　　　　bName
> 1　　　　　a20050111　　　　1　　　　　2006032401
> 2　　　　　a20050112　　　　2　　　　　2006032402
> 3　　　　　a20050113　　　　3　　　　　2006032403
> 4　　　　　a20050114　　　　4　　　　　2006032404
> 5　　　　　a20050115　　　　NULL　　　　　NULL
>
> （所影响的行数为 5 行）
> 结果说明:
> left join是以A表的记录为基础的,A可以看成左表,B可以看成右表,left join是以左表为准的.
> 换句话说,左表(A)的记录将会全部表示出来,而右表(B)只会显示符合搜索条件的记录(例子中为: A.aID = B.bID).
> B表记录不足的地方均为NULL.
> \--------------------------------------------
> 2.right join
> sql语句如下: 
> select * from A
> right join B 
> on A.aID = B.bID
>
> 结果如下:
> aID　　　　　aNum　　　　　bID　　　　　bName
> 1　　　　　a20050111　　　　1　　　　　2006032401
> 2　　　　　a20050112　　　　2　　　　　2006032402
> 3　　　　　a20050113　　　　3　　　　　2006032403
> 4　　　　　a20050114　　　　4　　　　　2006032404
> NULL　　　　　NULL　　　　　8　　　　　2006032408
>
> （所影响的行数为 5 行）
> 结果说明:
> 仔细观察一下,就会发现,和left join的结果刚好相反,这次是以右表(B)为基础的,A表不足的地方用NULL填充.
> \--------------------------------------------
> 3.inner join
> sql语句如下: 
> select * from A
> innerjoin B 
> on A.aID = B.bID
>
> 结果如下:
> aID　　　　　aNum　　　　　bID　　　　　bName
> 1　　　　　a20050111　　　　1　　　　　2006032401
> 2　　　　　a20050112　　　　2　　　　　2006032402
> 3　　　　　a20050113　　　　3　　　　　2006032403
> 4　　　　　a20050114　　　　4　　　　　2006032404
>
> 结果说明:
> 很明显,这里只显示出了 A.aID = B.bID的记录.这说明inner join并不以谁为基础,它只显示符合条件的记录.
> \--------------------------------------------
>
> 
>
> w3school的一套sql教程:
>
> http://www.w3school.com.cn/sql/index.asp 
>
> left join  :左连接，返回左表中所有的记录以及右表中连接字段相等的记录。
>
> right join :右连接，返回右表中所有的记录以及左表中连接字段相等的记录。
> inner join :内连接，又叫等值连接，只返回两个表中连接字段相等的行。
> full join :外连接，返回两个表中的行：left join + right join
> cross join :结果是笛卡尔积，就是第一个表的行数乘以第二个表的行数。
>
> # having 子查询
>
> having的用法
> having字句可以让我们筛选成组后的各种数据，where字句在聚合前先筛选记录，也就是说作用在group by和having字句前。而 having子句在聚合后对组记录进行筛选。
>
> SQL实例：
>
> 一、显示每个地区的总人口数和总面积．
> SELECT region, SUM(population), SUM(area) FROM bbc GROUP BY region
>
> 先以region把返回记录分成多个组，这就是GROUP BY的字面含义。分完组后，然后用聚合函数对每组中
> 的不同字段（一或多条记录）作运算。
>
> 二、 显示每个地区的总人口数和总面积．仅显示那些面积超过1000000的地区。
>
> SELECT region, SUM(population), SUM(area)
> FROM bbc
> GROUP BY region
> HAVING SUM(area)>1000000
>
> 在这里，我们不能用where来筛选超过1000000的地区，因为表中不存在这样一条记录。
> 相反，having子句可以让我们筛选成组后的各组数据
> ————————————————
> 版权声明：本文为CSDN博主「月亮弯弯2013」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/love_xsq/article/details/42417917
>
> **题目2: 用一条SQL语句查出订单表(order表)购买的每类产品付款都大于60元的客户姓名**
>
> ![img](http://bluesd7.com/Portals/0/QQ20190510115456.png)
>
> **分析：**
>
> \1. 先根据客户分组,得出每个客户购买的产品数量
>
> \2. 然后查出每个客户购买的大于60元的产品数量
>
> \3. 然后拿两个结果比较，如果"客户购买的所有产品数量"等于"大于60元的产品数量"，就代表这个客户购买的所有产品都超过了60元
>
> ```java
> select a.name  from order2 a
> group by a.name
> having count(1)=(select count(1) from order2 b where b.payment>60 and a.name=b.name)
> ```
>
> **输出结果：**
>
> ![img](http://bluesd7.com/Portals/0/QQ20190510115511.png)
>
> # Count(1) Count(*) Count(列名)
>
> 执行效果：
>
>
> 1.  count(1) and count(*)
>
> 当表的数据量大些时，对表作分析之后，使用count(1)还要比使用count(*)用时多了！ 
> 从执行计划来看，count(1)和count(*)的效果是一样的。 但是在表做过分析之后，count(1)会比count(*)的用时少些（1w以内数据量），不过差不了多少。 
>
> 如果count(1)是聚索引,id,那肯定是count(1)快。但是差的很小的。 
> 因为count(*),自动会优化指定到那一个字段。所以没必要去count(1)，用count(*)，sql会帮你完成优化的 因此： count(1)和count(*)基本没有差别！ 
>
> 2. count(1) and count(字段)
> 两者的主要区别是
> （1） count(1) 会统计表中的所有的记录数， 包含字段为null 的记录。
> （2） count(字段) 会统计该字段在表中出现的次数，忽略字段为null 的情况。即 不统计字段为null 的记录。  
> 转自：http://www.cnblogs.com/Dhouse/p/6734837.html
>
>
> count(*) 和 count(1)和count(列名)区别  
>
> 执行效果上 ：  
> count(*)包括了所有的列，相当于行数，在统计结果的时候， 不会忽略列值为NULL  
> count(1)包括了忽略所有列，用1代表代码行，在统计结果的时候， 不会忽略列值为NULL  
> count(列名)只包括列名那一列，在统计结果的时候，会忽略列值为空（这里的空不是只空字符串或者0，而是表示null）的计数， 即某个字段值为NULL时，不统计。
>
> 执行效率上：  
> 列名为主键，count(列名)会比count(1)快  
> 列名不为主键，count(1)会比count(列名)快  
> 如果表多个列并且没有主键，则 count（1） 的执行效率优于 count（*）  
> 如果有主键，则 select count（主键）的执行效率是最优的  
> 如果表只有一个字段，则 select count（*）最优。
> 转自：http://eeeewwwqq.iteye.com/blog/1972576
>
>
> 实例分析
>
> mysql> create table counttest(name char(1), age char(2));
> Query OK, 0 rows affected (0.03 sec)
>
> mysql> insert into counttest values
>     -> ('a', '14'),('a', '15'), ('a', '15'), 
>     -> ('b', NULL), ('b', '16'), 
>     -> ('c', '17'),
>     -> ('d', null), 
>     ->('e', '');
> Query OK, 8 rows affected (0.01 sec)
> Records: 8  Duplicates: 0  Warnings: 0
>
> mysql> select * from counttest;
> +------+------+
> | name | age  |
> +------+------+
> | a    | 14   |
> | a    | 15   |
> | a    | 15   |
> | b    | NULL |
> | b    | 16   |
> | c    | 17   |
> | d    | NULL |
> | e    |      |
> +------+------+
> 8 rows in set (0.00 sec)
>
> mysql> select name, count(name), count(1), count(*), count(age), count(distinct(age))
>     -> from counttest
>     -> group by name;
> +------+-------------+----------+----------+------------+----------------------+
> | name | count(name) | count(1) | count(*) | count(age) | count(distinct(age)) |
> +------+-------------+----------+----------+------------+----------------------+
> | a    |           3 |        3 |        3 |          3 |                    2 |
> | b    |           2 |        2 |        2 |          1 |                    1 |
> | c    |           1 |        1 |        1 |          1 |                    1 |
> | d    |           1 |        1 |        1 |          0 |                    0 |
> | e    |           1 |        1 |        1 |          1 |                    1 |
> +------+-------------+----------+----------+------------+----------------------+
> 5 rows in set (0.00 sec)
>
> 额外参考资料：http://blog.csdn.net/lihuarongaini/article/details/68485838
> ————————————————
> 版权声明：本文为CSDN博主「BigoSprite」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/iFuMI/article/details/77920767

6**.[算法题]()，最长公共连续子串**

一二面顺序可能也是混乱的，记不清楚了，二面面试官感觉好忙啊，写题的时候，他就在忙着敲什么，感觉好不容易啊，一边得面试还在工作😂，体验较好，部分问题也引导着问
第二天写的[面经]()所以一二面问题可能是混乱的！

## 一面（40分钟）

- 一面问的比较基础

- 项目

- Java Object类有哪些方法,分别作用

  > Object类，属于java.lang包，位于类层次结构树的顶部。每个类都是Object类的直接或间接的后代。使用或编写的每个类都继承Object的实例方法。Object类总共13个方法，直接一点上图(使用的是JDK1.7的源码)：
  >
  > ![img](https://img-blog.csdn.net/20180821100500542?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMwMjY0Njg5/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
  >
  > 1．clone方法
  >
  > 保护方法，实现对象的浅复制，只有实现了Cloneable接口才可以调用该方法，否则抛出CloneNotSupportedException异常。
  >
  > 主要是JAVA里除了8种基本类型传参数是值传递，其他的类对象传参数都是引用传递，我们有时候不希望在方法里将参数改变，这是就需要在类中复写clone方法。
  >
  > 2．getClass方法
  >
  > final方法，获得运行时类型。
  >
  > 3．toString方法
  >
  > 该方法用得比较多，一般子类都有覆盖。
  >
  > 4．finalize方法
  >
  > 该方法用于释放资源。因为无法确定该方法什么时候被调用，很少使用。
  >
  > 5．equals方法
  >
  > 该方法是非常重要的一个方法。一般equals和==是不一样的，但是在Object中两者是一样的。子类一般都要重写这个方法。
  >
  > 6．hashCode方法
  >
  > 该方法用于哈希查找，可以减少在查找中使用equals的次数，重写了equals方法一般都要重写hashCode方法。这个方法在一些具有哈希功能的Collection中用到。
  >
  > 一般必须满足obj1.equals(obj2)==true。可以推出obj1.hash- Code()==obj2.hashCode()，但是hashCode相等不一定就满足equals。不过为了提高效率，应该尽量使上面两个条件接近等价。
  >
  > **如果不重写hashcode(),在HashSet中添加两个equals的对象，会将两个对象都加入进去。**
  >
  > 7．wait方法
  >
  > wait方法就是使当前线程等待该对象的锁，当前线程必须是该对象的拥有者，也就是具有该对象的锁。wait()方法一直等待，直到获得锁或者被中断。wait(long timeout)设定一个超时间隔，如果在规定时间内没有获得锁就返回。
  >
  > 调用该方法后当前线程进入睡眠状态，直到以下事件发生。
  >
  > （1）其他线程调用了该对象的notify方法。
  >
  > （2）其他线程调用了该对象的notifyAll方法。
  >
  > （3）其他线程调用了interrupt中断该线程。
  >
  > （4）时间间隔到了。
  >
  > 此时该线程就可以被调度了，如果是被中断的话就抛出一个InterruptedException异常。
  >
  > 8．notify方法
  >
  > 该方法唤醒在该对象上等待的某个线程。
  >
  > 9．notifyAll方法
  >
  > 该方法唤醒在该对象上等待的所有线程。
  >
  > Object的notify、notifyAll和wait方法都在同步程序中独立运行线程的活动中起着作用
  >
  > \10.  finalize 方法
  >
  > Java允许在类中定义一个名为finalize()的方法。它的工作原理是：一旦垃圾回收器准备好释放对象占用的存储空间，将首先调用其finalize()方法。并且在下一次垃圾回收动作发生时，才会真正回收对象占用的内存。
  >
  > 关于垃圾回收，有三点需要记住：
  >
  > 　　1、**对象可能不被垃圾回收。只要程序没有濒临存储空间用完的那一刻，对象占用的空间就总也得不到释放。**
  >
  > 　　2、垃圾回收并不等于“析构”。
  >
  > 　　3、垃圾回收只与内存有关。使用垃圾回收的唯一原因是为了回收程序不再使用的内存。
  >
  > finalize()的用途：
  >
  > 　　无论对象是如何创建的，垃圾回收器都会负责释放对象占据的所有内存。这就将对finalize()的需求限制到一种特殊情况，即通过某种创建对象方式以外的方式为对象分配了存储空间。不过这种情况一般发生在使用“本地方法”的情况下，本地方法是一种在Java中调用非Java代码的方式。
  >
  > 为什么不能显示直接调用finalize方法？
  >
  >
  >  如前文所述，finalize方法在垃圾回收时一定会被执行，而如果在此之前显示执行的话，也就是说finalize会被执行两次以上，而在第一次资源已经被释放，那么在第二次释放资源时系统一定会报错，因此一般finalize方法的访问权限和父类保持一致，为protected。
  >
  >  1、为什么要重写equals()方法?
  >     因为Object类中定义的equals()方法是用来比较两个引用所指向的对象的内存地址是否一致。
  >     源码:
  >     public boolean equals(Object obj) {
  >         return (this == obj);
  >     }
  >      ("=="两边是基本类型比较的是内容，"=="两边是引用类型(class)比较的是否对同一对象的引用,即比较两个引用所指向的对象的内存地址是否一致。)
  >
  >
  >     而我们经常会希望两个不同对象的某些属性值相同时,就认为它们相同。所以我们要重新equals()方法。
  >    2、为什么要重写hashCode()方法?
  >    需要注意的是:
  >     a、在java应用程序运行时,无论何时多次调用同一个对象时的hashCode()方法,这个对象的hashCode()方法的返回值必须是相同的一个int值
  >     b、如果两个对象equals()返回值为true,则它们的hashCode()也必须返回相同的int值
  >     c、如果两个对象equals()返回值为false,则它们的hashCode()返回值也必须不同
  >     由于hashCode方法定义在Object类中,因此每个对象都有一个默认的散列码,其值为对象的存储地址。如果重新定义equals方法,就必须重新定义hashCode方法,以便用户可以将对象插入到散列表中。(要保证上面所述b,c原则)
  >
  > ————————————————
  > 版权声明：本文为CSDN博主「Hakuna-Matata」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
  > 原文链接：https://blog.csdn.net/WeDoi/article/details/75452092

- HashMap原理,线程安全?

- Java如何进行线程同步

- CAS

- JVM垃圾回收

- Mysq|索引原理

- 如何优化索引查询

- TCP ,拥塞控制

- **算法:求树的最左下节点(我说层次遍历,他说可以)**

- 智力:用正反面概率不相等的硬币,凑出50%

# 二面（60分钟）

- 项目
- 二面感觉主要考察的就是代码能力, 基本-直在码
- 知道什么设计模式,分别介绍
- 手写单例->线程安全的->还可以怎么写
- **算法:求无序数组中第k大的数( quick select )**
- **算法:求旋转数组找最小值(二分)**
- **算法:判断二叉树是否镜像(递归)**

# 三面（40分钟）

- 三面感觉问的问题都比较开放

- 你如何理解后端开发

- 有哪些后端开发经验,做了什么

- 介绍HashMap ,与TreeMap区别

  > HashMap通过hashcode对其内容进行快速查找，而 TreeMap中所有的元素都保持着某种固定的顺序，如果你需要得到一个有序的结果你就应该使用TreeMap（HashMap中元素的排列顺序是不固定的）。

- 用HashMap实现一个有过期功能的缓存,怎么实现

- 如果需要多个线程,那怎么保证线程安全

- 如果把数据都放进Map ,会占用多大内存

- 平时怎么学习新知识

- 最近看了什么书


作者：Java高级架构师
链接：https://juejin.cn/post/6844904062794596366
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## 头条后端 1面：
java gc
java class的加载过程
java hashmap、 为什么用红黑树、红黑树邻接点为啥是8 。

> # 为什么用红黑树
>
> 在Jdk1.8版本后，Java对HashMap做了改进，在链表长度大于8的时候，将后面的数据存在红黑树中，以加快检索速度。
>
> 那么很多人就有疑问为什么是使用红黑树而不是AVL树，AVL树是完全平衡二叉树阿？
>
> 最主要的一点是：
>
> 在CurrentHashMap中是加锁了的，实际上是读写锁，如果写冲突就会等待，
> 如果插入时间过长必然等待时间更长，而红黑树相对AVL树他的插入更快！
>
>
> 第一个问题为什么不一直使用树？
>
> 参考《为什么HashMap包含LinkedList而不是AVL树？》
>
> 我想这是内存占用与存储桶内查找复杂性之间的权衡。请记住，大多数哈希函数将产生非常少的冲突，因此为大小为3或4的桶维护树将是非常昂贵的，没有充分的理由。
>
> 作为参考，这是一个HashMap的Java 8 impl（它实际上有一个很好的解释，整个事情如何工作，以及为什么他们选择8和6，作为“TREEIFY”和“UNTREEIFY”阈值）
>
>  
>
> 第二个问题为什么hash冲突使用红黑树而不是AVL树呢
>
> 参考：AVL树和红黑树之间有什么区别？
>
> ![img](https://img-blog.csdnimg.cn/20190401020750843.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9saW51eHN0eWxlLmJsb2cuY3Nkbi5uZXQ=,size_16,color_FFFFFF,t_70)
>
> ![img](https://img-blog.csdnimg.cn/20190401020804925.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9saW51eHN0eWxlLmJsb2cuY3Nkbi5uZXQ=,size_16,color_FFFFFF,t_70)
>
> 红黑树和AVL树之间的区别
> AVL树比红黑树保持更加严格的平衡。AVL树中从根到最深叶的路径最多为~1.44 lg（n + 2），而在红黑树中最多为~2 lg（n + 1）。
>
> 因此，在AVL树中查找通常更快，但这是以更多旋转操作导致更慢的插入和删除为代价的。因此，如果您希望查找次数主导树的更新次数，请使用AVL树。
>
> AVL以及RedBlack树是高度平衡的树数据结构。它们非常相似，真正的区别在于在任何添加/删除操作时完成的旋转操作次数。
>
> 两种实现都缩放为a O(lg N)，其中N是叶子的数量，但实际上AVL树在查找密集型任务上更快：利用更好的平衡，树遍历平均更短。另一方面，插入和删除方面，AVL树速度较慢：需要更高的旋转次数才能在修改时正确地重新平衡数据结构。
>
> 对于通用实现（即先验并不清楚查找是否是操作的主要部分），RedBlack树是首选：它们更容易实现，并且在常见情况下更快 - 无论数据结构如何经常被搜索修改。一个例子，TreeMap而TreeSet在Java中使用一个支持RedBlack树。
>
> 对于小数据：
>
> insert：RB tree＆avl tree具有恒定的最大旋转次数，但RB树会更快，因为平均RB树使用较少的旋转。
>
> 查找：AVL树更快，因为AVL树的深度较小。
>
> 删除：RB树具有恒定的最大旋转次数，但AVL树可以将O（log N）次旋转视为最差。并且平均而言，RB树也具有较少的旋转次数，因此RB树更快。
>
> 对于大数据：
>
> insert：AVL树更快。因为您需要在插入之前查找特定节点。当您有更多数据时，查找特定节点的时间差异与O（log N）成比例增长。但在最坏的情况下，AVL树和RB树仍然只需要恒定的旋转次数。因此，瓶颈将成为您查找该特定节点的时间。
>
> 查找：AVL树更快。（与小数据情况相同）
>
> 删除：AVL树平均速度更快，但在最坏的情况下，RB树更快。因为您还需要在删除之前查找非常深的节点以进行交换（类似于插入的原因）。平均而言，两棵树都有恒定的旋转次数。但RB树有一个恒定的旋转上限。
>
> --------------
>
>  
>
> 参考：AVL树与红黑树？
>
> 在AVL树中，从根到任何叶子的最短路径和最长路径之间的差异最多为1。在红黑树中，差异可以是2倍。
>
> 这两个都给O（log n）查找，但平衡AVL树可能需要O（log n）旋转，而红黑树将需要最多两次旋转使其达到平衡（尽管可能需要检查O（log n）节点以确定旋转的位置）。旋转本身是O（1）操作，因为你只是移动指针。
> ————————————————
> 版权声明：本文为CSDN博主「21aspnet」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/21aspnet/article/details/88939297
>
> # 红黑树临界点为什么是8
>
> JDK 1.8 的 HashMap 和 ConcurrentHashMap 都有这样一个特点：最开始的 Map 是空的，因为里面没有任何元素，往里放元素时会计算 hash 值，计算之后，第 1 个 value 会首先占用一个桶（也称为槽点）位置，后续如果经过计算发现需要落到同一个桶中，那么便会使用链表的形式往后延长，俗称“拉链法”，如图所示：
>
> 
>
> ![img](https:////upload-images.jianshu.io/upload_images/2079881-1b26aecd7300cbe0.png?imageMogr2/auto-orient/strip|imageView2/2/w/1104/format/webp)
>
> 
>
> 图中，有的桶是空的， 比如第 4 个；有的只有一个元素，比如 1、3、6；有的就是刚才说的拉链法，比如第 2 和第 5 个桶。
>
> 当链表长度大于或等于阈值（默认为 8）的时候，如果同时还满足容量大于或等于 MIN_TREEIFY_CAPACITY（默认为 64）的要求，就会把链表转换为红黑树。同样，后续如果由于删除或者其他原因调整了大小，当红黑树的节点小于或等于 6 个以后，又会恢复为链表形态。
>
> HashMap 的结构示意图：
>
> 
>
> ![img](https:////upload-images.jianshu.io/upload_images/2079881-b1c5d8d3f9445795.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
> 
>
> 在图中我们可以看到，有一些槽点是空的，有一些是拉链，有一些是红黑树。
>
> 每次遍历一个链表，平均查找的时间复杂度是 O(n)，n 是链表的长度。红黑树有和链表不一样的查找性能，由于红黑树有自平衡的特点，可以防止不平衡情况的发生，所以可以始终将查找的时间复杂度控制在 O(log(n))。最初链表还不是很长，所以可能 O(n) 和 O(log(n)) 的区别不大，但是如果链表越来越长，那么这种区别便会有所体现。所以为了提升查找性能，需要把链表转化为红黑树的形式。
>
> 那为什么不一开始就用红黑树，反而要经历一个转换的过程呢？
>
> 通过查看源码可以发现，默认是链表长度达到 8 就转成红黑树，而当长度降到 6 就转换回去，这体现了时间和空间平衡的思想，最开始使用链表的时候，空间占用是比较少的，而且由于链表短，所以查询时间也没有太大的问题。可是当链表越来越长，需要用红黑树的形式来保证查询的效率。对于何时应该从链表转化为红黑树，需要确定一个阈值，这个阈值默认为 8，并且在源码中也对选择 8 这个数字做了说明，原文如下：
>
> 
>
> ```cpp
> In usages with well-distributed user hashCodes, tree bins 
> are rarely used.  Ideally, under random hashCodes, the 
> frequency of nodes in bins follows a Poisson distribution 
> (http://en.wikipedia.org/wiki/Poisson_distribution) with a 
> parameter of about 0.5 on average for the default resizing 
> threshold of 0.75, although with a large variance because 
> of resizing granularity. Ignoring variance, the expected 
> occurrences of list size k are (exp(-0.5) * pow(0.5, k) / 
> factorial(k)). The first values are:
>  
>  0:    0.60653066
>  1:    0.30326533
>  2:    0.07581633
>  3:    0.01263606
>  4:    0.00157952
>  5:    0.00015795
>  6:    0.00001316
>  7:    0.00000094
>  8:    0.00000006
>  more: less than 1 in ten million
> ```
>
> 如果 hashCode 分布良好，也就是 hash 计算的结果离散好的话，那么红黑树这种形式是很少会被用到的，因为各个值都均匀分布，很少出现链表很长的情况。在理想情况下，链表长度符合泊松分布，各个长度的命中概率依次递减，当长度为 8 的时候，概率仅为 0.00000006。这是一个小于千万分之一的概率，通常我们的 Map 里面是不会存储这么多的数据的，所以通常情况下，并不会发生从链表向红黑树的转换。
>
> 但是，HashMap 决定某一个元素落到哪一个桶里，是和这个对象的 hashCode 有关的，JDK 并不能阻止我们用户实现自己的哈希算法，如果我们故意把哈希算法变得不均匀，例如：
>
> 
>
> ```java
> @Override
> public int hashCode() {
>     return 1;
> }
> ```
>
> 这里 hashCode 计算出来的值始终为 1，那么就很容易导致 HashMap 里的链表变得很长。让我们来看下面这段代码：
>
> 
>
> ```cpp
> public class HashMapDemo {
>  
>     public static void main(String[] args) {
>         HashMap map = new HashMap<HashMapDemo,Integer>(1);
>         for (int i = 0; i < 1000; i++) {
>             HashMapDemo hashMapDemo1 = new HashMapDemo();
>             map.put(hashMapDemo1, null);
>         }
>         System.out.println("运行结束");
>     }
>  
>     @Override
>     public int hashCode() {
>         return 1;
>     }
> }
> ```
>
> 在这个例子中，我们建了一个 HashMap，并且不停地往里放入值，所放入的 key 的对象，它的 hashCode 是被重写过得，并且始终返回 1。这段代码运行时，如果通过 debug 让程序暂停在 System.out.println("运行结束") 这行语句，我们观察 map 内的节点，可以发现已经变成了 TreeNode，而不是通常的 Node，这说明内部已经转为了红黑树。
>
> 
>
> ![img](https:////upload-images.jianshu.io/upload_images/2079881-8533331c25f97601.png?imageMogr2/auto-orient/strip|imageView2/2/w/836/format/webp)
>
> 链表长度超过 8 就转为红黑树的设计，更多的是为了防止用户自己实现了不好的哈希算法时导致链表过长，从而导致查询效率低，而此时转为红黑树更多的是一种保底策略，用来保证极端情况下查询的效率。
>
> 通常如果 hash 算法正常的话，那么链表的长度也不会很长，那么红黑树也不会带来明显的查询时间上的优势，反而会增加空间负担。所以通常情况下，并没有必要转为红黑树，所以就选择了概率非常小，小于千万分之一概率，也就是长度为 8 的概率，把长度 8 作为转化的默认阈值。
>
> 所以如果平时开发中发现 HashMap 或是 ConcurrentHashMap 内部出现了红黑树的结构，这个时候往往就说明我们的哈希算法出了问题，需要留意是不是我们实现了效果不好的 hashCode 方法，并对此进行改进，以便减少冲突。
>
> 
>
> 作者：wuchao226
> 链接：https://www.jianshu.com/p/fdf3d24fe3e8
> 来源：简书
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

拜占庭问题
一致性哈希

> 要了解一致性哈希，首先我们必须了解传统的哈希及其在大规模分布式系统中的局限性。简单地说，哈希就是一个键值对存储，在给定键的情况下，可以非常高效地找到所关联的值。假设我们要根据其邮政编码查找城市中的街道名称。一种最简单的实现方式是将此信息以哈希字典的形式进行存储 `<Zip Code，Street Name>`。
>
> 当数据太大而无法存储在一个节点或机器上时，问题变得更加有趣，系统中需要多个这样的节点或机器来存储它。比如，使用多个 Web 缓存中间件的系统。**那如何确定哪个 key 存储在哪个节点上？针对该问题，最简单的解决方案是使用哈希取模来确定。** 给定一个 key，先对 key 进行哈希运算，将其除以系统中的节点数，然后将该 key 放入该节点。同样，在获取 key 时，对 key 进行哈希运算，再除以节点数，然后转到该节点并获取值。上述过程对应的哈希算法定义如下：
>
> ```
> node_number = hash(key) % N # 其中 N 为节点数。
> ```
>
> 下图描绘了多节点系统中的传统的哈希取模算法，基于该算法可以实现简单的负载均衡。
>
> ![traditional-hashing.png](https://segmentfault.com/img/bVbA7as)
>
> > **阅读更多关于 Angular、TypeScript、Node.js/Java 、Spring 等技术文章，欢迎访问我的个人博客 ——[全栈修仙之路](http://www.semlinker.com/)**
>
> ### 一、传统哈希取模算法的局限性
>
> 下面我们来分析一下传统的哈希及其在大规模分布式系统中的局限性。这里我们直接使用我之前所写文章 [布隆过滤器你值得拥有的开发利器](https://segmentfault.com/a/1190000021136424) 中定义的 SimpleHash 类，然后分别对 **semlinker、kakuqo 和 test** 3 个键进行哈希运算并取余，具体代码如下：
>
> ```
> public class SimpleHash {
>     private int cap;
>     private int seed;
> 
>     public SimpleHash(int cap, int seed) {
>         this.cap = cap;
>         this.seed = seed;
>     }
> 
>     public int hash(String value) {
>         int result = 0;
>         int len = value.length();
>         for (int i = 0; i < len; i++) {
>             result = seed * result + value.charAt(i);
>         }
>         return (cap - 1) & result;
>     }
> 
>     public static void main(String[] args) {
>         SimpleHash simpleHash = new SimpleHash(2 << 12, 8);
>         System.out.println("node_number=hash(\"semlinker\") % 3 -> " + 
>           simpleHash.hash("semlinker") % 3);
>         System.out.println("node_number=hash(\"kakuqo\") % 3 -> " + 
>           simpleHash.hash("kakuqo") % 3);
>         System.out.println("node_number=hash(\"test\") % 3 -> " + 
>           simpleHash.hash("test") % 3);
>     }
> }
> ```
>
> 以上代码成功运行后，在控制台会输出以下结果：
>
> ```
> node_number=hash("semlinker") % 3 -> 1
> node_number=hash("kakuqo") % 3 -> 2
> node_number=hash("test") % 3 -> 0
> ```
>
> 基于以上的输出结果，我们可以创建以下表格：
>
> ![ch-three-nodes-hash.jpg](https://segmentfault.com/img/bVbA7aw)
>
> #### 1.1 节点减少的场景
>
> **在分布式多节点系统中，出现故障很常见。任何节点都可能在没有任何事先通知的情况下挂掉，针对这种情况我们期望系统只是出现性能降低，正常的功能不会受到影响。** 对于原始示例，当节点出现故障时会发生什么？原始示例中有的 3 个节点，假设其中 1 个节点出现故障，这时节点数发生了变化，节点个数从 3 减少为 2，此时表格的状态发生了变化：
>
> ![ch-two-nodes-hash.jpg](https://segmentfault.com/img/bVbA7ay)
>
> 很明显节点的减少会导致键与节点的映射关系发生变化，这个变化对于新的键来说并不会产生任何影响，但对于已有的键来说，将导致节点映射错误，以 “semlinker” 为例，变化前系统有 3 个节点，该键对应的节点编号为 1，当出现故障时，节点数减少为 2 个，此时该键对应的节点编号为 0。
>
> #### 1.2 节点增加的场景
>
> **在分布式多节点系统中，对于某些场景比如节日大促，就需要对服务节点进行扩容，以应对突发的流量。** 对于原始示例，当增加节点会发生什么？原始示例中有的 3 个节点，假设进行扩容临时增加了 1 个节点，这时节点数发生了变化，节点个数从 3 增加为 4 个，此时表格的状态发生了变化：
>
> ![ch-four-nodes-hash.jpg](https://segmentfault.com/img/bVbA7az)
>
> 很明显节点的增加也会导致键与节点的映射关系发生变化，这个变化对于新的键来说并不会产生任何影响，但对于已有的键来说，将导致节点映射错误，同样以 “semlinker” 为例，变化前系统有 3 个节点，该键对应的节点编号为 1，当增加节点时，节点数增加为 4 个，此时该键对应的节点编号为 2。
>
> 当集群中节点的数量发生变化时，之前的映射规则就可能发生变化。如果集群中每个机器提供的服务没有差别，这不会有什么影响。**但对于分布式缓存这种的系统而言，映射规则失效就意味着之前缓存的失效，若同一时刻出现大量的缓存失效，则可能会出现 “缓存雪崩”，这将会造成灾难性的后果。**
>
> **要解决此问题，我们必须在其余节点上重新分配所有现有键，这可能是非常昂贵的操作，并且可能对正在运行的系统产生不利影响。当然除了重新分配所有现有键的方案之外，还有另一种更好的方案即使用一致性哈希算法。**
>
> ### 二、一致性哈希算法
>
> 一致性哈希算法在 1997 年由麻省理工学院提出，是一种特殊的哈希算法，在移除或者添加一个服务器时，能够尽可能小地改变已存在的服务请求与处理请求服务器之间的映射关系。一致性哈希解决了简单哈希算法在分布式[哈希表](https://baike.baidu.com/item/哈希表/5981869)（Distributed Hash Table，DHT）中存在的动态伸缩等问题 。
>
> #### 2.1 一致性哈希算法优点
>
> - 可扩展性。一致性哈希算法保证了增加或减少服务器时，数据存储的改变最少，相比传统哈希算法大大节省了数据移动的开销 。
>
> - 更好地适应数据的快速增长。采用一致性哈希算法分布数据，当数据不断增长时，部分虚拟节点中可能包含很多数据、造成数据在虚拟节点上分布不均衡，此时可以将包含数据多的虚拟节点分裂，这种分裂仅仅是将原有的虚拟节点一分为二、不需要对全部的数据进行重新哈希和划分。
>
>   虚拟节点分裂后，如果物理服务器的负载仍然不均衡，只需在服务器之间调整部分虚拟节点的存储分布。这样可以随数据的增长而动态的扩展物理服务器的数量，且代价远比传统哈希算法重新分布所有数据要小很多。
>
> #### 2.2 一致性哈希算法与哈希算法的关系
>
> 一致性哈希算法是在哈希算法基础上提出的，在动态变化的分布式环境中，哈希算法应该满足的几个条件：平衡性、单调性和分散性。
>
> - **平衡性：是指 hash 的结果应该平均分配到各个节点，这样从算法上解决了负载均衡问题。**
> - **单调性：是指在新增或者删减节点时，不影响系统正常运行。**
> - **分散性：是指数据应该分散地存放在分布式集群中的各个节点（节点自己可以有备份），不必每个节点都存储所有的数据。**
>
> ### 三、一致性哈希算法原理
>
> 一致性哈希算法通过一个叫作一致性哈希环的数据结构实现。这个环的起点是 0，终点是 2^32 - 1，并且起点与终点连接，故这个环的整数分布范围是 [0, 2^32-1]，如下图所示：
>
> ![hash-ring.jpg](https://segmentfault.com/img/bVbA7aA)
>
> #### 3.1 将对象放置到哈希环
>
> 假设我们有 "semlinker"、"kakuqo"、"lolo"、"fer" 四个对象，分别简写为 o1、o2、o3 和 o4，然后使用哈希函数计算这个对象的 hash 值，值的范围是 [0, 2^32-1]：
>
> ![hash-ring-hash-objects.jpg](https://segmentfault.com/img/bVbA7aB)
>
> 图中对象的映射关系如下：
>
> ```
> hash(o1) = k1; hash(o2) = k2;
> hash(o3) = k3; hash(o4) = k4;
> ```
>
> #### 3.2 将服务器放置到哈希环
>
> 接着使用同样的哈希函数，我们将服务器也放置到哈希环上，可以选择服务器的 IP 或主机名作为键进行哈希，这样每台服务器就能确定其在哈希环上的位置。这里假设我们有 3 台缓存服务器，分别为 cs1、cs2 和 cs3：
>
> ![hash-ring-hash-servers.jpg](https://segmentfault.com/img/bVbA7aG)
>
> 图中服务器的映射关系如下：
>
> ```
> hash(cs1) = t1; hash(cs2) = t2; hash(cs3) = t3; # Cache Server
> ```
>
> #### 3.3 为对象选择服务器
>
> **将对象和服务器都放置到同一个哈希环后，在哈希环上顺时针查找距离这个对象的 hash 值最近的机器，即是这个对象所属的机器。** 以 o2 对象为例，顺序针找到最近的机器是 cs2，故服务器 cs2 会缓存 o2 对象。而服务器 cs1 则缓存 o1，o3 对象，服务器 cs3 则缓存 o4 对象。
>
> ![hash-ring-objects-servers.jpg](https://segmentfault.com/img/bVbA7aH)
>
> #### 3.4 服务器增加的情况
>
> 假设由于业务需要，我们需要增加一台服务器 cs4，经过同样的 hash 运算，该服务器最终落于 t1 和 t2 服务器之间，具体如下图所示：
>
> ![hash-ring-add-server.jpg](https://segmentfault.com/img/bVbA7aI)
>
> 对于上述的情况，只有 t1 和 t2 服务器之间的对象需要重新分配。在以上示例中只有 o3 对象需要重新分配，即它被重新到 cs4 服务器。在前面我们已经分析过，如果使用简单的取模方法，当新添加服务器时可能会导致大部分缓存失效，而使用一致性哈希算法后，这种情况得到了较大的改善，因为只有少部分对象需要重新分配。
>
> #### 3.5 服务器减少的情况
>
> 假设 cs3 服务器出现故障导致服务下线，这时原本存储于 cs3 服务器的对象 o4，需要被重新分配至 cs2 服务器，其它对象仍存储在原有的机器上。
>
> ![hash-ring-remove-server.jpg](https://segmentfault.com/img/bVbA7aJ)
>
> #### 3.6 虚拟节点
>
> 到这里一致性哈希的基本原理已经介绍完了，但对于新增服务器的情况还存在一些问题。新增的服务器 cs4 只分担了 cs1 服务器的负载，服务器 cs2 和 cs3 并没有因为 cs4 服务器的加入而减少负载压力。如果 cs4 服务器的性能与原有服务器的性能一致甚至可能更高，那么这种结果并不是我们所期望的。
>
> **针对这个问题，我们可以通过引入虚拟节点来解决负载不均衡的问题。即将每台物理服务器虚拟为一组虚拟服务器，将虚拟服务器放置到哈希环上，如果要确定对象的服务器，需先确定对象的虚拟服务器，再由虚拟服务器确定物理服务器。**
>
> ![ch-virtual-nodes.jpg](https://segmentfault.com/img/bVbA7aK)
>
> 图中 o1 和 o2 表示对象，v1 ~ v6 表示虚拟服务器，s1 ~ s3 表示物理服务器。
>
> ### 四、一致性哈希算法实现
>
> 这里我们只介绍不带虚拟节点的一致性哈希算法实现：
>
> ```
> import java.util.SortedMap;
> import java.util.TreeMap;
> 
> public class ConsistentHashingWithoutVirtualNode {
>     //待添加入Hash环的服务器列表
>     private static String[] servers = {"192.168.0.1:8888", "192.168.0.2:8888", 
>       "192.168.0.3:8888"};
> 
>     //key表示服务器的hash值，value表示服务器
>     private static SortedMap<Integer, String> sortedMap = new TreeMap<Integer, String>();
> 
>     //程序初始化，将所有的服务器放入sortedMap中
>     static {
>         for (int i = 0; i < servers.length; i++) {
>             int hash = getHash(servers[i]);
>             System.out.println("[" + servers[i] + "]加入集合中, 其Hash值为" + hash);
>             sortedMap.put(hash, servers[i]);
>         }
>     }
> 
>     //得到应当路由到的结点
>     private static String getServer(String key) {
>         //得到该key的hash值
>         int hash = getHash(key);
>         //得到大于该Hash值的所有Map
>         SortedMap<Integer, String> subMap = sortedMap.tailMap(hash);
>         if (subMap.isEmpty()) {
>             //如果没有比该key的hash值大的，则从第一个node开始
>             Integer i = sortedMap.firstKey();
>             //返回对应的服务器
>             return sortedMap.get(i);
>         } else {
>             //第一个Key就是顺时针过去离node最近的那个结点
>             Integer i = subMap.firstKey();
>             //返回对应的服务器
>             return subMap.get(i);
>         }
>     }
> 
>     //使用FNV1_32_HASH算法计算服务器的Hash值
>     private static int getHash(String str) {
>         final int p = 16777619;
>         int hash = (int) 2166136261L;
>         for (int i = 0; i < str.length(); i++)
>             hash = (hash ^ str.charAt(i)) * p;
>         hash += hash << 13;
>         hash ^= hash >> 7;
>         hash += hash << 3;
>         hash ^= hash >> 17;
>         hash += hash << 5;
> 
>         // 如果算出来的值为负数则取其绝对值
>         if (hash < 0)
>             hash = Math.abs(hash);
>         return hash;
>     }
> 
>     public static void main(String[] args) {
>         String[] keys = {"semlinker", "kakuqo", "fer"};
>         for (int i = 0; i < keys.length; i++)
>             System.out.println("[" + keys[i] + "]的hash值为" + getHash(keys[i])
>                     + ", 被路由到结点[" + getServer(keys[i]) + "]");
>     }
> 
> }
> ```
>
> 以上代码成功运行后，在控制台会输出以下结果：
>
> ```
> [192.168.0.1:8888]加入集合中, 其Hash值为1326271016
> [192.168.0.2:8888]加入集合中, 其Hash值为1132535844
> [192.168.0.3:8888]加入集合中, 其Hash值为115798597
> 
> [semlinker]的hash值为1549041406, 被路由到结点[192.168.0.3:8888]
> [kakuqo]的hash值为463104755, 被路由到结点[192.168.0.2:8888]
> [fer]的hash值为1677150790, 被路由到结点[192.168.0.3:8888]
> ```
>
> 上面我们只介绍了不带虚拟节点的一致性哈希算法实现，如果有的小伙伴对带虚拟节点的一致性哈希算法感兴趣，可以参考 [一致性Hash(Consistent Hashing)原理剖析及Java实现](https://blog.csdn.net/suifeng629/article/details/81567777) 这篇文章。
>
> ### 五、总结
>
> 本文通过示例介绍了传统的哈希取模算法在分布式系统中的局限性，进而在针对该问题的解决方案中引出了一致性哈希算法。一致性哈希算法在 1997 年由麻省理工学院提出，是一种特殊的哈希算法，在移除或者添加一个服务器时，能够尽可能小地改变已存在的服务请求与处理请求服务器之间的映射关系。在介绍完一致性哈希算法的作用和优点等相关知识后，我们以图解的形式生动介绍了一致性哈希算法的原理，最后给出了不带虚拟节点的一致性哈希算法的 Java 实现。

如何控制负载均衡。

> ## 1、前言
>
> 
> 在一个典型的高并发、大用户量的Web互联网系统的架构设计中，对HTTP集群的负载均衡设计是作为高性能系统优化环节中必不可少的方案。HTTP负载均衡的本质上是将Web用户流量进行均衡减压，因此在互联网的大流量项目中，其重要性不言而喻。
>
> 本文将以简洁通俗的文字，为你讲解主流的HTTP服务端实现负载均衡的常见方案，以及具体到方案中的负载均衡算法的实现原理。理解和掌握这些方案、算法原理，有助于您今后的互联网项的技术选型和架构设计，因为没有哪一种方案和算法能解决所有问题，只有针对特定的场景使用合适的方案和算法才是最明智的选择。
>
> **即时通讯网注：**本文中所提及的HTTP负载均衡方案和算法，并不完全适用IM即时通讯Socket长连接的负载均衡，因为IM长连接、有状态的特性，跟HTTP这种短连接、无状态的特征是矛盾的，所以请勿盲目套用。但，一个完整的IM系统是由HTTP短连接+IM长连接组成，因而本文内容虽不能套用于IM长连接的负载均衡方案，但可以用于您IM的高并发、大用户量的HTTP短连接的方案设计。
>
> ## 2、相关文章
>
> 
> **深入阅读以下文章，有助于您更好地理解本篇内容：**
>
> 
>
> - 《[网络编程懒人入门(一)：快速理解网络通信协议（上篇）](http://www.52im.net/thread-1095-1-1.html)》
> - 《[网络编程懒人入门(二)：快速理解网络通信协议（下篇）](http://www.52im.net/thread-1103-1-1.html)》
> - 《[网络编程懒人入门(三)：快速理解TCP协议一篇就够](http://www.52im.net/thread-1107-1-1.html)》
> - 《[高性能网络编程(七)：到底什么是高并发？一文即懂！](http://www.52im.net/thread-3120-1-1.html)》
> - 《[一篇读懂分布式架构下的负载均衡技术：分类、原理、算法、常见方案等](http://www.52im.net/thread-2494-1-1.html)》
> - 《[腾讯资深架构师干货总结：一文读懂大型分布式系统设计的方方面面](http://www.52im.net/thread-1811-1-1.html)》
> - 《[新手入门：零基础理解大型分布式架构的演进历史、技术原理、最佳实践](http://www.52im.net/thread-2007-1-1.html)》
> - 《[通俗易懂：基于集群的移动端IM接入层负载均衡方案分享](http://www.52im.net/thread-802-1-1.html)》
> - 《[IM开发基础知识补课：正确理解前置HTTP SSO单点登陆接口的原理](http://www.52im.net/thread-1351-1-1.html)》
>
> 
>
> ## 3、什么是负载均衡？
>
> 
> 早期的互联网应用，由于用户流量比较小，业务逻辑也比较简单，往往一个单服务器就能满足负载需求。随着现在互联网的流量越来越大，稍微好一点的系统，访问量就非常大了，并且系统功能也越来越复杂，那么单台服务器就算将性能优化得再好，也不能支撑这么大用户量的访问压力了，这个时候就需要使用多台机器，设计高性能的集群来应对。
>
> 那么，多台服务器是如何去均衡流量、如何组成高性能的集群的呢？
>
> 此时就需要请出 「负载均衡器」 入场了。
>
> 负载均衡（Load Balancer）是指把用户访问的流量，通过「负载均衡器」，根据某种转发的策略，均匀的分发到后端多台服务器上，后端的服务器可以独立的响应和处理请求，从而实现分散负载的效果。负载均衡技术提高了系统的服务能力，增强了应用的可用性。
>
> **负载均衡的基本原理就像下图这样：**
>
> ![快速理解高性能HTTP服务端的负载均衡技术原理_1.jpeg](http://www.52im.net/data/attachment/forum/201809/09/200153i2tarh2z902z2js2.jpeg)
>
> 
>
> ## 4、主流负载均衡方案有几种？
>
> 
> **目前市面上最常见的负载均衡技术方案主要有三种：**
>
> 
>
> - 1）基于DNS负载均衡；
> - 2）基于硬件负载均衡：比如F5
> - 3）基于软件负载均衡：比如Nginx、Squid。
>
> 
> 三种方案各有优劣，DNS负载均衡可以实现在地域上的流量均衡，硬件负载均衡主要用于大型服务器集群中的负载需求，而软件负载均衡大多是基于机器层面的流量均衡。在实际场景中，这三种是可以组合在一起使用。
>
> 下面分别来详细讲讲这些方案。
>
> 
>
> ### 4.1基于DNS负载均衡
>
> 
>
> ![快速理解高性能HTTP服务端的负载均衡技术原理_2.png](http://www.52im.net/data/attachment/forum/201809/09/200530hcevw773zffjyy3u.png)
>
> 
>
> 基于DNS来做负载均衡其实是一种最简单的实现方案，通过在DNS服务器上做一个简单配置即可。
>
> **其原理就是：****当用户访问域名的时候，会先向DNS服务器去解析域名对应的IP地址，这个时候我们可以让DNS服务器根据不同地理位置的用户返回不同的IP。**比如南方的用户就返回我们在广州业务服务器的IP，北方的用户来访问的话，我就返回北京业务服务器所在的IP。
>
> 在这个模式下，用户就相当于实现了按照「就近原则」将请求分流了，既减轻了单个集群的负载压力，也提升了用户的访问速度。
>
> 使用DNS做负载均衡的方案，天然的优势就是配置简单，实现成本非常低，无需额外的开发和维护工作。
>
> **但是它也有一个明显的缺点：**当配置修改后，生效不及时。这个是由于DNS的特性导致的，DNS一般会有多级缓存，所以当我们修改了DNS配置之后，由于缓存的原因，会导致IP变更不及时，从而影响负载均衡的效果。
>
> 另外，使用DNS做负载均衡的话，大多是基于地域或者干脆直接做IP轮询，没有更高级的路由策略，所以这也是DNS方案的局限所在。
>
> 
>
> ### 4.2基于硬件负载均衡
>
> 
>
> ![快速理解高性能HTTP服务端的负载均衡技术原理_3.jpg](http://www.52im.net/data/attachment/forum/201809/09/202058oer8zh9r79lr9nrj.jpg)
>
> 
>
> 硬件的负载均衡那就比较牛逼了，比如大名鼎鼎的 F5 Network Big-IP，也就是我们常说的 F5，它是一个网络设备，你可以简单的理解成类似于网络交换机的东西，完全通过硬件来抗压力，性能是非常的好，每秒能处理的请求数达到百万级，即 几百万/秒 的负载，当然价格也就非常非常贵了，十几万到上百万人民币都有。
>
> 因为这类设备一般用在大型互联网公司的流量入口最前端，以及政府、国企等不缺钱企业会去使用。一般的中小公司是不舍得用的。
>
> 采用 F5 这类硬件做负载均衡的话，主要就是省心省事，买一台就搞定，性能强大，一般的业务不在话下。而且在负载均衡的算法方面还支持很多灵活的策略，同时还具有一些防火墙等安全功能。但是缺点也很明显，一个字：贵。
>
> 
>
> ### 4.3基于软件负载均衡
>
> 
>
> ![快速理解高性能HTTP服务端的负载均衡技术原理_4.jpeg](http://www.52im.net/data/attachment/forum/201809/09/200735n1gpm153g3lkdjzq.jpeg)
>
> 
>
> 软件负载均衡是指使用软件的方式来分发和均衡流量。软件负载均衡分为7层协议 和 4层协议。
>
> 网络协议有七层，基于第四层传输层来做流量分发的方案称为4层负载均衡，例如 LVS；而基于第七层应用层来做流量分发的称为7层负载均衡，例如 Nginx。这两种在性能和灵活性上是有些区别的。
>
> 基于4层的负载均衡性能要高一些，一般能达到 几十万/秒 的处理量，而基于7层的负载均衡处理量一般只在 几万/秒 。
>
> 基于软件的负载均衡的特点也很明显，便宜。在正常的服务器上部署即可，无需额外采购，就是投入一点技术去优化优化即可，因此这种方式是互联网公司中用得最多的一种方式。
>
> ## 5、常用的均衡算法有哪些？
>
> 
> 上面讲完了常见的负载均衡技术方案，那么接下来咱们看一下，在实际方案应用中，一般可以使用哪些均衡算法？
>
> **主要的均衡算法有：**
>
> 
>
> - 1）轮询策略；
> - 2）负载度策略；
> - 3）响应策略；
> - 4）哈希策略。
>
> 
> 下面来分别介绍一下这几种均衡算法/策略的特点。
>
> 
>
> ### 5.1轮询策略
>
>
> **轮询策略其实很好理解，就是当用户请求来了之后，「负载均衡器」将请求轮流的转发到后端不同的业务服务器上。**这个策略在DNS方案中用的比较多，无需关注后端服务的状态，只药有请求，就往后端轮流转发，非常的简单、实用。
>
> 在实际应用中，轮询也会有多种方式，有按顺序轮询的、有随机轮询的、还有按照权重来轮询的。前两种比较好理解，第三种按照权重来轮询，是指给每台后端服务设定一个权重值，比如性能高的服务器权重高一些，性能低的服务器给的权重低一些，这样设置的话，分配流量的时候，给权重高的更多流量，可以充分的发挥出后端机器的性能。
>
> 
>
> ### 5.2负载度策略
>
>
> **负载度策略是指当「负载均衡器」往后端转发流量的时候，会先去评估后端每台服务器的负载压力情况，对于压力比较大的后端服务器转发的请求就少一些，对于压力比较小的后端服务器可以多转发一些请求给它。**
>
> 这种方式就充分的结合了后端服务器的运行状态，来动态的分配流量了，比轮询的方式更为科学一些。
>
> 但是这种方式也带来了一些弊端，因为需要动态的评估后端服务器的负载压力，那这个「负载均衡器」除了转发请求以外，还要做很多额外的工作，比如采集 连接数、请求数、CPU负载指标、IO负载指标等等，通过对这些指标进行计算和对比，判断出哪一台后端服务器的负载压力较大。
>
> 因此这种方式带来了效果优势的同时，也增加了「负载均衡器」的实现难度和维护成本。
>
> 
>
> ### 5.3响应策略
>
>
> **响应策略是指，当用户请求过来的时候，「负载均衡器」会优先将请求转发给当前时刻响应最快的后端服务器。**
>
> 也就是说，不管后端服务器负载高不高，也不管配置如何，只要觉得这个服务器在当前时刻能最快的响应用户的请求，那么就优先把请求转发给它，这样的话，对于用户而言，体验也最好。
>
> 那「负载均衡器」是怎么知道哪一台后端服务在当前时刻响应能力最佳呢？
> 这就需要「负载均衡器」不停的去统计每一台后端服务器对请求的处理速度了，比如一分钟统计一次，生成一个后端服务器处理速度的排行榜。然后「负载均衡器」根据这个排行榜去转发服务。
>
> 那么这里的问题就是统计的成本了，不停的做这些统计运算本身也会消耗一些性能，同时也会增加「负载均衡器」的实现难度和维护成本。
>
> 
>
> ### 5.4哈希策略
>
>
> **Hash策略也比较好理解，就是将请求中的某个信息进行hash计算，然后根据后端服务器台数取模，得到一个值，算出相同值的请求就被转发到同一台后端服务器中。**
>
> 常见的用法是对用户的IP或者ID进行这个策略，然后「负载均衡器」就能保证同一个IP来源或者同一个用户永远会被送到同一个后端服务器上了，一般用于处理缓存、会话等功能的时候特别好用。
>
> ## 6、本文小结
>
> 
> 基于运营成本的考虑，目前在互联网项目中，HTTP服务端的负载均衡的具体解决方案，用的最多的还是Nginx（及其分支，比如淘宝的Tengin），如果有时间，建议可以把Nginx下载下来，仔细研究研究它的负载均衡原理，以及它所提供的几种负载均衡算法，理论联系实际来学习就更能促进你对相关知识的理解和掌握了。
>
> 得益于Nginx这种开源免费的高性能方案，它们间接地促进了互联网的繁荣，感谢这些伟大开源方案背后的无私贡献者们！

http码 302 403 。

> 302表示临时性重定向，403表示无访问权限

https 加密过程。
操作系统虚存实现原理，交换，覆盖区别。
paxos算法。
NP 问题、 举例。

> 为了避免对这四个问题有一定理解基础的人看的很烦，个人简单理解的四个问题：
>
> P问题：有多项式时间算法，算得很快的问题。
>
> NP问题：算起来不确定快不快的问题，但是我们可以快速验证这个问题的解。
>
> NP-complete问题：属于NP问题，且属于NP-hard问题。
>
> NP-hard问题：比NP问题都要难的问题。

缓冲区满异常是什么原因。
innodb 和 mysalm的区别。
堆排序的时间复杂度、空间复杂度、排序的的过程。

> **堆排序**
>
> 　　堆排序是利用**堆**这种数据结构而设计的一种排序算法，堆排序是一种**选择排序，**它的最坏，最好，平均时间复杂度均为O(nlogn)，它也是不稳定排序。首先简单了解下堆结构。
>
> **堆**
>
> 　　**堆是具有以下性质的完全二叉树：每个结点的值都大于或等于其左右孩子结点的值，称为大顶堆；或者每个结点的值都小于或等于其左右孩子结点的值，称为小顶堆。如下图：**
>
> ![img](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161217182750011-675658660.png)
>
> 同时，我们对堆中的结点按层进行编号，将这种逻辑结构映射到数组中就是下面这个样子
>
> ![img](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161217182857323-2092264199.png)
>
> 该数组从逻辑上讲就是一个堆结构，我们用简单的公式来描述一下堆的定义就是：
>
> **大顶堆：arr[i] >= arr[2i+1] && arr[i] >= arr[2i+2]**  
>
> **小顶堆：arr[i] <= arr[2i+1] && arr[i] <= arr[2i+2]**  
>
> ok，了解了这些定义。接下来，我们来看看堆排序的基本思想及基本步骤：
>
> 以大根堆为例，**基本思想**：
>
> - 将初始待排序关键字序列(R1,R2....Rn)构建成大顶堆，此堆为初始的无序区
> - 将堆顶元素R[1]与最后一个元素R[n]交换，此时得到新的无序区(R1,R2,......Rn-1)和新的有序区(Rn)
> - 由于交换后新的堆顶R[1]可能违反堆的性质，因此需要对当前无序区(R1,R2,......Rn-1)调整为新堆，然后再次将R[1]与无序区最后一个元素交换，得到新的无序区(R1,R2....Rn-2)和新的有序区(Rn-1,Rn)。不断重复此过程直到有序区的元素个数为n-1，则整个排序过程完成。
>
> # 堆排序基本思想及步骤
>
> > 　　**堆排序的基本思想是：将待排序序列构造成一个大顶堆，此时，整个序列的最大值就是堆顶的根节点。将其与末尾元素进行交换，此时末尾就为最大值。然后将剩余n-1个元素重新构造成一个堆，这样会得到n个元素的次小值。如此反复执行，便能得到一个有序序列了**
>
> **步骤一 构造初始堆。将给定无序序列构造成一个大顶堆（一般升序采用大顶堆，降序采用小顶堆)。**
>
> 　　a.假设给定无序序列结构如下
>
> ![img](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161217192038651-934327647.png)
>
> 2.此时我们从最后一个非叶子结点开始（叶结点自然不用调整，第一个非叶子结点 arr.length/2-1=5/2-1=1，也就是下面的6结点），从左至右，从下至上进行调整。
>
> ![img](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161217192209433-270379236.png)
>
> 4.找到第二个非叶节点4，由于[4,9,8]中9元素最大，4和9交换。
>
> ![img](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161217192854636-1823585260.png)
>
> 这时，交换导致了子根[4,5,6]结构混乱，继续调整，[4,5,6]中6最大，交换4和6。
>
> ![img](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161217193347886-1142194411.png)
>
> 此时，我们就将一个无需序列构造成了一个大顶堆。
>
> **步骤二 将堆顶元素与末尾元素进行交换，使末尾元素最大。然后继续调整堆，再将堆顶元素与末尾元素交换，得到第二大元素。如此反复进行交换、重建、交换。**
>
> a.将堆顶元素9和末尾元素4进行交换
>
> ![img](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161217194207620-1455153342.png)
>
> b.重新调整结构，使其继续满足堆定义
>
> ![img](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161218153110495-1280388728.png)
>
> c.再将堆顶元素8与末尾元素5进行交换，得到第二大元素8.
>
> ![img](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161218152929339-1114983222.png)
>
> 后续过程，继续进行调整，交换，如此反复进行，最终使得整个序列有序
>
> ![img](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161218152348229-935654830.png)
>
> 再简单总结下堆排序的基本思路：
>
> 　　**a.将无需序列构建成一个堆，根据升序降序需求选择大顶堆或小顶堆;**
>
> 　　**b.将堆顶元素与末尾元素交换，将最大元素"沉"到数组末端;**
>
> 　　**c.重新调整结构，使其满足堆定义，然后继续交换堆顶元素与当前末尾元素，反复执行调整+交换步骤，直到整个序列有序。**
>
> # 代码实现
>
> [![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)
>
> ```
> package sortdemo;
> 
> import java.util.Arrays;
> 
> /**
>  * Created by chengxiao on 2016/12/17.
>  * 堆排序demo
>  */
> public class HeapSort {
>     public static void main(String []args){
>         int []arr = {9,8,7,6,5,4,3,2,1};
>         sort(arr);
>         System.out.println(Arrays.toString(arr));
>     }
>     public static void sort(int []arr){
>         //1.构建大顶堆
>         for(int i=arr.length/2-1;i>=0;i--){
>             //从第一个非叶子结点从下至上，从右至左调整结构
>             adjustHeap(arr,i,arr.length);
>         }
>         //2.调整堆结构+交换堆顶元素与末尾元素
>         for(int j=arr.length-1;j>0;j--){
>             swap(arr,0,j);//将堆顶元素与末尾元素进行交换
>             adjustHeap(arr,0,j);//重新对堆进行调整
>         }
> 
>     }
> 
>     /**
>      * 调整大顶堆（仅是调整过程，建立在大顶堆已构建的基础上）
>      * @param arr
>      * @param i
>      * @param length
>      */
>     public static void adjustHeap(int []arr,int i,int length){
>         int temp = arr[i];//先取出当前元素i
>         for(int k=i*2+1;k<length;k=k*2+1){//从i结点的左子结点开始，也就是2i+1处开始
>             if(k+1<length && arr[k]<arr[k+1]){//如果左子结点小于右子结点，k指向右子结点
>                 k++;
>             }
>             if(arr[k] >temp){//如果子节点大于父节点，将子节点值赋给父节点（不用进行交换）
>                 arr[i] = arr[k];
>                 i = k;
>             }else{
>                 break;
>             }
>         }
>         arr[i] = temp;//将temp值放到最终的位置
>     }
> 
>     /**
>      * 交换元素
>      * @param arr
>      * @param a
>      * @param b
>      */
>     public static void swap(int []arr,int a ,int b){
>         int temp=arr[a];
>         arr[a] = arr[b];
>         arr[b] = temp;
>     }
> }
> ```
>
> [![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)
>
> 结果
>
> ```
> [1, 2, 3, 4, 5, 6, 7, 8, 9]
> ```
>
> # 最后
>
> 　　堆排序是一种选择排序，整体主要由构建初始堆+交换堆顶元素和末尾元素并重建堆两部分组成。其中构建初始堆经推导复杂度为O(n)，在交换并重建堆的过程中，需交换n-1次，而重建堆的过程中，根据完全二叉树的性质，[log2(n-1),log2(n-2)...1]逐步递减，近似为nlogn。所以堆排序时间复杂度一般认为就是O(nlogn)级。
>
> 作者： [dreamcatcher-cx](http://www.cnblogs.com/chengxiao/)
>
> 出处： [](http://www.cnblogs.com/chengxiao/)
>
> 本文版权归作者和博客园共有，欢迎转载，但未经作者同意必须保留此段声明，且在页面明显位置给出原文链接。

spring问题。
算法 ： 对一个八位数有三种操作： 加一、减一、反转 。 至少多少次操作可以把一个八位数A变成八位数B。
头条一面后，我觉得自己凉凉了，算法也不会，题目也有些不会。但还是给了二面

## 头条2面：
死锁必要条件
java如何处理死锁

> ### 背景
>
> 这个话题是源自笔者以前跟人的一次技术讨论，“你是怎么发现死锁的并且是如何预防、如何解决的？”以前听到的这个问题的时候，虽然脑海里也有一些思路，但是都是不够系统化的东西。直到最近亲身经历一次死锁，才做了这么一次集中的思路整理，撰录以下文字。希望对同样问题的同学有所帮助。
>
> ### 死锁定义
>
> 首先我们先来看看死锁的定义：“死锁是指两个或两个以上的进程在执行过程中，由于竞争资源或者由于彼此通信而造成的一种阻塞的现象，若无外力作用，它们都将无法推进下去。”那么我们换一个更加规范的定义：“集合中的每一个进程都在等待只能由本集合中的其他进程才能引发的事件，那么该组进程是死锁的。”
>
> 竞争的资源可以是：锁、网络连接、通知事件，磁盘、带宽，以及一切可以被称作“资源”的东西。
>
> ### 举个栗子
>
> 上面的内容可能有些抽象，因此我们举个例子来描述，如果此时有一个线程A，按照先锁a再获得锁b的的顺序获得锁，而在此同时又有另外一个线程B，按照先锁b再锁a的顺序获得锁。如下图所示：
>
> ![死锁](https://user-gold-cdn.xitu.io/2018/3/19/1623d495a36b4c2c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
>
> 
>
> 我们用一段代码来模拟上述过程：
>
> ```
> public static void main(String[] args) {
>     final Object a = new Object();
>     final Object b = new Object();
>     Thread threadA = new Thread(new Runnable() {
>         public void run() {
>             synchronized (a) {
>                 try {
>                     System.out.println("now i in threadA-locka");
>                     Thread.sleep(1000l);
>                     synchronized (b) {
>                         System.out.println("now i in threadA-lockb");
>                     }
>                 } catch (Exception e) {
>                     // ignore
>                 }
>             }
>         }
>     });
> 
>     Thread threadB = new Thread(new Runnable() {
>         public void run() {
>             synchronized (b) {
>                 try {
>                     System.out.println("now i in threadB-lockb");
>                     Thread.sleep(1000l);
>                     synchronized (a) {
>                         System.out.println("now i in threadB-locka");
>                     }
>                 } catch (Exception e) {
>                     // ignore
>                 }
>             }
>         }
>     });
> 
>     threadA.start();
>     threadB.start();
> }
> 复制代码
> ```
>
> 程序执行结果如下：
>
> ![程序执行结果](https://user-gold-cdn.xitu.io/2018/3/19/1623d495a356a7c7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
>
> 很明显，程序执行停滞了。
>
> 
>
> ### 死锁检测
>
> 在这里，我将介绍两种死锁检测工具
>
> #### 1、Jstack命令
>
> jstack是java虚拟机自带的一种堆栈跟踪工具。jstack用于打印出给定的java进程ID或core file或远程调试服务的Java堆栈信息。 Jstack工具可以用于生成java虚拟机当前时刻的线程快照。**线程快照**是当前java虚拟机内每一条线程**正在执行**的**方法堆栈**的集合，生成线程快照的主要目的是定位线程出现长时间停顿的原因，如`线程间死锁`、`死循环`、`请求外部资源导致的长时间等待`等。 线程出现停顿的时候通过jstack来查看各个线程的调用堆栈，就可以知道没有响应的线程到底在后台做什么事情，或者等待什么资源。
>
> 首先，我们通过jps确定当前执行任务的进程号:
>
> ```
> jonny@~$ jps
> 597
> 1370 JConsole
> 1362 AppMain
> 1421 Jps
> 1361 Launcher
> 复制代码
> ```
>
> 可以确定任务进程号是1362，然后执行jstack命令查看当前进程堆栈信息：
>
> ```
> jonny@~$ jstack -F 1362
> Attaching to process ID 1362, please wait...
> Debugger attached successfully.
> Server compiler detected.
> JVM version is 23.21-b01
> Deadlock Detection:
> 
> Found one Java-level deadlock:
> =============================
> 
> "Thread-1":
>   waiting to lock Monitor@0x00007fea1900f6b8 (Object@0x00000007efa684c8, a java/lang/Object),
>   which is held by "Thread-0"
> "Thread-0":
>   waiting to lock Monitor@0x00007fea1900ceb0 (Object@0x00000007efa684d8, a java/lang/Object),
>   which is held by "Thread-1"
> 
> Found a total of 1 deadlock.
> 复制代码
> ```
>
> 可以看到，进程的确存在死锁，两个线程分别在等待对方持有的Object对象
>
> #### 2、JConsole工具
>
> Jconsole是JDK自带的监控工具，在JDK/bin目录下可以找到。它用于连接正在运行的本地或者远程的JVM，对运行在Java应用程序的资源消耗和性能进行监控，并画出大量的图表，提供强大的可视化界面。而且本身占用的服务器内存很小，甚至可以说几乎不消耗。
>
> 我们在命令行中敲入jconsole命令，会自动弹出以下对话框，选择进程1362，并点击“**链接**”
>
> 
>
> ![新建连接](https://user-gold-cdn.xitu.io/2018/3/19/1623d495a37b40a7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
>
> 
>
> 进入所检测的进程后，选择“线程”选项卡，并点击“检测死锁”
>
> ![检测死锁](https://user-gold-cdn.xitu.io/2018/3/19/1623d495a36554b7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
>
> 可以看到以下画面：
>
> ![死锁检测结果](https://user-gold-cdn.xitu.io/2018/3/19/1623d495a56c849f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
>
> 可以看到进程中存在死锁。
>
> 
>
> 以上例子我都是用synchronized关键词实现的死锁，如果读者用ReentrantLock制造一次死锁，再次使用死锁检测工具，也同样能检测到死锁，不过显示的信息将会更加丰富，有兴趣的读者可以自己尝试一下。
>
> ### 死锁预防
>
> 如果一个线程每次只能获得一个锁，那么就不会产生锁顺序的死锁。虽然不算非常现实，但是也非常正确（一个问题的最好解决办法就是，这个问题恰好不会出现）。不过关于死锁的预防，这里有以下几种方案：
>
> #### 1、以确定的顺序获得锁
>
> 如果必须获取多个锁，那么在设计的时候需要充分考虑不同线程之前获得锁的顺序。按照上面的例子，两个线程获得锁的时序图如下：
>
> ![时序图](https://user-gold-cdn.xitu.io/2018/3/19/1623d495aa379fe7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
>
> 
>
> 如果此时把获得锁的时序改成：
>
> ![新时序图](https://user-gold-cdn.xitu.io/2018/3/19/1623d495c63027dc?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
>
> 那么死锁就永远不会发生。 针对两个特定的锁，开发者可以尝试按照锁对象的hashCode值大小的顺序，分别获得两个锁，这样锁总是会以特定的顺序获得锁，那么死锁也不会发生。
>
> ![哲学家进餐](https://user-gold-cdn.xitu.io/2018/3/19/1623d495c647d5c4?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
>
> 
>
> 问题变得更加复杂一些，如果此时有多个线程，都在竞争不同的锁，简单按照锁对象的hashCode进行排序（单纯按照hashCode顺序排序会出现“环路等待”），可能就无法满足要求了，这个时候开发者可以使用**银行家算法**，所有的锁都按照特定的顺序获取，同样可以防止死锁的发生，该算法在这里就不再赘述了，有兴趣的可以自行了解一下。
>
> #### 2、超时放弃
>
> 当使用synchronized关键词提供的内置锁时，只要线程没有获得锁，那么就会永远等待下去，然而Lock接口提供了`boolean tryLock(long time, TimeUnit unit) throws InterruptedException`方法，该方法可以按照固定时长等待锁，因此线程可以在获取锁超时以后，主动释放之前已经获得的所有的锁。通过这种方式，也可以很有效地避免死锁。 还是按照之前的例子，时序图如下：
>
> ![时序图](https://user-gold-cdn.xitu.io/2018/3/19/1623d495c8474263?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
>
> 
>
> ### 其他形式的死锁
>
> 我们再来回顾一下死锁的定义，“死锁是指两个或两个以上的进程在执行过程中，由于竞争资源或者由于彼此通信而造成的一种阻塞的现象，若无外力作用，它们都将无法推进下去。” 死锁条件里面的竞争资源，可以是线程池里的线程、网络连接池的连接，数据库中数据引擎提供的锁，等等一切可以被称作竞争资源的东西。
>
> #### 1、线程池死锁
>
> 用个例子来看看这个死锁的特征：
>
> ```
> final ExecutorService executorService = 
>         Executors.newSingleThreadExecutor();
> Future<Long> f1 = executorService.submit(new Callable<Long>() {
> 
>     public Long call() throws Exception {
>         System.out.println("start f1");
>         Thread.sleep(1000);//延时
>         Future<Long> f2 = 
>            executorService.submit(new Callable<Long>() {
> 
>             public Long call() throws Exception {
>                 System.out.println("start f2");
>                 return -1L;
>             }
>         });
>         System.out.println("result" + f2.get());
>         System.out.println("end f1");
>         return -1L;
>     }
> });
> 复制代码
> ```
>
> 在这个例子中，线程池的任务1依赖任务2的执行结果，但是线程池是单线程的，也就是说任务1不执行完，任务2永远得不到执行，那么因此造成了死锁。原因图解如下：
>
> ![线程池死锁](https://user-gold-cdn.xitu.io/2018/3/19/1623d495d05cfc0b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
>
> 
>
> 执行jstack命令，可以看到如下内容：
>
> ```
> "pool-1-thread-1" prio=5 tid=0x00007ff4c10bf800 nid=0x3b03 waiting on condition [0x000000011628c000]
>    java.lang.Thread.State: WAITING (parking)
> 	at sun.misc.Unsafe.park(Native Method)
> 	- parking to wait for  <0x00000007ea51cf40> (a java.util.concurrent.FutureTask$Sync)
> 	at java.util.concurrent.locks.LockSupport.park(LockSupport.java:186)
> 	at java.util.concurrent.locks.AbstractQueuedSynchronizer.parkAndCheckInterrupt(AbstractQueuedSynchronizer.java:834)
> 	at java.util.concurrent.locks.AbstractQueuedSynchronizer.doAcquireSharedInterruptibly(AbstractQueuedSynchronizer.java:994)
> 	at java.util.concurrent.locks.AbstractQueuedSynchronizer.acquireSharedInterruptibly(AbstractQueuedSynchronizer.java:1303)
> 	at java.util.concurrent.FutureTask$Sync.innerGet(FutureTask.java:248)
> 	at java.util.concurrent.FutureTask.get(FutureTask.java:111)
> 	at com.test.TestMain$1.call(TestMain.java:49)
> 	at com.test.TestMain$1.call(TestMain.java:37)
> 	at java.util.concurrent.FutureTask$Sync.innerRun(FutureTask.java:334)
> 	at java.util.concurrent.FutureTask.run(FutureTask.java:166)
> 	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
> 	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
> 	at java.lang.Thread.run(Thread.java:722)
> 复制代码
> ```
>
> 可以看到当前线程wait在java.util.concurrent.FutureTask对象上。
>
> 解决办法：扩大线程池线程数 or 任务结果之间不再互相依赖。
>
> #### 2、网络连接池死锁
>
> 同样的，在网络连接池也会发生死锁，假设此时有两个线程A和B，两个数据库连接池N1和N2，连接池大小都只有1，如果线程A按照先N1后N2的顺序获得网络连接，而线程B按照先N2后N1的顺序获得网络连接，并且两个线程在完成执行之前都不释放自己已经持有的链接，因此也造成了死锁。
>
> ```
> // 连接1
> final MultiThreadedHttpConnectionManager connectionManager1 = new MultiThreadedHttpConnectionManager();
> final HttpClient httpClient1 = new HttpClient(connectionManager1);
> httpClient1.getHttpConnectionManager().getParams().setMaxTotalConnections(1);  //设置整个连接池最大连接数
> 
> // 连接2
> final MultiThreadedHttpConnectionManager connectionManager2 = new MultiThreadedHttpConnectionManager();
> final HttpClient httpClient2 = new HttpClient(connectionManager2);
> httpClient2.getHttpConnectionManager().getParams().setMaxTotalConnections(1);  //设置整个连接池最大连接数
> 
> ExecutorService executorService = Executors.newFixedThreadPool(2);
> executorService.submit(new Runnable() {
>     public void run() {
>         try {
>             PostMethod httpost = new PostMethod("http://www.baidu.com");
>             System.out.println(">>>> Thread A execute 1 >>>>");
>             httpClient1.executeMethod(httpost);
>             Thread.sleep(5000l);
> 
>             System.out.println(">>>> Thread A execute 2 >>>>");
>             httpClient2.executeMethod(httpost);
>             System.out.println(">>>> End Thread A>>>>");
>         } catch (Exception e) {
>             // ignore
>         }
>     }
> });
> 
> executorService.submit(new Runnable() {
>     public void run() {
>         try {
>             PostMethod httpost = new PostMethod("http://www.baidu.com");
>             System.out.println(">>>> Thread B execute 2 >>>>");
>             httpClient2.executeMethod(httpost);
>             Thread.sleep(5000l);
> 
>             System.out.println(">>>> Thread B execute 1 >>>>");
>             httpClient1.executeMethod(httpost);
>             System.out.println(">>>> End Thread B>>>>");
> 
>         } catch (Exception e) {
>             // ignore
>         }
>     }
> });
> 复制代码
> ```
>
> 整个过程图解如下：
>
> ![连接池死锁](https://user-gold-cdn.xitu.io/2018/3/19/1623d495d24acafa?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
>
> 
>
> 在死锁产生后，我们用jstack工具查看一下当前线程堆栈信息，可以看到如下内容：
>
> ```
> "pool-1-thread-2" prio=5 tid=0x00007faa7909e800 nid=0x3b03 in Object.wait() [0x0000000111e5d000]
>    java.lang.Thread.State: WAITING (on object monitor)
> 	at java.lang.Object.wait(Native Method)
> 	- waiting on <0x00000007ea73f498> (a org.apache.commons.httpclient.MultiThreadedHttpConnectionManager$ConnectionPool)
> 	at org.apache.commons.httpclient.MultiThreadedHttpConnectionManager.doGetConnection(MultiThreadedHttpConnectionManager.java:518)
> 	- locked <0x00000007ea73f498> (a org.apache.commons.httpclient.MultiThreadedHttpConnectionManager$ConnectionPool)
> 	at org.apache.commons.httpclient.MultiThreadedHttpConnectionManager.getConnectionWithTimeout(MultiThreadedHttpConnectionManager.java:416)
> 	at org.apache.commons.httpclient.HttpMethodDirector.executeMethod(HttpMethodDirector.java:153)
> 	at org.apache.commons.httpclient.HttpClient.executeMethod(HttpClient.java:397)
> 	at org.apache.commons.httpclient.HttpClient.executeMethod(HttpClient.java:323)
> 	at com.test.TestMain$2.run(TestMain.java:79)
> 	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
> 	at java.util.concurrent.FutureTask$Sync.innerRun(FutureTask.java:334)
> 	at java.util.concurrent.FutureTask.run(FutureTask.java:166)
> 	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
> 	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
> 	at java.lang.Thread.run(Thread.java:722)
> 
> "pool-1-thread-1" prio=5 tid=0x00007faa7a039800 nid=0x3a03 in Object.wait() [0x0000000111d5a000]
>    java.lang.Thread.State: WAITING (on object monitor)
> 	at java.lang.Object.wait(Native Method)
> 	- waiting on <0x00000007ea73e0d0> (a org.apache.commons.httpclient.MultiThreadedHttpConnectionManager$ConnectionPool)
> 	at org.apache.commons.httpclient.MultiThreadedHttpConnectionManager.doGetConnection(MultiThreadedHttpConnectionManager.java:518)
> 	- locked <0x00000007ea73e0d0> (a org.apache.commons.httpclient.MultiThreadedHttpConnectionManager$ConnectionPool)
> 	at org.apache.commons.httpclient.MultiThreadedHttpConnectionManager.getConnectionWithTimeout(MultiThreadedHttpConnectionManager.java:416)
> 	at org.apache.commons.httpclient.HttpMethodDirector.executeMethod(HttpMethodDirector.java:153)
> 	at org.apache.commons.httpclient.HttpClient.executeMethod(HttpClient.java:397)
> 	at org.apache.commons.httpclient.HttpClient.executeMethod(HttpClient.java:323)
> 	at com.test.TestMain$1.run(TestMain.java:61)
> 	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
> 	at java.util.concurrent.FutureTask$Sync.innerRun(FutureTask.java:334)
> 	at java.util.concurrent.FutureTask.run(FutureTask.java:166)
> 	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
> 	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
> 	at java.lang.Thread.run(Thread.java:722)
> 复制代码
> ```
>
> 当然，我们在这里只是一些极端情况的假定，假如线程在使用完连接池之后很快就归还，在归还连接数后才占用下一个连接池，那么死锁也就不会发生。
>
> ### 总结
>
> 在我的理解当中，死锁就是“两个任务以不合理的顺序互相争夺资源”造成，因此为了规避死锁，应用程序需要妥善处理资源获取的顺序。 另外有些时候，死锁并不会马上在应用程序中体现出来，在通常情况下，都是应用在生产环境运行了一段时间后，才开始慢慢显现出来，在实际测试过程中，由于死锁的隐蔽性，很难在测试过程中及时发现死锁的存在，而且在生产环境中，应用出现了死锁，往往都是在应用状况最糟糕的时候——在高负载情况下。因此，开发者在开发过程中要谨慎分析每个系统资源的使用情况，合理规避死锁，另外一旦出现了死锁，也可以尝试使用本文中提到的一些工具，仔细分析，总是能找到问题所在的。
>
> 以上就是本次写作全部内容了，如果你喜欢，欢迎关注我的公众号~ 这是给我不断写作的最大鼓励，谢谢~
>
>
> 作者：江溢Jonny
> 链接：https://juejin.cn/post/6844903577660424206
> 来源：掘金
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

什么是重入锁、 sychronized 和 retrentlock实现区别、锁方法、锁class
算法题： 合并区间 快排
数据库 os
timewait close wait
好吧，二面算法写个快排， 居然死循环了，调了10分钟没调出来， 真心感觉凉了，但是没想到居然给了三面， 我真是佛了。。

## 头条三面 ：
唯一一个技术问题 ： 什么是线程安全。

> 线程安全是多线程领域的问题，线程安全可以简单理解为一个方法或者一个实例可以在多线程环境中使用而不会出现问题。
>
> 产生线程不安全的原因
> 在同一程序中运行多个线程本身不会导致问题，问题在于多个线程访问了相同的资源。如，同一内存区（变量，数组，或对象）、系统（数据库，web services等）或文件。实际上，这些问题只有在一或多个线程向这些资源做了写操作时才有可能发生，只要资源没有发生变化,多个线程读取相同的资源就是安全的。
>
> 多线程同时执行下面的代码可能会出错：
>
> public class Counter {
>     protected long count = 0;
>
>     public void add(long value){
>         this.count = this.count + value;  
>     }
> }
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 想象下线程A和B同时执行同一个Counter对象的add()方法，我们无法知道操作系统何时会在两个线程之间切换。JVM并不是将这段代码视为单条指令来执行的，而是按照下面的顺序：
>
> 从内存获取 this.count 的值放到寄存器
> 将寄存器中的值增加value
> 将寄存器中的值写回内存
> 1
> 2
> 3
> 观察线程A和B交错执行会发生什么：
>
>    this.count = 0;
>    A:   读取 this.count 到一个寄存器 (0)
>    B:   读取 this.count 到一个寄存器 (0)
>    B:   将寄存器的值加2
>    B:   回写寄存器值(2)到内存. this.count 现在等于 2
>    A:   将寄存器的值加3
>    A:   回写寄存器值(3)到内存. this.count 现在等于 3
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 两个线程分别加了2和3到count变量上，两个线程执行结束后count变量的值应该等于5。然而由于两个线程是交叉执行的，两个线程从内存中读出的初始值都是0。然后各自加了2和3，并分别写回内存。最终的值并不是期望的5，而是最后写回内存的那个线程的值，上面例子中最后写回内存的是线程A，但实际中也可能是线程B。如果没有采用合适的同步机制，线程间的交叉执行情况就无法预料。
>
> 竞态条件 & 临界区
> 当两个线程竞争同一资源时，如果对资源的访问顺序敏感，就称存在竞态条件。导致竞态条件发生的代码区称作临界区。上例中add()方法就是一个临界区,它会产生竞态条件。在临界区中使用适当的同步就可以避免竞态条件。
>
> 共享资源
> 允许被多个线程同时执行的代码称作线程安全的代码。线程安全的代码不包含竞态条件。当多个线程同时更新共享资源时会引发竞态条件。因此，了解Java线程执行时共享了什么资源很重要。
>
> 局部变量
> 局部变量存储在线程自己的栈中。也就是说，局部变量永远也不会被多个线程共享。所以，基础类型的局部变量是线程安全的。下面是基础类型的局部变量的一个例子：
>
> public void someMethod(){
>   long threadSafeInt = 0;
>   threadSafeInt++;
> }
> 1
> 2
> 3
> 4
> 局部的对象引用
> 上面提到的局部变量是一个基本类型，如果局部变量是一个对象类型呢？对象的局部引用和基础类型的局部变量不太一样。尽管引用本身没有被共享，但引用所指的对象并没有存储在线程的栈内，所有的对象都存在共享堆中，所以对于局部对象的引用，有可能是线程安全的，也有可能是线程不安全的。
>
> 那么怎样才是线程安全的呢？如果在某个方法中创建的对象不会被其他方法或全局变量获得，或者说方法中创建的对象没有逃出此方法的范围，那么它就是线程安全的。实际上，哪怕将这个对象作为参数传给其它方法，只要别的线程获取不到这个对象，那它仍是线程安全的。下面是一个线程安全的局部引用样例：
>
> public void someMethod(){
>   LocalObject localObject = new LocalObject();
>   localObject.callMethod();
>   method2(localObject);
> }
>
> public void method2(LocalObject localObject){
>   localObject.setValue("value");
> }
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9
> 上面样例中LocalObject对象没有被方法返回，也没有被传递给someMethod()方法外的对象，始终在someMethod()方法内部。每个执行someMethod()的线程都会创建自己的LocalObject对象，并赋值给localObject引用。因此，这里的LocalObject是线程安全的。事实上，整个someMethod()都是线程安全的。即使将LocalObject作为参数传给同一个类的其它方法或其它类的方法时，它仍然是线程安全的。当然，如果LocalObject通过某些方法被传给了别的线程，那它就不再是线程安全的了。
>
> 对象成员对象成员存储在堆上。如果两个线程同时更新同一个对象的同一个成员，那这个代码就不是线程安全的。下面是一个样例：
>
> public class NotThreadSafe{
>     StringBuilder builder = new StringBuilder();
>
>     public add(String text){
>         this.builder.append(text);
>     }  
> }
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 如果两个线程同时调用同一个NotThreadSafe实例上的add()方法，就会有竞态条件问题。例如：
>
> NotThreadSafe sharedInstance = new NotThreadSafe();
>
> new Thread(new MyRunnable(sharedInstance)).start();
> new Thread(new MyRunnable(sharedInstance)).start();
>
> public class MyRunnable implements Runnable{
>   NotThreadSafe instance = null;
>
>   public MyRunnable(NotThreadSafe instance){
>     this.instance = instance;
>   }
>
>   public void run(){
>     this.instance.add("some text");
>   }
> }
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9
> 10
> 11
> 12
> 13
> 14
> 15
> 16
> 注意两个MyRunnable共享了同一个NotThreadSafe对象。因此，当它们调用add()方法时会造成竞态条件。
>
> 当然，如果这两个线程在不同的NotThreadSafe实例上调用call()方法，就不会导致竞态条件。下面是稍微修改后的例子：
>
> new Thread(new MyRunnable(new NotThreadSafe())).start();
> new Thread(new MyRunnable(new NotThreadSafe())).start();
> 1
> 2
> 现在两个线程都有自己单独的NotThreadSafe对象，访问的不是同一资源，不满足竞态条件，是线程安全的。所以非线程安全的对象仍可以通过某种方式来消除竞态条件。
>
> 判断资源对象是否是线程安全
> 线程控制逃逸规则可以帮助你判断代码中对某些资源的访问是否是线程安全的。
>
> 如果一个资源的创建，使用，销毁都在同一个线程内完成，且永远不会脱离该线程的控制，则该资源的使用就是线程安全的。
> 1
> 资源可以是对象，数组，文件，数据库连接，套接字等等。Java中我们无需主动销毁对象，所以“销毁”指不再有引用指向对象。
>
> 注意即使对象本身线程安全，但如果该对象中包含其他资源（文件，数据库连接），整个应用也许就不再是线程安全的了。比如2个线程都创建了各自的数据库连接，每个连接自身是线程安全的，但它们所连接到的同一个数据库也许不是线程安全的。比如，2个线程执行如下代码：
>
> 检查记录X是否存在，如果不存在，插入X
> 1
> 如果两个线程同时执行，而且碰巧检查的是同一个记录，那么两个线程最终可能都插入了记录：
>
> 线程1检查记录X是否存在。检查结果：不存在
> 线程2检查记录X是否存在。检查结果：不存在
> 线程1插入记录X
> 线程2插入记录X
> 1
> 2
> 3
> 4
> 同样的问题也会发生在文件或其他共享资源上。因此，区分某个线程控制的对象是资源本身，还是仅仅到某个资源的引用很重要。
>
> 不可变的共享资源
>
> 当多个 线程同时访问同一个资源，并且其中的一个或者多个线程对这个资源进行了写操作，才会产生竞态条件。多个线程同时读同一个资源不会产生竞态条件。
>
> 我们可以通过创建不可变的共享对象来保证对象在线程间共享时不会被修改，从而实现线程安全。如下示例：
>
> public class ImmutableValue{
>     private int value = 0;
>
>     public ImmutableValue(int value){
>         this.value = value;
>     }
>     
>     public int getValue(){
>         return this.value;
>     }
> }
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9
> 10
> 11
> 如果你需要对ImmutableValue类的实例进行操作，如添加一个类似于加法的操作，我们不能对这个实例直接进行操作，只能创建一个新的实例来实现，下面是一个对value变量进行加法操作的示例：
>
> public class ImmutableValue{
>     private int value = 0;
>
>     public ImmutableValue(int value){
>         this.value = value;
>     }
>     
>     public int getValue(){
>         return this.value;
>     }
>     
>     public ImmutableValue add(int valueToAdd){
>         return new ImmutableValue(this.value + valueToAdd);
>     }
> }
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9
> 10
> 11
> 12
> 13
> 14
> 15
> 请注意add()方法以加法操作的结果作为一个新的ImmutableValue类实例返回，而不是直接对它自己的value变量进行操作。
>
> 引用不是线程安全的！
> 重要的是要记住，即使一个对象是线程安全的不可变对象，指向这个对象的引用也可能不是线程安全的。看这个例子：
>
> public void Calculator{
>     private ImmutableValue currentValue = null;
>
>     public ImmutableValue getValue(){
>         return currentValue;
>     }
>     
>     public void setValue(ImmutableValue newValue){
>         this.currentValue = newValue;
>     }
>     
>     public void add(int newValue){
>         this.currentValue = this.currentValue.add(newValue);
>     }
> }
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9
> 10
> 11
> 12
> 13
> 14
> 15
> Calculator类持有一个指向ImmutableValue实例的引用。注意，通过setValue()方法和add()方法可能会改变这个引用，因此，即使Calculator类内部使用了一个不可变对象，但Calculator类本身还是可变的，多个线程访问Calculator实例时仍可通过setValue()和add()方法改变它的状态，因此Calculator类不是线程安全的。
>
> 换句话说：ImmutableValue类是线程安全的，但使用它的类则不一定是。当尝试通过不可变性去获得线程安全时，这点是需要牢记的。
>
> 要使Calculator类实现线程安全，将getValue()、setValue()和add()方法都声明为同步方法即可。
>
> Java中实现线程安全的方法
> 在Java多线程编程当中，提供了多种实现Java线程安全的方式：
>
> 最简单的方式，使用Synchronization关键字:Java Synchronization介绍
> 使用java.util.concurrent.atomic 包中的原子类，例如 AtomicInteger
> 使用java.util.concurrent.locks 包中的锁
> 使用线程安全的集合ConcurrentHashMap
> 使用volatile关键字，保证变量可见性（直接从内存读，而不是从线程cache读）
> ————————————————
> 版权声明：本文为CSDN博主「Heaven-Wang」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/suifeng3051/article/details/52164267

代码：写 生产者-消费者 模型

> https://www.jianshu.com/p/7cbb6b0bbabc

三面一共聊了15分钟，写了15分钟，结束。

三天后收到意向书。

字节跳动真心奇怪， 打扰了，这都能过。
————————————————
版权声明：本文为CSDN博主「码农成神之路」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/lyl5454/article/details/109118575

## 百度一面：
锁的实现。悲观锁、乐观锁。

> # 1、为什么要用锁？
>
> 锁-是为了解决并发操作引起的脏读、数据不一致的问题。
>
> # 2、锁实现的基本原理
>
> ## 2.1、volatile
>
> > Java编程语言允许线程访问共享变量， 为了确保共享变量能被准确和一致地更新，线程应该确保通过排他锁单独获得这个变量。Java语言提供了volatile，在某些情况下比锁要更加方便。
> >
> > volatile在多处理器开发中保证了共享变量的“ 可见性”。可见性的意思是当一个线程修改一个共享变量时，另外一个线程能读到这个修改的值。
>
> ![img](https:////upload-images.jianshu.io/upload_images/5401760-9f7389128a85f7c1.png?imageMogr2/auto-orient/strip|imageView2/2/w/765/format/webp)
>
> image.png
>
> 结论：如果volatile变量修饰符使用恰当的话，它比synchronized的使用和执行成本更低，因为它不会引起线程上下文的切换和调度。
>
> ## 2.2、synchronized
>
> > synchronized通过锁机制实现同步。
>
> 先来看下利用synchronized实现同步的基础：Java中的每一个对象都可以作为锁。
>
> 具体表现为以下3种形式。
>
> - 对于普通同步方法，锁是当前实例对象。
> - 对于静态同步方法，锁是当前类的Class对象。
> - 对于同步方法块，锁是Synchonized括号里配置的对象。
>
> 当一个线程试图访问同步代码块时，它首先必须得到锁，退出或抛出异常时必须释放锁。
>
> ### 2.2.1 synchronized实现原理
>
> > synchronized是基于Monitor来实现同步的。
>
> Monitor从两个方面来支持线程之间的同步：
>
> - 互斥执行
> - 协作
>
> 1、Java 使用对象锁 ( 使用 synchronized 获得对象锁 ) 保证工作在共享的数据集上的线程互斥执行。
>
> 2、使用 notify/notifyAll/wait 方法来协同不同线程之间的工作。
>
> 3、Class和Object都关联了一个Monitor。
>
> ![img](https:////upload-images.jianshu.io/upload_images/5401760-de9a8db9928ca68a.png?imageMogr2/auto-orient/strip|imageView2/2/w/500/format/webp)
>
> Monitor 的工作机理
>
> - 线程进入同步方法中。
> - 为了继续执行临界区代码，线程必须获取 Monitor 锁。如果获取锁成功，将成为该监视者对象的拥有者。任一时刻内，监视者对象只属于一个活动线程（The Owner）
> - 拥有监视者对象的线程可以调用 wait() 进入等待集合（Wait Set），同时释放监视锁，进入等待状态。
> - 其他线程调用 notify() / notifyAll() 接口唤醒等待集合中的线程，这些等待的线程需要**重新获取监视锁后**才能执行 wait() 之后的代码。
> - 同步方法执行完毕了，线程退出临界区，并释放监视锁。
>
> 参考文档：[https://www.ibm.com/developerworks/cn/java/j-lo-synchronized](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.ibm.com%2Fdeveloperworks%2Fcn%2Fjava%2Fj-lo-synchronized%2F)
>
> ### 2.2.2 synchronized具体实现
>
> 1、同步代码块采用monitorenter、monitorexit指令显式的实现。
>
> 2、同步方法则使用ACC_SYNCHRONIZED标记符隐式的实现。
>
> 通过实例来看看具体实现：
>
> 
>
> ```csharp
> public class SynchronizedTest {
>  
>     public synchronized void method1(){
>         System.out.println("Hello World!");
>     }
>  
>     public  void method2(){
>         synchronized (this){
>             System.out.println("Hello World!");
>         }
>     }
> }
> ```
>
> javap编译后的字节码如下：
>
> ![img](https:////upload-images.jianshu.io/upload_images/5401760-0d29b096e9c77f09.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
> image.png
>
> **monitorenter**
>
> 每一个对象都有一个monitor，一个monitor只能被一个线程拥有。当一个线程执行到monitorenter指令时会尝试获取相应对象的monitor，获取规则如下：
>
> - 如果monitor的进入数为0，则该线程可以进入monitor，并将monitor进入数设置为1，该线程即为monitor的拥有者。
> - 如果当前线程已经拥有该monitor，只是重新进入，则进入monitor的进入数加1，所以synchronized关键字实现的锁是可重入的锁。
> - 如果monitor已被其他线程拥有，则当前线程进入阻塞状态，直到monitor的进入数为0，再重新尝试获取monitor。
>
> **monitorexit**
>
> 只有拥有相应对象的monitor的线程才能执行monitorexit指令。每执行一次该指令monitor进入数减1，当进入数为0时当前线程释放monitor，此时其他阻塞的线程将可以尝试获取该monitor。
>
> ### 2.2.3 锁存放的位置
>
> 锁标记存放在Java对象头的Mark Word中。
>
> ![img](https:////upload-images.jianshu.io/upload_images/5401760-f794a9da707c8884.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
> Java对象头长度
>
> ![img](https:////upload-images.jianshu.io/upload_images/5401760-423237ba213114c9.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
> 32位JVM Mark Word 结构
>
> ![img](https:////upload-images.jianshu.io/upload_images/5401760-d88cbe17d78b4ef4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
> 32位JVM Mark Word 状态变化
>
> ![img](https:////upload-images.jianshu.io/upload_images/5401760-dd289041866d7cb6.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
> 64位JVM Mark Word 结构
>
> ### 2.2.3 synchronized的锁优化
>
> JavaSE1.6为了减少获得锁和释放锁带来的性能消耗，引入了“偏向锁”和“轻量级锁”。
>
> 在JavaSE1.6中，锁一共有4种状态，级别从低到高依次是：无锁状态、偏向锁状态、轻量级锁状态和重量级锁状态，这几个状态会随着竞争情况逐渐升级。
>
> 锁可以升级但不能降级，意味着偏向锁升级成轻量级锁后不能降级成偏向锁。这种锁升级却不能降级的策略，目的是为了提高获得锁和释放锁的效率。
>
> #### 偏向锁：
>
> > 无锁竞争的情况下为了减少锁竞争的资源开销，引入偏向锁。
>
> ![img](https:////upload-images.jianshu.io/upload_images/5401760-c1f25c4a5f0001af.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
> image.png
>
> #### 轻量级锁：
>
> > 轻量级锁所适应的场景是线程交替执行同步块的情况。
>
> ![img](https:////upload-images.jianshu.io/upload_images/5401760-4f4487faff288712.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
> image.png
>
> **锁粗化（Lock Coarsening）：**也就是减少不必要的紧连在一起的unlock，lock操作，将多个连续的锁扩展成一个范围更大的锁。
>
> **锁消除（Lock Elimination）：**锁削除是指虚拟机即时编译器在运行时，对一些代码上要求同步，但是被检测到不可能存在共享数据竞争的锁进行削除。
>
> **适应性自旋（Adaptive Spinning）：**自适应意味着自旋的时间不再固定了，而是由前一次在同一个锁上的自旋时间及锁的拥有者的状态来决定。如果在同一个锁对象上，自旋等待刚刚成功获得过锁，并且持有锁的线程正在运行中，那么虚拟机就会认为这次自旋也很有可能再次成功，进而它将允许自旋等待持续相对更长的时间，比如100个循环。另一方面，如果对于某个锁，自旋很少成功获得过，那在以后要获取这个锁时将可能省略掉自旋过程，以避免浪费处理器资源。
>
> ### 2.2.4 锁的优缺点对比
>
> ![img](https:////upload-images.jianshu.io/upload_images/5401760-dc1cb474286e917c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
> image.png
>
> ## 2.3、CAS
>
> > CAS，在Java并发应用中通常指CompareAndSwap或CompareAndSet，即比较并交换。
>
> 1、CAS是一个原子操作，它比较一个内存位置的值并且只有相等时修改这个内存位置的值为新的值，保证了新的值总是基于最新的信息计算的，如果有其他线程在这期间修改了这个值则CAS失败。CAS返回是否成功或者内存位置原来的值用于判断是否CAS成功。
>
> 2、JVM中的CAS操作是利用了处理器提供的CMPXCHG指令实现的。
>
> 优点：
>
> - 竞争不大的时候系统开销小。
>
> 缺点：
>
> - 循环时间长开销大。
> - ABA问题。
> - 只能保证一个共享变量的原子操作。
>
> # 3、Java中的锁实现
>
> ## 3.1、队列同步器（AQS）
>
> > 队列同步器AbstractQueuedSynchronizer（以下简称同步器），是用来构建锁或者其他同步组件的基础框架。
>
> ### 3.1.1、它使用了一个int成员变量表示同步状态。
>
> ![img](https:////upload-images.jianshu.io/upload_images/5401760-58ec8eff9511a3e4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
> image.png
>
> ### 3.1.2、通过内置的FIFO双向队列来完成获取锁线程的排队工作。
>
> - 同步器包含两个节点类型的应用，一个指向头节点，一个指向尾节点，未获取到锁的线程会创建节点线程安全（compareAndSetTail）的加入队列尾部。同步队列遵循FIFO，首节点是获取同步状态成功的节点。
>
>   ![img](https:////upload-images.jianshu.io/upload_images/5401760-30e2658e38e1966c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
>   image.png
>
> - 未获取到锁的线程将创建一个节点，设置到尾节点。如下图所示：
>
> ![img](https:////upload-images.jianshu.io/upload_images/5401760-addd5edd9723c8db.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
> image.png
>
> - 首节点的线程在释放锁时，将会唤醒后继节点。而后继节点将会在获取锁成功时将自己设置为首节点。如下图所示：
>
>   ![img](https:////upload-images.jianshu.io/upload_images/5401760-d118af99f2bacad5.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
>   image.png
>
> ### 3.1.3、独占式/共享式锁获取
>
> > 独占式：有且只有一个线程能获取到锁，如：ReentrantLock。</pre>
> >
> > 共享式：可以多个线程同时获取到锁，如：CountDownLatch
>
> #### 独占式
>
> - 每个节点自旋观察自己的前一节点是不是Header节点，如果是，就去尝试获取锁。
>
>   ![img](https:////upload-images.jianshu.io/upload_images/5401760-943a473d1d87cc7d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
>   image.png
>
> - 独占式锁获取流程：
>
> ![img](https:////upload-images.jianshu.io/upload_images/5401760-50bc00c23df33d60.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
> image.png
>
> #### 共享式：
>
> - 共享式与独占式的区别：,
>
>   ![img](https:////upload-images.jianshu.io/upload_images/5401760-d0031b43b65487d7.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
>   image.png
>
> - 共享锁获取流程：
>
> ![img](https:////upload-images.jianshu.io/upload_images/5401760-8f66fdebba19eff8.png?imageMogr2/auto-orient/strip|imageView2/2/w/759/format/webp)
>
> image.png
>
> # 4、锁的使用用例
>
> ## 4.1、ConcurrentHashMap的实现原理及使用
>
> ![img](https:////upload-images.jianshu.io/upload_images/5401760-460898019c2b5b0a.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
> ConcurrentHashMap类图
>
> ![img](https:////upload-images.jianshu.io/upload_images/5401760-f2f0bb8727e6d4b2.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
> ConcurrentHashMap数据结构
>
> 结论：ConcurrentHashMap使用的锁分段技术。首先将数据分成一段一段地存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据的时候，其他段的数据也能被其他线程访问。
>
> 
>
> 作者：高广超
> 链接：https://www.jianshu.com/p/e674ee68fd3f
> 来源：简书
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
>
> 乐观锁与悲观锁是一种广义上的概念，体现了看待线程同步的不同角度。在Java和数据库中都有此概念对应的实际应用。
>
> **先说概念。对于同一个数据的并发操作，悲观锁认为自己在使用数据的时候一定有别的线程来修改数据，因此在获取数据的时候会先加锁，确保数据不会被别的线程修改。Java中，synchronized关键字和Lock的实现类都是悲观锁。**
>
> **而乐观锁认为自己在使用数据时不会有别的线程修改数据，所以不会添加锁，只是在更新数据的时候去判断之前有没有别的线程更新了这个数据。如果这个数据没有被更新，当前线程将自己修改的数据成功写入。如果数据已经被其他线程更新，则根据不同的实现方式执行不同的操作（例如报错或者自动重试）。**
>
> 乐观锁在Java中是通过使用无锁编程来实现，最常采用的是CAS算法，Java原子类中的递增操作就通过CAS自旋实现的。
>
> 根据从上面的概念描述我们可以发现：
>
> - 悲观锁适合写操作多的场景，先加锁可以保证写操作时数据正确。
> - 乐观锁适合读操作多的场景，不加锁的特点能够使其读操作的性能大幅提升。

sychronized 和 reentrantlock 实现原理
volatile原理
java 设计模式， jdk里用到了哪些设计模式。
NIO 讲一讲。

> ## NIO含义
>
> New I/O，原因在于它相对于之前的I/O类库是新增的。
> 由于之前老的I/O类库是阻塞I/O，New I/O类库的目标就是要让Java支持非阻塞I/O，所以，更多的人喜欢称之为非阻塞I/O（Non-block I/O）。
>
> ## SocketChannel和ServerSocketChannel
>
> 与Socket类和ServerSocket类相对应，NIO也提供了SocketChannel和ServerSocketChannel两种不同的套接字通道实现。这两种新增的通道都支持阻塞和非阻塞两种模式。
> 低负载、低并发的应用程序可以选择同步阻塞I/O以降低编程复杂度；对于高负载、高并发的网络应用，需要使用NIO的非阻塞模式进行开发。
>
> ## NIO类库简介
>
> NIO弥补了原来同步阻塞I/O的不足，它在标准Java代码中提供了高速的、面向块的I/O。
>
> 通过定义包含数据的类，以及通过以块的形式处理这些数据，NIO不用使用本机代码就可以利用低级优化，这是原来的I/O包所无法做到的。
>
> ## NIO类库简介 缓冲区Buffer
>
> Buffer是一个对象，它包含一些要写入或者要读出的数据。
> 在NIO类库中加入Buffer对象，体现了新库与原I/O的一个重要区别。
> 在面向流的I/O中，可以将数据直接写入或者将数据直接读到Stream对象中。
> 在NIO库中，所有数据都是用缓冲区处理的。在读取数据时，它是直接读到缓冲区中的；在写入数据时，写入到缓冲区中。任何时候访问NIO中的数据，都是通过缓冲区进行操作。
> 缓冲区实质上是一个数组。
> 缓冲区不仅仅是一个数组，缓冲区提供了对数据的结构化访问以及维护读写位置（limit）等信息。
> 一个ByteBuffer提供了一组功能用于操作byte数组。
> 每一种Java基本类型（除了Boolean类型）都对应有一种缓冲区：
> ByteBuffer：字节缓冲区
> CharBuffer：字符缓冲区
> ShortBuffer：短整型缓冲区
> IntBuffer：整形缓冲区
> LongBuffer：长整形缓冲区
> FloatBuffer：浮点型缓冲区
> DoubleBuffer：双精度浮点型缓冲区
>
> ![img](https:////upload-images.jianshu.io/upload_images/752311-70577147ebc6bcff.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
>
> 每一个Buffer类都是Buffer接口的一个子实例。除了ByteBuffer，每一个Buffer类都有完全一样的操作，只是它们所处理的数据类型不一样。
> 因为大多数标准I/O操作都使用ByteBuffer，所以它在具有一般缓冲区的操作之外还提供了一些特有的操作，以方便网络读写。
>
> ## NIO类库简介 通道Channel
>
> Channel是一个通道，网络数据通过Channel读取和写入。
> 通道与流的不同之处在于通道是双向的，流只是在一个方向上移动（一个流必须是InputStream或者OutputStream的子类），而通道可以用于读、写或者二者同时进行。
> 因为Channel是全双工的，所以它可以比流更好地映射底层操作系统的API。特别是在UNIX网络编程模型中，底层操作系统的通道都是全双工的，同时支持读写操作。
>
> ![img](https:////upload-images.jianshu.io/upload_images/752311-4840c75f1eeaaa0c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1137/format/webp)
>
> 自顶向下看，前三层主要是Channel接口，用于定义它的功能，后面是一些具体的功能类（抽象类）。从类图可以看出，实际上Channel可以分为两大类：用于网络读写的SelectableChannel和用于文件操作的FileChannel。
> ServerSocketChannel和SocketChannel都是SelectableChannel的子类
>
> ## 多路复用器Selector
>
> 多路复用器Selector是Java NIO编程的基础
> 多路复用器提供选择已经就绪的任务的能力。
> Selector会不断地轮询注册在其上的Channel，如果某个Channel上面发生读或者写事件，这个Channnel就处于就绪状态，会被Selector轮询出来，然后通过SelectionKey可以获取就绪Channel的集合，进行后续的I/O操作。
> 一个多路复用器Selector可以同时轮询多个Channel，由于JDK使用了epoll()代替传统的select实现，所以它并没有最大连接句柄1024/2048的限制。这也就意味着只需要一个线程负责Selector的轮询，就可以接入成千上万的客户端，这确实是个非常巨大的进步。
>
> ![img](https:////upload-images.jianshu.io/upload_images/752311-7878d027956f7f83.png?imageMogr2/auto-orient/strip|imageView2/2/w/1186/format/webp)
>
> 
>
> 作者：每天学点编程
> 链接：https://www.jianshu.com/p/5442b04ccff8
> 来源：简书
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
>
> ## 对于Java NIO，其主要由三个组件组成：Channel、Selector和Buffer。关于这三个组件的作用主要如下：
>
> - Channel是客户端连接的一个抽象，当每个客户端连接到服务器时，服务器都会为其生成一个Channel对象；
> - Selector则是Java NIO实现高性能的关键，其本质上使用了IO多路复用的原理，通过一个线程不断的监听多个Channel连接来实现多所有这些Channel事件进行处理，这样的优点在于只需要一个线程就可以处理大量的客户端连接，当有客户端事件到达时，再将其分发出去交由其它线程处理；
> - Buffer从字面上讲是一个缓存，本质上其是一个字节数组，通过Buffer，可以从Channel上读取数据，然后交由下层的处理器进行处理。这里的Buffer的优点在于其封装了一套非常简单的用于读取和写入数据Api。
>
> 关于Channel和Selector的整体结构，可以通过下图进行的理解，这也是IO多路复用的原理图：
>
> 
>
> ![img](https://pic3.zhimg.com/80/v2-c6900ff6bfaedb48a379225d07e998ce_720w.jpg)
>
> 
>
> 可以看到，对于每个Channel对象，其只要注册到Selector上，那么Selector上监听的线程就会监听这个Channel的事件，当任何一个Channel有对应的事件到达时，Selector就会将该事件分发到下层的应用进行处理。
>
> 本文首先会对Channel，Selector和Buffer的主要Api进行讲解，然后会结合一个服务器与客户端的例子来具体讲解它们三者的使用方式。
>
> ## 基础准备
>
> **阻塞/非阻塞区别**
>
> - 阻塞是指调用结果返回之前，当前线程被挂起。
> - 非阻塞是指在不能立刻得到结果之前，该调用不会阻塞当前线程。
> - 阻塞非阻塞着重在于**服务端**程序在等待结果时的状态
>
> **同步/异步区别**
>
> - 同步是指客户端发出请求后，在没有得到想要结果前，一直阻塞
> - 异步是指客户端发出请求后，马上返回但是没有结果。等服务端运行结束后通过回调再通知客户端
> - 同步异步的区别着重在于**客户端**在等待结果时的状态
>
> **概述**
>
> - BIO是同步阻塞，多线程多请求
> - NIO是同步非阻塞，单线程多请求
> - AIO是异步非阻塞
>
> ## BIO
>
> **同步阻塞**
>
> 先不讲概念，show me the code
>
> ```java
> public class Server {
>     static byte[] bs = new byte[1024];
>     public static void main(String[] args) throws IOException {
> 
>         //它只是一个监听机制
>         ServerSocket serverSocket = new ServerSocket(9098);
>         while (true){
>             System.out.println("开始阻塞");
>             //阻塞 放弃CPU
>             //当监听到有人连接就会返回一个新的socket
>             Socket clientSocket = serverSocket.accept();
>             System.out.println("连接成功");
>             //输入流，字面理解从外部输入
>             //阻塞，放弃CPU
>             clientSocket.getInputStream().read(bs);
>             System.out.println("data success");
>             System.out.println(new String(bs));
>         }
>     }
> }
> //让客户端进入阻塞状态
> public class Client {
>     public static void main(String[] args) {
>         try {
>             Socket socket = new Socket("127.0.0.1", 9098);
>             //OUTPUT输出流，字面理解向外输出
>             Scanner scanner = new Scanner(System.in);
>             String next = scanner.next();
>             socket.getOutputStream().write(next.getBytes());
>         }catch (IOException e){
>             e.printStackTrace();
>         }
>     }
> }
> ```
>
> 要向面试官介绍：BIO有两个地方阻塞
>
> - serverSocket.accept(); 在没有新连接时，就会阻塞
> - getInputStream().read(bs);在没有数据传输时，就会阻塞
>
> ## NIO
>
> BIO到NIO的衍变过程，BIO的两个阻塞是它的缺点，因此需要解决阻塞的问题。
>
> ```java
> public class NIOServer {
>     static ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
>     static List<SocketChannel> channelList = new ArrayList<>();
>     public static void main(String[] args) {
>         try{
>             ServerSocketChannel serverSocket = ServerSocketChannel.open();
>             SocketAddress socketAddress = new InetSocketAddress("127.0.0.1",9098);
>             serverSocket.bind(socketAddress);
>             serverSocket.configureBlocking(false);
>             while (true){
>                 for(SocketChannel socketChannel:channelList){
>                     int read = socketChannel.read(byteBuffer);
>                     if(read>0){
>                         byteBuffer.flip();
>                         System.out.println("收到");
>                     }
>                 }
>                 SocketChannel accept = serverSocket.accept();
>                 if(accept!=null){
>                     System.out.println("conn success");
>                     accept.configureBlocking(false);
>                     channelList.add(accept);
>                     System.out.println(channelList.size()+"list --size");
>                 }
>             }
>         } catch (IOException e) {
>             e.printStackTrace();
>         }
>     }
> }
> ```
>
> - 把read和accpet都改成非阻塞，如果没有数据就返回一个标志，有数据就返回数据
> - 使用一个数组存储每个socketChannel，然后通过轮询的方式非阻塞地获取客户端的输入
>
> 
>
> 但是依然存在一些问题：
>
> - while(true)当没有连接时会空转消耗CPU
> - 10000个连接，只有100个连接有数据，for轮询会浪费9900次
>
> 因此JAVA把轮询的工作交给了操作系统内层实现
>
> ```java
> ServerSocketChannel serverChannel = ServerSocketChannel.open();
> serverChannel.configureBlocking(false);
> serverChannel.socket().bind(new InetSocketAddress(port));
> Selector selector = Selector.open();
> serverChannel.register(selector, SelectionKey.OP_ACCEPT);
> while(true){
>     int n = selector.select();
>     if (n == 0) continue;
>     Iterator ite = this.selector.selectedKeys().iterator();
>     while(ite.hasNext()){
>         SelectionKey key = (SelectionKey)ite.next();
>         if (key.isAcceptable()){
>             SocketChannel clntChan = ((ServerSocketChannel) key.channel()).accept();
>             clntChan.configureBlocking(false);
>             //将选择器注册到连接到的客户端信道，
>             //并指定该信道key值的属性为OP_READ，
>             //同时为该信道指定关联的附件
>             clntChan.register(key.selector(), SelectionKey.OP_READ, ByteBuffer.allocate(bufSize));
>         }
>         if (key.isReadable()){
>             handleRead(key);
>         }
>         if (key.isWritable() && key.isValid()){
>             handleWrite(key);
>         }
>         if (key.isConnectable()){
>             System.out.println("isConnectable = true");
>         }
>       ite.remove();
>     }
> }
> ```
>
> 1、创建ServerSocketChannel实例，并绑定指定端口；
> 2、创建Selector实例；
> 3、将serverSocketChannel注册到selector，并指定事件OP_ACCEPT，最底层的socket通过channel和selector建立关联；
> 4、如果没有准备好的socket，select方法会被阻塞一段时间并返回0；
> 5、如果底层有socket已经准备好，selector的select方法会返回socket的个数，而且selectedKeys方法会返回socket对应的事件（connect、accept、read or write）；
> 6、根据事件类型，进行不同的处理逻辑；
> 在步骤3中，selector只注册了serverSocketChannel的OP_ACCEPT事件
> 1、如果有客户端A连接服务，执行select方法时，可以通过serverSocketChannel获取客户端A的socketChannel，并在selector上注册socketChannel的OP_READ事件。
> 2、如果客户端A发送数据，会触发read事件，这样下次轮询调用select方法时，就能通过socketChannel读取数据，同时在selector上注册该socketChannel的OP_WRITE事件，实现服务器往客户端写数据。
> (我感觉这个不需要介绍给面试官听，自己稍微理解一下真正的NIO代码是长什么样就可以了)
>
> 
>
> 
>
> **简单介绍BIO，以及它的缺点**
>
> BIO的“B”嘛，它表示的是blocking的意思，就是阻塞，我们使用serverSocket绑定完端口后 ，我们会监听该端口，等待accept事件，accept会阻塞当前主线程。当我们收到accpet事件时，程序会拿到客户端与服务端连接的socket，针对这个socket我们可以进行读写，但是这个socket的读写会阻塞当前线程。所以我们一般会使用多线程来进行cs交互，但这样在客户端连接数量很多情况下，线程的数量还有线程上下文切换对服务器都会造成压力。
>
> **NIO怎么解决这个缺点**
>
> nio为我们提供了非阻塞接口，我们使用一个线程去检查n个socket。nio包为我们提供了一个selector，然后我们把需要检查的socket注册到这个selector中。主线程阻塞在selector的select方法里面。当选择器发现某个socket就绪了，它就会唤醒主线程，通过selector获取到就绪状态的socket来进行相应的处理。connect、accept、read or write
>
> 
>
> NIOselector调用natice方法是使用操作系统的系统调用使用。
>
> ## 原始IO多路复用实现版本Select
>
> 我们每次调用kernel#select函数，它都会涉及到用户态/内核态的切换。它传递socket集合，即文件描述符fd。linux系统里面一切介文件。首先它会根据fd集合，检查socket套接字状态。这个复杂度是O(N),检查完一遍之后，如果有就绪状态的socket，直接返回，如果没有就进入阻塞。直到某个socket有数据之后，才唤醒线程，再重新遍历检查。
>
> 可以插入第三个问题
>
> 当它检查到就绪状态socket之后，它做了两件事，第一件事会到就绪状态socket的fd描述符里面设置一个标记，第二个件事返回select函数，唤醒java线程有多少个socket就绪。但是具体是哪个socket就绪，java程序目前不知道，所以接下来又是一个O(N)检查。
>
> **select监听fd集合，最大1024 为什么**
>
> 因为select函数的参数有一个fd_set这个结构是一个bimap位图结构，它是一个长的二进制数，默认是1024bit。想修改这个长度，非常麻烦。
>
> 出于性能考虑。因为XXX 这个fdset会从用户态传到内核态。如果bitmap过大，就会造成速度问题。
>
> **假设select函数第一次检查没发现socket就绪，过了一会有一个socket就绪了，它是怎么发现的呢，难道它一直占着CPU去轮询吗？**
>
> 这涉及到系统中断，所谓中断就是让cpu正在执行的进程先保留程序上下文，然后让出cpu，为中断程序让道，中断程序就拿到cpu的执行权限，执行程序，比如键盘的中断程序。
>
> 当数据传过来的时候，它会将数据写到内存里面，这个过程cpu不参与，当数据完成传输之后，它就会触发网络数据传输完成的中断程序，它会把cpu顶掉，执行中断程序。它的逻辑是根据内存中有的数据包，分析出数据包是哪个socket的数据，包里面有那个端口号，根据端口号就可以找到socket实例。导入到socket的读缓冲区，导入完成后，它会去检查socket的等待队列是否有等待者，如果有就把等待者移动到工作队列，然后我们的select函数就回到就绪状态，可以有机会获得cpu时间片。所以我们select再次检查socket。
>
> **poll函数与select函数区别**
>
> 最大区别是传参不一样了，select它使用的是bitmap，它表示需要检查的socket集合。poll使用的是数组结构，表示需要检查的socket集合。主要是为了解决select这个bitmap长度1024的问题。数组没有这个限制，它就可以让我们的线程监听超过1024个socket限制。主要区别是这个，其他没什么大的区别了。
>
> **为什么会出现epoll，它的背景是什么**
>
> select和poll函数的缺陷是：
>
> 1. **函数调用参数拷贝问题：**这两系统函数每次调用都需要我们提供给他所有需要监听的socket文件描述符集合，而且我们主线程是死循环调用select/poll函数，这涉及到用户空间数据到内核空间的拷贝过程，这个操作比较耗费性能。其实我们的fd集合是比较稳定的，可能它每次只有1-2个socket_fd需要更改，但是我们每次都需要传整个。因为select/poll没有在kernel层面保留任何数据信息，所以说每次调用都需要进行数据拷贝。
> 2. **系统调用返回后不知道哪些socket就绪问题：**select和poll函数它的返回值是个int整型，只能代表有几个socket就绪了，导致我们程序被唤醒之后，还需要新一轮的系统调用，去检查哪个socket是就绪状态的
>
> **怎么解决，epoll怎么设计的呢？**
>
> 需要epoll函数在内核空间内，它有一个对应的数据结构去存储一些数据。这个数据结构实际上就是eventpoll对象，eventpoll的结构，主要是三块重要的区域，一块是**检查列表，**存放需要监听的socket_fd监听符，红黑树，因为这个socket集合信息经常会有增删改查的需求，保持了时间复杂度为O(logN)
>
> 另一块就是**就绪列表**，存放就绪状态的socket信息,双向链表。 另一块是等待队列，把调用epollwait函数的进程放到这里面去。
>
> 它主要提供了两个函数，epoll_ctl函数，它负责维护**检查列表**，可以根据eventpoll-id去增加删除改动需要检查的socket文件描述符。 epoll_wait函数它主要参数是eventpoll-id,表示此次系统调用需要监测的socket_fd集合是eventpoll里面的信息。默认情况下它会阻塞调用线程，直到eventpoll中关联的某个socket就绪以后，epoll_wait函数才会返回。
>
> **怎么维护就绪队列**
>
> 靠的是中断程序，对等待队列和就绪队列进行操作
>
> **epoll_wait返回0表示没有就绪socket,大于0表示有几个就绪socket，它怎么解决第二个识别就绪socket问题的呢。**
>
> epollwait函数调用的时候，会传入一个epoll_event事件数组指针，epoll_wait函数正常返回之前会把就绪的socket事件信息拷贝到这个数组指针里面。上层程序就可以通过这个数组拿到就绪的socket列表。所以它的时间复杂度是O(1)
>
> **epoll_wait函数可以设置成非阻塞的 吗?**
>
> 默认是阻塞的，但可以设置阻塞时间，如果设置为0表示非阻塞，每次调用都会去检查就绪列表
>
> **检查队列采用的数据结构是什么？**
>
> 红黑树，因为这个socket集合信息经常会有增删改查的需求
>
> ## 边缘触发和水平触发的区别
>
> - 水平触发:如果文件描述符已经就绪可以非阻塞的执行IO操作了,此时会触发通知.允许在任意时刻重复检测IO的状态.select,poll就属于水平触发.
>
> - - 简单来说只要缓冲区还有内容，就会返回读就绪。
>
> - 边缘触发:如果文件描述符自上次状态改变后有新的IO活动到来,此时才会触发通知.在收到一个IO事件通知后要尽可能多的执行IO操作,因为如果在一次通知中没有执行完IO那么就需要等到下一次新的IO活动到来才能获取到就绪的描述符.
>
> - - 简单来说，只有当缓冲区内容发生变化时，才会返回读就绪
>
> 使用场景
>
> epoll默认是水平触发，优点是保证数据完整性，会一直通知你，缺点是这种通知涉及内核态到用户态的切换，会浪费一定性能。
>
> 边缘触发，优点是每次内核只会通知一次，提高效率，缺点是不能保证数据的完整性，不一定能及时取出所有数据。一般来说如果处理大数据的情况下会使用边缘触发。
>
> 一个管道收到了1kb的数据,epoll会立即返回,此时读了512字节数据,然后再次调用epoll.这时如果是水平触发的,epoll会立即返回,因为有数据准备好了.如果是边缘触发的不会立即返回,因为此时虽然有数据可读但是已经触发了一次通知,在这次通知到现在还没有新的数据到来,直到有新的数据到来epoll才会返回,此时老的数据和新的数据都可以读取到(当然是需要这次你尽可能的多读取).

数据库 两种引擎区别。
热备份。
四次挥手 越详细越好
如果一直都等不到连接会怎么样。
concurrenthashmap 实现原理。
二叉树 转 链表。

## 百度二面：

gc
java longadder
数据库 四种隔离级别
数据库的索引数据结构 ：哈希 、b 树、全文索引。
跳台阶
手撕 LRU

## 百度三面

fanal fanally fanalize 区别、
final修饰类能继承吗、
不用final还可以用什么办法使得这个类不被继承、

> private构造方法

java初始化的顺序 ：

> 对于静态变量、静态初始化块、变量、初始化块、构造器，它们的初始化顺序依次是（静态变量、静态初始化块）>（变量、初始化块）>构造器。

java锁机制、sychronnized 和 lock的区别
自旋锁 是公平吗？
自旋锁 怎么才能公平。
客户抱怨你们网站太慢，怎么排查问题？
tcp 三次四次
————————————————
版权声明：本文为CSDN博主「码农成神之路」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/lyl5454/article/details/109118575

## 一面：

1、一些Java基础知识。

2、倒排索引。

3、讲讲redis里面的哈希表？

4、happen-before的规则？

5、volatile修饰符，synchronize锁。

6、java单例模式的实现？

7、进程与线程的区别，多进程和多线程的区别？

8、HashMap原理，为什么用红黑树，红黑树的特点？

9、快排时间空间复杂度，最好最坏的情况，优化方案？

10、TCP的拥塞控制，具体过程是怎么样的？UDP有拥塞控制吗？如何解决？

11、讲讲了解的垃圾回收算法和回收器，什么时候执行STOP THE WORLD？

12、了解Go语言吗？

13、问项目相关的东西：负责哪个模块？有没有碰到什么问题？怎么解决的？

## 二面：

1、Kylin的项目架构。

2、Paxos和ZAB协议。

3、CAP理论，分区容错性的意义。

4、大表Join小表优化，如何处理数据倾斜？

5、讲一下最大堆和最小堆。

6、HDFS的读取、写入，容错处理。（源码）

7、MapReduce的过程。（第一版和第二版的）

8、MR shuffle，Spark shuffle。

9、namenode HA，脑裂，Yarn的调度机制。

10、Hive的内部表和外部表区别、数仓建模模型、数仓分层、雪花模型和星型模型。

11、了解ClickHouse吗？它与Kylin的区别？

## 三面：

1、LRU算法实现。（伪代码）

2、链表倒数第K个数。（讲思路）

3、一堆螺丝和螺母用最短时间匹配。（代码实现）

4、求每天浏览页面的新用户。（Hive QL实现）

5、求抖音小视频每日点击量最高的10个。（Hash + 最小堆）
————————————————
版权声明：本文为CSDN博主「Java白楠楠」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_47340771/article/details/109190454

## 前言

我大概我是从去年12月份开始看书学习，到今年的6月份，一直学到看大家的面经基本上百分之90以上都会，我就在5月份开始投简历，边面试边补充基础知识等。也是有些辛苦。终于是在前不久拿到了字节跳动的offer，现在我也来写面经，希望能帮助到大家！

## 面经                                                                   

### Java基础

0.HashMap的源码，实现原理，JDK8中对HashMap做了怎样的优化。

拉链结构，数组+链表，原理是hash找数组，冲突后拉链表，1.8优化为会进化成红黑树提高效率，并且使用2^n来做容量值

> 引申点：
>
> - equal & hashcode
> - 其他地方的hash处理，如redis的hash、集群slot等
> - 对hash算法类型的了解（安全哈希和非安全哈希如mermerhash）
> - 对hashMap实现的了解：取hashcode，高位运算，低位取模
> - 一致性hash（处理了什么问题，在什么场景用到）
> - 红黑树简单描述

1.HaspMap扩容是怎样扩容的，为什么都是2的N次幂的大小。

在容量到达抵达负载因子*最大容量的时候进行扩容，负载因子的默认值为0.75

2N的原因：

- hash的计算是通过hashcode高低位混合然后和容量的length进行与运算
- 在length=2n的时候，与运算相当于是一个取模操作
- 那么在每次rehash完毕之后mod2N的意义在于要么该元素是在原位置，要么是在最高位偏移多一位的位置，提高效率

> 引申点：
>
> - ConcurrentHashMap的扩容：1.7分段扩容以及1.8transfer并发协同的扩容
> - redis渐进式hash扩容处理

3.HashMap，HashTable，ConcurrentHashMap的区别。

Map线程不安全（没有用任何同步相关的原语），Table安全（直接加syn），Concurrent提供更高并发度的安全（分段锁思想orSyn+Cas）

> 引申点：
>
> - 对线程安全的定义：如hashmap在1.7前会头插死循环，但是在1.8改善后还是不能叫线程安全，因为没有可见性
> - 对锁粒度的思考：在介于map和table之间存在tradeoff之后的均衡解
> - Syn和ReentranceLock的区别
> - 锁升级

4.极高并发下HashTable和ConcurrentHashMap哪个性能更好，为什么，如何实现的。

分两种情况讨论：

1. 极高并发读：并发读的情况下，Table也为读加了锁，没有并发可言，ConcurrentMap读锁并没有加并发，直接可读，若读resize的某个tab为空则转到新tab去读，Node的元素val和指针next都是volatile修饰的，可以保证可见性，所以concurrentMap获胜
2. 极高并发写：在并发写的情况下，table也是直接加了Syn做锁，强制串行，并且resize也只能单线程扩容，ConcurrentMap首先对于每个数组都有并发度，其次在resize的时候支持多线程协同，所以concurrentMap获胜

所以整体而言concurrentMap优势在于：

1. 读操作基于volatile可见性所以无锁
2. 写操作优势在于一是粗粒度的数组锁，二是协同resize

这个问题的思路是先分类讨论然后描述细节最后在下结论

> 引申点：
>
> - volatile的实现：保证内存可见、禁止指令重排序但无法保证原子性
> - java内存模型
> - JVM做的并行优化、先行发生原则与指令重排序
> - 底层细节的熟悉

5.HashMap在高并发下如果没有处理线程安全会有怎样的安全隐患，具体表现是什么。

1.7前死锁，1.7后线程会获取脏值导致逻辑不可靠

6.java中四种修饰符的限制范围。

public：公用，谁来了都给你用

protected：包内使用，子类也可使用

default：包内使用，子类不可使用

private：自己用

7.Object类中的方法。

waithashcodeequalwaitnotifygetclasstostringnofityallfinalize

> 引申点：
>
> - wait和sleep区别
> - hashcode存在哪儿（对象头里）
> - finalize作用：GC前执行，但是不一定能把这个函数跑完
> - getClass后能获取什么信息：引申到反射

8.接口和抽象类的区别，注意JDK8的接口可以有实现。

接口：可以imp多个接口，1.7之前不允许实现，1.8后可以实现方法

抽象类：只能继承一个类，抽象类中可以存在默认实现方法

接口的语义是继承该接口的类有该类接口的行为

抽象类的语义是继承该抽象类的类本身就是该抽象类

9.动态代理的两种方式，以及区别。

1. CGLIB：其本质是在内存中继承了一个子类，可以代理希望代理的那个类的所有方法
2. JDK动态代理：实现InvocationHandler，通过生成一个Proxy来反射调用所有的接口方法

优劣：

- CGLIB：会在内存中多存额外的class信息，对metaspace区的使用有影响，但是性能好，可以访问非接口的方法
- JDK动态代理：本质是生成一个继承所有接口的Proxy来反射调用方法，局限性在于其只能代理接口的方法

> 引申点：
>
> - Spring的AOP实现以及应用场景
> - 反射的开销：检查方法权限，序列化以及匹配入参
> - ASM

10.Java序列化的方式。

继承Serializable接口并添加SerializableId（idea有组件可以直接生成），ID实际上是一个版本，标志着序列化的结构是否相同

11.传值和传引用的区别，Java是怎么样的，有没有传值引用。

本质上来讲Java传递的是引用的副本，实际上就是值传递，但是这个值是引用的副本，比如方法A中传入了一个引用ref，那么在其中将ref指向其他对象并不影响在方法A外的ref，因为ref在传入方法A的时候实际上是指向同一个对象的另一个引用，可以称之为ref'，ref'若直接修改引用的对象会影响ref，但若ref'指向其他对象则和ref没有关系了

12.一个ArrayList在循环过程中删除，会不会出问题，为什么。

分情况讨论：

1. fori删除，不会直接抛异常，但是会产生异常访问
2. foreach删除（实际就是迭代器），会直接抛出并发修改异常，因为迭代器会进行获取迭代器时的exceptModCount和真实的modCount的对比

> 引申点：
>
> - 迭代器实现
> - ArrayList内部细节

13.@transactional注解在什么情况下会失效，为什么。

方法A存在该注解，同时被方法B调用，外界调用的是Class.B的方法，因为内部实际上的this.a的调用方式没走代理类所以不会被切面切到

## 数据结构和算法

1.B+树

出度为m的一颗树，节点的子女在[M/2,M]之间

叶子节点存储全量信息

非叶子节点只充当索引进行叶子节点的路由（内存友好、局部性友好）

底层的叶子节点以链表的形式进行相连（范围查找友好）

2.快速排序，堆排序，插入排序（其实八大排序算法都应该了解

快排：核心是分治logn

堆排：基于二叉树nlogn

插入：暴力n2

3.一致性Hash算法，一致性Hash算法的应用

一致性hash，将整个hash的输出空间当成一个环，环中设立多个节点，每个节点有值，当对象的映射满足上个节点和这个节点中间值的时候它就落到这个节点当中来

应用：redis缓存，好处是平滑的数据迁移和快速的rebalance

> 引申点：
>
> - 一致性hash热点怎么处理：虚拟节点
> - redis如何实现的：客户端寻址

## JVM

1.JVM的内存结构。

程序计数器：计算读到第几行了，类似一个游标

方法栈：提供JVM方法执行的栈空间

本地方法栈：提供native方法执行的栈空间

堆：存对象用的，young分eden,s0,s1，分配比例大概是8:1:1，Old只有一个区

方法区：1.8后为metaspace，存class信息，常量池（后迁移到堆中），编译出来的热点代码等

> 引申点：
>
> - heap什么时候发生溢出
> - stack什么时候发生溢出
> - 方法区什么时候发生溢出
> - hotspot code的机制
> - 流量黑洞如何产生的

2.JVM方法栈的工作过程，方法栈和本地方法栈有什么区别。

方法栈是JVM方法使用的，本地方法栈是native方法使用的，在hotspot其实是用一个

3.JVM的栈中引用如何和堆中的对象产生关联。

引用保存地址，直接可以查找到堆上对应地址的对象

4.可以了解一下逃逸分析技术。

方法中开出来的local变量如果在方法体外不存在的话则称之为无法逃逸

- 可以直接分配在栈上，随着栈弹出直接销毁，省GC开销
- 消除所有同步代码，因为本质上就是个单线程执行

> 引申点：
>
> JVM编译优化：
>
> - 逃逸分析
>
> - 栈上分配
>
> - 分层编译与预热
>
> - 栈上替换
>
> - 常量传播
>
> - 方法内联
>
>   ...

5.GC的常见算法，CMS以及G1的垃圾回收过程，CMS的各个阶段哪两个是Stop the world的，CMS会不会产生碎片，G1的优势。

常见算法：

1. 标记清楚：存在内存碎片，降低内存使用效率
2. 标记整理：整理可分为复制整理和原地整理，不存在内存碎片，但是需要额外的cpu算力来进行整理，若为复制算法还需要额外的内存空间

CMS流程：

1. 初始标记(stw)：获得老年代中跟GCRoot以及新生代关联的对象，将其标记为root
2. 并发标记：将root标记的对象所关联的对象进行标记
3. 重标记：在并发标记阶段，并没有stw，所以会有一些脏对象产生，即标记完毕之后又产生关联对象修改
4. 最终标记(stw)：最终确定所有没有脏对象的存活对象
5. 并发清理：并发的清理所有死亡对象
6. Reset：重设程序为下一次FGC做准备

CMS优劣：

- 优点：
  - 不像PN以及Serial一样全程需要stw，只需要在两个标记阶段stw即可
  - 并发标记、清楚来提升效率，减少stw的时间和整体gc时间
  - 在最终标记前通过预设次数的重标记来清理脏页减少stw时间
- 缺点：
  - 仍然存在stw
  - 基于标记清楚算法的GC，节省算力但是会产生内存碎片
  - 并发标记和清楚会造成cpu的高负担

## G1流程：

这个我只懂个大概，如下

分块分代回收，可分为youngGC和MixedGC，特点是可预测的GC时间（即所谓的软实时特性）

> 引申点：
>
> - 是否进行过线上分析
> - GC日志是否读过，里面有什么信息
> - 你们应用的YGC和FGC频率以及时间是多少
> - 你清楚当前应用YGC最多的一般是什么吗
>
> 业务相关：
>
> - 在线上大部分curd业务当中，实际上造成ygc影响较严重且可优化的是日志系统
> - 对dump出来的堆进行分析的话里面有很大一块是String，而其中大概率会是日志中的各种入参出参
> - 优化方案有很多：
>   - 将不需要打日志的地方去除全量日志打印功能
>   - 日志在不同环境分级打印
>   - 只打出错误状态的日志
>   - 在大促期间关闭非主要日志打印
>   - 同步改异步等

6.标记清除和标记整理算法的理解以及优缺点。

上文已答

7.eden survivor区的比例，为什么是这个比例，eden survivor的工作过程。

8:2

定性的来讲：大部分对象都只有极短的存活时间，基本就是函数run到尾就释放了，所以给新晋对象的buffer需要占较多的比例，而s区可以相对小一点来容纳长时间存活的对象，较小的另一个原因是在几次年龄增长后对象会进入老年代

定量的来讲：实验所得，也可以根据自己服务器的情况动态调整（不过笔者没调过）

8.JVM如何判断一个对象是否该被GC，可以视为root的都有哪几种类型。

没有被GCRoot所关联

Root对象：（tips：不用硬记，针对着JVM内存区域来理解即可）

1. 函数栈上的引用：包括虚拟机栈和native栈
2. static类的引用：存在方法区内
3. 常量池中的常量：堆中

> 引申点：
>
> - gc roots和ref count的区别

9.强软弱虚引用的区别以及GC对他们执行怎样的操作。

强：代码中正常的引用，存在即不会被回收

软：在内存不足的时候会对其进行GC，可用于缓存场景（类似redis淘汰）

弱：当一个对象只有弱引用关联的时候会被下一次GC给回收

虚：又称幽灵引用，基本没啥用，在GC的时候会感知到

> 引申点：
>
> - 每个引用的使用场景
> - 是否在源码或者项目中看到过or使用过这几种引用类型（ThreadLocal里用了WeakReference）

10.Java是否可以GC直接内存。

在GC过程中如果发现堆外内存的Ref被使用则GC

11.Java类加载的过程。

12.双亲委派模型的过程以及优势。

13.常用的JVM调优参数。

14.dump文件的分析。

15.Java有没有主动触发GC的方式（没有）。

## 多线程

1.Java实现多线程有哪几种方式。

2.Callable和Future的了解。

3.线程池的参数有哪些，在线程池创建一个线程的过程。

4.volitile关键字的作用，原理。

5.synchronized关键字的用法，优缺点。

6.Lock接口有哪些实现类，使用场景是什么。

7.可重入锁的用处及实现原理，写时复制的过程，读写锁，分段锁（ConcurrentHashMap中的segment）

8.悲观锁，乐观锁，优缺点，CAS有什么缺陷，该如何解决。

9.ABC三个线程如何保证顺序执行。

10.线程的状态都有哪些。

11.sleep和wait的区别。

12.notify和notifyall的区别。

13.ThreadLocal的了解，实现原理。

## 数据库相关

1.常见的数据库优化手段

1. log同步刷盘改异步刷盘
2. 集群的话强双写改异步同步
3. 针对sql优化（explain慢sql）
4. 添加索引

2.索引的优缺点，什么字段上建立索引

优点：查的快，支持range

缺点：大部分查询实际需要回表，索引建立会额外消耗内存和磁盘，对开发者的sql也有要求

字段：区分度大的字段

3.数据库连接池。

4.durid的常用配置。

计算机网络

1.TCP，UDP区别。

2.三次握手，四次挥手，为什么要四次挥手。

3.长连接和短连接。

4.连接池适合长连接还是短连接。

## 设计模式

1.观察者模式

2.代理模式

举例子JDK动态代理，通过一层proxy对真实对象进行代理，进行一些额外操作（e.g.:增强行为、负载均衡等）

3.单例模式，有五种写法，可以参考文章单例模式的五种实现方式

1. 普通单例
2. lazyloading+syn单例
3. lazyloading+doublecheck单例
4. 枚举
5. 最后一种不知道，查了发现是静态内部类单例，利用静态内部类第一次访问才加载的机制实现lazyloading

4.可以考Spring中使用了哪些设计模式

分布式相关

1.分布式事务的控制。

2.分布式锁如何设计。

3.分布式session如何设计。

4.dubbo的组件有哪些，各有什么作用。

duboo不熟悉

5.zookeeper的负载均衡算法有哪些。

zookeeper就会个zab，不过负载均衡无非是公平轮询、加权轮询、随机轮询或者维护某些资源信息的动态路由这几种

6.dubbo是如何利用接口就可以通信的。

不太熟，估计涉及到服务注册以及序列化反序列化相关内容

## 缓存相关

1.redis和memcached的区别。

2.redis支持哪些数据结构。

3.redis是单线程的么，所有的工作都是单线程么。

4.redis如何存储一个String的。

5.redis的部署方式，主从，集群。

6.redis的哨兵模式，一个key值如何在redis集群中找到存储在哪里。

7.redis持久化策略。

## 框架相关

1.SpringMVC的Controller是如何将参数和前端传来的数据一一对应的。

2.Mybatis如何找到指定的Mapper的，如何完成查询的。

3.Quartz是如何完成定时任务的。

4.自定义注解的实现。

5.Spring使用了哪些设计模式。

6.Spring的IOC有什么优势。

7.Spring如何维护它拥有的bean。

一些较新的东西

1.JDK8的新特性，流的概念及优势，为什么有这种优势。

2.区块链了解

3.如何设计双11交易总额面板，要做到高并发高可用。

## 最后

希望我总结的这些东西对你们会有帮助，你们看完之后有什么的不懂的欢迎在下方留言讨论，也可以私信问我，我一般看完之后都会回的，也可以关注我的公众号：前程有光，马上金九银十跳槽面试季，整理了1000多道将近500多页pdf文档的Java面试题资料，文章都会在里面更新，整理的资料也会放在里面。

作者：极乐妙音
链接：https://www.nowcoder.com/discuss/427064
来源：牛客网



**自我介绍** 

 **[项目]()介绍** 

 **Java相关** 

 1、线程池介绍，任务队列如果满了怎么办； 

 参考：https://www.cnblogs.com/dolphin0520/p/3932921.html 

 2、CAS介绍，CAS有什么问题，Java是否有解决方法； 

 参考：https://www.cnblogs.com/qjjazry/p/6581568.html 

 3、HashMap介绍，equals和hashCode函数； 

 参考：https://blog.csdn.net/woshimaxiao1/article/details/83661464 

 4、final关键字，final常量存储位置，常量池的好处； 

 参考：[https://www.cnblogs.com/dolphin0520/p/3736238.html ](https://www.cnblogs.com/dolphin0520/p/3736238.html)  

 https://www.jianshu.com/p/c7f47de2ee80 

 **网络相关** 

 1、HTTPS介绍； 

 参考：https://www.jianshu.com/p/14cd2c9d2cd2 

 2、TCP三次握手和四次挥手； 

 参考：https://yuanrengu.com/2020/77eef79f.html 

 **[算法题]()** 

 合并两个有序[链表]()；面试官让我先写函数，然后在主函数中测试运行。 

  Leetcode：https://leetcode-cn.com/problems/merge-two-sorted-lists/






  ***\*2020-5-13\****  ***\*1面结束后2小时，\*******\*hr约2面\*******\*，因2面面试官当天有事，所以约了下周二。\****  



 ***\*2020-5-19  2面  60min\**** 

 ***\*[项目]()介绍\**** 

 主要做了什么，是否还有可以优化的地方。 

 ***\*[算法题]()\**** 

 三个线程，线程1打印a，线程2打印b，线程3打印c，要求循环打印abc10次。 

   参考：  

   https://blog.csdn.net/xiaokang123456kao/article/details/77331878?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase    

 ***\*Java相关\**** 

 volatile关键字作用。 

   参考：  

   [https://blog.csdn.net/u012723673/article/details/80682208?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-tas](https://blog.csdn.net/u012723673/article/details/80682208?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)  

   ***\*2020-5-1\****  ***\*9\****   ***\*2面结束后\****  ***\*4\****  ***\*小时，hr约\****  ***\*3\****  ***\*面。\****  

 ***\*2020-5-\*******\*20\**** ***\*3\*******\*面 60min\**** 

 ***\*[算法题]()\**** 

 计算从一棵多叉树（节点取值不相同）的根节点走N步，能走到节点x的概率，任何一个走过的节点不能走第二次（即不能往回走），如果没路可以走可以原地走，如果有路可走，但是步数没用完需要接着走。 

 例：  

 1  

 2       3       4  

 5  6    7     8   9  

 N = 2  

 x = 6  

 result = 1/6（第1步，从节点1走到节点2的概率为1/3；第2步，从节点2走到节点6的概率为1/2） 

 class Node {  

  int val; 

  List<Node> subNodes; 

 }  

 float func(Node root, int N, int x){  

 } 

 个人思路：层次遍历，累积每层的数目，注意考虑结束条件。时间复杂度O(n)。 

 ***\*自我介绍\**** 

 ***\*场景题\**** 

 1、如何实现[牛客]()网的在线编程。 

 个人理解：[客户端]()通过HTTP的POST请求把代码传给服务端，服务端在编译运行之后把结果反馈给[客户端]()。 

 2、在上述过程中，如果有多个服务器，如何平衡任务量。 

 个人理解：将服务器分成主服务器和辅服务器两种，主服务器负责接收[客户端]()请求，并根据负载情况，将任务分派给相应的辅服务器编译运行，最后主服务器把结果反馈给[客户端]()。 

 3、在上述过程中，如果主服务器的请求量过大，如何解决。 

 个人理解：设置多个主服务器，根据请求所在地域划分主服务器的处理范围，比如按照省份划分。 

 4、在上述过程中，如何实现不同地域的[客户端]()输入同一个URL后访问不同的主服务器。 

 个人理解：在域名解析阶段，DNS服务器根据请求所在地域返回相应服务器的ip地址。 

 5、如果服务器在执行一个任务时，出现了异常，比如陷入死循环，一直占用CPU资源，那么如何监测出来。 

 个人理解：服务器启动一个监测进程，每隔一段时间监测一下其它进程的运行情况。但如何区分死循环还是程序在处理一个耗时任务，我不清楚。 

 注：以上的个人理解不一定正确。 

 ***\*[项目]()介绍\**** 

 ***\*Java相关\**** 

 如何判断堆中哪些对象需要被回收。 

   参考：https://www.cnblogs.com/czwbig/p/11127159.html  

​       

 ***\*2020-5-22 HR面 20min\**** 

 **2020-5-****25 录用通知** 

 ***\*总结：\**** 

 人生第一次实习面试，面试体验不错，感谢字节给予的面试机会。 

 各位[牛客]()同仁发的[面经]()还是挺有用的，有了这些面试问题，再结合搜索到的博客，认真学习、思考和总结。 

 感谢[牛客]()同仁和博客博主的无私分享，南无阿弥陀佛。