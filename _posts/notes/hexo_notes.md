---
title: 本站维护手册
date: 2022-04-12 16:00:00
updated: 2022-04-14 14:00:00
tag:
- note
- hexo
---

本文档记录了建立基于hexo的网页系统的步骤，并对维护所需要的操作一一列出，日后的本站更新将依据该文档进行。

Note: 随时会更新维护策略

<!-- more -->

## 🌟 目前正在维护的论文主题列表

### 按任务分类

- Anomaly detection / Outlier / Out-of-distribution
- Interpretable / Explainable
- Causal discovery
- Data augmentation 

### 按数据分类

- Time series
- Missing value / Irregular sampled / Imputation
- Sequence
- Heterogeneous

### 按深度学习架构分类

- Recurrent neural network / RNN / LSTM / GRU 
- Autoencoder

### 按应用分类

- Cloud native
- Micro-service

## 🌟 目前正在维护的顶会列表

- NeurIPS
- SIGKDD
- IJCAI
- ICML
- SIGIR
- CVPR
- WWW
- ICLR
- AAAI
- SIGMOD
- NDSS
- ESEC/FSE
- ICSE
- ASE
- ISSRE

## 🌟 制作新页面的规范

⚠️⚠️⚠️ 由于配置原因，最好将文章中的**全角符号**替换为**半角符号+空格**，以获得最佳的字体效果

### 页面配置

1. 对于新的标签页

- 可以直接在`<hexo_path>`使用命令`hexo new page <new_page_name>`
- 可以在`<hexo_path>/source`下新建相应的文件夹，并手动在其中新建`index.md`文件

```console
$ cd <hexo_path>/source
$ mkdir <new_folder>
$ cd <new_folder>
$ vim index.md
```

随后在`<hexo_path>/_config.mashiro.yml`中的`menu`字段进行配置：`<display_name>: /<new_page_name>`

2. `index.md`文件的书写规范

⚠️ 仅有在`<hexo_path>/source/_post`中的文档，其中的`tag`字段可以被正确索引，其他目录中若包含`tag`字段则会导致部署失败。

3. 普通markdown文件的书写规范

文件头示例如下，后续的文章标题从**二级目录**开始书写。

```yaml
---
title: Some Notes
updated: 2022-04-12 20:00:00
date: 2022-04-12 16:07:32
tag: 
- note
- 5G
- 4G
- 5G NR
---
```

4. 在适当的位置添加`<--! more -->`以控制文章在首页显示的摘要内容。

5. 文档内部引用

⚠️ 需要参考`<hexo_path>/public`中生成的文档结构。一般的，`<hexo_path>/source/_post`中`<dir_name>/<file_name>.md`文档将存储在`<hexo_path>/public/uncategorized/<dir_name>/<file_name>/index.html`

⚠️⚠️ hexo现已配置为无须`/index.html`后缀，文章内引用请使用`/uncategorized/<dir_name>/<file_name>`

### 页面部署

1. 页面本地测试

访问https://localhost:4000以测试

```console
$ hexo clean && hexo g && hexo s
```

2. 页面部署

官方方法：代码如下。注意`font-spider`那一行，您的`public`文件夹中最深的html文件嵌套了几层，就应当在后面写几层的通配符，可以将上面这些语句保存为一个脚本文件，部署时运行一下就行了。

```console
$ hexo clean
$ hexo g
$ font-spider public/*.html public/*/*.html public/*/*/*.html public/*/*/*/*.html public/*/*/*/*/*.html
$ hexo deploy
```

⚠️⚠️ 上述代码已写入`<hexo_path>/mydeploy.sh`，运行`sudo ./mydeploy.sh`


## 基于模版的小修改

### 添加文章更新时间, 并按更新时间倒序排列文章

本站模版中没有内置更新时间的显示，需要按需对模版做以下修改

#### 更改Hexo策略

1. 调整Hexo中post的默认生成格式：在`{hexo_path}/scaffolds/post.md`中指定默认模版样式

```yaml
---
title: {{ title }}
date: {{ date }}
updated: {{ date }}
---
```

2. 开启`updated`排序: 在`.md`文件的头部加入`updated字段`, 并完善该字段
3. 调整 Hexo 主配置文件: 在`<hexo_path>/_config.yml`中更新文章排序为<u>按照更新时间排序</u>

