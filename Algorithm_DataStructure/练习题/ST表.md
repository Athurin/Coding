
[P3865 【模板】ST 表 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P3865)

## 题目背景

这是一道 ST 表经典题——静态区间最大值

**请注意最大数据时限只有 0.8s，数据强度不低，请务必保证你的每次查询复杂度为 $O(1)$。若使用更高时间复杂度算法不保证能通过。**

如果您认为您的代码时间复杂度正确但是 TLE，可以尝试使用快速读入：

```cpp
inline int read()
{
	int x=0,f=1;char ch=getchar();
	while (ch<'0'||ch>'9'){if (ch=='-') f=-1;ch=getchar();}
	while (ch>='0'&&ch<='9'){x=x*10+ch-48;ch=getchar();}
	return x*f;
}
```

函数返回值为读入的第一个整数。

**快速读入作用仅为加快读入，并非强制使用。**

## 题目描述

给定一个长度为 $N$ 的数列，和 $M$ 次询问，求出每一次询问的区间内数字的最大值。

## 输入格式

第一行包含两个整数 $N,M$，分别表示数列的长度和询问的个数。

第二行包含 $N$ 个整数（记为 $a_i$），依次表示数列的第 $i$ 项。

接下来 $M$ 行，每行包含两个整数 $l_i,r_i$，表示查询的区间为 $[l_i,r_i]$。

## 输出格式

输出包含 $M$ 行，每行一个整数，依次表示每一次询问的结果。

## 样例

### 样例输入 #1

```
8 8
9 3 1 7 5 6 0 8
1 6
1 5
2 7
2 6
1 8
4 8
3 7
1 8
```

### 样例输出 #1

```
9
9
7
7
9
8
7
9
```

## 提示

对于 $30\%$ 的数据，满足 $1\le N,M\le 10$。

对于 $70\%$ 的数据，满足 $1\le N,M\le {10}^5$。

对于 $100\%$ 的数据，满足 $1\le N\le {10}^5$，$1\le M\le 2\times{10}^6$，$a_i\in[0,{10}^9]$，$1\le l_i\le r_i\le N$。

## 题解

#ST算法 #快速读入 #模板题 

这一题是模板题，但是坑人的是必须要加上快速读入。

最开始提交的时候，最后几个点会超时，于是做了如下的优化：
- 开启快读
- 取消数组`a[i]`的创建，直接将值赋给`dpmax[i][0]` 
- 将最后的输出 `cout` 换成了 `printf()`， 明显`scanf` 和`printf` 比`cin` 与`cout` 快很多
- O2 优化

AC代码：
```cpp
#include<iostream>
#include<cmath>

using namespace std;
const int N = 1e5+10;

int n, m;
//int a[N];
int dpmax[N][20];

inline int read()
{
	int x=0,f=1;char ch=getchar();
	while (ch<'0'||ch>'9'){if (ch=='-') f=-1;ch=getchar();}
	while (ch>='0'&&ch<='9'){x=x*10+ch-48;ch=getchar();}
	return x*f;
}

void InitST()
{
    int q = log2(n);
    // for(int i = 1; i<=n; i++)
    // {
    //     dpmax[i][0] = a[i];
    // }
    
    for(int i=1; i<=q; i++)
        for(int s=1; s+(1<<i) -1 <= n; s++)
        {
            dpmax[s][i] = max(dpmax[s][i-1], dpmax[s+(1<<(i-1))][i-1]);
        }
}

int query(int l ,int r)
{
    int p = log2(r - l +1);
    int maxx = 0;
    
    maxx = max(dpmax[l][p], dpmax[r-(1<<p)+1][p]);
    
    return maxx;
}

int main()
{
    n = read();
    m = read();
    
    for(int i=1; i<=n; i++) dpmax[i][0] = read();
    
    InitST();
    
    int l, r;
    while(m--)
    {
        l = read();
        r = read();
        printf("%d\n", query(l, r));
    }
    
    return 0;
}
```

