---
title: 我的个人博客之旅：从jekyll到hexo
comments: true
mathjax: true
categories:
  - 博客搭建
tags:
  - jekyll
  - hexo
  - github
  - gitee
---

## 前言

&emsp;&emsp;喜欢写Blog的人，会经历三个阶段。

- 第一阶段，刚接触Blog，觉得很新鲜，试着选择一个免费空间来写。
- 第二阶段，发现免费空间限制太多，就自己购买域名和空间，搭建独立博客。
- 第三阶段，觉得独立博客的管理太麻烦，最好在保留控制权的前提下，让别人来管，自己只负责写文章。

引自[阮一峰](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)

&emsp;&emsp;第一阶段我已经经历过了，目前在CSDN的文章仍然在更新。但是作为一个免费空间，一个技术博客的聚集地，其管理和运营虽说正在变得越来越好，但是恶心人的事件也时有发生，比如对新手不友好的审核机制、近期改版造成的各种不兼容问题。

&emsp;&emsp;于是，就想着挣脱枷锁，向第二第三阶段发展。

&emsp;&emsp;我这人吧凡事都考虑的比较详尽，，我感觉我如果再去经历第二阶段的话既浪费精力又消耗时间，而且自己也过了玩网站、玩博客的年纪，如果申请个域名再搞个网站，我不知道这股热度会持续多久。

&emsp;&emsp;所以，我就直接跳到了第三个阶段，开始在github上搭建自己的博客。由于自己对前端一无所知，即使使用现成的博客框架，刚开始玩的时候特别费劲。但是经过不断摸索，我的博客已经基本成型，传送门开启：[wordzzzz的个人博客-托管于github](https://wordzzzz.github.io/) [wordzzzz的个人博客-托管于gitee](https://wordzzzz.gitee.io/)。

&emsp;&emsp;本篇博文并不打算长篇大论的介绍基于GitHub Pages或者Gitee Pages搭建博客的步骤，因为这类的文章实在是太多了，青菜萝卜又各有所爱，不如给出资源，让大家自己折腾。所以我只是在此有序贴出我在搭建博客的过程中用到的各种有用资源，以及搭建博客的大致流程，也算是对我这段时间的一个告别仪式吧。

## GitHub Pages

&emsp;&emsp;我想在GitHub Pages推出之前，由于技术门槛的存在，第三个阶段应该会很少有人涉足。所以在开始一切之前，我们先来看看什么是[GitHub Pages](https://pages.github.com/)。

&emsp;&emsp;Github Pages 是面向用户、组织和项目开放的公共静态页面搭建托管服 务，站点可以被免费托管在 Github 上，你可以选择使用 Github Pages 默 认提供的域名 github.io 或者自定义域名来发布站点。Github Pages 支持 自动利用 Jekyll 生成站点，也同样支持纯 HTML 文档，将你的 Jekyll 站 点托管在 Github Pages 上是一个不错的选择。

&emsp;&emsp;网站首页就是搭建GitHub Pages的过程其中第一步之后，选择不同的git客户端选项，会出现相应的初始化步骤，很人性化。



&emsp;&emsp;大家可以跟着上面的链接先在自己的github新建仓库，仓库名称为username.github.io，其中username要替换成你github的名称，比如我的github名称为wordzzzz，所以我新建的仓库就应该是wordzzzz.github.io。那么等我以后搭建好了我的博客，我就可以通过https://wordzzzz.github.io来访问我的主页了。

&emsp;&emsp;到现在为止，只是搭建博客的准备工作。搭建博客的下一步是选择合适的静态博客框架。

## jekyll or hexo

&emsp;&emsp;目前有两大静态博客主流框架：[jekyll](http://jekyllcn.com/)和[hexo](https://hexo.io/)。





&emsp;&emsp;我一开始用的是jekyll，这是中文社区翻译出来的[中文开发文档](http://jekyllcn.com/)。我使用的主题是[Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/)，开发文档很详细。但是后来由于jekyll体验不是很好（中文资料少，我英语比较差我会说嘛），依赖环境总是出问题（需要安装ruby），而且我使用的这个主题是个人维护的，种种原因导致最后做出来的博客很难符合我的胃口，最后被我扔进了停尸房[jekyll_mysite](https://github.com/WordZzzz/jekyll_mysite)。



&emsp;&emsp;就在我将要放弃之时，hexo拯救了我。对，没错，它有[中文开发文档](https://hexo.io/zh-cn/)。而在hexo界，使用最多的主题就是[next](http://theme-next.iissnan.com/)了。光是看到这两份资料，我就已经激动的不行了，这种扁平化设计的网站，不就正是我需要的么。加上详尽的开发文档和丰富的第三方接口，让我对它爱不释手。最终定稿了自己的个人博客，存储在github[wordzzzz的个人博客-托管于github](https://wordzzzz.github.io/)和gitee[wordzzzz的个人博客-托管于gitee](https://wordzzzz.gitee.io/)上。



## next主题

&emsp;&emsp;next主题支持三种外观显示，支持多国语言，5套代码高亮主题，可以深度定制。在其[Github](https://github.com/iissnan/hexo-theme-next)上，更是有三个主题的代表作，其中我最喜欢的莫过于基于Muse scheme的[wanghao的博客](https://notes.wanghao.work/)。于是，我就在wanghao的博客的基础上进行了相应的更改，形成了我现在的博客，主题文件全部在我的[github](https://github.com/WordZzzz/hexo-next)上，欢迎大家fork、star、follow。

&emsp;&emsp;其实先按照hexo配置开发环境，再按照next文档配置站点文件，完全可以轻松搭建起自己的博客。但是还是藏不住内心那颗年轻的心啊，终究还是搜罗了一些好玩的东西放到了自己的博客上，比如音乐播放器。

