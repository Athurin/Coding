```cpp
#include<iostream>
#include<queue>
#include<cstring>

using namespace std;
const int N = 20100, M = 3e5+10;

int h[N], e[M], ne[M], w[M], idx;
int d[N], cnt[N];
bool st[N];
int n, m;

void add(int a, int b, int c)
{
    e[idx] = b, ne[idx] = h[a], w[idx] = c;
    h[a] = idx++;
}

bool spfaloop()
{
    queue<int> q;
    
    memset(d, 0x3f, sizeof d);
    memset(cnt, 0, sizeof cnt);
    d[1] = 0;
    
    q.push(1);
    //st[1] = true;
    
    while(q.size())
    {
        int t = q.front();
        q.pop();
        st[t] = false;
        
        for(int i = h[t]; i != -1; i = ne[i])
        {
            int k = e[i];
            
            if(d[k] > d[t] + w[i])
            {
                d[k] = d[t] + w[i];
                cnt[k] = cnt[t] + 1;
                
                if(cnt[k] >= n) return true;
                
                if(st[k] == false)
                {
                    q.push(k);
                    st[k] = true;
                }
            }
        }
    }
    
    return false;
}

int main()
{
    
    int T;
    cin>>T;
    
    while(T--)
    {
        memset(h, -1, sizeof h);
        // memset(e, 0, sizeof e);
        // memset(ne, 0, sizeof ne);
        // memset(w, 0, sizeof w);
        memset(st, 0, sizeof st);
        //idx = 0;
        
        cin>>n>>m;
        
        int u, v, w;
        while(m--)
        {
            scanf("%d%d%d", &u, &v, &w);
            add(u, v, w);
            
            if(w >= 0) add(v, u, w);
        }
        
        if(spfaloop()) puts("YES");
        else puts("NO");
    }
    
    return 0;
}
```

