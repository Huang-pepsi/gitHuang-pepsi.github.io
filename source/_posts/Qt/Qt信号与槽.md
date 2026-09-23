---
title: Qt信号与槽
date: 2023-10-16 09:34:24
tags: 
  - Qt
---
# 写法
SIGNAL（f（int ））SLOT（g（int ））
此写法只是简单的字符串匹配，信号和槽的参数类型在字符串上必须是一致的。所以该写法不能适配typdef与namespace以及兼容类型的参数。

## 写法1的扩展
![alt text](Pastedimage20230921142328.png)
connect为qobject的函数

## 以指针形式指定
![alt text](Pastedimage20230921142451.png)
此时会有一个问题：若函数发生重载，无法指定唯一的函数
解决：使用函数指针
![alt text](Pastedimage20230921142752.png)
简化版：static_cast<(void)(&B::*)(int)>(&B::fun)

# 信号槽背后的秘密
## 本质——观察者模式+函数指针注册表+moc代码生成

## Q_OBJECT---告诉moc处理这个类（元对象信息）
生成类似const QMetaObject Counter::staticMetaObject =
{
    ...
};
存储类名、信号名称、槽函数名称、参数类型、函数索引等
## connect函数
![alt text](image.png)
- 参数数组的打包方式：![alt text](image1.png)
- 信号索引：![alt text](image2.png)