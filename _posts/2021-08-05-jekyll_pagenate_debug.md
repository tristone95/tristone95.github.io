---
layout: post
title:  "关于jekyll博客未生成文章列表排障"
date:   2021-08-05 15:43:19 +0800
categories: [杂记] 
tags: [Jekyll,Debug] 
---
> 本文仅记录了搭建后遇到的一个 bug ，Jekyll搭建过程可见另一篇博文[留坑，github 博客搭建]()。bug 原因是 Jekyll 版本不一致，原主题 [maupassant](https://github.com/imkarl/maupassant-jekyll) 版本 Jekyll 低于 3.0，而博主 Jekyll 版本为 4.0.2。
https://jekyllrb.com/docs/variables/#paginator

## 搭建即遭遇bug
经过一番折腾，万事俱备,满怀期待的敲入

```cmd
jekyll build
ekyll server
```
然后打开 [127.0.0.1:4000]，结果如下

![初次网站打开图片](https://cdn.jsdelivr.net/gh/tristone95/imgs/2021/202108100901513.png)

Σ(っ °Д °;)っ 我的文章呢？Jekyll给我吃了吗？
是不是我本地 Jekyll 搭建有问题呢(后来知道本地真可能出问题，机缘巧合下鬼使神差提前安装了 jekyll-paginate 包)？上传 Github 看一下。好的，还是一样，没有列表。

怎么办，换主题？浪了半天，就看上这款啊！自己动手解决？Ruby 、Jekyll 都没用过啊，完全不了解啊，怎么解决呢？

暂时不管，先进行其他内容的个人定制修改吧。

A Few Moment Later......

定制修改完成，经此过程，对于jekyll的架构、运行逻辑、原理有了了解，学过的一点前端知识也有了些许的回忆。然后，回到最初的问题，**首页不加载文章列表**，试试自己动手解决吧。

第一步，不管三七二十一，先F12

先看Console,没有报错，那再看代码吧。

![控制台查找](https://cdn.jsdelivr.net/gh/tristone95/imgs/2021/202108100920090.png)

通过掌握的一点前端知识，直接问题 ```_layouts``` 中的 ```base.html``` 代码段 ```\{\{ content }}``` 为空

```html
<div class="content_container">
    {{ content }}
</div>
```

再转到 ```{{content}}``` 代码段即 ```index.html``` 

![](https://cdn.jsdelivr.net/gh/tristone95/imgs/2021/202108100933859.png)

for 循环没加载，那么```paginator.posts``` 有问题

for循环没内容，那么问题就出在```paginator.posts```这个字段咯。测试一下，将paginator改为site,文章成功加载。至此，定位分页器有问题导致。很顺嘛，一阵得意。

第二步，遇事不知问 Google ，尝试 Google-->jekyll paginate no pages，发现
[Jekyll paginator generates no pages - Stack Overflow](https://stackoverflow.com/questions/19308854/jekyll-paginator-generates-no-pages)一贴，这不正好嘛（其实在此之前也不知道看了多少无用的文章）。

顺利找到问题原因

![stackoverflow Jekyll paginator](https://cdn.jsdelivr.net/gh/tristone95/imgs/2021/202108101322096.png)
Jekyll 3.0弃用了默认pagination，需要在 ```_config.yml``` 中添加 
```yml
gems: [jekyll-paginate]
```
以启用。Caught you！顺便查看一下[ Github Page 依赖](https://pages.github.com/versions/)，Jekyll 版本为 3.9.0 ，没问题。

- 注：用 gems 变量，还需要在 Gemfile 中添加:
    ```r
    gem 'jekyll-paginate', group: :jekyll_plugins
    ```
    否则有告警。可使用
    ```yml
    plugins: 
    - jekyll-paginate
    ```
    则无需配置 Gemfile 。

那么接下来，```_config.yml``` 中添加
```yml
plugins: 
    - jekyll-paginate
```
再次
```
jekyll build
jekyll server
```
应该解决了吧...... What ，还是不行？
再次检查paginate相关代码及配置，没问题了呀？还是没生效呢？还有哪里疏漏了吗？再次网上逛一逛有关paginate文章，找到一篇较详细的[ Paginate食用指北 ](https://segmentfault.com/a/1190000007709682)，跟着文章比对每一处设置，没问题呀？在看了一遍又一遍后，又一次回到第一步，```_config.yml``` 中配置
```yml
paginate:5
paginate_path: "page:num"
```
我也设置了呀，等等
```yml
page_size: 10 
paginate_path: ":num"
```
page_size 什么鬼，旧版本变量？搜一圈没看到相关信息，先修改，再次 build，server。

![博客成功截图](https://cdn.jsdelivr.net/gh/tristone95/imgs/2021/202108110918627.png)

OK ！至此，博客初步搭建完成，剩余一些小修改就可以安心写文章了！

*★,°*:.☆(￣▽￣)/$:*.°★* 。

最后，附上[Jekyll 官方文档 https://jekyllrb.com/docs](https://jekyllrb.com/docs/)

