[P1248 加工生产调度 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1248)

# 加工生产调度

## 题目描述

某工厂收到了 $n$ 个产品的订单，这 $n$ 个产品分别在 A、B 两个车间加工，并且必须先在 A 车间加工后才可以到 B 车间加工。

某个产品 $i$ 在 A、B 两车间加工的时间分别为 $A_i,B_i$。怎样安排这 $n$ 个产品的加工顺序，才能使总的加工时间最短。

这里所说的加工时间是指：从开始加工第一个产品到最后所有的产品都已在 A、B 两车间加工完毕的时间。

## 输入格式

第一行仅—个整数 $n$，表示产品的数量。

接下来一行 $n$ 个整数是表示这 $n$ 个产品在 A 车间加工各自所要的时间。

最后的 $n$ 个整数是表示这 $n$ 个产品在 B 车间加工各自所要的时间。

## 输出格式

第一行一个整数，表示最少的加工时间。

第二行是一种最小加工时间的加工顺序。

## 样例 #1

### 样例输入 #1

```
5
3 5 8 7 10
6 2 1 4 9
```

### 样例输出 #1

```
34
1 5 4 2 3
```

## 提示

$1\leq n\leq 1000$。


## 题解

#贪心 #数学模型 #johnson不等式

![[微信图片_20241016171211.png]]
![[微信图片_20241016171213 1.png]]
![[微信图片_20241016171216.png]]
![[微信图片_20241016171219.png]]

第一次的提交，有两个测试点WA了
其中一个测试点信息如下：
```
输入：
20
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20
20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1

输出：
211
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20

```
但是我的程序输出的结果是：
```cpp
212
1 2 3 4 5 6 7 8 9 10 20 11 12 13 14 15 16 17 18 19
```
很明显是20的处理有问题，最后一组数据应该属于N0组，但是在输出的时候处于N1组的最后端或者N0组的最前端，这是错误的。
```cpp
#include<iostream>
#include<vector>
#include<algorithm>
#include<vector>

using namespace std;
const int N = 1010;
int a[N], b[N];
int n;

struct work
{
    int no;//作业序号
    int group;//分组
    int time;//
    
    //根据 time进行递增排序
    //重载小于运算符
    bool operator < (const work& a) const
    {
        return time < a.time;
    }
};

work w[N];

int main()
{
    cin>>n;
    for(int i=1; i<=n; i++)
        cin>>a[i];
    for(int i=1; i<=n; i++)
        cin>>b[i];
        
    //开始分组
    for(int i=1; i<=n; i++)
    {
        w[i].no = i;
        
        if(a[i] <= b[i])
        {
            w[i].group = 1;
            w[i].time = a[i];
        }
        else
        {
            w[i].group = 0;
            w[i].time = b[i];
        }
    }
    
    //按照time进行递增排序，每个组别也是递增的
    sort(w, w+n);
    
    int f1, f2;//f1:累计M1上所用的时间
    //f2:累计M2上所用的总时间，等价于总时间
    f1 = f2 = 0;//初始时刻均为0
    
    int N1[N], N0[N];//N1、N0分组的排序
    
    int i = 0, j = n-1;//N1、N0分组的专属指针
    //N1记录递增排序 i
    //N0记录递减排序 j
    int cntn1 = 0;
    for(int k=1; k<=n; k++)
    {
        if(w[k].group == 1)
        {
            cntn1 ++;//给第一组的计数
            N1[i++] = w[k].no;//记录执行顺序 //N1的有效下标是0~cntn1-1
        }
        else N0[j--] = w[k].no; //N0的有效下标是 n-1 -> cntn1
    }
    
    //计算执行时间
    //首先顺序执行N1的作业（递增执行）
    for(int k=0; k<cntn1; k++)
    {
        f1 = f1 + a[N1[k]];
        f2 = max(f1, f2) + b[N1[k]];
    }
    
    //然后顺序执行N0的作业（递减执行）
    for(int k=cntn1; k<n; k++)
    {
        f1 = f1 + a[N0[k]];
        f2 = max(f1, f2) + b[N0[k]];
    }
    
    cout<<f2<<endl;
    for(int k=0; k<cntn1; k++)
    {
        cout<<N1[k]<<" ";
    }
    
    //然后顺序执行N0的作业（递减执行）
    for(int k=cntn1; k<n; k++)
    {
        cout<<N0[k]<<" ";
    }
    
    
    return 0;
}
```

实际上，思路并没有问题，问题出在了排序
```cpp
//按照time进行递增排序，每个组别也是递增的
    sort(w, w+n);
```
上面这句是错误的，`w[]`的有效下标在`1~n` 而排序区间是前闭后开的，
所以只有当区间为`[1, n+1)`时才是对`[1, n]`进行正确的排序。

==不要想当然地使用排序，要考虑好区间问题==

AC代码如下：
```cpp
#include<iostream>
#include<vector>
#include<algorithm>
#include<vector>

using namespace std;
const int N = 1010;
int a[N], b[N];
int n;

struct work
{
    int no;//作业序号
    int group;//分组
    int time;//
    
    //根据 time进行递增排序
    //重载小于运算符
    bool operator < (const work& a) const
    {
        return time < a.time;
    }
};

work w[N];

int main()
{
    cin>>n;
    for(int i=1; i<=n; i++)
        cin>>a[i];
    for(int i=1; i<=n; i++)
        cin>>b[i];
        
    //开始分组
    for(int i=1; i<=n; i++)
    {
        w[i].no = i;
        
        if(a[i] <= b[i])
        {
            w[i].group = 1;
            w[i].time = a[i];
        }
        else
        {
            w[i].group = 0;
            w[i].time = b[i];
        }
    }
    
    //按照time进行递增排序，每个组别也是递增的
    sort(w+1, w+n+1);//注意员工的有效下标是在 1~n, 排序区间的右端点是开区间
    
    int f1, f2;//f1:累计M1上所用的时间
    //f2:累计M2上所用的总时间，等价于总时间
    f1 = f2 = 0;//初始时刻均为0
    
    int N1[N], N0[N];//N1、N0分组的排序
    
    int i = 0, j = n-1;//N1、N0分组的专属指针
    //N1记录递增排序 i
    //N0记录递减排序 j
    int cntn1 = 0;
    for(int k=1; k<=n; k++)
    {
        if(w[k].group == 1)
        {
            cntn1 ++;//给第一组的计数
            N1[i++] = w[k].no;//记录执行顺序 //N1的有效下标是0~cntn1-1
        }
        else N0[j--] = w[k].no; //N0的有效下标是 n-1 -> cntn1
    }
    
    //计算执行时间
    //首先顺序执行N1的作业（递增执行）
    for(int k=0; k<cntn1; k++)
    {
        f1 = f1 + a[N1[k]];
        f2 = max(f1, f2) + b[N1[k]];
    }
    
    //然后顺序执行N0的作业（递减执行）
    for(int k=cntn1; k<n; k++)
    {
        f1 = f1 + a[N0[k]];
        f2 = max(f1, f2) + b[N0[k]];
    }
    
    cout<<f2<<endl;
    for(int k=0; k<cntn1; k++)
    {
        cout<<N1[k]<<" ";
    }
    
    //然后顺序执行N0的作业（递减执行）
    for(int k=cntn1; k<n; k++)
    {
        cout<<N0[k]<<" ";
    }
    
    
    return 0;
}
```