```cpp
#include<iostream>

using namespace std;

const int N = 1e5+10;

//son[N][26]，N表示当前是第N个结点，26表示每个节点最多能过向外延申26条边
//cnt[N],表示以第N个结点结尾的单词有多少个
//idx，主要用于插入操作时，临时存储当前访问的结点的序号
int son[N][26],cnt[N],idx;
char str[N];

//在TRIE树当中，结点son[i][j]=k,
//表示第i个结点的第j个孩子是第k个结点
//因此可以通过p=son[p][u]来不断地向下遍历TRIE树
void insert(char *str)
{
    int p=0;
    for(int i=0;str[i];i++)
    {
        int u = str[i] - 'a';
        if(!son[p][u]) son[p][u] = ++idx;
        p=son[p][u];
    }

    cnt[p]++;
}

int query(char* str)
{
    int p=0; 
    for(int i=0;str[i];i++)
    {
        int u = str[i] - 'a';
        if(!son[p][u]) return 0;
        p=son[p][u];
    }
    return cnt[p];
}

int main()
{
    int n;
    cin>>n;

    while(n--)
    {
        //op[]算是一个输入小技巧
        //op[1]可以吸收缓冲区中的空格符
        char op[2];
        scanf("%s%s",op,str);

        if(*op=='I')
            insert(str);

        else if(*op=='Q') printf("%d\n",query(str));
    }

    return 0;
}
```