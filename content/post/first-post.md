---
title: "切换到hugo来构建"
date: 2019-02-14T10:58:17+08:00
---

生活在于折腾，用[Hugo](https://gohugo.io/)来弄一弄博客。

<!--more-->
最早在`github pages`上搭建博客用的是Octopress，后来切换到Hexo，现在换到了Hugo。其实工具都是差不多的，博客最重要的东西是内容，谁让我就是喜欢折腾呢，下面就简单介绍下怎么搭建吧。

> 首先得准备一个github账号，我们发布的内容都是挂在这上面的，本文的操作都是基于Mac环境

#### 安装Hugo

```js
brew install hugo
//就是这么简单
```

#### 创建目录

```js
hugo new site myblog
//创建一个网站
```

这样就生成了一个网站的工作目录

```js
├── archetypes //默认模板
├── config.toml	//配置文件
├── content	//MD格式的文章
├── data	//一些数据文件可以忽略
├── layouts	//自定义的一些页面用了主题之后可以忽略
├── static	//存放资源文件发布之后该目录得资源处于网页根目录
└── themes	//主题文件
```

#### 开始写文章

```js
hugo new post/first-post.md
```

在content目录中会自动以archetypes/default.md为模板在content/posts目录下生成一篇名为first-post.md的文章草稿，打开md文件加上`Hello Hugo`：

```js
---
title: "First Post"
date: 2019-02-15T16:36:13+08:00
draft: true
---
### Hello Hugo
```

#### 安装主题

在myblog目录里执行一下代码，我选用得主题是[hugo-nuo](https://github.com/laozhu/hugo-nuo)

```js
cd themes
git clone https://github.com/laozhu/hugo-nuo
```

下载完毕之后我们打开myblog的config.toml文件增加一行说明

```js
baseURL = "http://example.org/"
languageCode = "en-us"
title = "My New Hugo Site"
theme = "hugo-nuo"	//主题引用hugo-nuo
```

#### 预览文章

```js
hugo server -D	//启动服务 -D标识可以预览草稿
```

没有报错的话可以在命令行看到打开`http://localhost:1313/`预览

![预览](https://raw.githubusercontent.com/qcliwei/picbed/master/hugo/jcyl.jpg)

这样本地的工作就完成了，接着我们要准备发布到github pages工作了

Hugo有2种方式发布到github pages，我采用docs目录推送到master branch这种，感觉更方便点

先在config.toml配置里面增加一个说明，生成doc目录，方便我们push到github，这个目录包含了网站所有内容

```js
publishDir = "docs"
```

接着在myblog目录执行命令

```js
hugo
```

我们可以看到在myblog目录多了一个docs目录，里面存放了所有静态文件

```js
├── 404.html
├── categories
├── favicon.ico
├── icons
├── images
├── index.html
├── index.xml
├── manifest.json
├── page
├── robots.txt
├── scripts
├── service-worker.js
├── sitemap.xml
├── styles
└── tags
```

#### 发布到Github

在github上创建一个仓库，把docs目录关联到远程仓库，将所有文档push到Github的`master branch`，进入Github对应repository的Settings标签菜单，在`GitHub Pages`选项的Source栏选择`master branch /docs folder`

![githubpages](https://raw.githubusercontent.com/qcliwei/picbed/master/hugo/jcgh.jpg)

等待片刻即可访问`http://your_name.github.io`看到之前用`Hugo`生成的网页了。
#### 配置个人域名

`github pages`里面生成的默认域名是yourname.github.io，这里我们可以绑定自己的域名，首先进入自己域名的服务商，配置域名解析规则，我的域名[qcliwei](https://qcliwei.com)是在万网上买的，在万网的管理后台增加2条记录

![域名配置](https://raw.githubusercontent.com/qcliwei/picbed/master/hugo/jcym.jpg)

其中ip地址直接去ping yourname.github.io就可以得到

最后在static目录里面增加一个CNAME文件里面写上你绑定的域名就可以了，用hugo命令生成docs的时候，会直接复制到/docs的根目录

等10分钟左右域名解析生效之后就可以用自己的域名访问了

#### 使用HTTPS

由于`Github Pages`不支持在自定义域名中使用`HTTPS`协议，所以浏览器中访问自己的域名使用的是`HTTP`协议。浏览器都提示该网页不受信任，如果网页中还有待加载的`JavaScript`代码,就得单独点浏览器地址栏右侧的`load`按钮才能正常加载全部页面，非常麻烦。

好在[CloudFlare](https://www.cloudflare.com/)为我们提供了一套免费的解决方案，首先我们去该网站注册帐号，输入我们的域名，选择一个免费的套餐，此时`CloudFlare`会给我们提供其DNS服务器的IP，此时需要去万网的域名管理页面中更新默认DNS服务商到CloudFlare

![DNS服务器](https://raw.githubusercontent.com/qcliwei/picbed/master/hugo/jcdns.jpg)

更新好DNS服务器之后，继续在CloudFlare中添加`DNS Records`

![DNS Records](https://raw.githubusercontent.com/qcliwei/picbed/master/hugo/jcrecords.jpg)

点击**Crypto**选项，使用HTTPS加载网页

![](https://raw.githubusercontent.com/qcliwei/picbed/master/hugo/jchttps.jpg)

在下面**Always Use HTTPS** 的选项卡中选择on

最后设置**Page Rules**

![](https://raw.githubusercontent.com/qcliwei/picbed/master/hugo/jcrules.jpg)

到这里就大功告成了，等待几个小时，打开自己的域名就发现上HTTPS了，网页也可以正常显示了