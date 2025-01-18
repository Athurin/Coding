```cpp
#include<iostream>
#include<cstring>
#include<algorithm>

using namespace std;

const int N = 1e5+10 ;

int h[N], e[N], ne[N], idx ;
int n, m ;
int q[N], hh, tt ,d[N] ;

void add(int a, int b)
{
    e[idx]=b, ne[idx]=h[a], h[a]=idx++ ;
}

bool topsort()
{
    hh=0, tt=-1 ;

    for(int i=1; i<=n; i++)
        if(d[i]==0) q[++tt]=i ;

    while(hh<=tt)
    {
        int t=q[hh++];

        for(int i=h[t]; i!=-1; i=ne[i])
        {
            int j=e[i];
            --d[j];
            if(d[j]==0) q[++tt]=j;
        } 
    }

    return tt==n-1;
}

int main()
{
    cin>>n>>m;

    memset(h, -1, sizeof(h));

    int a, b ;
    for(int i=0; i<m; i++)
    {
        scanf("%d%d", &a, &b);
        add(a, b);

        d[b]++ ;
    }

    if(!topsort()) printf("-1");
    else
    {
        for(int i=0; i<n; i++) printf("%d ",q[i]);
        puts("");
    }

    return 0;
}
```