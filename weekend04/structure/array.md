---
title: 数据结构学习--数组
date: 2019-04-10 09:48:22
categories:  #分类
    - structure
tags:   #标签
    - basic
    - 数据结构
    - 数组
description: 
    数据结构之数组的学习.
---

## 数组的定义
>> 数组是一种`线性表`数据结构。它用一组`连续的内存空间`，来存储一组具有相同类型的数据。


## 数组的特性
>> 线性表：数据排成像一条线一样的结构。数组、链表、队列、栈等也都是线性表数据结构。图、二叉树、堆等是非线性表结构
>> 连续的内存空间：基于这个特性能够做对数组内的任一元素进行随机访问，如果想对数组进行插入、删除操作，为保证内存实用的连续性，就需要做大量的数据搬移操作。（动态扩缩容）
>> 


## 各种操作的时间复杂度
>> 插入操作最好/最坏/平均时间复杂度：O(1)/O(n)/O(n)
>> 删除操作最好/最坏/平均时间复杂度：O(1)/O(n)/O(n)
>> 数组可以支持随机访问，根据下标随机访问的时间复杂度为O(1),查找的时间复杂度并不是O(1)

## 数组与编程语言比较
>> 从ArrayList的优势上来看,ArrayList最大的强项是把很多对数组的操作都封装了起来，还支持动态扩容。每次扩容大小1.5倍，但是扩容操作也是比较耗时的，所以建议在创建ArrayList的时候事先指定数据大小。
>> 从ArrayList的不足上来看，ArrayList无法存储基本类型，比如int long,需要Autoboxing成Integer/Long类才可以使用。而这一过程会有一定的性能消耗，如果过分的关注性能的话，建议使用数组。此外，数组在表示多维数组的时候往往比较直观。

## 数组的内存地址计算
```Bash
一维数组：
address[i]=base_address+i*data_type_size
二维数组：
address[i][j] = base_address + ( i * n + j) * data_type_size
```

## 数组小学问
为什么数组的下标都从0开始呢？

## leetcode数组练习题目
```Java
	//执行用时 : 15 ms, 在Fibonacci Number的Java提交中击败了44.97% 的用户
	//内存消耗 : 32.2 MB, 在Fibonacci Number的Java提交中击败了87.19% 的用户
	public int fib(int n) {
        if(n==0) return 0;
        if(n==1) return 1;
        return fib(n-1)+fib(n-2);
    }
    //执行用时 : 0 ms, 在Fibonacci Number的Java提交中击败了100.00% 的用户
	//内存消耗 : 32.4 MB, 在Fibonacci Number的Java提交中击败了82.18% 的用户
	public int fib(int n) {
        if(n==1) return 1;
        if(n==0) return 0;
        int pre=1;
        int prepre=0;
        int result=0;
        for(int i=2;i<n+1;i++){
            result=pre+prepre;
            prepre=pre;
            pre=result;
        }
        return result;
    }



```