---
layout: post
title:  "golang字串与进制相互转形"
date:   2018-05-25
excerpt: "golang字串与进制相互转形"
description: "golang字串与进制相互转形"
tag:
- golang
- strconv
comments: true
---


package main

import (
	"fmt"
	"strconv"
)

func main() {
	/*
		string与其他数据类型的转换
		string-->int,float64,bool....
			ParseXXX()
				ParseInt(),ParseBool(),ParseFloat()


		int,float,bool--->string
				FormatXXX()
				FormatBool(),FormatInt(),FormatFloat()

		Atoi(string)-->int,10进制
		Itoa(int)-->string,10进制整数-->字符串
	*/
	//字符串：
	s1 := "200"
	fmt.Println(s1)
	//"12" -->int 12
	i1, err := strconv.ParseInt(s1, 10, 32)
	fmt.Println(i1, err) //int64
	fmt.Printf("%T\n", i1)

	s2 := "true"
	b1, err := strconv.ParseBool(s2)
	fmt.Println(b1, err)

	num1 := 100
	s3 := strconv.FormatInt(int64(num1), 10)
	fmt.Printf("%T,%s\n", s3, s3)
	s4 := strconv.FormatBool(false)
	fmt.Printf("%T,%s\n", s4, s4)

	i2, err := strconv.Atoi(s1) //相当于ParseInt(10进制)
	fmt.Println(i2)
	s5 := strconv.Itoa(1100)
	fmt.Println(s5)

	//i1 := 18 //默认读10进制
	//fmt.Println(i1)
	//i2 := 016 //以0开头，表示8进制
	//i3 := 0x16 // 以0x开头，表16进制
	//fmt.Println(i1)
	//fmt.Println(i2)
	//fmt.Println(i3)

}

