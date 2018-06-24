---
layout: post
title:  "golang常用字符串操作"
date:   2018-05-22
excerpt: "golang常用字符串操作"
description: "golang常用字符串操作"

tag:
- strings
- golang
comments: true
---
 
  golan常用字符串
----
		strings包下的关于字符串 的常用方法：
		Contains(s,substr),是否包含指定内容
		ContainsAny(s,chars),是否包含指定内容中的任意一个
		HasPrefix(),判断以指定内容开头
		HasSuffix(),判断以指定内容结尾
		Index(),搜索指定的内容，第一次出现的索引下标，如果没有返回-1
		LastIndex(),最后一次出现的索引内容，如果没有也是-1

		Count(),统计指定的内容出现的次数


		Split(s1,"")-->[]string
		SplitN(s1,"",n)-->[]string,指定次数的切割，不超过n次。如果全切n=-1
		Join([]strings,"")-->string,合并

		ToLower()
		ToUpper()

		Trim(),去除首尾的指定内容

		Replace(s,old,new,n),n替换的次数，-1表示全部替换
		Repeat(),重复的次数

#### json转对象
```go

rdr := strings.NewReader(`{"First":"James", "Last":"Bond", "Age":20}`)
json.NewDecoder(rdr).Decode(&p1)

or

bs := []byte(`{"First":"James", "Last":"Bond", "wisdom score":20}`)
	json.Unmarshal(bs, &p1)


```

#### 对象转json
```go

p1 := person{"James", "Bond", 20, 007}
	json.NewEncoder(os.Stdout).Encode(p1)
----------
对象转字符串在转json
	p1 := person{"James", "Bond", 20, 007}
	bs, _ := json.Marshal(p1)
	fmt.Println(bs)
	fmt.Printf("%T \n", bs)
	fmt.Println(string(bs))
-----------
	p1 := person{"James", "Bond", 20}
	fmt.Println(p1)
	bs, _ := json.Marshal(p1)
	fmt.Println(string(bs))


```
#### json添加别名
```go
type person struct {
	First string
  //"_"表示舍弃
    Last  string `json:"-"`

	Age   int `json:"wisdom score"`
}
```