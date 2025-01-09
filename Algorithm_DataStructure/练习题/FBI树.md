链接：[https://ac.nowcoder.com/acm/problem/16660](https://ac.nowcoder.com/acm/problem/16660)
## 题目描述

我们可以把由“0”和“1”组成的字符串分为三类：全“0”串称为B串，全“1”串称为I串，既含“0”又含“1”的串则称为F串。

FBI树是一种二叉树[1]，它的结点类型也包括F结点，B结点和I结点三种。由一个长度为2N的“01”串S可以构造出一棵FBI树T，递归的构造方法如下：

**1) T****的根结点为****R****，其类型与串****S****的类型相同；**

**2)** **若串****S****的长度大于****1****，将串****S****从中间分开，分为等长的左右子串****S1****和****S2****；由左子串****S1****构造****R****的左子树****T1****，由右子串****S2****构造****R****的右子树****T2****。**

现在给定一个长度为2N的“01”串，请用上述构造方法构造出一棵FBI树，并输出它的后序遍历[2]序列。

> [1] 二叉树：二叉树是结点的有限集合，这个集合或为空集，或由一个根结点和两棵不相交的二叉树组成。这两棵不相交的二叉树分别称为这个根结点的左子树和右子树。
> 
> [2] 后序遍历：后序遍历是深度优先遍历二叉树的一种方法，它的递归定义是：先后序遍历左子树，再后序遍历右子树，最后访问根。

## 输入描述:

第一行是一个整数N（0 <= N <= 10）

第二行是一个长度为2N的“01”串。

## 输出描述:

一个字符串，即FBI树的后序遍历序列。

## 样例

输入
```
3
10001011
```

输出
```
IBFBBBFIBFIIIFF
```

对于40%的数据，N <= 2；  
对于全部的数据，N<= 10。


## 题解

#二叉树 #树的遍历 #递归 

递归的时候直接多次循环判断，很暴力的一种思路。

```cpp
#include<iostream>
#include<cstring>
using namespace std;

const int N = (1 << 10) + 10;
char s[N];
int len;
//检查当前区间为何种串类型
char check(int l, int r)
{
    int flag_1 , flag_2;
    flag_1 = flag_2 = 0;
    for (int i = l; i <= r; ++i)
        if (flag_1 && flag_2) break;
        else if (s[i] == '0') flag_1 = 1;
        else if (s[i] == '1') flag_2 = 1;
    
    if (flag_1 && flag_2) return 'F';
    else if (flag_2) return 'I';
    else return 'B';
}

void dfs(int l, int r)
{
    //后序编列
    if (l != r) 
    {
        int mid = l + ((r - l) >> 1);
        dfs(l, mid);
        dfs(mid + 1, r);
    }
    putchar(check(l, r));
}


int main()
{
    int n;
    scanf("%d", &n);
    scanf("%s", s + 1);
    int len = (1 << n);
    dfs(1, len);
    
    return 0;
}
```

我在想，可不可以直接建立一棵树，每次判断的时候，只用判断左右孩子的属性即可。
(似乎有点麻烦)

