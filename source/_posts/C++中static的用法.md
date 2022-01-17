title: C++中static的用法
date: 2019-09-24 07:47:00
categories: 技术
tags: [C++]
---

### static作用

  * 存储在<a href="https://sudo.plus/index.php/archives/112/#menu_index_4" target="_blank">**静态存储区**</a>，程序执行之前就已分配内存，程序执行结束后才销毁
  * 限制作用域在**本文件**
  * 若没有初始化，则被默认初始化为**0**

<!-- more -->

###  静态全局变量
  * 在全局变量前加上 static 关键字，即为静态全局变量
  * 静态全局变量**仅在该文件内可见**，从变量定义处开始直到文件结束， 而在其他文件中**不可见**

###  静态局部变量
  * 在局部变量前加上 static 关键字，即为静态局部变量
  * 静态局部变量**只在首次执行到声明处初始化一次**，之后再执行该语句时**不再初始化**
  * 作用域与局部变量的作用域一样

```c
#include <stdio.h>
void Func() {
  static int x = 0;
  // |x| is initialized only once across five calls of |Func| and the variable
  // will get incremented five times after these calls. The final value of |x|
  // will be 5.
  x++;
  printf("%d\n", x);  // outputs the value of |x|
}

int main() {
  Func();  // prints 1
  Func();  // prints 2
  Func();  // prints 3
  Func();  // prints 4
  Func();  // prints 5

  return 0;
}
```
  输出   
  ```c
  1
  2
  3
  4
  5
  ```

###  静态函数
  * 在普通函数的返回类型前加上 static 关键字，即为静态函数
  * 只能在**本文件**中使用

###  静态数据成员
  * 在类内数据成员前加上 static 关键字，即为静态数据成员
  * 静态数据成员，无论有多少个该类的对象，该静态数据成员在**内存中只有一份拷贝**，该静态数据成员由所有该类对象共享
  * 静态数据成员不能在类中定义和初始化，只能在类中声明，在**类外进行定义和初始化**
  * 静态数据成员的初始化为**<类型名> <类名>::<变量名> = <值>**
  * 静态数据成员遵从 public private protected 访问规则
  * 静态数据成员可以直接使用类名加作用域运算符(::)直接访问 **<类名>::<变量名>**(访问规则允许的情况下)
###  静态成员函数
  * 在普通类成员函数前加上 static 关键字，即为静态成员函数
  * 在类外定义静态成员函数时，不用再加 static 关键字，只要在类中声明时加上即可
  * **静态成员函数只能访问静态数据成员和静态成员函数，普通成员函数可以访问静态成员函数和静态数据成员**
  * 静态成员函数属于类，**不属于任意一个类对象**
  * 静态成员函数**没有 this 指针**
  * 可以使用 **<类名>::<函数名>** 访问，也可由**类对象使用(./->)**访问

示例:

```c
class A{
    public:
        static int x;    // 声明一个静态成员变量 
        // static int x = 10;    // error 声明一个静态成员变量 
        static void fun(); // 声明一个静态成员函数
        
        int y1 = 10;
        int y2;
        
};
// int A::x = 10; // 定义静态成员变量并初始化
int A::x; // 默认初始化为0，必须定义静态数据成员，不然会报错，在类内只能是声明
void A::fun(){
    cout<<x<<endl;
    // cout<<y1<<endl;  error 不能访问非静态成员函数
    // cout<<y2<<endl;
}
int main()
{
    A a;   // 创建一个 类 A 对象 
    A *b = &a;
    cout<<A::x<<'\t'<<a.x<<'\t'<<b->x<<endl;  // 访问静态数据成员的方式 A::x  / a.x  / b->x
    A::fun();    // 访问静态成员函数的方式 A::fun()  / a.fun()
    a.fun();
    b->fun();   // b->fun()
    return 0;
}   
```