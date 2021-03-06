<!-- TOC -->

- [1 Linux进程间通信方式](#1-linux进程间通信方式)

<!-- /TOC -->


# 1 Linux进程间通信方式

管道、命名管道、内存映射、消息队列、共享内存、信号量、信号、套接字。

**1、管道 PIPE**：特指无名管道，是一种半双工的通讯方式，数据只能单向流动。只能用于**父子进程**之间的通讯。一个管道有两个端口，一个读口，一个写口。
管道实际上是内核的一个缓冲区，在内存中。缓冲区不需要很大，它被设计为环形的。当缓冲区满了，写进程会等待。当两个进程都结束时，管道会自动消失。

**2、命名管道 FIFO**：是一种特殊类型的文件，它在系统中以文件形式存在。这样克服了管道的弊端，他可以**允许没有亲缘关系的进程间通信**。在命名管道中，管道可以是事先已经创建好的。（匿名对象）

**3、共享内存：**共享内存是在多个进程之间共享内存区域的一种进程间的通信方式。由**IPC**(进程间通信) 为进程创建的一个特殊地址范围。共享内存本身并没有同步机制，需要程序员自己控制。共享内存是IPC最快捷的方式，因为共享内存方式的通信没有中间过程。

**4、信号：**信号机制是 Unix 系统中最为古老的进程之间的通信机制，用于一个或几个进程之间传递异步信号。信号可以由各种异步事件产生，比如键盘中断等。shell也可以使用信号将作业控制命令传递给它的子进程。
信号是在软件层次上对**中断机制的一种模拟**。信号是异步的。信号事件发生的来源有两个：硬件来源和软件来源。
信号的分类：可靠信号, 不可靠信号；实时信号, 非实时信号。
进程对信号的响应：1、忽略信号；2、捕捉信号；执行默认操作。
信号的生命周期：信号诞生、信号在进程中注册、信号在进程中注销、信号处理函数执行。

**5、消息队列：**消息队列是内核地址空间中的**内部链表**，通过 Linux 内核在各个进程直接传递内容，消息顺序地发送到消息队列中。不同的消息队列直接是相互独立的。（类比线程池）

与命名管道相比，消息队列的优势在于：
1、消息队列也可以独立于发送和接收进程而存在，从而消除了在同步命名管道的打开和关闭时可能产生的困难。（类比 UDP TCP）
2、同时通过发送消息还可以避免命名管道的同步和阻塞问题，不需要由进程自己来提供同步方法。
3、接收程序可以通过消息类型有选择地接收数据，而不是像命名管道中那样，只能默认地接收。

**6、信号量：**主要作为进程间以及同一进程不同线程之间的**同步**手段。

**7、套接字：**更为一般的进程间通信机制，可用于**不同机器**之间的进程间通信。