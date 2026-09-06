---
title: 关于构造函数QWidget（parent）
date: 2023-11-12 14:02:28
tags:
  - Qt
---
### Widget::Widget(QWidget *parent) :QWidget(parent)
例子：
基类
<img src="Pasted image 20231110154435.png" width=60%>
子类
<img src="Pasted image 20231110154527.png" width=60%>

结果：
<img src="Pasted image 20231110154545.png" width=60%>
结论：子类构造函数指定父类的构造函数会调用父类的构造函数，若不指定，则调用父类的默认构造函数

源码：
<img src="Pasted image 20231110155342.png" width=60%>
结论：如果parent参数非空的话，那么该构造函数使用了其父窗口的调色板，并且发送了QChildEvent事件，这会让新的窗口成为parent所指窗口的子窗口，那么当父窗口被删除时，子窗口也会自动的被删除。
