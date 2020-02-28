# 旋转链表 解题思路

## **题目：**

给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。

示例 1:

输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL
示例 2:

输入: 0->1->2->NULL, k = 4
输出: 2->0->1->NULL
解释:
向右旋转 1 步: 2->0->1->NULL
向右旋转 2 步: 1->2->0->NULL
向右旋转 3 步: 0->1->2->NULL
向右旋转 4 步: 2->0->1->NULL

 * ## **思路：**

   链表中的点已经相连，一次旋转操作意味着：

   - 先将链表闭合成环
   
   - 找到相应的位置断开这个环，确定新的链表头和链表尾
   
     ![61.png](https://pic.leetcode-cn.com/e3371c6b03e3c8d3758dcf0b35a45d0a6b39c111373cf7b5bde53e14b6271a04-61.png)
   
   **新的链表头在哪里？**
   
   在位置 n-k 处，其中 n 是链表中点的个数，新的链表尾就在头的前面，位于位置 n-k-1。
   
   我们这里假设 k < n
   
   **如果 k >= n 怎么处理？**
   
   k 可以被写成 k = (k // n) * n + k % n 两者加和的形式，其中前面的部分不影响最终的结果，因此只需要考虑 k%n 的部分，这个值一定比 n 小。
   
   **算法实现很直接：**
   
   找到旧的尾部并将其与链表头相连 old_tail.next = head，整个链表闭合成环，同时计算出链表的长度 n。
   找到新的尾部，第 (n - k % n - 1) 个节点 ，新的链表头是第 (n - k % n) 个节点。
   断开环 new_tail.next = None，并返回新的链表头 new_head。
   
   ## **代码**:

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null) return null;
    if (head.next == null) return head;
    
        ListNode first = head;
        int n;
        //将头和尾连起来
        for(n = 1;first.next != null;n++){
            first = first.next;
        }
        first.next = head; 
        ListNode second = head;
        for(int i = 0;i<n - k % n - 1;i++){
            second = second.next;
        }
        ListNode newHead = second.next;
        second.next = null;
       
        return newHead;
    }
}
```

**复杂度分析**

- 时间复杂度：O(N)*O*(*N*)，其中 N*N* 是链表中的元素个数
- 空间复杂度：O(1)*O*(1)，因为只需要常数的空间