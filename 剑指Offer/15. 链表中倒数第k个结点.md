题目描述
输入一个链表，输出该链表中倒数第k个结点。


Solution:(java)
```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    /*
    Idea: Use two pointers first, second and let |first - second | = k - 1, 
            the second pointer will point to the last k node when the first pointer arrive at the end.
    */
    public ListNode FindKthToTail(ListNode head,int k) {
        if (head == null || k < 1){
            return null;
        }
        ListNode firstPointer = head;
        while (firstPointer.next != null && k > 1){
            firstPointer = firstPointer.next;
            k --;
        }
        if (firstPointer.next == null && k > 1){
            return null;
        }
        ListNode secondPointer = head;
        while (firstPointer.next != null){
            firstPointer = firstPointer.next;
            secondPointer = secondPointer.next;
        }
        return secondPointer;
    }
}
```
