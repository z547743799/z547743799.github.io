---
layout: post
title:  "golang实现简易版的raft"
date:   2018-05-25
excerpt: "golang实现简易版的raft"
description: "golang实现简易版的raft"
tag:
- golang
- raft
comments: true
---

# golang实现简易版的raft

```go
package main

import (
	"os"
	"strconv"
	"fmt"
	"sync"
	"net/rpc"
	"net/http"
	"math/rand"
	"time"
	"log"
)
//设置节点个数
const raftCount  = 3
//设置存储leader类型的变量AppendEntriesArsg
type AppendEntriesArgs struct {
	//任期
	Term int
	//leader 编号
	LeaderId int
}
//初始化存储leader的类型变量
var args = AppendEntriesArgs{0,-1}
//存储缓存信息
var bufferMessage = make(map[string]string)
//处理数据库信息
var mysqlMessage = make(map[string]string)
//操作消息数组下标
var messageId =1
//用nodeTable存储每个节点中的id和port
var nodeTable map[string]string
//对每个节点地址和端口的封装成结构体
type nodeInfo struct {
	id string
	port string
}
func main() {
	if len(os.Args)>1 {
		//接收终端输入的信息
		userId:=os.Args[1]
		//字符串转int
		id,_:=strconv.Atoi(userId)
		fmt.Println(id)
		//设置node id 和端口号 和总量
		nodeTable = map[string]string {
			"1":":1200",
			"2":":1201",
			"3":":1212",
		}
		//将当前node的信息封装成结构体nodeInfo==node
		node:=nodeInfo{id:userId,port:nodeTable[userId]}
		//创建Raft结构体指针并传入几号服务器
		rf:=Make(id)
		//设置Raft结构体指针的node变量值是信息是当前node的信息 -ip -端口
		rf.node = node
		//注册rpc
		go func() {
			//注册rpc，为了实现远程链接
			//调用开启端口监听传入当前node的端口-port
			rf.raftRegisterRPC(node.port)
		}()
		if userId == "1" {
			go func() {
				//接受浏览器发送的请求
				http.HandleFunc("/req", rf.getRequest)
				fmt.Println("监听5000")
				if err := http.ListenAndServe(":5000", nil); err != nil {
					fmt.Println(err)
					return
				}
			}()
		}
	}
	for{;}
}
var clientWriter http.ResponseWriter

func (rf *Raft)getRequest(writer http.ResponseWriter, request *http.Request) {

	fmt.Println("come")
	request.ParseForm()
	if len(request.Form["age"]) > 0 {
		clientWriter = writer
		fmt.Println("1.主节点广播客户端请求\age:", request.Form["age"][0])

		param := Param{Msg:request.Form["age"][0], MsgId:strconv.Itoa(messageId)}
		messageId++
		//主节点广播请求
		if args.LeaderId == rf.me {
			rf.sendMessageToOtherNodes(param)
		} else {
			//将消息转发给leader
			leaderId := nodeTable[strconv.Itoa(args.LeaderId)]
			//连接远程rpc服务
			rpc, err := rpc.DialHTTP("tcp", "127.0.0.1"+leaderId)
			if err != nil {
				log.Fatal("\nrpc转发连接server错误:",args.LeaderId, err)
			}
			var bo bool = false
			//首先geileader传递
			err = rpc.Call("Raft.ForwardingMessage", param, &bo)
			if err != nil {
				log.Fatal("\nrpc转发调用server错误:",args.LeaderId, err)
			}
		}
	}
}
//开始广播
func (rf *Raft) sendMessageToOtherNodes(param Param) {
	bufferMessage[param.MsgId] = param.Msg
	// 只有领导才能给其它服务器发送消息
	if rf.currentLeader == rf.me {
		var success_count int = 0
		fmt.Printf("领导者发送数据中 。。。\n")
		go func() {
			//向从节点发送日志并让从节点存储
			rf.broadcast(param, "Raft.LogDataCopy", func(ok bool) {
				//回调函数
				//需要其它服务端回应
				rf.message <- ok
			})
		}()
		for i := 0; i < raftCount-1; i++ {

			fmt.Println("等待其它服务端回应")
			select {
			case ok := <-rf.message:
				if ok {
					success_count++
					if success_count >= raftCount/2 {
						rf.mu.Lock()
						rf.lastMessageTime = milliseconds()
						//模模拟向数据库存储日志
						mysqlMessage[param.MsgId] = bufferMessage[param.MsgId]
						delete(bufferMessage, param.MsgId)
						if clientWriter != nil {
							fmt.Fprintf(clientWriter, "OK")
						}
						fmt.Printf("\n领导者发送数据结束\n")
						rf.mu.Unlock()
					}
				}
			}
		}
	}
}
//注册Raft对象，注册后的目的为确保每个节点（raft) 可以远程接收
func (node *Raft) raftRegisterRPC(port string) {
	//注册一个服务器
	rpc.Register(node)
	//把服务绑定到http协议上
	rpc.HandleHTTP()
	err :=http.ListenAndServe(port,nil)
	if err!=nil {
		fmt.Println("注册rpc服务的错",err)
	}
}
//创建节点对象
func Make(me int) *Raft{
	//创建Raft结构体指针
	rf:=&Raft{}
	rf.me = me
	rf.votedFor = -1
	rf.state = 0//0 follower ,1 candidate ,2 leader
	rf.timeout = 0
	rf.currentLeader = -1
	rf.setTerm(0)
	//创建通道对象
	rf.message = make(chan  bool)
	rf.heartbeat = make(chan bool)
	rf.heartbeatRe = make(chan bool)
	rf.eclectCh = make(chan bool)

	//每个节点都有选举权
	go rf.election()
	//每个节点都有心跳功能
	go rf.sendLeaderHeartBeat()

	return rf

}
//选举成功后，应该广播所有的节点，我成为了leader
func (rf *Raft)sendLeaderHeartBeat() {
	for {
		select{
		//
		case<-rf.heartbeat:
			rf.sendAppendEntriesImpl()
		}
	}
}
//向所有节点发送广播
func (rf *Raft)sendAppendEntriesImpl() {
	if rf.currentLeader == rf.me {
		var success_count = 0
		go func() {
			param:=Param{Msg:"leader heartbeat",
			//设置leader信息
			Arg:AppendEntriesArgs{rf.currentTerm,rf.me}}
			//
			rf.broadcast(param,"Raft.Heartbeat",func(ok bool){
					//回调函数
					rf.heartbeatRe<-ok
			})
		}()

		for i:=0;i<raftCount-1;i++{
			select{
			case ok:=<-rf.heartbeatRe:
				if ok {
					success_count++
					if success_count>=raftCount/2 {
						rf.mu.Lock()
						rf.lastMessageTime = milliseconds()
						fmt.Println("接收到了子节点们的返回信息")
						rf.mu.Unlock()
					}
				}
			}
		}
	}
}
func randomRange(min,max int64) int64 {
	//设置随机时间
	rand.Seed(time.Now().UnixNano())
	return rand.Int63n(max-min)+min
}
//获得当前时间（毫秒）
func milliseconds() int64 {
	return time.Now().UnixNano()/int64(time.Millisecond)
}
//节点开始选举
func(rf *Raft)election() {
	//控制跟随是否继续进行选举
	var result bool
	//每隔一段时间发一次心跳
	for {
		//延时时间
		timeout:= randomRange(1500,3000)
		//设置该节点最有一次处理消息的时间
		rf.lastMessageTime = milliseconds()

		select {
		//间隔时间为1500-3000ms的随机值
		case <-time.After(time.Duration(timeout)*time.Millisecond):
		}
        //初始化result值
		result=false
		for !result {
			//开始选择leader
			result = rf.election_one_round(&args)
		}
	}
}
//开始选举领导人
func (rf *Raft)election_one_round(args *AppendEntriesArgs) bool {
	//已经有了leader，并且不是自己，那么return
	if args.LeaderId > -1 && args.LeaderId != rf.me {
		fmt.Printf("%d已是leader，终止%d选举\n", args.LeaderId, rf.me)
		return true
	}
    //设置超时时间
	var timeout int64
	//判断投票节点的多少
	var done int
    //无用
	var triggerHeartbeat bool
	//设置超时时间
	timeout = 2000
	//设置last为当前时间
	last := milliseconds()
	//判断是否进行领领导人的确认
	success := false
	rf.mu.Lock()
	rf.becomeCandidate()
	rf.mu.Unlock()
	//打印当前的node为id
	fmt.Printf("候选人=%d 开始 选举 领导人\n", rf.me)
	//开始 选举 领导人
	for {
		//5.24 pm
		//printTime()
		fmt.Printf("候选人=%d 发送请求其他server投票\n", rf.me)
		//启动线程-候选人发送请求其他server投票
		go func() {

			rf.broadcast(Param{Msg:"求其他server投票"}, "Raft.ElectingLeader", func(ok bool) {
				//回调函数:
				//使在broadcast函数中传入fun(ok bool)中传入ok
				// 然后在返回到本函数将ok传入到选举通道
				rf.eclectCh <- ok
			})
		}()
        //初始化done的值
		done = 0
		triggerHeartbeat = false
		for i := 0; i < raftCount-1; i++ {
			//printTime()
			fmt.Printf("候选人=%d 等待其他服务器选择=%d\n", rf.me, i)
             //等待其他服务器的选择
			select {
			case ok := <-rf.eclectCh:
				if ok {
					done++
					//大于当前节点的一半设置success为ture
					success = done >= raftCount/2 || rf.currentLeader > -1
					//为true时确认当前候选人
					if success && !triggerHeartbeat {
						fmt.Println("okok", args)
						triggerHeartbeat = true
						rf.mu.Lock()
						rf.becomeLeader()
						args.Term = rf.currentTerm+1
						args.LeaderId = rf.me
						rf.mu.Unlock()
						//printTime()
						fmt.Printf("候选人=%d 成为领袖\n", rf.currentLeader)
						//设置开启向所有节点发送广播
						rf.heartbeat <- true
					}
				}
			}
			//printTime()
			fmt.Printf("候选人=%d 完成我的选择=%d\n", rf.me, i)
		}
		if (timeout < milliseconds()-last) || (done >= raftCount/2 || rf.currentLeader > -1) {
			break
		} else {
			select {
			case <-time.After(time.Duration(5000) * time.Millisecond):
			}
		}
	}
	//printTime()
	fmt.Printf("候选人=%d 都到票的状态=%t\n", rf.me, success)
	return success
}
//确认当前领导人
func (rf *Raft)becomeLeader() {
	rf.state = 2
	fmt.Println(rf.me,"成为了leader")
	rf.currentLeader = rf.me
}
//设置发送参数的数据类型
type Param struct{
	Msg string
	MsgId string
	Arg AppendEntriesArgs
}
//请求其他server为自己投票
func(rf *Raft)broadcast(msg Param,path string ,f func(ok bool)){
	//设置不要自己给自己广播
	for nodeID,port :=range nodeTable {
		if nodeID == rf.node.id {
			continue;
		}
		//链接远程rpc
		rp,err:=rpc.DialHTTP("tcp","127.0.0.1"+port)
		if err!=nil {
			f(false)
			continue
		}
		var bo = false
		err = rp.Call(path,msg,&bo)
		if err!= nil {
			f(false)
			continue
		}
		f(bo)
	}
}
//设置本机服务器node为candidate候选人状态
func (rf *Raft)becomeCandidate() {
	//如果当前状态为跟随者状态或没有领导人
	if rf.state ==0 || rf.currentLeader == -1 {
		rf.state = 1//候选人状态
		rf.votedFor = rf.me//为自己投一票
		rf.setTerm(rf.currentTerm+1)//领导人
		rf.currentLeader = -1//设置当前领导人为-1
	}
}
//设置当前领导期数
func(rf *Raft)setTerm(term int){
	rf.currentTerm = term
}
//声明节点对象类型Raft
type Raft struct {
	//当前节点信息
	node nodeInfo
	//线程所
	mu sync.Mutex
	//当前节点编号
	me int
	//当前领导期数
	currentTerm int
	//为哪个节点投票
	votedFor int
	//当前节点状态
	state int
	//超时时间
	timeout int
	//设置当前节点的领导
	currentLeader int
	//该节点最后一次处理数据的时间
	lastMessageTime int64
	//节点间发送消息的通道
	message chan bool
	//选举通道
	eclectCh chan bool
	//心跳信号通道
	heartbeat chan bool
	//子节点给主节点返回心跳信号
	heartbeatRe chan bool
}
//Rpc处理
func (rf *Raft)ElectingLeader(param Param,a *bool) error {
	//给leader投票
	*a = true
	rf.lastMessageTime = milliseconds()
	return  nil
}
func (rf *Raft)Heartbeat(param Param,a *bool)error {
	fmt.Println("\nrpc:heartbeat:",rf.me,param.Msg)
	if param.Arg.Term < rf.currentTerm {
		*a = false
	} else {
		args = param.Arg
		fmt.Printf("%d收到leader%d心跳\n", rf.currentLeader,args.LeaderId)
		*a = true
		rf.mu.Lock()
		rf.currentLeader = args.LeaderId
		rf.votedFor = args.LeaderId
		rf.state = 0
		rf.lastMessageTime = milliseconds()
		fmt.Printf("server = %d learned that leader = %d\n", rf.me, rf.currentLeader)
		rf.mu.Unlock()
	}
	return nil
}
//1，连接到leader节点
//从节点向leader节点做转发
func (rf *Raft) ForwardingMessage(param Param, a *bool) error {	fmt.Println("\nrpc:forwardingMessage:",rf.me,param.Msg)
	rf.sendMessageToOtherNodes(param)
	*a = true
	rf.lastMessageTime = milliseconds()
	return nil
}
//接收leader传过来的日志
func (r *Raft) LogDataCopy(param Param, a *bool) error {
	fmt.Println("\nrpc:LogDataCopy:",r.me,param.Msg)
	bufferMessage[param.MsgId] = param.Msg
	*a = true
	return nil
}
```