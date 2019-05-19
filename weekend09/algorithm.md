---
title: 202. 快乐数
date: 2019-05-19 10:36:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - 算法 
description: 
    加班加的，干什么都没有心情
---
找点自信吧
```Java
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> hs = new HashSet<>();
        int res = 0;
        while(true){
            while(n > 0){
                res = res + ((n%10)*(n%10));
                n /= 10;         
               }
            if(hs.contains(res))  return false;
            if(res == 1)  return true;
            hs.add(res);
            n = res;
            res = 0;
        } 
    }
}
```