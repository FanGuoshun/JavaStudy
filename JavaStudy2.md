# m

## 英文自我介绍

Hello, My name is Fan Guoshun, it is a great honor to have this opportunity for an interview. I am 23 years old, born in Anhui province. I am currently a second-year graduate student in Shanghai Jiao Tong University. My major is computer technology. Thank you for giving me the chance. 

## 英文项目介绍

This personal blog project is built using SpringBoot. For visitors, it implements the functions of browsing, thumbing up and commenting blog posts. For administrators, it realizes the function of adding, deleting and updating blog posts. It also leverages SpringSecurity and JWT for authentication authorization. In addition, it uses Redis to cache the latest and most popular blog posts. It also leverages RabbitMQ to asynchronously update blog posts and send mail.

## 设计模式

https://blog.csdn.net/weixin_43122090/article/details/105462226

设计模式原则 https://blog.csdn.net/httphttpcn/article/details/6079004

https://cloud.tencent.com/developer/article/1688358

**1. 请列举出在JDK中几个常用的设计模式？**

- 单例模式：保证被创建一次，节省系统开销。
- 工厂模式（简单工厂、抽象工厂）：解耦代码。
- 观察者模式：定义了对象之间的一对多的依赖，这样一来，当一个对象改变时，它的所有的依赖者都会收到通知并自动更新。
- 外观模式：提供一个统一的接口，用来访问子系统中的一群接口，外观定义了一个高层的接口，让子系统更容易使用。
- 模版方法模式：定义了一个算法的骨架，而将一些步骤延迟到子类中，模版方法使得子类可以在不改变算法结构的情况下，重新定义算法的步骤。
- 状态模式：允许对象在内部状态改变时改变它的行为，对象看起来好像修改了它的类。
- 装饰器设计模式：（Decorator design pattern）被用于多个 Java IO 类中。

**2. 什么是设计模式？你是否在你的代码里面使用过任何设计模式？**

设计模式是世界上各种各样程序员用来解决特定设计问题的尝试和测试的方法。设计模式是代码可用性的延伸

**3. Java 中什么叫单例设计模式？请用Java 写出线程安全的单例模式**

单例模式重点在于在整个系统上共享一些创建时较耗资源的对象。整个应用中只维护一个特定类实例，它被所有组件共同使用。`Java.lang.Runtime`是单例模式的经典例子。从 Java 5 开始你可以使用枚举（enum）来实现线程安全的单例。

**4. 在 Java 中，什么叫观察者设计模式（observer design pattern）**？

观察者模式是基于对象的状态变化和观察者的通讯，以便他们作出相应的操作。简单的例子就是一个天气系统，当天气变化时必须在展示给公众的视图中进行反映。这个视图对象是一个主体，而不同的视图是观察者。

**5. 使用工厂模式最主要的好处是什么？在哪里使用？**

工厂模式的最大好处是增加了创建对象时的封装层次。如果你使用工厂来创建对象，之后你可以使用更高级和更高性能的实现来替换原始的产品实现或类，这不需要在调用层做任何修改。

**6. 举一个用 Java 实现的装饰模式(decorator design pattern)？它是作用于对象层次还是类层次？**

装饰模式增加强了单个对象的能力。Java IO 到处都使用了装饰模式，典型例子就是 Buffered 系列类如`BufferedReader`和`BufferedWriter`，它们增强了`Reader`和`Writer`对象，以实现提升性能的 Buffer 层次的读取和写入。

**7. Java 编程为什么不允许从静态方法中访问非静态变量？**

Java 中不能从静态上下文访问非静态数据只是因为非静态变量是跟具体的对象实例关联的，而静态的却没有和任何实例关联。

**8. 如果需要设计一个 ATM 机，你的设计思路是什么？**

比如设计金融系统来说，必须知道它们应该在任何情况下都能够正常工作。不管是断电还是其他情况，ATM 应该保持**正确的状态（事务）** , 想想 **加锁（locking）、事务（transaction）、错误条件（error condition）、边界条件（boundary condition）** 等等。尽管你不能想到具体的设计，但如果你可以指出非功能性需求，提出一些问题，想到关于边界条件，这些都会是很好的。

**9. 在 Java语言 中，什么时候用重载，什么时候用重写？**

