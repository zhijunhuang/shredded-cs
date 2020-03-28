## 876.链表的中间节点

给定一个带有头结点 head 的非空单链表，返回链表的中间结点。

如果有两个中间结点，则返回第二个中间结点。

 

示例 1：

输入：[1,2,3,4,5]
输出：此列表中的结点 3 (序列化形式：[3,4,5])
返回的结点值为 3 。 (测评系统对该结点序列化表述是 [3,4,5])。
注意，我们返回了一个 ListNode 类型的对象 ans，这样：
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, 以及 ans.next.next.next = NULL.
示例 2：

输入：[1,2,3,4,5,6]
输出：此列表中的结点 4 (序列化形式：[4,5,6])
由于该列表有两个中间结点，值分别为 3 和 4，我们返回第二个结点。


提示：

给定链表的结点数介于 1 和 100 之间。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/middle-of-the-linked-list
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
  //还是用快慢指针，每次慢指针走一步，快指针走两步，快指针到尾部（next==null）时，慢指针指向中间节点
  //注意充分利用举例，判断好边界
  public ListNode middleNode(ListNode head) {
    ListNode slowPointer = head;
    ListNode quickPointer = head;
    while (slowPointer != null && quickPointer != null) {
      if (quickPointer.next == null) { //奇数,第一个例子5的位置
        break;
      }
      if (quickPointer.next.next == null) { //偶数，第二个例子5的位置
        slowPointer = slowPointer.next;
        break;
      }
      quickPointer = quickPointer.next.next;
      slowPointer = slowPointer.next;
    }
    return slowPointer;
  }
}
```

写得太复杂了，参照<https://github.com/andavid/leetcode-java/tree/master/note/234>，重新写了一版

```java
class Solution {
  public ListNode middleNode(ListNode head) {
    ListNode slow=head, fast=head;
    while (fast != null && fast.next != null) {
      fast = fast.next.next;
      slow = slow.next;
    }
    //次数fast为null为偶数，不为null为奇数
    return slow;
  }
}
```





## 234.回文链表

请判断一个链表是否为回文链表。

示例 1:

输入: 1->2
输出: false
示例 2:

输入: 1->2->2->1
输出: true
进阶：
你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/palindrome-linked-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

借助寻找中间节点和反转的技巧，我们来解这题。

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
  //边找中间节点，先把中间节点之前的链表反转，找到中间节点后再对比两个方向链表的值
  public boolean isPalindrome(ListNode head) {
    ListNode p1=head, p2=head, p1Pre = null, p1Next;
    ListNode left=null, right=null;
    while (p1 != null && p2 != null) {
      if (p2.next == null) { //奇数节点，此时p1Pre为左链表，p1.next为右链表
        left = p1Pre;
        right = p1.next;
        break;
      } 
      //偶数节点,此时p1还得反转一次，p1Pre为左链表，p1为右链表
      if (p2.next.next == null) { 
        p1Next = p1.next; 
        p1.next = p1Pre;
        p1Pre = p1;
        p1 = p1Next;
        
        left = p1Pre;
        right = p1;
        break;
      } 
      p2 = p2.next.next;
      //反转p1
      p1Next = p1.next; 
      p1.next = p1Pre;
      p1Pre = p1;
      p1 = p1Next;
    }
    //开始遍历左右链表，比较回文
    while (left != null && right != null) {
      if (left.val != right.val) {
        return false;
      }
      left = left.next;
      right = right.next;
    }
    return true;
    //写完之后用空链表、一个节点链表、两个节点链表检查一下边界条件
  }
}
```

写得太复杂了，参照<https://github.com/andavid/leetcode-java/tree/master/note/234>，重新写了一版

```java
class Solution {
  public boolean isPalindrome(ListNode head) {
    ListNode slow=head, fast=head, pre=null, next;
    while (fast != null && fast.next != null) {
      fast = fast.next.next;
      //反转slow
      next = slow.next;
      slow.next = pre;
      pre = slow;
      slow = next;
    }
    //当为奇数时，slow指向中间节点，还得移动一位
    if (fast != null) {
      slow = slow.next;
    }
    //开始回文比较
    while (slow != null && pre != null) {
      if (slow.val != pre.val) {
        return false;
      }
      slow = slow.next;
      pre = pre.next;
    }
    return true;
  }
}
```

