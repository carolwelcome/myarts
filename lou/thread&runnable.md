---
title: Thread与Runnable的区别 (转)
date: 2019-06-09 18:38:22
categories:  #分类
    - attendance
tags:   #标签
    - Java
    - multi
description: 
    学习多线程
---

也难怪，上学的时候没怎么关注这一块儿（或者说老师根本就没有讲）导致一看到这块儿就头疼。但是老这么逃避也不是办法呀，从头开始学吧。今天就来学这个，最最经典的问题，这两个有什么区别和联系呢？都知道创建线程是可以有多种方式的
比如：
1、继承Thread类
2、实现Runnable接口（估计以前也就停留在这个层次上了）
3、匿名内部类
4、带返回值的线程
5、线程池
6、Lambda表达式

列出来之后都好说，但是我还是相信很多人跟我一样对开头的问题有所疑问。没关系，咱们一点点来看，学一点是一点，但求甚解。
先来看一下使用继承Thread方式来创建的线程
```Java
public class MultiDemo1 extends Thread{
	
	
	private int count=4;
	@Override
	public void run() {
		while(count>0) {
			System.out.println(Thread.currentThread().getName() + "-------" + count--);
	        try {
	            //休眠4000毫秒
	            sleep(2000);
	        } catch (InterruptedException e) {
	            // TODO Auto-generated catch block
	            e.printStackTrace();
	        }
		}

	}

	public static void main(String[] args) {
		new MultiDemo1().start();
		new MultiDemo1().start();
		new MultiDemo1().start();
		new MultiDemo1().start();
	}
```
执行结果是这样的：
```bash
Thread-0-------4
Thread-1-------4
Thread-2-------4
Thread-3-------4
Thread-1-------3
Thread-2-------3
Thread-0-------3
Thread-3-------3
Thread-2-------2
Thread-3-------2
Thread-0-------2
Thread-1-------2
Thread-2-------1
Thread-3-------1
Thread-0-------1
Thread-1-------1

```
从上面的例子可以看得出来，每次New MultiDemo1()都会创建一个新的线程实例。 
这4个线程彼此之间互不干扰，各自占有各自的资源，并不是统一完成任务。 
由此可以得出：`Thread类无法达到资源共享的目的`


再来Runnable的实例
```Java
public class MultiDemo2 implements Runnable{

	private int count=4;

	@Override
	public void run() {
		while(count>0) {
			System.out.println(Thread.currentThread().getName() + "-------" + count--);
	        
		}
		
	}
	
	public static void main(String[] args) {
		MultiDemo2 r=new MultiDemo2();
		new Thread(r).start();
		new Thread(r).start();
		new Thread(r).start();
		new Thread(r).start();
		
	}

}

```
执行结果：
```bash
Thread-1-------4
Thread-3-------2
Thread-2-------3
Thread-0-------4
Thread-1-------1

```
可以看得出这四个线程是共享资源的
### 结论
1、Runnable适合拥有相同程序代码的线程去处理统一资源的情况，把虚拟的CPU(线程)、程序和数据有效的分离，较好体现面向编程思想。
2、Runnable可以避免由于单继承机制带来的局限性。可以在继承其他类的同时，是能实现多线程功能。
3、Runnable能够增强程序的健壮性，代码能够被多个线程共享。

### 题外话
此外，我们还可以发现，Runnable实例类其实不是一个线程，而是一个线程任务。它是做为一个形参传递到Thread类中去的，Thread才是真正的线程。线程和任务是分开的，任务只有放在线程中才可以被执行。把Runnable改成Task会更好理解。

## 总结
Runnable接口定义的是线程任务，而Thread类是创建线程。
