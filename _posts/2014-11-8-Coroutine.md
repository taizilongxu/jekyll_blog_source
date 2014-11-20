---
layout: post
title:  "协程"
category: linux
tags: linux 
---

知乎上的[这个](http://www.zhihu.com/question/20511233)答案很好,总结一下协程.

协程，又称微线程，纤程。英文名Coroutine。

## 进程、线程和协程

进程拥有自己独立的堆和栈，既不共享堆，亦不共享栈，进程由操作系统调度。

线程拥有自己独立的栈和共享的堆，共享堆，不共享栈，线程亦由操作系统调度(标准线程是的)。

协程和线程一样共享堆，不共享栈，协程由程序员在协程的代码里显示调度。

| | 共享堆 | 共享栈 | 操作系统调度 |
|:-:|:-:|:-:|:-:|
| 进程 | × | × | √ |
| 线程 | √ | × | √ |
| 协程 | √ | × | × |

#### 与进程&线程的不同之处

* 操作系统内核不知道协程的存在
* 协程不独享进程控制块和处理机时间片

#### 与进程&线程的相同之处

* 协程的状态存在挂起、唤醒、死亡
* 协程和协程之前有控制权的释放和获取

打个比方吧，假设有一个操作系统，是单核的，系统上没有其他的程序需要运行，有两个线程 A 和 B ，A 和 B 在单独运行时都需要 10 秒来完成自己的任务，而且任务都是运算操作，A B 之间也没有竞争和共享数据的问题。现在 A B 两个线程并行，操作系统会不停的在 A B 两个线程之间切换，达到一种伪并行的效果，假设切换的频率是每秒一次，切换的成本是 0.1 秒(主要是栈切换)，总共需要 20 + 19 * 0.1 = 21.9 秒。如果使用协程的方式，可以先运行协程 A ，A 结束的时候让位给协程 B ，只发生一次切换，总时间是 20 + 1 * 0.1 = 20.1 秒。如果系统是双核的，而且线程是标准线程，那么 A B 两个线程就可以真并行，总时间只需要 10 秒，而协程的方案仍然需要 20.1 秒。

## 协程的优势

最大的优势就是协程极高的执行效率。因为子程序切换不是线程切换，而是由程序自身控制，因此，没有线程切换的开销，和多线程比，线程数量越多，协程的性能优势就越明显。

第二大优势就是不需要多线程的锁机制，因为只有一个线程，也不存在同时写变量冲突，在协程中控制共享资源不加锁，只需要判断状态就好了，所以执行效率比多线程高很多。

## Python中的协程

一个实际一点的例子：thread.py

```python
#!/usr/bin/python
# python thread.py
# python -m gevent.monkey thread.py

import threading

class Thread(threading.Thread):

    def __init__(self, name):
        threading.Thread.__init__(self)
        self.name = name

    def run(self):
        for i in xrange(10):
            print self.name

threadA = Thread("A")
threadB = Thread("B")

threadA.start()
threadB.start()
```

运行:

```
python thread.py
```

如果你是均匀输出的:

```
A
B
A
B
...
```

那么总共发生了 20 次切换：主线程 -> A -> B -> A -> B …

再看一个协程的例子：gr.py

```python
#!/usr/bin/python
# python gr.py

import greenlet

def run(name, nextGreenlets):
    for i in xrange(10):
        print name
    if nextGreenlets:
        nextGreenlets.pop(0).switch(chr(ord(name) + 1), nextGreenlets)

greenletA = greenlet.greenlet(run)
greenletB = greenlet.greenlet(run)

greenletA.switch('A', [greenletB])
```

greenlet 是 python 的协程实现。

运行：

```
python gr.py
```

此时发生了 2 次切换：主协程 -> A -> B

可能你已经注意到了，还有一个命令：

```
python -m gevent.monkey thread.py
```

gevent 是基于 greenlet 的一个 python 库，它可以把 python 的内置线程用 greenlet 包装，这样在我们使用线程的时候，实际上使用的是协程，在上一个协程的例子里，协程 A 结束时，由协程 A 让位给协程 B ，而在 gevent 里，所有需要让位的协程都让位给主协程，由主协程决定运行哪一个协程，gevent 也会包装一些可能需要阻塞的方法，比如 sleep ，比如读 socket ，比如等待锁，等等，在这些方法里会自动让位给主协程，而不是由程序员显示让位，这样程序员就可以按照线程的模式进行线性编程，不需要考虑切换的逻辑。

假设代码质量相同，用原生的协程实现需要切换 n 次，用协程包装后的线程实现，就需要 2n - 1 次，姑且算是两倍吧。很显然，单纯从效率上来说，代码质量相同的前提下，用 gevent 永远也不可能比用 greenlet 快，然而，问题往往不那么单纯，比方说，单纯从效率上来说，代码质量相同的前提下，用 C 实现的程序永远不可能比汇编快。

再来说说 python 的线程，python 的线程不是标准线程，在 python 中，一个进程内的多个线程只能使用一个 CPU 。

重新来看一下协程和线程的区别：协程避免了无意义的调度，由此可以提高性能，但也因此，程序员必须自己承担调度的责任，同时，协程也失去了标准线程使用多CPU的能力。

如果使用 gevent 包装后的线程，程序员就不必承担调度的责任，而 python 的线程本身就没有使用多 CPU 的能力，那么，用 gevent 包装后的线程，取代 python 的内置线程，不是只有避免无意义的调度，提高性能的好处，而没有什么坏处了吗？

答案是否定的。举一个例子，有一个 GUI 程序，上面有两个按钮，一个 运算 一个 取消 ，点击运算，会有一个运算线程启动，不停的运算，点击取消，会取消这个线程，如果使用 python 的内置线程或者标准线程，都是没有问题的，即便运算线程不停的运算，调度器仍然会给 GUI 线程分配时间片，用户可以点击取消，然而，如果使用 gevent 包装后的线程就完蛋了，一旦运算开始，GUI 就会失去相应，因为那个运算线程(协程)霸着 CPU 不让位。不单是 GUI ，所有和用户交互的程序都会有这个问题。

# 参考资料

* http://blog.leiqin.info/2012/12/02/%E8%BF%9B%E7%A8%8B%E3%80%81%E7%BA%BF%E7%A8%8B%E5%92%8C%E5%8D%8F%E7%A8%8B%E7%9A%84%E7%90%86%E8%A7%A3.html
* https://github.com/tonyseek/introduce-to-coroutine/blob/master/docs/source/index.rst
* http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/0013868328689835ecd883d910145dfa8227b539725e5ed000
* http://www.zhihu.com/question/20511233