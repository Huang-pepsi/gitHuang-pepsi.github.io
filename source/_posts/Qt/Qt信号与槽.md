---
title: Qt信号与槽
date: 2023-10-16 09:34:24
tags: 
  - Qt
---
1.传统写法
SIGNAL（f（int ））SLOT（g（int ））
此写法只是简单的字符串匹配，信号和槽的参数类型在字符串上必须是一致的。所以该写法不能适配typdef与namespace以及兼容类型的参数。

2.写法1的扩展
<img src="Pasted image 20230921142328.png" width=60%>

connect为qobject的函数

3.以指针形式指定
<img src="Pasted image 20230921142451.png" width=60%>

此时会有一个问题：若函数发生重载，无法指定唯一的函数
解决：使用函数指针
<img src="Pasted image 20230921142752.png" width=60%>
简化版：static_cast<(void)(&B::*)(int)>(&B::fun)