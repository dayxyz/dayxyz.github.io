title: STL常用算法
date: 2019-09-05 05:09:00
categories: 技术
tags: [C++,STL]
---

### 非修改序列操作
`adjacent_find`   查找两个相邻（Adjacent）的等价（Identical）元素
`all_of` (C++11)  检测在给定范围中是否所有元素都满足给定的条件
`any_of` (C++11)  检测在给定范围中是否存在元素满足给定条件
`count`   返回值等价于给定值的元素的个数
`count_if`    返回值满足给定条件的元素的个数
`equal`   返回两个范围是否相等
`find`    返回第一个值等价于给定值的元素
`find_end`    查找范围 A 中与范围 B 等价的子范围最后出现的位置
`find_first_of`   查找范围 A 中第一个与范围 B 中任一元素等价的元素的位置
`find_if` 返回第一个值满足给定条件的元素
`find_if_not` (C++11) 返回第一个值不满足给定条件的元素
`for_each`    对范围中的每个元素调用指定函数
`mismatch`    返回两个范围中第一个元素不等价的位置
`none_of` (C++11) 检测在给定范围中是否不存在元素满足给定的条件
`search`  在范围 A 中查找第一个与范围 B 等价的子范围的位置
`search_n`    在给定范围中查找第一个连续 n 个元素都等价于给定值的子范围的位置
<!-- more -->
```C
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
int main()
{
	vector<int> ivec{ 1,2,2,4,5,6,7,8,9,10 };
	vector<int> v{ 5,6,7,8,9,10 };
	//判断容器内是否所有元素都满足条件
	if (all_of(ivec.begin(), ivec.end(), [](int i) {return i < 20; }))
		cout << "容器内所有元素都满足条件" << endl;
	//判断容器内是否有元素满足条件
	if (any_of(ivec.begin(), ivec.end(), [](int i) {return i > 9; }))
		cout << "容器内有任意一个元素满足条件" << endl;
	//判断容器内是否所有的元素都不满足条件
	if (none_of(ivec.begin(), ivec.end(), [](int i) {return i > 10; }))
		cout << "容器内所有元素都不满足条件" << endl;
	//遍历容器内元素
	for_each(ivec.begin(), ivec.end(), [](const int& n) { cout << n << " "; });
	cout << endl << "返回满足条件的元素个数：" << endl;
	cout << count(ivec.begin(), ivec.end(), 5) << endl;
	cout << count_if(ivec.begin(), ivec.end(), [](int i) {return i < 4; }) << endl;
	cout << "返回一个pair对象，first为第一个不匹配元素的迭代器，second为第一个匹配的元素的迭代器：" << endl;
	cout << *mismatch(ivec.begin(), ivec.end(), v.begin(), v.end()).first << endl;
	cout << *mismatch(ivec.begin(), ivec.end(), v.begin(), v.end()).second << endl;
	cout << "返回满足条件的第一个元素的迭代器：" << endl;
	cout << *find(ivec.begin(), ivec.end(), 5) << endl;
	cout << *find_if(ivec.begin(), ivec.end(), [](int i) {return i > 5; }) << endl;
	cout << "返回不满足条件的第一个元素的迭代器" << endl;
	cout << *find_if_not(ivec.begin(), ivec.end(), [](int i) {return i < 5; }) << endl;
	cout << "如果后一个区间是前一个区间的子区间，则返回后一个区间的起始迭代器，否则返回第一个区间的末尾迭代器：" << endl;
	cout << *find_end(ivec.begin(), ivec.end(), v.begin(), v.end()) << endl;
	cout << "返回两个区间首个相等元素的迭代器，如果没有则返回第一个区间的末端迭代器：" << endl;
	cout << *find_first_of(ivec.begin(), ivec.end(), v.begin(), v.end()) << endl;
	cout << "返回首个相邻元素相等的第一个元素的迭代器，如果没有则返回末端迭代器：" << endl;
	cout << *adjacent_find(ivec.begin(), ivec.end()) << endl;
	cout << "返回第一个相等的元素的迭代器，如果没有则返回第一个区间的末端迭代器：" << endl;
	cout << *search(ivec.begin(), ivec.end(), v.begin(), v.end()) << endl;
	cout << "返回满足条件(值为2，出现次数为2)的第一个元素的迭代器，如果没有则返回末端迭代器：" << endl;
	cout << *search_n(ivec.begin(), ivec.end(), 2, 2) << endl;
}
```



