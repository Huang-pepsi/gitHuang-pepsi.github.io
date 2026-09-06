---
title: QTitan中MVC小结
date: 2023-09-18 21:40:55
tags: Qt
---
研究了QTitan中的Grid例子，自己也写了自己的demo，写写对其中代码编写的理解。

## MVC是什么？

MVC是一种软件设计模式，用于组织和管理应用程序的代码。它代表Model-View-Controller（模型-视图-控制器）模式，是一种将应用程序分成三个主要组成部分的方法，每个部分都有不同的责任和任务。这种分层方法有助于提高应用程序的可维护性、可扩展性和可重用性。

简单来说，model相当于是一个在后台负责整理数据逻辑组织方式的一个进程，例如Tree的组织结构，model就为我们把Tree的结点内容、父子节点关系等等给串联起来。controller属于一个中转程序，它是model和view之间的一个桥梁，model有更新会通过controller处理，再通知view进行更新，之前不是很理解为什么要有这一层，因为直接model与view互动就行了呀，感觉这样太麻烦。当然，如果只是很简单的model&view感觉直接传递好像也没什么，但是当model与view互动多了，就会出现很多的问题，例如view的异常操作、view或model要与其他的类进行关联等等，这时你会发现由于model与view之间的耦合度太高导致改动很繁琐。所以呢这也突出了controller的重要性-----解耦，通过这个中转机构model与view耦合度大大下降，对用户的操作也更加简单明了，这也突出了可维护性与可扩展性。view作为与用户直接打交道的界面，能实时显示用户需要的信息，也能接受系列GUI操作。如果你想在其他的view上显示该model的内容，直接调用相应的接口就可以实现，是不是很方便？

![](https://picx.zhimg.com/80/v2-a9964761f3e2d74c5aa9147b35d20fa5_720w.png?source=d16d100b)


左图表示一个视图对应多个模型，表示用户在一个界面上查看不同模块的内容，例如打开某地图app，画面不仅有地图，还有天气等信息。右图表示一个模型对应多个视图，表示用户在多个界面查看同一模块的不同部分，例如火箭升空，地面操作人员在不同的区域查看火箭不同部分的实时信息。

## QTitan中MVC的使用小结

### 搭建MVC框架流程

QTitan中使用TreeGrid类创建一个widget作为view的载体，它可以进行一些layout操作。创建好了TreeGrid，通过调用它的setViewType(GridViewType type)就可以设置自己的view，这里将type设为TreeView，然后执行完这一行代码，QTitan为我们做了很多事（初始化），如创建自己的controller、options（视图相关的选项。关于GridViewOptions类，我通过调用它的setSelectionPolicy使View变成行单选模式，这个类控制view的一些行为。）等。

    m_Tree = new Qtitan::TreeGrid(this);//创建treeGrid
    m_Tree->setViewType(Qtitan::TreeGrid::TreeView);//设置view类型，初始化view
    m_View = m_Tree->view<Qtitan::GridTreeView>();//m_view为grid的view的指针

创建好了view，此时调用view的setmodel函数关联自己的model，执行完该函数，view会通过自己的controller对自己原有的model解绑系列信号槽并绑定新的信号槽。

    const QStringList headers({ tr("column1"), tr("column2") });
    m_Model = new TreeModel(headers);//自定义treemodel，设置表头
    m_View->options().setSelectionPolicy(GridViewOptions::RowSelection);//设置行单选
    m_View->setModel(m_model);//绑定model
    m_View->bestFit(Qtitan::FitToHeaderAndContent);//设置自适应数据调整表格大小

到这里，一套MVC就算搭建完成了，后面可以设置具体的model内容与view风格了。

### 关于TreeModel（继承QAbstractItemModel）

QAbstractItemModel是一个数据模型的基类，它定义了数据的组织和访问方式。它是一个抽象类，提供了一些接口与信号槽，通过继承它去实现自己需要的一些功能，这里的TreeModel就是继承QAbstractItemModel实现基本接口并扩展了部分功能。

Model的分类：

model有list model、table model与tree model三种类型，list model适合显示具有列表结构的数据集，table model适合显示表格结构的数据集，tree model适合显示树状结构的数据集。其中list model与table model可以看作是特殊的tree model。

![](https://picx.zhimg.com/80/v2-49b8b67df194bd1e8a12a2c7d7fe8ec7_720w.png?source=d16d100b)


QModelIndex类：

可以看出，若要找到model中某个cell或者item，则需要索引查找，那么就有了QModelIndex，这个索引类提供了行和列的数据，它还在内部定义了一个指针，指向它所属的模型对象，对于list model和table model可以通过行列用来定位item或者cell。但是对于tree model，只知道行列的信息还不能进行定位，此时还需要传递它的父节点的索引。这样，每个最小的单元就会对应一个ModelIndex。Model就可以根据ModelIndex来显示或者设置数据了。

TreeItem类：

Model的每一行可以看作一个item，对于每一个item，可以自定义TreeItem类，例如Qt里的QTreeWidgetItem类就是自定义的单独类，item存放自己的数据以及父子节点关系。

需注意的是初始化TreeModel时在其构造函数是需要创造自己的rootItem的，该item存放model的表头信息，QTitan通过系列调用最后识别这个rootitem的表头信息创建表头。

### 关于TreeView（继承自ViewBase）

View类创造出直接与用户交互的界面，它负责设计界面的框架（如Panel、Filter、Chooser、Menu、Zoom、stroller等）、接受用户的系列操作（单击、双击、拖动、选择多行）等功能，不负责每个数据项的绘制。

与它相关的类有ViewOptions、ColumnBase等。ViewOptions类用于设置界面的风格、状态等，例如行的多选与单选、界面是否可放大等，它继承自ViewOptionsAbstract，该类绑定自己的View。ColumnBase类用于设置列的系列参数和状态，如表头信息、是否可拖动、大小等，该类绑定自己的View。

         Qtitan::GridTableColumn* column = (Qtitan::GridTableColumn *)View->getColumnByModelColumnName(tr("索引"));//可通过列名得到列的指针
         m_View->tableOptions().setRowsQuickSelection(true);//设置checkbox
         m_View->options().setSelectionPolicy(GridViewOptions::MultiRowSelection);//设置行多选

### 关于ModelController

Controller作为中间层扮演着很重要的的角色。它的职能有很多，包括对view的系列操作的识别和传递信息，对于model的变化的信息传递，对于异常和错误的处理，若没有这个中间层则可能导致model与view交互的众多问题。

ModelController类主要控制着与model交互的信息，如行列的增删、焦点设置、数据的排列方式等，View类里很多的操作函数底层都是运用相应的Controller进行操作。相关联的类有GridModelDataBinding、GridSelection、GridFilter、GridPersistentRow等。

GridModelDataBinding类

此类维护一个列的数据（列号、列名、Role），唯一绑定一个控制器，也可设置数据，但还是用其控制器执行操作。控制器里用一个QVector容器存放所有列的信息。

GridFilter类

此类可执行信息的验证、过滤信息等操作，唯一绑定一个控制器， 控制器类里有m_filter，m_relationFilter，m_findFilter这几个私有成员变量。

GridSummary类

用于统计列的数据，平均值、和、最大最小值等。