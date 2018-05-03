---
layout: post
title: "Golang经典笔试题及答案（上篇）"
date: 2018-04-24
description: "Golang经典笔试题及答案（上篇）Golang经典笔试题及答案（上篇）Golang经典笔试题及答案（上篇）Golang经典笔试题及答案（上篇）Golang经典笔试题及答案（上篇）Golang经典笔试题及答案（上篇）Golang经典笔试题及答案（上篇）"
tag: 笔试题
keywords: "golang经典笔试题及答案 "
---

> 整理：黎跃春
> 博客：http://liyuechun.org
> 官网：http://kongyixueyuan.com
 
### 1. 写出下面代码输出内容

```Go
package main

import (
	"fmt"
)

func main() {
	defer_call()
}

func defer_call() {

	defer func() {
		fmt.Println("打印前")
	}()

	defer func() {
		fmt.Println("打印中")
	}()

	defer func() {
		fmt.Println("打印后")
	}()

	panic("触发异常")
}
```

在这个案例中，`触发异常`这几个字打印的顺序其实是不确定的。`defer`, `panic`, `recover`一般都会配套使用来捕捉异常。先看下面的案例：

- 案例一

```
package main

import (
	"fmt"
)

func main() {
	defer_call()
}

func defer_call() {

	defer func() {
		fmt.Println("打印前")
	}()

	defer func() {
		fmt.Println("打印中")
	}()

	defer func() { // 必须要先声明defer，否则recover()不能捕获到panic异常

		if err := recover();err != nil {
			fmt.Println(err) //err 就是panic传入的参数
		}
		fmt.Println("打印后")
	}()

	panic("触发异常")
}
```

**输出内容为：**

```
触发异常
打印后
打印中
打印前

Process finished with exit code 0
```

- 案例二

```
package main

import (
	"fmt"
)

func main() {
	defer_call()
}

func defer_call() {

	defer func() {
		fmt.Println("打印前")
	}()

	defer func() { // 必须要先声明defer，否则recover()不能捕获到panic异常
		if err := recover();err != nil {
			fmt.Println(err) //err 就是panic传入的参数
		}
		fmt.Println("打印中")
	}()

	defer func() {

		fmt.Println("打印后")
	}()

	panic("触发异常")
}
```

**输出内容为：**

```
打印后
触发异常
打印中
打印前

Process finished with exit code 0
```

- 案例三

```
package main

import (
	"fmt"
)

func main() {
	defer_call()
}

func defer_call() {

	defer func() {
		if err := recover();err != nil {
			fmt.Println(err) //err 就是panic传入的参数
		}
		fmt.Println("打印前")
	}()

	defer func() { // 必须要先声明defer，否则recover()不能捕获到panic异常
		if err := recover();err != nil {
			fmt.Println(err) //err 就是panic传入的参数
		}
		fmt.Println("打印中")
	}()

	defer func() {
		if err := recover();err != nil {
			fmt.Println(err) //err 就是panic传入的参数
		}
		fmt.Println("打印后")
	}()

	panic("触发异常")
}
```

**输出内容为：**

```
触发异常
打印后
打印中
打印前

Process finished with exit code 0
```

**总结：**

1. `defer`函数属延迟执行，延迟到调用者函数执行 `return` 命令前被执行。多个`defer`之间按`LIFO`先进后出顺序执行。
2. `Go`中可以抛出一个`panic`的异常，然后在`defer`中通过`recover`捕获这个异常，然后正常处理。
3. 如果同时有多个`defer`，那么异常会被最近的`recover()`捕获并正常处理。

### 2. 以下代码有什么问题，说明原因

```Go
package main
import (
	"fmt"
)
type student struct {
	Name string
	Age  int
}
func pase_student() map[string]*student {
	m := make(map[string]*student)
	stus := []student{
		{Name: "zhou", Age: 24},
		{Name: "li", Age: 23},
		{Name: "wang", Age: 22},
	}
	for _, stu := range stus {
		m[stu.Name] = &stu
	}
	return m
}
func main() {
	students := pase_student()
	for k, v := range students {
		fmt.Printf("key=%s,value=%v \n", k, v)
	}
}
```

**运行结果：**

```
key=zhou,value=&{wang 22} 
key=li,value=&{wang 22} 
key=wang,value=&{wang 22} 

Process finished with exit code 0
```

**修改一下代码：**

**将下面的代码：**

```
for _, stu := range stus {
    m[stu.Name] = &stu
}
```

**修改为：**

