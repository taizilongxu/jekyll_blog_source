---
layout: post
title:  "Github不再需要密码,ssh连接模式"
category: github
tags: github
---

只介绍主要方法

总共分3部:

1. 本地生成ssh密钥
2. github上生成公钥
3. 修改git的remote url为ssh协议

###1 生成密钥对

再linux主机下

```
 ssh-keygen -t rsa -C “468137306@qq.com”
```

在~/.ssh下会产生id_rsa和id_rsa.pub两个文件

###2 添加密钥到github

进入github的settings里左侧有个SSHkeys,看到那个add一个就行

把id_rsa.pub里的密钥放入这里就可以了(注意不要多加空格)

用下面的命令测试

```
 ssh -T git@github.com
 ```

输出

```
PTY allocation request failed on channel 0
Hi taizilongxu! You've successfully authenticated, but GitHub does not provide shell access.
Connection to github.com closed.
```

###3 修改git的remote url

一般在./.git/config中修改url = ssh地址就可以了
以后就可以和密码说88了


