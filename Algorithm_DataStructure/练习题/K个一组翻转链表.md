
[25. K 个一组翻转链表 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-nodes-in-k-group/description/)

给你链表的头节点 `head` ，每 `k` 个节点一组进行翻转，请你返回修改后的链表。

`k` 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 `k` 的整数倍，那么请将最后剩余的节点保持原有顺序。

你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

**示例一**
![](../zPictureStore/Pastedimage20240314142525.jpg)
**输入：** `head = [1,2,3,4,5], k = 2`
**输出：** `[2,1,4,3,5]` 

**示例二**
![](../zPictureStore/Pastedimage20240314142643.jpg)

**输入：** `head = [1,2,3,4,5], k = 3`
**输出：** `[3,2,1,4,5]` 

**提示：**

- 链表中的节点数目为 `n`
- `1 <= k <= n <= 5000`
- `0 <= Node.val <= 1000`

**进阶：** 你可以设计一个只用 `O(1)` 额外内存空间的算法解决此问题吗？

## 题解

#链表操作 

像这样需要对链表进行操作的都需要一个新的头节点来记录原来头节点的位置，不然会在对链表操作的时候，丢失头节点的地址，或者需要分类讨论很麻烦。

像这样的局部操作，对链表反转时，返回头节点和尾节点的地址。

AC代码：
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution 
{
public:
//反转链表
    pair<ListNode*, ListNode*> reverseList(ListNode* head, ListNode* tt)
    {
        ListNode* tail = head;//初始的头节点，反转后变成尾节点
        ListNode* pre = nullptr, *cur = head;
        ListNode *q = nullptr;
		
        while(pre != tt)
        {
            q = cur->next;
            cur->next = pre;
            pre = cur;
            cur = q;
        }
        //return pre;
        return{pre, tail};
	}
	
    ListNode* reverseKGroup(ListNode* head, int k) 
    {
    //表为空或者表里只有一个元素，无需进行多余的操作
        if(head == nullptr || head->next == nullptr) return head;
		
        ListNode* newhead = new ListNode(-1, head);
        ListNode* pre = newhead, *cur = head;
		
        while(cur)
        {
            ListNode* tt = pre;
            for(int i=0; i<k; i++)
            {
                tt = tt->next;
                if(tt == nullptr) return newhead->next; 
            }
            ListNode* follow = tt->next;
			
            pair<ListNode*, ListNode*> res = reverseList(cur, tt);
            
            pre->next = res.first;
            res.second->next = follow;
			
            pre = res.second;
            cur = follow;
        }
		
        return newhead->next;
    }
};
```