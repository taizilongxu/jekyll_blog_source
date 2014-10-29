---
layout: post
title:  "Douban Fm 之多进程,多线程"
category: python
tags: python
---

最近Python版本的Douban Fm制作接近尾声,不过困扰我的有一个最大的难题,那就是前台界面和后台程序的融合.

在以前的编程中虽然语言是面向对象的,但程序的设计主旨一般都还是面向过程的,也就是说平常的程序设计中从来没考虑过进程,线程,以及他们之间的通信.虽然操作系统中对线程,进程都有所了解,但是一想到的是系统编程里的就望而却步了.这会终于好好的研究了一下进程编程,稍后会写笔记,先写写Douban Fm碰到的问题.

没错,就是线程,进程的问题,当你设计一个cli样式的界面,而且还需要有进程在后端运行,那么你一定需要多线程或是多进程的.在Python里最最原始的方法有一个fork()方法,相信这个方法是从linux中继承过来的,不过这种方法既难掌握,又非常的混乱,比如我们有一个while循环的话,那么在其中的fork()就是一个灾难了.

相对比较高级的一个方法是multiprocessing,这个函数可以创建一个进程,如下:

```python
import multiprocessing

p = multiprocessing.Process(target=fun, args=(i,)) # 注意函数没有括号
p.start()
```

这个函数允许你创建一个进程,运行在主程序之外,当然可以和主程序进行一些交互.

另外一个比较高级的方法是subprocess,这是一个非常重要的函数,相信大多数人已经知道啦,这个函数最主要的就是调用一个外部程序或者shell里的命令,可以用阻塞的方式调用(subprocess.call()),也可以用非阻塞的方式(subprocess.Popen())

程序中遇到的问题是这样的,cli界面是主程序,用一个while循环去抓取按键,当空格的时候就会播放选中频道的歌曲,然而如果用subprocess调用mplayer播放器,只能播放一首,第二首就会停止,怎么办?

1. 把播放歌曲封装进一个函数,当做一个进程,在进程里无线循环调用歌曲,不停的播放,而界面程序会当做另一个进程监控按键
2. 创建一个守护进程或者守护线程不停的监听subprocess的状态,如运行完成则立刻进行反应

最后我还是选了2,因为2比较简单,1需要的交互太多,比较麻烦

下面这个程序是个实验小程序,用一个守护进程去监听mplayer,如果播放完毕则重新启用mplayer,无限循环的过程

```python
#-*- encoding: UTF-8 -*-
#---------------------------------import------------------------------------
import subprocess
import time
import threading
#---------------------------------------------------------------------------
p = subprocess.Popen('mplayer ~/01\ 21\ Guns\ \(feat.\ The\ Cast\ of\ _Ameri.m4a  >/dev/null 2>&1', shell=True)
def protect(p):
    while True:
        # time.sleep(1)
        p.poll()
        print 'returncode:' + str(p.returncode) + ' ' + str(p.pid)
        if p.returncode == 0:
            p = subprocess.Popen('mplayer ~/01\ 21\ Guns\ \(feat.\ The\ Cast\ of\ _Ameri.m4a  >/dev/null 2>&1', shell=True)
        time.sleep(0.5)
pr = threading.Thread(target=protect, args=(p,))
pr.start()
print 'protect'
############################################################################
```