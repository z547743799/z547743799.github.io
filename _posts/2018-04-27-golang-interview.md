---
layout: post
title: "Golang经典笔试题及答案（下篇）"
date: 2018-04-27
description: "Golang经典笔试题及答案（下篇）Golang经典笔试题及答案（下篇）Golang经典笔试题及答案（下篇）Golang经典笔试题及答案（下篇）Golang经典笔试题及答案（下篇）Golang经典笔试题及答案（下篇）Golang经典笔试题及答案（下篇）"
tag: 笔试题
keywords: "golang经典笔试题及答案 "
---

> 整理：黎跃春
> 
> 博客：http://liyuechun.org
> 
> 官网：http://kongyixueyuan.com
> 
> 备注：如有错误，请指正，不断更新迭代


### 1. 下面代码能运行吗？为什么

```
type Param map[string]interface{}

type Show struct {
	Param
}

func main1() {
	s := new(Show)
	s.Param["RMB"] = 10000
}
```

**运行结果：**

```
panic: assignment to entry in nil map

goroutine 1 [running]:
main.main()
```

如上所示，运行过程中会发生异常，原因是因为字典`Param`的默认值为`nil`，当给字典`nil`增加键值对是就会发生运行时错误`panic: assignment to entry in nil map`。

**正确的修改方案如下：**

```
package main

import "fmt"

type Param map[string]interface{}

type Show struct {
	Param
}

func main() {

	// 创建Show结构体对象
	s := new(Show)
	// 为字典Param赋初始值
	s.Param = Param{}
	// 修改键值对
	s.Param["RMB"] = 10000
	fmt.Println(s)
}
```

**运行结果如下：**

```
&{map[RMB:10000]}

Process finished with exit code 0
```

### 2. 请说出下面代码存在什么问题

```
type student struct {
	Name string
}

func f(v interface{}) {
	switch msg := v.(type) {
    	case *student, student:
    		msg.Name
	}
}
```

**有两个问题：**

1. 问题一：`interface{}`是一个没有声明任何方法的接口。
2. 问题二：`Name`是一个属性，而不是方法，`interface{}`类型的变量无法调用属性。


### 3. 写出打印的结果。

```
type People struct {
	name string `json:"name"`
}

func main() {
	js := `{
		"name":"11"
	}`
	var p People
	err := json.Unmarshal([]byte(js), &p)
	if err != nil {
		fmt.Println("err: ", err)
		return
	}
	fmt.Println("people: ", p)
}
```

**输出内容如下：**

```
people:  {}
```


**p中的属性值为空的原因是因为，name的首字母小写，修改成大写，重新运行即可。**

```
package main

import (
	"encoding/json"
	"fmt"
)

type People struct {
	Name string `json:"name"`
}

func main() {
	js := `{
        "name":"11"
    }`
	var p People
	err := json.Unmarshal([]byte(js), &p)
	if err != nil {
		fmt.Println("err: ", err)
		return
	}
	fmt.Println("people: ", p)
}
```

**运行结果如下：**

```
people:  {11}

Process finished with exit code 0
```


### 4. 下面的代码是有问题的，请说明原因。


```
package main

import "fmt"

type People struct {
	Name string
}

func (p *People) String() string {
	return fmt.Sprintf("print: %v", p)
}

func main() {
	p := &People{}
	p.String()
}

```

**运行结果如下：**

```
runtime: goroutine stack exceeds 1000000000-byte limit
fatal error: stack overflow

runtime stack:
runtime.throw(0x10c122b, 0xe)
```

**如下所示，上面的代码出现了栈溢出，原因是因为%v格式化字符串是本身会调用String()方法，上面的栈溢出是因为无限递归所致。**



### 5. 请找出下面代码的问题所在。

