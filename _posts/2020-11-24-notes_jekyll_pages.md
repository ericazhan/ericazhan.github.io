---
layout: post
title: 博客修护笔记
category: geek
description: "如何修改GitHub page中每篇博客的链接？如何加入全文搜索功能？如何把fork来的仓库之前的commit都清零？。。记录博客折腾过程"
tags: [web,code]
---

如何修改GitHub page中每篇博客的链接？如何加入全文搜索功能？如何把fork来的仓库之前的commit都清零？等等问题，随时学习，随时记录。

* TOC
{:toc}

## 修改文章post链接地址

jekyll默认为博客配置的地址实在有些长，比如本文曾经默认的链接地址是*/2020/11/24/improve_jekyll_pages*。这么多层叠加，不利于搜索引擎寻找都该页面。所以为了方便搜索，所谓SEO优化。

一是修改博客链接。

博客的地址是采用`permalink`设置的。参考官网 [Permalinks_Jekyll](https://jekyllrb.com/docs/permalinks/)。

只要在根目录下的`_config.yml`文件中，写上一行`permalink: /:title.html`。中间有一个空格。

二是在每个博客页面的前面，加一个description.几句话描述下这个页面。

## 加入全文搜索功能

使用的现成的代码仓库在这里：[Simple-Jekyll-Search](https://github.com/christian-fei/Simple-Jekyll-Search)

参考： [可能是最全面的github pages搭建个人博客教程](https://lemonchann.github.io/create_blog_with_github_pages/)

自己的折腾步骤记录如下：

1. 在[这里](https://github.com/christian-fei/Simple-Jekyll-Search/tree/master/example/js)下载文件 `simple-jekyll-search.min.js`放到博客仓库里。

2. 再在仓库里放一个json文件，代码在其[wiki](https://github.com/christian-fei/Simple-Jekyll-Search/wiki#enabling-full-text-search)中。

   这样可以开展全文搜索。不过我照搬后报错，于是删掉第二部分搜索页面功能，只保留搜索博文。代码在[ericazhan.github.io/search.json](https://github.com/ericazhan/ericazhan.github.io/blob/master/public/search.json)

3. 在html文件中想放搜索框的地方，抄入如下别人的代码，两个引用文件的地址按情况调整：

    ```
    <div class="search-container">
      <input type="text" id="search-input" placeholder="search blog posts...">
      <ul id="results-container"></ul>
    </div>
    <script src="../public/simple-jekyll-search.min.js"></script>
    <script>
        window.simpleJekyllSearch = new SimpleJekyllSearch({
        searchInput: document.getElementById('search-input'),
        resultsContainer: document.getElementById('results-container'),
        json: '../public/search.json',
        searchResultTemplate: '<li><a href="{url}?query={query}" title="{desc}">{title}</a></li>',
        noResultsText: 'No results found',
        limit: 10,
        fuzzy: false,
        exclude: ['Welcome']
      })
    </script>
    ```

4. 修改搜索框样式

   设计样式挑选自[20 Creative Search Bar Design](https://www.mockplus.com/blog/post/search-bar-design)，照抄代码在[这里](https://codepen.io/AlbertFeynman/pen/BPvzWZ)。SCSS和CSS格式有差异，手动改了下，放到博客的css文件中。

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

5. 强制更新远端仓库

    `git push -f origin master`

## 使用自定义域名

1. 购买域名。

   我的域名是今年年初在godaddy买的。直接买了10年。买之前一定先搜索 godaddy优惠码！比如我用的 [最新GoDaddy 优惠码及GoDaddy常见问题汇总](https://www.laozuo.org/godaddy-youhuima)

   买完后有朋友说买贵了，godaddy并不实惠。。推荐实惠网站[NameSilo](https://www.namesilo.com/)以及实惠服务器 [Cloudways](https://www.cloudways.com/en/) 

2. 在GitHub博客仓库中新建一个文件，`CNAME` ，没有文件后缀。

   用文本打开，写入域名 *zhanluyan.com*

3. 去godaddy设置DNS，添加至少3条记录

   - CNAME-WWW-ericazhan.github.io
   - A-@-185.199.108.153
   - A-@-185.199.109.153

   A 这行的ip地址是 github page的ip 地址。官方查询在 [这里](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site)。官方一共提供4个ip地址，也可以都加上。

这三步做完，等待1小时就可以了~ 

主要参考： [陈素封：如何搭建个人独立博客？ - 知乎](https://www.zhihu.com/question/20463581/answer/25478916)

## 给博客增加目录

jekyll默认使用Kramdown来解析markdown语法，kramdown自带生成目录功能。先检查你的jekyll是不是kramdown解析，需要看 **_config.yml** 文件中是否有这一行：

```
markdown: kramdown
```


如果的确是kramadown,那么增加目录特别简单，不需要任何附加插件，自带生成目录功能。添加目录，只要在对应博客的md文件中，希望增加目录的地方写这两行代码就OK啦。

```
* TOC
{:toc}
```


参考：[How I Add a Table of Contents to my Jekyll Blog Written in Markdown](http://www.seanbuscay.com/blog/jekyll-toc-markdown/)

## 电脑手机的responsive设计

原则：哪个设备有特殊格式需求，就专门给这个设备写一个css格式。

比如，我希望自己的博客文章在电脑端的文本宽度是550px，其他手机端不变。那么对于*.post*我需要专门加一个为电脑屏幕的css格式：新格式(wid:550px)

responsive的语法很简单：

`@media (device condition) { /* CSS Rules */ }`

电脑屏幕宽度一定大于500px，所以设备条件是 min-width: 500px。

前面是*.post*原格式，后面是为*.post*加入reponsive格式

```
.post {
  margin-bottom: 1em;
}	}

@media (min-width: 500px){
  .post {
    width: 550px;
  }
}
```

commit实例：[文本宽度电脑端变短](https://github.com/ericazhan/ericazhan.github.io/commit/26567f34a3c3e2b75ccbbc0fc7e3b64b8bbcab10) ;  [搜索框手机端变小](https://github.com/ericazhan/ericazhan.github.io/commit/385b1630549d8020ba80c2f53150210adbc39d99)

ref:  [Learn Responsive Web Design Principles: Create a Media Query - freeCodeCamp.org](https://www.freecodecamp.org/learn/responsive-web-design/responsive-web-design-principles/create-a-media-query)

## 添加google analytic

1. 去[Google Analytics](https://marketingplatform.google.com/about/analytics/)注册一个号。这里用户名不用考虑太多，是属于你google账号下的分用户名。按照自己需求取名就好。
2. 选择“衡量网站”，输入你想衡量统计的网址，点击“创建”。
3. 一般创建完会自动跳到代码页。把这些js代码复制到博客<head>中就完成啦。如果没有自动跳转，那么在管理(administration) --- 跟踪信息(Flux de données)，点进去会看到"跟踪代码js"。

## 索引页面增加post计数和排序

post计数的变量是 {{ site.posts | size }}
按照categories计数，变量是 {{ site.categories.category | last | size }}

排序的功能需要使用`sort`这个liquid语言自带的filter来排序。

比如给category排序。如果不使用sort，默认是按照category添加的顺序排序。

按照字母顺序alphabetic把各category排序: 先通过assing定义一个list,再在这个排序过的list里循环  

```
{% raw %} 
{% assign sorted_categories = site.categories | sort %}
{% for category in sorted_categories %}
{% endraw %}
```

比如如果想把文章按标题排序：
```
{% raw %} 
{{ site.pages | sort: "title", "last" }}
{% endraw %}
```
PS, 为了呈现liquid语言，但又不想它被parser,需要使用[Raw标签](https://shopify.github.io/liquid/tags/raw/)。

Ref: [sort-Stack Overflow](https://stackoverflow.com/questions/59890847/how-to-sort-categories-alphabetically-with-liquid)，
[Advanced Liquid: Sort - Siteleaf](https://www.siteleaf.com/blog/advanced-liquid-sort/)， 
[Liquid Filters-Jekyll](https://jekyllrb.com/docs/liquid/filters/)

## Changelog
- 210114 +google analytic + posts统计排序
- 210110 + reponsive设计
- 201231 +博客增加目录
- 201130 加入自定义域名章节，补充搜索章节的具体步骤
- 201124 init