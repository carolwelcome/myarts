---
title: 232. Implement Queue using Stacks
date: 2019-05-31 10:36:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - 算法 
description: 
    栈实现的队列
---

跟上周的最小栈差不多思路，很容易就解题了,效率一般。

```Java
//执行用时 : 88 ms, 在Implement Queue using Stacks的Java提交中击败了64.87% 的用户
//内存消耗 : 33.2 MB, 在Implement Queue using Stacks的Java提交中击败了94.93% 的用户
class MyQueue {
    
    private Stack<Integer> s1 = new Stack<>();//执行入队操作的栈
    private Stack<Integer> s2 = new Stack<>();//执行出队操作的栈

    /** Initialize your data structure here. */
    public MyQueue() {
        
    }
    
   public void push(int x) {
        s1.push(x);
    }
    public int pop() {
        if(s2.empty())  
            while(!s1.empty())
            {
                s2.push(s1.peek());
                s1.pop();
            }
        int ans = s2.peek();
        s2.pop();
        return ans;
    }
    public int peek() {
        if(s2.empty())  
            while(!s1.empty())
            {
                s2.push(s1.peek());
                s1.pop();
            }
        int ans = s2.peek();
        return ans;
    }
    
    public boolean empty() {
        return s1.empty()&&s2.empty();
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */

```