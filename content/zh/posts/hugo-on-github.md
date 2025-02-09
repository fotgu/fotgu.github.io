---
date: '2025-02-07'
draft: false
title: 'Hugo on GitHub'
description: "探索 Hugo 生态的奇妙历险"
summary: "山重水复疑无路，柳暗花明又一村"
tags: ['博客建站', '白嫖']
---

> 安全声明：文章中可能存在错误或过时的信息，请在辨别之后再决定是否采纳。

我最近在学习不少感兴趣的东西，希望找个地方记录学习过程和心得。。

由于国内平台的审核都比较严格，为了避免把时间浪费在对抗审核上，我决定自己搭建博客。

经过调研，我选择 Hugo 作为建站工具，托管在 GitHub Pages 上。

整个建站的过程持续了好几天，值得记录一下。

## 给要借鉴的同学

如果你想在本地运行我的博客，可以参照 [本地运行](#本地运行)。

如果你甚至要参照我的博客做部署，可以参考 [部署到GitHub](#部署到github)。

在遇到问题时，可以先试着向成熟的大模型求助，比如 [deepseek](https://chat.deepseek.com/)。

如果大模型无法解决你的问题，欢迎在文章末尾评论反馈。

### 本地运行

#### 前置知识

- 使用浏览器打开网址
- 在电脑上安装软件
- 了解自己电脑所运行的操作系统
- 使用命令行工具执行命令
- 使用 git 克隆仓库
- 阅读简单的英文文章，或者有一个牛逼的翻译工具

#### 运行步骤

> **注意**，以下方法已在 macOS 上验证，大概率也适用于 Linux，但在 Windows 上可能无法运行。 \
> **注意**，评论功能与仓库绑定，这里提供的方法不包括评论部分。请参考 [如果需要评论功能](#如果需要评论功能) 来增加评论功能。

**注意**: 把下面所有步骤中的 username 替换成你的 GitHub 用户名

```shell
# 1. 克隆代码仓库
cd /path # 使用命令行进入一个可以放代码的地方
git clone https://github.com/fotgu/fotgu.github.io.git # 克隆代码到本地

# 2. 安装依赖

## 2.1 安装 git
## 参考 https://git-scm.com/book/en/v2/Getting-Started-Installing-Git

## 2.2 安装 hugo v0.143.1
## 参考 https://gohugo.io/installation/

## 2.3 安装 npm
## 参考 https://docs.npmjs.com/downloading-and-installing-node-js-and-npm

## 2.4 安装 Pagefind 1.3.0 版
cd /path/fotgu/fotgu.github.io  # 进入克隆的代码的根目录
npm install pagefind@1.3.0

# 3. 本地启动博客

hugo && npx pagefind && hugo server

# 4. 使用浏览器打开博客网页，网址一般是 http://localhost:1313/
```

### 部署到GitHub

#### 部署前置知识

- 完成 [本地运行](#本地运行)
- GitHub 基础使用
- 使用 git 推送代码到 GitHub

#### 部署步骤

**注意**: 把下面所有步骤中的 username 替换成你的 GitHub 用户名

```shell
# 1. 如果你还没有 GitHub 账号，创建一个
## 创建地址 https://github.com/signup

# 2. 在 GitHub 上创建名为 username.github.io 项目
## 创建地址 https://github.com/new

# 3. 启用从 GitHub Actions 部署
## 打开刚才创建的仓库的 pages 设置页面
## 设置页面地址 https://github.com/username/username.github.io/settings/pages
## 把部署方式(Build and deployment)从 "Deploy from a branch" 改成 "GitHub Actions"

# 4. 把 /path/fotgu/fotgu.github.io 中的代码推到刚刚创建的仓库
cd /path # 进入存放代码的目录
git clone https://github.com/username/username.github.io.git # 克隆仓库到本地
cp -r /path/fotgu/fotgu.github.io/* /path/username/username.github.io/ # 复制文件到你的仓库
git add . # 将所有更改添加到 Git 暂存区
git commit -m "Initial commit" # 提交更改并添加提交信息
git push # 将本地更改推送到 GitHub 仓库。如果遇到认证问题，请参考 GitHub 的文档配置 SSH 密钥或个人访问令牌

# 5. 等待部署完成
## 部署进度页面地址 https://github.com/username/username.github.io/actions

# 6. 打开博客网页
## 博客地址 username.github.io

```

#### 如果需要评论功能

1. 在仓库中开启 discussion 功能
2. 在仓库中安装 giscus app
3. 获取仓库id、分类id
> 在 giscus 的官网 <https://giscus.app/zh-CN> \
> 填写仓库名后，它会检查前面的配置是否成功，并在页面下方的"启用 giscus"处提供 仓库id \
> 选择分类为 Announcements，它会在页面下方的"启用 giscus"处提供 分类id
4. 本地启动博客，观察效果
```shell
hugo --ignoreCache --cleanDestinationDir && \
npx pagefind && \
HUGOxPARAMSxCOMMENTS=true \
HUGOxPARAMSxREPO_NAME=你的仓库名 \
HUGOxPARAMSxREPO_ID=你的仓库id \
HUGOxPARAMSxCATEGORY_NAME=Announcements \
HUGOxPARAMSxCATEGORY_ID=你的分类id \
hugo server
```
5. 替换 GitHub Pages 的配置
```yaml
# .github/workflows/hugo.yaml
          HUGOxPARAMSxREPO_NAME: 你的仓库名
          HUGOxPARAMSxREPO_ID: 你的仓库id
          HUGOxPARAMSxCATEGORY_NAME: Announcements
          HUGOxPARAMSxCATEGORY_ID: 你的分类id
```

## 选型说明

### 为什么是 hugo

博客这个场景一般只需要静态内容，在开源的静态内容建站工具中，名气比较大的有：

| 工具   | 地址                    | 编写语言            | 优点           |
| :----- | :---------------------- | :------------------ | :------------- |
| Jekyll | <https://jekyllrb.com/> | Ruby                | GitHub原生支持 |
| Hugo   | <https://gohugo.io/>    | Go                  | 快             |
| Hexo   | <https://hexo.io/>      | JavaScript(Node.js) | 生态丰富       |

这些工具不仅都支持使用 markdown 编写博客，而且都支持自定义网页模板。

根据我这几年的开发经验，在使用现成模板的时候，少不了自己折腾的步骤，而模板一般是用奇形怪状的 dsl(Domain Specific Language) 来写。

为了节省精力，最好选择一个已经熟悉的 dsl。

由于我对 Hugo 使用的 go template 有一点了解，同时我不太了解 Ruby 和 Node.js，我最终选择了 Hugo。

### 为什么是 GitHub Pages

在 2025 年，已经有很多成熟且好用的静态内容托管平台。

但是短期来看，GitHub Pages 还是最稳定的，毕竟背靠微软，财大气粗。

最重要的是，它能跟 GitHub Actions 无缝集成，还完全免费。

作为一个用户，我最终选择了白嫖。

## 建站

### 本地跑通基础博客

我的习惯是在本地做好验证后，再做部署。

先找了 Hugo 的入门文档 <https://gohugo.io/getting-started/quick-start/>，一顿操作后，博客在本地跑起来了。

很开心。

### 挑选博客主题

#### 为什么是 PaperMod

在 [Hugo 主题网站](https://themes.gohugo.io/) 看了一圈，最终选择了 [PaperMod](https://themes.gohugo.io/themes/hugo-papermod/)。

看到示例图的第一眼，我就喜欢上了这个主题，我比较喜欢简洁的风格，而 PaperMod 的风格如其名，就像是纸质网页一般简洁。

PaperMod 自带搜索和评论，还支持切换搜索组件和评论组件，这一点非常重要，让我后续白嫖评论功能成为可能。

PaperMod 还提供了 extend_footer.html 的扩展点，我利用这个扩展点添加了 mermaid 代码块的渲染功能。 \
(参考 <https://gohugo.io/content-management/diagrams/#mermaid-diagrams>)

#### 安装主题的方式

在安装主题时发现一个小问题，Hugo 入门文档可能是出于兼容性的考虑，还在使用 git submodule 导入主题/theme，这种方式有点重，我不太喜欢。

我更喜欢使用 [hugo module](https://gohugo.io/hugo-modules/use-modules/) 引入，这样就只需要初始化一次模块，后面就只需要在配置文件中切换主题，像在编程语言中管理依赖一样，比较方便。

初始化命令:
```shell
hugo mod init github.com/<your_user>/<your_project>
# 如果这一步报错，可能要考虑是否环境中缺少 go
# 因为 hugo module 强依赖 go 的 module 功能，但有些安装方式不会自动安装 go
```

在配置文件中指定主题:
```yaml
# 在 hugo.yml 中
theme:
  - github.com/adityatelange/hugo-PaperMod
```

### 添加搜索功能

#### 为什么是 Pagefind

[Pagefind](https://pagefind.app/) 是全静态的搜索库，无需部署，搜索时的带宽消耗也低。

为了理解 Pagefind 的特性，需要先理解搜索这个常见的功能：

- 搜索由两个阶段组成：
  - 构建索引：索引可以理解为 关键词 到 内容的映射。这个过程一般在内容生产完成后立刻进行，是内容生产侧的处理，用户感知不到
  > 最暴力的索引实现是，直接使用原始内容作为索引，这种做法只在搜索少量内容时可用，内容量增多后，就会慢到难以接受
  - 匹配：使用用户的输入，在索引中寻找匹配的内容。这个过程是用户直接感知的搜索过程

##### 动态 vs 静态

根据匹配过程发生的位置，可以把搜索组件分成 动态和静态 两种：

- 静态搜索库：在浏览器上完成匹配。用户在搜索框输入后，浏览器下载索引，并在浏览器内部完成分析输入、匹配输入与内容的过程，找到相关内容后，直接显示
- 动态搜索库：在服务器上完成匹配。用户在搜索框输入后，浏览器把用户的输入传递到服务器，在服务器上完成分析输入、匹配输入与内容，找到相关内容后，服务器把内容回传给浏览器，浏览器再显示内容

可以看到，动态搜索库需要我们在服务器上部署搜索服务、或者购买供应商提供的搜索服务，不仅要花钱，还凭空多了一个依赖，造成搜索能力的不稳定。

而静态搜索库，则只依赖网站托管平台，静态搜索库胜。

> 静态搜索库也有很大的局限，比如承载的内容量级比较小、索引要暴露在公网上，但是在个人博客这个场景，这些都不是问题

##### 带宽消耗与耗时

Hugo 生态中，常见的静态搜索方案会把所有索引放到一个索引文件，这样在下载时会有较大的开销。
> 在文章数量少的时候，问题不大

而 Pagefind 则把索引分块放到了不同的文件中，在执行匹配前按需下载；减少了搜索过程中的带宽消耗，同时也降低了搜索的耗时。

按照 Pagefind 的说法，在对 10000 个网页做全局搜索时，只需要消耗 300 kB；文章数量少的情况下，只需要消耗不到 100 kB。

##### 丰富的功能

Pagefind 有很多功能，我已经用了构建索引时过滤内容的功能。
后面可能会用到 [定制搜索框](https://pagefind.app/docs/ui/#translations)。

#### 集成方式

##### 生成索引

生成索引很简单，只需要执行：

```shell
npx pagefind --site public
```

如果有定制需求，可以参考 <https://pagefind.app/docs/config-options/>

##### 集成到 PaperMod 中

PaperMod 的示例中，点击菜单里的 Search 按钮会跳转到搜索页面。这个方案虽然糙，但是意外地好用，我也采用这个方案。

步骤如下：

1. 添加一个搜索模板，模板中使用 Pagefind 提供的前端资源:
```html
# layouts/_default/search.html
<link href="/pagefind/pagefind-ui.css" rel="stylesheet">
<script src="/pagefind/pagefind-ui.js"></script>
<div id="search"></div>
<script>
    window.addEventListener('DOMContentLoaded', (event) => {
        new PagefindUI({ element: "#search", showSubResults: true });
    });
</script>
```
2. 在 Hugo 的 content 目录中添加搜索页面，并使用上面的搜索模板:
```markdown
# content/search.md
---
title: "搜索"
layout: "search"
---
```

### 添加评论功能

#### 为什么是 giscus

评论功能需要存储数据。根据存储的实现方式，Hugo 生态中的评论组件可以分为以下四种：

- 使用 GitHub issue 实现
- 使用 GitHub discussion 实现
- 使用其他存储的开源实现
- 使用 saas 服务实现

评论跟 discussion 很接近，我倾向于使用 GitHub discussion 实现。

由于评论功能需要服务器处理身份验证，为了简化流程，最好选择免费且支持自托管的服务。

使用 GitHub discussion 实现 + 免费服务 + 支持自托管，这三个条件指向了 giscus。

使用的前置条件参考 [如果需要评论功能](#如果需要评论功能)。

#### 集成到 PaperMod 中

1. 开启 PaperMod 的评论
```yaml
# hugo.yml
params:
  comments: true
```
2. 配置 giscus
参考 [如果需要评论功能](#如果需要评论功能)
3. 集成到 PaperMod
```html
# layouts/partials/comments.html
# 使用变量配置仓库和分类信息，方便管理配置
<script src="https://giscus.app/client.js"
        data-repo={{ .Site.Params.repo_name }}
        data-repo-id={{ .Site.Params.repo_id }}
        data-category={{ .Site.Params.category_name }}
        data-category-id={{ .Site.Params.category_id }}
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="top"
        data-theme="preferred_color_scheme"
        data-lang="zh-CN"
        data-loading="lazy"
        crossorigin="anonymous"
        async>
</script>
```
4. 使用 hugo.yml 配置 仓库和分类信息

### 使用 GitHub Actions 自动构建和部署

参考 <https://gohugo.io/hosting-and-deployment/hosting-on-github/#procedure>。 \
我删除了安装 Dart Sass 的步骤，因为暂时还不需要，而且这一步比较耗时； \
我还添加了使用 Pagefind 生成索引的步骤，其余部分保持不变。

## 总结

整体的体验是轻松愉快的。

这是我第一次使用 GitHub Actions，体验到了墙外安装软件的速度，很快。

选择组件的过程也很有意思，总能发现白嫖的快乐，感谢赛博菩萨们。

折腾 Hugo 的过程非常欢乐。Hugo 的功能非常丰富，PaperMod、giscus、Pagefind 也有各自的功能和限制，所以有时候想实现一个特性，需要在各个组件的文档与代码中刨根问底。

例如，为了增加字数统计功能，需要启用 PaperMod 的字数统计配置； \
为了准确统计中文字数，需要在每篇博客的 Front matter 中添加 isCJKLanguage: true 声明； \
为了让每篇博客自动支持中文的字数统计，我首先在 archetypes（用于创建新内容的模板）中添加了 `isCJKLanguage: true` 配置；后来发现，hugo.yml 也支持通过 hasCJKLanguage 进行全局或语言下配置。

再比如增加目录时，需要启用 PaperMod 的目录配置； \
为了在目录栏的标题展示中文的 "目录" 而不是英文的 "Table of Contents"，要把默认语言设置成中文(zh)，再在 `i18n/zh.yml` 中设置 `toc` 的翻译。

又比如使用 giscus 添加评论功能时，我图简单用了 [hugomods/giscus](https://github.com/hugomods/giscus) ； \
但是很快就发现评论加载不出来(访问<https://giscus.app/zh/widget>报404)，而使用 giscus 原生的集成方式又能正常加载(访问<https://giscus.app/zh-CN/widget>)； \
原因是 hugomods/giscus 在拼接 url 时用了语言变量(`site.Language.Lang`)，而我在配置目录时把默认的语言从 `zh-CN` 改成了 `zh`； \
虽然 hugomods/giscus 提供了 `languages_mapping` 配置用于映射，但是我看了它的代码后，感觉继续用的话心智负担比较重，因此改回使用 giscus 原生方式做集成。

另外，为了使用环境变量配置 Giscus 参数，我尝试了 [Hugo 的环境变量](https://gohugo.io/getting-started/configuration/#configure-with-environment-variables)。
> 参考：[Hugo 环境变量的命名规范]({{< ref "/posts/config-hugo-with-env.md" >}})

搞清楚怎么用环境变量配置后，我又被 `hugo server` 一定会重新执行构建的行为 硬控了1个多小时；
> 由于需要使用 Pagefind 生成索引，我最开始的本地构建命令是 `hugo && npx pagefind && hugo server`； \
> 为了使用环境变量配置 giscus 参数，我把构建命令改成了 `HUGO_PARAMS_XXX=xxx hugo && npx pagefind && hugo server`； \
> 而 `HUGO_PARAMS_XXX=xxx` 这种方式是临时环境变量，只会在第一个 `hugo` 构建命令中生效，在最后的 `hugo server` 命令中没生效。 \
> 再加上 `hugo server` 会重新构建，这就会在没有配置 giscus 参数的情况下覆盖之前的结果。

虽然遇到了各种各样的问题，好在都顺利解决了，这是一个好的开始。