```
for _, stu := range stus {
	fmt.Printf("%v\t%p\n",stu,&stu)
	m[stu.Name] = &stu
}
```

**运行结果为：**

```
{shen 24}	0xc4200a4020
{li 23}	0xc4200a4020
{wang 22}	0xc4200a4020
key=shen,value=&{wang 22} 
key=li,value=&{wang 22} 
key=wang,value=&{wang 22} 

Process finished with exit code 0
```

**通过上面的案例，我们不难发现`stu`**变量的地址始终保持不变，每次遍历仅进行`struct`值拷贝，故`m[stu.Name]=&stu`实际上一直指向同一个地址，最终该地址的值为遍历的最后一个`struct`的值拷贝。

**形同如下代码：**

```
var stu student 
for _, stu = range stus {
	m[stu.Name] = &stu
} 
```

**修正方案，取数组中原始值的地址：**

```
for i, _ := range stus {
	stu:=stus[i]
	m[stu.Name] = &stu
}
```

**重新运行，效果如下：**

```
{shen 24}	0xc42000a060
{li 23}	0xc42000a0a0
{wang 22}	0xc42000a0e0
key=shen,value=&{shen 24} 
key=li,value=&{li 23} 
key=wang,value=&{wang 22} 

Process finished with exit code 0
```


### 3. 下面的代码会输出什么，并说明原因

```
package main

import (
	"fmt"
	"runtime"
	"sync"
)

func init() {
	fmt.Println("Current Go Version:", runtime.Version())
}
func main() {
	runtime.GOMAXPROCS(1)

	count := 10
	wg := sync.WaitGroup{}
	wg.Add(count * 2)
	for i := 0; i < count; i++ {
		go func() {
			fmt.Printf("[%d]", i)
			wg.Done()
		}()
	}
	for i := 0; i < count; i++ {
		go func(i int) {
			fmt.Printf("-%d-", i)
			wg.Done()
		}(i)
	}
	wg.Wait()
}
```

**运行效果：**

```
Current Go Version: go1.10.1
-9-[10][10][10][10][10][10][10][10][10][10]-0--1--2--3--4--5--6--7--8-
Process finished with exit code 0
```

两个`for`循环内部`go func` 调用参数i的方式是不同的，导致结果完全不同。这也是新手容易遇到的坑。

第一个`go func`中`i`是外部`for`的一个变量，地址不变化。遍历完成后，最终`i=10`。故`go func`执行时，`i`的值始终是`10`（`10`次遍历很快完成）。

第二个`go func`中`i`是函数参数，与外部`for`中的`i`完全是两个变量。尾部(`i`)将发生值拷贝，`go func`内部指向值拷贝地址。

### 4. 下面代码会输出什么？

```
package main

import "fmt"

type People struct{}

func (p *People) ShowA() {
	fmt.Println("showA")
	p.ShowB()
}
func (p *People) ShowB() {
	fmt.Println("showB")
}

type Teacher struct {
	People
}

func (t *Teacher) ShowB() {
	fmt.Println("teacher showB")
}

func main() {
	t := Teacher{}
	t.ShowA()
}
```

**运行结果如下：**

```
showA
showB

Process finished with exit code 0
```

`Go`中没有继承,上面这种写法叫组合。

上面的`t.ShowA()`等价于`t.People.ShowA()`，将上面的代码修改如下：

```
func main() {
	t := Teacher{}
	t.ShowA()
	fmt.Println("---------------")
	t.People.ShowA()
}
```

**运行结果为：**

```
showA
showB
---------------
showA
showB

Process finished with exit code 0
```


### 5. 下面代码会触发异常吗？请详细说明

```
package main

func main() {
	runtime.GOMAXPROCS(1)
	int_chan := make(chan int, 1)
	string_chan := make(chan string, 1)
	int_chan <- 1
	string_chan <- "hello"
	select {
    	case value := <-int_chan:
    		fmt.Println(value)
    	case value := <-string_chan:
    		panic(value)
	}
}
```

有可能会发生异常，如果没有`selct`这段代码，就会出现线程阻塞，当有`selct`这个语句后，系统会随机抽取一个`case`进行判断，只有有其中一条语句正常`return`，此程序将立即执行。

### 6. 下面代码输出什么？

```
package main

import "fmt"

func calc(index string, a, b int) int {
	ret := a + b
	fmt.Println(index, a, b, ret)
	return ret
}

func main() {
	a := 1
	b := 2
	defer calc("1", a, calc("10", a, b))
	a = 0
	defer calc("2", a, calc("20", a, b))
	b = 1
}
```

