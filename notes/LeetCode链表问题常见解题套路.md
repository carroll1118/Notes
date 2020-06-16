@[toc]
#  160. 相交链表
* [原题地址](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

```java
class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA==null||headB==null){
            return null;
        }

        ListNode pA = headA;
        ListNode pB = headB;
        while(pA != pB){
            pA = pA==null ? headB:pA.next;
            pB = pB==null ? headA:pB.next;
        }
        return pA;
    }
}
```
#  141. 环形链表
* [原题地址](https://leetcode-cn.com/problems/linked-list-cycle/)

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head == null || head.next == null)   return false;
        ListNode fast = head.next;
        ListNode slow = head;
        while(fast != slow){
            if(fast.next==null || fast.next.next==null) return false;
            fast = fast.next.next;
            slow = slow.next;
        }
        return true;
    }
}
```
#  142. 环形链表 II
* [原题地址](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while(true){
            if(fast==null || fast.next==null){
                return null;
            }
            fast = fast.next.next;
            slow = slow.next;
            if(fast==slow){
                break;
            }
        }
        fast = head;
        while(fast != slow){
            slow = slow.next;
            fast = fast.next;
        }
        return slow;
    }
}
```
#  206. 反转链表
* [原题地址](https://leetcode-cn.com/problems/reverse-linked-list/)
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode first = new ListNode(-1);
        ListNode curr = null;
        while(head!=null){
            //首先保存当前节点的下一个节点
            curr = head.next;
            //将当前节点的下一个节点指向前一个节点
            head.next = first.next;
            //然后将当前节点保存起来，下一个当前节点的前一个节点
            first.next = head;
            //改变当前节点
            head = curr;
        }
        return first.next;
    }
}
```

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head==null) return head;
        return reverse(head);
    }

    private ListNode reverse(ListNode head){
        if(head.next==null) return head;
        ListNode last = reverse(head.next);
        head.next.next = head;
        head.next = null;
        return last;
    }
}
```

#  92. 反转链表 II
* [原题地址](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

```java
class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;

        ListNode g = dummyHead;
        ListNode p = dummyHead.next;

        int step = 0;
        while (step < m - 1) {
            g = g.next; 
            p = p.next;
            step ++;
        }

        for (int i = 0; i < n - m; i++) {
            ListNode removed = p.next;
            p.next = p.next.next;

            removed.next = g.next;
            g.next = removed;
        }

        return dummyHead.next;
    }
}
```
**你知道的越多，你不知道的越多。
有道无术，术尚可求，有术无道，止于术。
如有其它问题，欢迎大家留言，我们一起讨论，一起学习，一起进步**
