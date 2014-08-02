---
layout: post
title:  "Jekyll使用MathJax来显示数学式"
category: blog
tags: jekyll
---

用Jekyll来显示数学公式：

```html

<script type="text/javascript"src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

```

如果要加 ```$``` ```\\```匹配：

```html

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}
});
</script>

```


