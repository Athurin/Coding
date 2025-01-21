
```cpp
#include<iostream>
#include<algorithm>
#include<cstring>

using namespace std;
const int N = 510, M = 1e5+10;
int n, m, k;
int dist[N], last[N];

struct Edge
{
    int a, b, c;
}edges[M];

void Bellman()
{
    memset(dist, 0x3f3f3f3f, sizeof dist);
    dist[1]=0;

    for(int i=1; i<=k; i++)
    {
        memcpy(last, dist, sizeof dist);
        for(int j=1; j<=m; j++)
        {
            auto e=edges[j];
            dist[e.b]=min(dist[e.b], last[e.a]+e.c);
        }
    }
}

int main()
{
    cin>>n>>m>>k;

    int a, b, c;
    for(int i=1; i<=m; i++)
    {
        scanf("%d%d%d", &a, &b, &c);
        edges[i]={a, b, c};
    }

    Bellman();

    if(dist[n]>0x3f3f3f3f/2) cout<<"impossible"<<endl;
    else cout<<dist[n]<<endl;

    return 0;
}

```