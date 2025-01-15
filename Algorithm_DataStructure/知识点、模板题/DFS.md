```cpp
#include<iostream>
using namespace std;

const int N = 10 ;
int path[N], n;
bool state[N];

void dfs(int u)
{
    if(u==n)
    {
        for(int i=0; i<n; i++) printf("%d ",path[i]);
        puts("");
        return ;
    }

    for(int i=1; i<=n; i++)
        if(!state[i])
        {
            path[u]=i ;
            state[i]=true;
            dfs(u+1);
            state[i]=false;
        }
}

int main()
{
    cin>>n;

    //从序列第零位开始深搜
    //即path[]的有效值从path[0]~path[n-1];
    //当u==n时，意味着从path[0]~path[n-1]已经在前几次递归就排好了，
    //这次只需要简单的输出path[0]~path[n-1] 就好了
    dfs(0);

    return 0;
}
```