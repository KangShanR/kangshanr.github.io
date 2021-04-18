---
title: 计算机组成原理
tags: [Computer Organization]
date: 2021-04-18 12:24:33
---

## tips

- 使用 `tcc -g -c text.c` 编译 c 语言代码,生成 .o 对象
- 再使用命令 `objdump -S test.o` 查看生成的 .o 对象 里面的编译好的机器码与汇编码.可以看出一行代码写在前面,而后面跟着的多行就是编译后的机器码与汇编码
