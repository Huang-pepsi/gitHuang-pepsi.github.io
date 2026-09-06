---
title: Qt的Pro文件小结
date: 2023-09-17 12:40:34
tags: Qt
---
写一些Qt里pro文件相关知识，喜欢的小伙伴可以点点赞哦

![](https://picx.zhimg.com/80/v2-aefbe0e7961fbcd68111cd7aad79d013_720w.png?source=d16d100b)

QT += 表示项目使用的模块，gui表示使用Qt的GUI部分功能，network表示使用Qt的网络部分功能

greaterThan(QT_MAJOR_VERSION, 4): QT += widgets 表示若目前Qt版本大于4，则增加widgets模块

## CONFIG关键字

CONFIG += c++17表示该项目将使用 C++17 标准进行编译。

CONFIG += debug或release或release_and_debug表示指定编译配置是Debug模式还是Release模式。

CONFIG += console表示项目是一个控制台应用程序

CONFIG +=shared或static表示指定生成动态链接库（shared）还是静态链接库（static）

CONFIG += thread，支持线程，当 CONFIG 参数包含 qt 的时候，这个是默认支持的

## 向项目中添加文件

SOURCES += 、HEADERS += 、FORMS += 分别表示包含的源文件、头文件、ui文件（注意\表示换行）

TARGET=ssqqss 表示设置生成的应用程序文件名

## 目标安装路径INSTALLS

qnx: target.path = /tmp/$${TARGET}/bin: 这一行意味着如果当前目标平台是 QNX（一个实时操作系统），那么目标文件将会被安装到 /tmp/$${TARGET}/bin 目录中。其中，$${TARGET} 会被替换为你在.pro文件中指定的目标名称。

else: unix:!android: target.path = /opt/$${TARGET}/bin: 如果目标平台不是 QNX（因为上面的条件中包括了qnx），并且是一个 Unix-like 系统但不是 Android，那么目标文件将会被安装到 /opt/$${TARGET}/bin 目录中。

!isEmpty(target.path): INSTALLS += target: 这行的作用是检查 target.path 是否被设置，如果已经设置，就将目标文件添加到安装列表中。也就是说，只有当前目标路径 target.path 不为空时，才会将目标文件添加到安装列表中。

总的来说，这些行的作用是根据当前的构建平台设置了不同的目标安装路径。在不同平台上，可能会有不同的目录结构和标准，因此可以使用条件语句来根据需要进行设置。

## TEMPLATE关键字

TEMPLATE 是一个关键字，用于指定项目的类型，生成相应类型的makefile文件。它决定了项目将会生成一个应用程序（可执行文件）还是一个库文件（动态链接库或静态库）。

TEMPLATE = lib表示这个项目将会生成一个库文件（动态链接库或静态库），而不是一个可执行文件。生成lib相关的makefile文件

TEMPLATE = app表示项目是一个应用程序项目，将会生成一个可执行文件。生成app相关的makefile文件

TEMPLATE = subdirs表示项目是一个子项目集合，可以包含多个子项目，每个子项目可以是一个应用程序项目或者库项目。

TEMPLATE = app_bundle表示项目是一个 macOS 应用程序项目，将会生成一个 macOS 应用程序 bundle。

## LIBS关键字

LIBS += -lsystem_library表示链接系统库

LIBS += -L/path/to/library -lmylibrary表示链接静态或动态库

## DESTDIR关键字

DESTDIR = $$PWD/../../../Bin表示指定编译输出文件的目标目录。通常，DESTDIR 用于指定编译后生成的可执行文件、库文件等的输出路径。

- 如果没有指定 DESTDIR，默认情况下，Qt 会将编译生成的文件放在项目所在目录的一个子目录中，例如 debug 或者 release。
    
- DESTDIR 可以是一个相对路径或者绝对路径。
    
- DESTDIR 只影响编译后生成的文件的存放位置，不会影响程序的运行时行为。
    

## INCLUDEPATH/DEPENDPATH关键字

INCLUDEPATH 是一个关键字，用于指定头文件的搜索路径。

DEPENDPATH 用于指定依赖关系的搜索路径。

INCLUDEPATH 用于编译器，以确保正确包含头文件，而 DEPENDPATH 用于构建系统，以确保正确查找项目的依赖关系。在许多情况下，你需要同时使用它们，以确保构建和编译都能够顺利进行。

## DEFINES 关键字

DEFINES 是一个关键字，用于指定预处理器宏的定义。

CONFIG(release, debug|release): DEFINES += RELEASE_STATUS表示如果项目处于 release 模式下，则会定义一个名为 RELEASE_STATUS 的宏。通过使用 DEFINES，你可以在编译过程中通过预处理器来控制代码的行为，从而使得在不同情况下能够灵活地配置和定制你的项目。

## TARGET 关键字

指定生成的目标可执行文件的名称