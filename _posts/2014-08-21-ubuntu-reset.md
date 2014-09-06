---
layout: post
title:  "Ubuntu迁移系统,SSD与HDD混盘分区"
category: linux
tags: linux
---

最近忍受不了打开matlab的速度,果断换了一个ssd,速度果然飞快.不过又得迁移一次系统,这重新安装一下软件,记录一下.

最开始安装的是硬盘里的i386 32位的ubuntu,一开始没仔细看,等安完系统,下完sougou一看不满足依赖,这才检查了下系统,32位,真坑啊,于是又做了一下系统盘重新来了一遍.

####分区策略

一共两张硬盘,一张500GHDD,一张128GSSD

需求:因为想着放假回家要带着SSD,给我的老笔记本用着,平常的时候就放在实验室的台式里(毕竟天天你要用台式),所以SSD必须可单独拆卸,完整的安装系统,同时还要挂着那个台式机的HDD.

ssd:

1. /boot 200M
2. swap 1000M
3. / 50000M
4. /home 余下的空间

hdd:

1. /home/limbo/database 500G

这样我就可以再台式机上用ssd做系统,hhd做仓库,把大容量的文件放进/home/limbo/database,方便存储,最后再装完系统后,改变一下database的权限就可以了

**注意要建一个limbo的用户**

####搜狗拼音

这货也挺坑的,按照[官网的地址](http://pinyin.sogou.com/linux/)直接下载deb包,用软件中心直接傻瓜安装就可以了,这时候千万要注意了,许多网上教程要**卸载ibus,fcitx**什么的**千万不要卸载**,否则就要重装了,只在设置->语言里把默认的ibus输入方式改成fcitx就OK了(重装了3遍,泪崩~~)

![](https://raw.githubusercontent.com/taizilongxu/taizilongxu.github.io/master/img/2014-08-27%2018:48:25%20%E7%9A%84%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)

####安装基本软件

几个常用的软件和支持

```
sudo apt-get install openjdk-7-jre
sudo apt-get install tree
sudo apt-get install vim
sudo apt-get install zsh
sudo apt-get install tmux
sudo apt-get install guake
sudo apt-get install git
sudo apt-get install tig
sudo apt-get install ipython
sudo apt-get install autojump
------------
September 6 2014 10:22 AM 更新
sudo apt-get install htop #比top好用太多
------------
```

python的[pip](https://bootstrap.pypa.io/get-pip.py)

```
https://bootstrap.pypa.io/get-pip.py
```

几个有用的改变guake终端颜色的脚本

```
git clone https://github.com/erroneousboat/guake-colors-monokai.git
git clone https://github.com/sigurdga/gnome-terminal-colors-solarized


```

vim支持,首先下载事先保存的dotfile



```
git clone git@github.com:taizilongxu/dotfiles.git
cd dotfiles
chmod +x install.sh
./install.sh
```
再下载vim需要的依赖

```
git clone https://github.com/terryma/vim-expand-region.git
git clone https://github.com/terryma/vim-multiple-cursors.git
git clone https://github.com/Yggdroot/indentLine.git
sudo apt-get install ctags
sudo apt-get install curl
```

非常奇怪,上面3个插件vundle里竟然没有,所以要单独git clone下,再把3个插件覆盖到~/.vim下即可

几个zsh的theme放在[gist](https://gist.github.com/taizilongxu/f8881c4ff61fbf702641)上了,以后有功夫再做脚本吧

####安装jekyll

jekyll是ruby支持,所以要下载ruby和gem

```
sudo apt-get install ruby
sudo apt-get install gem
sudo apt-get install ruby ruby-dev
sudo gem install jekyll
sudo apt-get install nodejs
```

####github

~/.gitconfig

```
[user]
    name = taizilongxu
    email = 468137306@qq.com
[core]
    editor = vim
[alias]
    ci = commit -a -v
    co = checkout
    st = status
    br = branch
    throw = reset --hard HEAD
    throwh = reset --hard HEAD^
[color]
    ui = true
[push]
    default = current
[merge]
    tool = meld
```

####其他有用的软件

1. [haroopad](http://pad.haroopress.com/):感觉是linux下markdown支持最好,用的最舒服的编辑器.
2. smplayer:各个格式的音频都支持,满分
3. virtualbox:虚拟机必备,轻量,快捷
4. chrome:这个不必多说了
5. unity-tweak-tool:ubuntu的unity桌面定制
6. System Load Indicator:系统监视器,放在状态栏里,比起什么conky的简单些,也实用些.
7. sublime text:这个可装可不装,作为vimer其实vim一个编辑器就够了,哈哈

####SSD优化

ext4文件系统自动4k对齐,BIOS要开achi

其实SSD也不必担心,优不优化的平常使用,它的寿命完全够用了,最起码有3年的质保呢,3年就该换了.网上说的什么trim的平常执行以下命令就可以了:

```
sudo fstrim -v \
sudo fstrim -v \home
```

####感触

SSD真的挺快的,浏览器 文本 ppt什么的都是秒开,最可歌可泣的是ubuntu的启动速度,目测10秒以内,快的我BIOS都来不及开就进去了,体验了SSD真的不想再等慢腾腾的HDD了.
