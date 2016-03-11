---
title: "core java"
date: 2015-01-05 23:17:04 +0800
categories: java
tags: [core java]
---

## 语法规则

### 一个".java"源文件中是否可以包括多个类(不是内部类)?有什么限制?
可以有多个类，但只能有一个public的类，并且public的类名必须与文件名相一致。

### 使用final关键字修饰一个变量时，是引用不能变，还是引用的对象不能变?
使用final关键字修饰一个变量时，是指引用变量不能变，引用变量所指向的对象中的内容还是可以改变的。

### final，finally，finalize的区别?
* final用于声明属性，方法和类，分别表示属性不可变，方法不可覆盖，类不可继承。内部类要访问局部变量，局部变量必须定义成final类型，例如，一段代码
* finally是异常处理语句结构的一部分，表示总是执行。
* finalize是Object类的一个方法，在垃圾收集器执行的时候会调用被回收对象的此方法，可以覆盖此方法提供垃圾收集时的其他资源回收，例如关闭文件等。JVM不保证此方法总被调用

### Void 类的作用
在java方法中参数类型不能是void，因此可以用Void类解决这个问题。

### boolean占几个字节?
boolean在java规范中没有规定所占的字节数，它所占的字节数由jvm自己规定。
sun jvm中boolean占1个字节。

### java中有几种类型的流?
* 字节流，字符流。
* 字节流继承于InputStream，OutputStream，字符流继承于InputStreamReader，OutputStreamWriter。

<!-- more -->
### 什么是java序列化，如何实现java序列化?或者请解释Serializable接口的作用。
我们有时候将一个java对象变成字节流的形式传出去或者从一个字节流中恢复成一个java对象，例如，要将java对象存储到硬盘或者传送给网络上的其他计算机，这个过程我们可以自己写代码去把一个java对象变成某个格式的字节流再传输，但是，jre本身就提供了这种支持，我们可以调用OutputStream的writeObject方法来做，如果要让java帮我们做，要被传输的对象必须实现serializable接口，这样，javac编译时就会进行特殊处理，编译的类才可以被writeObject方法操作，这就是所谓的序列化。需要被序列化的类必须实现Serializable接口，该接口是一个mini接口，其中没有需要实现的方法，implementsSerializable只是为了标注该对象是可被序列化的。

例如，在web开发中，如果对象被保存在了Session中，tomcat在重启时要把Session对象序列化到硬盘，这个对象就必须实现Serializable接口。如果对象要经过分布式系统进行网络传输或通过rmi等远程调用，这就需要在网络上传输对象，被传输的对象就必须实现Serializable接口。

### heap和stack有什么区别。
java的内存分为两类，一类是栈内存，一类是堆内存。

* 栈内存是指程序进入一个方法时，会为这个方法单独分配一块私属存储空间，用于存储这个方法内部的局部变量，当这个方法结束时，分配给这个方法的栈会释放，这个栈中的变量也将随之释放。
* 堆是与栈作用不同的内存，一般用于存放不放在当前方法栈中的那些数据，例如，使用new创建的对象都放在堆里，所以，它不会随方法的结束而消失。**方法中的局部变量使用final修饰后，放在堆中，而不是栈中。**

### 垃圾回收器的基本原理是什么?垃圾回收器可以马上回收内存吗?有什么办法主动通知虚拟机进行垃圾回收?
GC采用有向图的方式记录和管理堆(heap)中的所有对象。通过这种方式确定哪些对象是"可达的"，哪些对象是"不可达的"。当GC确定一些对象为"不可达"时，GC就有责任回收这些内存空间。

程序员可以手动执行System.gc()，通知GC运行，但是Java语言规范并不保证GC 一定会执行。

