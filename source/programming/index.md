---
title: Java & JS Commonness
date: 2020-09-04 20:24:51
tags: [IT, Programming]
categories: [IT, Programming]
---
# Code Structure
- expr, stmt `;` [..](https://javascript.info/structure) & blocks `{..}` [..](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/expressions.html)
	- JS line break & auto-semicolon insertion (no implicit ";" af incomplete expr & bf `[]` `()` [..](https://javascript.info/reference-type#syntax-check)) [..](https://javascript.info/structure#semicolon)
- comment: inline / mtp-line [..](https://javascript.info/structure#code-comments), javadoc/JSDoc [..](https://javascript.info/comments)
- Coding style & linters [..](https://javascript.info/coding-style)

# Variable
A **named storage** in memory for data
- `<type>`: allowed value, ops & memory used, e.g. primitive / reference type
	- static/dynamic typing: compile/runtime type checking
- `<name>`: descriptive ref of value (no keyword, UPPERCASE_CONST)
- `<value>`: [literal](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html "non-sharable, store in stack") / [reference]( "memory addr of obj, sharable, store in heap")
	- `null` literal: obj unavailable marker (`instanceof` => false)
- [var scope](https://javascript.info/closure#code-blocks "store temporary internal state, only visible inside the declaring block") & var shadow

# Char


# Operator
- comparison & equality check ops => bool
- conditional op `?..:`, logical ops `! && ||`
- bitwise/bit shift ops [..](https://www.geeksforgeeks.org/bitwise-operators-in-java/), op precedence & [op return](https://javascript.info/operators#assignment-returns-a-value)

# Control flow
- `if..else`, `switch` (block falls througth 贯穿 ==> case group)
	- `switch/case` arg: in JS, can be any expr, value compare `===`; [working types in Java](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/switch.html)
- `while` & `do..while`, `for` &  `for..of` (JS) & `for..:` (Java)
- `break`/`continue` (non-expr in JS, not in `?..`) & **label**

# Function
**Fn basics** [..](https://javascript.info/function-basics)
- fn naming ...
- fn-param: declare in fn(..) & init when fn-call by **args**
	- pass args to fn: copy value to fn-params (use as a local var in fn-body, change not seen outside, default value)
	- pass method to fn: callback fn, lambda expr
	- rest param: `fn(.., ...arr)` & `method(.., int... arr)`
- local var "shadow" outer var, global var
- return: default return `undefined` / `null` , `(...)` for mtp-line value

# Array
- `arr.length`, `arr[i]`
- `186****5678`: `fill(..)`

# String
- immutable, concat op `+`
- get info: `charAt(i)`, `indexOf()`, `starts/endsWith()`, `contains()`; 
- modify: `replace()`, `split()`, `substr()`, rm space：`trim()`, `replaceAll("\\s", "")`

# Obj-oriented
- OOP 3大基本特性: 封装，继承，多态 (一种定义，多种实现)

Class
- class & obj: prototype & instance 
- class member: **field** (value of state) & **method** (implementation of behavior), nested type;
- **constructor** & `new` op, `obj.<member>`; `this(..)` & `this.<member>`; 
- `static` method: no access to instance members, no `this`

- [Enum](https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html): a fixed set of consts

Inheritance
- `extends`, `super`; method/constructor & fields override 
- call `super(..)` in child.con(): 
	- JS uses parent.con() to create & init `this` for child instance when `new` [..](https://javascript.info/class-inheritance#overriding-constructor)
	- In JS, call parent.con() in child class, parent.con uses parent field value, not the overridden one in child class [..](https://javascript.info/class-inheritance#overriding-class-fields-a-tricky-note)
- `super()` called implicitly (Object has a empty con()), call `super(..)` explicitly [p159]
- `instanceof` op

[GC](https://javascript.info/garbage-collection): manually drop ref by `null`

# Refs
- [Oracle Java Tut](https://docs.oracle.com/javase/tutorial/java/TOC.html) & [Javase 8 API](https://docs.oracle.com/javase/8/docs/api/index.html)
- 《Java 从入门到精通》(项目案例版) 2017.9
- [javascript.info](https://javascript.info/)