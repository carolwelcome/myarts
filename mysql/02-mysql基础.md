---
title: 02.MySQL:一条更新语句的执行过程
date: 2019-05-30 13:45:22
categories:  #分类
    - attendance
tags:   #标签
    - MySQL
    - 架构 
description: 
    MySQL基础知识学习
---
![MySQL基础架构图](img/basicarch.png)
### MySQL执行更新语句的流程
同查询语句的流程类似，更新语句的流程也需要先连接到数据库（连接器），并且在更新的时候不建议使用查询缓存，分析器通过词法分析、语法分析得出这是一条更新语句。优化器决定使用哪个索引，执行器负责具体执行，找到这一行，然后更新。
但与查询流程不一样的是，更新流程还涉及另外两个重要的日志模块：重写日志（redo log) 和归档日志(binlog)


#### redo log
类比小酒馆使用黑板与账本配合的方式来完成整个酒馆的记账的过程，MySQL对应的技术是WAL（Write-Ahead Logging），关键点是：先写日志，再写磁盘（也就是先写黑板，再写账本）

具体实现是，当有一个记录需要更新的时候，InnoDB引擎会先把记录写到redo log里，并更新内存，这个时候更新就完成了。同时InnoDB引擎会在适当的时候将这个操作更新到磁盘里面，而这个时候往往是系统比较空闲的时候。

InnoDB的redo log是固定大小的，也是可配的，比如可以配置为一组4个文件，每个文件的大小是1G。这4个文件是循环的形式利用的，类似循环链表，边用边清理。

有了redo log,可以保证InnoDB，即使在数据库发生异常重启之前的记录也不会丢失，这就是所谓的crash-safe能力。

redo log是InnoDB所独有的特性，是属于存储引擎层的技术。这点需要跟Server层的功能区分开来。


![redo log关于insert和delete的记录方式](img/redolog.png)

[上面算是比较易懂的了](https://www.cnblogs.com/f-ck-need-u/archive/2018/05/08/9010872.html#auto_id_5)
引：因为innodb存储引擎存储数据的单元是页(和SQL Server中一样)，所以redo log也是基于页的格式来记录的。默认情况下，innodb的页大小是16KB(由 innodb_page_size 变量控制)，一个页内可以存放非常多的log block(每个512字节)，而log block中记录的又是数据页的变化。
#### binlog
既然redo log是存储引擎层的日志，那binlog呢？其实他才是Server层自己的日志，你肯定又在想那为什么又会有两层日志呢？
这个问题是跟MySQL发展的历史有关，熟悉MySQL历史版本的朋友，都是知道InnoDB并不是MySQL自带的存储引擎，他自带的是MyISAM,但是MyISAM这个存储引擎并不能实现crash-safe这个能力，而binlog也是只能用于归档。所以这才有InnoDB的戏份儿。


#### redo log和binlog的区别
1.redo log是InnoDB引擎特有的，binlog是MySQL的Server层实现，所有存储引擎都可以用。
2.redo log是物理日志，记录的是“某个数据页上做了什么修改”；binlog是逻辑日志，记录提这个语句的原始逻辑，比如“给ID是2的这一行记录c字段加1（如果有一个redo log的记录，一个bin log的记录是不是会更好理解这句话）
3.redo log是循环写的，空间固定会用完；binlog是可以追加写入的，不会覆盖写完就再换一个文件写

在实际更新语句在执行过程中，是先由InnoDB将更新操作记录到redo log中，但此时redo log的状态是prepare,而后由执行器生成这个操作的binlog，并把binlog写入磁盘，最后通知InnoDB引擎把redo log改成commit状态。
注：MySQL集群的主从复制是基于binlog来完成数据同步的

#### 两阶段提交
看到redo log的prepare状态和commit状态了吗？这就是`两阶段提交`，之所以这么设计的原因是为了让这两份日志之间的逻辑一致。
如果不用“两阶段提交”就可能会产生两个日志不一致，而导致在进行数据恢复时得到不同的结果。
两阶段提交是跨系统维持数据逻辑一致性时常用的一个方案。

### MySQL如何恢复到半个月前任意一秒的状态
能够做这点的基础肯定是备份中保存了最近半个月所有的binlog，同时系统会做定期（按业务调整）整库备份。

redo log设置参数innodb_flush_log_at_trx_commit为1，每次事务的redo log都会持久化到磁盘，保证MySQL异常重启之后数据不会丢失。

binlog设置参数sync_binlog为1，每次事务的binlog都会持久化到磁盘，保证binlog不会丢失。