### java中会存在内存泄漏吗，请简单描述。
* 长生命周期的对象持有短生命周期对象的引用就很可能发生内存泄露。
尽管短生命周期对象已经不再需要，但是因为长生命周期对象持有它的引用而导致不能被回收，这就是java中内存泄露的发生场景，通俗地说，就是程序员可能创建了一个对象，以后一直不再使用这个对象，这个对象却一直被引用，即这个对象无用但是却无法被垃圾回收器回收的，这就是java中可能出现内存泄露的情况，例如，缓存系统，我们加载了一个对象放在缓存中(例如放在一个全局map对象中)，然后一直不再使用它，这个对象一直被缓存引用，但却不再被使用。
* 如果一个外部类的实例对象的方法返回了一个内部类的实例对象，这个内部类对象被长期引用了，即使那个外部类实例对象不再被使用，但由于内部类持久外部类的实例对象，这个外部类对象将不会被垃圾回收，这也会造成内存泄露。
* 清空堆栈中的某个元素，并不是彻底把它从数组中拿掉，而是把存储的总数减少，假如堆栈加了10个元素，然后全部弹出来，虽然堆栈是空的，没有我们要的东西，但是这是个对象是无法回收的，这个才符合了内存泄露的两个条件:无用，无法回收。
* 当一个对象被存储进HashSet集合中以后，就不能修改这个对象中的那些参与计算哈希值的字段了，否则，对象修改后的哈希值与最初存储进HashSet集合中时的哈希值就不同了，在这种情况下，即使在contains方法使用该对象的当前引用作为的参数去HashSet集合中检索对象，也将返回找不到对象的结果，这也会导致无法从HashSet集合中单独删除当前对象，造成内存泄露。

### 能不能自己写个类，也叫java.lang.String?
可以，但在应用的时候，需要用自己的类加载器去加载，否则，系统的类加载器永远只是去加载jre.jar包中的那个java.lang.String。

### java中实现多态的机制是什么?
靠的是父类或接口定义的引用变量可以指向子类或具体实现类的实例对象，而程序调用的方法在运行期才动态绑定，就是引用变量所指向的具体实例对象的方法，也就是内存里正在运行的那个对象的方法，而不是引用变量的类型中定义的方法。

### abstract的method是否可同时是static，是否可同时是native，是否可同时是synchronized?
* abstract的method不可以是static的，因为抽象的方法是要被子类实现的，而static与子类扯不上关系。
* native方法表示该方法要用另外一种依赖平台的编程语言实现的，不存在着被子类实现的问题，所以它也不能是抽象的，不能与abstract混用。
* synchronized应该是作用在一个具体的方法上才有意义。而且方法上的synchronized同步所使用的同步锁对象是this，而抽象方法上无法确定this是什么。

### abstract和interface的区别
* 继承关系：一个类只能使用一个继承关系（单继承），但是一个类可以实现多个接口。
* 在abstract class的定义中，可以赋予方法默认行为，但是在interface中方法不能拥有默认行为。
* abstract class表示的是"is a"关系，interface表示的是"like a"关系。

### 接口是否可继承接口?抽象类是否可实现(implements)接口?抽象类是否可继承具体类(concrete class)?抽象类中是否可以有静态的main方法？
* 接口可以继承接口。
* 抽象类可以实现(implements)接口。
* 抽象类可以继承具体类。
* 抽象类中可以有静态的main方法。

### 泛型有什么好处
* 编译的时候检查类型安全
* 所有强制转换都是自动和隐式的
* 提高代码的重用率

### switch语句能否作用在byte上，能否作用在long上，能否作用在String上?
在switch(expr1)中，expr1只能是一个整数表达式或者枚举常量，整数表达式可以是int基本类型或Integer包装类型，
由于byte，short，char都可以隐含转换为int，所以，这些类型以及这些类型的包装类型也是可以的。
显然，long和String类型都不符合switch的语法规定，并且不能被隐式转换成int类型，所以，它们不能作用于swtich语句中。

### 是否可以从一个static方法内部发出对非static方法的调用？
不可以。因为非static方法是要与对象关联在一起的，必须创建一个对象后，才可以在该对象上进行方法调用，而static方法调用时不需要创建对象，可以直接调用。

### 运行时异常与一般异常有何异同？
异常表示程序运行过程中可能出现的非正常状态，运行时异常表示虚拟机运行时可能遇到的异常。java编译器要求方法必须声明抛出可能发生的非运行时异常，但是并不要求必须声明抛出未被捕获的运行时异常。

### error和exception有什么区别?
* error表示恢复不是不可能但很困难的情况下的一种严重问题。比如说内存溢出。不可能指望程序能处理这样的情况。
* exception表示一种设计或实现问题。也就是说，它表示如果程序运行正常，从不会发生的情况。

