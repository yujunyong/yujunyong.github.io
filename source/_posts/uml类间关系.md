---
title: uml类间关系
date: 2016-03-21 11:10:36
tags: [uml]
---

# 泛化 Generalization (继承 inheritance)
继承指的是一个类（称为子类、子接口）继承另外一个类（称为父类、父接口）的功能，并可以增加它自己的新功能的能力，在Java中此类关系通过关键字extends明确标识
![uml class generalization](/images/uml-class-generalization.png)

# 实现 Realization
实现指的是一个class类实现interface接口（可以是多个）的功能，在Java中此类关系通过关键字implements明确标识
![uml class realization](/images/uml-class-realization.png)

# 依赖 Dependency
依赖可以理解为类A使用到了另一个类B，而这种使用关系是具有偶然性的、临时性的，非常弱的，但是B类的变化 会影响到A；比如某人要过河，需要借用一条船，此时人与船之间的关系就是依赖。

表现在代码层面就是类B作为参数被类A在某个method中使用
![uml class dependency](/images/uml-class-dependency.png)

# 关联 Association
关联体现的是两个类或者类与接口之间语义级别的一种强依赖关系，比如我和我的朋友；这种关系比依赖更强，关系一般是长期的，稳定的。双方的关系一般是平等的、关联可以是单向，也可以是双向，箭头表示依赖的方向。

表现在代码层面就是被关联类B以类属性的形式出现在关联类A中，也可能是关联类A引用了一个类型为被关联类型为B的全局变量
![uml class association](/images/uml-class-association.png)

# 聚合 Aggregation
聚合是关联关系的一个特例，他体现的是整体与部分、拥有的关系，即`has-a`的关系；此时整体与部分之间是可分离的，他们可以具有各自的生命周期，部分可以属于多个整体对象，也可以为多个整体对象共享；比如计算机与CPU、公司与员工的关系等

表现在代码层面是和关联关系一致的，只能从语义级别来区分。
![uml class aggregation](/images/uml-class-aggregation.png)

# 组合 Composition
组合也是关联关系的一种特例，他体现的是一种contains-a的关系，这种关系比聚合更强，也称为强聚合；他同样体现整体与部分间的关系，但此时整体与部分是不可分的，整体的生命周期结束也就意味着部分的生命周期结束；比如你和你的大脑

表现在代码层面是和关联关系一致的，只能从语义级别来区分。
![uml class composition](/images/uml-class-composition.png)

# 总结
* 泛化、实现这两种关系体现的是类与类或者类与接口间的纵向关系
* 依赖、关联、聚合、组合体现的是类与类、类与接口间的引用，是横向关系
* 横向关系的强弱依次为组合 > 聚合 > 关联 > 依赖
