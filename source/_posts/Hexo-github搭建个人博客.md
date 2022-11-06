---
title: Hexo+github搭建个人博客
date: 2022-10-31 22:24:53
tags: [hexo,BlueLake]
categories: 博客搭建
top: true
---

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。本文记录如何搭建Hexo，并使用比较干净整洁的BlueLake主题进行渲染。综合官网教程以及自己在安装过程写下了本教程，一步一步来就可以无痛安装。

<!--more-->

## 1. 安装 Hexo

> nodejs、git等安装，与github的配置略

所有必备的应用程序安装完成后，即可使用 npm 安装 Hexo。

```shell
npm install -g hexo-cli
```

安装以后，可以使用以下两种方式执行 Hexo：

1. `npx hexo <command>`
2. 将 Hexo 所在的目录下的 `node_modules` 添加到环境变量之中即可直接使用 `hexo <command>`：

```
echo 'PATH="$PATH:./node_modules/.bin"' >> ~/.profile
```

安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

```shell
 hexo init <folder>
 cd <folder>
 npm install
```

新建完成后，指定文件夹的目录如下：

```shell
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

上传到Github配置

```shell
deploy:
  type: git
  repo: https://github.com/huangjun0210/huangjun0210.github.io.git
  branch: gh-pages
```

生成本地页面

```shell
hexo s
```

安装hexo-deployer-git 自动部署发布工具

```shell
npm install hexo-deployer-git --save
```

生成静态页面

```shell
hexo g
```

本地文件上传到Github上面

```shell
hexo d
```

## 2. 安装BlueLake

### 2.1 安装主题

在根目录下打开终端窗口：

```
git bashgit clone https://github.com/chaooo/hexo-theme-BlueLake.git themes/bluelake
```

### 2.2 安装主题插件

在根目录下打开终端窗口：

```
git bashnpm install hexo-renderer-jade --save
npm install hexo-renderer-stylus --save
npm install hexo-generator-feed --save
npm install hexo-generator-sitemap --save
```

### 2.3 启用主题

打开`_config.yml`配置文件，找到theme字段，将其值改为`bluelake`(先确认主题文件夹名称是否为bluelake)。

```
theme: bluelake
```

### 2.4 验证

首先启动 Hexo 本地站点，并开启调试模式：

```
git bashhexo s --debug
```

在服务启动的过程，注意观察命令行输出是否有任何异常信息，如果你碰到问题，这些信息将帮助他人更好的定位错误。 当命令行输出中提示出：`INFO Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.`
此时即可使用浏览器访问 `http://localhost:4000`，检查站点是否正确运行。

### 2.5 更新主题

今后若主题添加了新功能正是您需要的，您可以直接`git pull`来更新主题。

```
git bashcd themes/bluelake
git pull
```

### 2.6 配置

#### 2.6.1  配置网站头部显示文字

打开`_config.yml`，找到：

```
title:     # 标题，如：秋过冬漫长
subtitle:  # 副标题，如：没有比脚更长的路，走过去，前面是个天！
description: # 网站关键字，如：key, key1, key2, key3
keywords:
author:
```

- `title`和`subtitle`分别是网站主标题和副标题，会显示在网站头部；
- `description`在网站界面不会显示，内容会加入网站源码的`meta`标签中，主要用于SEO；
- `keywords`在网站界面不会显示，内容会加入网站源码的`meta`标签中，主要用于SEO；
- `author`就填写网站所有者的名字，会在网站底部的`Copyright`处有所显示。

#### 2.6.2 设置语言

该主题目前有七种语言：简体中文（zh-CN），繁体中文（zh-TW），英语（en），法语（fr-FR），德语（de-DE），韩语 （ko）,西班牙语（es-ES）；例如选用简体中文，在`根_config.yml`配置如下：

```
language: zh-CN
```

#### 2.6.3 设置菜单

打开`主题_config.yml`，找到：

```
menu:
  - page: home
    directory: .
    icon: fa-home
  - page: archive
    directory: archives/
    icon: fa-archive
  # - page: about
  #   directory: about/
  #   icon: fa-user
  - page: rss
    directory: atom.xml
    icon: fa-rss
```

主题默认是展示四个菜单，即`主页home`，`归档archive`，`关于about`，`订阅RSS`；about需要手动添加，RSS需要安装插件，若您并不需要，可以直接注释掉。

#### 2.6.4 添加about页

此主题默认page页面是关于我页面的布局，在根目录下打开命令行窗口，生成一个关于我页面：

```
hexo new page 'about'
```

打开`主题_config.yml`，补全about页面的详细信息：

