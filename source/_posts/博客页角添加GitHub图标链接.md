---
title: 博客页角添加GitHub图标链接
url: blog-add-github-logo
date: 2019-01-26 00:30:00
tags: 博客完善
categories: 博客
---

&#160; &#160; &#160; &#160;在我们博客上都会添加GitHub链接，让网友可以方便的fork你的项目，但是如何才能优雅的添加一个链接而不失整个页面的美观？经过几个版本终于找到这个认为比较优雅的方式，不私藏分享给大家。

<!--more-->

&#160; &#160; &#160; &#160;在我们博客上都会添加GitHub链接，让网友可以方便的fork你的项目，但是如何才能优雅的添加一个链接而不失整个页面的美观？经过几个版本终于找到这个认为比较优雅的方式，不私藏分享给大家。

其实很简单，只需要两步不需要引任何文件就可以添加，如果是博客园则在管理、设置中添加对应代码即可，下面详细说明。

1. 找到博客公用的模板，也就是所有页面都引用的位置，比如我用hexo，`layout.ejs`文件就可以。
2. 添加代码片段即可。

## HTML代码片段

- hexo搭建博客，将下述代码添加到`layout.ejs`文件`<body>`下就可以。
- 博客园在 管理--> 设置 中的 `页首Html代码`下添加如下代码即可。

``` html
<a href="https://github.com/HongwenHan" target="_blank" class="fork_github" aria-label="View source on GitHub">
    <svg width="80" height="80" viewbox="0 0 250 250" aria-hidden="true"><path d="M0,0 L115,115 L130,115 L142,142 L250,250 L250,0 Z"/><path d="M128.3,109.0 C113.8,99.7 119.0,89.6 119.0,89.6 C122.0,82.7 120.5,78.6 120.5,78.6 C119.2,72.0 123.4,76.3 123.4,76.3 C127.3,80.9 125.5,87.3 125.5,87.3 C122.9,97.6 130.6,101.9 134.4,103.2" fill="currentColor" style="transform-origin: 130px 106px;" class="octo-arm"/><path d="M115.0,115.0 C114.9,115.1 118.7,116.5 119.8,115.4 L133.7,101.6 C136.9,99.2 139.9,98.4 142.2,98.6 C133.8,88.0 127.5,74.4 143.8,58.0 C148.5,53.4 154.0,51.2 159.7,51.0 C160.3,49.4 163.2,43.6 171.4,40.1 C171.4,40.1 176.1,42.5 178.8,56.2 C183.1,58.6 187.2,61.8 190.9,65.4 C194.5,69.0 197.7,73.2 200.1,77.6 C213.8,80.2 216.3,84.9 216.3,84.9 C212.7,93.1 206.9,96.0 205.4,96.6 C205.1,102.4 203.0,107.8 198.3,112.5 C181.9,128.9 168.3,122.5 157.7,114.1 C157.9,116.9 156.7,120.9 152.7,124.9 L141.0,136.5 C139.8,137.7 141.6,141.9 141.8,141.8 Z" fill="currentColor" class="octo-body"/></svg>
</a>
```

## CSS代码片段

- 在公共CSS文件中添加如下样式即可。
- 博客园在 管理--> 设置 中的 `页面定制CSS代码`下添加如下样式即可。

``` css
    .fork_github {
      display: none;
      }
      .fork_github svg{
        fill: #15151375;
        color: #ffffff6e;
        position: fixed; 
        top: 0; 
        border: 0; 
        left: 0; 
        z-index:9999; 
        -webkit-transform: rotate(270deg);
      }
    @media (min-width: 1100px) {
      .fork_github svg {
        fill:#151513; 
        color:#fff; 
        position: fixed; 
        top: 0; 
        border: 0; 
        left: 0; 
        z-index:9999; 
        -webkit-transform: rotate(270deg);
      }
    }

    @media (min-width: 805px) {
      .fork_github{
        display: inline;
      }
    }
    .fork_github:hover .octo-arm{animation:octocat-wave 560ms ease-in-out}@keyframes octocat-wave{0%,100%{transform:rotate(0)}20%,60%{transform:rotate(-25deg)}40%,80%{transform:rotate(10deg)}}@media (max-width:500px){.github-corner:hover .octo-arm{animation:none}.github-corner .octo-arm{animation:octocat-wave 560ms ease-in-out}}
```

## bulingbuling

&#160; &#160; &#160; &#160;现在保存后是不是就在你的页面左上角看到了优雅的GitHub链接图标了，如果想显示在右上角只需要将上述的CSS中`-webkit-transform: rotate(270deg);`的`270deg`改成`0deg`，这个是旋转角度的CSS，然后将`left: 0; `改成`right: 0; `。其实大家完全可以根据自己的需要对上述的代码进行修改，无论颜色大小还是图形，尽情的发挥吧。

&#160; &#160; &#160; &#160;喜欢的化评论一下让我看到你，这也是对我的前进的动力！！！
