## 83.删除排序链表中的重复元素

给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

示例 1:

输入: 1->1->2
输出: 1->2
示例 2:

输入: 1->1->2->3->3
输出: 1->2->3

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list
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
  //练习删除操作的题
	public ListNode deleteDuplicates(ListNode head) {
    ListNode cur = head;
    while (cur != null && cur.next != null) {
      if (cur.val == cur.next.val) { //删除next
        cur.next = cur.next.next;
      } else { //直到没有重复才移动
        cur = cur.next;
      }
    }
    return head;
	}
}
```



## 82.删除排序列表的重复元素II

给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中 没有重复出现 的数字。

示例 1:

输入: 1->2->3->3->4->4->5
输出: 1->2->5
示例 2:

输入: 1->1->1->2->3
输出: 2->3

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii
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
	public ListNode deleteDuplicates(ListNode head) {
		ListNode sentinel = new ListNode(-1);
    sentinel.next = head;
    ListNode cur = sentinel;
    while (cur.next != null && cur.next.next != null) {
      ListNode tmp = cur.next;
      if (tmp.val == tmp.next.val) { 
        //找到第一个不等的
        do {
          tmp = tmp.next;
        } while (tmp != null && tmp.next != null && tmp.val == tmp.next.val);
        cur.next = tmp.next;
      } else {
        cur = cur.next;
      }
    }
    
    return sentinel.next;
  }
}
```

