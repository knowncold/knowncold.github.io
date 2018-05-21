---
title: SML笔记
layout: page
category: wiki
---

## Installation

```
brew install smlnj
```

或者从[官网](http://www.smlnj.org/index.html)下载`pkg`文件安装。

### Emacs

`C-c C-s`

`use "fisrt-sml;`

## Syntax

```sml
val x = 2;
(* dynamic enviroment: x --> 2 *)
(* static enviroment: x : int *)

val y = x + 3;

if a>b then e1 else e2;  (* e0 must be bool, e1 and e2 must be the same type *)
```

## Semantics

- Type-checking before run -- static enviroment
- Evaluation while run -- dynamic enviroment

### Type-checking

- type fails
- int bool unit

### Evaluation

- value
- exception
- infinited loop

## Variables

### Syntax

not starting with digit

### Type-checking

look up type in current static enviroment

### Evaluation

look up value in current dynamic enviroment

## Addition

### Synatx

e1+e2

### Type-checking

If e1 and e2 have type int, then e1+e2 has type int.

### Evaluation

e1=v1,e2=v2 then e1+e2 = v1+v2

## Value

values are expressions
not all expressions are values

() has type `unit`

## REPL

### use

It enters bindings from the .sml file, results is () bound to variable it.


Read-Eval-Print-Loop

`C-d` to terminate the REPL.

there is 0 - 5 not -5.
~5
