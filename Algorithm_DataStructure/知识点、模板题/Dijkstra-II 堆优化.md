```cpp
//堆优化版的dijkstra算法
#include<iostream>
#include<cstring>
#include<queue>
#include<algorithm>

using namespace std; 

const int  N = 1.5e5+10 ;
typedef pair<int, int> PII ;

int n, m;
int h[N], w[N], e[N], ne[N], idx ;
bool st[N];
int dist[N];

void add(int a, int b, int c)
{
    e[idx]=b, w[idx]=c, ne[idx]=h[a], h[a]=idx++ ;
}

int dijkstra02()
{
    memset(dist, 0x3f, sizeof dist);

    dist[1]=0;

    priority_queue<PII, vector<PII>,greater<PII>> heap;//小根堆

    // 只能将距离放在first位，因为大小堆排序都是按照first排序的
    heap.push({0,1});

    while(heap.size())
    {
        auto t=heap.top();
        heap.pop();

        int ver=t.second;

        if(st[ver]) continue;
        st[ver]=true;

        for(int i=h[ver]; i!=-1; i=ne[i])
        {
            auto j=e[i];
            if(dist[ver]+w[i]<dist[j])
            {
                dist[j]=dist[ver]+w[i];
                heap.push({dist[j], j});
            }
        }
    }

    if(dist[n]==0x3f3f3f3f) return -1;

    return dist[n];
}

int main()
{
    cin>>n>>m;

    memset(h, -1, sizeof h);

    int a, b, c;
    while(m--)
    {
        scanf("%d%d%d",&a, &b, &c);
        add(a, b, c);
    }

    printf("%d\n", dijkstra02());

    return 0;
}
```