---
title: C++语法
date: 2026-10-12 13:39:06
tags: C++
---
## #pragma once
作用：禁止头文件重复包含，否则报重复定义
例：![alt text](1.png)
![alt text](2.png)
解释：预处理器看到include包含后，会把h内容展开插入到当前源文件中，该语法告诉预处理器已处理过，无需展开。
多层包含贴贴乐，有include就在这个位置贴头文件，贴过了就不贴。

## 全局变量初始化顺序
1.静态初始化【编译器】
**cpp**：
    int g_a = 5;
    static const char* g_name = "abc";

2.动态初始化【运行期，main之前】
**cpp**：
QMap<int, QString> g_map;                       // 调用 QMap 构造函数
FaultMonitor* FaultMonitor::m_instance = new FaultMonitor();   // new + 构造
static QHash<int, QSet<int>> unitcodetoclfXRAY; // 构造函数

<p style="color: rgb(223, 79, 79);">顺序规则：</p>
- 同一cpp，按书写顺序
- 不同cpp，顺序未指定，由链接顺序等决定，不能依赖

<p style="color: rgb(223, 79, 79);">由此带来的注意点：</p>
1.全局对象的构造函数里不要依赖 QApplication（比如 QCoreApplication::applicationName()、创建 widget、QPixmap、启动 QThread），此时它还不存在
2.不要依赖别的 .cpp 里的全局对象，可能还没构造
3.全局对象的析构在 main() 返回之后，顺序与构造相反

关于QApplication：
![alt text](image-3.png)

## 声明和定义
声明：有这个东西
定义：长什么样