[454. 四数相加 II - 力扣（LeetCode）](https://leetcode.cn/problems/4sum-ii/description/)

AC：
```cpp
class Solution 
{
public:
    int fourSumCount(vector<int>& A, vector<int>& B, vector<int>& C, vector<int>& D) 
    {
        unordered_map<int, int> umap;
        for(auto a : A)
            for(auto b : B)
                umap[a+b]++;
        
        int res = 0;
        for(auto c : C)
            for(auto d : D)
            {
                if(umap.find(0-(c+d)) != umap.end())
                    res += umap[0-c-d];
            }
        
        return res;
    }
};
```