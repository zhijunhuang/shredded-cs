## 24.两两交换链表中的节点

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

 

示例:

给定 1->2->3->4, 你应该返回 2->1->4->3.

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/swap-nodes-in-pairs
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

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
  public ListNode swapPairs(ListNode head) {
    ListNode dummy = new ListNode(-1);
    dummy.next = head;
    ListNode pre = dummy; //下两个节点的前节点

    while (pre.next != null && pre.next.next != null) {
      //实在搞不清楚一堆next，就分别命名保存为临时变量
      ListNode n3 = pre.next.next.next; //3
      ListNode n2 = pre.next.next; //2
      ListNode n1 = pre.next; //1
      n1.next = n3;
      n2.next = n1;
      pre.next = n2;
      
      pre = n1; //下两个节点的前节点
    }
    return dummy.next;
  }
}
```

