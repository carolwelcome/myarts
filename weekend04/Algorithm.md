---
title:leetcode-707,234,160,203,83,237,148
description:leetcode上所有简单的题目

```Java

//回文链表-234:最终还是向自己屈服了，其实我想用递归，但是想了好久也没有想通。但我不会放弃。
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
//执行用时 : 2 ms, 在Palindrome Linked List的Java提交中击败了88.79% 的用户
//内存消耗 : 46.5 MB, 在Palindrome Linked List的Java提交中击败了15.05% 的用户
class Solution {
   public  ListNode reverse01(ListNode head){
        ListNode next=null;
        ListNode pre=null;
        while(head!=null){
            next=head.next;
            head.next=pre;
            pre=head;
            head=next;
        }
        return pre;
    }
   public boolean isPalindrome(ListNode head) {
        if(head==null) return true;
        ListNode dataNode=head;
        ListNode headNode=head;
        while(head.next!=null&&head.next.next!=null){//时间复杂度n
            //leng++;
            dataNode=dataNode.next;
            head=head.next.next;
        }
        dataNode=reverse01(dataNode.next);
        while(dataNode!=null){
            if(headNode.val!=dataNode.val) return false;
            headNode=headNode.next;
            dataNode=dataNode.next;
        }
        return true;
    }
    
}

//回文链表-234:果其不然，我还是找到了递归的实现代码,哈哈，虽说时间复杂度差点，还是挺高兴的
//执行用时 : 4 ms, 在Palindrome Linked List的Java提交中击败了31.04% 的用户
//内存消耗 : 46.6 MB, 在Palindrome Linked List的Java提交中击败了15.81% 的用户
public class Solution{
	ListNode tmp;
	public boolean isPalindrome(ListNode head) {
        tmp=head;
        return isPalindromeTrue(head);
    }

    public boolean isPalindromeTrue(ListNode head){
    	if(head==null) return true;
    	boolean result=isPalindromeTrue(head.next)&(tmp.val==head.val);
    	tmp=tmp.next;
    	return result;
    }
}
}
```