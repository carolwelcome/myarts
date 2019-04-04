---
title: Algorithm-141
date: 2019-04-03 11:38:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - Link
description: 
    Given a linked list, determine if it has a cycle in it.
    To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.
---
```Java
public class Solution {
	//使用set的独特属性
    public boolean hasCycle(ListNode head) {
        if(head==null) return false;
        Set<ListNode> si=new HashSet<>();
        while(head.next!=null){
            
            if(si.contains(head))
                return true;
            else
                si.add(head);
            head=head.next;
            
        }
        return false;
    }
    //想着用每个节点的内存地址作为key，来检查，竟然没有提交通过
    public boolean hasCycle(ListNode head) {
        if(head==null) return false;
        StringBuilder sb=new StringBuilder();
        sb.append(System.identityHashCode(head));
        sb.append(",");
        boolean flag=false;
        while(head.next!=null){
            int addr=System.identityHashCode(head.next);
            if(sb.indexOf(addr+"")>-1){
                flag=true;
                break;
            }
            sb.append(addr).append(",");
            head=head.next;
            
        }
        return flag;
    }

    //快慢指针的做法
    public boolean hasCycle(ListNode head) {
        if(head == null || head.next ==null){
            return false;
        }
        ListNode slow = head;
        ListNode fast = head;
        while (fast !=null && fast.next!=null){
            slow = slow.next;
            fast = fast.next.next;
            if(slow == fast){
                return true;
            }
        }
        return false;
    }
}
```