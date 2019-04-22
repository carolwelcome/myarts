分享一下关于volatile的全部知识

volatile的作用、由来、实现原理、使用、优势、不足、跟synchronized的关系

Java编程语言允许线程访问共享变量， 为了确保共享变量能被准确和一致地更新，线程应该确保通过排他锁单独获得这个变量。Java语言提供了volatile，在某些情况下比锁要更加方便。

volatile在多处理器开发中保证了共享变量的“可见性”。
可见性的意思是当一个线程修改一个共享变量时，另外一个线程能读到这个修改的值。

结论：如果volatile变量修饰符使用恰当的话，它比synchronized的使用和执行成本更低，因为它不会引起线程上下文的切换和调度

链接地址：
https://mp.weixin.qq.com/mp/profile_ext?action=home&__biz=MzAwNTQ4MTQ4NQ==&uin=&key=&devicetype=Windows+10&version=62060739&lang=zh_CN&a8scene=7&winzoom=1