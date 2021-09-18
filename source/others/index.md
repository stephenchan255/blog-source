---
title: Others
date: 2020-09-04 20:23:20
tags: [IT, Programming, Cpp]
categories: [IT, Programming, Cpp]
---

# 入栈出栈问题
任何出栈的元素后面出栈的元素必须满足以下三点：
1. 在原序列中相对位置比它小的，必须是逆序；
1. 在原序列中相对位置比它大的，顺序没有要求；
1. 以上两点可以间插进行。

一个栈的入栈序列为ABCDEF，则不可能的出栈序列是（D）
　　A、DEFCBA　　　　B、DCEFBA　　　　C、FEDCBA
　　D、FECDBA　　　　E、ABCDEF　　　　F、ADCBFE
> D选项, F后面出栈的，顺序必须是EDCBA。 [参考](https://blog.csdn.net/qq_1932568757/article/details/82752325)
