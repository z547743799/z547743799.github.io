---
layout: post
title:  "KaiJu_Engine介绍"
date:   2018-05-14
excerpt: "KaiJu_Engine介绍"
description: "KaiJu_Engine介绍"
project: true
tag:
- KaiJu_Engine
comments: true
---

>### 比 Unity 快数倍
>- 二、左侧截图信息（D3D12 渲染）
>- Capturing D3D12. Frame: 9011. 1 ms (647 FPS)
>- 正在捕获 D3D12 帧。帧号：9011，单帧耗时：1 毫秒（帧率：647 FPS）
>- F12, PrtScrn to capture. 0 Captures saved.
>- 按 F12 或 PrintScreen 键捕获截图，已保存 0 张截图
>- 画面：一个带木纹纹理的 3D 立方体，展示基础 3D 渲染能力
>- 三、右侧截图信息（Vulkan 渲染）
>- Capturing Vulkan. Frame: 69440. 0.37 ms (2712 FPS)
>- 正在捕获 Vulkan 帧。帧号：69440，单帧耗时：0.37 毫秒（帧率：2712 FPS）
>- F12, PrtScrn to capture. 1 Captures saved.
>- 按 F12 或 PrintScreen 键捕获截图，已保存 1 张截图
>- Captured frame 69015.
>- 已捕获第 69015 帧
>- 画面：数独游戏的主菜单界面，包含 3D 文字与悬浮数字方块，展示 UI 与 3D 场景结合的渲染能力
>- 四、性能对比与背景解读
>
>- 表格
>
> |渲染 API|	单帧耗时|	帧率|
> |---|---|---|
> |D3D12|	1 ms|	647 FPS|
> |Vulkan|	0.37 ms|	2712 FPS|
>
>- 这是 Kaiju Engine 与 Unity 的性能对比演示：
>- Vulkan 后端性能达到 2712 FPS，是 D3D12 后端的约 4.2 倍，也远超 Unity 的常规表现
>- 体现了 Kaiju Engine 极简架构、缓存友好、Go 语言并发优化带来的极致性能优势
>- 右侧数独 Demo 同时展示了 UI 系统与 3D 渲染的高效结合







# 一
>#### A game engine that lets me make games the way I want to
>
>- 一款能让我按照自己想要的方式开发游戏的引擎
>
>强调高度自定义与开发者友好的设计理念
>
> #### Maximally multithreaded
>
>- 最大化多线程化
>
>充分利用多核 CPU，提升性能与并发处理能力
>
>#### Fast build iteration
>
>- 快速构建迭代
>
>缩短编译与部署时间，提升开发效率
>
>#### Cross platform (Windows, Linux, Mac, Android, iOS, AR, VR)
>
>- 跨平台（支持 Windows、Linux、Mac、Android、iOS、增强现实、虚拟现实）
>
>覆盖主流桌面、移动与沉浸式设备平台
>
>#### Minimal dependencies
>
>- 最小化依赖
>
>减少第三方库依赖，降低构建复杂度与潜在冲突
>
>#### Simplified editor
>
>- 简化版编辑器
>
>提供轻量、高效的开发工具，避免过度冗余
>
>#### Vulkan - 3D/2D
>
>- 基于 Vulkan 图形 API，支持 3D/2D 渲染
>
>采用现代高性能图形接口，兼顾 2D 与 3D 场景渲染
>
>#### Unified engine and game programming language
>
>- 引擎与游戏使用统一的编程语言
>
>避免多语言切换成本，提升代码复用与维护性（结合之前信息可知为 Go 语言）

# 二

>#### Use the right tool for the job
>- 为具体任务选择合适的工具
>#### Singular function/algorithm measurements are fun but pointless
>- 对单个函数 / 算法做性能测试很有趣，但毫无意义
>#### As someone who’s made an identical game engine in 3 different languages, as well as created multiple game engines, I’ve learned the following
>作为一个用 3 种不同语言实现过同款游戏引擎、还开发过多个游戏引擎的人，我总结出以下经验
>-  核心结论
>#### If you don’t lean into the language, you’ll lose a lot of performance
>- 如果你不顺应语言的设计哲学去开发，就会损失大量性能
>#### The overall design of all your systems, and the communication between or outside of them, dictate your performance
>- 系统的整体架构、模块间 / 模块外的通信方式，才是决定性能的关键
>#### Individual features can run fast for what they are doing, but will also eat your performance bandwidth
>- 单个功能模块本身可以跑得很快，但它们会消耗整体的性能带宽（即频繁交互带来的开销）

