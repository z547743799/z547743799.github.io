---
layout: post
title: "第二篇 - EOS Currency 合约案例"
date: 2018-04-13
description: "目前来讲，任何学习`EOS`相关技术的资料都来自[https://github.com/EOSIO/eos](https://github.com/EOSIO/eos)，可能大家在搭建`EOS`开发环境的过程中，会很迷茫，网上资料都很乱，自己也理不清头绪，不知如何下手。在这里，[春哥](http://liyuechun.org)将一步步为你揭开层层面纱。"
tag: EOS
keywords: "EOS Currency 案例 MAC"
---

> 主讲人：黎跃春 | 孔壹学院创始人
> 
> 课程咨询，添加莉莉微信(kongyixueyuan)咨询

切换到`eos/build/programs/cleos`路径下面。


### 1. 钱包设置

秘钥需要保存好，后面解锁时会用到。

```
liyuechun:cleos yuechunli$ ./cleos wallet create
Creating wallet: default
Save password to use in the future to unlock this wallet.
Without password imported keys will not be retrievable.
"PW5HtkrzGaNKGNKVGXc6icWiWdo7PFsjWJAdE2dJbt5bSnbwb7spd"
```


### 2. 为账号部署合约

```
liyuechun:cleos yuechunli$ ./cleos set contract eosio ../../contracts/eosio.bios -p eosio
```

```
liyuechun:cleos yuechunli$ ./cleos set contract eosio ../../contracts/eosio.bios -p eosio 
Reading WAST/WASM from ../../contracts/eosio.bios/eosio.bios.wast...
Assembling WASM...
Publishing contract...
executed transaction: 32735994889238e18ba60341bb79f6b674738727e9d5996991f8f34b496e3a18  3288 bytes  2200576 cycles
#         eosio <= eosio::setcode               {"account":"eosio","vmtype":0,"vmversion":0,"code":"0061736d0100000001581060037f7e7f0060057f7e7e7e7e...
#         eosio <= eosio::setabi                {"account":"eosio","abi":{"types":[],"structs":[{"name":"set_account_limits","base":"","fields":[{"n...
liyuechun:cleos yuechunli$ 
```

### 3. 创建2个`key`，导入`key`的私钥

```
liyuechun:cleos yuechunli$ ./cleos create key
Private key: 5Kg4i6WW6mfGXGjriU552KdAQ8tQyCU4mqtevVganWWbzQf1rD1
Public key: EOS5MSkE5DGgSurc7k3Sv9kWrev6E3GBBqasdiiC3yajPwrW7c4Uq

liyuechun:cleos yuechunli$ ./cleos create key 
Private key: 5Kg2P7PRA7wWrW2s53JiaBur7PhDtDsCMZUwQ8Yvn8uAmu8xEMB
Public key: EOS7wERhooVJwqYLuQn5v6UDZnL5KQpGJBDMQoktkxz4baNzicwLX

liyuechun:cleos yuechunli$ ./cleos wallet import 5Kg4i6WW6mfGXGjriU552KdAQ8tQyCU4mqtevVganWWbzQf1rD1
imported private key for: EOS5MSkE5DGgSurc7k3Sv9kWrev6E3GBBqasdiiC3yajPwrW7c4Uq
liyuechun:cleos yuechunli$ ./cleos wallet import 5Kg2P7PRA7wWrW2s53JiaBur7PhDtDsCMZUwQ8Yvn8uAmu8xEMB
imported private key for: EOS7wERhooVJwqYLuQn5v6UDZnL5KQpGJBDMQoktkxz4baNzicwLX
liyuechun:cleos yuechunli$ 

```

### 4. 根据生成的公钥，创建帐号

```
$ liyuechun:cleos yuechunli$ ./cleos create account eosio eostoken EOS5MSkE5DGgSurc7k3Sv9kWrev6E3GBBqasdiiC3yajPwrW7c4Uq EOS7wERhooVJwqYLuQn5v6UDZnL5KQpGJBDMQoktkxz4baNzicwLX
```

```
liyuechun:cleos yuechunli$ ./cleos create account eosio eostoken EOS5MSkE5DGgSurc7k3Sv9kWrev6E3GBBqasdiiC3yajPwrW7c4Uq EOS7wERhooVJwqYLuQn5v6UDZnL5KQpGJBDMQoktkxz4baNzicwLX
executed transaction: e691042df8074a25f47b8364f58a03c2c407e64d2cbc7248e1e9a611a06fe547  352 bytes  102400 cycles
#         eosio <= eosio::newaccount            {"creator":"eosio","name":"eostoken","owner":{"threshold":1,"keys":[{"key":"EOS5MSkE5DGgSurc7k3Sv9kW...
```

### 5. 查看账号信息

```
liyuechun:cleos yuechunli$ ./cleos get account eostoken
```

