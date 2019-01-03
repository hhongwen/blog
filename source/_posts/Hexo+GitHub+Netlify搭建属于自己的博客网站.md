---
title: 'Hexo+GitHub+Netlify搭建属于自己的博客网站'
url: 'build-own-blog'
date: 2018-12-16 19:00:00
tags: 博客搭建
categories: 博客
top: true
playlist:
  -
    name: Where are you
    artist: AniFace
    url: "//music.163.com/song/media/outer/url?id=453843751.mp3"
    cover: "//p1.music.126.net/AUfXMljLBgB3PBV731IzRg==/109951162857118370.jpg?param=130y"

---

> **“吾生也有涯，而知也无涯。”**

都说每个做技术的人都应该有一个属于自己的博客网站，但是总是因为种种事情半途而弃，借着刚刚搭建完博客的热情写一下是如何搭建此博客的，其实现在搭建博客很简单，这套博客就是采用 Hexo + Github + Netlify 搭建的静态博客网站。

<!--more-->

> **“吾生也有涯，而知也无涯。”**

都说每个做技术的人都应该有一个属于自己的博客网站，但是总是因为种种事情半途而弃，借着刚刚搭建完博客的热情写一下是如何搭建此博客的，其实现在搭建博客很简单，这套博客就是采用 Hexo + Github + Netlify 搭建的静态博客网站。

博客运行流程本地运行Hexo程序创建文章、程序修改等，将修改后的代码提交到GitHub，然后通过Netlify自动获取GitHub上的更新、部署、发布，这样就形成了一套自动更新部署发布的静态博客网站，下面让我一起来看一下是如何实现的。

## Hexo

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

## 安装

