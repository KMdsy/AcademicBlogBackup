---
title: Markdown常用语法
updated: 2022-04-14 21:47:00
date: 2022-01-01 18:00:00
tag:
- note
- markdown
---



[Remark: HTML常用语法](http://www.w3chtml.com/html/html-basic-grammar.html)

<!-- more -->

1. 插入并调整图片大小

```html
<div align="center">
    <img src="fig.png" width = "70%" />
</div>
```

2. 设置页面支持latex公式，在页面头部插入以下内容

```html
<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [ ['$','$'], ["\\(","\\)"] ],
            }
        });
    </script>
</head>
```

3. 为文章加入锚点

    1. markdown的标题行本身自带锚点，直接使用`[some_text](#title)`即可实现页内跳转，注意，`title`中不可以包含符号，否则会跳转失败。
    2. 使用`<a>`标签，注意，包围在标签中的部分，不适用于markdown语法，以及公式语句。

```html
<!-- HTML方法 -->
<a id="tag"> 添加锚点 </a>
<a href="#tag"> 链接锚点 </a>
```
```markdown
<!-- markdown方法 -->
[链接锚点](#tag)
[链接到另一个文章的锚点](other_file.md#tag)
```

