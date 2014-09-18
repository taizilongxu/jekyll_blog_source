---
layout: post
title:  "Atom编辑器"
category: atom
tags: github atom
---

![](https://raw.githubusercontent.com/taizilongxu/taizilongxu.github.io/master/img/2014-09-17 19:21:54 的屏幕截图.png)

最近在用atom编辑器,github出的东西想想怎么都不该差的,不过atom可能正处于一个刚开始起步的阶段,许多功能还没有被完善,发现了许多欠缺和不妥的地方

缺点:

1. 运行相比sublime慢.sublime是用c++写的,而atom运行在庞大冗余的V8内核之上
2. 没有上一次文件夹保存功能.每次我打开atom都得重新寻找项目路径,不知道是不是我打开的方式不对.
3. 插件不全.刚起步atom的插件数量明显比不过sublime之类啊

优点:

1. 作为一个github强推的核心编辑器,必然与github有深度的整合,这方面无人能比,修改,增添文件都在侧边栏可以高亮显示(这个再sublime上没有找到),文件中边框也显示哪行修改,删除或者新加(虽然sublime和vim都有这个功能),这也是我喜欢atom的主要原因之一
2. 插件一键安装.的确atom提供的不是一个编辑器,而是一个线上的服务,更新插件so easy
3. 因为atom是基于chrome的V8的,所以全平台支持,而且插件都是用coffeescript编写,方便开发者

作为一个非前端coder,感觉还是够用了,自己虽然是一个vimer,但是涉及到前端的东西感觉还得是atom,sublime之类的编辑器更方便点(代码看得更舒服).

试用的几个插件:

[atom-beautify](https://atom.io/packages/atom-beautify):美化丑陋的代码~

[git-plus](https://atom.io/packages/git-plus):git操作

[color-picker](https://atom.io/packages/color-picker):获取颜色的插件,右键直接打开一个颜色值进行调整

[Markdown Preview package](https://atom.io/packages/markdown-preview):预览markdown插件``` ctrl + shift +m```

[sort-lines](https://atom.io/packages/sort-lines):一个给文本单词排序的插件

[Bracket Matcher package ](https://atom.io/packages/bracket-matcher):括号匹配插件,```ctrl + m```跳转匹配括号,好像linux不太好使~

[Tree View package]( ):自带插件,```ctrl + \ ``` 打开 , ```ctrl + 0 ``` 选中树形图, ```a,m,delete``` 分别为增加,移动和删除

[fuzzy-finder](https://atom.io/packages/fuzzy-finder): ```ctrl+t``` 打开快速查找项目中的文件

[gitlog](https://atom.io/packages/git-log):查看git日志