### 修改序列操作
`copy`    将一个范围中的元素拷贝到新的位置处
`copy_backward`   将一个范围中的元素按逆序拷贝到新的位置处
`copy_if` (C++11) 将一个范围中满足给定条件的元素拷贝到新的位置处
`copy_n` (C++11)  拷贝 n 个元素到新的位置处
`fill`    将一个范围的元素赋值为给定值
`fill_n`  将某个位置开始的 n 个元素赋值为给定值
`generate`    将一个函数的执行结果保存到指定范围的元素中，用于批量赋值范围中的元素
`generate_n`  将一个函数的执行结果保存到指定位置开始的 n 个元素中
`iter_swap`   交换两个迭代器（Iterator）指向的元素
`move` (C++11)    将一个范围中的元素移动到新的位置处
`move_backward` (C++11)   将一个范围中的元素按逆序移动到新的位置处
`random_shuffle`  随机打乱指定范围中的元素的位置
`remove`  将一个范围中值等价于给定值的元素删除
`remove_if`   将一个范围中值满足给定条件的元素删除
`remove_copy` 拷贝一个范围的元素，将其中值等价于给定值的元素删除
`remove_copy_if`  拷贝一个范围的元素，将其中值满足给定条件的元素删除
`replace` 将一个范围中值等价于给定值的元素赋值为新的值
`replace_copy`    拷贝一个范围的元素，将其中值等价于给定值的元素赋值为新的值
`replace_copy_if` 拷贝一个范围的元素，将其中值满足给定条件的元素赋值为新的值
`replace_if`  将一个范围中值满足给定条件的元素赋值为新的值
`reverse` 反转排序指定范围中的元素
`reverse_copy`    拷贝指定范围的反转排序结果
`rotate`  循环移动指定范围中的元素
`rotate_copy` 拷贝指定范围的循环移动结果
`shuffle` (C++11) 用指定的随机数引擎随机打乱指定范围中的元素的位置
`swap`    交换两个对象的值
`swap_ranges` 交换两个范围的元素
`transform`   对指定范围中的每个元素调用某个函数以改变元素的值
`unique`  删除指定范围中的所有连续重复元素，仅仅留下每组等值元素中的第一个元素。
`unique_copy` 拷贝指定范围的唯一化（参考上述的 unique）结果

### 划分
`is_partitioned` (C++11)  检测某个范围是否按指定谓词（Predicate）划分过
`partition`   将某个范围划分为两组
`partition_copy` (C++11)  拷贝指定范围的划分结果
`partition_point` (C++11) 返回被划分范围的划分点
`stable_partition`    稳定划分，两组元素各维持相对顺序
 
### 排序
`is_sorted` (C++11)   检测指定范围是否已排序
`is_sorted_until` (C++11) 返回最大已排序子范围
`nth_element` 部份排序指定范围中的元素，使得范围按给定位置处的元素划分
`partial_sort`    部份排序
`partial_sort_copy`   拷贝部分排序的结果
`sort`    排序（快速排序）
`stable_sort` 稳定排序

### 二分法查找（用于已划分/已排序的序列）
`binary_search`   判断范围中是否存在值等价于给定值的元素
`equal_range` 返回范围中值等于给定值的元素组成的子范围
`lower_bound` 返回指向范围中第一个值大于或等于给定值的元素的迭代器
`upper_bound` 返回指向范围中第一个值大于给定值的元素的迭代器


### 合并（用于已排序的序列）
`includes`    判断一个集合是否是另一个集合的子集
`inplace_merge`   就绪合并
`merge`   合并
`set_difference`  获得两个集合的差集
`set_intersection`    获得两个集合的交集
`set_symmetric_difference`    获得两个集合的对称差
`set_union`   获得两个集合的并集

### 堆
`is_heap` 检测给定范围是否满足堆结构
`is_heap_until` (C++11)   检测给定范围中满足堆结构的最大子范围
`make_heap`   用给定范围构造出一个堆
`pop_heap`    从一个堆中删除最大的元素
`push_heap`   向堆中增加一个元素
`sort_heap`   将满足堆结构的范围排序

### 最大/最小值
`is_permutation` (C++11)  判断一个序列是否是另一个序列的一种排序 
`max` 返回两个元素中值最大的元素
`max_element` 返回给定范围中值最大的元素
`min` 返回两个元素中值最小的元素
`min_element` 返回给定范围中值最小的元素
`minmax` (C++11)  返回两个元素中值最大及最小的元素
`minmax_element` (C++11)  返回给定范围中值最大及最小的元素

### 其他
`lexicographical_compare` 比较两个序列的字典序
`next_permutation`    返回给定范围中的元素组成的下一个按字典序的排列
`prev_permutation`    返回给定范围中的元素组成的上一个按字典序的排列

>http://www.cplusplus.com/reference/algorithm/