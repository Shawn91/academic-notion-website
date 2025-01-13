# 1. Blog Post 模板
## 1.1 基本信息
在 docs/blog/posts 文件夹内可以随意创建文件夹，只要是 markdown 文档，都会识别为帖子。

开头的 `---` 之间的部分用于定义一个帖子的 metadata，其中 date 是必须的。

`#` 开头的标题将作为帖子的标题。

`#` 与 `<!-- more -->` 之间的文本本身是帖子的内容，同时也是首页展示时的预览文本。
```text
---
date:
  created: 2023-12-31
---

# Happy new years eve!

We hope you are all having fun and wish you all the best for the new year!
<!-- more -->

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua.
```
## 1.2 metadata
帖子模板中开头的 `---` 之间的部分用于定义一个帖子的 metadata，其中 date 是必须的。

* draft: 标记当前帖子是否是草稿。如果值为 true，则 `mkdocs build` 时不会将其打包发布，但是使用 `mkdocs serve` 本地运行时，会看到该帖子（被标记为 draft）
* updated: 帖子更新日期。这个字段只是用于在帖子内显示在xxx日期更新过，不会反映在首页
* categories：分类。分类本身会自动在首页创建。
* tags：tags 不会在首页创建分类。如何管理tags参考 [5. 设置 tags](#5. 设置 tags)

```
---
date:
  created: 2024-01-01
  updated: 2024-01-02
draft: true
categories:
  - Hello
  - World
tags:
  - Foo
  - Bar
---
```

## 1.3 站内超链接

从一个 Post 链接到另一个 post

```markdown
---
date:
  created: 2025-01-10
---

# Happy new years eve!

This is a [link](./test2.md) to a post.
```

最后渲染出来后，`link` 就是超链接，点击后就会进入 test2.md 对应的 post（test2.md 是另一个 post 的模板内容，会自动打包生成对应的网页）。

注意：只能使用相对于当前的 post 在项目中的 relative path 来写链接地址。使用 `..` 表示向上一个层级

# 2. 设置网站为 blog only

mkdocs 默认是为文档网站服务的，额外提供了 blog功能，当我们只需要 blog 功能时，如下设置

1. 将 posts 文件夹从原本的 `root/docs/blog/posts` 路径改为 `root/docs/posts`路径，即移除了 `blog` 这个文件夹
2. 在 `mkdocs.yml` 文件中如下设置

```yaml
plugins:
  - blog:
      blog_dir: . 
```

# 3. 设置 mkdocs.yml

## 3.1 限制帖子分类

在帖子内，可以使用 categories 来设置当前帖子的 category。为了避免因为 type 而创建新的分类，可以在 mkdocs.yml 中如下设置

```yaml
plugins:
  - blog:
      categories_allowed:
        - Search
        - Performance
```

## 3.2 设置每页显示多少个帖子

默认是每页10个帖子

```yaml
- blog:
    pagination_per_page: 5
```

## 3.3 post 阅读过程中自动更新 url 中的 anchor

```yaml
theme:
  features:
    - navigation.tracking
```

# 4. 设置导航

## 4.1 设置导航

```yaml
nav:
  - Home: index.md
  - Install: install.md
  - Usage: usage.md
  - Blog:
     - ./index.md
```

注意：之所以是 `- ./index.md` 是因为参考 `2. 设置网站为 blog only` 设置了 Blog 功能没有独立的 `blog` 文件夹，如果是默认情况下有 `blog`文件夹，则应该写为 `- blog/index.md`，即这里的开头路径应该始终和 `blog_dir` 参数一致

## 4.2 在页面顶部显示导航

```yaml
theme:
  name: material
  features:
    - navigation.tabs
    - navigation.tabs.sticky

nav:
  - Home: index.md
  - Terms of Use: terms_of_use.md
  - Blog:
     - index.md
     - Tags: tags.md
```

如上设置后，Home, Terms of Use, Blog 这几个导航项会显示在页面顶部，打开 Blog 页面后，Blog 内的导航项（含自定义的 Tags）则在页面左侧显示

如果没有 `navigation.tabs` 的设置，则所有导航项都在页面左侧

`navigation.tabs.sticky` 用于让导航项是否自动隐藏，可以设置，也可以不设置

# 5. 设置 tags

首先在 docs 文件夹内创建一个 `tags.md`（位置可以自定义，以下路径修改即可），内容为空即可。这个文件的作用是告诉 mkdocs 在哪保存 tags 与posts的关系

```yaml
plugins:
    - blog
    - tags:
        tags_file: tags.md

nav:
    - Blog:
        - blog/index.md
        - Tags: tags.md
```

然后，在导航栏就会创建一个 Tags 标签，点击后，就会展示每个 tag 下的所有文章