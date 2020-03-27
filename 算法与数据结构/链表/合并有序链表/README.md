## 21.合并两个有序链表

将两个升序链表合并为一个新的升序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

示例：

输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/merge-two-sorted-lists
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

````java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
	public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode head = new ListNode(-1); //哨兵，val无所谓
    ListNode cur = head;
    while (l1 != null && l2 != null) {
      if (l1.val <= l2.val) {
        cur.next = l1;
        l1 = l1.next;
        cur = cur.next;
      } else {
        cur.next = l2;
        l2 = l2.next;
        cur = cur.next;
      }
    }
    if (l1 == null) {
      cur.next = l2;
    }
    if (l2 == null) {
      cur.next = l1;
    }
    
    return head.next;
	}
}
````



## 23.合并K个排序链表

合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

示例:

输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/merge-k-sorted-lists
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
  //移动指针不难，难的是两两比较，这里借助优先队列（堆），简化比较
	public ListNode mergeKLists(ListNode[] lists) {
    PriorityQueue<ListNode> queue = new PriorityQueue(new Comparator<ListNode>(){
      @Override
      public int compare(ListNode n1, ListNode n2) {
        return n1.val - n2.val;
      }
    });
    //建立哨兵
    ListNode head = new ListNode(-1);
    //把每个链表的头节点放到最小堆中
    for (ListNode list : lists) {
      if (list != null) queue.offer(list);
    }
    ListNode cur = head;
    while (!queue.isEmpty()) {
      ListNode tmp = queue.poll();
      cur.next = tmp;
      cur = cur.next;
      if (tmp.next != null) queue.offer(tmp.next);
    }
    return head.next;
  }
}
```

