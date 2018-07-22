---
layout:       post
title:        "Java 使用 POI 读取 word/excel 文档"
subtitle:     ""
date:         2018-07-20 21:28:00
author:       "LiuTong"
header-img:   "img/in-post/post-bg-java-poi.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - Java
---

# Java 使用 POI 读取 word/excel 文档

最近在做一份排版的工作，工作内容是 TeX 文档录入，雇方提供了文档模板，在录入过程中，手工录入存在效率低、速度慢、工作累的问题，于是我希望能够写一个脚本，使用 Java 直接读入 word 文档，使用 Java 正则表达式检索其中的一些特定内容实现替换。在后期的编程工作中，经过尝试，我发现这份工作并不能够很好的完成，一方面是因为给的文稿属于真真正正的草稿，文稿变数太大，并且有一些无法识别其 Unicode 码的字符（既不在中文文字编码序列中，也不属于英文字符编码序列），所以只能实现部分替换的功能，对工作效率提升不大，但对于学习使用 Java 处理 word 文档和学习了解正则表达式具有积极的意义。以下介绍一下如何使用 Java 读取 word/excel 文档。

## POI 简介

## 导入 jar 包

## word/excel io 流
