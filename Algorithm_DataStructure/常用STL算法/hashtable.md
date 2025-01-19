## 常见的三种哈希结构

一般会选择如下三种数据结构:

- 数组
- set （集合）
- map(映射)

这里数组就没啥可说的了，我们来看一下set。

在C++中，set 和 map 分别提供以下三种数据结构，其底层实现以及优劣如下表所示：

|集合|底层实现|是否有序|数值是否可以重复|能否更改数值|查询效率|增删效率|
|---|---|---|---|---|---|---|
|std::set|红黑树|有序|否|否|O(log n)|O(log n)|
|std::multiset|红黑树|有序|是|否|O(logn)|O(logn)|
|std::unordered_set|哈希表|无序|否|否|O(1)|O(1)|

`std::unordered_set`底层实现为哈希表，`std::set 和std::multiset` 的底层实现是红黑树，红黑树是一种平衡二叉搜索树，所以`key`值是有序的，但`key`不可以修改，改动`key`值会导致整棵树的错乱，所以只能删除和增加。

| 映射                 | 底层实现 | 是否有序  | 数值是否可以重复 | 能否更改数值  | 查询效率     | 增删效率     |
| ------------------ | ---- | ----- | -------- | ------- | -------- | -------- |
| std::map           | 红黑树  | key有序 | key不可重复  | key不可修改 | O(logn)  | O(logn)  |
| std::multimap      | 红黑树  | key有序 | key可重复   | key不可修改 | O(log n) | O(log n) |
| std::unordered_map | 哈希表  | key无序 | key不可重复  | key不可修改 | O(1)     | O(1)     |
`std::unordered_map` 底层实现为哈希表，`std::map` 和`std::multimap` 的底层实现是红黑树。同理，`std::map` 和`std::multimap `的`key`也是有序的（这个问题也经常作为面试题，考察对语言容器底层的理解）。

当我们要使用集合来解决哈希问题的时候，优先使用`unordered_set`，因为它的查询和增删效率是最优的，如果需要集合是有序的，那么就用`set`，如果要求不仅有序还要有重复数据的话，那么就用`multiset`。

那么再来看一下`map` ，在`map` 是一个`key value` 的数据结构，`map`中，对`key`是有限制，对`value`没有限制的，因为`key`的存储方式使用红黑树实现的。

`Set` 是值的集合，而`Map`是`KV`集合。


[std::set - cppreference.com](https://en.cppreference.com/w/cpp/container/set)
[std::multiset - cppreference.com](https://en.cppreference.com/w/cpp/container/multiset)
[std::unordered_set - cppreference.com](https://en.cppreference.com/w/cpp/container/unordered_set)


[std::map - cppreference.com](https://en.cppreference.com/w/cpp/container/map)
[std::multimap - cppreference.com](https://en.cppreference.com/w/cpp/container/multimap)
[std::unordered_map - cppreference.com](https://en.cppreference.com/w/cpp/container/unordered_map)

