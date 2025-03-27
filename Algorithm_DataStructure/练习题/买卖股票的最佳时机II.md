[122. 买卖股票的最佳时机 II - 力扣（LeetCode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/?envType=study-plan-v2&envId=top-interview-150)

前传[[买卖股票的最佳时机]]

#dp #贪心 

## 方法一：

如果只考虑最后获得的最大利润而不考虑购买方案，可以直接使用贪心。

核心思想就是：每一次上升的时候都记录下股票的涨幅，然后直接累加到利润里面。这个思想就是贪心，只计算盈利而忽略所有亏损。
```cpp
class Solution {
    
public:
    int maxProfit(vector<int>& prices) 
    {
        int n = prices.size();
        int sum = 0;
        for(int i=0; i<n-1; i++)
        {
            if(prices[i+1] > prices[i])
                sum += (prices[i+1] - prices[i]);
        }
        return sum;
    }
};
```

为什么说这个方法无法记录购买方案呢？
因为每一次我都只在意相邻两天是不是可以赚到钱。
比如`prices:[2, 5, 7, 9, 1]` 我可以在第一天买进，第四天卖出，实际上只有一次。但是在这个算法当中，买入买进分了三次完成。
所以这个算法的计算过程并不等价于最优方案。

## 方法二：

动态规划：
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        int dp[n][2];
        dp[0][0] = 0, dp[0][1] = -prices[0];
        for (int i = 1; i < n; ++i) {
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
        }
        return dp[n - 1][0];
    }
};
```