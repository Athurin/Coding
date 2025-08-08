
## 模板
#模拟

[59. 螺旋矩阵 II - 力扣（LeetCode）](https://leetcode.cn/problems/spiral-matrix-ii/description/)

矩形边长为`n`，每次循环就会少两格，所以循环的次数是`n/2`。
对于`n`为奇数的时候，循环遍历完之后还需要处理最中心的小方格。

AC1：
```cpp
class Solution 
{
public:
    vector<vector<int>> generateMatrix(int n) 
    {
        vector<vector<int>> res(n, vector<int>(n, 0));
        int startx = 0, starty = 0;
        int loop = n/2;
        int mid = n/2;
        int count = 1;
        int offset = 1;
        int i, j;

        while(loop--)
        {
            i = startx;
            j = starty;

            for(; j < n-offset; j++) //top
                res[i][j] = count++;
            
            for(; i < n-offset; i++)//right
                res[i][j] = count++;
            
            for(; j > starty; j--)
                res[i][j] = count++;
            
            for(; i > startx; i--)
                res[i][j] = count++;
            
            startx++;
            starty++;

            offset++;
        }

        if(n%2)
            res[n/2][n/2] = count;

        return res;
    }
};
```


AC2:
startx和starty可以使用一个变量。每次循环的起始点都是对角线上的格子。
```cpp
class Solution 
{
public:
    vector<vector<int>> generateMatrix(int n) 
    {
        vector<vector<int>> res(n, vector<int>(n, 0));
        int startxy = 0;
        int loop = n/2;
        int count = 1;
        int offset = 1;
        int i, j;

        while(loop--)
        {
            i = startxy;
            j = startxy;

            for(; j < n-offset; j++) //top
                res[i][j] = count++;
            
            for(; i < n-offset; i++)//right
                res[i][j] = count++;
            
            for(; j > startxy; j--)
                res[i][j] = count++;
            
            for(; i > startxy; i--)
                res[i][j] = count++;
            
            startxy++;

            offset++;
        }

        if(n%2)
            res[n/2][n/2] = count;

        return res;
    }
};
```

AC3:
利用dfs，递归出口不加上`matrix[i][j] != 0` 就会无限递归然后爆栈。
```cpp
class Solution {
public:
    vector<vector<int>> dir{{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    int k = 0, num = 1;

    bool dfs(vector<vector<int>> &matrix, int i, int j)
    {
        if( i<0 || i >= matrix.size() || j<0 || j >= matrix[0].size() || matrix[i][j] != 0)
            return false;

        matrix[i][j] = num++;

        if(!dfs(matrix, i+dir[k][0], j+dir[k][1]))//向其余四个方向dfs
        {
            k = (k+1)%4;
            dfs(matrix, i+dir[k][0], j+dir[k][1]);
        }
        return true;
    }

    vector<vector<int>> generateMatrix(int n) 
    {
        vector<vector<int>> matrix(n, vector<int>(n, 0));
        dfs(matrix, 0, 0);
        return matrix;
    }
};
```

## 2 [54. 螺旋矩阵 - 力扣（LeetCode）](https://leetcode.cn/problems/spiral-matrix/)

| 变量            | 含义          |
| ------------- | ----------- |
| `starty`      | 当前圈最上面一行的索引 |
| `m - offsety` | 当前圈最下面一行的索引 |
| `startx`      | 当前圈最左边一列的索引 |
| `n - offsetx` | 当前圈最右边一列的索引 |
AC:
```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) 
    {
        int offsetx = 1;
        int offsety = 1;
        int startx = 0;
        int starty = 0;
        int cnt = 0;
        int i, j;

        int m = matrix.size(), n = matrix[0].size();
        vector<int> res(m*n);

        //完整的循环次数应该取决于较短的边
        int loop = min(m, n) /2;

        // while(cnt < m*n) 这个条件会死循环
        while(loop--)
        {
            i = starty;
            j = startx;

            for(; j < n-offsetx; j++)
                res[cnt++] = matrix[i][j];

            for(; i < m-offsety; i++)
                res[cnt++] = matrix[i][j];
            
            for(; j > startx; j--)
                res[cnt++] = matrix[i][j];
            
            for(; i > starty; i--)
                res[cnt++] = matrix[i][j];

            offsetx++;
            offsety++;
            startx++;
            starty++;
        }
		
		//处理剩下的一行或者一列
        if (starty == m - offsety) //开始等于结束
        {                // 只剩一行
            for (int j = startx; j <= n - offsetx; ++j)
                res[cnt++] = matrix[starty][j];
        } 
        else if (startx == n - offsetx) 
        {         // 只剩一列
            for (int i = starty; i <= m - offsety; ++i)
                res[cnt++] = matrix[i][startx];
        }

        return res;
    }
};
```