### 请写出你最常见到的5个runtime exception?
ClassCastException，IllegalArgumentException，IllegalStateException，IndexOutOfBoundsException，NullPointerException，FileNotFoundException

### Java中的异常处理机制的简单原理和应用?
Java对异常进行了分类，不同类型的异常分别用不同的Java类表示，所有异常的根类为java.lang.Throwable，Throwable下面又派生了两个子类：Error和Exception：

* Error表示应用程序本身无法克服和恢复的一种严重问题，程序只有死的份了，例如，说内存溢出和线程死锁等系统问题。
* Exception表示程序还能够克服和恢复的问题，其中又分为系统异常和普通异常，系统异常是软件本身缺陷所导致的问题，但在这种问题下还可以让软件系统继续运行或者让软件死掉，例如，数组脚本越界(ArrayIndexOutOfBoundsException)，空指针异常(NullPointerException)、类转换异常(ClassCastException)；普通异常是运行环境的变化或异常所导致的问题，是用户能够克服的问题，例如，网络断线，硬盘空间不够，发生这样的异常后，程序不应该死掉。

Exception的处理方案：

* 普通异常必须try..catch处理或用throws声明继续抛给上层调用方法处理，所以普通异常也称为checked异常。
* 系统异常可以处理也可以不处理，所以译器不强制用try..catch处理或用throws声明，所以系统异常也称为unchecked异常

### try{}里有一个return语句，那么紧跟在这个try后的finally{}里的code会不会被执行，什么时候被执行，在return前还是后?

``` java
public class ReturnFinallyTest {
  public static void main (String[] args) {
    System.out.println("main a=" + a());
  }

  public static String a() {
    String a = "return";
    try {
      System.out.println("starting return a=" + a);
      return a;
    } catch(Exception e) {

    } finally {
      a = "finally";
      System.out.println("runing finally a=" + a);
    }
    return a;
  }
}
```
返回结果:

``` plain
starting return a=return
runing finally a=finally
main a=return
```
从程序运行的结果可以知道：

* finally中的代码在return以后执行。
* 方法的返回值会另外做存储，在执行完finally后使用，因此在finally中给a赋值不会影响函数的返回值。
* 需要指出的是finally中不能使用return语句。

### `short s1 = 1; s1 = s1 + 1;`有什么错? `short s1 = 1; s1 += 1;`有什么错?
* 对于s`hort s1 = 1; s1 = s1 + 1;`由于s1+1运算时会自动提升表达式的类型，所以结果是int型，再赋值给short 类型s1时，编译器将报告需要强制转换类型的错误。
* 对于`short s1 = 1; s1 += 1;`由于+=是java语言规定的运算符，java编译器会对它进行特殊处理它等价于`s1 = (short)(s1 + 1)`，因此可以正确编译。


### &和&&的区别

相同点：

* &和&&都可以用作逻辑与的运算符，表示逻辑与(and)，当运算符两边的表达式的结果都为true时，整个运算结果才为true，否则，只要有一方为false，则结果为false。

不同点：

* &&还具有短路的功能，即如果第一个表达式为false，则不再计算第二个表达式，例如，对于if(str != null&& !str.equals(""))表达式，当str 为null 时，后面的表达式不会执行，所以不会出现NullPointerException 如果将&&改为&，则会抛出NullPointerException 异常。If(x==33 &++y>0) y 会增长，If(x==33 && ++y>0)不会增长。
* &还可以用作位运算符，当&操作符两边的表达式不是boolean 类型时，&表示按位与操作，我们通常使用0x0f来与一个整数进行&运算，来获取该整数的最低4个bit位，例如，0x31 &0x0f的结果为0x01。

### Overload和Override的区别?
* Overload是重载的意思。重载Overload表示同一个类中可以有多个名称相同的方法，但这些方法的参数列表各不相同（即参数个数或类型不同）。
* Override是覆盖的意思，也就是重写。重写Override表示子类中的方法可以与父类中的某个方法的名称和参数完全相同，通过子类创建的实例对象调用这个方法时，将调用子类中的定义方法，这相当于把父类中定义的那个完全相同的方法给覆盖了，这也是面向对象编程的多态性的一种表现。