安装 Hexo 相当简单。然而在安装前，您必须检查电脑中是否已安装下列应用程序，这里默认你会使用Git，如果不会请看[廖雪峰老师的Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)：
- [Node.js](https://nodejs.org/en/)，安装请参考文章[Nodejs安装](http://www.runoob.com/nodejs/nodejs-install-setup.html)
- [Git](https://git-scm.com/)，安装请参考[Git安装](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137396287703354d8c6c01c904c7d9ff056ae23da865a000)

### 验证程序

Node.js验证：Windows系统打开cmd，然后输入`node -v`查看是否显示版本号，显示即表示安装成功，Mac和Linux则打开命令窗口同样验证即可。
Git验证：Windows系统在桌面右键看是否有Git bash here即可，打开后输入git验证，Mac和Linux则打开命令窗口输入git验证。

### 安装Hexo

如果您的电脑中已经安装上述必备程序，那么恭喜您！接下来只需要使用 npm 即可完成 Hexo 的安装。

``` bash
$ npm install -g hexo-cli
```

安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。`<folder>`就是你要将程序安装在所在文件夹的位置。

``` bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```

新建完成后，指定文件夹的目录如下：

``` folder
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

然后在指定的`<folder>`文件内执行`hexo server`命令，然后访问 `http://localhost:4000/` 即可访问本地已经部署好的Hexo网站。

``` bash
$ hexo server
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```

## Hexo配置及使用

### 配置

在指定的`<folder>`文件内找到`_config.yml`配置文件，Hexo的配置都在`_config.yml`文件内，大部分说明可以查看官网[Hexo配置](https://hexo.io/zh-cn/docs/configuration)，如下配置可根据需要自行更改。


#### 网站

|配置|说明|
|----|----|
|title|网站标题|
|subtitle|网站副标题|
|description|网站描述|
|author|你的名字|
|language|网站使用的语言，中文：zh-Hans|

其中，description主要用于SEO，告诉搜索引擎一个关于您站点的简单描述，通常建议在其中包含您网站的关键词。author参数用于主题显示文章的作者。

#### 网址

|参数|描述|默认值|
|----|----|----|
|url|网址||
|root|网站根目录||
|permalink|文章的 永久链接|格式	:year/:month/:day/:title/|

网站存放在子目录
如果您的网站存放在子目录中，例如 `http://yoursite.com/blog`，则请将您的 `url` 设为 `http://yoursite.com/blog` 并把 `root` 设为 `/blog/`。

#### 扩展

|配置|说明|
|----|----|
|theme|当前主题名称。值为false时禁用主题|
|deploy|部署部分的设置|

`theme`参数后期修改模板会用到，自己设置模板修改成对应参数。

### 使用

配置结束后让开始创建一片文章，使用命令`hexo new [layout] <title>`。如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout 参数代替。如果标题包含空格的话，请使用引号括起来。`hexo generate`命令在`<folder>/public`文件夹下生成静态文件，该命令可以简写为`hexo g`。发表草稿命令`hexo publish [layout] <filename>`。

``` bash
# 创建文件
hexo new HelloHexo
# 生成静态文件
$ hexo generate
# 发布文件
$ hexo publish HelloHexo
# 启动服务
$ hexo server
```

启动服务后即可在`<folder>/source/_posts`文件下看到生成的`HelloHexo.md`文件，可以修改后即可刷新`http://localhost:4000/`查看最新内容，更多使用参考[官网标签使用](https://hexo.io/zh-cn/docs/commands)。

## 上传GitHub

在托管给GitHub之前先做一些准备工作，在`<folder>`文件下执行`git init`对git进行初始化，将对不需要上传的文件放入`.gitignore`文件中，使用命令`echo "/public" >> .gitignore`屏蔽public文件夹，然后提交所有文件。

``` bash
# git 初始化
git init
# 屏蔽 public文件夹
echo "/public" >> .gitignore
# 添加到仓库
git add .
# 提交文件
git commit -m "install hexo project"
```

GitHub上创建远程仓库，选项都是默认即可。然后将`<folder>`文件夹与远程仓库绑定，绑定后提交文件到远程仓库。

``` bash
# 绑定远程仓库，<your repo url>是指你远程仓库的ssh地址或者https地址
git remote add origin <your repo url>
# 提交到远程仓库
git push origin master
```

## Netlify托管

什么是Netlify？Netlify是一家国外的静态网站的托管平台，提供免费的https，自动化部署和升级，可以监控GitHub、GitLab或者Bitbucket做到自动更新发布，是不是很赞！这也是为什么现在选择Netlify的原因，至于以后Netlify怎么发展只能再议。

走到现在就差最后一步，是不是有点小激动，接下来让我们[Netlify](https://www.netlify.com/)`https://www.netlify.com/`的官网，然后点击右上角Sign up注册账号，选择GitHub关联登录。

![Netlify Sign up](/images/netlify_signup.png)

注册后点击创建新站点，选择GitHub会有提示框进行认证。

![Create a new site](/images/create_new_site.png)

选择上传到远程仓库的hexo项目文件，然后进行关联。

![continue paoject](/images/continue_project.png)

最后发布项目，这个时候点击deploy site按键，万事具备可以嗑会瓜子喝点茶水然后回来看一下应该就部署成功了。

![deploy site](/images/deploy_site.png)

部署成功后即可生成一个`https://xxx.netlify.com`带有netlify的域名，这时候访问域名就可以看到一个属于自己的博客网站了，是不是激动的跳了起来。随机生成的二级域名是一串随机的字母数字，如果不喜欢可以自己设置二级域名或者选择配置自己拥有的域名。

## 修改域名

### 修改随机生成域名

下面教你怎么修改`https://xxx.netlify.com`域名为你想要的免费域名，点击`setting`然后点击`change site name`初见弹出框，即可将`xxx`修改成想象中的名称。是不是很完美，没有问题就可以开始自己的博客之旅了！如果还想追求完美那么接着往下看。

![change site name](/images/change_site_name.png)

### 修改为自己的域名

很多朋友不想用带有netlify名称的域名，所以也可以像域名改为自己购买的域名，首先你要有一个域名并且已经备案，然后查看就可以进行下面操作了。

#### 域名解析

首先对域名进行解析，添加两条解析记录，一条A记录，一条CNAME记录。A记录的记录值IP是你`https://xxx.netlify.com`域名对应的ip，CNAME记录的记录值是`https://xxx.netlify.com`的`xxx.netlify.com`值，添加完解析后就可将你的域名绑定到Netlify了。

![domain deal](/images/domain_deal.png)

## 绑定域名

点击第二步：

![custom domain](/images/custom_domain.png)

输入域名后验证：

![add domain](/images/add_domain.png)

添加成功后即可用自身域名进行访问：

![domain_success_own](/images/domain_success_own.png)

是不是很完美了，这个时候访问你的域名即可访问到属于自己的博客网站，恨不得现在就让全宇宙之后你的博客已经建成！下面还有一步就是添加https。

## 添加HTTPS

`Netlify` 使用的是 `Let’s Encrypt Certificate.`的免费证书，自行按着步骤添加即可，我这里因为域名都是腾讯注册的，所以也附赠了ssl，不用白不用，现在教你如何添加自己的ssl证书。

首先下载自己的证书，会看到证书里有一个Apache的文件夹，里面有三个文件，结构如下：

``` folder
.
├── 1_root_bundle.crt
├── 2_hhwen.cn.crt
└── 3_hhwen.cn.key
```

分别打开三个文件填入复制到下面的内容框中保存即可：

![add sll](/images/add_ssl.png)

## 模板修改

完美的博客之旅到这里是不是就是结束了，每一个拥有自己博客的程序猿都想有一个与众不同的页面风格，这里可以参考[官网的主题](https://hexo.io/themes/)进行修改，本博客使用的是[GODBMW博主的BMW主题](https://godbmw.com/passages/2018-11-15-theme-bmw-docs-zh/)主题，喜欢的也可以关注，这里就由你们自己发挥我就不多介绍了。
修改主题后记得去`_config.yml`中修改`themes`参数！


## 总结

到这里已经结束了搭建博客之旅，但是我知道你还在兴奋当中，如果喜欢请收藏我的博客，写作当真不易比我搭建博客还要累。

这算是搭建此博客写的第一篇博客，写博客需要静下来把做过的事情再捋一遍一步一步进行拆解，也让自己更加深刻，也希望每一个搭建了自己博客朋友善待自己的博客不要冷落。

如果大家有疑问可以留言评论，感谢支持。

**打卡：2018-12-17 01:33**
