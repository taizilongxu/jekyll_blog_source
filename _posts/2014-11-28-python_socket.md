---
layout: post
title:  "Python中的socket编程"
category: python
tags: python linux socket
---

最近在看tornado源码,发现里面的tcp层会用到很多socket的知识,所以特地恶补一下.

## 1 socket基础

### 1.1 socket 套接字

```
套接字 socket = (IP地址: 端口号)
```

就这么简单,IP负责主机到主机(点到点)通讯,而端口号负责进程到进程(端到端)的通讯.python中socket编程在网络的应用层,涉及到些许运输层,所以极大简化了编程,我们只要对socket进行操作就可以了.

### 1.2 套接字家族

套接字起源于20世纪70年代加州大学伯克利分校版本的Unix，即人们所说的BSD Unix。因此，有时人们也把套接字称为“伯克利套接字”或“BSD套接字”。套接字有两种，分别是**基于文件型**的和**基于网络型**的。

#### 基于文件型

`AF_UNIX`或者`AF_LOCAL`

主要特点是两个进程都运行在同一台机器上，而且这些套接字是基于文件的,所以，它们的底层结构是由文件系统来支持的.

#### 基于网络型

`AF_INET`或者`AF_INET6`或者`AF_NETLINK`

它们的特点是基于网络的,所有地址家族中，`AF_INET`是使用最广泛的一个.

