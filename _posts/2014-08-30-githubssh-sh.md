---
layout: post
title:  "github中ssh替换https脚本"
category: python
tags: python
---

为了进一步提高程序效率(懒惰),把所有github项目的链接改成ssh,方便不用再打密码,哈哈

看了几个项目的共同点

```
git@github.com:taizilongxu/ici.git
https://github.com/taizilongxu/ici.git

git@github.com:taizilongxu/jekyll_blog_source.git
https://github.com/taizilongxu/jekyll_blog_source.git

git@github.com:taizilongxu/report.git
https://github.com/taizilongxu/report.git

git@github.com:taizilongxu/wangdaoOJ.git
https://github.com/taizilongxu/wangdaoOJ.git
```

只需要把```https://github.com/```替换成```git@github.com:```就可以了,脚本如下:

```python
#-*- encoding: UTF-8 -*-
#---------------------------------import------------------------------------
import os
import os.path
import re
#---------------------------------------------------------------------------
rootdir = "/home/limbo/"                                   # 指明被遍历的文件夹
username = 'taizilongxu'

def git_ssh(rootdir,username):
    for parent,dirnames,filenames in os.walk(rootdir):    #三个参数：分别返回1.父目录 2.所有文件夹名字（不含路径） 3.所有文件名字
        if parent[-4:] == '.git':
            if 'config' in filenames:
                path = parent + '/' + 'config'
                print path
                with open(path,'r') as F:
                    s = re.sub(r'https://github.com/','git@github.com:',F.read())
                    with open(path,'w') as f:
                        f.write(s)

def main():
    git_ssh(rootdir,username)

if __name__ == '__main__':
    main()
############################################################################
```
