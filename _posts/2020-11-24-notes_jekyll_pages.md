---
layout: post
title: 博客修护笔记
category: geek
description: "如何修改GitHub page中每篇博客的链接？如何加入全文搜索功能？如何把fork来的仓库之前的commit都清零？"
tags: [web,code]
---

如何修改GitHub page中每篇博客的链接？如何加入全文搜索功能？如何把fork来的仓库之前的commit都清零？


## 修改文章post链接地址

jekyll默认为博客配置的地址实在有些长，比如本文曾经默认的链接地址是*/2020/11/24/improve_jekyll_pages*。这么多层叠加，不利于搜索引擎寻找都该页面。所以为了方便搜索，所谓SEO优化。

一是修改博客链接。

博客的地址是采用`permalink`设置的。参考官网 [Permalinks_Jekyll](https://jekyllrb.com/docs/permalinks/)。

只要在根目录下的`_config.yml`文件中，写上一行`permalink: /:title.html`。中间有一个空格。

二是在每个博客页面的前面，加一个description.几句话描述下这个页面。

## 加入全文搜索功能

使用的现成的代码仓库在这里：[Simple-Jekyll-Search](https://github.com/christian-fei/Simple-Jekyll-Search)

ref. [可能是最全面的github pages搭建个人博客教程](https://lemonchann.github.io/create_blog_with_github_pages/)

设计样式挑选自[20 Creative Search Bar Design](https://www.mockplus.com/blog/post/search-bar-design)，照抄代码在[这里](https://codepen.io/AlbertFeynman/pen/BPvzWZ)

## 修改配色

想把原来的颜色变浅一点，但是在同样的色系里，比如之前的搜索键的深红色换成浅红。

可以通过代码中的颜色编码 **#ca4d4d** 在这个网站 [Hex Color Codes](https://color-hex.org/color/ca4d4d)可以找到比它更深或更浅的颜色代码。

## 将所有之前别人的commits都清零

保持现在博客状态，但是如何把commit历史清零？以下是安全做法：
[git - how to delete all commit history in github? - Stack Overflow](https://stackoverflow.com/questions/13716658/how-to-delete-all-commit-history-in-github)

1. 新建一个孤子分支，并进入这个分支

    `git checkout --orphan temp_branch`

2. 在孤子分支中复制所有master分支的文件，commit

    `git add -A`

    `git commit -am "my first message"`

3. 删除原本的master分支

    `git branch -D master`

4. 把孤子分支改名为master

    `git branch -m master`

4. 强制更新远端仓库

    `git push -f origin master`

## Changelog

- 201124 erica init