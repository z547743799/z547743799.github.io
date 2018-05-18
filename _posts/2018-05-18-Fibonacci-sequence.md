---
layout: post
title:  "go-斐波那契数列"
date:   2018-05-16
excerpt: "go-斐波那契数列"
description: "go-斐波那契数列"
tag:
- 斐波那契数列
comments: true
---
# 斐波那契数列
#### 链接:[斐波那契数列讲解](https://www.youtube.com/watch?v=VCJsUYeuqaY)

斐波那契数列中的斐波那契数会经常出现在我们的眼前——比如松果、凤梨、树叶的排列、某些花朵的花瓣数（典型的有向日葵花瓣），蜂巢，蜻蜓翅膀，超越数e（可以推出更多），黄金矩形、黄金分割、等角螺线，十二平均律等。

```go
package main

import "fmt"

func main()  {
	x:=1
	s:=0
	a:=0
	for i:=0;i<12 ;i++  {


	a=s+x
     x=s
     s=a

		fmt.Println(a)

	}
	fmt.Println(a)
}

```

