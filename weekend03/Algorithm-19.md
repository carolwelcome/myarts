---
title: Algorithm-19
date: 2019-04-04 15:38:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - Link
description: 
    Remove Nth Node From End of List。
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
	//使用快慢表的形式实现
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if(head==null||head.next==null) return null;
        if(n==0) return head;
        int k=0;
        ListNode slow=head,remove=head;
        while(head!=null){
            if(k>n){
                slow=slow.next;
            }
            k++;
            head=head.next;
        }
        if(k>n){//链表长度大于删除节点的位置
            //slow表示要删除结前的前驱节点
            slow.next=slow.next.next;
        }
        if(k==n){
            remove=slow.next;
        }
        return remove;
    }
    //哨兵实现方式，果然比我的代码简洁多了，高明
    public static ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode pioneer = head;
        for (int i=0;i<n-1;i++){
            pioneer = pioneer.next;
        }
        //哨兵
        ListNode pre = new ListNode(-1);
        ListNode start = pre;
        pre.next = head;
        while (pioneer.next!=null){
            pioneer = pioneer.next;
            pre = pre.next;
        }
        //执行删除操作
        pre.next = pre.next.next;
        return start.next;
    }
}
```