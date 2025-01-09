
**导入库：**
```cpp
include<vector>
```

**一维初始化：**
```cpp
vector<int> a; //定义了一个名为a的一维数组,数组存储int类型数据
vector<double> b;//定义了一个名为b的一维数组，数组存储double类型数据
vector<node> c;//定义了一个名为c的一维数组，数组存储结构体类型数据，node是结构体类型
```

**拷贝初始化：**
```cpp
vector<int> a(n + 1, 0);
vector<int> b(a); // 两个数组中的类型必须相同,a和b都是长度为n+1，初始值都为0的数组
vector<int> c = a; // 也是拷贝初始化,c和a是完全一样的数组
```


**方法函数：**

| 代码                             | 含义                                     |
| ------------------------------ | -------------------------------------- |
| `c.front()`                    | 返回第一个数据O ( 1 ) O(1)O(1)                |
| `c.back()`                     | 返回数组中的最后一个数据 O ( 1 ) O(1)O(1)          |
| `c.pop_back()`                 | 删除最后一个数据O ( 1 ) O(1)O(1)               |
| `c.push_back(element)`         | 在尾部加一个数据O ( 1 ) O(1)O(1)               |
| `c.size()`                     | 返回实际数据个数（unsigned类型）O ( 1 ) O(1)O(1)   |
| `c.clear()`                    | 清除元素个数O ( N ) O(N)O(N)，N为元素个数          |
| `c.resize(n, v)`               | 改变数组大小为`n`,`n`个空间数值赋为`v`，如果没有默认赋值为`0`  |
| `c.insert(it, x)`              | 向任意迭代器`it`插入一个元素`x` ，O ( N ) O(N)O(N)  |
| 例：`c.insert(c.begin() + 2,-1)` | 将`-1`插入`c[2]`的位置                       |
| `c.erase(first,last)`          | 删除`[first,last)`的所有元素，O ( N ) O(N)O(N) |
| `c.begin()`                    | 返回首元素的迭代器（通俗来说就是地址）O ( 1 ) O(1)O(1)    |
| `c.end()`                      | 返回最后一个元素后一个位置的迭代器（地址）O ( 1 ) O(1)O(1)  |
| `c.empty()`                    | 判断是否为空，为空返回真，反之返回假 O ( 1 ) O(1)O(1)    |

注意：
- `end()`返回的是最后一个元素的后一个位置的地址，不是最后一个元素的地址，**所有STL容器均是如此**
    
- 使用 `vi.resize(n, v)` 函数时，若 `vi` 之前指定过大小为 `pre`
    
    - `pre > n` ：即数组大小变小了，数组会保存前 `n` 个元素，前 `n` 个元素值为原来的值，不是都为 `v`
    - `pre < n` ：即数组大小变大了，数组会在后面插入 `n - pre` 个值为 `v` 的元素
    
    也就是说，这个初始值 `v` 只对新插入的元素生效。
```cpp
#include<bits/stdc++.h>
using namespace std;
void out(vector<int> &a) { for (auto &x: a) cout << x << " "; cout << "\n"; }
int main() {
	vector<int> a(5, 1);
	out(a); // 1 1 1 1 1
	a.resize(10, 2);
	out(a); // 1 1 1 1 1 2 2 2 2 2
	a.resize(3, 3);
	out(a); // 1 1 1
	return 0;
}
```



**排序**
使用`sort`排序要： `sort(c.begin(), c.end());`
对所有元素进行排序，如果要对指定区间进行排序，可以对`sort()`里面的参数进行加减改动。
```cpp
vector<int> a(n + 1);
sort(a.begin() + 1, a.end()); // 对[1, n]区间进行从小到大排序
```



**访问**
共三种方法：

- **下标法** ： 和普通数组一样

注意：一维数组的下标是从 `0` 到 `v.size()-1` ，访问之外的数会出现越界错误
```cpp
//添加元素
for(int i = 0; i < 5; i++)
	vi.push_back(i);
	
//下标访问 
for(int i = 0; i < 5; i++)
	cout << vi[i] << " ";
cout << "\n";
```

- **迭代器法** ： 类似指针一样的访问 ，首先需要声明迭代器变量，和声明指针变量一样，可以根据代码进行理解（附有注释）。
```cpp
vector<int> vi; //定义一个vi数组
vector<int>::iterator it = vi.begin();//声明一个迭代器指向vi的初始位置
```
```cpp
vector<int> vi{1, 2, 3, 4, 5};
//迭代器访问
vector<int>::iterator it;   
// 相当于声明了一个迭代器类型的变量it
// 通俗来说就是声明了一个指针变量
```
```cpp
//遍历方法一
vector<int>::iterator it = vi.begin(); 
for(int i = 0; i < 5; i++)
	cout << *(it + i) << " ";
cout << "\n";

//遍历方法二
vector<int>::iterator it;
for(it = vi.begin(); it != vi.end();it ++)
	cout << *it << " ";
//vi.end()指向尾元素地址的下一个地址

// 或者
auto it = vi.begin();
while (it != vi.end()) {
    cout << *it << "\n";
    it++;
}
```


- **使用auto** ：非常简便，但是会访问数组的所有元素（特别注意0位置元素也会访问到）
```cpp
// 1. 输入
vector<int> a(n);
for (auto &x: a) {
    cin >> x; // 可以进行输入，注意加引用
}
// 2. 输出
vector<int> v;
v.push_back(12);
v.push_back(241);
for(auto val : v) {
	cout << val << " "; // 12 241
}
```

> [!NOTE]
> `vector`注意：
> 
> - `vi[i]` 和 `*(vi.begin() + i)` 等价，与指针类似。
>     
> - `vector`和`string`的`STL`容器支持`*(it + i)`的元素访问，其它容器可能也可以支持这种方式访问，但用的不多，可自行尝试。