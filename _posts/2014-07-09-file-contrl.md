---
layout: post
title:  "Python文件遍历方法"
category: Python
tags: python
---

##方法1：使用os.listdir

```python
import os
for filename in os.listdir(r'c:\windows'):
    print filename
```


##方法2：使用glob模块，可以设置文件过滤

```python
import glob
for filename in glob.glob(r'c:\windows\*.exe'):
    print filename
```
 
##方法3：通过os.path.walk递归遍历，可以访问子文件夹

```python
import os.path
def processDirectory ( args, dirname, filenames ):
    print 'Directory',dirname
    for filename in filenames:
        print ' File',filename

 
os.path.walk(r'c:\windows', processDirectory, None )
```

##方法4：非递归

```python
import os
for dirpath, dirnames, filenames in os.walk('c:\\winnt'):
    print 'Directory', dirpath
    for filename in filenames:
        print ' File', filename
```

