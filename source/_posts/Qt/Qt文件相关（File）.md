---
title: Qt文件相关（File）
date: 2023-11-12 13:57:19
tags:
  - Qt
---
1.QFile File(`QDir::currentPath()+"/../Config/IDExpert.config"`);
使用绝对路径或相对路径(注意当前路径为exe文件路径还是该文件路径)
- Qt对于`/`和`\`都可以识别

2.使用QDomDocument读取xml文件的两种方式
![](Pastedimage20231107135135.png)
![](Pastedimage20231107135147.png)
3.用户选择特定文件QFileDialog
QString QFileDialog::**getOpenFileName**(
QWidget *parent = nullptr, 
const QString &caption = QString(),
const QString &dir = QString(), 
const QString &filter = QString(),
QString *selectedFilter = nullptr, 
Options options = 0);

4.QDomElement继承QDomNode
元素、文本、注释三者有所区别
![](Pastedimage20231107152756.png)
![](Pastedimage20231107152020.png)为元素结点
![](Pastedimage20231107152037.png)为文本节点
![](Pastedimage20231107152807.png)为注释结点

5.例：`<span id="hh">文本节点</span>`
![](Pastedimage20231122142711.png)
结果：![](Pastedimage20231122142722.png)

可看出：
	- 元素结点可以添加文本节点，结果是为自己的中间的文本赋值
