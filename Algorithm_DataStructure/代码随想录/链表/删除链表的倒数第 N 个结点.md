[19. 删除链表的倒数第 N 个结点 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

难点在于如何快速确定链表的倒数的节点是哪一个。
由于链表访问是单向的，且时间复杂度是O(n)。
所以普通的搜索至少需要O（2n)。

双指针算法降低到O（n）

删除链表的倒数第n个节点，双指针法，保持窗口的大小始终等于n+1

1. fast 指针先走 n + 1 步。
2. slow 指针和fast指针同时向后走。fast - slow == n+1
3. 当fast指向nullptr的时候，slow指向倒数第n+1个。
4. 删除slow指针的后一个节点，即倒数第n个节点。

AC1： 
针对题目数据范围给出的便利没有做出判断
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`
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
    ListNode* removeNthFromEnd(ListNode* head, int n) 
    {
        ListNode* dummyhead = new ListNode(0, head);
        ListNode* fast = dummyhead;
        ListNode* slow = dummyhead;
        
        while(n-- /*&& fast != nullptr*/) //fast走了n步
            fast = fast->next;
        
        if(fast) fast = fast->next; //再走一步

        while(fast != nullptr)
        {
            fast = fast->next;
            slow = slow->next;
        }
        
        ListNode* tmp = slow->next;
        if(tmp) slow->next = tmp->next;
        delete tmp;

        //delete dummyhead;
        //return head;head可能被释放
        return dummyhead->next;
    }
};
```

AC2: 假设  n > 链表长度 
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
    ListNode* removeNthFromEnd(ListNode* head, int n) 
    {
        ListNode* dummyhead = new ListNode(0, head);
        ListNode* fast = dummyhead;
        ListNode* slow = dummyhead;
        
        //写法一：由于题目条件的特殊性不会走到fast == nullptr因此不用判断
        // while(n-- /*&& fast != nullptr*/) //fast走了n步 
        //     fast = fast->next;

        //写法二：如果 n > 链表长度
        while(n-- && fast != nullptr) //fast走了n步 
            fast = fast->next;
        if(n > 0) return dummyhead->next;
        
        if(fast) fast = fast->next; //再走一步

        while(fast != nullptr)
        {
            fast = fast->next;
            slow = slow->next;
        }
        
        ListNode* tmp = slow->next;
        if(tmp) slow->next = tmp->next;
        delete tmp;

        //delete dummyhead;
        //return head;head可能被释放
        return dummyhead->next;
    }
};
```

