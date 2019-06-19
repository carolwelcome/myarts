---
title: 9. Valid Parentheses
date: 2019-05-31 10:36:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - 数据结构
description: 
    回文字符串，两个点：1、不会是负数；2、不会有溢出
---
做个简单的

```Java
//这个真的是有点简单，但是简单的事情还是没有做到极致
class Solution {
    public boolean isPalindrome(int x) {
       String s = String.valueOf(x);
        char[] sr = s.toCharArray();
        int i = 0;
        int j = s.length()-1;
        while(i < j) {
            if(sr[i] != sr[j]){
                return false;
            }
            i++;
            j--;
        } 
        return true;
    }
}
//反正这也行，不关注溢出不溢出的，差别也就是在空间的使用上会有差异，
//执行用时 :56 ms, 在所有 Java 提交中击败了48.81%的用户
//内存消耗 :35.6 MB, 在所有 Java 提交中击败了97.50%的用户
class Solution {
    public boolean isPalindrome(int x) {
       if(x<0) return false;//排除负数
        String reverse=new StringBuilder(x+"").reverse().toString();
        int re;
        try{
             re=Integer.parseInt(reverse);
        }catch(Exception e){
            return false;
        }
        return re-x==0;
    }
}
//这就是区别
//执行用时 :67 ms, 在所有 Java 提交中击败了35.56%的用户
//内存消耗 :40.9 MB, 在所有 Java 提交中击败了69.94%的用户
class Solution {
    public boolean isPalindrome(int x) {
       if(x<0) return false;//排除负数
        String reverse=new StringBuilder(x+"").reverse().toString();
        int re;
        try{
             re=Integer.parseInt(reverse);
        }catch(Exception e){
            return false;
        }
        return re-x==0;
    }
}
//
```