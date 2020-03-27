## 141.环形链表

给定一个链表，判断链表中是否有环。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

 

示例 1：

输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。


示例 2：

输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。


示例 3：

输入：head = [1], pos = -1
输出：false
解释：链表中没有环。




进阶：

你能用 O(1)（即，常量）内存解决此问题吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/linked-list-cycle
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
  public boolean hasCycle(ListNode head) {
  	return method2(head);
  }
  
  //方法1:快慢指针，空间复杂度O(1),每次快指针移动两步，慢指针移动一步，如果两指针相等则有环
  public boolean method1(ListNode head) {
    ListNode quickPointer = head;
    ListNode slowPointer = head;
    while (quickPointer != null && slowPointer != null) {
      if (quickPointer.next != null && quickPointer.next.next != null) {
        quickPointer = quickPointer.next.next;
      } else {
        return false;
      }
      slowPointer = slowPointer.next;
      if (quickPointer == slowPointer) {
        return true;
      }
    }
    return false;
  }
  
  //方法2:集合方法，每访问一个节点放到set里，如果里面节点存在，则有环，空间复杂度O(n)
  public boolean method2(ListNode head) {
    Set<ListNode> nodeSet = new HashSet();
    ListNode cur = head;
    while (cur != null) {
      if (nodeSet.contains(cur)) {
        return true;
      }
      nodeSet.add(cur);
      cur = cur.next;
    }
    return false;
  }
  
  //方法3：标记法，将每个val标记成一个不可能出现的值，然后检查到这个值存在就表明有环,空间复杂度O(1)
  public boolean method3(ListNode head) {
    ListNode cur = head;
    while (cur != null) {
      if (cur.val == Integer.MIN_VALUE) {
        return true;
      }
      cur.val = Integer.MIN_VALUE;
      cur = cur.next;
    }
    return false;
  }
  
  //方法4:节点摘除法，每遍历一个节点则把这个节点摘除，摘除方式是将其next指向自己
  //如果有环存在，势必有两个节点指向同一个节点，通过检查该节点是否是摘除节点（next指向自己）就能判断
  //空间复杂度O(1)
  public boolean method4(ListNode head) {
    ListNode cur = head;
    ListNode next;
    while (cur != null) {
      if (cur.next == cur) {
        return true;
      }
      next = cur.next;
      cur.next = cur; //摘除
      cur = next;
    }
    return false;
  }
}
```



## 142.环形链表II

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

说明：不允许修改给定的链表。

**进阶：**
你是否可以不用额外空间解决此题？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/linked-list-cycle-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

分析：因为不允许修改链表，那标记法和节点摘除法就不能使用了，快慢指针直觉上也不能保证相等的地方就是入口节点，所以上述检测方法唯一能用的就剩集合方法。

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
	public ListNode detectCycle(ListNode head) {
		return method1(head);
  }
  
  //集合法
  private ListNode method1(ListNode head) {
    Set<ListNode> nodes = new HashSet();
    while (head != null) {
      if (nodes.contains(head)) {
        return head;
      }
      nodes.add(head);
      head = head.next;
    }
    return null;
  }
  
  //快慢指针数步数法：当快指针赶上慢指针的时候，快指针走的步数是慢指针的两倍，
  //假设慢指针已经走了m步，m=L+R，
  //L是慢指针从头走到入口节点处的步数，R是慢指针从入口处走到与快指针相遇的步数
  //因为快指针步数是慢指针的两倍，那就意味着从相遇处，慢指针如果走了m步会回到相遇处，
  //那慢指针从相遇处如果走了L步呢，就会走到入口处（m-R=L）
  //基于这个事实，那当快慢指针相遇的时候，慢指针接着走，另外有个指针从head开始走，直到两个指针指向同一个节点
  //这个节点就是入口节点
  private ListNode method2(ListNode head) {
    ListNode slowPointer = head;
    ListNode quickPointer = head;
    while (slowPointer != null && quickPointer != null) {
      if (quickPointer.next != null && quickPointer.next.next != null) {
        quickPointer = quickPointer.next.next;
      } else {
        return null;
      }
      slowPointer = slowPointer.next;
      if (quickPointer == slowPointer) {
        break;
      }
    }
    //慢指针接着走，另一个指针从头走，直到两者相遇
    ListNode cur = head;
    while (true) {
      if (cur == slowPointer) {
        return cur;
      }
      cur = cur.next;
      slowPointer = slowPointer.next;
    }
  }
}
```



