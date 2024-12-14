---
title: 第一次使用 Hexo
date: 2024-12-14 17:00:24
tags:
    - Hexo
    - 博客搭建
---

## 前言

作为一名游戏开发者，我一直想要一个自己的博客来记录学习心得。经过对比多个博客框架后，最终选择了 Hexo 作为我的博客框架。这篇文章记录一下我使用 Hexo 搭建博客的过程。

## 为什么选择 Hexo？

1. **简单易用**：Hexo 的安装和使用都很简单，对于像我这样主要专注于游戏开发的开发者来说很友好
2. **Markdown 支持**：可以使用 Markdown 语法写文章，这对于写技术博客很方便
3. **主题丰富**：有很多优秀的主题可以选择
4. **部署方便**：可以轻松部署到各种平台

## 安装过程

1. 首先需要安装 Node.js
2. 然后通过 npm 全局安装 Hexo：

```bash
npm install -g hexo-cli
```

3. 创建一个新的 Hexo 项目：

```bash
hexo init my-blog
cd my-blog
npm install
```

## 基本使用

### 常用命令

```bash
hexo new "post-name" # 创建新文章
hexo generate # 生成静态文件
hexo server # 启动本地服务器
hexo deploy # 部署到远程服务器
```

### 文章编写

文章都在 `source/_posts` 目录下，使用 Markdown 格式编写。

## 主题配置

我选择了 cactus 主题，主要是因为它简洁大方。安装主题的步骤：

1. 在博客根目录下克隆主题：

```bash
git clone https://github.com/probberechts/hexo-theme-cactus.git themes/cactus
```

2. 修改博客根目录下的 `_config.yml`，将主题设置为 cactus：

```yml
# theme: landscape
theme: cactus
```

3. 主题提供了多种配色方案可选：

-   dark（深色）
-   light（浅色）
-   white（白色）
-   classic（经典）

可以在主题配置文件中设置：

```yml
colorscheme: light # 选择你喜欢的配色方案
```

4. 配置导航菜单，在主题配置文件中设置：

```yml
nav:
    home: /
    about: /about/
    articles: /archives/
    projects: http://github.com/你的用户名
```

5. 添加社交媒体链接：

```yml
social_links:
    - icon: github
      link: https://github.com/你的用户名
    - icon: twitter
      link: https://twitter.com/你的用户名
```

6. 如果需要启用搜索功能，需要先安装搜索插件：

```bash
npm install hexo-generator-search --save
```

然后创建搜索页面：

```bash
hexo new page search
```

在新创建的 `source/search/index.md` 中添加：

```markdown
title: Search
type: search

---
```

最后在导航菜单中添加搜索链接：

```yml
nav:
    search: /search/
```

主题的详细配置选项可以参考 [cactus 主题文档](https://github.com/probberechts/hexo-theme-cactus)。

## 遇到的问题

1. **导航栏显示异常**：使用 cactus 主题时，页面向下滚动导航栏会隐藏，但滚动回顶部时导航栏不会自动显示出来。

解决方案：修改 `themes/cactus/source/js/main.js` 文件中的滚动监听代码。将：

```javascript
var topDistance = menu.offset().top;
```

改为：

```javascript
var topDistance = document.documentElement.scrollTop;
```

这个修改解决了导航栏的显示问题，现在可以正常根据滚动位置显示/隐藏导航栏了。

原因分析：原代码使用 `menu.offset().top` 获取菜单距离顶部的距离，但这个值在某些情况下可能不准确。改用 `document.documentElement.scrollTop` 直接获取页面的滚动位置会更可靠。

## 后续计划

1. 优化博客样式
2. 添加更多功能
3. 持续更新游戏开发相关的技术文章

## 总结

Hexo 确实是一个很好的博客框架，虽然刚开始使用可能会遇到一些问题，但整体来说还是很顺利的。希望这个博客能够帮助我更好地记录和分享游戏开发的经验。

---

如果你也想搭建自己的博客，欢迎参考我的经验，有任何问题都可以通过评论区或者联系方式找我交流。
