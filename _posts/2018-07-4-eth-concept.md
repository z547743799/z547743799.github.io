---
layout: post
title:  "eth-concept"
date:   2018-06-24
excerpt: "eth-concept"
description: "eth-concept"
tag:
- eth
comments: true
---


## 以太坊概念集

>### 以太坊是图灵完备的交易状态机

>### 账户模型
eth 账户模型和UTXO 模型的区别 
这张图话的不是很好
![](http://p8am46xs9.bkt.clouddn.com/18-7-7/60281399.jpg)
 以太坊的账户模型没有 utxo复杂
  如果让我比喻的话
 以太坊的钱就像 泥巴是可以拆分和组合的
 但是utxo就不行 utxo很复杂
 简单说utxo有两个属性一个是intput ,和output
 output 就是钱 intput 就是记录已花费的钱
 如果一个账户中intput所记录的output 是已花费的钱剩下的output就是可以用的钱
 output是不可拆分的就像10块钱不能拆成7块和3块一样
 
           
>#### [MPT( Merkle Patricia Tree)](https://yangchenglong11.github.io/2018/03/16/merkle-patricia-tree/) 
 
>Merkle Patricia Tree（又称为MPT）是一种经过改良的、融合了默克尔树和前缀树两种树结构优点的数据结构，是以太坊中用来组织管理账户数据、生成交易集合哈希的重要数据结构。
mpt是 Merkle tree(默克树)和Patricla tree(前缀树)的结合
并在前缀树的基础上做了改进


>树的深度是有限制的，即使考虑攻击者会故意地制造一些交易，使得这颗树尽可能地深。不然，攻击者可以通过操纵树的深度，执行拒绝服务攻击（DOS attack），使得更新变得极其缓慢。
树的根只取决于数据，和其中的更新顺序无关。换个顺序进行更新，甚至重新从头计算树，并不会改变根。
而帕特里夏树，简单地说，或许最接近的解释是，我们可以同时实现所有的这些特性。其工作原理，最为简单的解释是，一个以编码形式存储到记录树的“路径”的值。每个节点会有16个子（children），所以路径是由十六进制编码来确定的：例如，狗（dog）的键的编码为 6 4 6 15 6 7，所以你会从这个根开始，下降到第六个子，然后到第四个，并依次类推，直到你达到终点。在实践中，当树稀少时也会有一些额外的优化，我们会使过程更为有效，但这是基本的原则。
>### [kad(Kademlia)](https://www.cnblogs.com/blockchain/p/7943962.html)
以太坊底层分布式网络即P2P网络，使用了经典的Kademlia网络，简称kad。
Kademlia在2002年由美国纽约大学的PetarP.Manmounkov和DavidMazieres提出，是一种分布式散列表(DHT)技术，以异或运算为距离度量基础，已经在BitTorrent、BitComet、Emule等软件中得到应用。
Kad的路由表是通过称为K桶的数据构造而成，K桶记录了节点NodeId，distance，endpoint，ip等信息。以太坊K桶按照与target节点距离进行排序，共256个K桶，每个K桶包含16个节点。
z
>### [布隆过滤器](https://baike.baidu.com/item/布隆过滤器/5384697?fr=aladdin)
布隆过滤器：由Howard Bloom 在 1970 年提出的二进制向量数据结构，它具有很好的空间和时间效率，被用来检测一个元素是不是集合中的一个成员。（百度百科)


>#### [rlp](https://www.jianshu.com/p/da638d0fcea4)
RLP(Recursive Length Prefix)，叫递归长度前缀编码，它是以太坊序列化所采用的编码方式。RLP主要用于以太坊中数据的网络传输和持久化存储。
RLP实际只给以下两种类型数据编码：

 >* byte数组
> * byte数组的数组，称之为列表
>#### [软分叉和硬分叉](https://zhuanlan.zhihu.com/p/28300379)
区块链版本更新就会产生软分叉和硬分叉
软分叉是向后兼容
硬分叉是只向前兼容

>#### [GHOST(幽灵协议)](https://eprint.iacr.org/2013/881.pdf)
简单来说，GHOST协议就是让我们必须选择一个在其上完成计算最多的路径。一个方法确定路径就是使用最近一个区块（叶子区块）的区块号，区块号代表着当前路径上总的区块数（不包含创世纪区块）。区块号越大，路径就会越长，就说明越多的挖矿算力被消耗在此路径上以达到叶子区块。使用这种推理就可以允许我们赞同当前状态的规范版本。

>#### [web3.js](http://ethdocs.org/en/latest/connecting-to-clients/web3.js/)
为了让你的Ðapp运行上以太坊，一种选择是使用web3.js library提供的web3。对象。底层实现上，它通过RPC 调用与本地节点通信。web3.js可以与任何暴露了RPC接口的以太坊节点连接。
>#### [solidity](https://solidity.readthedocs.io/en/v0.4.24)
solidity是一门静态语音
是由以太坊cto:Gavin Wood发明
与js不同
 
>### [uncle(过时区块)](https://www.jianshu.com/p/b567a987241a)
A挖出区块后广播途中，B也挖出了区块（过时区块），此时区块链会出现分叉。过时分叉上的区块就叫uncle区块。它不是这个块的父区块，父区块的兄弟区块（平级关系）。

>#### [Whisper(私语)](http://qjpcpu.github.io/blog/2018/02/07/shen-ru-ethereumyuan-ma-whisperxie-yi-jie-du/)
whisper是完全基于ID的消息系统,它的设计目的是形成一套p2p节点间的异步广播系统。whisper网络上的消息是加密传送的,完全可以暴露在公网进行传输;此外,为了防范DDos攻击,whisper使用了proof-of-work(PoW)工作量证明提高消息发送门槛。
[p2p whisper聊天室](https://xgyopen.github.io/2018/04/20/2018-04-20-whisper-p2p-chat/)
>#### [Swarm(蜂群)](https://ethfans.org/toya/articles/279)
Swarm是一种分布式存储平台和内容分发服务 
主要存储Dapp代码与数据以及区块数据提供一个足够去中心化以及足够重复的存储
>[官网](http://swarm-gateways.net/bzz:/theswarm.eth/)


未完待更新