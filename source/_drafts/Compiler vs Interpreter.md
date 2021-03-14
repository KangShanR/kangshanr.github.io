---
title: Compiler VS Interpreter
tags: [Complier, Interpreter]
date: 2021-03-14 11:28
categories: Programming
---

## 编译器与解释器的区别

[reference](https://medium.com/@rahul77349/difference-between-compiler-and-interpreter-with-respect-to-jvm-java-virtual-machine-and-pvm-22fc77ae0eb7)

编译器是将高级语言编译成机器码,解析器是在编译成机器码后立即执行代码.

在Java 平台中,先有编译器将Java语言编译成字节码(.class file),然后再到JVM中,进行解释执行(interpreter)或实时编译(Just in time compiler)后执行.这儿涉及到两个编译器,将 Java 文件编译成字节码文件的编译器与 JVM 中实时编译器,两者目标完全不一致.
