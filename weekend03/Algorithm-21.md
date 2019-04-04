---
title: Algorithm-21
date: 2019-04-04 15:38:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - Link
description: 
    Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.
---
```Java
//排序算法？归并，嗯
//执行用时 : 2 ms, 在Merge Two Sorted Lists的Java提交中击败了100.00% 的用户
//内存消耗 : 34.8 MB, 在Merge Two Sorted Lists的Java提交中击败了0.97% 的用户
public ListNode mergeTwoLists(ListNode head1, ListNode head2) {
        if(head1==null&&head2==null) return null;
        if(head1==null) return head2;
        if(head2==null) return head1;
        ListNode p1 = head1, p2 = head2, head;
        //得到头节点的指向
        if (head1.val < head2.val) {
            head = head1;
            p1 = p1.next;
        } else {
            head = head2;
            p2 = p2.next;
        }
        ListNode merge=head;//最终返回节点
        while(p1!=null&&p2!=null){
            if(p1.val<=p2.val){
                head.next=p1;
                head=head.next;
                p1=p1.next;
            }else{
                head.next=p2;
                head=head.next;
                p2=p2.next;
            }
        }
        //收拾一下残渣儿
        if(p1==null) head.next=p2;
        if(p2==null) head.next=p1;

        return merge;
    }
    //别人的递归实现，看来我是真的不会写递归算法，只能迭代。太笨了
     public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null)
            return l2;
        
        if (l2 == null)
            return l1;
        
        int v1 = l1.val, v2 = l2.val;
        ListNode node;
        if (v1 < v2) {
            node = new ListNode(v1);
            node.next = mergeTwoLists(l1.next, l2); 
        } else {
            node = new ListNode(v2);
            node.next = mergeTwoLists(l2.next, l1); 
        }
        return node;
    }

```