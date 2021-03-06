# Review 2022.6.20

## 6.24考完期末更新

他妈的...

上机部分不能用自己的机器，学校提供的是VS2010和VC++6.0。他妈的VS2010不知道怎么回事写个std::cout都跑不起来，我只能拿VC++6.0写。说实话上古ide狗都不用，通篇cpp文件下来除了关键字是蓝色其他他妈的全是黑色，光是花在看自己代码有没有写错字都能花掉一半的时间，最后来不及还剩下半道题...

## 前言

这个文档是2022.6.20东南大学C++期末复习课摘要，主要罗列了一些必要的考点，在src部分中是对本markdown提到的知识点的代码实例，有助于扫清一部分的知识盲区（当然考完发现了有一点点的遗漏，比如virtual函数碰到两次继承之类的）


## Charpter 9

### 总览

1. 构造函数析构函数调用顺序，和其他组合关系混在一起
2. copy构造函数
3. 传值传引用
4. const
5. 友元

### 类作用域

1. this->var = var 或者标明类名 A::var = var  同名传参情况下

2. const type & var提高性能

3. 静态数据成员

   1. 所有该类的实例共享一个数据成员，一般用来记录该类有多少个对象构造函数++析构函数-- 操作
   2. 类内部声明，类外部初始化，类外部可以不赋值但是一定要声明

4. 构造 析构

   1. 全局对象 所有函数开始前 程序结束后
   2. 局部对象
      1. auto 定义时 块作用域结束时
      2. static 首次定义时 程序结束时

5. 组合关系

   1. 类的对象作为另一个类的数据成员

   2. 显性调用初始化列表初始成员对象 除非有default构造函数可以不用初始化列表
   3. 先构造成员对象再自己，先自己再析构成员对象

6. **copy构造函数**

   防止dead loop 关注何时会调用

   传值会调用 传引用不会

   值传递返回会调用，返回引用不会

   如果返回值是临时对象或者局部对象不能返回引用（虚悬引用），只能返回值

   如果成员变量有指针，则应提供copy构造函数并重载赋值运算符

   指针不应该指向同一块区域，而应该是两块区域但是内容一致

7. 静态存储区域 - 栈空间存储 - 堆空间存储

8. new & delete

   1. classname * varname = new classname[length];

      delete [] varname;	//普通的类数组

   2. classname ** varname = new classname\*[length];

      for varname[i] = new classname()

      for delete [] varname[i];	//指针数组

   3. 要自定义析构函数去delete ptrs

9. 何时用到初始化列表？4种方式ppt里面有

10. 静态成员函数

    可以视为对于一个类的服务，不能访问非静态数据成员

11. const成员函数

    1. 不能修改本对象的数据成员

    2. 不能调用其他的non-const成员函数

12. this指针级联调用 return *this  返回引用 应该不考

13. **友元类和友元函数**

    1. 友元函数可以访问该类所有数据成员

       类内声明，类外定义

       例如运算符重载

    2. 友元类

       例如链表类和节点类？！

## 自赋值和delete

dangerous



## Charpter 10 运算符重载

### 两种方式

1. 重载为成员函数

   要求运算左操作数必须是自己

   ()[]->必须

2. 重载为全局函数

   记得在类里面声明为友元函数

   参数个数 = 目数

   \>>和<<必须

### >>和<<

要定义为引用 istream&  ostream&



### 对象转换

1. 同类型

   拷贝构造函数 初始化对象

2. 非同类型

   转换构造函数

   编译器隐式转换自动调用转换构造函数

   隐式有时候很危险 声明为explicit

3. 强制类型转换符重载

   `A::operater OtherClass() const;` //不需要返回值，不需要修改原对象 A $\rightarrow$ OtherClass

   调用的时候就是`static_cast<OtherClass>(Avar);`



### 临时对象

取决于是否有句柄

*右值引用（C++11）*



## Charpter 11 继承和多态

### 多态（一定会考）

对不同的子类统一用基类指针，用基类的函数

使用方式：虚函数Virtual Function

动态绑定 两个条件



### 抽象类和纯虚函数（考）

常常结合多态

### 继承关系中的构造和析构顺序

若没有显式调用构造函数则要看有没有基类的dafault constructor

### Protected成员

仅仅在派生的时候用得到





## Charpter 14 文件

考试内容较多

#include <fstream>

### 读写操作

读istream对象

写oftream对象



### 二进制文件读写（看一下,考试不会太复杂）

相比普通文本文件（顺序文件），不需要对文件结构有了解

多给一个ios::binary

调用成员函数write和read进行写入和读取，不同于之前的<<和>>

write给一个buffer的地址和要写入的字节数，buffer可以是一个数组的地址

read同理

地址（指针）转换reinterpret_cast

sizeof()



## Charpter 17 异常处理（一定考）

栈展开以及里面的析构比较重要 

发生异常到找到catch中间部分的对象都会被析构

throw - catch

new运算符的failure



## Charpter   STL and Iters（非重点）

1. 容器和迭代器的定义和使用
2. vector list deque序列式容器
3. set multiset multimap关联式容器
4. stack queue priority_queue容器适配器



## Charpter 19-20 模板（会考）

1. 函数模板可以直接调用
2. 类模板需要先显性特化再创建对象

类模板实现动态数组，类模板允许类型实参和非类型实参

类模板成员函数的定义要和头文件写在一起！！！





## 杂项

1. 包含继承关系与成员对象的 情况下构造函数和析构函数的调用顺序
2. static数据成员和成员函数
3. new运算符的处理 对象数组的构建
4. 文件 异常处理 偏多
5. 运算符重载 string的例子，针对用户自定义类型一般要重载哪些函数
6. 临时对象
7. STL模板类的使用 入stack 栈的压入和弹出操作
8. 指针数组和二维指针
9. 文件读写（顺序和二进制） 与对象的操作
10. 异常处理和栈展开 异常处理的流程
11. 多态的实现方式 调用方式 以及编程实现
12. 临时对象的问题
