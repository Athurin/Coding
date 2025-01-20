//邻接矩阵
```cpp
#include<iostream>
#include<cstring>
#include<algorithm>

using namespace std ;

const int N = 500+10, M = 1e5+10 ;
int n, m, g[N][N], dist[N] ;
bool state[N];

int dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1]=0;
    
    for(int i=1; i<=n; i++)
    {
        int t=-1;
        for(int j=1; j<=n;j++)
            if( !state[j] && ( t==-1 || dist[t]>dist[j] ) )
                t=j;
                
        for(int j=1; j<=n; j++)
            dist[j]=min(dist[j], dist[t]+g[t][j]);
            
        state[t]=true;
    }
    
    if(dist[n]==0x3f3f3f3f) return -1;
    
    return dist[n];
}

int main()
{
    cin>>n>>m;
    
    memset(g, 0x3f, sizeof g);
    
    int a, b, c;
    for(int i=0; i<m; i++)
    {
        scanf("%d%d%d",&a, &b, &c);
        
        g[a][b]=min(g[a][b], c);
    }
    
    cout<<dijkstra()<<endl;
    
    return 0;
}
```
//邻接表
```cpp
#include<iostream>
#include <cstring>
#include <algorithm>
using namespace std;

const int N = 510, M = 100010;

int h[N], e[M], ne[M], w[M], idx;//邻接表存储图
int state[N];//state 记录是否找到了源点到该节点的最短距离
int dist[N];//dist 数组保存源点到其余各个节点的距离
int n, m;//图的节点个数和边数

void add(int a, int b, int c)//插入边
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
}

void Dijkstra()
{
    memset(dist, 0x3f, sizeof(dist));//dist 数组的各个元素为无穷大
    dist[1] = 0;//源点到源点的距离为置为 0
    for (int i = 0; i < n; i++)
    {
        int t = -1;
        for (int j = 1; j <= n; j++)//遍历 dist 数组，找到没有确定最短路径的节点中距离源点最近的点t
        {
            if (!state[j] && (t == -1 || dist[j] < dist[t]))
                t = j;
        }

        state[t] = 1;//state[i] 置为 1。

        for (int j = h[t]; j != -1; j = ne[j])//遍历 t 所有可以到达的节点 i
        {
            int i = e[j];
            dist[i] = min(dist[i], dist[t] + w[j]);//更新 dist[j]
        }


    }
}

int main()
{
    memset(h, -1, sizeof(h));//邻接表初始化

    cin >> n >> m;
    while (m--)//读入 m 条边
    {
        int a, b, w;
        cin >> a >> b >> w;
        add(a, b, w);
    }

    Dijkstra();
    if (dist[n] != 0x3f3f3f3f)//如果dist[n]被更新了，则存在路径
        cout << dist[n];
    else
        cout << "-1";
}
```