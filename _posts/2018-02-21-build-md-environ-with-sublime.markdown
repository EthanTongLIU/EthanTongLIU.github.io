---
layout:       post
title:        "Sublime Text 的 Markdown 编辑环境搭建"
subtitle:     "不使用 Markdown 的 IDE，搭建一个称心如意的 Markdown 写作环境"
date:         2018-02-21 19:17:00
author:       "LiuTong"
header-img:   "img/in-post/post-bg-hello-2018.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - Sublime Text
    - LaTeX
    - Markdown
---

> 世上无难事，只要肯登攀。
>             ------ 毛泽东

## 初识 Markdown

使用 Git 已经将近一年了，到现在基本上进步不是很大，只会做一些基本操作，在写这篇文章之前，我很好奇在创建每个项目的时候系统都会给添加一个 README.md 文件，我一直搞不清楚这个 md 文件是一种什么类型的文件。最近，一是因为有点时间，二是因为好奇，我比较详细地了解了下 Markdown 这种语言，虽然我不是很了解 HTML ，但是总归是有点基础，我很惊艳于 Markdown 的简单易用，感觉就像是发现了新大陆一样。由于我之前一直使用 LaTeX 进行写作，不管是写论文、技术文档、平时的大作业或者各种报告，我十分喜欢使用 LaTeX，因为其排版十分美观，而且功能强大，但是其也有很大的局限性。然而 Markdown 的出现可以说是完美的解决了我现在的需求，不仅仅可以方便快速的写博客，还可以兼容 LaTeX 的许多属性，比如可以插入数学公式。我希望以后 Markdown 可以成为我学习生活中的新锐利器。

工欲善其事，必先利其器。由于我一直不喜欢使用庞大冗余的 IDE 工具，因此在上网查找了一圈以后，我决定暂时不去使用目前的 Markdown 编辑工具，而是使用 Sublime Text，最终达到了我比较满意的写作环境效果。

由于这个工作可能之前在网上有许多人已经做过了，类似的技术文档也有不少，但是我在按照网上现存的许多技术文档在进行环境搭建和配置的时候，没有办法参照一篇文档完整而且细致的做好整个工作，而且为了搭建一个优秀的编辑环境，是有许许多多的细节需要去做的。软件每天都在更新，可能本文所介绍的工作在不久的将来也会out，但是为了不让更多想我一样的小白在初次接触 Markdown 的环境搭建的时候遇到同样的问题，少走一些弯路，我还是决定班门弄一下斧吧，希望本文所介绍的教程会对大家有所帮助，权当抛砖引玉吧。

## 安装 Sublime Text

