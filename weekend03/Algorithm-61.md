---
title: Algorithm-61
date: 2019-04-03 11:38:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - Link
description: 
    给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。
---
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
 ```Java
class Solution {
    //超出时间限制了，原因是在哪儿呢，一个链表的长度是有限的，旋转n次，如果n很大，很多次都是无用功！！
    public ListNode rotateRight(ListNode head, int k) {
        if(head==null||head.next==null) return head;
        ListNode rotate=head;
        ListNode dataNode=null;
        for(int i=0;i<k;i++){
            while(head.next!=null){
                if(head.next.next==null){//倒数第二个节点
                    //head表示当前节点
                    dataNode=head.next;
                    head.next=null;
                    dataNode.next=rotate;
                    head=rotate=dataNode;
                    dataNode=null;
                    break;
                }
                head=head.next;
            }
        }
        return head;

        
    }
    //执行用时 : 4 ms, 在Rotate List的Java提交中击败了100.00% 的用户
    //内存消耗 : 35 MB, 在Rotate List的Java提交中击败了0.84% 的用户
    public ListNode rotateRight(ListNode head, int k) {
        if(head==null||head.next==null) return head;
        ListNode rotate=head,leng=head;
        ListNode dataNode=null;
        int len=0;
        while(leng!=null){
            len++;
            leng=leng.next;
        }
        leng=null;
        for(int i=0;i<k%len;i++){//循环体内部可能写的有点啰嗦了
            while(head.next!=null){
                if(head.next.next==null){//倒数第二个节点
                    //head表示当前节点
                    dataNode=head.next;
                    head.next=null;
                    dataNode.next=rotate;
                    head=rotate=dataNode;
                    dataNode=null;
                    break;
                }
                head=head.next;
            }
        }
        return head;

        
    }
    //2ms
    public ListNode rotateRight(ListNode head, int k) {
        if(head==null || head.next==null) return head;
        ListNode old_end = head;
        int n=1;
        while(old_end.next!=null){
            old_end=old_end.next;
            n++;
        }
        if(k>n) k=k%n;
        if(k==n) return head;
        old_end.next=head;
        ListNode new_end = head;
        for(int i=0;i<n-k-1;i++){
            new_end=new_end.next;
        }
        ListNode new_head = new_end.next;
        new_end.next = null;
        return new_head;
    }
    //循环链表，这不是真不会写
    public ListNode rotateRight(ListNode head, int k) {
        if(head==null||k==0){
        return head;
    }
    ListNode cursor=head;
    ListNode tail=null;//尾指针
    int length=1;
    while(cursor.next!=null)//循环 得到总长度
    {
        cursor=cursor.next;
        length++;
    }
    int loop=length-(k%length);//得到循环的次数
    tail=cursor;//指向尾结点
    cursor.next=head;//改成循环链表
    cursor=head;//指向头结点
    for(int i=0;i<loop;i++){//开始循环
        cursor=cursor.next;
        tail=tail.next;
    }
    tail.next=null;//改成单链表
    return cursor;//返回当前头

        
    }
    //这个代码比较整洁
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null) {
            return null;
        }
        ListNode cur = head;
        int len = 1;
        while (cur.next != null) {
            cur = cur.next;
            len++;
        }
        ListNode tail = cur;
        tail.next = head;
        int loop = len - k % len;
        for (int i = 1; i < loop; i++) {
            head = head.next;
        }
        ListNode newHead = head.next;
        head.next = null;
        return newHead;
    }




}
```