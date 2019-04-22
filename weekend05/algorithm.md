---
title: 分发饼干
date: 2019-04-21 15:38:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - 贪心算法 
description: 
    使用贪心算法解决
---
```Java

class Solution {
    // 快速排序，a是数组，n表示数组的大小
  public  void quickSort(int[] a, int n) {
    quickSortInternally(a, 0, n-1);
  }

  // 快速排序递归函数，p,r为下标
  private  void quickSortInternally(int[] a, int p, int r) {
    if (p >= r) return;

    int q = partition(a, p, r); // 获取分区点
    quickSortInternally(a, p, q-1);
    quickSortInternally(a, q+1, r);
  }

  private  int partition(int[] a, int p, int r) {
    int pivot = a[r];
    int i = p;
    for(int j = p; j < r; ++j) {
      if (a[j] < pivot) {
        if (i == j) {
          ++i;
        } else {
          int tmp = a[i];
          a[i++] = a[j];
          a[j] = tmp;
        }
      }
    }

    int tmp = a[i];
    a[i] = a[r];
    a[r] = tmp;

    //System.out.println("i=" + i);
    return i;
  }
    //执行用时 : 102 ms, 在Assign Cookies的Java提交中击败了5.86% 的用户
    //内存消耗 : 39.4 MB, 在Assign Cookies的Java提交中击败了95.97% 的用户
    public int findContentChildren(int[] g, int[] s) {
        quickSort(g,g.length);
        quickSort(s,s.length);
        //首先对数数组进行排序。
        int count=0;

        for(int i=0;i<g.length;i++){
            for(int j=0;j<s.length;j++){
                if(g[i]<=s[j]&&s[j]>0){                    
                    count++;
                   //System.out.println("第"+(i+1)+"个孩子,得到满足，他的胃口是:"+g[i]+",他分到第"+(j+1)+"号饼干，该饼干大小是:"+s[j]);
                    s[j]=0;
                    break;
                }
            }
        }
        return count;
    }
    //这个结果还是比较满意的
    //执行用时 : 14 ms, 在Assign Cookies的Java提交中击败了93.12% 的用户
    //内存消耗 : 46.8 MB, 在Assign Cookies的Java提交中击败了77.78% 的用户
    public int findContentChildren(int[] g, int[] s) {
        quickSort(g,g.length);//这边也可以直接用Arrays.sort,但是从效率上来讲没有上面的算法高效
        quickSort(s,s.length);
        //首先对数数组进行排序。
        int count=0;
        int scount=0;
        //for (int i = 0; i < s.length && scount < g.length;i++){
        while(count<g.length&&scount<s.length){
            if(g[count]<=s[scount]&&s[scount]>0){ 
                count++;
                //s[scount]=0;
                 //break;
            }
            scount++;
        }
        
        return count;
    }
}
```