![](https://raw.githubusercontent.com/taizilongxu/taizilongxu.github.io/master/img/TCP_IP.JPG)

### 1.3 面向连接与无连接

上面的图可以看到`AF_INET`家族可以分为3种,我们常用的就两种TCP和UDP,对应的套接字类型为`SOCK_STREAM`和`SOCK_DGRAM`

## 2 Python中网络编程

Python 提供了两个基本的 socket 模块。

* 第一个是 Socket，它提供了标准的 BSD Sockets API。
* 第二个是 SocketServer， 它提供了服务器中心类，可以简化网络服务器的开发。

### 2.1 socket()函数

在Python里我们用socket（）函数来创建套接字.

```
socket(socket_family, socket_type, protocol=0)
```

参数:

* socket_family: 套接字家族可以使AF_UNIX或者AF_INET
* socket_type: 套接字类型可以根据是面向连接的还是非连接分为`SOCK_STREAM`或`SOCK_DGRAM`
* protocol: 一般不填默认为0.

### 2.2 套接字对象(内建)方法

下面是经常用到的套接字方法:

| 函数 | 描述 |
|--------|--------|
| 服务器端套接字 |
| s.bind() | 绑定地址(主机号,端口号)到套接字 |
| s.listen() | 开始TCP监听 |
| s.accept() | 被动接受TCP客户端连接,(阻塞式)等待连接的到来 |
| 客户端套接字 |
| s.connect() | 主动初始化TCP服务器连接 |
| s.connect_ex() | connect()函数的扩展版本,出错时返回出错码,而不是抛出异常 |
| 公共用途的套接字函数 |
| s.recv() | 接收TCP数据 |
| s.send() | 发送TCP数据 |
| s.sendall() | 完整发送TCP数据 |
| s.recvform() | 接收UDP数据 |
| s.sendto() | 发送UDP数据 |
| s.close() | 关闭套接字 |

### 2.3 客户端和服务器编程

#### server

1. 创建socket对象。调用socket构造函数。如：

		socket = socket.socket( family, type )

	
2. 将socket绑定到指定地址。这是通过socket对象的bind方法来实现的：

		socket.bind( address ) 

	由AF_INET所创建的套接字，address地址必须是一个双元素元组，格式是(host,port)。host代表主机，port代表端口号。如果端口号正在使用、主机名不正确或端口已被保留，bind方法将引发socket.error异常。
    
3. 使用socket套接字的listen方法接收连接请求。

		socket.listen( backlog )

	backlog指定最多允许多少个客户连接到服务器。它的值至少为1。收到连接请求后，这些请求需要排队，如果队列满，就拒绝请求。

4. 服务器套接字通过socket的accept方法等待客户请求一个连接。

		connection, address = socket.accept()

	调用accept方法时，socket会时入“waiting”状态。客户请求连接时，方法建立连接并返回服务器。accept方法返回一个含有两个元素的 元组(connection,address)。第一个元素connection是新的socket对象，服务器必须通过它与客户通信；第二个元素 address是客户的Internet地址。

5. 处理阶段，服务器和客户端通过send和recv方法通信(传输 数据)。服务器调用send，并采用字符串形式向客户发送信息。send方法返回已发送的字符个数。服务器使用recv方法从客户接收信息。调用recv 时，服务器必须指定一个整数，它对应于可通过本次方法调用来接收的最大数据量。recv方法在接收数据时会进入“blocked”状态，最后返回一个字符 串，用它表示收到的数据。如果发送的数据量超过了recv所允许的，数据会被截短。多余的数据将缓冲于接收端。以后调用recv时，多余的数据会从缓冲区 删除(以及自上次调用recv以来，客户可能发送的其它任何数据)

6. 传输结束，服务器调用socket的close方法关闭连接。


#### client

1. 创建一个socket以连接服务器：

		socket = socket.socket( family, type )

2. 使用socket的connect方法连接服务器。对于AF_INET家族,连接格式如下：

		socket.connect( (host,port) )

	host代表服务器主机名或IP，port代表服务器进程所绑定的端口号。如连接成功，客户就可通过套接字与服务器通信，如果连接失败，会引发socket.error异常。

3. 处理阶段，客户和服务器将通过send方法和recv方法通信。
4. 传输结束，客户通过调用socket的close方法关闭连接。

#### 代码

server:

```python
#-*- encoding: UTF-8 -*-
#---------------------------------import------------------------------------
from socket import *
from time import ctime
#---------------------------------------------------------------------------
"""
阻塞方式进行连接,当客户端退出继续监听,等待下一个客户端连接
"""
HOST = ''
PORT = 21568
BUFSIZ = 1024
ADDR = (HOST, PORT)

tcpSerSock = socket(AF_INET, SOCK_STREAM)
tcpSerSock.bind(ADDR)
tcpSerSock.listen(5)

while True:
    print 'waiting for connection...'
    tcpCliSock, addr = tcpSerSock.accept()  # 等待连接
    print '...connected from:', addr

    while True:
        try:
            data = tcpCliSock.recv(BUFSIZ)  # 接收数据
            print '<', data
            tcpCliSock.send('[%s] %s' % (ctime(), data))  # 发送数据
        except:
            print 'disconnect from:', addr
            tcpCliSock.close()  # 退出
            break
tcpSerSock.close()
############################################################################
```

client:

```python
#-*- encoding: UTF-8 -*-
#---------------------------------import------------------------------------
from socket import *
#---------------------------------------------------------------------------
"""
只进行一次连接,输入`close`后退出程序
"""
HOST = 'localhost'
PORT = 21568
BUFSIZ = 1024
ADDR = (HOST, PORT)

tcpCliSock = socket(AF_INET, SOCK_STREAM)
tcpCliSock.connect(ADDR)  # 套接字连接

try:
    while True:
        data = raw_input('>')
        if data == 'close':
            break
        if not data:
            continue
        tcpCliSock.send(data)  # 发送数据
        data = tcpCliSock.recv(BUFSIZ)  # 接受数据
        print data
except:
    tcpCliSock.close()  # 退出
############################################################################
```


# 参考资料

* [python_socket 网络编程 ](http://blog.163.com/yi_yixinyiyi/blog/static/136286889201152814341144/)
* [TCP/IP 地址家族 ，协议类型 ，套接字类型 ，协议字段!](http://blog.csdn.net/jasonm2008/article/details/3964065)
* [python socket编程详细介绍](http://yangrong.blog.51cto.com/6945369/1339593)
* [一个简单的python socket编程](http://openexperience.iteye.com/blog/145701)