[Sublime Text 3 官方下载地址](https://www.sublimetext.com/3){:target="_blank"}

## 安装插件 Package Control

Sublime Text 需要使用包控制插件（也就是这里的 Package Control 插件）实现对插件的安装卸载和管理，也就是说，想要在 Sublime Text 中安装其他的插件，需要首先安装 Package Control。

[官方给出的安装方法](https://packagecontrol.io/installation){:target="_blank"}

## 安装插件 MarkdownEditing

打开 Sublime Text，依次点击 `Preferences`、`Package Control`，接着在弹出的对话框中键入 `Install Package`。稍等，在弹出的对话框中搜索 `MarkdownEditing`，找到之后按 <kbd>enter</kbd> 键确认安装。

如果安装成功，之后会自动弹出纯文本形式显示的插件说明。点击 `Preferences`，在 `Package Settings` 里面会显示有 `Markdown Editing` 的插件设置选项，在里面可以更改该插件的设置。

## 安装插件 Markdown Preview

打开 Sublime Text，依次点击 `Preferences`、`Package Control`，接着在弹出的对话框中键入 `Install Package`。稍等，在弹出的对话框中搜索 `Markdown Preview`，找到之后 <kbd>enter</kbd> 键确认安装。

如果安装成功，之后会自动弹出纯文本形式显示的插件说明。点击 `Preferences`，在 `Package Settings` 里面会显示有 `Markdown Preview` 的插件设置选项，在里面可以更改该插件的设置。

* * *

在安装完上面两个插件之后，还需要知道上面的两个插件具体应该怎么使用，并且更改插件设置，以搭建最令我们满意的编辑环境，于是需要进行接下来的工作。

* * *

## 更改 MarkdownEditing 插件设置

首先我们来更改 MarkdownEditing 插件的设置。进入`Preferences`，`Package Settings`，找到`Markdown Editing`，在它的下拉子菜单中会发现许多选项，阅读或者更改这些选项来进行插件的配置。

*   阅读 README.md

    如果希望初步了解这个插件，可以阅读 README.md，如果按照本文所介绍的步骤完成整个环境的搭建，则可以在打开这个 README.md 文档之后利用本文所搭建的环境来预览生成的 HTML。

*   Markdown 的三种风格

    MarkdownEditing 插件自带三种风格，分别是 GFM Markdown(GitHub Flavored Markdown)、Standard Markdown、MultiMarkdown，具体的这些风格都在 README 中有介绍。

*   更改颜色主题

    Markdown 自带三种颜色主题，分别是 Yellow 黄色主题，Dark 暗色主题，ArcDark 暗色主题和 tm 明亮主题，设置的方法为进入`Preferences`，`Package Settings`，找到`Markdown Editing` 中的 `Change color scheme`，接着会出现下拉子菜单，相应的可以选择 Markdown 的编辑环境颜色主题。

    这里需要注意的一点就是，如果进入`Preferences`中直接修改`Color Scheme`，则修改结果可能不会改变当前 Markdown 编辑环境的颜色主题，而可以修改初 Markdown 以外的各种语言环境的颜色主题，原因就在于，要确保 Markdown 编辑环境是被 Sublime Text 默认的首选项用户设置排除在外的，这样说可能有点拗口了，接下来我来具体的说明一下。

    进入`Preferences`，点击`Settings`，这里就是 Sublime Text 的首选项设置界面，在这里你可以设置整个 Sublime Text 的编辑环境（对所有的语言环境都适用），而且在弹出的窗口中分为左右两栏，左边是 default 设置，也就是默认设置，右边是 user 设置，也就是用户设置，用户可以在 user 设置中设置自己喜好的选项，这一切设置都需要使用 json 语言，这里列出我的用户设置代码：

    ```json
    {
        "auto_complete_selector": "source, text",
        "color_scheme": "Packages/Theme - Brogrammer/brogrammer.tmTheme",
        "font_size": 14,
        "highlight_line": true,
        "ignored_packages":
        [
            "Markdown",
            "Vintage"
        ],
        "line_numbers": true,
        "mde.keep_centered": true,
        "rulers":
        [
            100
        ],
        "tab_size": 4,
        "theme": "Adaptive.sublime-theme",
        "translate_tabs_to_spaces": true,
        "trim_trailing_white_space_on_save": true,
        "word_wrap": true,
        "wrap_width": 100
    }
    ```

    注意这几行代码：

    ```json
    {
        "ignored_packages":
        [
            "Markdown",
            "Vintage"
        ],
    }
    ```

    这代表 Markdown 环境是被 Sublime Text 总的环境首选项设置所忽略的，所以任何外部首选项设置都不会影响到 Markdown 编辑环境的风格，这也就是为什么直接修改`Preferences`中的`Color Scheme`不会对 Markdown 编辑环境的颜色主题产生任何影响。类似的，修改上述代码段中的代码也是会自动跳过 Markdown 环境的。

    那么如何才能配置自己想要的 Markdown 编辑环境风格呢？聪明的你可能会想到将上述代码段中的 Markdown 取消忽略不就可以了吗，事实上这样是完全可以的，但事实上我还是建议你单独为 Markdown 搭建一个编辑环境，至于为什么这么做，于我而言，Markdown 环境和我平时使用的其他语言编辑环境有区别，对于其他语言，尤其是 LaTeX 环境，我偏偏钟爱 brogrammer 主题，而这个主题如果设置给 Markdown 的话效果很不理想。所以，接下来我们看一看如何单独配置 Markdown 的编辑环境。

* 配置 Markdown 编辑环境

    * 方法一

        进入`Preferences`，点击`Preferences`，`Package Settings`，`Markdown Editing`。选择`Markdown GFM Settings - User`、`Markdown (Standard) Settings - User`、`MultiMarkdown Settings - User`中的任何一个来设置用户选项，以下是我的配置：

        ```json
        {
            "auto_complete_selector": "source, text",
            "color_scheme": "Packages/MarkdownEditing/MarkdownEditor-Dark.tmTheme",
            "highlight_line": true,
            "line_numbers": true,
            "mde.keep_centered": true,
            "tab_size": 4,
            "translate_tabs_to_spaces": true,
            "trim_trailing_white_space_on_save": true,
            "word_wrap": true,
            "wrap_width": 80
        }
        ```

    * 方法二

        进入`Preferences`，点击`Browse Packages`，进入插件文件夹，进入`User`文件夹，找到`Markdown.sublime-settings`，使用 Sublime Text 打开，在里面可以添加用户设置，以下是我的配置：

        ```json
        {
            "auto_complete_selector": "source, text",
            "color_scheme": "Packages/MarkdownEditing/MarkdownEditor-Dark.tmTheme",
            "highlight_line": true,
            "line_numbers": true,
            "mde.keep_centered": true,
            "tab_size": 4,
            "translate_tabs_to_spaces": true,
            "trim_trailing_white_space_on_save": true,
            "word_wrap": true,
            "wrap_width": 80
        }
        ```

    以上两种方法在本质上其实是相同的，只是进入的路径不同，如果通过其中一种路径修改了用户配置，另一种路径下的文件都会做出相同的改变。

* * *

通过以上的设置，我们初步搭建成功 Markdown 的编辑环境，只有编辑环境是不够的，我们还需要搭建 Markdown 文件预览环境，插件 Markdown Preview 的作用就在于此，上面我们已经成功安装了此插件，接下来让我们了解一下这个插件的配置和使用方法。

* * *

## 更改 Markdown Preview 插件设置

*   设置预览热键

进入`Preferences`，点击`Key Bindings`，在弹出的界面右侧用户设置中添加如下代码：

```json
[
     { "keys": ["alt+m"], "command": "markdown_preview", "args": {"target": "browser", "parser":"markdown"} },
]
```

则可以设置 <kbd>alt</kbd> + <kbd>m</kbd> 组合为预览排版好的 HTML 样式的热键，预览工具是本机上设置的默认浏览器。

*   开启 Mathjax 数学环境支持

如果想要在 Markdown 中插入数学公式，可以使用 LaTeX 的语法来书写，但是直接使用 LaTeX 语法来书写一段数学公式，使用 Markdown Preview 插件预览时的效果是不成功的，我们需要开启 Mathjax 数学环境支持，设置的方法为进入`Preferences`，点击`Package Settings`，`Markdown Preview`，选择`Settings - User`，在里面添加如下代码：

```json
{
    "enable_mathjax": true,
}
```

之后即可直接在 Markdown 中使用 LaTeX 语法书写数学公式，例如，这里有一段 LaTeX 代码：

```latex
\begin{equation}
    \sum\limits_{0}^{+\infty}\dfrac{n^2}{2}
\end{equation}
```

显示效果如下：

\begin{equation}
    \sum\limits_{0}^{+\infty}\dfrac{n^2}{2}
\end{equation}

*   开启代码高亮支持

由于需要经常在 Markdown 中插入代码，如果希望插入的代码保持高亮风格，需要按照上一条的方法在相同的位置添加如下代码：

```json
{
    "enable_highlight": true,
}
```

***

至此，搭建 Sublime Text 下的 Markdown 编辑环境到此结束，或许中间还有一些细节没有完善，我会在后续的工作中逐渐将其完善。希望本文能对您有所帮助。






