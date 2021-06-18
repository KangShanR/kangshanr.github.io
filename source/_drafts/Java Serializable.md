---
title: Java Serializable
tags: [Java, Serializable]
date: 2021-06-18 10:50:00
categories: Java
---

## Java Serializable

- 反/序列化使用 serialVersionUID 标志一个版本
- 类对象 serialVersionUID 是独立于类的，所有不需要类间保持 serialVersionUID 唯一
- 如果不显示添加一个 serialVersionUID JVM 会为其自动生成一个，也正因为如此，如果不显示添加一个在给对象添加一个属性（兼容性操作）时，再反序列化升级前被序列化的数据时会出现 InvalidClassException 异常
- 兼容性升级类（添加属性之类）不需要升级 serialVersionUID ,升级后的属性会被反序化成 null