# 三
>### 对 Go 语言的核心评价
>#### UAbsolutely love C
>- 我非常喜欢 C 语言
>#### UUse a language (tool) that fits what I need/want
>选- 择符合我需求 / 目标的语言（工具）
>#### UFeels like C with modern features
>- Go 给人的感觉就像带了现代特性的 C 语言
>#### UGreat performance
>- 性能优异
>#### UOutperformed identical engine written in C++
>- 用 Go 实现的同款引擎，性能甚至超过了 C++ 版本
>#### UEase of use
>- 易于使用
>#### UExtensive, useful standard library
>- 拥有丰富且实用的标准库
>#### UMost performance is lost by poor design, not by which system language you use
>- 性能损失大多源于糟糕的设计，而非你选择的系统语言
>###  Go 语言的核心特性
>#### USystems programming language
>- 系统级编程语言
>#### UAllows for Assembly programming
>- 支持汇编级编程
>#### UBuilt for threading/concurrency
>- 原生为多线程 / 并发而设计
>#### UBuilt in reflection
>- 内置反射机制
>#### UVery fast build times
>- 极快的编译速度
>#### UCooperates with C very well
>- 与 C 语言交互非常顺畅
>#### UCan even “be” C if needed
>- 必要时甚至可以 “扮演” C 语言的角色
>#### UCode layout standardization
>- 代码布局标准化（Go fmt 强制统一格式）
>#### UBuilt in AST
>- 内置抽象语法树（AST）
# 四
#### Kaiju Engine 的核心架构设计原则
>#### UInterlocking primitives (much like Unix)
>- 相互关联的基础组件（非常类似 Unix 设计哲学）
>
>强调由小而独立的基础模块组合成复杂系统，每个模块职责单一
>#### UCPU Cache friendly
>- 对 CPU 缓存友好
>
>优化数据布局与访问模式，减少缓存未命中，提升运行时性能
>#### UAssembly where needed
>- 必要时使用汇编语言
>
>在性能关键路径上，可直接编写汇编代码榨干硬件性能
>#### UDesign and redesign systems until they work
>- 反复迭代设计系统，直到其正常工作
>
>拥抱迭代式开发，不追求一步到位的完美设计
>#### USimple code/interfaces
>- 简洁的代码与接口
>
>保持代码可读性与可维护性，降低协作与调试成本
>#### UGarbage collector friendly
>- 对垃圾回收（GC）友好
>
>设计上避免频繁内存分配与释放，减少 GC 停顿对实时性能的影响
>#### UNote on garbage collection and game engines
>- 关于垃圾回收与游戏引擎的特别说明
>
>（后续内容会展开）针对游戏引擎的实时性要求，优化 GC 行为
#### 小结
- 延续了 Unix “小工具组合” 的思想，用相互协作的基础模块构建复杂系统
- 兼顾性能优化（缓存友好、汇编级优化）与开发效率（简洁代码、迭代设计）
- 针对 Go 语言的 GC 特性，专门做了 “GC 友好” 的设计适配，避免游戏运行时出现卡顿
# 五
>### 全局变量设计思路
>#### Globals are not allowed
>- 不允许使用全局变量
>#### How can you do things simply without globals?
>- 如何在不使用全局变量的情况下简洁地完成开发工作？
>#### The "Host" is a mediator pattern
>“Host” 是一种中介者模式
>#### The host is passed nearly everywhere, you access the system through the host
>- Host 实例几乎会被传递到代码的每一处，你通过 Host 来访问整个系统的各类功能
>#### The host is a composition of primitives, which are a composition of primitives; it’s turtles all the way down
>- Host 是基础组件的组合，而这些基础组件又是更小基础组件的组合；这是一种 “层层嵌套” 的架构（源自谚语 “乌龟驮着世界，一直往下都是乌龟”，用来形容递归式的组合结构）
#### 小结
- 核心思想：用 Host 中介者模式 替代全局变量，避免全局状态带来的耦合、测试困难等问题
- 架构特点：所有系统功能都封装在 Host 中，通过依赖传递的方式在模块间共享，实现高内聚、低耦合
- 设计哲学：递归式组合（“turtles all the way down”），复杂系统由更小的基础组件层层组合而成
# 六
>#### Entities would be called “Actors” or “Game Objects” in other engines
>- 在其他游戏引擎中，这类实体通常被称为 “演员（Actors）” 或 “游戏对象（Game Objects）”
>#### No inheritance or interfacing
>- 不使用继承或接口
>避免面向对象继承层级带来的耦合与复杂度，保持设计简洁
>#### Holds a transform and optionally some data
>- 仅包含变换信息（位置 / 旋转 / 缩放），并可选择性携带额外数据
>实体是轻量化的 “数据容器”，核心是位置信息，而非复杂行为载体
>#### Updates are registered and manually managed, typically not per-entity but often per-system
>- 更新逻辑需要手动注册与管理，通常不按 “每个实体” 分配，而是按 “每个系统” 集中处理
>符合 ECS（实体 - 组件 - 系统）架构思想，将数据与行为分离，提升性能与可维护性
#七
>#### Retained mode
>- 保留模式（Retained Mode）
>
>UI 元素以树形结构持久化存储，更新时仅修改变化部分，区别于立即模式（Immediate Mode）
>#### Only 2 primitive types, "panel" & "label"
>- 仅包含两种基础类型：「面板（panel）」与「标签（label）」
>
>极简设计，所有复杂 UI 都由这两个基础元素组合而成
>#### Multithreaded
>- 多线程支持
>
>UI 逻辑可并行处理，提升响应速度与性能
>#### CPU cache friendly
>- 对 CPU 缓存友好
>
>数据布局优化，减少缓存未命中，提升渲染效率
>#### Custom built
>- 完全自定义构建
>
>不依赖第三方 UI 框架，自主实现所有功能
>#### HTML & CSS
>- 支持 HTML & CSS 语法
>
>用 Web 技术栈描述 UI 布局与样式，降低学习成本
# 七
### 自给自足
#####  长期目标是尽可能减少对第三方库的依赖。目前依赖的库，未来都计划替换。
>
>|库名 | 用途 | 计划替换？ |
> |---|---|---|
> | github.com/google/uuid  | 生成唯一 ID  | 否  |
> | tdewolff/parse/v2  | CSS 解析器 | 是|
> |x/clipboard|CSS 解析器| 是|
> |Soloud|	音乐 / 音效播放|可能|
> |Bullet3|CSS 解析器|是|
>
---
