[80. 删除有序数组中的重复项 II - 力扣（LeetCode）](https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii/description/?envType=study-plan-v2&envId=top-interview-150)

```cpp
class Solution 
{
    //双指针算法
    //p指向有序去重部分的后的第一位
    //q指向的数即将填入nums[p]中
    //一个元素一定是有序去重的
public:
    int removeDuplicates(vector<int>& nums) 
    {
        int n =nums.size();
        if(n == 1) return n;

        int p = 2, q = 2; //[0,1]区间里面一定是符合要求的
        while(q < n)
        {
            if(nums[q] != nums[p-2])
            {
                nums[p] = nums[q];
                p++;
            }
            q++;  //等于对的时候如果再赋值nums[p] = nums[q]，就会出现[p-2, p-1, p]三个下标中的值相同
        }
        return p;
    }
};
```