```
liyuechun:cleos yuechunli$ ./cleos get account eostoken
{
  "account_name": "eostoken",
  "permissions": [{
      "perm_name": "active",
      "parent": "owner",
      "required_auth": {
        "threshold": 1,
        "keys": [{
            "key": "EOS7wERhooVJwqYLuQn5v6UDZnL5KQpGJBDMQoktkxz4baNzicwLX",
            "weight": 1
          }
        ],
        "accounts": []
      }
    },{
      "perm_name": "owner",
      "parent": "",
      "required_auth": {
        "threshold": 1,
        "keys": [{
            "key": "EOS5MSkE5DGgSurc7k3Sv9kWrev6E3GBBqasdiiC3yajPwrW7c4Uq",
            "weight": 1
          }
        ],
        "accounts": []
      }
    }
  ]
}
liyuechun:cleos yuechunli$
```


### 6. 检测，并部署合约

```
$ ./cleos get code currency
$ ./cleos set contract currency ../../contracts/currency 
$ ./cleos get code currency 
```

```
liyuechun:cleos yuechunli$ ./cleos get code currency 
code hash: 0000000000000000000000000000000000000000000000000000000000000000
liyuechun:cleos yuechunli$ ./cleos set contract currency ../../contracts/currency 
Reading WAST/WASM from ../../contracts/currency/currency.wast...
Assembling WASM...
Publishing contract...
executed transaction: eec4d42d5c1aab8785ce8e55618bf209d2e4dbcaf881e004c260c779d5571cef  7112 bytes  2200576 cycles
#         eosio <= eosio::setcode               {"account":"currency","vmtype":0,"vmversion":0,"code":"0061736d010000000199011860000060027e7e0060017...
#         eosio <= eosio::setabi                {"account":"currency","abi":{"types":[{"new_type_name":"account_name","type":"name"}],"structs":[{"n...
liyuechun:cleos yuechunli$ ./cleos get code currency 
code hash: d6c891fbdfcff597d82e17c81354574399b01d533e53d412093f03e1950fb9d4
liyuechun:cleos yuechunli$ 
```

### 7. 创建货币

```
$ ./cleos push action currency create '{"issuer":"currency","maximum_supply":"1000000.0000 CUR","can_freeze":"0","can_recall":"0","can_whitelist":"0"}' --permission currency@active
```

```
liyuechun:cleos yuechunli$ ./cleos push action currency create '{"issuer":"currency","maximum_supply":"1000000.0000 CUR","can_freeze":"0","can_recall":"0","can_whitelist":"0"}' --permission currency@active
executed transaction: ae92a14b810e1437e9a0e8ae0527268a1e0fbd834d8c0a6906c8b65d9b503dc4  248 bytes  103424 cycles
#      currency <= currency::create             {"issuer":"currency","maximum_supply":"1000000.0000 CUR","can_freeze":0,"can_recall":0,"can_whitelis...
>> create
liyuechun:cleos yuechunli$
```

### 8. 发行货币

```
$ ./cleos push action currency issue '{"to":"currency","quantity":"1000.0000 CUR","memo":""}' --permission currency@active 
```

```
liyuechun:cleos yuechunli$ ./cleos push action currency issue '{"to":"currency","quantity":"1000.0000 CUR","memo":""}' --permission currency@active 
executed transaction: 7b8108d19aba6c8dcc601c321ef3c433cd2a3aedbd3ac1b217f06477792b40f7  248 bytes  106496 cycles
#      currency <= currency::issue              {"to":"currency","quantity":"1000.0000 CUR","memo":""}
>> issue
liyuechun:cleos yuechunli$ 
```


### 9. 查看帐号信息

```
$ ./cleos get table currency currency accounts 
```

```
liyuechun:cleos yuechunli$ ./cleos get table currency currency accounts 
{
  "rows": [{
      "balance": "1000.0000 CUR",
      "frozen": 0,
      "whitelist": 1
    }
  ],
  "more": false
}
liyuechun:cleos yuechunli$ 
```

### 10. 转账

```
$ ./cleos push action currency transfer '{"from":"currency","to":"eosio","quantity":"20.0000 CUR","memo":"my first transfer"}' --permission currency@active 
$ ./cleos get table currency currency accounts
```

```
liyuechun:cleos yuechunli$ ./cleos push action currency transfer '{"from":"currency","to":"eosio","quantity":"20.0000 CUR","memo":"my first transfer"}' --permission currency@active 
executed transaction: 4a24e5a9afd17abe61dcb3b3aff06a1c5ec94663f803d3d1801bd7a5046fea9e  272 bytes  109568 cycles
#      currency <= currency::transfer           {"from":"currency","to":"eosio","quantity":"20.0000 CUR","memo":"my first transfer"}
>> transfer
#         eosio <= currency::transfer           {"from":"currency","to":"eosio","quantity":"20.0000 CUR","memo":"my first transfer"}
liyuechun:cleos yuechunli$ ./cleos get table currency currency accounts 
{
  "rows": [{
      "balance": "980.0000 CUR",
      "frozen": 0,
      "whitelist": 1
    }
  ],
  "more": false
}
```

### 11. 视频获取方式

> 关注公众，发送`eos`自动获取视频链接

![](http://om1c35wrq.bkt.clouddn.com/%E5%8C%BA%E5%9D%97%E9%93%BE%E9%83%A8%E8%90%BD-1.jpg)


