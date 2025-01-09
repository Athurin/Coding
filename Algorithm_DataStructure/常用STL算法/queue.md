队列是一种先进先出的数据结构

**头文件**
```cpp
//头文件
#include<queue>
//定义初始化
queue<int> q;
```

**方法函数：**

|代码|含义|
|---|---|
|`q.front()`|返回队首元素 O ( 1 ) O(1)O(1)|
|`q.back()`|返回队尾元素 O ( 1 ) O(1)O(1)|
|`q.push(element)`|尾部添加一个元素`element` 进队O ( 1 ) O(1)O(1)|
|`q.pop()`|删除第一个元素 出队 O ( 1 ) O(1)O(1)|
|`q.size()`|返回队列中元素个数，返回值类型`unsigned int` O ( 1 ) O(1)O(1)|
|`q.empty()`|判断是否为空，队列为空，返回`true` O ( 1 ) O(1)O(1)|


**队列模拟**

使用`q[]`数组模拟队列

`hh`表示队首元素的下标，初始值为`0`

`tt`表示队尾元素的下标，初始值为`-1`，表示刚**开始队列为空**
`一般来说单调栈和单调队列写法均可使用额外变量 `tt` 或 `hh` 来进行模拟` 
```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1e5 + 5;
int q[N];

int main() {
	int hh = 0,tt = -1;
//	入队 
	q[++tt] = 1;
	q[++tt] = 2; 
//	将所有元素出队 
	while(hh <= tt) {
		int t = q[hh++];
		printf("%d ",t);
	}
	return 0;
 } 
```

