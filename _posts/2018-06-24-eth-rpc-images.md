---
layout: post
title:  "eth rpc简单解析图"
date:   2018-06-24
excerpt: "eth rpc简单解析图"
description: "eth rpc 简单解析图"
tag:
- golang
- raft
comments: true
---

以太坊在与控制台交互时使用你的cli命令框架

 github.com/go-ethereum/vendor/gopkg.in/urfave/cli.v1

localConsole
以太坊与前端的主要交互代码是
go-ethereum/cmd/geth/consolecmd.go
```go 

 func localConsole(ctx *cli.Context) error {
	// Create and start the node based on the CLI flags
	//创建节点
	node := makeFullNode(ctx)
	//启动节点
	startNode(ctx, node)
	defer node.Stop()

	// Attach to the newly started node and start the JavaScript console
	//连接到新启动的节点并启动JavaScript控制台
	client, err := node.Attach()
	if err != nil {
		utils.Fatalf("Failed to attach to the inproc geth: %v", err)
	}
	config := console.Config{
		DataDir: utils.MakeDataDir(ctx),
		DocRoot: ctx.GlobalString(utils.JSpathFlag.Name),
		Client:  client,
		Preload: utils.MakeConsolePreloads(ctx),
	}

	console, err := console.New(config)
	if err != nil {
		utils.Fatalf("Failed to start the JavaScript console: %v", err)
	}
	defer console.Stop(false)

	// If only a short execution was requested, evaluate and return
	// 如果只请求短执行，则评估并返回

	if script := ctx.GlobalString(utils.ExecFlag.Name); script != "" {
		console.Evaluate(script)
		return nil
	}
	// Otherwise print the welcome screen and enter interactive mod
	//否则打印欢迎屏幕并输入交互式模式
	console.Welcome()
	//判断代码并进行指定的交互
	console.Interactive()

	return nil
}
```





# eth rpc  简单解析流程图

 
 ![rpc](http://p8am46xs9.bkt.clouddn.com/18-6-23/85347295.jpg )
 