子类覆盖父类的方法时，只能比父类抛出更少的异常，或者是抛出父类抛出的异常的子异常，因为子类可以解决父类的一些问题，不能比父类有更多的问题。子类方法的访问权限只能比父类的更大，不能更小。

如果父类的方法是private类型，那么，子类则不存在覆盖的限制，相当于子类中增加了一个全新的方法。

### 如果两个方法的参数列表完全一样，是否可以让它们的返回值不同来实现重载Overload。
不行。
因为我们有时候调用一个方法时也可以不定义返回结果变量，即不要关心其返回结果。
例如，我们调用map.remove(key)方法时，虽然remove方法有返回值，但是我们通常都不会定义接收返回结果的变量，这时候假设该类中有两个名称和参数列表完全相同的方法，仅仅是返回类型不同，java就无法确定编程者倒底是想调用哪个方法了，因为它无法通过返回结果类型来判断。

### 构造器Constructor是否可被override?
构造器Constructor不能被继承，因此不能重写Override，但可以被重载Overload。










## String相关

### 下面的代码创建了几个String Object?二者之间有什么区别?

``` java
String s = new String("xyz");
```

两个或一个，"xyz"对应一个对象，这个对象放在字符串常量缓冲区，常量"xyz"不管出现多少遍，都是缓冲区中的那一个。
new String 每写一遍，就创建一个新的对象，它用常量"xyz"对象的内容来创建出一个新String对象。
如果以前就用过"xyz"，就不会再创建了，会直接从缓冲区拿。

### 下面这条语句一共创建了多少个对象?

``` java
String s = "a" + "b" + "c" + "d";
```

一个对象。
java编译器会对它进行优化，上面的代码相当与`String s = "abcd";`

``` java
String name = "name";
String s = "your " + " full " + name + " is" + " good!";
```

上述代码创建了4个对象，因为第二行代码会优化为`String s = "your full " + name + " is" + " good!";`

### 这下面两行代码执行后，原始的String对象中的内容到底变了没有？

``` java
String s = "Hello";
s = s + "World!";
```

没有。因为String被设计成不可变(immutable)类，所以它的所有对象都是不可变对象。
在这段代码中，s原先指向一个String对象，内容是"Hello"，然后我们对s进行了+操作，那么s所指向的那个对象是否发生了改变呢？
答案是没有。这时，s不指向原来那个对象了，而指向了另一个String对象，内容为"Hello world!"，原来那个对象还存在于内存之中，只是s这个引用变量不再指向它了。

### char型变量中能不能存贮一个中文汉字?为什么?
char型变量是用来存储Unicode编码的字符的，unicode编码字符集中包含了汉字，所以char型变量可以存储汉字。
补充说明：unicode编码占用两个字节，所以，char类型的变量也是占用两个字节。这个不同于c/c++，只占用一个字节。

### 为什么Java中的密码优先使用 char[] 而不是String？
String是不可变的。这意味着一旦创建了字符串，如果另一个进程可以进行内存转储，在GC发生前，（除了反射）没有方法可以清除字符串数据。
使用数组操作完之后，可以显式地清除数据：可以给数组赋任何值，密码也不会存在系统中，甚至垃圾回收之前也是如此。




## java SE API相关

### super.getClass()方法调用

``` java
import java.util.Date;

public class GetClassTest {
  public static void main (String[] args) {
      new GetClassTest().test();
  }

  public void test() {
      System.out.println(super.getClass().getName());
  }
}

```

输出结果：

``` plain
GetClassTest
```
由于getClass()在Object类中定义成了final，子类不能覆盖该方法，所以在test方法中调用getClass().getName()方法，其实就是在调用从父类继承的getClass()方法等效于调用super.getClass().getName()方法，所以，super.getClass().getName()方法返回的也应该是GetClassTest;

如果想得到父类的名称，应该用如下代码:
getClass().getSuperClass().getName();

### `Math.round(11.5)`等于多少?`Math.round(-11.5)`等于多少？
Math.round()表示“四舍五入”，算法为Math.floor(x+0.5)，即将原来的数字加上0.5后再向下取整，所以，Math.round(11.5)的结果为12，Math.round(-11.5)的结果为-11。

