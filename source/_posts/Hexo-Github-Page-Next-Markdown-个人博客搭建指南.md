---
title: Hexo + Github Page + Next + Markdown 个人博客搭建指南
abbrlink: c5f6c3bd
date: 2024-03-30 17:45:17
tags: [Hexo, Blog]
categories: [Other]
---

来折腾一个自己的博客吧！

<!-- more -->

# 环境配置

安装版本控制工具 [Git](https://git-scm.com/downloads) 和 [Nodejs](https://nodejs.org/en/download) （包括包管理软件 `npm`）。

> **Git** 是一个开源的分布式版本控制系统，旨在处理从小到大的项目与速度和效率。Git 的核心理念是，让代码版本控制变得简单且高效，特别是支持非线性开发过程。在 Git 中，每个工作副本实际上都是一个带有完整历史和完整版本跟踪能力的仓库，不依赖网络访问或中央服务器。
>
> **Node.js** 是一个基于 Chrome 引擎的 JavaScript 运行环境。**npm**（Node Package Manager）是一个为 JavaScript 提供的包管理和分发工具，主要用于管理 Node.js 模块的依赖关系。NPM 让开发者能够安装、分享、分发代码，以及管理项目中的依赖关系。它是 Node.js 默认的包管理工具，随 Node.js 安装包一起安装。

使用以下指令检查是否安装成功：

```
yangkai@YangKai:~/blog/hexo$ git --version && node --version && npm --version
git version 2.34.1
v20.12.0
10.5.0
```

在这之后通过 `npm` 安装 `hexo-cli`（如果安装过程提示 `permission denied` 就加上 `sudo`）：

```
npm install -g hexo-cli
```
在你喜欢的目录（本文用 `/hexo` 目录代替）下执行以下指令：

```
mkdir blog && cd blog
mkdir hexo && cd hexo
hexo init
```

以上，我们完成了基础环境的搭建。如果想要知道当前环境下安装了哪些包，只需要在 `/hexo` 目录下执行指令：

```
npm list --depth=0
```

# 更换 Next 主题

安装 `Next` 主题包，为了方便自定义，我们直接克隆原始项目：

```
git clone https://github.com/next-theme/hexo-theme-next themes/next
```

并更改：`/hexo/_config.yml` 文件

```yaml
theme: next
```

使用如下命令生成静态网页文件并开启本地端口：

```
hexo clean && hexo g && hexo s
```

在浏览器输入 `http://localhost:4000/` 以浏览网页。

# 添加 Markdown 数学公式支持

## 背景简介

> 在使用 Hexo 搭建博客时，选择合适的 Markdown 数学公式渲染器对于展示数学、物理或工程类文章中的公式至关重要。不同的渲染器在渲染效果、兼容性和配置难度等方面各有特点。
>
> **MathJax**: MathJax 是一个流行的、功能强大的数学公式渲染库，支持 LaTeX、MathML 和 AsciiMath 标记。它在质量和兼容性方面表现优异，几乎可以在所有浏览器中无缝渲染数学公式。由于其渲染过程是在浏览器端完成的，可能会稍微增加页面加载时间。配置相对简单，大部分 Hexo 主题支持或容易集成 MathJax。
>
> **KaTeX**: KaTeX 以其渲染速度快闻名，它由 Khan Academy 开发。对于需要快速渲染大量公式的页面，KaTeX 是一个很好的选择。尽管 KaTeX 的速度比 MathJax 快，但它不支持 LaTeX 的全部命令和包。如果你的公式非常复杂，可能需要检查 KaTeX 是否支持你需要的功能。
>
> **Pandoc**: Pandoc 不是一个专门的数学公式渲染器，而是一个“万能文档转换器”，支持将 Markdown 文档转换成各种格式，包括支持数学公式的格式。Pandoc 的使用相对复杂，需要对命令行有一定了解。它的优势在于能够支持多种文档格式的转换，非常适合需要将博客内容发布到多个平台的用户。

Hexo 支持的 Markdown 渲染器通常来说有以下几种：

* [hexo-renderer-marked](https://github.com/hexojs/hexo-renderer-marked) (default)
  * 支持的渲染格式有限，而且容易发生冲突。可以通过  `MathJax` 支持 ` $$ ... $$` 的识别，但**不支持**识别 `$ ... $`。
* [hexo-renderer-markdown-it](https://github.com/hexojs/hexo-renderer-markdown-it)
  * 比 `marked` 更快而且仍在频繁维护中，但配置起来相对麻烦。
* [hexo-renderer-markdown-it-plus](https://github.com/CHENXCHEN/hexo-renderer-markdown-it-plus)
  * 与`markdown-it ` 相比简单很多，而且其功能经过测试也是比较稳定的，因此我们选这个包。
* [hexo-renderer-pandoc](https://github.com/hexojs/hexo-renderer-pandoc)
  * 需要引入外部程序，这就很烦，不符合我小而美的博客美学，因此被我主观忽略掉了，后续如果有需要可以再研究研究分享一下。
* [hexo-renderer-kramed](https://github.com/sun11/hexo-renderer-kramed)
  * 是一个可行的解决方案，但其项目自 2016 年开始就没有再更新过，为了博客的长时间稳定，我**不推荐**使用这种方案，如果希望图省事使用 ```kramed```，网上还有很多其他教程讲解了如何进行配置。

## 更换渲染引擎

```
npm un hexo-renderer-marked --save
npm i hexo-renderer-markdown-it-plus --save
```

在 `/hexo/_config.yml` 中添加如下内容：

```yaml
markdown_it_plus:
  highlight: true
  html: true
  xhtmlOut: true
  breaks: true
  langPrefix:
  linkify: true
  typographer:
  quotes: “”‘’
  pre_class: highlight
```

在 `/hexo/node_modules/hexo-theme-next/_config.yml` 更改如下设置：

```yaml
math:
  every_page: true
  mathjax:
    enable: false
    tags: none
  katex:
    enable: true
    copy_tex: true
```

创建一个文件简单测试一下效果：

```
hexo new post "Markdown Render Example"
```

并在 `/hexo/source/_posts` 之中的对应文章中添加如下内容：

```text
Inline math: $e^{i\pi} + 1 = 0$, and block math:
$$
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
$$
```

期望效果如下：

![0](/Hexo-Github-Page-Next-Markdown-个人博客搭建指南/markdown-render-exmaple.png)

# 部署到 Github Page

## 创建并关联 Github 仓库

在 Github 上创建一个新的仓库，仓库名为 `username.github.io`，其中 `username` 是你的 Github 用户名。在本地执行以下指令：

```
git init
git remote add origin /your-repo-url
```

## 配置 Github Actions

在 Github 仓库的 `Settings` 页面中找到 `Pages` 部分，选择 `Github Actions` 作为源。

在 `/hexo/.github/workflows` 目录下创建一个 `pages.yml` 文件，内容如下，并将示例中的 `20` 改为你的 Node.js 版本（通过`node -v`查看）：

```yaml
name: Pages

on:
  push:
    branches:
      - main # default branch

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          # If your repository depends on submodule, please see: https://github.com/actions/checkout
          submodules: recursive
      - name: Use Node.js 20.x
        uses: actions/setup-node@v2
        with:
          node-version: '20'
      - name: Cache NPM dependencies
        uses: actions/cache@v2
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./public
  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```
## 取消主题文件夹的子模块属性

我们直接通过克隆获得了主题模块，这导致其被识别为 `submodule`，我们可以如下操作：

```
cd themes/next
rm -rf .git
```

## 部署到 Github

```
git add .
git commit -m "The Message You Want"
git push -u origin main
```

# 图片管理以及显示 BUG 修复方案

## 图片管理

可以在 `/hexo/_config.yml` 中添加如下配置：

```yaml
post_asset_folder: true
```

这样在每次新建文章时，会自动在 `/hexo/source/_posts` 目录下创建一个与文章同名的文件夹，用于存放文章中的图片。

## 图片无法显示 BUG

在使用的过程中如果遇见图片无法显示的问题，我们可以通过 `F12` 看一下加载错误的图片究竟发送了一个什么请求，看看它和 `/hexo/public` 文件夹中的图片相对路径到底一不一样。

我的问题表现为：实际上图片的路径为 `/:year/:month/:day/:title/xxx.png`，但是请求的路径为 `:title/xxx.png`，这就导致了图片无法显示。

这给我们提供了一种非常邪道的解决方案，具体操作是修改 `/hexo/_config.yml` 文件：

```yaml
# permalink: :year/:month/:day/:title/
permalink: :title/
```

这会强行同步所有文章和图片的永久链接使文章中的图片能够被正确访问。

# 总结

提供最终包版本列表如下：
```
yangkai@YangKai:~/blog/hexo$ npm list
hexo-site@0.0.0 /home/yangkai/blog/hexo
├── hexo-generator-archive@2.0.0
├── hexo-generator-category@2.0.0
├── hexo-generator-index@3.0.0
├── hexo-generator-tag@2.0.0
├── hexo-renderer-ejs@2.0.0
├── hexo-renderer-markdown-it-plus@1.0.6
├── hexo-renderer-stylus@2.1.0
├── hexo-server@3.0.0
└── hexo@6.3.0
```

# 后续折腾

## 暴打图片显示 BUG 邪道

直接从 github 安装 archive 版本的 `hexo-asset-image`（千万不要直接`npm install hexo-asset-image` 下载，我折腾了半天，有大恐怖，会全身红毛晚年不详）。

```
npm install https://github.com/xcodebuild/hexo-asset-image
```

这样不管怎么改 `permalink` 都没事啦。

## 增添文章的独特链接并解决兼容问题

安装 `hexo-abbrlink` 插件：

```
npm install hexo-abbrlink --save
```

修改/增添配置`/hexo/_config.yml`如下：

```
permalink: post/:abbrlink.html
abbrlink:
  alg: crc32
  rep: hex
```

丝毫不出人意料的是图片又裂开了。观察到如下日志：

```
update link as:-->/post/c5f6c3bd.htm/markdown-render-exmaple.png
```

我们设置的 `permalink` 为 `post/:abbrlink.html`，但是图片的链接却变成了 `/post/c5f6c3bd.htm/*`，这就导致了图片无法显示。

我们的 `l` 被删掉了，但是 `.htm` 却留下来了。不难看出这是因为 `hexo-asset-image` 插件采用硬编码计算路径导致的。我们可以通过修改 `/hexo/node_modules/hexo-asset-image/index.js` 文件解决这个问题。

第 24 行的：

```javascript
      var endPos = link.length-1;
```

改为：

```javascript
      var endPos = link.length-5;
```

新的日志变为：

```
update link as:-->/post/c5f6c3bd/markdown-render-exmaple.png
```

计划通。

再次生成静态页面，发现图片显示回归正常。

## 让 Google 能够搜索到自己的博客

在 Google 搜索中输入 `site:your-blog-url`，如果没有搜索到自己的博客，那么就需要添加站点地图。

[Google Search Console](https://search.google.com/search-console/welcome)

进入这个站点，将包含`https://`的博客地址添加到右边的输入框中，然后选择其他验证方法-HTML标记，复制其中的 `context="XXX"` 中的 `XXX`，填入到 `themes/next/_config.yml`

```yaml
google_site_verification: XXX
```

提交验证即可成功，接下来提供站点地图，安装一个包：

```bash
npm install hexo-generator-sitemap --save
```

使用如下命令重新生成静态页面后可以在 `/hexo/public` 目录下找到 `sitemap.xml` 文件：

```bash
hexo clean && hexo g
```

提交到 Github 后，再次进入 Google Search Console，点击站点地图，输入 `sitemap.xml`，点击提交即可。

等待一段时间之后你的站点就可以在 Google 中搜索到了。