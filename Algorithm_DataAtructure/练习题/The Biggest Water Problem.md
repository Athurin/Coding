链接：[https://ac.nowcoder.com/acm/problem/15173](https://ac.nowcoder.com/acm/problem/15173) 
## 题目描述

给你一个数，让他进行巴啦啦能量，沙鲁沙鲁，小魔仙大变身，如果进行变身的数不满足条件的话，就继续让他变身。。。直到满足条件为止。

巴啦啦能量，沙鲁沙鲁，小魔仙大变身：对于一个数，把他所有位上的数字进行加和，得到新的数。

如果这个数字是个位数的话，那么他就满足条件。  

## 输入描述:

给一个整数数字n(1<=n<=1e9)。

## 输出描述:

输出由n经过操作满足条件的数

## 题解

#递归 #分治

```cpp
#include<iostream>
#include<string>

using namespace std;

const int N = 1e9+10;

//不考虑负数？
int solve(int x)
{
    if(x<10) return x;
    
    int a=0;
    while(x)
    {
        a += x % 10;
        x /= 10;
    }
    
    return solve(a);
}

int main()
{
    int n;
     cin>>n;
    
    int res = solve(n);
    cout<<res;
    
    return 0;
}
```

