---
layout: post
title:  "go-RSA非对称加密-server"
date:   2018-05-16
excerpt: "go-RSA非对称加密-server"
description: "go-RSA非对称加密"
tag:
- go-RSA非对称加密
- server
comments: true
---
# go-RSA非对称加密-server

>`go服务器端没什么好说的
>
>第一个连的话是100
>
>第二个是101
>
>第三102
>.....`


```go
package main

import (
	"net"
	"fmt"
	"log"
	"encoding/pem"
	"crypto/x509"
	"crypto/rsa"
	"crypto/rand"
)
var publicKey = []byte(`-----BEGIN PUBLIC KEY-----
MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBAOHgtbakoBV283XEwaYElQvzZD3gj3pO
IVO7348N0cfhM5M0FRGJFKwByP8DThW73Pl6C93288DGlz+g5wn94/sCAwEAAQ==
-----END PUBLIC KEY-----`)
var privateKey = []byte(`-----BEGIN RSA PRIVATE KEY-----
MIICXQIBAAKBgQDfw1/P15GQzGGYvNwVmXIGGxea8Pb2wJcF7ZW7tmFdLSjOItn9
kvUsbQgS5yxx+f2sAv1ocxbPTsFdRc6yUTJdeQolDOkEzNP0B8XKm+Lxy4giwwR5
LJQTANkqe4w/d9u129bRhTu/SUzSUIr65zZ/s6TUGQD6QzKY1Y8xS+FoQQIDAQAB
AoGAbSNg7wHomORm0dWDzvEpwTqjl8nh2tZyksyf1I+PC6BEH8613k04UfPYFUg1
0F2rUaOfr7s6q+BwxaqPtz+NPUotMjeVrEmmYM4rrYkrnd0lRiAxmkQUBlLrCBiF
u+bluDkHXF7+TUfJm4AZAvbtR2wO5DUAOZ244FfJueYyZHECQQD+V5/WrgKkBlYy
XhioQBXff7TLCrmMlUziJcQ295kIn8n1GaKzunJkhreoMbiRe0hpIIgPYb9E57tT
/mP/MoYtAkEA4Ti6XiOXgxzV5gcB+fhJyb8PJCVkgP2wg0OQp2DKPp+5xsmRuUXv
720oExv92jv6X65x631VGjDmfJNb99wq5QJBAMSHUKrBqqizfMdOjh7z5fLc6wY5
M0a91rqoFAWlLErNrXAGbwIRf3LN5fvA76z6ZelViczY6sKDjOxKFVqL38ECQG0S
pxdOT2M9BM45GJjxyPJ+qBuOTGU391Mq1pRpCKlZe4QtPHioyTGAAMd4Z/FX2MKb
3in48c0UX5t3VjPsmY0CQQCc1jmEoB83JmTHYByvDpc8kzsD8+GmiPVrausrjj4p
y2DQpGmUic2zqCxl6qXMpBGtFEhrUbKhOiVOJbRNGvWW
-----END RSA PRIVATE KEY-----`)


//加密函数
func RsaEncrypt(origData [] byte) [] byte {
	//公钥加密
	block ,_:= pem.Decode(publicKey)
	//解析公钥
	pubInterface ,_:=x509.ParsePKIXPublicKey(block.Bytes)
	//设置刚才公钥为public key 类型断言
	pub:= pubInterface.(*rsa.PublicKey)
	//加密[]byte("hello world")明文
	bts,_:=rsa.EncryptPKCS1v15(rand.Reader,pub,origData)
	return  bts
}

//解密函数
func RsaDecrypt(origData []byte) [] byte {
	//通过私钥解密
	block ,_:=pem.Decode(privateKey)
	//解析私钥
	priv,_:=x509.ParsePKCS1PrivateKey(block.Bytes)
	//解码
	bts,_:=rsa.DecryptPKCS1v15(rand.Reader,priv,origData)
	return  bts

}

func main() {
	//1.提供服务器地址
	tcpAddr, _ := net.ResolveTCPAddr("tcp4", "10.0.154.250:9527")
	//2.连接
	tcpConn, _ := net.DialTCP("tcp", nil, tcpAddr)
	defer tcpConn.Close()
	//*TCPConn
	//交互数据：
	handleData(tcpConn)
}


func handleData(tcpConn *net.TCPConn){

	overChan := make(chan  bool)
	go func() {
		for {
			//读取服务器

			bs := make([] byte, 512)
			n,_:= tcpConn.Read(bs) //阻塞式
			message := string(bs[:n])
			if n == 0{
				log.Fatal("客户端即将结束。。")
			}
			fmt.Println("服务器说：", message)
			var lin = RsaDecrypt([]byte(message))
			fmt.Println(string(lin))
		}
	}()

	go func() {
		for {
			//读取键盘

			line := ""
			id:=""
			fmt.Scanln(&id) //键盘输入，阻塞的
			fmt.Scanln(&line) //键盘输入，阻塞的
			 
			fmt.Println("输入了：", line)

			//var ss = RsaEncrypt([]byte(line))
			//写给服务器
			var lin =RsaEncrypt([]byte(line))
			ssd:=fmt.Sprintf("%s:%s",id,lin)
			ss:=string(id)+":"+string(lin)
			tcpConn.Write([]byte(ssd))
			fmt.Println(ss)
			if line == "over"{
				log.Fatal("客户端即将结束。。")
			}
		}
	}()

	<- overChan
}

```