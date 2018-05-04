---
layout: post
title: 欧洲人养成计划
date:   2017-11-12 15:34:32 +0900
excerpt: Genetic Algorithm
---
# 欧洲人养成计划

****

## 我TM吹爆

这学期修了一门叫Topics in IT的课，主要内容是介绍Genetic Algorithm（以下简称GA），中文译名叫做遗传算法，这个算法的魔性之处在于从根本上否定了传统王道的「数学模型→算法」的操作路径，抛弃数学分析，使用生物进化论的思想来解决问题，尤其适用于最优化/极限值这样的问题，生动诠释了计算机科学发于数学却又不依赖数学，作为一个完全的独立学科的创造力与可能性。

## 没例子你说个〇〇

这是一个很常见的数学问题：

**对函数f(x)=cos(2x)sin(2x^2)，求其在[-4,4]区间内的最小值及取最小值时的x值**

对于这个问题，不同的人有不同的解法

- 一般人首先想到的解法：f'(x)=0
- 我首先想到的解法：![](http://7xt9ka.com2.z0.glb.qiniucdn.com/googlegraph.png)
- CS大佬想到的解法：GA

## 所以，说好的欧洲人呢

想象这里有一个非洲部落，他们的终极目标就是步入白脸的欧洲部落行列，于是这个部落的基本法里规定了将计划生育作为基本国策，把每一世代的人召集起来按照肤色从白到黑排列，比较白的那一半人可以领到准生证，剩下的那一半脸黑的就被禁止生育，这样就可以让部落里最终诞生一个完全白脸的欧洲人，同时为了预防遗传病，酋长会以对抗高龄化的名义随机钦定一些人接受一些非常随意的基因改造实验然后允许他们生育很多胎，高层的头头脑脑们认为这样就可以快速稳步实现欧洲梦和非洲部落伟大复兴~~（并且已经开会研究决定了大概需要100代）~~

以上就是运用GA生成欧洲人的社会实践案例，看了这个案例大家应该对GA有了非常深刻的认识（笑）

所以我们可以总结出来GA的要点有4个：

- Select（选择）
- Crossover（交配/啪啪啪）
- Mutation（变异）
- Iteration（迭代）

那么现在就赶紧实装一下看看能不能在100代之内实现欧洲梦吧！

首先对欧洲梦问题做一个抽象~~（喂刚才的函数最小值问题就这么被遗忘了吗）~~：

- 每个人的DNA长度是10
- 全部基因都是控制肤色的
- 全部基因都是0表示黑，1表示白
- DNA里1越多的人肤色越白
- 最终目标是诞生一个DNA为[1,1,1,1,1,1,1,1,1,1]的纯种欧洲人
- 这个部落每一代都是10个人，计划生育决定了人口不会增长
- 最开始的世代的DNA是随机的
- 迭代100次
- 每一代接受基因改造实验的人是5%

那么就赶紧开始愉快的编程吧～

![](http://imgcc.naver.jp/kaze/mission/USER/20170920/78/7396088/11/383x338x834e82215bb5e43407a7fddf.jpg)

## 基础设定&初期化

```python
import random

dna_bits = 10
population_size = 10
prob_mutation = 0.05
num_iterations = 100

generation = [[random.randint(0, 1) for i in range(dna_bits)]
              for i in range(population_size)]
iteration(generation, num_iterations)
```

- 生物进化是大量的随机演变所以无疑需要random模块
- generation是一个大小为population_size*dna_bits的矩阵，里面填充随机的0或1
- iteration是执行GA的主函数

## Select

```python
def fitness(list):
    return sum(list)


def tournament(c1, c2):
    f1 = fitness(c1)
    f2 = fitness(c2)
    if f1 > f2:
        return c1
    else:
        return c2


def select(generation):
    N = len(generation)
    new_generation = []
    for i in range(N):
        c1 = random.choice(generation)
        c2 = random.choice(generation)
        c12 = tournament(c1, c2)
        new_generation.append(c12)
    return new_generation
```

首先需要知道谁的脸比较白，由于DNA就是一个只包含0或1的list，1当然是越多越好，所以这里用简单累加list元素的方式来获得每个DNA的fitness，纯种欧洲人[1,1,1,1,1,1,1,1,1,1]的fitness就是10，而完全的非洲人[0,0,0,0,0,0,0,0,0,0]~~就是你啦~~的fitness就是0咯，fitness函数就是实现这个功能的

然后就是如何选出比较白的那一半的人了，最直观的方法就是按照fitness排序然后取上位的5个，但是这样搞有一些问题：

1. 每个人的fitness值并不储存在DNA里面，需要新建一个数据结构储存全员的fitness
2. 需要新建一个数据结构储存第几位是第几号个体
3. 单独抽出上位的一半会限制进化可能性，如果迭代几次后前5名的DNA高度相似会使进化趋于停滞
4. **5个人怎么结婚啪啪啪？共妻吗？**

所以这次这样搞：随机抽两个人，比一下fitness，留下比较白的，再随机抽两个人，比一下fitness，留下比较白的，最后将这两个人天选之人加进new_generation里准备啪啪啪，这样就保证了只要你不是全服最非的（全服最非意味着在fitness竞赛里无法战胜任何人），你就有机率被发到准生证，当然**这种做法跟刚才说好的不一样啊**，而且会出现一个人被加进new_generation里好几次的情况……emmm，你就当一个准生证只能生育一次，被加进new_generation好几次就代表被发了好几次准生证可以啪啪啪好几次就好了~~其实就是为了我编程方便~~这就是select和tournament函数的功能

## Crossover

```python
def crossover(c1, c2):
    idx = random.randint(0, dna_bits - 1)  # where to cut
    c12 = c1[:idx] + c2[idx:]
    c21 = c2[:idx] + c1[idx:]
    return [c12, c21]
```
因为要保证人口规模不变，所以必须是一对夫妇生育两个孩子，具体操作方法是先决定一个随机DNA切点~~还记得高中生物里用酶切蛋白质吗~~然后根据这个切点对夫妇的DNA做X交叉（A的前半段+B的后半段&B的前半段+A的后半段），生成的两个新DNA就是两个后代

## Mutation

```python
def mutation(child):
    child0 = child
    idx = random.randint(0, len(child) - 1) # where to cut
    for i in range(len(child)):
        if i < idx:
            if child0[i] == 0:
                child[i] = 1
            else:
                child[i] = 0
        child[i] = random.choice(child0)
```

跟Crossover一样首先确定一个切点，切点之前的位直接翻转（0→1/1→0），切点之后的位是在原来的序列里随机选择**如果仅仅全部位翻转会大幅降低变异的丰富度最终导致变异形同虚设**

## Iteration

```python
def iteration(generation, iter_num):
    def fitness_avr():
        fit_sum = 0
        for i in generation:
            fit_sum += fitness(i)
        return fit_sum / population_size

    for i in range(iter_num):
        print("iteration:" + str(i))
        for j in range(population_size):
            gen_selected = select(generation)
            gen_crossovered = []
            for k in range(0, population_size, 2):
                two_children = crossover(gen_selected[k], gen_selected[k + 1])
                for l in range(2):
                    mp = random.random()
                    if mp <= prob_mutation:
                        mutation(two_children[l])
                    gen_crossovered.append(two_children[l])
        generation = gen_crossovered
        print("fitness_avr=" + str(fitness_avr()))
        for item in generation:
            if fitness(item) == dna_bits:
                print("iteration=", i)
                for j in range(population_size):
                    print(generation[j])
                return 0
#        if fitness_avr() == 10:
#            return 0
```

gen_selected就是经过选择之后的准生证持有者，在其中按顺序两两交配，对生成的两个后代分别进行变异概率判定，概率命中则对后代执行基因改造实验，最后将后代加入gen_crossovered，这个gen_crossovered就是新一代generation，最后对新的generation进行遍历，如果发现有fitness=dna_bits，即全部DNA位均为1的个体，就输出当前迭代次数和全部个体的DNA结束迭代，另外还有一个计算种群平均fitness的函数fitness_avr用于监测种群进化程度

最后有一个当全员fitness都是10的时候才终止迭代的语句，用这个可以观测到非洲部落全员偷渡欧洲所需的迭代次数，使用这个功能需要修改迭代终止的条件判定，所以默认姑且先注释掉

## 完整代码

这里的代码设定为DNA长度192，人口数300，变异率5%，迭代次数上限300

```python
import random

dna_bits = 192
population_size = 300
prob_mutation = 0.05
num_iterations = 300


def fitness(list):
    return sum(list)


def crossover(c1, c2):
    idx = random.randint(0, dna_bits - 1)  # where to cut
    c12 = c1[:idx] + c2[idx:]
    c21 = c2[:idx] + c1[idx:]
    return [c12, c21]


def tournament(c1, c2):
    f1 = fitness(c1)
    f2 = fitness(c2)
    if f1 > f2:
        return c1
    else:
        return c2


def select(generation):
    N = len(generation)
    new_generation = []
    for i in range(N):
        c1 = random.choice(generation)
        c2 = random.choice(generation)
        c12 = tournament(c1, c2)
        new_generation.append(c12)
    return new_generation


def mutation0(child):
    for i in child:
        if i == 0:
            i = 1
        else:
            i = 0


def mutation(child):
    child0 = child
    idx = random.randint(0, len(child) - 1)
    for i in range(len(child)):
        if i < idx:
            if child0[i] == 0:
                child[i] = 1
            else:
                child[i] = 0
        child[i] = random.choice(child0)


def iteration(generation, iter_num):
    def fitness_avr():
        fit_sum = 0
        for i in generation:
            fit_sum += fitness(i)
        return fit_sum / population_size

    for i in range(iter_num):
        print("iteration:" + str(i))
        for j in range(population_size):
            gen_selected = select(generation)
            gen_crossovered = []
            for k in range(0, population_size, 2):
                two_children = crossover(gen_selected[k], gen_selected[k + 1])
                for l in range(2):
                    mp = random.random()
                    if mp <= prob_mutation:
                        mutation(two_children[l])
                    gen_crossovered.append(two_children[l])
        generation = gen_crossovered
        print("fitness_avr=" + str(fitness_avr()))
        for item in generation:
            if fitness(item) == dna_bits:
                print("iteration=", i)
                for j in range(population_size):
                    print(generation[j])
                return 0
#        if fitness_avr() == 10:
#            return 0


generation = [[random.randint(0, 1) for i in range(dna_bits)]
              for i in range(population_size)]
iteration(generation, num_iterations)
```

## 后记

所以说一开始的函数最小值问题去哪里了？

![](http://imgcc.naver.jp/kaze/mission/USER/20170920/78/7396088/11/383x338x834e82215bb5e43407a7fddf.jpg)
