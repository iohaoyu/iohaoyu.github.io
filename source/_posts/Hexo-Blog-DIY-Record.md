---
title: Hexo博客踩坑记录
date: 2020-07-03 08:02:17
tags: ["博客"]
---

### 修改代码框和高亮颜色

`themes\ocean\source\css\_partial\highlight.styl`

### 新建一个空白page，不受原来的hexo模板影响；

[在html文件头加`layout: false`](https://segmentfault.com/q/1010000002564944)

### 显示摘要，不显示全文；

在写 Markdown 的时候自定义摘要文字，正文之前加`<!--more-->`，比如：

```
摘要...
<!--more-->
正文...
```
<!--more-->

### 选择一篇文章置顶

需要安装插件 [hexo-generator-index-pin-top](https://github.com/netcan/hexo-generator-index-pin-top)：

```
$ npm uninstall hexo-generator-index --save
$ npm install hexo-generator-index-pin-top --save
```

然后在需要置顶的文章的 Front-matter 区域加上 `top: ture` ，示例：

```
---
title: 新增文章置顶
author: zhwangart
date: 2019-07-18 15:45:03
top: ture
---
```

### 文章添加封面图片

```
---
title: Post name
date: 2019-07-24 22:01:03
photos: [
        ["/images/相机.jpg"], // themes/ocean/source/images目录下
        ["https://tuchong.pstatp.com/2716763/f/531173888.jpg"]
  ]
---
```

在首页只会显示第一张，详情页会按顺序显示这两张

### 一篇文章添加多个标签

比如`tags: ["Photo", "Life"]`

```html
---
title: 作品《燕大视觉印象》
date: 2019-02-18 15:11:21
tags: ["Photo", "Life"]
top: ture
---
```

### 往相册页里加图片

已经新建了一个相册页面，怎么发布照片？写在 markdown 的 Front-matter 区域，注意格式，和markdown里的插入链接有区别：

```
---
title: Gallery
albums: [
        ["img_url","img_caption"],
        ["img_url","img_caption"]
        ]
---
```

### 修改右侧导航栏图标

1.在[开源图标feathericons](https://feathericons.com/)里找到心仪的图标；

2.通过图标给的英文关键词去`themes/ocean/source/css/_feathericon.styl`寻找对应的图标编号；

3.然后去修改`./themes/ocean/source/css/_partial/navbar.styl`对应的图标编号

先修改`source/css/_partial/navbar.styl`

再去修改主题目录下的_config.yml里的menu

### 导航栏图表改为在文字左侧

修改\themes\ocean\source\css\_partial\navbar.styl

```
&.nav-main
      .nav-item-link
        &::before, i.fe
          display block
          line-height 1
        &::before
          font-family 'feathericon'
```

修改为：

```
    &.nav-main
      .nav-item-link
        &::before, i.fe
          // display block
          line-height 1
          margin-right: 10px;
        &::before
          font-family 'feathericon'
```

如在gitbash编辑里遇到缩进异常，则可以在Sublime txt里打开，在sublimeText的preferences-setting(user)中设置显示空格符号，把`“draw_white_space”`的属性设置成`“all”`，就可以在编辑器里看见空格和tab的显示，对tab进行修改就可以了。[参考](https://blog.csdn.net/soxiuzi/article/details/79313655)

### 修改网站logo

替换`themes\ocean\source\images`下的两个svg文件，`hexo-inverted.svg`和`hexo.svg`，文件名不要变动；

### 添加歌单页

[参考教程](https://blog.csdn.net/soul7y/article/details/105768906)

我这里用的是自己的网易云歌单“我最喜欢的音乐”，怎么获取这个歌单的id？打开手机网易云音乐app，分享这个歌单时选择复制链接，生成的链接是这样的：

> 分享王浩宇_创建的歌单「我喜欢的音乐」: http://music.163.com/playlist/148890665/120047292/?userid=120047292 (来自@网易云音乐)

`playlist`后紧跟的148890665就是需要的歌单id。

### 添加网站字数，阅读时间统计

### hexo本地和github不一致

试试

```
hexo clean
hexo d -g
```

#### 添加网站总字数记录

在根目录下运行：

```shell
npm install hexo-wordcount –save
```

在`\themes\ocean\_config.yml`主题配置文件中加入：

```yaml
post_wordcount:
  item_text: true
  wordcount: true
  min2read: true
  totalcount: true
  separated_meta: true
```

在`\themes\ocean\layout\_partial\footer.ejs`文件中，在`<ul class="list-inline">`标签后加入：

```html
<ul class="list-inline">
	<li>全站共<span class="post-count"><%= totalcount(site) %></span>字</li>
</ul>
```

#### 添加文章字数统计和预估阅读时间

在`themes/ocean/layout/_partial/article.ejs`里的

```html
<div class="article-meta">
    <%- partial('post/date', {class_name: 'article-date', date_format: null}) %>
   <%- partial('post/category') %>
</div>
```
中间加上
```html
&emsp;<i class="fe fe-bar-chart"></i> <span class="post-count"><%- wordcount(post.content) %></span>字
&emsp;<i class="fe fe-clock"></i> <span class="post-count"><%- min2read(post.content) %></span>分钟
```
变成这样：
```html
<div class="article-meta">
   <%- partial('post/date', {class_name: 'article-date', date_format: null}) %>
   &emsp;<i class="fe fe-bar-chart"></i> <span class="post-count"><%- wordcount(post.content) %></span>字
   &emsp;<i class="fe fe-clock"></i> <span class="post-count"><%- min2read(post.content) %></span>分钟
   <%- partial('post/category') %>
</div>
```

### 图标从文字上面，放到文字左边；

### **hexo系列问题之部署到github时会删掉README文件**

[设置skip_render](https://blog.csdn.net/wxl1555/article/details/79291865/)

### 解决使用hexo deploy命令之后自定义域名失效的问题

https://blog.csdn.net/qq_42893625/article/details/102574242

### [hexo deploy时重复输入用户名密码的问题](https://www.jianshu.com/p/f8b6717e1238)

[github提示Permission denied (publickey)，如何才能解决？](https://www.zhihu.com/question/21402411)

### 参考链接

[关于 Ocean 使用中的问题](https://zhwangart.github.io/2019/07/02/Ocean-Issues/)

[hexo从零开始到搭建完整](https://www.cnblogs.com/visugar/p/6821777.html)

[基于Hexo-Ocean主题博客搭建-andus](http://blog.andus.top/2019/09/30/基于Hexo-Ocean主题博客搭建/)

[开源图标feathericons](https://feathericons.com/)

[高亮配置文件highlight.js demo](https://highlightjs.org/)

[RGB颜色值与十六进制颜色码转换工具](https://www.sioe.cn/yingyong/yanse-rgb-16/)

[Ocean主题作者的博客](https://zhwangart.github.io/)

### 备注

- 修改theme下的文件，不需要断开hexo s，直接hexo g就可刷新；

- 修改根目录下的文件，hexo g后还要重新hexo s才能刷新更改；