**运行结果如下：**

```
10 1 2 3
20 0 2 2
2 0 2 2
1 1 3 4

Process finished with exit code 0
```

**在解题前需要明确两个概念：**

- `defer`是在函数末尾的`return`前执行，先进后执行。
- 函数调用时 `int` 参数发生值拷贝。

不管代码顺序如何，`defer calc func`中参数`b`必须先计算，故会在运行到第三行时，执行`calc("10",a,b)`输出：`10 1 2 3`得到值`3`，将`cal("1",1,3)`存放到延后执执行函数队列中。

执行到第五行时，现行计算`calc("20", a, b)`即`calc("20", 0, 2)`输出：`20 0 2 2`得到值`2`,将`cal("2",0,2)`存放到延后执行函数队列中。

执行到末尾行，按队列先进后出原则依次执行：`cal("2",0,2)、cal("1",1,3) `，依次输出：`2 0 2 2、1 1 3 4` 。

### 7. 请写出以下输入内容


```
package main

import "fmt"

func main() {
	s := make([]int, 5)
	fmt.Printf("%p\n", s)
	s = append(s, 1, 2, 3)
	fmt.Printf("%p\n", s) //new pointer
	fmt.Println(s)
}
```

**运行结果:**

```
0xc4200180c0
0xc42001c0a0
[0 0 0 0 0 1 2 3]

Process finished with exit code 0
```

### 8. 下面的代码有什么问题

```

package main

import (
	"fmt"
	"sync"
)

type UserAges struct {
	ages map[string]int
	sync.Mutex
}

func (ua *UserAges) Add(name string, age int) {
	ua.Lock()
	defer ua.Unlock()
	ua.ages[name] = age
}

func (ua *UserAges) Get(name string) int {
	if age, ok := ua.ages[name]; ok {
		return age
	}
	return -1
}

func main() {
	count := 1000
	gw := sync.WaitGroup{}
	gw.Add(count * 3)
	u := UserAges{ages: map[string]int{}}
	add := func(i int) {
		u.Add(fmt.Sprintf("user_%d", i), i)
		gw.Done()
	}
	
	for i := 0; i < count; i++ {
		go add(i)
		go add(i)
	}
	
	for i := 0; i < count; i++ {
		go func(i int) {
			defer gw.Done()
			u.Get(fmt.Sprintf("user_%d", i))
		}(i)
	}
	gw.Wait()
	fmt.Println("Done")
}
```

**输出结果：**

```
fatal error: concurrent map read and map write

goroutine 2022 [running]:
runtime.throw(0x10c5472, 0x21)
```

**结论：** 在执行 `Get` 方法时可能被`panic`。

虽然有使用`sync.Mutex`做写锁，但是`map`是并发读写不安全的。`map`属于引用类型，并发读写时多个协程见是通过指针访问同一个地址，即访问共享变量，此时同时读写资源存在竞争关系。所以会报错误信息:`fatal error: concurrent map read and map write`。

如果第一次没复现`panic`问题，可以再次运行，复现该问题。那么如何改善呢? 在`Go1.9`新版本中将提供并发安全的`map`。首先需要了解两种锁的不同：

- `sync.Mutex`互斥锁
- `sync.RWMutex`读写锁，基于互斥锁的实现，可以加多个读锁或者一个写锁。

**RWMutex相关方法：**

```
type RWMutex
    func (rw *RWMutex) Lock() 
    func (rw *RWMutex) RLock()
    func (rw *RWMutex) RLocker() Locker
    func (rw *RWMutex) RUnlock()
    func (rw *RWMutex) Unlock()
```

**代码改进如下：**

```
package main

import (
	"fmt"
	"sync"
)

type UserAges struct {
	ages map[string]int
	sync.RWMutex
}

func (ua *UserAges) Add(name string, age int) {
	ua.Lock()
	defer ua.Unlock()
	ua.ages[name] = age
}

func (ua *UserAges) Get(name string) int {
	ua.RLock()
	defer ua.RUnlock()
	if age, ok := ua.ages[name]; ok {
		return age
	}

	return -1
}

func main() {
	count := 10000
	gw := sync.WaitGroup{}
	gw.Add(count * 3)
	u := UserAges{ages: map[string]int{}}
	add := func(i int) {
		u.Add(fmt.Sprintf("user_%d", i), i)
		gw.Done()
	}
	for i := 0; i < count; i++ {
		go add(i)
		go add(i)
	}
	for i := 0; i < count; i++ {
		go func(i int) {
			defer gw.Done()
			u.Get(fmt.Sprintf("user_%d", i))
			fmt.Print(".")
		}(i)
	}
	gw.Wait()
	fmt.Println("Done")
}
```