如果你看到一个类的不同实现有着不同的方式来做同一件事，那么就应该用重写（overriding），而重载（overloading）是用不同的输入做同一件事。在 Java 中，重载的方法签名不同，而重写并不是。

**10. 请举例说明什么情况下会更倾向于使用抽象类而不是接口？**

接口和抽象类都遵循”面向接口而不是实现编码”设计原则，它可以增加代码的灵活性，可以适应不断变化的需求。下面有几个点可以帮助你回答这个问题：

- 在 Java 中，你只能继承一个类，但可以实现多个接口。所以一旦你继承了一个类，你就失去了继承其他类的机会了。
- 接口通常被用来表示附属描述或行为如：`Runnable、Clonable、Serializable`等等，因此当你使用抽象类来表示行为时，你的类就不能同时是`Runnable`和`Clonable`(注：这里的意思是指如果把`Runnable`等实现为抽象类的情况)，因为在 Java 中你不能继承两个类，但当你使用接口时，你的类就可以同时拥有多个不同的行为。
- 在一些对时间要求比较高的应用中，倾向于使用抽象类，它会比接口稍快一点。
- 如果希望把一系列行为都规范在类继承层次内，并且可以更好地在同一个地方进行编码，那么抽象类是一个更好的选择。有时，接口和抽象类可以一起使用，接口中定义函数，而在抽象类中定义默认的实现。

**11. 简单工厂和抽象工厂有什么区别？**

- 简单工厂：用来生产同一等级结构中的任意产品，对于增加新的产品，无能为力。
- 工厂方法：用来生产同一等级结构中的固定产品，支持增加任意产品。
- 抽象工厂：用来生产不同产品族的全部产品，对于增加新的产品，无能为力；支持增加产品族。

本文来简单介绍一下设计模式采用的几大原则。

## **一. 单一职责原则**

**含义**

单一职责原则（Single Responsibility Principle，SRP）：一个类只负责一个功能领域中的相应职责或可以定义为：就一个类而言，应该只有一个引起它变化的原因。

**举例**

做一个简单的程序用做图表展示客户列表数据，其中包括图表的创建和展示、数据库连接和获取客户列表的方法，如下图所示：

