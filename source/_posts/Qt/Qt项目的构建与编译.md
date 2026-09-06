---
title: Qt项目的构建与编译
date: 2023-09-25 15:41:22
tags:
  - Qt
---

**pro文件**：包含内容为项目整体所依赖的库，项目构建方式，项目包含的头文件和源文件等。

**qmake**：**qmake是Qt公司制造专门生成Qt专用makefile文件的项目管理工具**。因为项目编译前会调用Qt专有的moc（The Meta-Object Compiler)编译器和uic（User Interface Complier）编译器对Qt源文件进行预处理，所以需要专门的工具来生成Qt风格的makefile文件。
注：执行qmake会更新makefile文件
<img src="Pasted image 20230831143744.png" width=60%>

**MinGW**：是一套用于Windows平台的**自由开源软件开发工具集合**。包含GCC编译器用于编译C、C++文件，包含GNU Make工具用于自动化构建过程，还包含调试器（GDB）等。
**我们使用它的GNU Make工具来对makefile文件进行编译**。
<img src="Pasted image 20230925152508.png" width=60%>

**makefile文件**：当包含多个源文件时，使用gcc命令逐个去编译会导致工作量很大，所以需要一个文件包含编译指令。**这个Makefile文件包含了编译、链接等构建规则的信息，make工具根据makefile中的命令进行编译和链接**，makefile命令中包含了gcc去编译某个源文件的命令。

**makefile、GNU Make、GCC配合使用**：Makefile定义了项目的构建过程，而GNU Make执行这些构建规则以确保正确构建项目的各个部分。同时，GCC负责编译和链接源代码文件。