```yaml
# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator:
  path: ''
  per_page: 10
  order_by: -updated
```

#### 更改模版样式

原主题默认没有更新时间显示的，所以需要自己新增更新时间显示。记主题文件夹为`{theme_path}`, 一般位于`{hexo_path}/themes/{theme_name}`

1. 确定修改范围

查看了`{theme_path}/layout`文件夹下的`post.ejs`文件，里面引入的是 `_partial/article`的内容。找到`{theme_path}/layout/_partial`文件夹下的 `article.ejs`。其中关于时间的内容如下，说明日期部分引用了`post/date`，样式文件为`article-date`

```js
<div class="article-meta">
<%- partial('post/date', {class_name: 'article-date', date_format: null}) %>
<%- partial('post/category') %>
</div>
```

修改范围即为:
- `{theme_path}/layout/_partial/article.ejs`
- `{theme_path}/layout/_partial/post/date.ejs`
- `{theme_path}/source/css/_partial/article.styl`

2. 新建`updated.ejs`

考虑兼容性，模仿`{theme_path}/layout/_partial/post/date.ejs`新建一个`updated.ejs`文件。

修改`date.ejs`如下，其中仅添加了`<%= __('published') %>`字段，用于定义语言。

```js
<a href="<%- url_for(post.path) %>" class="<%= class_name %>">
  <%= __('published') %> <time class="dt-published" datetime="<%= date_xml(post.date) %>" itemprop="datePublished"><%= date(post.date, date_format) %></time>
</a>
```

新建相似的`updated.ejs`如下，其中定义了`<%= __('updated') %>`字段，用于定义语言。

```js
<a href="<%- url_for(post.path) %>" class="<%= class_name %>">
  <%= __('updated') %> <time class="dt-published" datetime="<%= date_xml(post.updated) %>" 
  itemprop="dateUpdated"><%= date(post.updated, date_format) %></time>
</a>
```

3. 增加字段语言定义

由于`date.ejs`和`updated.ejs`中分别引入了`published`和`updated`字段，因此需要在语言文件中新增对应的字段。
语言文件在 `{theme_path}/languages`中，按照主题的语言设置，修改对应的语言文件，没指定就修改`default.yml`文件。

在`{theme_path}/languages/zh-CN.yml`中增加下述内容，格式为`字段名: 字段值`

```yaml
published: 发布于
updated: 更新于
```

4. 修改`article.ejs`

该文件中记录了文章日期列的显示内容，在已有的发布时间后增加更新时间。

```js
<div class="article-meta">
<%- partial('post/date', {class_name: 'article-date', date_format: null}) %>
<%- partial('post/updated', {class_name: 'article-date', date_format: null}) %>
<%- partial('post/category') %>
</div>
```

5. 对日期的样式进行微调

由于发布时间于更新时间的间隔太小，影响美观，因此在`{theme_path}/source/css/_partial/article.styl`中找到样式`article-date`，进行样式微调。

原样式：
```css
.article-date
  @extend $block-caption
  float: left
```

在样式后增加一个空格，更新为

```css
.article-date
  @extend $block-caption
  float: left
  &:after
    content: "\00a0"
```

©️ 本节内容部分参考 [link](https://blog.vanxnf.top/2018/09/03/Hexo-%E5%8D%9A%E5%AE%A2-Hiker-%E4%B8%BB%E9%A2%98%E5%A2%9E%E5%8A%A0%E6%96%87%E7%AB%A0%E6%9C%80%E5%90%8E%E7%BC%96%E8%BE%91%E6%97%B6%E9%97%B4%EF%BC%8C%E5%B9%B6%E6%8C%89%E7%85%A7%E6%9C%80%E5%90%8E%E7%BC%96%E8%BE%91%E6%97%B6%E9%97%B4%E6%8E%92%E5%BA%8F/)


## 参考

1. 本站使用的模版：[hexo-theme-mashiro](https://github.com/bill-xia/hexo-theme-mashiro)
2. TODO：站内搜索 [link](https://liam.page/2017/09/21/local-search-engine-in-Hexo-site/)