```
package main

import (
	"fmt"
	"time"
)

func main() {
	ch := make(chan int, 1000)
	go func() {
		for i := 0; i < 10; i++ {
			ch <- i
		}
	}()
	go func() {
		for {
			a, ok := <-ch
			if !ok {
				fmt.Println("close")
				return
			}
			fmt.Println("a: ", a)
		}
	}()
	close(ch)
	fmt.Println("ok")
	time.Sleep(time.Second * 100)
}
```

**运行结果如下：**

```
panic: send on closed channel
ok

goroutine 5 [running]:
main.main.func1(0xc420098000)
```

**解析：出现上面错误的原因是因为提前关闭通道所致。**

**正确代码如下：**

```
package main

import (
	"fmt"
	"time"
)

func main() {
	// 创建一个缓冲通道
	ch := make(chan int, 1000)

	go func() {
		for i := 0; i < 10; i++ {
			ch <- i
		}
	}()

	go func() {
		for i := 0; i < 10; i++ {
			a, ok := <-ch
			
			if !ok {
				fmt.Println("close")
				close(ch)
				return
			}
			fmt.Println("a: ", a)
		}
	}()

	fmt.Println("ok")
	time.Sleep(time.Second)
}
```

**运行结果如下：**

```
ok
a:  0
a:  1
a:  2
a:  3
a:  4
a:  5
a:  6
a:  7
a:  8
a:  9
```

### 6. 请说明下面代码书写是否正确。

```
var value int32

func SetValue(delta int32) {
	for {
		v := value
		if atomic.CompareAndSwapInt32(&value, v(v+delta)) {
			break
		}
	}
}
```

**`atomic.CompareAndSwapInt32`里面一共三个参数，上面的书写错误，正确的书写是：`atomic.CompareAndSwapInt32(&value, v,v+delta)`**


- `func CompareAndSwapInt32(addr *int32, old, new int32) (swapped bool)`

- 第一个参数的值应该是指向被操作值的指针值。该值的类型即为`*int32`。
- 后两个参数的类型都是`int32`类型。它们的值应该分别代表被操作值的旧值和新值
- `CompareAndSwapInt32·函数在被调用之后会先判断参数`addr`指向的被操作值与参数`old`的值是否相等。
- 仅当此判断得到肯定的结果之后，该函数才会用参数`new`代表的新值替换掉原先的旧值。否则，后面的替换操作就会被忽略。

**完整代码如下：**

```
package main

import (
	"sync/atomic"
	"fmt"
)

var value int32

func SetValue(delta int32) {
	for {
		v := value
		// 比较并交换

		if atomic.CompareAndSwapInt32(&value, v,v+delta) {
			fmt.Println(value)
			break
		}
	}
}

func main()  {
	SetValue(100)
}
```

**运行结果为`100`.**

### 7. 下面的程序运行后为什么会爆异常。

```
package main

import (
	"fmt"
	"time"
)

type Project struct{}

func (p *Project) deferError() {
	if err := recover(); err != nil {
		fmt.Println("recover: ", err)
	}
}

func (p *Project) exec(msgchan chan interface{}) {
	for msg := range msgchan {
		m := msg.(int)
		fmt.Println("msg: ", m)
	}
}

func (p *Project) run(msgchan chan interface{}) {
	for {
		defer p.deferError()
		go p.exec(msgchan)
		time.Sleep(time.Second * 2)
	}
}

func (p *Project) Main() {
	a := make(chan interface{}, 100)
	go p.run(a)
	go func() {
		for {
			a <- "1"
			time.Sleep(time.Second)
		}
	}()
	time.Sleep(time.Second * 100)
}

func main() {
	p := new(Project)
	p.Main()
}
```

**运行结果如下：**

```
panic: interface conversion: interface {} is string, not int

goroutine 17 [running]:
main.(*Project).exec(0x1157c08, 0xc420068060)
```

**出现异常的原因是因为写入到管道的数据类型为`string`,而`m := msg.(int)`这句代码里面却使用了`int`，修改方法，将`int`修改为`string`即可。**

**完整正确代码如下：**

```
package main

