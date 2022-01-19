title: C++中的foreach总结
date: 2019-09-12 02:51:00
categories: 技术
tags: [STL,boost]
---------------------


遍历是写代码时的常用操作，但使用标准for遍历有时是一件很麻烦的事。本文总结一下C++中的一些**foreach**用法，包括：
* C++11之后的**Range-based for loop**
* STL中的**std::for_each**
* Boost中的**BOOST_FOREACH**
<!--more-->
#### for
C++11增加了range-for用法，简化了遍历写法
```C
#include <iostream>
#include <vector>
using namespace std;

int main()
{
	vector<int> vec{ 1,2,3,4,5 };
   //C++11前
	for (vector<int>::iterator i = vec.begin(); i != vec.end(); ++i) 
	{
	    cout << *i <<" ";
	}
	
	cout << endl;
	
	//C++11后
	for(int &i : vec)
	{
	    cout << i << " ";
	}
	
	
	return 0;
}
```
#### std::for_each
for_each是C++标准库中的算法，C++11前即可使用。
使用for_each需要 `#include <algorithm>`
```C
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

void print(int i)
{
    cout << i << " ";
}

int main()
{
	vector<int> vec{ 1,2,3,4,5 };
	//C++11前
	for_each (vec.begin(),vec.end(),print);
    cout << endl;
    for_each (begin(vec), end(vec),print);
    cout << endl;
    //C++11后，匿名函数写法
    for_each(begin(vec), end(vec), [](int num) {cout << num << " "; });
	cout << endl;
 
	for_each(vec.begin(), vec.end(), [](int num) {cout << num << " "; });	
	cout << endl;

	return 0;
}
```
#### BOOST_FOREACH
BOOST_FOREACH带来了一种简单有效的遍历方式，支持遍历所有被Boost.Range识别为序列类型的对象。   
BOOST_FOREACH定义在 **boost/foreach.hpp** 中，boost还提供了逆序遍历**BOOST_REVERSE_FOREACH**。
特点：
* 支持所有被Boost.Range识别为序列类型的对象，如容器、数组、字符串等
* 支持return/continue/break控制
* 支持传入引用参数
* 和for一样，如果循环体只有一行，大括号可以省略
* 支持反向遍历

```c
#include <iostream>
#include <vector>
#include <boost/foreach.hpp>
using namespace std;

int main()
{
	vector<int> vec{ 1,2,3,4,5 };
	BOOST_FOREACH(int num, vec) // 也可以将 int num 定义写在外边
	{
		cout << num << " ";
	}
	cout << endl;

	BOOST_REVERSE_FOREACH(int& num, vec) // 逆序
		cout << num << " ";

	cout << endl;
	return 0;
}
```

如果想写得更简单一点，可以使用以下官方推荐的宏替换：

```c
#define foreach_         BOOST_FOREACH
#define foreach_r_       BOOST_REVERSE_FOREACH
```
官方推荐不要定义为***foreach***，以免发生命名冲突。
#### 总结
* 如果使用C++11以后的标准，range-for用法不失为最佳选择
* 如果使用C++11以前的标准，可以选择**BOOST_FOREACH**
* 如果不使用boost,也可以选择STL的**for_each**算法

#### 参考
> <a href="https://en.cppreference.com/w/cpp/language/range-for" target="_blank">range-for介绍</a>

><a href="https://www.boost.org/doc/libs/1_71_0/doc/html/foreach.html" target="_blank">BOOST_FOREACH介绍 </a>

><a href="https://en.cppreference.com/w/cpp/algorithm/for_each" target="_blank">STL for_each介绍 </a>



