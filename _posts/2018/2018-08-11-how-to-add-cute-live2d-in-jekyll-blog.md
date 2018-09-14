---
layout: post
title: 如何给你的Jekyll博客添加可爱的二次元看板娘(Live2D)
author: suki
tags:
- jekyll
- live2d
- hexo
categories: Tutorial
english: false
featured: "live2d-kanban.jpg"
---

有朋友问这个右下角的小东西是怎么弄出来的，今天就手把手教你。请往下看：

## Live2D

Add the Sseexxyyy live2d to your webpages! [^1] [点击查看作者博客和看板娘效果](https://huaji8.top/post/live2d-plugin-2.0/)

[^1]: 大神链接：https://github.com/xiazeyu/live2d-widget.js

给你的博客一个萌（超）萌（性）哒（感）的二次元看板娘！


可惜作者并没有直接提供Jekyll的安装方式，要我们用Jekyll的怎么活呢？

## 在Jekyll上安装

本人比较懒，也懒得研究里面的原理，所以这里就提供一个简单的“笨办法”。

既然作者提供了Hexo博客的安装方式，为什么不将计就计，用Hexo把看板娘引出来~~~。想来无论是Jekyll还是Hexo，最后都是静态的对吧？那就一定可以揪出那段代码。

事不宜迟，赶紧动手来做。重点在如何获取那段代码，其他的细节如果有疑问请留下评论。

### 安装Hexo

按照默认的方式安装Hexo。

```shell
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
$ hexo server
```
有关Hexo的细节，详见[Hexo官网](https://hexo.io/zh-cn/docs/)，这里不再赘述。



### 安装Live2D

先安装``hexo-helper-live2d``，见[该项目](https://github.com/EYHN/hexo-helper-live2d)

经测试我使用``npm install --save hexo-helper-live2d``并没有安装成功，所以可以用``yarn add hexo-helper-live2d``来安装。

将下面的代码添加到Hexo博客的配置文件``_config.xml``中：

```yaml
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  debug: false
  model:
    use: wanko
  display:
    position: right
    width: 150
    height: 300
  mobile:
    show: true
```

但这一步只下载了个空壳，不包含看板娘的。所以还需要下载看板娘``资源``。详见[live2d-widget-models](https://github.com/xiazeyu/live2d-widget-models)。

直接把这个项目下载下来，这里包含了所有的看板娘资源。下面以``live2d-widget-model-wanko``（也就是本站所用的）为例。

找到项目里的``live2d-widget-model-wanko``文件夹，把里面``assets``里面的内容（不含``assets``文件夹），拷贝到Hexo的``blog``文件夹下新建一个``live2d_models\wanko``文件夹中。


注意``_config.xml``中的``model``下面的``use``和``live2d_models``下面的文件夹相对应。

### 提取代码

之后用``hexo serve``运行看看，右下角是否有wanko了？

如果一切正常，``hexo deploy``来编译Hexo博客了。执行后，你会欣喜的发现在``public``（Hexo编译后的网站静态文件）文件夹中，找到``live2dw``文件夹，这就是编译后的看板娘文件了。


然后打开``index.html``，拉倒最底下，看到如下代码：

```html
<script src="/live2dw/lib/L2Dwidget.min.js?0c58a1486de42ac6cc1c59c7d98ae887"></script><script>L2Dwidget.init({"pluginRootPath":"live2dw/","pluginJsPath":"lib/","pluginModelPath":"assets/","tagMode":false,"debug":false,"model":{"jsonPath":"/live2dw/assets/wanko.model.json"},"display":{"position":"right","width":150,"height":300},"mobile":{"show":true},"log":false});</script>
```
恭喜你这就是召唤看板娘的代码了！其中``width``和``height``你可以根据看板娘的大小进行修改。

将``live2dw``文件夹复制到你的jekyll博客目录下，将上面的代码复制到``footer.html``模板或者想要的页面。

**大功告成！**


[模板及原文By Ike](https://done.moe/tutorial/2018/08/11/how-to-add-cute-live2d-in-jekyll-blog/)
