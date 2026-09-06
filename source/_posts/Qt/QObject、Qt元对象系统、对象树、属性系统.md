---
title: QObject、Qt元对象系统、对象树、属性系统
date: 2023-11-05 22:32:03
tags:
  - Qt
---
1.RTTI机制
**由于C++是静态类型语言，有关类的信息只在编译期被使用，编译后就不再保留，因此程序运行时无法获取类的信息。这时就需要使用「运行期类型信息」，即RTTI（Run-Time Type Information）**。一般拥有程序运行时保存对象类型信息能力的语言，我们就称该语言支持RTTI。C++标准定义了dynamic_cast和typeid两个关键字用于支持RTTI机制。

dynamic_cast的作用是将指向基类对象的指针转换为指向派生类对象的指针，如果转换失败则返回NULL。因此，dynamic_cast唯一的功能就是判断一个对象具有哪些类型。例如有4个类GrandFather、Father、Son和GrandSon，右边的类分别是左边类的派生类。此时有一个类型为`GrandFather *`的指针p，为了判断p所指对象是否具有Son的类型，可以使用语句`Son *son = dynamic_cast<Son*>(p`)。如果返回的son不为NULL，则p所指的对象具有Son类型。同理我们也可以得到该对象是否具有Father、GrandSon类型，

但是仅仅知道这些是不够的。因为有时候某个指针所指的对象行为不正常，如果无法知道所指对象的实际类型，就必须审查该基类及其所有派生类的代码。反过来，如果我们能够知道该对象的实际类型，那就只需要审查该类型的代码即可。typeid的作用是返回类型的名字。因此，typeid的功能就是在dynamic_cast的范围内进一步的确定指针所指对象的实际类型。

2.元对象系统
Meta Object的所有数据和方法都封装在QMetaObject类中，它包含一个Qt类的meta信息，并且提供查询功能。

元对象系统基于三件事：
1. [QObject](https://runebook.dev/en/docs/qt/qobject) 类为可以利用元对象系统的对象提供基类。
2. 类声明的 private 部分内的 [Q_OBJECT](https://runebook.dev/en/docs/qt/qobject#Q_OBJECT) 宏用于启用元对象功能，例如动态属性、信号和插槽。
3. [元对象编译器](https://runebook.dev/en/docs/qt/moc) （） 为每个 [QObject](https://runebook.dev/en/docs/qt/qobject) 子类提供实现元对象功能所需的代码。`moc`

meta信息包含：
1. 信号表（signal table），与对应Qt类相关的系统定义及自定义的signal的名字。
2. 槽表（slot table），与对应Qt类相关的系统定义及自定义的slot的名字。
3. 类信息表（class info table），Qt类的类型信息。
4. 属性表（property table），与对应类中的所有属性的名字。
5. 指向parent meta object的指针。

Q_OBJECT宏：
moc预处理器将对**头文件**中标记了该宏的类进行处理，不会处理cpp文件中的类似声明，

3.对象树
这个概念非常好理解。因为 QObject 类就有一个私有变量 `QList<QObject *>`，专门存储这个类的子孙后代们。比如创建一个 QObject 并指定父对象时，就会把自己加入到父对象的 childre() 列表中，也就是 `QList<QObject *>` 变量中。

注意setparent细节

4.属性系统
QObject中的子类的私有域中使用Q_PROPERTY宏来声明一个属性


