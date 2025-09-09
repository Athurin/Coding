[347. 前 K 个高频元素 - 力扣（LeetCode）](https://leetcode.cn/problems/top-k-frequent-elements/description/)
AC:
```cpp
class Solution 
{
public:
    class cmp
    {
    public:
        bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs)
        {
            return lhs.second > rhs.second;
        } //默认从小到大排列
    };

    vector<int> topKFrequent(vector<int>& nums, int k) 
    {
        unordered_map<int, int> map;
        
        for(int i=0; i<nums.size(); i++)
        {
            map[nums[i]]++;
        }

        priority_queue<
            pair<int, int>, 
            vector<pair<int, int>>,
            cmp
            > prique; //小根堆

        for(unordered_map<int, int>::iterator it = map.begin(); it != map.end(); it++)
        {
            prique.push(*it);
            if(prique.size() > k) prique.pop();
        }
        
        vector<int> res(k);
        for(int i=k-1; i>=0; i--)
        {
            res[i] = prique.top().first;
            prique.pop();
        }
        return res;
    }
};
```




