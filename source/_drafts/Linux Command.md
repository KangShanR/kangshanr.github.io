---
title: Linux Commands
tags: [Linux, Ubuntu]
date: 2021-05-24 22:37:00
---

## [apt & apt-get](https://itsfoss.com/apt-vs-apt-get-difference/)

> apt 与 apt-get 差异

- 从参考文章中可知，apt 工具是更新的用来替换 apt-get 工具的。其中 apt search = apt-cache search
- 查看已安装工具 `apt list --installed`

### vi

> [vi 编辑器](http://vimdoc.sourceforge.net/htmldoc/syntax.html)

- 查找高亮：命令模式输入 `:set hlsearch`
- 设置高亮：`:hi search cterm=NONE ctermbg=gray ctermfg=red` ctermbg 设置背景色， ctermfg 设置前景色

## vim

- search `/` 从首至尾查找，`?` 从尾到首查找
    - 使用正则表达式查找 `/rex\(\n\|\s\)\+` 时多个表数量的符号需要添加 backslash `\` 符号。诸如：`\(` `\?` `\+` `\)`, `*` 不用转义
    - 使用 very magic 查找可省略 backslash `/\vrex(\n|\s)+`

## [正则表达式](https://en.wikipedia.org/wiki/Regular_expression)
