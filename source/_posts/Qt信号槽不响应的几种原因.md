title: Qt信号槽不响应的几种原因
date: 2019-08-08 07:33:00
categories: 技术
tags: [Qt]
---
#### 用Qt进行信号与信号槽连接后，会出现信号槽不响应信号的情况，原因可能是以下的情况：

1. 类没有声明**`Q_OBJECT`**;

2. 槽函数没有定义为**`pubic/private slots`**;

3. 信号和槽之间存在参数传递，但是二者的参数数量或者类型不一致（信号里的参数数量可以多于槽函数里的参数数量，但是二者都有的参数，类型必须对应）

4. 事件被子控件过滤掉了。比如**`QListWidget`**,当**QListWidgetItem**已经处理**keypress**事件后，**QListWidget**就不能响应**itemDoubleClicked**事件了

5. 信号槽的参数是自定义的，这时需要用**`qRegisterMetaType`**注册一下这种类型