---
title: java内存区域
date: 2016-03-18 11:07:59
tags: [java, jvm]
---

# jvm 运行时数据区域
![jvm runtime data area](/images/jvm-runtime-data-area.jpg)

## 单线程私有
单线程私有的数据区在其所属线程创建时初始化，并随着所属线程结束被销毁

### 程序计数器(Program Counter (PC) Register)
Java中的程序计数器用来记录当前线程中正在执行的指令，如果当前正在执行的方法是本地方法，那么此刻程序计数器的值为`undefined`。**这个区域是唯一一个不抛出`OutOfMemoryError`的运行时数据区**。

### JVM栈(Java Virtual Machine Stacks)
> #### 栈帧(Stack Frame)
栈帧是用来存储数据和部分过程结果的数据结构，同时也被用来处理动态链接、方法返回值和异常分派
一个栈帧随着一个方法的调用开始创建，这个方法调用完成则销毁。

JVM栈只对栈帧进行存储，压栈和出栈操作。栈内存的大小可以有两种设置，固定值和根据线程需要动态增长。

在JVM栈这个数据区可能会抛出两种异常
* `StackOverflowError`出现在栈内存设置成固定值的时候，当程序执行需要的栈内存超过设定的固定值会抛出这个异常
* `OutOfMemoryError`出现在栈内存设置成动态增长的时候，当JVM尝试申请的内存大小超过了其可用内存时会抛出这个异常

### 本地方法栈(Native Method Stacks)
本地方法栈与虚拟机栈的作用相似，不同点在于它是为虚拟机用到的Native方法(就是非Java语言实现的方法，一般是C或者C++)服务的

抛出的异常同JVM栈

## 多线程共享
### 堆内存(Heap Memory)
堆数据用来存放对象和数组(特殊的对象)。堆内存随着JVM启动被创建，它存储了被GC（Garbage Collector 垃圾收集器）所管理的各种对象，这些对象无需，也无法显式地被销毁。

堆内存可能会抛出`OutOfMemoryError`异常

### 方法区(Method Area)
方法区中存放了每一个类的结构信息，例如运行时常量池(Runtime Constant Pool)、字段和方法数据、构造函数和普通方法的字节码内容、还包括一些在类、实例、接口初始化时用到的特殊方法。

方法区可能会抛出`OutOfMemoryError`异常

### 运行时常量池(Runtime Constant Pool)
运行时常量池是每一个类或接口的常量池的运行时表示形式，它包括了若干种不同的常量：从编译期可知的数值字面量到必须运行期解析后才能获得的方法或字段引用。
每一个运行时常量池都分配在Java虚拟机方法区之中，在类和接口被加载到虚拟机后，对应的运行时常量池才被创建出来。

可能会抛出`OutOfMemoryError`异常
