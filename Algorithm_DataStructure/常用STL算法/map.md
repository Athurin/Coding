[C++STL之map详解_c++ stl map-CSDN博客](https://blog.csdn.net/qq_50285142/article/details/120368977)


映射类似于函数的对应关系，每个`x`对应一个`y`，而`map`是每个键对应一个值。

**头文件**
```cpp
//头文件
#include<map>
//初始化定义
map<string, string> mp;
map<string, int> mp;
map<int, node> mp;//node是结构体类型
```

map特性：map会按照键的顺序从小到大自动排序，键的类型必须可以比较大小。


**函数方法**

|代码|含义|
|---|---|
|`mp.find(key)`|返回键为key的映射的迭代器 $O(logN) $ 注意：用find函数来定位数据出现位置，它返回一个迭代器。当数据存在时，返回数据所在位置的迭代器，数据不存在时，返回m p . e n d ( ) mp.end()mp.end()|
|`mp.erase(it)`|删除迭代器对应的键和值O ( 1 ) O(1)O(1)|
|`mp.erase(key)`|根据映射的键删除键和值 O ( l o g N ) O(logN)O(logN)|
|`mp.erase(first,last)`|删除左闭右开区间迭代器对应的键和值 O ( l a s t − f i r s t ) O(last-first)O(last−first)|
|`mp.size()`|返回映射的对数$ O(1)$|
|`mp.clear()`|清空map中的所有元素O ( N ) O(N)O(N)|
|`mp.insert()`|插入元素，插入时要构造键值对|
|`mp.empty()`|如果map为空，返回true，否则返回false|
|`mp.begin()`|返回指向map第一个元素的迭代器（地址）|
|`mp.end()`|返回指向map尾部的迭代器（最后一个元素的**下一个**地址）|
|`mp.rbegin()`|返回指向map最后一个元素的迭代器（地址）|
|`mp.rend()`|返回指向map第一个元素前面(上一个）的逆向迭代器（地址）|
|`mp.count(key)`|查看元素是否存在，因为map中键是唯一的，所以存在返回1，不存在返回0|
|`mp.lower_bound()`|返回一个迭代器，指向键值>= **key**的第一个元素|
|`mp.upper_bound()`|返回一个迭代器，指向键值> key的第一个元素|