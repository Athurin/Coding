
**头文件**
```cpp
//头文件
#include<queue>
//初始化定义
priority_queue<int> q;
```

**函数方法**

| 代码                              | 含义                                      |
| ------------------------------- | --------------------------------------- |
| `q.top()`                       | 访问队首元素 O ( 1 ) O(1)O(1)                 |
| `q.push()`                      | 入队 O ( l o g N ) O(logN)O(logN)         |
| `q.pop()`                       | 堆顶（队首）元素出队 O ( l o g N ) O(logN)O(logN) |
| `q.size()`                      | 队列元素个数 O ( 1 ) O(1)O(1)                 |
| `q.empty()`                     | 是否为空 O ( 1 ) O(1)O(1)                   |
| **注意**没有`clear()`！              | 不提供该方法                                  |
| 优先队列只能通过`top()`访问队首元素（优先级最高的元素） |                                         |

**设置优先级**

基本数据类型的优先级：
```cpp
priority_queue<int> pq; // 默认大根堆, 即每次取出的元素是队列中的最大值
priority_queue<int, vector<int>, greater<int>> q; // 小根堆, 每次取出的元素是队列中的最小值
```

参数解释：

- 第一个参数：就是优先队列中存储的数据类型
    
- 第二个参数：
    
    `vector<int>` 是用来承载底层数据结构 堆 的容器，若优先队列中存放的是`double`型数据，就要填`vector< double >`  
    **总之存的是什么类型的数据，就相应的填写对应类型。同时也要改动第三个参数里面的对应类型。**
    
- 第三个参数：  
    `less<int>` 表示数字大的优先级大，堆顶为最大的数字  
    `greater<int>`表示数字小的优先级大，堆顶为最小的数字  
    **int代表的是数据类型，也要填优先队列中存储的数据类型**


1. 基础写法（非常常用）：
```cpp
priority_queue<int> q1; // 默认大根堆, 即每次取出的元素是队列中的最大值
priority_queue<int, vector<int>, less<int> > q2; // 大根堆, 每次取出的元素是队列中的最大值，同第一行

priority_queue<int, vector<int>, greater<int> > q3; // 小根堆, 每次取出的元素是队列中的最小值
```

2. 自定义排序（不常见，主要是写着麻烦）：
```cpp
struct cmp1 {
	bool operator()(int x, int y) {
		return x > y;
	}
};
struct cmp2 {
	bool operator()(const int x, const int y) {
		return x < y;
	}
};
priority_queue<int, vector<int>, cmp1> q1; // 小根堆
priority_queue<int, vector<int>, cmp2> q2; // 大根堆
```

**复杂数据类型优先级设置的写法：**

_即优先队列中存储结构体类型，必须要设置优先级，即结构体的比较运算（因为优先队列的堆中要比较大小，才能将对应最大或者最小元素移到堆顶）。_

优先级设置可以定义在**结构体内**进行小于号重载，也可以定义在**结构体外**。
```cpp
//要排序的结构体（存储在优先队列里面的）
struct Point {
	int x, y;
};
```

- **版本一：自定义全局比较规则**
```cpp
//定义的比较结构体
//注意：cmp是个结构体 
struct cmp {//自定义堆的排序规则 
	bool operator()(const Point& a,const Point& b) {
		return a.x < b.x;
	}
};

//初始化定义， 
priority_queue<Point, vector<Point>, cmp> q; // x大的在堆顶
```

**本二：直接在结构体里面写**
```cpp
//1
struct node {
	int x, y;
	friend bool operator < (Point a, Point b) {//为两个结构体参数，结构体调用一定要写上friend
		return a.x < b.x;//按x从小到大排，x大的在堆顶
	}
};

//2
struct node {
    int x, y;
    bool operator < (const Point &a) const {//直接传入一个参数，不必要写friend
        return x < a.x;//按x升序排列，x大的在堆顶
    }
};
```

优先队列的定义
```cpp
priority_queue<Point> q;
```
**注意：** 优先队列自定义排序规则和`sort()`函数定义`cmp`函数很相似，但是最后返回的情况是**相反**的。即相同的符号，最后定义的排列顺序是完全相反的。  
所以只需要记住`sort`的排序规则和优先队列的排序规则是相反的就可以了。

> [!NOTE]
> 当理解了堆的原理就会发现，堆调整时比较顺序是孩子和父亲节点进行比较，如果是 `>` ，那么孩子节点要大于父亲节点，堆顶自然是最小值。


**存储特殊类型的优先级**

**存储pair类型**
- 排序规则：  
    默认先对`pair`的`first`进行降序排序，然后再对`second`降序排序  
    对`first`先排序，大的排在前面，如果`first`元素相同，再对`second`元素排序，保持大的在前面。

