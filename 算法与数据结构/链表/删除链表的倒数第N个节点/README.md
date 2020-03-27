## 19.删除链表的倒数第N个节点

给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

示例：

给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
说明：

给定的 n 保证是有效的。

进阶：

你能尝试使用一趟扫描实现吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
	//因为是倒数，所以我们得想办法在到达链表尾部的时候，能得到第n个节点的前驱节点
	//我们利用两个指针，两个指针间隔n，就能在第二个指针到达尾部（next==null）的时候，得到前驱节点
	//注意数字细节
	public ListNode removeNthFromEnd(ListNode head, int n) {
		//建个哨兵，避免处理删除头节点的情况
		ListNode sentinel = new ListNode(-1);
		sentinel.next = head;
		ListNode p1 = sentinel;
		ListNode p2 = sentinel;
		for (int i=0; i<n && p2 != null; i++) { //数n个节点
      p2 = p2.next;
		}
		if (p2 == null) { //n太大，超过链表长度
      return sentinel.next;
		}
		//两个指针一起移动
		while (p2.next != null) {
      p1 = p1.next;
      p2 = p2.next;
		}
		p1.next = p1.next.next;
		return sentinel.next;
	}
}
```

