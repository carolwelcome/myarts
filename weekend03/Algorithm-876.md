---
title: Algorithm-876
date: 2019-04-05 00:04:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - Link
description: 
    Middle of the Linked List.
---
```Java

/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    //执行用时 : 0 ms, 在Middle of the Linked List的Java提交中击败了100.00% 的用户
    //内存消耗 : 35.4 MB, 在Middle of the Linked List的Java提交中击败了0.94% 的用户
    public ListNode middleNode(ListNode head) {
        if(head==null||head.next==null) return head;
        int len=0;
        ListNode middle=head;
        while(head!=null){
            len++;
            head=head.next;
        }
        for(int i=1;i<Math.round(len/2)+1;i++){
            middle=middle.next;
        }
        return middle;
    }
    //记不记得有一招从天而降的套路：快慢指针
    //执行用时 : 0 ms, 在Middle of the Linked List的Java提交中击败了100.00% 的用户
    //内存消耗 : 36.2 MB, 在Middle of the Linked List的Java提交中击败了0.94% 的用户
    public ListNode middleNode(ListNode head) {
        if(head==null||head.next==null) return head;
        if(head.next.next==null) return head.next;
        ListNode slowy=head,quick=head;
        while(quick!=null&&quick.next!=null){
            slowy=slowy.next;
            quick=quick.next.next;
        }
        return slowy;
    }
    //我屮，再省一个变量，漂亮
    //执行用时 : 0 ms, 在Middle of the Linked List的Java提交中击败了100.00% 的用户
    //内存消耗 : 33.5 MB, 在Middle of the Linked List的Java提交中击败了0.94% 的用户

    public ListNode middleNode(ListNode head) {
        if(head==null||head.next==null) return head;
        if(head.next.next==null) return head.next;
        ListNode quick=head;
        while(quick!=null&&quick.next!=null){
            head=head.next;
            quick=quick.next.next;
        }
        return head;
    }
}
```