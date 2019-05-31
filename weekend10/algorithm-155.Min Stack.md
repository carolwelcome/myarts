---
title: 155. 最小栈
date: 2019-05-31 10:36:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - 算法 
description: 
    最小栈，栈不是重点，重点是最小。
	这个地址很好：https://www.cnblogs.com/smyhvae/p/4795984.html
---

### 最小栈
栈不是重点，重点是最小，尝试着没有用IntegerStack做的一个，结果发现调试的时候怎么调不过去,我都不知道哪出做了。
```Java
class MinStack {
    
    private int[] elementData;

    private volatile int elementCount=0;//元素个数

    private volatile int minPos=1;//最小值下标



    /** initialize your data structure here. */
    public MinStack() {
        elementData=new int[10];
    }

    public void push(int x) {
        elementCount++;
        if(elementCount>(elementData.length*0.75)){
            int newCapacity=elementData.length*2;
            elementData = Arrays.copyOf(elementData, newCapacity);
        }
        elementData[elementCount]=x;
        if(elementData[elementCount]<elementData[minPos]){//如果是更小的元素
            minPos=elementCount;
        }
    }

    public void pop() {
        
        if(elementCount!=minPos){//如果栈元素正好是最小的，就需要重新找最小的了。
            elementData[elementCount]=0;
            elementCount--;
            return ;
        }
        int minIndex=elementData[1];
        int tmpPos=1;
        for (int i = 1; i < elementCount-1; i++) {
            if(minIndex > elementData[i]){
                minIndex = elementData[i];
                tmpPos=i;
            }
        }
        minPos=tmpPos;
        elementData[elementCount]=0;
        elementCount--;
    }

    public int top() {
        return elementData[elementCount];
    }

    public int getMin() {
        return elementData[minPos];
    }
}
```
无耐把最小值的方式给切换了,通过倒是通过了，这效率低的让人挠头。
```Java
//执行用时 : 513 ms, 在Min Stack的Java提交中击败了5.57% 的用户
//内存消耗 : 58.1 MB, 在Min Stack的Java提交中击败了9.62% 的用户
class MinStack {
    
    private int[] elementData;

    private volatile int elementCount=0;//元素个数



    /** initialize your data structure here. */
    public MinStack() {
        elementData=new int[20];
    }

    public void push(int x) {
        elementCount++;
        if(elementCount>(elementData.length*0.75)){
            int newCapacity=elementData.length*2;
            elementData = Arrays.copyOf(elementData, newCapacity);
        }
        elementData[elementCount]=x;
    }

    public void pop() {
        if(elementCount<(elementData.length*0.25)&&elementCount>5){
            int newCapacity=elementData.length>>1;
            elementData = Arrays.copyOf(elementData, newCapacity);
        }
        elementData[elementCount]=0;
        elementCount--;

    }

    public int top() {
        return elementData[elementCount];
    }

    public int getMin() {
        int min=elementData[1];
        int minPos=1;
        for(int i=1;i<=elementCount;i++){
            if(min>elementData[i]){
                min=elementData[i];
                minPos=i;
            }
        }
        return elementData[minPos];
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```
这就行了，当然不会。借助辅助栈再搞一搞,有点进步了
```Java
//执行用时 : 108 ms, 在Min Stack的Java提交中击败了53.18% 的用户
//内存消耗 : 46.5 MB, 在Min Stack的Java提交中击败了36.36% 的用户
class MinStack {
    
    Stack stack;
    Stack stackMin;//存放求最小值的栈

    /** initialize your data structure here. */
    public MinStack() {
        stack=new Stack();
        stackMin=new Stack();
    }

    public void push(int x) {
        stack.push(x);
        if(stackMin.isEmpty()||x<(int)stackMin.peek())
            stackMin.push(x);
        else {
            stackMin.add(stackMin.peek());//这是一个晶狗的地方，push和add都可以，其实在这儿只是给这些元素一个占位符而矣（表达的是这种意思 ：在该元素之前的所有元素中，最小元素是栈顶元素就可以，很妙）
        }

    }

    public void pop() {
        stack.pop();
        stackMin.pop();
    }

    public int top() {
        return  (int)stack.peek();
    }

    public int getMin() {
        //stack.search()
        return  (int)stackMin.peek();
    }
}
```
