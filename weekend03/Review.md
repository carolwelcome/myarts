---
title: Review
date: 2019-04-04 15:38:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - CAP
description: 
    CAP定理：这篇文章来自于Wikipedia,讲述了CAP定理的三个原则 （一致性、可用性、分区容错性），并对三个原则选择的场景做了简单的说明，主是针对存在网络分区的情况如何更好的在一致性和可用性中做出好的选择。最后阐述了CAP定理发展的历史，一开始只是这哥们的一个猜想，后来被麻省那个哥们证实之后才成为定理
---
## CAP theorem
CAP定理

In theoretical computer science, the CAP theorem, also named Brewer's theorem after computer scientist Eric Brewer, states that it is impossible for a distributed data store to simultaneously provide more than two out of the following three guarantees:

Consistency: Every read receives the most recent write or an error
Availability: Every request receives a (non-error) response – without the guarantee that it contains the most recent write
Partition tolerance: The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes
1、计算机科学理论，CAP定理，布鲁尔定理 计算机科学家 Eric Brewer 不可能 分布式数据存储 同时提供 超过两个 下面的三个保证
>> ME：在计算机科学理论中，CAP定理也被称为Brewer定理，计算机科学家Eric Brewer指出：对于一个分布式数据存储不可能同时提供超过以下三个条件中的两条件：
>> Google:在理论计算机科学中，CAP定理，也称为计算机科学家Eric Brewer之后的Brewer定理，指出分布式数据存储不可能同时提供以下三种保证中的两种：
2、C一致性 每个读请求 接收到最近的写入或一个错误
>> ME:一致性：每次读取都会接收到最新的写入或错误
>> Google:一致性：每次读取都会收到最近的写入或错误
3、A可用性 每个请求 接收 正确的响应 无法保障 包含最新的写入
>> ME：可用性：每个请求都会接收一个正确的响应，但无法保障这个响应包含最新的写入数据
>> Google:可用性：每个请求都会收到（非错误）响应 - 不保证它包含最新的写入
4、P分区容错 系统继续操作 尽管 任何数量的消息 被删除或延迟 通过网络之间的节点
>> ME:分区容错性：系统不被中断，即使不定数据的消息在通过网络传输的过程会被丢失或延迟
>> Google:分区容差：尽管节点之间的网络丢弃（或延迟）任意数量的消息，系统仍继续运行

<br/>
In particular, the CAP theorem implies that in the presence of a network partition, one has to choose between consistency and availability. Note that consistency as defined in the CAP theorem is quite different from the consistency guaranteed in ACID database transactions

1、特定情况下 CAP定理 暗示 网络分区的存在 不得不在一致性和可用性方面做出选择
ME:在一定的情况下，由于网络分区的存在，CAP定理更倾向于在一致性和可用性方面做出选择
2、注意 一致性 被定义 在CAP定理中 相当不同于 一致性 ACID 数据库事务中的保证
ME：需要注意的是，关于一致性的定义在这儿需要区别于数据库事务对ACID保证的一致性
Google:特别是，CAP定理意味着在存在网络分区时，必须在一致性和可用性之间进行选择。 请注意，CAP定理中定义的一致性与ACID数据库事务中保证的一致性完全不同

<br/>
说明：
No distributed system is safe from network failures, thus network partitioning generally has to be tolerated. In the presence of a partition, one is then left with two options: consistency or availability. When choosing consistency over availability, the system will return an error or a time-out if particular information cannot be guaranteed to be up to date due to network partitioning. When choosing availability over consistency, the system will always process the query and try to return the most recent available version of the information, even if it cannot guarantee it is up to date due to network partitioning.

ME:在网络失败的情况下没有分布式系统是安全的，因此网络分区的问题通常都是被容忍的。这种情况下一致性或可用性只能选其一。当选择一致性时，由于网络分区导致不能保障特定的信息得不到最近的更新，因此系统可能会返回错误或者超时。
当选择可用性时，系统会不断去查询，并努力返回信息最近的可用版本，即使它不能保障这些信息已经是最新的了。
Google:没有分布式系统可以避免网络故障，因此通常必须容忍网络分区。 在存在分区的情况下，可以选择一个分区：一致性或可用性。 在选择一致性而非可用性时，如果由于网络分区而无法保证特定信息是最新的，则系统将返回错误或超时。 在选择可用性而非一致性时，系统将始终处理查询并尝试返回最新的可用信息版本，即使由于网络分区而无法保证其是最新的。

