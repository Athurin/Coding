[92. 反转链表 II - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-linked-list-ii/description/)

给你单链表的头指针 `head` 和两个整数 `left` 和 `right` ，其中 `left <= right` 。请你反转从位置 `left` 到位置 `right` 的链表节点，返回 **反转后的链表** 。

**示例1**
![[Pastedimage20240310170941.jpg]]

**输入：** `head = [1,2,3,4,5], left = 2, right = 4`
**输出：** `[1,4,3,2,5]` 

**示例2**

**输入：** `head = [5], left = 1, right = 1`
**输出：** `[5]` 

**提示：**

- 链表中节点数目为 `n`
- `1 <= n <= 500`
- `-500 <= Node.val <= 500`
- `1 <= left <= right <= n`

**进阶：** 你可以使用一趟扫描完成反转吗？

> [!NOTE]
> 链表的操作问题，一般而言面试（机试）的时候不允许我们修改节点的值，而只能修改节点的指向操作。
> 
> 思路通常都不难，写对链表问题的技巧是：一定要先想清楚思路，并且必要的时候在草稿纸上画图，理清「穿针引线」的先后步骤，然后再编码。


## 题解

#穿针引线 #链表 


### 方法一
以下图中黄色区域的链表反转为例。
![[Pastedimage20240311080435.png]]使用「[206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)」的解法，反转 `left` 到 `right` 部分以后，再拼接起来。我们还需要记录 `left` 的前一个节点，和 `right` 的后一个节点。如图所示：
![[Pastedimage20240311080518.png]]**算法步骤：**

- 第 1 步：先将待反转的区域反转；
- 第 2 步：把 `pre` 的 `next` 指针指向反转以后的链表头节点，把反转以后的链表的尾节点的 `next` 指针指向 `succ`。

![[Pastedimage20240311080600.png]]

最开始通过的30/44的代码；当`head = [3, 5]`时错误输出`[3]`， 应该输出`[5, 3]` .
```cpp
class Solution
{

public:
    ListNode* reverseBetween(ListNode* head, int left, int right)
    {
        ListNode* pre = head, *curr = head;
        ListNode* outl = head, *outr = head;

        int i = 0;
        while(i < left-1)
        {
            if(i == left-2) outl = pre;
            i++;
            pre = pre->next;
        }

        ListNode* prefirst = pre;
        curr = pre->next;
        i = 0;
        while(i < right)
        {
            i++;
            outr = outr->next;
        }
        for(int num = left+1; num <= right; num++)
        {
            ListNode* q = curr->next;
            curr -> next = pre;
            pre = curr;
            curr = q;
        }

        outl->next = pre;
        prefirst->next = outr;
        return head;
    }
};
```

既然能过一部分数据，就说明这个单链表的头节点是有效的，物理上`left-1` 表示逻辑`left` . 和C++数组下标一样。

如果用以上的方法就避免不了复杂的分类讨论。 于是可以考虑增加一个虚拟的头节点，这个时候，虚拟头节点的数据域是无效的。而原来的头节点则是充当了第一个有效的头节点。

AC代码：
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */

class Solution
{
private:

//参考反转链表的第一个例子也可以用递归的方法
    ListNode* reverseList(ListNode* head)
    {
        ListNode* pre = NULL;
        ListNode* cur = head;

        while(cur != NULL)
        {
            ListNode* q = cur->next;
            cur->next = pre;
            pre = cur;
            cur = q;
        }
        return pre;
    }

public:

    ListNode* reverseBetween(ListNode* head, int left, int right)
    {
    //虚拟头节点
        ListNode* newhead = new ListNode();
        newhead -> next = head;

		//左端点的左边一个和右端点的右边一个
        ListNode* outl = newhead, *outr = newhead;
       
       //区间左端点和右端点
        ListNode* leftNode = head, *rightNode = head;
  

        //从虚拟节点走步到left的前一个节点
        for(int i=0; i<left-1; i++)
        {
            outl = outl->next;
        }
        leftNode = outl->next;

        outr = outl;
        for(int i=left-1; i<right+1; i++)
        {
            if(i == right) rightNode = outr;
            outr = outr->next;
        }

//截断链表
        outl->next = nullptr;
        //中间要反转的部分人看成是一个完整的链表，因此尾节点的指针域置空
        rightNode->next = nullptr;

//链接链表
        ListNode* tmphead = reverseList(leftNode);
        outl->next = tmphead;
        leftNode->next = outr;

        return newhead->next;

    }

};
```

复杂度分析

时间复杂度：O(N)，其中 N 是链表总节点数。最坏情况下，需要遍历整个链表。
空间复杂度：O(1)。只使用到常数个变量。


### 方法二

#### 一次遍历「穿针引线」反转链表（头插法）

方法一的缺点是：如果 `left` 和 `right` 的区域很大，恰好是链表的头节点和尾节点时，找到 `left` 和 `right` 需要遍历一次，反转它们之间的链表还需要遍历一次，虽然总的时间复杂度为 O(N)，但遍历了链表 2 次，可不可以只遍历一次呢？答案是可以的。依然画图进行说明。

以方法一的示例为例进行说明。
![[Pastedimage20240311091042.png]]

整体思想是：在需要反转的区间里，每遍历到一个节点，让这个新节点来到反转部分的起始位置。下面的图展示了整个流程。

![[Pastedimage20240311091110.png]]

下面我们具体解释如何实现。使用三个指针变量 `pre`、`curr`、`next` 来记录反转的过程中需要的变量，它们的意义如下：

- `curr`：指向待反转区域的第一个节点 `left`；
- `next`：永远指向 `curr` 的下一个节点，循环过程中，`curr` 变化以后 `next` 会变化；
- `pre`：永远指向待反转区域的第一个节点 `left` 的前一个节点，在循环过程中不变。

第 1 步，我们使用 ①、②、③ 标注「穿针引线」的步骤。
![[Pastedimage20240311091150.png]]

**操作步骤**：

- 先将 `curr` 的下一个节点记录为 `next`；
- 执行操作 ①：把 `curr` 的下一个节点指向 `next` 的下一个节点；
- 执行操作 ②：把 `next` 的下一个节点指向 `pre` 的下一个节点；
- 执行操作 ③：把 `pre` 的下一个节点指向 `next`。

第 1 步完成以后「拉直」的效果如下：
![[Pastedimage20240311091208.png]]

第 2 步，同理。同样需要注意 **「穿针引线」操作的先后顺序**。
![[Pastedimage20240311091223.png]]
第 2 步完成以后「拉直」的效果如下：
![[Pastedimage20240311091235.png]]

第 3 步，同理。
![[Pastedimage20240311091248.png]]

第 3 步完成以后「拉直」的效果如下：
![[Pastedimage20240311091259.png]]

**复杂度分析**：

- 时间复杂度：O(N)，其中 N 是链表总节点数。最多只遍历了链表一次，就完成了反转。

- 空间复杂度：O(1)。只使用到常数个变量。

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */

class Solution
{
public:
    ListNode* reverseBetween(ListNode* head, int left, int right)
    {
        ListNode* newhead = new ListNode();
        newhead->next = head;
        ListNode* pre=newhead;

        for(int j=0; j<left-1; j++) pre = pre->next;

        ListNode* cur = pre->next;
        ListNode* q = cur->next;

        for(int j=1; j<=(right-left); j++)
        {
            q = cur->next;
            
           //其他操作次序总是会出错
            cur->next = q->next;
            q->next = pre->next;
            pre->next = q;
        }

        return newhead->next;
    }
};
```

运行结果，小小的骄傲一下：
![[Pastedimage20240311192521.png]]


==如果把for循环改成这样==
```cpp
for(int j=1; j<=(right-left); j++)
        {

            q = cur->next;pre->next = q;

            cur->next = q->next;
            q->next = pre->next;
        }
```
虽然逻辑上面并没有什么问题。但是会报错，目前好像只有在leetcode出现过这种错误。

> [!NOTE]
> ================================================================= ==22==ERROR: AddressSanitizer: heap-use-after-free on address 0x5020000000b8 at pc 0x55f2a85280e1 bp 0x7fff63fa3460 sp 0x7fff63fa3458 READ of size 8 at 0x5020000000b8 thread T0 #2 0x7f493d703d8f (/lib/x86_64-linux-gnu/libc.so.6+0x29d8f) (BuildId: c289da5071a3399de893d2af81d6a30c62646e1e) #3 0x7f493d703e3f (/lib/x86_64-linux-gnu/libc.so.6+0x29e3f) (BuildId: c289da5071a3399de893d2af81d6a30c62646e1e) 0x5020000000b8 is located 8 bytes inside of 16-byte region [0x5020000000b0,0x5020000000c0) freed by thread T0 here: #3 0x7f493d703d8f (/lib/x86_64-linux-gnu/libc.so.6+0x29d8f) (BuildId: c289da5071a3399de893d2af81d6a30c62646e1e) previously allocated by thread T0 here: #4 0x7f493d703d8f (/lib/x86_64-linux-gnu/libc.so.6+0x29d8f) (BuildId: c289da5071a3399de893d2af81d6a30c62646e1e) Shadow bytes around the buggy address: 0x501ffffffe00: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 0x501ffffffe80: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 0x501fffffff00: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 0x501fffffff80: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 0x502000000000: fa fa fd fa fa fa fd fa fa fa fd fd fa fa 00 00 =>0x502000000080: fa fa 00 00 fa fa fd[fd]fa fa 00 00 fa fa 00 00 0x502000000100: fa fa fd fd fa fa fd fd fa fa fa fa fa fa fa fa 0x502000000180: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa 0x502000000200: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa 0x502000000280: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa 0x502000000300: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa Shadow byte legend (one shadow byte represents 8 application bytes): Addressable: 00 Partially addressable: 01 02 03 04 05 06 07 Heap left redzone: fa Freed heap region: fd Stack left redzone: f1 Stack mid redzone: f2 Stack right redzone: f3 Stack after return: f5 Stack use after scope: f8 Global redzone: f9 Global init order: f6 Poisoned by user: f7 Container overflow: fc Array cookie: ac Intra object redzone: bb ASan internal: fe Left alloca redzone: ca Right alloca redzone: cb ==22==ABORTING

