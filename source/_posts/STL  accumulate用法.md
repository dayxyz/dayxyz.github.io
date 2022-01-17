title: STL  accumulate用法
date: 2019-09-05 03:05:00
categories: 技术
tags: [C++,STL]
---
 目录
<!-- index-menu -->
### 介绍

#### 函数原型
函数有两个重载模板
```c
1. template< class InputIt, class T >
T accumulate( InputIt first, InputIt last, T init ); 

2. template< class InputIt, class T, class BinaryOperation >
T accumulate( InputIt first, InputIt last, T init,
              BinaryOperation op );
```
#### 作用
1. 模板1用来计算初始值init和特定范围[first, last)内所有元素的和
2. 模板2用来计算初始值init和特定范围[first, last)内所有元素的指定操作的结果，如相减、相乘等

#### 参数
first, last ：计算的范围是[first, last）
init		：计算的初始值
op			：要应用的计算规则，自定义的计算需要有 `Ret fun(const Type1 &a, const Type2 &b);`的原型，其中const不是必须的。可以通过自定义函数或重载对象的`()`运算符来实现这个操作。

举例:
```C
// accumulate example
#include <iostream>     // std::cout
#include <functional>   // std::minus
#include <numeric>      // std::accumulate
#include <vector>      // std::vector
#include <string>      // std::string

using std::cout;
using std::endl;

int myfunction(int x, int y){ return x + 2 * y; }
struct myclass {
	int operator()(int x, int y) { return x + 3 * y; }
} myobject;

int main() {
	int init = 1;
	int numbers[] = {1,2};
	
//计算1+1+2
	cout << "using default accumulate: ";
	cout << std::accumulate(numbers, numbers + 2, init) << endl;
	
//计算1-1-2
	cout << "using functional's minus: ";
	cout << std::accumulate(numbers, numbers + 2, init, std::minus<int>()) << endl;
	
//计算（1+1*2）+2*2
	cout << "using custom function: ";
	cout << std::accumulate(numbers, numbers + 2, init, myfunction) << endl;
	
//计算（1+1*3）+2*3
	cout << "using custom object: ";
	cout << std::accumulate(numbers, numbers + 2, init, myobject) << endl;
	
//计算1+1+2
	cout << "using functional's plus: ";
	cout << std::accumulate(numbers, numbers + 2, init, std::plus<int>()) << endl;
//计算1*1*2
	cout << "using functional's multiplies: ";
	cout << std::accumulate(numbers, numbers + 2, init, std::multiplies<int>()) << endl;


	std::vector<std::string> str{"Hello ","world","!"};
	cout << std::accumulate(str.begin(), str.end(), std::string("")) << endl;

	return 0;
}
```
输出：
```C
using default accumulate: 4
using functional's minus: -2
using custom function: 7
using custom object: 10
using functional's plus: 4
using functional's multiplies: 2
Hello world!
```

`<functional>`中实现了很多运算操作
加：plus<T>
减：minus<T>
乘：multiplies<T>
除：divides<T>
模：modulus<T>
否定：negate<T>

#### 参考
> <a href="https://en.cppreference.com/w/cpp/algorithm/accumulate" target="_blank">cppreference.com </a>