```
about:
  photo_url: # 头像的链接地址 e.g. http://cdn.itdn.top/blog/themeauthor.jpg
  items:
  - label: email # 个人邮箱
    url: ## Your email with mailto: e.g.  mailto:zhenggchaoo@gmail.com
    title: ## Your email e.g.  zhenggchaoo@gmail.com
  - label: github # github
    url: ## Your github'url e.g.  https://github.com/chaooo
    title: ## Your github'name e.g.  chaooo
  - label: weibo # 微博
    url: ## Your weibo's url e.g.  http://weibo.com/zhengchaooo
    title: ## Your weibo's name e.g.  秋过冬漫长
```

当然您也可以自定义重新布局about页面，只需要修改`layout/page.jade`模板就好。

#### 2.6.5 添加本地搜索

默认本地搜索是用原生JS写的，但还需要HEXO插件创建的JSON数据文件配合。安装插件[hexo-generator-json-content](https://github.com/alexbruno/hexo-generator-json-content)来创建JSON数据文件：

```
git bashnpm install hexo-generator-json-content@2.2.0 --save
```

然后在`根_config.yml`添加配置：

```
jsonContent:
  meta: false
  pages: false
  posts:
    title: true
    date: true
    path: true
    text: true
    raw: false
    content: false
    slug: false
    updated: false
    comments: false
    link: false
    permalink: false
    excerpt: false
    categories: false
    tags: true
```

最后在`主题_config.yml`添加配置：

```
local_search: true
```

#### 2.6.6 首页添加文章置顶

在根目录下打开命令行窗口安装：

```
npm uninstall hexo-generator-index --save
npm install hexo-generator-index-pin-top --save
```

然后在需要置顶的文章的Front-matter中加上top: true即可。

```
---
title: BlueLake博客主题的详细配置
tags: [hexo,BlueLake]
categories: Hexo博客
top: true
---
```

#### 2.6.7 撰写文章

```shell
hexo new post "新建博客文章名"
```



### 2.7 集成第三方服务

#### 2.7.1 添加评论

目前主题集成多种第三方评论，比如基于Github Issue的[GITALK](https://gitalk.github.io/)），[Valine](https://valine.js.org/)评论，[畅言评论](http://changyan.kuaizhan.com/)，[Disqus评论](https://disqus.com/)、[来必力评论](https://livere.com/)、[友言评论](http://www.uyan.cc/)、[网易云跟帖评论](https://gentie.163.com/info.html)等。

配置`主题_config.yml`：

```
# Gitalk comment
gitalk:
  enable: false
  owner: ## Your GitHub ID, e.g. username
  repo: ## The repository to store your comments, make sure you're the repo's owner, e.g. gitalk.github.io
  client_id: ## GitHub client ID, e.g. 75752dafe7907a897619
  client_secret: ## GitHub client secret, e.g. ec2fb9054972c891289640354993b662f4cccc50
  admin: ## Github repo owner and collaborators, only these guys can initialize github issues.
  language: 'zh-CN' ## Language
  pagerDirection: last # Comment sorting direction, available values are last and first.

# Valine comment. https://valine.js.org
valine:
  enable: false # if you want use valine,please set this value is true
  appId: # leancloud application app id
  appKey: # leancloud application app key
  notify: false # valine mail notify (true/false) https://github.com/xCss/Valine/wiki
  verify: false # valine verify code (true/false)
  pageSize: 10 # comment list page size
  avatar: mm # gravatar style https://valine.js.org/#/avatar
  lang: zh-cn # i18n: zh-cn/en
  placeholder: Just go go # valine comment input placeholder(like: Please leave your footprints )
  guest_info: nick,mail,link #valine comment header info

# Changyan comment. 畅言
changyan:
  enable: false
  appid: ## changyan appid
  appkey: ## changyan appkey

# Other comments
comment:
  disqus: ## disqus_shortname
  livere: ## 来必力(data-uid)
  uyan: ## 友言(uid)
  cloudTie: ## 网易云跟帖(productKey)
```

#### 2.7.2 站点统计

```
busuanzi: true  # 卜算子阅读次数统计
baidu_analytics: # 百度统计
google_analytics: # 谷歌统计
gauges_analytics: # Gauges
```

##### 2.7.2.1 卜算子阅读次数统计

若busuanzi设置为true将计算文章的阅读量，及网站的访问量与访客数，并显示在文章标题下和网站底部。

##### 2.7.2.2 百度统计

登录百度统计，定位到站点的代码获取页面。复制`//hm.baidu.com/hm.js?`后面那串统计脚本id(假设为：`8006843039519956000`)

```
baidu_analytics: 8006843039519956000
```

> 注意： baidu_analytics不是你的百度id或者百度统计id，如若使用其他统计，配置方法与百度统计类似。



参考链接：

[hexo官网]: https://hexo.io/zh-cn/
[BlueLake博客主题的详细配置]: https://chaooo.github.io/2016/12/29/bluelake.html

