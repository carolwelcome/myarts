---
title: 997. 找到小镇的法官
date: 2019-05-5 18:36:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - 图 
description: 
    小镇法官，用了一种特别恶心的办法还是解决了。
---
### 解题思路
今天早上看了布隆过滤器就想着怎么才能试一试呢。结果还是用最笨的办法让提交了，一看结果还真是有点乐。
这种方式其中p1肯定是多余的，也是灵光一现的想法。纠结了好久，`怎么才能排除你信任我，我又信任你的问题`。
说实话，能提交我都感觉到意外。也算是一种空间换时间的做法吧。心有一万条不甘的理由，我该怎么找呢。嗯应该是按照出入度的方式来找，这是看了别人的答案才想到的。还是思维受限呀
```Java
class Solution {
    //执行用时 : 9 ms, 在Find the Town Judge的Java提交中击败了73.10% 的用户
    //内存消耗 : 67.2 MB, 在Find the Town Judge的Java提交中击败了73.07% 的用户
    public int findJudge(int N, int[][] trust) {
        if(trust.length<1) return 1;
        //if(trust.length==N) return -1;
        int[] p=new int[N*10];
        int[] p1=new int[N*10];
        for(int i=0;i<trust.length;i++){
            if(p[trust[i][0]]>0){
                //如果这个trust[i][0]这个数大于0说明已经有人信任他了，而这时他又选择信任别人，
                //那么他一定是个骗子，但是如果想当法官连骗子也得支持他。这块儿肯定是问题的所在了
                p[trust[i][0]]=-1;//打入地狱
                if(p[trust[i][1]]>-1)//继续支持别人
                    p[trust[i][1]]++;
                continue;
            }
            if(p[trust[i][1]]>-1)
                p[trust[i][1]]++;
        }
        //返过来再搞一遍
        for(int i=trust.length-1;i>=0;i--){
            if(p1[trust[i][0]]>0){
                p1[trust[i][0]]=-1;
                if(p1[trust[i][1]]>-1)
                    p1[trust[i][1]]++;
                continue;
            }
            if(p1[trust[i][1]]>-1)
                p1[trust[i][1]]++;
        }
        int result=0,result1=0;
        for(int i=0;i<p.length;i++  ){
            if(p[i]>0&&p[i]==N-1){
                result=i;
                break;
            }
        }
        for(int i=0;i<p1.length;i++  ){
            if(p1[i]>0&&p1[i]==N-1){
                result1=i;
                break;
            }
        }
        if(result!=result1||result==0) return -1;
        return result;
    }

    //使用出度入度实现思路再来实现一下吧，待续。。。。
}
```