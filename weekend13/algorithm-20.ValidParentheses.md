---
title: 20. Valid Parentheses
date: 2019-05-31 10:36:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - 数据结构
description: 
    使用栈解决有效的括号问题
---

这可是一个简单的题，有一行没一行的，我搞了好久，真是怀疑自己的编码能力

```Java
//这效率还不去撞墙？
//执行用时 :31 ms, 在所有Java提交中击败了8.06%的用户
//内存消耗 :34.6 MB, 在所有Java提交中击败了83.16%的用户
class Solution {
    static Map<String,String> map=new HashMap<>();

    static{
        map.put("(",")");
        map.put("[","]");
        map.put("{","}");
        map.put("（","）");
    }
     Stack<String> in=new Stack<>();
     Stack<String> out=new Stack<>();
    public  boolean isValid(String s) {
        if("".equals(s)) return true;
         in.addAll( Arrays.asList(s.replace(" ","").split("")));        
        if(in.size()<2) return false;
        while(!in.isEmpty()){

            if(null==map.get(in.peek())){
                out.push(in.peek());
                in.pop();
                continue;
            }
            if(out.isEmpty()){
                out.push(in.peek());
                in.pop();
                continue;
            }
            if(!(out.peek()).equals(map.get(in.peek()))){
                    out.push(in.peek());
                    in.pop();
                    continue;
            }
            out.pop();
            in.pop();
        }
        if(!out.isEmpty()) return false;
        return true;
    }
}
//哈哈，看我超级无敌不要脸的解法，能写出这样的代码也是挺不要脸的
//执行用时 :249 ms, 在所有Java提交中击败了5.04%的用户
//内存消耗 :48.7 MB, 在所有Java提交中击败了5.01%的用户
public  boolean isValid(String s) {
        s=s.replaceAll("\\(","c");
        s=s.replaceAll("\\)","x");
        s=s.replaceAll("\\[","a");
        s=s.replaceAll("\\]","z");
        s=s.replaceAll("\\{","b");
        s=s.replaceAll("\\}","y");
        while(true){
            s=s.replace("cx","");
            s=s.replace("az","");
            s=s.replace("by","");
            if(!s.contains("cx")&&!s.contains("az")&&!s.contains("by")) break;
            if("".equals(s)) return true;
        }
        if(!"".equals(s)) return false;
        return true;
    }
//执行用时 :188 ms, 在所有Java提交中击败了5.04%的用户
//内存消耗 :63.7 MB, 在所有Java提交中击败了5.01%的用户
    class Solution {
   
    public  boolean isValid(String s) {
        while (s.contains("{}")||s.contains("[]")|| s.contains("()")){
            if(s.contains("{}")) s=s.replace("{}","");
            if(s.contains("()")) s=s.replace("()","");
            if(s.contains("[]")) s=s.replace("[]","");
        }

        return s.isEmpty();
    }
}


```