import (
	"fmt"
	"time"
)

type Project struct{}

func (p *Project) deferError() {
	if err := recover(); err != nil {
		fmt.Println("recover: ", err)
	}
}

func (p *Project) exec(msgchan chan interface{}) {
	for msg := range msgchan {
		m := msg.(string)
		fmt.Println("msg: ", m)
	}
}

func (p *Project) run(msgchan chan interface{}) {
	for {
		defer p.deferError()
		go p.exec(msgchan)
		time.Sleep(time.Second * 2)
	}
}

func (p *Project) Main() {
	a := make(chan interface{}, 100)
	go p.run(a)
	go func() {
		for {
			a <- "1"
			time.Sleep(time.Second)
		}
	}()
	time.Sleep(time.Second * 100)
}

func main() {
	p := new(Project)
	p.Main()
}
```

**运行结果如下：**

```
msg:  1
msg:  1
msg:  1
.
.
.
msg:  1
msg:  1
msg:  1
msg:  1
msg:  1
```

### 8. 请说出下面代码哪里写错了。

```
func main() {
	abc := make(chan int, 1000)
	for i := 0; i < 10; i++ {
		abc <- i
	}
	go func() {
		for {
			a := <-abc
			fmt.Println("a: ", a)
		}
	}()
	close(abc)
	fmt.Println("close")
	time.Sleep(time.Second * 100)
}
```

`go中的for循环是死循环，应该设置出口。正确代码如下：`

```
package main

import (
	"fmt"
	"time"
)

func main() {
	abc := make(chan int, 1000)
	for i := 0; i < 10; i++ {
		abc <- i
	}
	go func() {
		for {
			a,ok := <-abc
			if !ok {
				fmt.Println("结束！")
				return
			}
			fmt.Println("a: ", a)
		}
	}()
	close(abc)
	fmt.Println("close")
	time.Sleep(time.Second * 100)
}
```

**运行结果为：**

```
close
a:  0
a:  1
a:  2
a:  3
a:  4
a:  5
a:  6
a:  7
a:  8
a:  9
结束！
```

### 9. 请说出下面代码，执行时为什么会报错

```
type Student struct {
	name string
}

func main() {
	m := map[string]Student{"people": {"liyuechun"}}
	m["people"].name = "wuyanzu"
}
```

**答案：报错的原因是因为不能修改字典中`value`为结构体的属性值。**

**代码作如下修改方可运行：**

```
package main

import "fmt"

type Student struct {
	name string
}

func main() {
	m := map[string]Student{"people": {"liyuechun"}}
	fmt.Println(m)
	fmt.Println(m["people"])

	// 不能修改字典中结构体属性的值
	//m["people"].name = "wuyanzu"
	
	var s Student = m["people"] //深拷贝
	s.name = "xietingfeng"
	fmt.Println(m)
	fmt.Println(s)
}
```

**运行结果如下：**

```
map[people:{liyuechun}]
{liyuechun}
map[people:{liyuechun}]
{wuyanzu}
```






### 10. 请说出下面的代码存在什么问题

```
type query func(string) string

func exec(name string, vs ...query) string {
	ch := make(chan string)
	fn := func(i int) {
		ch <- vs[i](name)
	}
	for i, _ := range vs {
		go fn(i)
	}
	return <-ch
}

func main() {
	ret := exec("111", func(n string) string {
		return n + "func1"
	}, func(n string) string {
		return n + "func2"
	}, func(n string) string {
		return n + "func3"
	}, func(n string) string {
		return n + "func4"
	})
	fmt.Println(ret)
}
```

**`return <-ch`**之执行一次，所以不管传入多少`query`函数，都只是读取最先执行完的`query`。

### 关注区块链部落，实时获取最新技术文章

![](http://om1c35wrq.bkt.clouddn.com/%E5%8C%BA%E5%9D%97%E9%93%BE%E9%83%A8%E8%90%BD-1.jpg)










