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

----------

[toc]

----------

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

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20180110111003603?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

&emsp;&emsp;大家可以跟着上面的链接先在自己的github新建仓库，仓库名称为username.github.io，其中username要替换成你github的名称，比如我的github名称为wordzzzz，所以我新建的仓库就应该是wordzzzz.github.io。那么等我以后搭建好了我的博客，我就可以通过https://wordzzzz.github.io来访问我的主页了。

&emsp;&emsp;到现在为止，只是搭建博客的准备工作。搭建博客的下一步是选择合适的静态博客框架。

## jekyll or hexo

&emsp;&emsp;目前有两大静态博客主流框架：[jekyll](http://jekyllcn.com/)和[hexo](https://hexo.io/)。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20180110111311091?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20180110111420247?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

&emsp;&emsp;我一开始用的是jekyll，这是中文社区翻译出来的[中文开发文档](http://jekyllcn.com/)。我使用的主题是[Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/)，开发文档很详细。但是后来由于jekyll体验不是很好（中文资料少，我英语比较差我会说嘛），依赖环境总是出问题（需要安装ruby），markdown采用的是Kramdown（Kramdown对我之前的一些博客格式支持的不是很好，我自己写文档用的都是小书匠，然后发表到CSDN，所以并不想花时间在改格式上面），而且我使用的这个主题是个人维护的，种种原因导致最后做出来的博客很难符合我的胃口，最后被我扔进了停尸房[jekyll_mysite](https://github.com/WordZzzz/jekyll_mysite)。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20180110111512090?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20180110111533728?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

&emsp;&emsp;就在我将要放弃之时，hexo拯救了我。对，没错，它有[中文开发文档](https://hexo.io/zh-cn/)。而在hexo界，使用最多的主题就是[next](http://theme-next.iissnan.com/)了。光是看到这两份资料，我就已经激动的不行了，这种扁平化设计的网站，不就正是我需要的么。加上详尽的开发文档和丰富的第三方接口，让我对它爱不释手。最终定稿了自己的个人博客，存储在github[wordzzzz的个人博客-托管于github](https://wordzzzz.github.io/)和gitee[wordzzzz的个人博客-托管于gitee](https://wordzzzz.gitee.io/)上。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20180110142732584?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20180110142739639?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

## next主题

&emsp;&emsp;next主题支持三种外观显示，支持多国语言，5套代码高亮主题，可以深度定制。在其[Github](https://github.com/iissnan/hexo-theme-next)上，更是有三个主题的代表作，其中我最喜欢的莫过于基于Muse scheme的[wanghao的博客](https://notes.wanghao.work/)。于是，我就在wanghao的博客的基础上进行了相应的更改，形成了我现在的博客，主题文件全部在我的[github](https://github.com/WordZzzz/hexo-next)上，欢迎大家fork、star、follow。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20180110114341558?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

&emsp;&emsp;其实先按照hexo配置开发环境，再按照next文档配置站点文件，完全可以轻松搭建起自己的博客。但是还是藏不住内心那颗年轻的心啊，终究还是搜罗了一些好玩的东西放到了自己的博客上，比如音乐播放器。

&emsp;&emsp;下面我先简单介绍一下基于github平台、hexo框架的next主题博客开发步骤：

- [Github上新建username.github.io仓库并初始化]()
- [PC端安装hexo及其依赖项并熟悉开发流程](https://hexo.io/zh-cn/docs/index.html)
- [下载我的主题文件](https://github.com/WordZzzz/hexo-next)或者[下载next主题文件](https://github.com/iissnan/hexo-theme-next)
- [按照next官方教程验证主题](http://theme-next.iissnan.com/getting-started.html)
- [按照next官方教程配置站点文件和主题文件](http://theme-next.iissnan.com/theme-settings.html)
- [按照next官方教程集成第三方服务](http://theme-next.iissnan.com/third-party-services.html)
- [生成静态文件](https://hexo.io/zh-cn/docs/generating.html)
- [开启本地服务查看站点效果](https://hexo.io/zh-cn/docs/server.html)
- [部署至Github](https://hexo.io/zh-cn/docs/deployment.html)

&emsp;&emsp;文档都非常详细，下面我主要就第三方服务做一些说明。我提到的大部分三方服务在[next的官方文档](http://theme-next.iissnan.com/third-party-services.html)都提及到了，所以具体配置大家跟着官方文档走就行，我只是为每一类服务选择哪个做一下建议。

### 评论系统

&emsp;&emsp;我用的韩国的[livere](https://livere.com/)，从国内到国外，支持几乎全部社交账号登陆，具体步骤请按照[next的官方文档](http://theme-next.iissnan.com/third-party-services.html)操作。

### 数据统计与分析

&emsp;&emsp;[百度统计](https://tongji.baidu.com/web/welcome/login)和[google分析](https://www.google.com/intl/zh-CN/analytics/)我都加上了，具体步骤请按照[next的官方文档](http://theme-next.iissnan.com/third-party-services.html)操作。

### 阅读量统计

&emsp;&emsp;我用的[LeanCloud](https://leancloud.cn/)，具体操作步骤请直接跳转至[ 为NexT主题添加文章阅读量统计功能](https://notes.wanghao.work/2015-10-21-%E4%B8%BANexT%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E9%98%85%E8%AF%BB%E9%87%8F%E7%BB%9F%E8%AE%A1%E5%8A%9F%E8%83%BD.html#%E9%85%8D%E7%BD%AELeanCloud)。

### 内容分享服务

&emsp;&emsp;我采用的是[need-more-share2](https://github.com/revir/need-more-share2)，直接在主题配置文件里面打开就行。

### 搜索服务

&emsp;&emsp;我采用的是[Swiftype](https://swiftype.com/)，具体步骤请按照[next的官方文档](http://theme-next.iissnan.com/third-party-services.html)操作。

### 网站收录

&emsp;&emsp;[Google Webmaster tools](https://www.google.com/webmasters/tools/)收录特别快，具体步骤请按照[next的官方文档](http://theme-next.iissnan.com/third-party-services.html)操作。但是[百度站长](http://ziyuan.baidu.com/?castk=LTE%3D)收录的就很慢了，我的到现在还没被收录。

### 其他服务

&emsp;&emsp;NexT 借助于 MathJax 来显示数学公式，此选项默认关闭，如果博客中有公式，那么一定要打开这个选项。

## next进阶

&emsp;&emsp;最后想说一下其他一些配置，比如添加背景图片、侧边栏头像旋转、侧边栏鼠标滑入显示、背景音乐等等，此处大部分参考[这个博客](http://mashirosorata.vicp.io/HEXO-NEXT%E4%B8%BB%E9%A2%98%E4%B8%AA%E6%80%A7%E5%8C%96%E9%85%8D%E7%BD%AE.html)。

&emsp;&emsp;next人性化的为用户提供了custom接口，我们可以在不影响主题文件的基础上进行个性化定制。

### 给页面添加背景图片

&emsp;&emsp;在theme/next/source/css/_custom文件夹下打开custom.styl文件，往里面添加以下代码：

```styl
body {
  background: url(/images/blogbk.jpg) no-repeat;
  /* 背景图垂直、水平均居中 */
  background-position: center center;
  /* 当内容高度大于图片高度时，背景图像的位置相对于viewport固定 */
  background-attachment: fixed;
  /* 让背景图基于容器大小伸缩 */
  background-size: cover;
  /* 设置背景颜色，背景图加载过程中会显示背景色 */
  background-color: rgba(0, 0, 0, 0.5);
}
```

&emsp;&emsp;其中的css样式属性都可以根据你的自定义图片来更改，以达到最佳的效果。

### 给侧边栏添加背景图片

&emsp;&emsp;在theme/next/source/css/_custom文件夹下打开custom.styl文件，往里面添加以下代码：

```styl
#sidebar {
            background:url(/images/sidebar.jpg);
            background-size: cover;
            background-position:center;
            background-repeat:no-repeat;
            p,span,a {color: rgba(255, 255, 255, 1);}
}
```

###  文字背景色以及半透明的设置

&emsp;&emsp;在theme/next/source/css/_custom文件夹下打开custom.styl文件，往里面添加以下代码：

```styl
.content {
            border-radius: 20px;
			padding: 30px 60px 30px 60px;
            background:rgba(255, 255, 255, 0.8) none repeat scroll !important;
         }
```

&emsp;&emsp;其中border-radius是给文章背景设置圆角，margin-top是设置文章到顶部的距离，其中属性可根据自己的需要进行调整。

### 评论(来必力)添加背景色

&emsp;&emsp;在theme/next/source/css/_custom文件夹下打开custom.styl文件，往里面添加以下代码：

```styl
#lv-container {
       border-radius: 20px;
	   padding: 30px 60px 30px 60px;
       background:rgba(255, 255, 255, 0.8) none repeat scroll !important;
    }
```

&emsp;&emsp;和上面一样，背景色和圆角可自己调整更改。

### 实现点击出现桃心效果

&emsp;&emsp;在网址输入如下

```
http://7u2ss1.com1.z0.glb.clouddn.com/love.js
```

&emsp;&emsp;然后将里面的代码copy一下，新建love.js文件并且将代码复制进去，然后保存。将love.js文件放到路径/themes/next/source/js/src里面，然后打开\themes\next\layout\_layout.swig文件,在末尾（在前面引用会出现找不到的bug）添加以下代码：

```html
<!-- 页面点击小红心 -->
<script type="text/javascript" src="/js/src/love.js"></script>
```

### 侧边栏头像旋转

&emsp;&emsp;打开\themes\next\source\css\_common\components\sidebar\sidebar-author.styl，在里面添加如下代码：

```styl
.site-author-image {
  display: block;
  margin: 0 auto;
  padding: $site-author-image-padding;
  max-width: $site-author-image-width;
  height: $site-author-image-height;
  border: $site-author-image-border-width solid $site-author-image-border-color;
  
  /* start*/
  border-radius: 50%
  webkit-transition: 1.4s all;
  moz-transition: 1.4s all;
  ms-transition: 1.4s all;
  transition: 1.4s all;
  /* end */
}

/* start */
.site-author-image:hover {

  background-color: #55DAE1;
  webkit-transform: rotate(360deg) scale(1.1);
  moz-transform: rotate(360deg) scale(1.1);
  ms-transform: rotate(360deg) scale(1.1);
  transform: rotate(360deg) scale(1.1);
}
/* end */
```

### 设置鼠标划入侧边栏才显示站点信息：

#### 设置自定义div

&emsp;&emsp;在theme/next/layout/_macro文件夹下打开sidebar.swig文件，找到以下代码行的位置：

```html
<nav class="site-state motion-element">
```

&emsp;&emsp;在其上添加以下代码：

```html
<!--my custom code begin-->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.0/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.5.0/velocity.min.js"></script>
<script type="text/javascript">
  $("#sidebar").hover(function(){
    $("#mydivshow").velocity('stop').velocity({opacity: 1});
  },function(){
    $("#mydivshow").velocity('stop').velocity({opacity: 0});
  });
</script>
<div id="mydivshow" class="mydivshow">
<!--my custom code end-->
```

&emsp;&emsp;然后找到代码行：

```html
</section>
{% if display_toc and toc(page.content).length > 1 %}
<!--noindex-->
<section class="post-toc-wrap motion-element sidebar-panel sidebar-panel-active">
```

&emsp;&emsp;在此</section>的上方添加一个</div>，如下所示：

```html
<!--my custom code begin-->
</div>
<!--my custom code end-->
</section>
{% if display_toc and toc(page.content).length > 1 %}
<!--noindex-->
  <section class="post-toc-wrap motion-element sidebar-panel sidebar-panel-active">
    <div class="post-toc">
```

#### 自定义区域的初始化设置

&emsp;&emsp;在theme/next/source/css/_custom文件夹下打开custom.styl文件，向里面增添下列代码：

```styl
#mydivshow{opacity: 0;}
```

注：具体代码添加位置以及代码里的section.site-overview可以自己修改，<div id="mydivshow" class="mydivshow">和末尾的</div>是控制显示区域，section.site-overview则是用户鼠标滑入划出时的触发事件区域。

### 自定义音乐播放器

&emsp;&emsp;描述：本站所用的音乐播放器是由DIYgod所制作的APlayer，其详细资料可参见[这里](https://aplayer.js.org/docs/#/)。

#### 安装APlayer插件

```
$ npm install aplayer --save
```

&emsp;&emsp;安装完后在node_modules目录下找到APlayer.min.js文件，将其复制到theme/next/source/js/src/目录下。

#### 生成音乐播放器

&emsp;&emsp;在你想要加入音乐播放器的地方插入以下代码，本站把他放在了侧边栏里，具体操作如下。

&emsp;&emsp;打开theme/next/layout/_custom/文件夹下的sidebar.swig文件，向其中添加以下代码：

```html
<div id="player1" class="aplayer"></div>
<script src="/js/src/APlayer.min.js"></script>
<script type="text/javascript">
var ap = new APlayer({
    element: document.getElementById('player1'),                       // Optional, player element
    narrow: false,                                                     // Optional, narrow style
    autoplay: true,                                                    // Optional, autoplay song(s), not supported by mobile browsers
    showlrc: 0,                                                        // Optional, show lrc, can be 0, 1, 2, see: ###With lrc
    mutex: true,                                                       // Optional, pause other players when this player playing
    theme: '#e6d0b2',                                                  // Optional, theme color, default: #b7daff
    mode: 'random',                                                    // Optional, play mode, can be `random` `single` `circulation`(loop) `order`(no loop), default: `circulation`
    preload: 'metadata',                                               // Optional, the way to load music, can be 'none' 'metadata' 'auto', default: 'auto'
    listmaxheight: '513px',                                             // Optional, max height of play list
    music: {                                                           // Required, music info, see: ###With playlist
        title: 'Preparation',                                          // Required, music title
        author: 'Hans Zimmer/Richard Harvey',                          // Required, music author
        url: 'http://7xifn9.com1.z0.glb.clouddn.com/Preparation.mp3',  // Required, music url
        pic: 'http://7xifn9.com1.z0.glb.clouddn.com/Preparation.jpg',  // Optional, music picture
        lrc: '[00:00.00]lrc here\n[00:01.00]aplayer'                   // Optional, lrc, see: ###With lrc
    }
});
</script>
```

#### 自定义播放器样式

&emsp;&emsp;包含颜色更改，列表歌曲信息的排版修改。

&emsp;&emsp;在theme/next/source/css/_custom文件夹下打开custom.styl文件，往里面添加以下代码：

```styl
.aplayer-list ol li:hover {   /*列表悬停颜色*/
                  background:rgba(255, 255, 255, 1) none repeat scroll !important;}
.aplayer-list ol li {   /*列表底色*/
                        background:rgba(255, 255, 255, 1);}
.aplayer-list-light {   /*列表播放歌曲颜色*/
                      background:rgba(255, 255, 255, 1) none repeat scroll !important;}
#player1 {    /*边框样式*/
          border-radius: 6px;
          div,ol {border-radius: 6px;}
        }
#player1 *{color: rgba(255, 255, 255, 1);}    /*字体颜色*/
/*列表歌曲信息的排版*/
.aplayer-list-index{float:left;}
.aplayer-list-title{float:left;}
.aplayer-list-author{float:right;}
```

### 自定义萌萌哒音乐播放控制边栏

&emsp;&emsp;这一步要在自定义音乐播放器的配置完成之后才能进行，因为aplayer-controler依赖于aplayer来实现播放功能。

#### 安装aplayer-controler插件

```
npm install aplayer-controler --save
```

#### 添加js代码

&emsp;&emsp;安装APlayer-Controler的js文件：[APlayer-Controler.js](https://github.com/Mashiro-Sorata/APlayer-Controler/blob/master/demo/src/Aplayer-Controler.js)

&emsp;&emsp;将其放入theme/next/source/js/src下。

#### 创建按钮区域

&emsp;&emsp;在theme/next/layout/_custom/文件夹下新建一个myapcontroler.swig的文件。向其中添加以下代码：

```html
<script src="/js/src/Aplayer-Controler.js"></script>
<div id="AP-controler"></div>
<script type="text/javascript">
var myapc=new APlayer_Controler({
		APC_dom:$('#AP-controler'),
		aplayer:ap, //此为绑定的aplayer对象
		attach_right:true,
		position:{top:'300px',bottom:''},
		fixed:true,
		btn_width:100,
		btn_height:120,
		img_src:['http://oty1v077k.bkt.clouddn.com/bukagirl.jpg',
				'http://oty1v077k.bkt.clouddn.com/jumpgirl.jpg',
				'http://oty1v077k.bkt.clouddn.com/pentigirl.jpg',
				'http://oty1v077k.bkt.clouddn.com/%E8%90%8C1.gif'],
		img_style:{repeat:'no-repeat',position:'center',size:'contain'},
		ctrls_color:'rgba(173,255,47,0.8)',
		ctrls_hover_color:'rgba(255,140,0,0.7)',
		tips_on:true,
		tips_width:140,
		tips_height:25,
		tips_color:'rgba(255,255,255,0.6)',
		tips_content:{},
		timeout:30
	});
</script>
```

#### 将控制按钮加入body页面

&emsp;&emsp;在theme/next/layout文件夹下打开_layout.swig文件，在</body>前添加以下代码：

```html
{% include '_custom/myapcontroler.swig' %}
```

&emsp;&emsp;到此，自定义音乐播放控制边栏就基本完成，完成整个配置需要根据自己的主题背景进一步修改完善。

----------

**<font color="red" size=3 face="仿宋">本教程到此结束，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**