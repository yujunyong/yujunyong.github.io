---
title: java中方法继承详解
date: 2016-03-25 16:08:04
tags: [java, 多态]
---
# java中的方法绑定
连接一个方法调用到对应的方法体称为绑定。java中有**预绑定**和**动态绑定**两种方式。预绑定在性能上并不优与延迟绑定

## 预绑定 early binding
预绑定，也可以说静态绑定，发生在编译时，用`static`, `final`(`private`是隐式的`final`)修饰的方法是预绑定的。

## 延迟绑定 late binding
延迟绑定，也可以说是动态绑定，运行时绑定，发生在运行时，java中除预绑定的方法外都是延迟绑定。

# 继承时有同名的方法
## 继承的两个接口中有签名相同的的方法
### 两个方法的返回值相同
``` java
public class InterfaceTest {
  interface Gift  { void present(); }
  interface Guest { void present(); }

  interface Presentable extends Gift, Guest { }

  public static void main(String[] args) {
    Presentable johnny = new Presentable() {
      @Override public void present() {
        System.out.println("Heeeereee's Johnny!!!");
      }
    };
    johnny.present();                     // "Heeeereee's Johnny!!!"

    ((Gift) johnny).present();            // "Heeeereee's Johnny!!!"
    ((Guest) johnny).present();           // "Heeeereee's Johnny!!!"

    Gift johnnyAsGift = (Gift) johnny;
    johnnyAsGift.present();               // "Heeeereee's Johnny!!!"

    Guest johnnyAsGuest = (Guest) johnny;
    johnnyAsGuest.present();              // "Heeeereee's Johnny!!!"
  }
}
```
上面的`present`方法在两个父接口中完全相同，子接口Presentable可以认为同时继承了两个方法（因为两个方法完全相同），所以代码可以正常执行。

如果继承的一个类和一个接口中有完全相同的方法present，则子类中present方法默认实现就是父类中的。

可以用java的延迟绑定机制解释这个原因：
1. 父类和父接口中的present方法完全相同，所以编译时和两个都是接口的情况相同
2. 程序运行后当调用方法present，此时才会为这个方法绑定对应的方法体，也就是继承自父类的方法体

### 两个方法的返回值不同
``` java
public class InterfaceTest {
  interface Gift  { void present(); }
  interface Guest { boolean present(); }

  interface Presentable extends Gift, Guest { } // DOES NOT COMPILE!!!
  // "types InterfaceTest.Guest and InterfaceTest.Gift are incompatible;
  //  both define present(), but with unrelated return types"
}
```
Gift中的present方法返回void, Guest中的present方法返回boolean，当Presentable继承Gift和Guest时编译不通过。两个父类中的present方法的继承优先级是相同的(继承关系的判断不包含方法的返回类型)，但它们的返回类型不同，子类就不知道应该继承哪个方法了，所以会出现编译错误。

## 继承的方法是default方法
### 继承自2个接口的方法都是default方法
``` java
public interface Jukebox {
  public default String rock() {
    return "... all over the world!";
  }
}
```
---
``` java
public interface Carriage {
  public default String rock() {
    return "... from side to side";
  }
}
```
---
``` java
public class MusicalCarriage implements Carriage, Jukebox {} // DOES NOT COMPILE!!!
// class MusicalCarriage inherits unrelated defaults
// for rock() from types Carriage and Jukebox
```
上面的MusicalCarriageo类编译不通过，因为两个default方法的继承优先级相同，编译器在处理的时候不知道应该继承哪个default方法中的方法体，所以出现编译错误。

既然是不知道选择哪个方法体引起的错误，那么提供一个优先级高的方法体就可以解决这个错误，代码如下：
``` java
public class MusicalCarriage implements Carriage, Jukebox {
  @Override
  public String rock() {
    return Carriage.super.rock();
  }
}
```
### 继承的是一个接口，一个类
``` java
public class Jukebox {
  public String rock() {
    return "... all over the world!";
  }
}
```
---
``` java
public class MusicalCarriage extends Jukebox implements Carriage {
  public static void main(String[] args) {
    Carriage c = new MusicalCarriage();
    System.out.println(c.rock());   // ... all over the world!

    Jukebox j = new MusicalCarriage();
    System.out.println(j.rock());   // ... all over the world!

    MusicalCarriage m = new MusicalCarriage();
    System.out.println(m.rock());   // ... all over the world!
  }
}
```
将上面的接口Jukebox改成了一个类，则代码编译通过。因为在继承时类的优先级要比接口中的default方法高。