**运行结果如下：**

```
.
.
.
.
Done

Process finished with exit code 0
```


### 9. 下面的迭代会有什么问题？

```
package main

import "fmt"
import "sync"
import "time"

type ThreadSafeSet struct {
	sync.RWMutex
	s []int
}

func (set *ThreadSafeSet) Iter() <-chan interface{} {
	ch := make(chan interface{})
	go func() {
		set.RLock()

		for elem := range set.s {
			ch <- elem
			fmt.Print("get:", elem, ",")
		}

		close(ch)
		set.RUnlock()

	}()
	return ch
}

func main() {
	//read()
	unRead()
}
func read() {
	set := ThreadSafeSet{}
	set.s = make([]int, 100)
	ch := set.Iter()
	closed := false
	for {
		select {
		case v, ok := <-ch:
			if ok {
				fmt.Print("read:", v, ",")
			} else {
				closed = true
			}
		}
		if closed {
			fmt.Print("closed")
			break
		}
	}
	fmt.Print("Done")
}

func unRead() {
	set := ThreadSafeSet{}
	set.s = make([]int, 100)
	ch := set.Iter()
	_ = ch
	time.Sleep(5 * time.Second)
	fmt.Print("Done")
}

```

**结论：**内部迭代出现阻塞。默认初始化时无缓冲区，需要等待接收者读取后才能继续写入。



chan在使用make初始化时可附带一个可选参数来设置缓冲区。默认无缓冲，题目中便初始化的是无缓冲区的chan，这样只有写入的元素直到被读取后才能继续写入，不然就一直阻塞。

设置缓冲区大小后，写入数据时可连续写入到缓冲区中，直到缓冲区被占满。从chan中接收一次便可从缓冲区中释放一次。可以理解为chan是可以设置吞吐量的处理池。



> `ch := make(chan interface{}) `和 `ch := make(chan interface{},1)`是不一样的
无缓冲的 不仅仅是只能向 `ch` 通道放 一个值 而是一直要有人接收，那么`ch <- elem`才会继续下去，要不然就一直阻塞着，也就是说有接收者才去放，没有接收者就阻塞。

> 而缓冲为`1`则即使没有接收者也不会阻塞，因为缓冲大小是`1`只有当 放第二个值的时候 第一个还没被人拿走，这时候才会阻塞 



### 10. 以下代码能编译过去吗？为什么？

```
package main
import (
	"fmt"
)
type People interface {
	Speak(string) string
}
type Stduent struct{}
func (stu *Stduent) Speak(think string) (talk string) {
	if think == "bitch" {
		talk = "You are a good boy"
	} else {
		talk = "hi"
	}
	return
}
func main() {
	var peo People = Stduent{}
	think := "bitch"
	fmt.Println(peo.Speak(think))
}
```


**结论：**编译失败，值类型 `Student{}` 未实现接口`People`的方法，不能定义为 `People` 类型。

**两种正确修改方法：**

- 方法一

```
package main
import (
	"fmt"
)
type People interface {
	Speak(string) string
}

type Stduent struct{}

func (stu Stduent) Speak(think string) (talk string) {
	if think == "bitch" {
		talk = "You are a good boy"
	} else {
		talk = "hi"
	}
	return
}
func main() {
	var peo People = Stduent{}
	think := "hi"
	fmt.Println(peo.Speak(think))
}
```

- 方法二

```
package main
import (
	"fmt"
)
type People interface {
	Speak(string) string
}

type Stduent struct{}

func (stu Stduent) Speak(think string) (talk string) {
	if think == "bitch" {
		talk = "You are a good boy"
	} else {
		talk = "hi"
	}
	return
}
func main() {
	var peo People = &Stduent{}
	think := "bitch"
	fmt.Println(peo.Speak(think))
}
```

**总结：**指针类型的结构体对象可以同时调用结构体值类型和指针类型对应的方法。而值类型的结构体对象只能调用值类型对应的接口方法。

### 关注区块链部落，实时获取最新技术文章

![](http://om1c35wrq.bkt.clouddn.com/%E5%8C%BA%E5%9D%97%E9%93%BE%E9%83%A8%E8%90%BD-1.jpg)