### 下面代码的输出结果
``` java
public class Test {
  public static void main (String[] args) {
    System.out.println(Math.min(Double.MIN_VALUE, 0.0d));
  }
}
```

输出：

``` plain
0.0
```

Double.MIN_VALUE = 4.9E-324D是所有double中数值最小的。Double.MIN_VALUE > 0;

### 1.0 / 0 会输出什么？会异常？编译错误？
不会抛出ArithmeticException而是返回Double.INFINITY

### 1 / 0 会输出什么？会异常？编译错误？
会抛出"ArithmeticException / by zero"

### 2 / 3 会输出什么？会异常？编译错误？
输出`0`，整数相除，返回的数也是整数。相除结果是小数，则只返回整数部分

### 2 / 3f 会输出什么？会异常？编译错误？
输出`0.6666667`, 2会先转换成float再进行运行。

### 数组有没有length()这个方法? String有没有length()这个方法
数组没有length()这个方法，有length的属性。String有有length()这个方法

### Collection和Collections的区别
* Collection是集合类的上级接口，继承与他的接口主要有Set和List。
* Collections是针对集合类的一个帮助类，他提供一系列静态方法实现对各种集合的搜索、排序、线程安全化等操作。

### List，Set，Map是否继承自Collection接口
List，Set是;

Map不是

### List、Map、Set 三个接口，存取元素时，各有什么特点?

相同点：

* List和Set都是单列元素的集合，有共同的父接口Collection。

不同点：

* Set 里面不允许有重复的元素，所谓重复，即不能有两个相等(注意， 不是仅仅是相同)的对象。
* Set 取元素时，没法说取第几个，只能以 Iterator 接口取得所 有的元素，再逐一遍历各个元素。
* List 表示有先后顺序的集合，可以有重复元素，可以调用 get(index i)来明确取第几个。


### Vector和ArrayList、LinkedList区别

相同点：

* 继承了List接口（List接口继承了Collection接口）
* 是有序集合
* 可以按位子索引号取出某个元素
* 其中的数据是允许重复的

不同点：

* LinkedList内部以链表形式存储数据；ArrayList内部以数组形式存储数据；Vector同ArrayList，不过它与ArrayList比较起来是线程安全(thread-safe)的。
* Vector增长原来的一倍，ArrayList增加原来的0.5倍；Vector(setSize())可以设置空间的大小，而ArrayList不行。

### Hashtable和HashMap之间的区别

相同点：

* HashMap是Hashtable的轻量级实现（非线程安全的实现），他们都完成了Map接口

不同点：

* HashMap允许空（null）键值（key）；HashTable不允许，Value的值也不能为null
* HashMap在单线程下，效率高
* HashMap没有contains方法，有containsKey()，containsValue()，方法；HashTable三个都有
* HashTable实现了Dictionary类，也继承Map接口；HashMap只继承了Map接口。

###String，StringBuffer，StringBuilder之间区别

相同点：

* 用来存储和操作字符串

不同点：

