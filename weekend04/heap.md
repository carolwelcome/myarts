---
title: 数据结构学习--堆
date: 2019-04-09 13:48:22
categories:  #分类
    - structure
tags:   #标签
    - basic
    - 数据结构
    - 堆
description: 
    数据结构之堆的学习.
---

## 什么是堆
堆是一种特殊的`树`，最典型应用是`堆排序`，它是一种原地的、不稳定的排序算法,时间复杂度为`O(nlogn)`。

### 堆的特点
* 堆是一个完全二叉树（安全二叉树要求，除了最后一层其他层节点的个数都是满的且最后一层的节点靠左排列）
* 堆的每一个节点值都必须大于等于（或小于等于）其子树中每个节点的值（大于等于子树中每个节点的值叫作“大顶堆”，小于等于子树中的每个节点值就叫作“小顶堆”）

### 如何存储一个堆
完全二叉树适合用数组进行存储，而且用数组存储也是非常节省空间的（不用存储其他诸如左右节点指针之类的内容），通过下标就可以轻松快乐的获取每一个节点的值。

## 堆的实现
* 往堆中插入一个元素
* 删除堆顶元素
我们知道，一个包含n个节点的完全二叉树，树的高度不会超过log2 n,所以以上两个操作的时间复杂度都是O(logn)。

### 往堆中插入一个元素
方法：把新插入的元素放到最后，顺着节点所在路径，向上或向下对比，然后互换
```Java
public class Heap {
  private int[] a; // 数组，从下标 1 开始存储数据
  private int n;  // 堆可以存储的最大数据个数
  private int count; // 堆中已经存储的数据个数

  public Heap(int capacity) {
    a = new int[capacity + 1];
    n = capacity;
    count = 0;
  }

  public void insert(int data) {
    if (count >= n) return; // 堆满了
    ++count;
    a[count] = data;
    int i = count;
    while (i/2 > 0 && a[i] > a[i/2]) { // 自下往上堆化
      swap(a, i, i/2); // swap() 函数作用：交换下标为 i 和 i/2 的两个元素
      i = i/2;
    }
  }
 }

```

### 删除堆顶元素
删除堆顶元素之后，把最后一个元素放到堆顶的位置，然后利用父子节点对比的方法进行交换，重复这个过程直到父子节点满足大小关系为止
```Java
public void removeMax() {
  if (count == 0) return -1; // 堆中没有数据
  a[1] = a[count];
  --count;
  heapify(a, count, 1);
}

private void heapify(int[] a, int n, int i) { // 自上往下堆化
  while (true) {
    int maxPos = i;
    if (i*2 <= n && a[i] < a[i*2]) maxPos = i*2;
    if (i*2+1 <= n && a[maxPos] < a[i*2+1]) maxPos = i*2+1;
    if (maxPos == i) break;
    swap(a, i, maxPos);
    i = maxPos;
  }
}

```

## 实现堆排序
* 建堆
* 排序

### 建堆
将一个数组原地建一个堆，有两种办法
* 方法一：在堆中插入一个元素的思路（从后往前）
* 方法二：从前往后建堆
方法二实现代码,这个建堆过程的时间复杂度是O(n)
```Java
private static void buildHeap(int[] a, int n) {
  for (int i = n/2; i >= 1; --i) {
    heapify(a, n, i);
  }
}

private static void heapify(int[] a, int n, int i) {
  while (true) {
    int maxPos = i;
    if (i*2 <= n && a[i] < a[i*2]) maxPos = i*2;
    if (i*2+1 <= n && a[maxPos] < a[i*2+1]) maxPos = i*2+1;
    if (maxPos == i) break;
    swap(a, i, maxPos);
    i = maxPos;
  }
}

```

### 排序 
排序可以参考上面从堆顶删除一个元素的过程，整个堆排序的过程，只需要个别临时存储空间，所以堆排序是原地排序算法，排序的过程时间复杂度是O(nlogn)。结合建堆的过程，所以整个堆排序的过程是时间复杂度是O(nlogn)
```Java
// n 表示数据的个数，数组 a 中的数据从下标 1 到 n 的位置。
public static void sort(int[] a, int n) {
  buildHeap(a, n);
  int k = n;
  while (k > 1) {
    swap(a, 1, k);
    --k;
    heapify(a, k, 1);
  }
}


```


## 堆的应用
* 堆排序 
* 优先级队列
* 利用堆求topK
* 求中位数问题