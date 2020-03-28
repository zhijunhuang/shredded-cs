## 206.反转一个单链表。

示例:

输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
进阶:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-linked-list
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
    //迭代版本
    public ListNode reverseList1(ListNode head) {
        ListNode pre=null, cur = head;
        while(cur != null) {
            ListNode next = cur.next; //暂存next
            cur.next = pre; //当前节点的next指向pre
            pre = cur; //pre指到当前（最后一个）节点
            cur = next; //cur移到next
        }
        return pre;
    }
    
    //递归版本1，借助辅助函数和成员变量,这个比较容易想到
    ListNode newHead;
    public ListNode reverseList2(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        recursiveReverseList(head);
        
        return newHead;
    }
    
    private ListNode recursiveReverseList(ListNode node) {
        if (node == null || node.next == null) {
            return node;
        }
        ListNode next = recursiveReverseList(node.next);
        if (newHead == null) {
            newHead = next;
        }
        //反转next和node
        next.next = node;
        node.next = null;
        return node;
    }
    
    //递归版本2，建设递归方法返回的是新的头指针
    //Nm->Nn<-Np, 因为Nm.next == Nn, 那就意味着反转操作Nn.next = Nm的操作等同于Nm.next.next = Nm
    public ListNode reverseList(ListNode node) {
        if (node == null || node.next == null) {
            return node;
        }
        ListNode head = reverseList(node.next);
        node.next.next = node;
        node.next = null;
        return head;
    }
}
```



## 92.反转链表II

反转从位置 m 到 n 的链表。请使用一趟扫描完成反转。

说明:
1 ≤ m ≤ n ≤ 链表长度。

示例:

输入: 1->2->3->4->5->NULL, m = 2, n = 4
输出: 1->4->3->2->5->NULL

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-linked-list-ii
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
    public ListNode reverseBetween(ListNode head, int m, int n) {
      if (head == null) {
        return head;
      }
      assert(m >= 1 && m <= n);
      int i=1; //从第一个节点开始数，这是第1个细节
      ListNode mPre = null;
      ListNode mNode = head;
      //寻找第m个节点和前节点
      while (i < m && mNode != null) { //这里小于m，这是第2个细节
        mPre = mNode;
        mNode = mNode.next;
        i++;
      }
      //找到尾巴了，没有需要反转的
      if (mNode == null) {
        return head;
      }
      //开始反转
      ListNode pre = null;
      ListNode cur = mNode;
      ListNode next = null;
      while (i <= n && cur != null) { //反转完第n个节点结束，<=是第3个细节
        next = cur.next;
        cur.next = pre;
        pre = cur;
        cur = next;
        i++;
      }
      //pre为局部反转的head，cur为剩余队列
      if (mPre != null) { //这里是第4个细节，考虑从第一个开始反转的情况
        mPre.next = pre; //头接上
      } else {
        head = pre; //没有前序节点，就得更新head指向pre
      }
      mNode.next = cur; //尾接上
      return head;
    }
}
```



