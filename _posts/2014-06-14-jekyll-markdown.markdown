---
layout: post
title:  "jekyll的markdown设置"
category: blog
tags: blog 
---


jekyll可以有多种markdown的解析器，目前找到的支持GFM的解析器有两个，一个是kramdown，另一个是redcarpet。

###kramdown
最开始用的是kramdown，配置都很简单。

_config.yml配置：

```ruby
markdown: kramdowm
kramdown:
  input: GFM
```

###redcarpet

redcarpet用起来还是比较方便的。

_config.yml配置：

```ruby
markdown: redcarpet
redcarpet:
  extensions: []
```

[]中可以添加的常用扩展选项有如下：

* no_intra_emphasis：不解析单词中的下划线，如foo_em_body
* fenced_code_blocks :这个比较实用，就是用GFM的方式标记代码段，即三个__`__
* autolink：自动解析链接
* strikethrough：删除线，this is ~~good~~ bad
* space_after_headers：后边必须接空格，否则不会被解析为标题

用redcarpet的一个好处是，可以和[highlights.js][1]配合，可以选的css样式也很多，方便布局和修改，这个博客的样式就是GFM的样式。


  [1]: http://highlightjs.org/