* String提供的是数值不可改变的字符串，其它两个是可改变的，StringBuffer是线程安全的，而StringBuilder不是线程安全的。
* String实现了equals方法，其它两个没有。new String("abc").equals(newString("abc")的结果为true，而StringBuffer没有实现equals方法，所以，new StringBuffer("abc").equals(newStringBuffer("abc")的结果为false。



## 代码编写

### 什么时候用assert?
assertion(断言)在软件开发中是一种常用的调试方式，很多开发语言中都支持这种机制。在实现中，assertion就是在程序中的一条语句，它对一个boolean表达式进行检查，一个正确程序必须保证这个boolean表达式的值为true；如果该值为false，说明程序已经处于不正确的状态下，系统将给出警告或退出。一般来说，assertion用于保证程序最基本、关键的正确性。assertion检查通常在开发和测试时开启。为了提高性能，在软件发布后，assertion检查通常是关闭的

### 写clone()方法时，通常都有一行代码，是什么？
clone有缺省行为，super.clone();因为首先要把父类中的成员复制到位，然后才是复制自己的成员。

### 下面的代码有什么不妥之处?

``` java
if(username.equals("zxx")){}
```

username可能为null，会报空指针错误;改为"zxx".equals(username)

### 去掉一个 Vector集合中重复的元素。

``` java
HashSet set = new HashSet(vector);
```

## 代码查错

``` java
abstract class Name {
  private String name;
  public abstract boolean isStupidName(String name) {}
}
```

**错误**，abstract method必须以分号结尾，且不带花括号。

``` java
public class Something {
  void doSomething() {
    private String s = "";
    int l = s.length();
  }
}
```

**错误**，局部变量前不能放置任何访问修饰符(private，public，和protected)。final可以用来修饰局部变量

``` java
abstract class Something {
  private abstract String doSomething ();
}
```

**错误**，abstract的methods不能以private修饰。abstract的methods就是让子类implement(实现)具体细节的，怎么可以用private把abstract
method封锁起来呢?(同理，abstractmethod前不能加final)。

``` java
public class Something {
  public int addOne(final int x) {
    return ++x;
  }
}
```

**错误**，int x被修饰成final，意味着x不能在addOne method中被修改。

``` java
public class Something {
  public static void main(String[] args) {
    Other o = new Other();
    new Something().addOne(o);
  }
  public void addOne(final Other o) {
    o.i++;
  }
}

class Other {
  public int i;
}
```
**正确**，对象o没有变，变的是o指向的对象。

``` java
class Something {
  int i;
  public void doSomething() {
    System.out.println("i = "+ i);
  }
}
```
**正确**，输出的是"i = 0"。int i 属于 instant variable (实例变量，或叫成员变量)。instant variable有default value。int的default value 是0。

``` java
public int compareTo(Object o){
   Employee emp = (Employee) emp;
   return this.id - o.id;
}
```
**错误**，如果this.id是一个比较大的负数的话，会发生向下溢出的情况[详细](http://javarevisited.blogspot.sg/2011/11/how-to-override-compareto-method-in.html)

``` java
class Something {
  final int i;
  public void doSomething() {
    System.out.println("i = "+ i);
  }
}
```
**错误**，final int i是个final的instant variable(实例变量，或叫成员变量)。final的instant variable 没有 default value，必须在 constructor (构造器)结束之前被赋予一个明确的值 。可 以修改为"final int i =0;"。

``` java
public class Something {
  public static void main(String[] args) {
    Something s = new Something();
    System.out.println("s.doSomething() returns " + doSomething());
  }
  public String doSomething() {
    return "Do something ...";
  }
}
```

**错误**，static method 不能直接 call non-staticmethods。可改成`System.out.println("s.doSomething()returns " + s.doSomething());`。同理，static method 不能访问 non-static instant variable。

此处，Something 类的文件名叫 OtherThing.java

``` java
class Something {
  private static void main(String[] something_to_do) {
    System.out.println("Dosomething ...");
  }
}
```

**正确**，从来没有人说过Java的Class名字必须和其文件名相同。但public class的名字必须和文件名相同。

``` java
interface A {
  int x = 0;
}
class B {
  int x =1;
}
class C extends B implements A {
  public void pX(){
    System.out.println(x);
  }
  public static void main(String[] args) {
    new C().pX();
  }
}
```

**错误**，在编译时会发生错误(错误描述不同的JVM有不同的信息，意思就是未明确的x调用，两个x都匹配(就象在同时import java.util和java.sql两个包时直接声明Date一样)。对于父类的变量，可以用super.x来明确，而接口的属性默认隐含为public static final.所以可以通过A.x来明确。)

``` java
interface Playable {
  void play();
}
interface Bounceable {
  void play();
}
interface Rollable extends Playable, Bounceable {
  Ball ball = new Ball("PingPang");
}
class Ball implements Rollable {
  private String name;
  public String getName() {
    return name;
  }
  public Ball(String name) {
    this.name = name;
  }
  public void play() {
    ball = new Ball("Football");
    System.out.println(ball.getName());
  }
}
```

**错误**，任何在interface里声明的interface variable(接口变量，也可称成员变量)，默认为public static final。也就是说`Ball ball = new Ball("PingPang");`实际上是`public static final Ball ball = new Ball("PingPang");`。在 Ball 类的 Play()方法中，`ball = new Ball("Football");`改变了ball的reference，而这里的ball来自Rollable interface，Rollable interface 里的 ball 是 public static final 的，final的object是不能被改变reference的。因此编译器将在`ball = newBall("Football");`这里显示有错。

