---
title: string类模拟实现
date: 2022-03-05 21:14:33
tags: cpp
categories: cpp
---

# 本文是关于cpp中string类的模拟实现

## 代码结构

其中代码结构如下图所示：

![](/images/string/string类结构.png)

***

## 建立工程文件

首先我们创建出工程文件，并在自己的命名空间内定义好自己的string类(为了防止和std库中的string发生命名污染),如图所示：

![](/images/string/string类创建工程1.png)

![](/images/string/string类创建工程2.png)

其中文件string.h我们用来实现string类，而文件Test.cpp我们用来对string类的测试。

## 设置类中的成员变量

对于string类中的属性成员，因为我们要实现的是string的增删改查功能，所以必须维护三个成员，这三个成员变量如下图所示：

![](/images/string/string成员变量.png)

其中，_str维护了我们的字符串， _size记录字符串中字符的数量， _capacity记录着这个string空间的大小。

## 编写构造函数与析构函数

### 构造函数

我们不用编写默认构造和带参数的构造函数两种，我们可以通过带缺省值的构造函数来少写一个不含参数的构造函数：

![](/images/string/构造函数.png)

所以缺省值自然是一个空的字符串了

### 析构函数

释放内存空间并置指针为空：

![](/images/string/析构函数.png)

### 拷贝构造

我们不能使用编译器给我们自动提供的拷贝构造，因为自动提供的拷贝构造是浅拷贝，而我们维护的字符串是char*类型，浅拷贝只拷贝了它的地址，之后析构的时候会对这个空间析构两次从而出现错误。

对于拷贝构造，我们有两种实现方式

- 第一种是传入字符串，规规矩矩的开空间，拷贝字符串，拷贝 _size和 _capacity。

![](/images/string/传统拷贝构造.png)

- 第二种是通过复用构造函数，我们首先先将需要拷贝的类中的 _str设置为空指针，然后调用构造函数，构造出和传入的需要拷贝的string类有着相同字符串的一个类，之后再通过swap函数把 _str和构造出的类中的 _str调换，由于需要拷贝的类中的 _str 一开始被我们设置为nullptr，所以调换完不用理会它，不会产生内存泄漏。

![](/images/string/现代拷贝构造.png)

### 赋值运算符重载

有了拷贝构造，我们直接复用它去实现赋值运算符的重载：

![](/images/string/赋值运算符重载.png)

## 返回c语言类型字符串和字符数

在写完构造函数后，我们想验证构造函数的正确性，可以写一个返回c语言类型的字符串的一个函数，这样我们就能直接打印字符串了，如图所示：

![返回c字符串](/images/string/返回c字符串.png)

同样一并实现返回字符个数的函数：

![返回字符个数](/images/string/返回字符个数.png)

## 完成遍历字符串操作

有了返回字符串中字符个数的函数，我们只需要对”[]“操作符进行重载就可以用for循环去打印字符串了。

### []操作符重载

![](/images/string/[]操作符重载.png)

### 迭代器的定义

对于实现范围for和迭代器循环打印，我们只需要定义一下迭代器就好了，在string中其实就是一个指针：

![](/images/string/迭代器.png)

在提供一些begin和end函数：

![](/images/string/begin和end.png)

这样就能够实现打印循环打印字符了

## 字符串增加操作

### 实现push_back()函数

push_back(ch)作用为在字符串最后面追加一个字符ch，为了保证字符串空间够大，我们必须写一个扩容函数reserve()

#### 实现reserve()函数

reserve函数的参数是一个非负数n，意思是把string类管理的空间扩容到n字节，逻辑很好实现，首先判断传入的参数是否大于现有空间，如果不大于就直接不处理，大于就开一个新空间，把原来的字符串拷贝到新空间，把新空间的地址给到 _str:
![扩容函数](/images/string/扩容.png)

#### 实现resize()函数

既然有了reserve()函数，我们可以实现一下resize()函数。

resize()函数的作用是将字符串设置为n个字符，并用指定字符填充。

![](/images/string/resize.png)

有了扩容函数，实现push_back()就变得非常简单了:

![](/images/string/pushback.png)

### 

### 实现append()函数

append()函数的作用是在字符串末尾追加字符串，所以实现的逻辑和push_back()类似：首先检查空间是否充足，如果不够就扩容，最后把字符串放进去即可：

![](/images/string/append.png)

### +=操作符重载

实现了append()函数，对append()函数复用，就可以得到+=操作符重载的实现：

![](/images/string/+=重载.png)

### 实现insert()函数

insert函数有两种：

- 第一种是插入一个字符：
  ![](/images/string/字符插入.png)

- 第二种插入一个字符串：

  ![](/images/string/字符串插入.png)

## 字符串逻辑运算符

对于逻辑运算符，我们只需要实现小于和等于，其他的都可以通过复用实现，比如要实现大于，其实就是不小于等于。

### <运算符实现

![string小于](/images/string/string小于.png)

### ==运算符实现

![string等于](/images/string/string等于.png)

### 其余各种逻辑运算符

![](/images/string/string逻辑运算符.png)

## 字符串查找

find()函数的实现比较简单，这里不再赘述：

![](/images/string/find.png)

对于这里的查找字串函数，可以使用更高效的算法比如kmp，这里笔者偷个懒直接使用时间复杂度较高的strstr()库函数

## 实现clear()和erase()以及流插入运算符

#### clear()函数

clear函数的实现比较简单，直接在下标为0处放入'\0'，再把 _size改成0即可:

![](/images/string/stringclear.png)

#### erase()函数

erase()函数第一个参数是一个非负整数，表示从这个位置开始删除，第二个参数也是一个非负整数，表示要删除的字符数：

![](/images/string/stringerase.png)

#### 流插入运算符重载

这个比较简单，跟着模板来，遍历输出字符串就好：

![](/images/string/string流插入.png)





至此，string常用的函数我们都已经实现，本篇文章到此结束，谢谢观看！

***the end***