![img](https://ask.qcloudimg.com/http-save/yehe-6094000/9rlbnycg54.png?imageView2/2/w/1620)

从上图可以看出，CustomerDataChart其职责不够单一，其包括数据库连接、数据库操作、图表创建和展示，这些功能应该拆分到不同的类中，比如：

![img](https://ask.qcloudimg.com/http-save/yehe-6094000/5l1i180dka.png?imageView2/2/w/1620)

这样修改以后，数据库连接就由DBUtils来完成；CustomDao定义获取客户列表方法，其具体实现由DBUtil来完成；CustomerDataChart则包括图表的创建和展示，展示的内容则由CustomerDao来完成；经过上述调整，每个类都有单一的清晰的职责。

## **二. 开闭原则**

**意图**

开闭原则就是说对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有的代 码，而是要扩展原有代码，实现一个热插拔的效果。

先来看一个示例：

还是图表展示，其支持饼图和柱状图的展示：

![img](https://ask.qcloudimg.com/http-save/yehe-6094000/zcfie9sfr9.png?imageView2/2/w/1620)

![img](https://ask.qcloudimg.com/http-save/yehe-6094000/u8a6n2an04.png?imageView2/2/w/1620)

如果我们需要增加一个其他图形的展示，我们需要增加else if的条件来进行扩展，这样原来的代码就别修改。

那么为了满足开闭原则，需要怎么做呢？那就要对系统采用**抽象化设计**。**抽象化**是开闭原则的关键。

![img](https://ask.qcloudimg.com/http-save/yehe-6094000/pykzwt2ww6.png?imageView2/2/w/1620)

经过抽象化之后，ChartDisplay的变量就成为AbstarctChart，如果有新的Chart需要扩展，如增加一个AbcChart，那么AbcChart只要继承AbstractChart即可，如果ChartDisplay需要使用AbcChart进行图表展示，则其只要setChart（AbcChart）即可，这就满足了扩展，且不会对原有代码进行修改。

## **三. 里氏替换原则**

**意图**

里氏代换原则(Liskov Substitution Principle LSP)面向对象设计的基本原则之一。里氏代换原 则中说，任何基类可以出现的地方，子类一定可以出现。

比如，我们有一个功能用于给普通用户和VIP用户发送邮件的功能，如下图所示：

![img](https://ask.qcloudimg.com/http-save/yehe-6094000/01moa4plmk.png?imageView2/2/w/1620)

我们可以看到，其实CommonCustomer和VipCustomer发送邮件很类似。我们可以采用里氏替换原则，可以采用如下具体步骤：

- 建立抽象；通过抽象来建立规范；
- 具体实现在运行时取代抽象；保证了系统的扩展性和灵活性；

修改后如下图所示，抽象一个Customer，EmailSender中send方法的参数为Customer即可。

![img](https://ask.qcloudimg.com/http-save/yehe-6094000/5ijrv81ifr.png?imageView2/2/w/1620)

这样，

```javascript
Customer commomCus = new CommonCustomer();
EmailSender.send(commomCus );就可以发送普通用户邮件；
```

同样，

```javascript
Customer vipCus = new VipCustomer();
EmailSender.send(vipCus );就可以发送VIP用户邮件；
```

## **四. 依赖倒转原则**

**意图**

面向接口编程，依赖于抽象而不依赖于具体。写代码时用到具体类时，不与具体类交互，而与具体类的上层接口交互。

比如，有一个小功能，读取文件中的用户名单，然后添加到数据库中。可以采用如下方式实现：

![img](https://ask.qcloudimg.com/http-save/yehe-6094000/y0f3wlkct0.png?imageView2/2/w/1620)

上述示例中，CustomDao与具体的文件数据转换类进行交互式，其不符合依赖倒转原则。依据依赖倒转原则，需要与具体类的上层接口或者抽象类交互，那么我们可以采用如下方式进行。

![img](https://ask.qcloudimg.com/http-save/yehe-6094000/gds1vtxtc4.png?imageView2/2/w/1620)

定义一个抽象类DataConvertor，其具有两个子类TXTDataConvertor和ExcelDataConvertor完成具体的数据转换。

至于CustomDao采用哪一种类型进行数据转换，其可以采用配置的方式进行。

这样，程序将不在于具体的实现类进行直接交互。关于依赖倒转原则，相信使用Spring来进行开发的同学深有体会。

## **五. 接口隔离原则**

**意图**

每个接口中不存在子类用不到却必须实现的方法，如果不然，就要将接口拆分。使用多个隔离的接口，比使用单个接口（多个接口方法集合到一个的接口）要好。

举个例子，自己实现ArrayList和LinkedList，我们可以定义一个List接口，

```javascript
public interface List<T> {
  public T get();
  public void add(T t);
  public T poll();
  public T peek();  
}
```

我们来实现LinkedList：

```javascript
public class LinkedList implements List<Integer>{

  @Override
  public Integer get() {
    // Implement this method
    return null;
  }

  @Override
  public void add(Integer t) {
    // Implement this method
  }

  @Override
  public Integer poll() {
    // Implement this method
    return null;
  }

  @Override
  public Integer peek() {
    // Implement this method
    return null;
  }
}
```

然后再来实现ArrayList：

```javascript
public class ArrayList implements List<Integer>{

  @Override
  public Integer get() {
    // Implement this method
    return null;
  }

  @Override
  public void add(Integer t) {
    // Implement this method
  }

  @Override
  public Integer poll() {
    // ArrayList does not require this method
    return null;
  }

  @Override
  public Integer peek() {
    // ArrayList does not require this method
    return null;
  }
}
 
```

***想想这样做，有什么问题吗？***

我们可以看到poll和peek方法，在ArrayList中用不到，但是我们却一定要实现它们，这就是因为List接口太大导致的。

我们可以创建一个新的接口：

```javascript
public interface Deque<T> {
  public T poll();
  public T peek();  
}
```

然后移除之前List中的poll和peek方法：

```javascript
public interface List<T> {
  public T get();
  public void add(T t);  
}
 
```

这样，

ListLinked的实现就变成：

```javascript
public class LinkedList implements List<Integer>,Deque<Integer>{

  @Override
  public Integer get() {
    // Implement this method
    return null;
  }

  @Override
  public void add(Integer t) {
    // Implement this method
  }

  @Override
  public Integer poll() {
    // Implement this method
    return null;
  }

  @Override
  public Integer peek() {
    // Implement this method
    return null;
  }
}
 
```

ArrayList的实现为：

```javascript
public class ArrayList implements List<Integer>{

  @Override
  public Integer get() {
    // Implement this method
    return null;
  }

  @Override
  public void add(Integer t) {
    // Implement this method
  }
}
 
```

这思想不就是和JDK源代码中ArrayList和LinkList的思想一致吗~~

![img](https://ask.qcloudimg.com/http-save/yehe-6094000/8kdj7qdb22.png?imageView2/2/w/1620)

![img](https://ask.qcloudimg.com/http-save/yehe-6094000/bgdgjcmvoq.png?imageView2/2/w/1620)

## **六. 迪米特法则**

**意图**

一个类对自己依赖的类知道的越少越好。也就是说无论被依赖的类多么复杂，都应该将逻辑封装在方法的内部，通过 public 方法提供给外部。

```javascript
一个软件实体应该尽可能少地和其它实体产生作用。
```

比如，我们很多人一起在线聊天，我说话不需要和每个人都说一下进行交互，只需要一个聊天室，就能将大家联系在一起。迪米特法则思想的体现，可以在中介者模式中体现，具体可以参考：[中介者模式浅析](http://mp.weixin.qq.com/s?__biz=MzAxMTY0Nzg1Mg==&mid=2648942281&idx=1&sn=59e97912b9547d08084f5fe86398526e&chksm=83aae8cab4dd61dcbadf7aa66d36940c473ab57f24160164997faad90d5e77b0f37caafbc7ce&scene=21#wechat_redirect)

## **七. 合成复用原则**

**意图**

尽量使用合成复用，而不是采用继承来达到复用的目的。

比如，CustomerDao可以采用继承DBUtil完成数据库操作。

![img](https://ask.qcloudimg.com/http-save/yehe-6094000/hjqke6qkmy.png?imageView2/2/w/1620)

采用复用的方式，CustomerDao只要委托DBUtil来完成数据库操作，DBUtil其可以采用Oracle、[MySQL](https://cloud.tencent.com/product/cdb?from=10680)或者其他数据库的链接实现，这样更加灵活。

![img](https://ask.qcloudimg.com/http-save/yehe-6094000/dno3kh9puu.png?imageView2/2/w/1620)

再如：

```javascript
当一个抽象可能有多个实现时，通常用继承来协调它们。抽象类定义对该抽象的接口，
而具体的子类则有不同的方式加以实现。但是此方法有时候不够灵活。
继承机制将抽象部分与它的实现部分固定在一起，使得难以对抽象部分和实现部分
独立地进行修改、扩充和重用。
```

这个我在《[桥接模式浅析](http://mp.weixin.qq.com/s?__biz=MzAxMTY0Nzg1Mg==&mid=2648942340&idx=1&sn=7dc96c88bcd4bc0e58ab42794fbe31e2&chksm=83aae907b4dd6011d9c20ee9cd9c74099e1c7b66f3d9108b67ce19de1a666978e5511c71e94b&scene=21#wechat_redirect)》中已经做了详细的说明，有兴趣的同学可以去扩展看看。

## **八. 小结**

面向对象的一般原则，和其关系如下如图所示：

![img](https://ask.qcloudimg.com/http-save/yehe-6094000/vbesw8dzzx.png?imageView2/2/w/1620)

## 一个数的各位数之和能被3整除，则该数能被3整除

对于一位数，无需多言；

 

对于两位数，可写成10x+y的方式,改写一下

10x+y=(9x)+(x+y)

前面括号的部分无疑是3的倍数，而如果（x+y）是3的倍数的话，那10x+y就一定是3的倍数。

 

对于三位数，可写成100x+10y+z的形式，我们可以把它改变一下

100x+10y+z=99x+x+9y+y+z=（99x+9y）+（x+y+z）

前面括号的部分无疑是3的倍数，而如果（x+y+z）是3的倍数的话，那100x+10y+z就是3的倍数。

 

对于四位数，可写成1000x+100y+10z+w,我们又可以变换一下

1000x+100y+10z+w=(999x+99y+9z)+(x+y+z+w)

同理如果(x+y+z+w)是3的倍数的话，1000x+100y+10z+w就是3的倍数

 

以此类推，五位数，六位数到n位数都是一样的推导过程。

## 死锁

## 算法题

https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/

判断一个字符串是否为某一单个字串的重复，比如ABABABAB，如果是输出AB4，如果不是输出null

给定入栈序列和出栈序列，判断出栈序列是否合法

求二叉树中任意两个结点的距离 https://blog.csdn.net/weixin_30762087/article/details/98933928?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_title-0&spm=1001.2101.3001.4242

```java
public class Main {
    public static void main(String[] args) {

    }
    public int path(TreeNode root, TreeNode node, int path){
        if(root == null) return -1;
        if(root.val == node.val) return path;
        return Math.max(path(root.left, node, path + 1), path(root.right, node, path + 1));
    }

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left == null) return right;
        if (right == null) return left;
        return root;
    }


}


class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int x) {
        val = x;
    }
}
```

https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/

手写HashMap

 [leetcode](https://www.nowcoder.com/jump/super-jump/word?word=leetcode) 124，22

[leetcode](https://www.nowcoder.com/jump/super-jump/word?word=leetcode)39.组合总和 [leetcode](https://www.nowcoder.com/jump/super-jump/word?word=leetcode)40.组合总和 II

.n个结点的[二叉树](https://www.nowcoder.com/jump/super-jump/word?word=二叉树)的形态个数 卡特兰数 https://www.cnblogs.com/hujunzheng/p/5040334.html https://zhuanlan.zhihu.com/p/97619085

![这里写图片描述](https://img-blog.csdn.net/20150628170417849)

日期差值 https://leetcode-cn.com/problems/number-of-days-between-two-dates/

二叉搜索树的迭代器 https://leetcode-cn.com/problems/binary-search-tree-iterator/

## 面试总结

- 都了解哪些设计模式

- 介绍一下单例模式

  - 所谓单例，就是整个程序有且仅有一个实例。该类负责创建自己的对象，同时确保只有一个对象被创建。

  - ### 优点：

  1. 在内存里只有一个实例，减少了内存的开销，尤其是频繁的创建和销毁实例（比如管理学院首页页面缓存）。

  2. 避免对资源的多重占用（比如写文件操作）。

     ### 缺点：

  3. 没有接口，不能继承，与单一职责原则冲突，一个类应该只关心内部逻辑，而不关心外面怎么样来实例化。

- 单例模式的优缺点是什么

- 介绍一下MySQL的索引

  - 索引是一种特殊的文件(InnoDB数据表上的索引是表空间的一个组成部分)，它们包含着对数据表里所有记录的引用指针。

    索引是一种数据结构。数据库索引，是数据库管理系统中一个排序的数据结构，以协助快速查询、更新数据库表中数据。索引的实现通常使用B树及其变种B+树。

    更通俗的说，索引就相当于目录。为了方便查找书中的内容，通过对内容建立索引形成目录。索引是一个文件，它是要占据物理空间的。
    ————————————————
    版权声明：本文为CSDN博主「小杰要吃蛋」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
    原文链接：https://blog.csdn.net/weixin_43122090/article/details/105531113

- 为什么MySQL要用B+树呢

  - B+树能显著减少IO次数，提高效率
  - B+树的查询效率更加稳定，因为数据放在叶子节点
  - B+树能提高范围查询的效率，因为叶子节点指向下一个叶子节点
  - https://blog.csdn.net/zzti_erlie/article/details/82973742

- 系统做到高内聚低耦合

- 说一下面向对象中的多态

  - https://www.cnblogs.com/kaleidoscope/p/9790766.html

- 你觉得像Java和C#这样的语言采用自动内存管理都有哪些好处和弊端

  - https://blog.csdn.net/qq_31664497/article/details/79879071

- 操作系统中进程和线程的区别

- 聊了聊并发的一些内容

- 网页[客户端](https://www.nowcoder.com/jump/super-jump/word?word=客户端)发送请求，服务段回复的过程，越详细越好

  - https://blog.csdn.net/qq_40959677/article/details/94873075

  http的状态码

  堆和栈的区别

  mmap了解吗（不了解QAQ）
  
  mmap是一种内存映射文件的方法，即将一个文件或者其它对象映射到进程的地址空间，实现文件磁盘地址和进程虚拟地址空间中一段虚拟地址的一一对映关系。实现这样的映射关系后，进程就可以采用指针的方式读写操作这一段内存，而系统会自动回写脏页面到对应的文件磁盘上，即完成了对文件的操作而不必再调用read,write等系统调用函数。相反，内核空间对这段区域的修改也直接反映用户空间，从而可以实现不同进程间的文件共享。如下图所示：
  
  mmap用于把文件映射到内存空间中，简单说mmap就是把一个文件的内容在内存里面做一个映像。映射成功后，用户对这段内存区域的修改可以直接反映到内核空间，同样，内核空间对这段区域的修改也直接反映用户空间。那么对于内核空间<---->用户空间两者之间需要大量数据传输等操作的话效率是非常高的。
  ————————————————
  版权声明：本文为CSDN博主「BruceZhang」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
  原文链接：https://blog.csdn.net/dlutbrucezhang/article/details/9080173

# xiecheng

讲一下Java的堆模型
死锁是什么（用java写一个java死锁的场景）

6， stringBuilder ，StringBuffer的区别

7， String 和 StringBuilder在合并字符串上的区别

1.说了MySQL有什么锁，锁机制

1.四层模型、七层模型

2.物理层与链路层的通信方式

SpringMVC说一下？（只是[项目](https://www.nowcoder.com/jump/super-jump/word?word=项目)用一下，没有具体了解过）

gc root包含那些

[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)翻转

给你5张牌，其中包含两张大小王，判断是否是顺子

**[二叉树](https://www.nowcoder.com/jump/super-jump/word?word=二叉树)非递归后续遍历**

[红黑树](https://www.nowcoder.com/jump/super-jump/word?word=红黑树)计算hash时，是如何计算的（与容量与？）

为什么是与，为什么不用取余，平时为什么不使用与？（懵 答：hash容量相对比较小，只需要使用尾部几位）

\3.    抽象、继承、 实现 （八股，先背一套）

\4. final 关键字 哪些东西 （类，方法，属性，String里面有final char[]数组）

作者：杀死那只大白鹿
链接：https://www.nowcoder.com/discuss/627993?type=post&order=time&pos=&page=1&channel=-1&source_id=search_post_nctrack
来源：牛客网



\7. [redis]()的数据类型？zset一般什么时候用？zset分数相同的怎么处理？（emmm） 

 \8.    平时有用过自定义注解吗？ （emmm没有怎么用过） 

 \9. aop的一般什么时候使用，使用方法，切入点怎么设置 。 

 \10.   为什么是最左前缀原则（b+树的特点？） 

 \11. mysql的三种存储引擎，innodb为什么常用

5.说一下从浏览器打开一个链接中间发生了什么？

a.删除[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)重复元素

b.判断两颗[二叉树](https://www.nowcoder.com/jump/super-jump/word?word=二叉树)是否相同

c.字符串中第一次重复出现的字符

作者：coco201903192152680
链接：https://www.nowcoder.com/discuss/586465?type=post&order=time&pos=&page=1&channel=-1&source_id=search_post_nctrack
来源：牛客网

写一个前缀树，要实现的方法包括put、search、startWith。（基本写出来了） 
  2，一个很大的无序数组，找出其中最小的k个数。时间复杂度。（只需要说思路，我具体说了用快速[排序]()里面的那种思想）

Linux操作系统中的调度[算法](https://www.nowcoder.com/jump/super-jump/word?word=算法)。

，JVM中一个对象从创建到被回收所经历的整个过程

了解哪些设计模式，说一下单例模式

，连接数据库的操作怎么做的

1..哪些线程安全的list

2.线程不安全的list会导致什么问题

4.post和get的区别
5.用tcp传递post和get的区别
6.布隆过滤器（不晓得） 反射（不晓得）

输入:{{1,2,3,4},{5,6,7,8},{9,10,11,12}}

输出:1,2,3,4,8,12,11,10,9,5,6,7

5.2

类似于“{{}”求是否匹配

5.3

建立二叉查找树

[算法](https://www.nowcoder.com/jump/super-jump/word?word=算法)答出来思路，面试官会提醒，引导你，看你写得有思路了，会让你写下一题 。

进程通讯的几种方式

分页与分段的区别

左连接与外连接

三种范式

图的存储方式 邻接矩阵和[链表](https://www.nowcoder.com/jump/super-jump/word?word=链表)

两种方式的引用场景

图的深度搜索

判断图是否有环

[平衡二叉树](https://www.nowcoder.com/jump/super-jump/word?word=平衡二叉树)介绍 左旋与右旋产生的条件

[红黑树](https://www.nowcoder.com/jump/super-jump/word?word=红黑树)性质

强连通图？

[300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

判断字符串中最大字符