In the absence of network failure – that is, when the distributed system is running normally – both availability and consistency can be satisfied.
ME:假使不存在网络故障，分布式系统正常运行，那也就没必要兼顾可用性和一致性了。
Google:在没有网络故障的情况下 - 也就是说，当分布式系统正常运行时 - 可以满足可用性和一致性。
CAP is frequently misunderstood as if one has to choose to abandon one of the three guarantees at all times. In fact, the choice is really between consistency and availability only when a network partition or failure happens; at all other times, no trade-off has to be made.
ME：CAP经常被误解为，所有情况一旦被选择就必须放弃三个条件中一个。实际上，只有在发生网络分区或者失败发生时才必须在一致性和可用性上做出一个选择。在其他任何情况下，不需要在它们三个中做出权衡
Google:CAP经常被误解，好像必须始终选择放弃三种保证中的一种。 实际上，只有在发生网络分区或故障时，才能在一致性和可用性之间进行选择; 在所有其他时间，不需要进行任何权衡。

Database systems designed with traditional ACID guarantees in mind such as RDBMS choose consistency over availability, whereas systems designed around the BASE philosophy, common in the NoSQL movement for example, choose availability over consistency.

ME：数据库系统被设计为事务ACID保障，比如在关系型数据中选择一致性而不是可用性。而系统围绕BASE哲学进行设计，以NOSQL迁移为例，就选择了可用性而不是一致性。(这个译的不咋滴)
Google:考虑到传统ACID设计的数据库系统，例如RDBMS选择一致性而非可用性，而围绕BASE理念设计的系统（例如NoSQL运动中常见的）选择可用性而不是一致性。

The PACELC theorem builds on CAP by stating that even in the absence of partitioning, another trade-off between latency and consistency occurs.
ME:PACELC定理在CAP的基础上指出：即使在一个存储网络分区的情况下，另一个关于潜伏性和一致性的权衡也会出现 

<br/>
历史
According to University of California, Berkeley computer scientist Eric Brewer, the theorem first appeared in autumn 1998.It was published as the CAP principle in 1999 and presented as a conjecture by Brewer at the 2000 Symposium on Principles of Distributed Computing (PODC). In 2002, Seth Gilbert and Nancy Lynch of MIT published a formal proof of Brewer's conjecture, rendering it a theorem.
ME：根据加利福尼亚大学伯克利计算机科学家Eric Brewer的说法：这个定理在1998年秋天第一次提出，在1999年被公开发表为CAP原理，并在2000年PODC座谈会被作为Brewer的一种推测。在2002年，MIT的Seth Gilbert和Nancy Lynch证实了Brewer的推测，并把它作为一个定理。
Google:根据加州大学伯克利分校计算机科学家Eric Brewer的说法，该定理于1998年秋季首次出现。它于1999年作为CAP原理出版，并作为Brewer在2000年分布式计算原理（PODC）研讨会上的一个猜想。 2002年，麻省理工学院的Seth Gilbert和Nancy Lynch发表了Brewer猜想的正式证明，使其成为一个定理。

In 2012, Brewer clarified some of his positions, including why the often-used "two out of three" concept can be misleading or misapplied, and the different definition of consistency used in CAP relative to the one used in ACID.
ME:2012年，Brewer澄清了他的一些定位，包括：为什么经常使用“三分之二”这样的概念，这会引起相当的误导或者误用；还有CAP中使用的一致性的不同定义相对于ACID中使用的一致性
Google:2012年，Brewer澄清了他的一些立场，包括为什么经常使用的“三分之二”概念可能会误导或误用，以及CAP中使用的一致性的不同定义与ACID中使用的一致。
A similar theorem stating the trade-off between consistency and availability in distributed systems was published by Birman and Friedman in 1996. The result of Birman and Friedman restricted this lower bound to non-commuting operations.
ME：在1996年Birman and Friedman发表了在一类似的关于分布工系统中一致性和可用性中权衡的定理。Birman和Friedman的结果限制了非通勤操作的下限
Google:Birman和Friedman在1996年发表了一个类似的定理，说明了分布式系统中一致性和可用性之间的权衡.Birman和Friedman的结果限制了这种非通勤操作的下限。