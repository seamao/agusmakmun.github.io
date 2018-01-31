---
layout: post
title:  "Top 10 golang libraries of 2018"
date:   2018-01-30 18:34:10 +0700
categories: [python, django]
---

# 1月30日学习总结

## 小概念　/ 非官方
- 管道

　若后边没有被赋值，则为非缓冲管道，反之则为缓冲管道

```
var ch chan int //创建了一个管道
```
- go语法


在方法的前面加上go　则表示此为一个子线程，又名分线程

```
go func() {
}()
```
- time　方法


调用time方法，`time.Sleep(time.Second)` 表示runtime的时候休息１秒
```
import (
	"fmt"
	"time"
)
```
