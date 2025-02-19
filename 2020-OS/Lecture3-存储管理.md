Lecture3-存储管理
---
1. 存储管理是操作系统的重要组成部分，负责管理计算机系统的重要资源——内存储器。
2. 内存空间一般分为两部分
   1. 系统区：存放操作系统内核程序和数据结构等。
   2. 用户区：存放应用程序和数据。
3. 存储管理包括以下功能：
   1. 存储分配：位进程分配内存空间以便运行，完成内存区的分配和去配工作。
   2. 地址映射：内存被抽象为一维或二维地址空间；逻辑空间到物理空间映射。
   3. 存储保护：系统隔离分配给进程的内存区，防止地址越界或操作越权。
   4. 存储共享：系统允许多个进程共享内存区。
   5. 存储扩充：形成虚拟存储器。

<!-- TOC -->

- [1. 存储管理的基础](#1-存储管理的基础)
  - [1.1. 逻辑地址](#11-逻辑地址)
  - [1.2. 物理地址：从处理器角度看到的物理内存单元。](#12-物理地址从处理器角度看到的物理内存单元)
  - [1.3. 段式程序设计](#13-段式程序设计)
  - [1.4. 主存储器的复用](#14-主存储器的复用)
  - [1.5. 存储管理的基本模式](#15-存储管理的基本模式)
  - [1.6. 存储管理模式示意图](#16-存储管理模式示意图)
- [2. 存储管理的功能](#2-存储管理的功能)
  - [2.1. 地址转换](#21-地址转换)
  - [2.2. 存储保护](#22-存储保护)
  - [2.3. 主存储器空间的分配与去配](#23-主存储器空间的分配与去配)
  - [2.4. 主存储器空间的共享](#24-主存储器空间的共享)
  - [2.5. 主存储器空间的扩充](#25-主存储器空间的扩充)
- [3. 连续存储管理](#3-连续存储管理)
  - [3.1. 单连续分区存储管理](#31-单连续分区存储管理)
    - [3.1.1. 单用户连续分区存储管理](#311-单用户连续分区存储管理)
    - [3.1.2. 固定分区存储管理](#312-固定分区存储管理)
      - [3.1.2.1. 固定分区方式的基本思想](#3121-固定分区方式的基本思想)
      - [3.1.2.2. 固定分区方式的主存分配](#3122-固定分区方式的主存分配)
      - [3.1.2.3. 固定分区方式的地址转换](#3123-固定分区方式的地址转换)
      - [3.1.2.4. 固定分区存储管理的缺点](#3124-固定分区存储管理的缺点)
  - [3.2. 可变分区存储管理](#32-可变分区存储管理)
    - [3.2.1. 可变分区方式的内存分配示例](#321-可变分区方式的内存分配示例)
    - [3.2.2. 可变分区方式的主存分配表](#322-可变分区方式的主存分配表)
    - [3.2.3. 可变分区方式的内存回收](#323-可变分区方式的内存回收)
    - [3.2.4. 可变分区方式的内存分配](#324-可变分区方式的内存分配)
  - [3.3. 地址转换与存储保护](#33-地址转换与存储保护)
  - [3.4. 分区方式的内存零头](#34-分区方式的内存零头)
  - [3.5. 内存不足的存储技术](#35-内存不足的存储技术)
    - [3.5.1. 移动技术(程序浮动技术)](#351-移动技术程序浮动技术)
    - [3.5.2. 对换技术](#352-对换技术)
    - [3.5.3. 覆盖技术](#353-覆盖技术)
- [4. 虚拟存储器的概念](#4-虚拟存储器的概念)
  - [4.1. 虚拟存储器思想的提出](#41-虚拟存储器思想的提出)
    - [4.1.1. 分区存储的限制](#411-分区存储的限制)
    - [4.1.2. 程序运行的局部性原理](#412-程序运行的局部性原理)
  - [4.2. 虚拟存储器的基本思想](#42-虚拟存储器的基本思想)
  - [4.3. 虚拟存储器的实现思路](#43-虚拟存储器的实现思路)
  - [4.4. 虚拟存储器](#44-虚拟存储器)
- [5. 存储管理的硬件支撑](#5-存储管理的硬件支撑)
  - [5.1. 存储器的组织层次](#51-存储器的组织层次)
  - [5.2. 存储管理涉及的存储对象](#52-存储管理涉及的存储对象)
  - [5.3. 高速缓存存储器(Cache)](#53-高速缓存存储器cache)
    - [5.3.1. 高速缓存存储器的构成](#531-高速缓存存储器的构成)
    - [5.3.2. 高速缓存存储器的组织](#532-高速缓存存储器的组织)
    - [5.3.3. 高速缓存存储器的分级](#533-高速缓存存储器的分级)
    - [5.3.4. 早期奔腾处理器架构](#534-早期奔腾处理器架构)
    - [5.3.5. 奔腾4处理器架构](#535-奔腾4处理器架构)
    - [5.3.6. i5处理器架构](#536-i5处理器架构)
    - [5.3.7. i7处理器架构](#537-i7处理器架构)
  - [5.4. 地址转换/存储保护的硬件支撑](#54-地址转换存储保护的硬件支撑)
  - [5.5. 存储管理与硬件支撑](#55-存储管理与硬件支撑)
  - [5.6. 虚拟存储与硬件支撑](#56-虚拟存储与硬件支撑)
- [6. 页式存储管理](#6-页式存储管理)
  - [6.1. 页式存储管理的基本原理](#61-页式存储管理的基本原理)
  - [6.2. 页式存储管理中的地址](#62-页式存储管理中的地址)
  - [6.3. 页式存储管理的地址转换例子](#63-页式存储管理的地址转换例子)
  - [6.4. 页式存储管理的内存分配/去配](#64-页式存储管理的内存分配去配)
  - [6.5. 页的共享](#65-页的共享)
  - [6.6. 页式存储管理的地址转换](#66-页式存储管理的地址转换)
    - [6.6.1. 页式存储管理的地址转换代价](#661-页式存储管理的地址转换代价)
    - [6.6.2. 页式存储管理的快表](#662-页式存储管理的快表)
    - [6.6.3. 基于快表的地址转换流程](#663-基于快表的地址转换流程)
    - [6.6.4. 引入快表后的地址转换代价](#664-引入快表后的地址转换代价)
    - [6.6.5. 多道程序环境下的进程表](#665-多道程序环境下的进程表)
    - [6.6.6. 多道程序环境下的地址转换](#666-多道程序环境下的地址转换)
  - [6.7. 多级页表](#67-多级页表)
    - [6.7.1. 多级页表的概念](#671-多级页表的概念)
    - [6.7.2. 多级页表地址转换过程](#672-多级页表地址转换过程)
    - [6.7.3. 多级页表结构的本质](#673-多级页表结构的本质)
  - [6.8. 反置页表(IPT)](#68-反置页表ipt)
    - [6.8.1. 反置页表的提出](#681-反置页表的提出)
    - [6.8.2. 反置页表的基本设计思想](#682-反置页表的基本设计思想)
    - [6.8.3. 反置页表的页表项](#683-反置页表的页表项)
    - [6.8.4. 反置页表的逻辑地址](#684-反置页表的逻辑地址)
    - [6.8.5. 反置页表的地址转换](#685-反置页表的地址转换)
    - [6.8.6. 反置页表](#686-反置页表)
    - [6.8.7. 反置页表下的地址转换示意](#687-反置页表下的地址转换示意)
- [7. 段式存储管理](#7-段式存储管理)
  - [7.1. 程序分段结构](#71-程序分段结构)
  - [7.2. 段式存储逻辑地址](#72-段式存储逻辑地址)
  - [7.3. 段式存储的段表](#73-段式存储的段表)
  - [7.4. 段式存储管理的基本思想](#74-段式存储管理的基本思想)
  - [7.5. 段式存储管理的地址转换流程](#75-段式存储管理的地址转换流程)
  - [7.6. 段的共享](#76-段的共享)
- [8. 分页和分段的寻址计算](#8-分页和分段的寻址计算)
  - [8.1. 分段和分页的比较](#81-分段和分页的比较)
  - [8.2. 分页:逻辑地址到物理地址](#82-分页逻辑地址到物理地址)
- [9. 段页式存储管理](#9-段页式存储管理)
  - [9.1. 段页式存储管理的基本思想](#91-段页式存储管理的基本思想)
  - [9.2. 段页式存储管理的段表和页表](#92-段页式存储管理的段表和页表)
  - [9.3. 段页式存储管理的地址转换](#93-段页式存储管理的地址转换)
- [10. 页式虚拟存储管理](#10-页式虚拟存储管理)
  - [10.1. 页式虚拟存储管理的基本原理](#101-页式虚拟存储管理的基本原理)
    - [10.1.1. 页式虚拟存储管理的页表](#1011-页式虚拟存储管理的页表)
  - [10.2. 页式虚拟存储管理的实现](#102-页式虚拟存储管理的实现)
    - [10.2.1. 页式虚拟存储管理的地址转换](#1021-页式虚拟存储管理的地址转换)
    - [10.2.2. 页式虚拟存储管理的地址转换全过程](#1022-页式虚拟存储管理的地址转换全过程)
    - [10.2.3. TLB(快表)](#1023-tlb快表)
- [11. 页面调度](#11-页面调度)
  - [11.1. 交换区](#111-交换区)
  - [11.2. 页面装入策略和清除策略](#112-页面装入策略和清除策略)
  - [11.3. 页面分配策略](#113-页面分配策略)
    - [11.3.1. 固定分配，本地范围](#1131-固定分配本地范围)
    - [11.3.2. 变量分配，全局范围](#1132-变量分配全局范围)
    - [11.3.3. 变量分配，本地范围](#1133-变量分配本地范围)
  - [11.4. 缺页中断率](#114-缺页中断率)
    - [11.4.1. 缺页中断率的影响因素](#1141-缺页中断率的影响因素)
    - [11.4.2. 用户编程的例子](#1142-用户编程的例子)
  - [11.5. 全局页面替换策略](#115-全局页面替换策略)
    - [11.5.1. OPT页面调度算法(Belady算法)](#1151-opt页面调度算法belady算法)
    - [11.5.2. 先进先出页面调度算法(FIFO)](#1152-先进先出页面调度算法fifo)
    - [11.5.3. 页面缓冲算法](#1153-页面缓冲算法)
    - [11.5.4. 最近最少用LRU页面调度算法](#1154-最近最少用lru页面调度算法)
    - [11.5.5. 第二次机会页面替换算法(SCR，Second Chance Replacement)](#1155-第二次机会页面替换算法scrsecond-chance-replacement)
    - [11.5.6. 最不常用LFU的页面调度算法](#1156-最不常用lfu的页面调度算法)
    - [11.5.7. 时钟CLOCK页面调度算法](#1157-时钟clock页面调度算法)
      - [11.5.7.1. CLOCK算法的工作流程](#11571-clock算法的工作流程)
      - [11.5.7.2. CLOCK算法的例子](#11572-clock算法的例子)
      - [11.5.7.3. 第三次机会时钟替换算法：结合引用位和修改位](#11573-第三次机会时钟替换算法结合引用位和修改位)
    - [11.5.8. 不同算法性能比较](#1158-不同算法性能比较)
  - [11.6. 局部页面替换算法(不考)](#116-局部页面替换算法不考)
    - [11.6.1. 局部最佳页面替换算法(MIN)](#1161-局部最佳页面替换算法min)
    - [11.6.2. 工作集模型和工作集置换算法(WS)](#1162-工作集模型和工作集置换算法ws)
      - [11.6.2.1. 进程工作集](#11621-进程工作集)
      - [11.6.2.2. 示例](#11622-示例)
    - [11.6.3. 模拟工作集替换算法](#1163-模拟工作集替换算法)
    - [11.6.4. 缺页频率替换算法](#1164-缺页频率替换算法)
    - [11.6.5. 通过工作集确定驻留集大小](#1165-通过工作集确定驻留集大小)
- [12. 段式虚拟存储管理](#12-段式虚拟存储管理)
  - [12.1. 段式虚拟存储管理的基本思想](#121-段式虚拟存储管理的基本思想)
  - [12.2. 段式虚拟存储管理的段表扩充](#122-段式虚拟存储管理的段表扩充)
  - [12.3. 段式虚拟存储管理的地址转换](#123-段式虚拟存储管理的地址转换)
- [13. 段页式虚拟存储管理](#13-段页式虚拟存储管理)
  - [13.1. 段页式虚拟存储基本原理](#131-段页式虚拟存储基本原理)
  - [13.2. 段页式虚拟存储管理的地址转换](#132-段页式虚拟存储管理的地址转换)
- [14. 存储管理方案以及虚存页面替换算法小结](#14-存储管理方案以及虚存页面替换算法小结)
- [15. 补充：关于快表问题](#15-补充关于快表问题)
- [16. Linux虚拟存储管理](#16-linux虚拟存储管理)
  - [16.1. 伙伴系统(一种算法)](#161-伙伴系统一种算法)
    - [16.1.1. 例子：类似二叉树的形式进行分配](#1611-例子类似二叉树的形式进行分配)
    - [16.1.2. Linux伙伴系统](#1612-linux伙伴系统)
      - [16.1.2.1. Linux基于伙伴的slab分配器](#16121-linux基于伙伴的slab分配器)
      - [16.1.2.2. slab分配器主要操作](#16122-slab分配器主要操作)

<!-- /TOC -->

# 1. 存储管理的基础

## 1.1. 逻辑地址
1. 逻辑地址：又称相对地址，即用户编程所使用的地址空间
2. 逻辑地址从零开始编号，有两种形式：
   1. 一维逻辑地址(地址)
   2. 二维逻辑地址(段号:段内地址)
。
## 1.2. 物理地址：从处理器角度看到的物理内存单元。
1. 物理地址：又称绝对地址，即程序执行所使用的地址空间
2. 处理器执行指令时按照物理地址进行

## 1.3. 段式程序设计
1. 把一个程序设计成多个段：代码段、数据段、堆栈段等等
2. 用户可以自己应用**段覆盖技术**扩充内存空间使用量，这一技术是程序设计技术，不是OS存储管理的功能：只是用一些段构成一个比较小的程序，然后动态来调整。
3. 结合虚存完成内存部分的扩充

## 1.4. 主存储器的复用
1. 多道程序设计需要复用主存
2. 按照分区复用：
   1. 主存划分为多个固定/可变尺寸的分区
   2. 一个程序/程序段占用一个分区
3. 按照页架复用：
   1. 主存划分成多个固定大小的页架
   2. 一个程序/程序段占用多个页架

## 1.5. 存储管理的基本模式
1. 单连续存储管理：一维逻辑地址空间的程序占用一个主存固定分区或可变分区
2. 段式存储管理：段式二维逻辑地址空间的程序占用多个主存可变分区
3. 页式存储管理：一维逻辑地址空间的程序占用多个主存页架区
4. 段页式存储管理：段式二维逻辑地址空间的程序占用多个主存页架区
5. 注意是否可以虚拟化

## 1.6. 存储管理模式示意图
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/1.png)

1. 应用级程序员直接面对的是**逻辑地。址**，而最后运行需要使用的是**物理地址：从处理器角度看到的物理内存单元。**。
2. 一般目前常用的是**动态重定位**，将逻辑地址转换为对应的物理地址。
3. 分页：形成页框加载程序，页框和页框之间可以是不连续的。
4. 分段：每一个段有相应分区，使用段表完成逻辑段向物理段的映射
5. 段页式：在分段基础上，还实现了页框部分。

# 2. 存储管理的功能

## 2.1. 地址转换
1. **地址转换**：又称重定位，即把逻辑地址转换成绝对地址
   1. **静态重定位**：在**程序装入内存**时进行地址转换：由装入程序执行，早期小型OS使用，基于地址固定值进行偏移。
   1. **动态重地位(主流)**：在**CPU执行程序时进行地址转换**：从效率出发，依赖硬件地址转换机构，运行时正确的将其逻辑地址转换为物理地址。
2. 解释执行指令的时候才进行地址的转换，必须要借助硬件电路完成，而不能用软件完成(效率考量)

## 2.2. 存储保护
1. 为**避免**主存中的多个进程**相互干扰**，必须**对主存中的程序和数据进行保护**
   1. 私有主存区中的信息：可读可写
   2. 公共区中的共享信息：根据授权
   3. 非本进程信息：不可读写
2. 这一功能需要软硬件协同完成：CPU检查是否允许访问，不允许则**产生地址保护异常**，由OS进行相应处理
   1. 地址越界保护依赖于硬件设施、常用的有界地址和存储键。
   2. 进程在访问分配给自己的内存区时，要对访问权限进行检查

## 2.3. 主存储器空间的分配与去配
1. **分配**：进程装入主存时，存储管理软件进行具体的**主存分配**操作，并设置**一个表格**记录主存空间的分配情况
2. **去配**：当某个进程撤离或主动归还主存资源时，存储管理软件要收回它所占用的全部或者部分存储空间，调整主存分配表信息

## 2.4. 主存储器空间的共享
1. **多个进程共享主存储器资源**：多道程序设计技术使若干个程序同时进入主存储器，各自占用一定数量的存储空间，共同使用一个主存储器
2. **多个进程共享主存储器的某些区域**：若干个协作进程有共同的主存程序块或者主存数据块

## 2.5. 主存储器空间的扩充
1. **存储扩充**：把磁盘作为主存扩充，只把部分进程或进程的部分内容装入内存：扩大多道程序设计的道数
   1. 对换技术：把部分不运行的进程调出
   2. 虚拟技术：只调入进程的部分内容，对单个进程不使用对换技术完成，特点是自动化、透明
2. 这一工作需要软硬件协作完成
   1. 对换进程决定对换，硬件结构完成调入
   2. CPU处理到不在主存的地址，发出**虚拟地址异常**，OS将其调入，重执指令
3. 进程的内存为$4MB$，一个页框$4KB$，有$1024$个页框，页框表一共$16$个页框，页的压缩比是$\frac{1024}{16} = 64$。

# 3. 连续存储管理

## 3.1. 单连续分区存储管理
1. 每个进程占用一个物理上完全连续的存储空间(区域)
2. 单连续分区存储管理细分:
   1. 单用户连续存储管理
   2. 固定分区存储管理
   3. 可变分区存储管理
3. 分区方式不能实现虚拟存储。

### 3.1.1. 单用户连续分区存储管理
1. 适用于单用户单任务操作系统，如DOS
2. 主存区域(内存空间)划分为**系统区**与**用户区**
   1. 系统区用于存放操作系统内核程序和数据结构等
   2. 用户区用于存放应用程序和数据
3. 设置一个**栅栏寄存器**界分两个区域，硬件用它在执行时进行存储保护
4. 一般采用**静态重定位**进行地址转换
5. 硬件实现代价低
6. 单用户连续分区存储管理示意
   1. 静态重定位：在装入一个作业时，把该作业中程序的指令地址和数据地址全部转换成绝对地址
   2. 界限地址:放置软件访问到操作系统的部分

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/6.png)

### 3.1.2. 固定分区存储管理
1. 固定分区存储管理又称静态分区模式

#### 3.1.2.1. 固定分区方式的基本思想
1. 内存空间被划分为数目固定不变的分区，各分区大小不等，每个分区只装入一个作业，若多个分区中都装有作业，则它们都可以并发执行。
2. 可用静态/动态重定位、硬件实现代价低、被早期OS采用
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/7.png)

#### 3.1.2.2. 固定分区方式的主存分配
1. 主存分配表：包含内容：起始地址、长度、占用标志
2. 内存分配方法很简单，其任务有何时吧内存空间划分成分区：由系统管理员和操作系统初始化模块协同完成。

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/8.png)

3. 作业进入分区的排队策略：
   1. 每个分区有自己的作业等待队列，作业等待能装下自身的最小分区。
   2. 所有等待处理作业排成等待队列，每当有空闲，找到队列中能进入的最大的一个。

#### 3.1.2.3. 固定分区方式的地址转换
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/9.png)

#### 3.1.2.4. 固定分区存储管理的缺点
1. 由于预先规定了分区的大小，使得大作业无法装入，而不得不采用覆盖技术，带来负担。
2. 内存空间利用率不高，作业很少填满分区：固定分区存储管理不够灵活，既不适应大尺寸程序，又存在内存**内零头**，有浪费，内存内零头是因为在分区内部有零头。
3. 如果作业在运行中要求动态扩展内存空间是困难的。
4. 分区数目是操作系统初启动时确定的，会限制多道运行程序的道数。

## 3.2. 可变分区存储管理
1. 可变分区存储管理又称动态分区模式，按照作业大小划分分区，但划分的时间、大小和位置都是动态的。
2. 创建一个进程时，根据进程所需主存量查看主存中是否有足够的连续空闲空间
   1. 若有，则按需要量分割一个分区
   2. 若无，则令该进程等待主存资源
3. 由于分区大小按照进程实际需要量来确定，因此分区个数是随机变化的

### 3.2.1. 可变分区方式的内存分配示例
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/10.png)

### 3.2.2. 可变分区方式的主存分配表
1. 管理的数据结构：已分配区表与未分配区表，采用链表实现

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/11.png)

2. 找一个最大的空闲的位置进行分配

### 3.2.3. 可变分区方式的内存回收
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/12.png)

1. 可变分区方式的内存回收会导致内存空间的转换
2. 作业X撤离后有且仅有如上4种情况。

### 3.2.4. 可变分区方式的内存分配
1. 最先适应分配算法：
   1. 最先适应就是从上向下查找，找到第一块区域放进去，将剩下的区域分割后仍作为空闲区。
   2. 有利于大作业装入，但也使得内存低地址和搞地质两端的分区利用不均衡，回收分区麻烦。
2. 邻近适应分配算法：
   1. 从上次查找结束的地方开始执行最先适应分配算法
   2. 缩短平均查找时间，且存储空间利用率更均衡，不会使得小空闲区集中在内存一侧
3. 最优适应分配算法：
   1. 每次都是分配最接近需要使用大小的部分，会生成很多很小的内存内零头。
   2. 通常会将空闲区按照长度递增顺序排列，等同于最先适应分配算法，查找时间最长
4. 最坏适应分配算法：
   1. 每次都是挑选最大的一块区域进行分配
   2. 有利于中小型作业。
   3. 可把空闲区按长度递减顺序排列，等同于最先适应分配算法。
5. 快速适应分配算法：课本补充
   1. 为经常用到的长度的空闲区设立单独的空闲区链表，查找非常快速
   2. 归还内存空间时和邻近空闲区的合并复杂且耗时。
6. 最常用的是最先适应分配算法，其次是邻近适应分配算法和最优适应分配算法

## 3.3. 地址转换与存储保护
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/34.png)

1. 硬件实现机制与动态重定位
2. 进程的程序和数据的地址由硬件完成
   1. 基址寄存器：分配进程的起始地址
   2. 限长寄存器：进程占用的连续存储空间的长度

## 3.4. 分区方式的内存零头
1. 固定分区方式会产生**内存内零头**
2. 可变分区方式也会随着进程的内存分配产生一些小的不可用的内存分区，称为**内存外零头**，内存外零头是指分区内部是没有零头的，而是在外面的零头。
3. **最优适配算法最容易产生外零头**
4. 任何适配算法都**不能避免**产生外零头

## 3.5. 内存不足的存储技术

### 3.5.1. 移动技术(程序浮动技术)
1. 碎片：内存中的小空闲区，移动分区来解决内存外零头问题。
2. 当未分配区表中找不到足够大的空闲区来装入新进程时，我们使用移动技术来完成内存紧凑，实现方法：
   1. 全部移动到一侧
   2. 移动直到有足够大的空闲区
3. 需要动态重定位支撑:静态重定位无法解决内存外零头
4. 问题：移动技术有极大的系统开销，而且并不是任何时间下都可以进行的，比如通道或DMA等按照绝对物理地址交换信息时。

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/13.png)

5. 移动技术的工作流程
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/14.png)

6. 注意如果剩余空间地方不足，那么是不会移动分区的

### 3.5.2. 对换技术
1. 对换技术广泛应用于分时系统的调度，用来解决内存容量不足的问题，也可以应用于批处理系统，以平衡系统负载。
2. 如果当前一个或多个驻留进程都处于阻塞态，此时选择其中一个进程，将其暂时移出内存，腾出空间给其他进程使用；同时把磁盘中的某个进程换人内存，让其投人运行，这种互换称为**对换**。
3. 被对换出去的进程的状态会调整为就绪态，并且通知存储管理程序，一旦内存可用，立即将该进程对换回内存。
4. 对换技术关键点
   1. 被对换进程：通常系统选择时间片耗尽或优先级较低的进程对换出去。
   2. 对换的进程信息：将数据区和堆栈通过文件系统转换为特殊文件保存。
   3. 被对换的时机：
      1. 批处理系统中：进程需要扩充内存空间但不能被满足时
      2. 分时系统：
         1. 时间片结束时
         2. 执行I/O操作时
5. 对换需要访问磁盘，是I/O集中型操作，但是操作系统可以让计算型任务与对换并行，不会造成系统效率显著下降。
6. 详见P205

### 3.5.3. 覆盖技术
1. 移动和对换技术解决因多个程序存在而导致内存区不足问题。
2. 但是如果程序长度超过物理内存的总和，或者超出固定分区大小，则会出现内存永久性短缺，大程序无法运行，解决方案是覆盖技术。
3. 覆盖是指程序执行过程中程序的不同模块在内存中相互替代，以达到小内存执行大程序的目的。
4. 基本的实现技术是把用户空间分成固定区和一个或多个覆盖区，把控制或不可覆盖的部分放到固定区，其余按照调用结构以及先后关系分段并存放在磁盘上，运行时一次调入覆盖区。
5. 不足是将存储管理工作转给程序员，他们必须根据可用物理内存空间来设计和编写程序。

# 4. 虚拟存储器的概念
1. 之前所介绍的存储管理，我们称为实存管理，必须为进程分配足够内存空间，装入其全部信息，否则无法运行。

## 4.1. 虚拟存储器思想的提出
1. 主存容量限制带来诸多不便
   1. 用户编写程序必须考虑主存容量限制
   2. 多道程序设计的道数受到限制
2. 用户编程行为分析
   1. 全面考虑各种情况，执行时有互斥性
   2. 顺序性和循环性等空间局部性行为
   3. 某一阶段执行的时间局部性行为
3. 因此可以考虑部分调入进程内容

### 4.1.1. 分区存储的限制
1. 每个进程(每个连续逻辑地址空间)必须获得物理地址上的完全连续，突破：分区，分段
2. 必须一次性满足满足每个进程运行时的全部内存需求，突破：虚拟存储，部分装入对换
3. 注：一旦发生缺页，则会从运行态调整到阻塞态，可能会导致部分进程的速度被拖慢，但是整体效率会提高

### 4.1.2. 程序运行的局部性原理
1. 在一个周期内，这个进程在运行时会集中访问一些存储区：某存储单元被访问，改单元机器相邻存储单元很可能会被访问(空间局部性)，或者最近访问过的存储单元很快又能被访问(时间局部性)。
2. 根据程序运行的局部性原理，会保证程序的访问效率比较高，缺页率相对低，以时间换取空间

## 4.2. 虚拟存储器的基本思想
1. 部分装入：存储管理把进程全部信息放在辅存中，执行时先将其中一部分装入主存，以后根据执行行为**随用随调入**
2. 按需调出：如主存中没有足够的空闲空间，存储管理需要根据执行行为把主存中暂时不用的信息**调出**到辅存上去

## 4.3. 虚拟存储器的实现思路
1. 需要建立与自动管理两个地址空间
   1. (辅存)**虚拟地址**空间：容纳进程装入
   2. (主存)**实际地址**空间：承载进程执行
2. 对于用户，计算机系统具有一个容量大得多的主存空间，即**虚拟存储器**

## 4.4. 虚拟存储器
1. 在具有层次结构存储器的计算机系统中，自动实现部分装入和部分替换功能，能从逻辑上为用户提供一个比物理内存容量大得多的、可寻址的内存储器。
2. 虚拟存储器是一种地址空间扩展技术，通常意义上对用户编程是透明的，除非用户需要进行高性能的程序设计
   1. 逻辑地址：进程角度看到的逻辑内存单元。
   2. 物理地址：从处理器角度看到的物理内存单元。
3. 对换技术以进程为单位，虚存管理以页或段为单位。

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/2.png)

# 5. 存储管理的硬件支撑

## 5.1. 存储器的组织层次
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/3.png)

1. 越处于顶端，访问速度越快，容量越小，单位字节价格会越高。我们根据实际情况，选择使用什么样子的存储器。
   1. **寄存器、缓存和内存**属于操作系统存储管理的范畴，掉电后信息丢失。
   2. **磁盘和磁带**属于稳健管理和设备管理的管辖对象，信息永久保存。
2. 可执行程序必须被保存在**内存**中，与设备交换的信息也依托于**内存地址空间**。
3. 由于程序处理数据时存在**顺序性和局部性**，故执行时仅需调入当前运行使用的一部分，其他部分待需要时再逐步调入。

## 5.2. 存储管理涉及的存储对象
1. 存储管理是OS管理主存储器的**软件部分**
2. 为获得更好的处理性能，部分主存程序与数据(特别是关键性能数据)被调入Cache，存储管理需要对其进行管理，甚至包括对联想存储器的管理
3. 为获得更大的虚拟地址空间，存储管理需要对存放在硬盘、固态硬盘、甚至网络硬盘上的虚拟存储器文件进行管理，首选固态硬盘

## 5.3. 高速缓存存储器(Cache)
1. Cache是介于CPU和主存储器间的高速小容量存储器，由**静态存储芯片SRAM**组成，容量较小但比主存DRAM技术更加昂贵而快速，接近于CPU的速度
2. CPU往往需要重复读取同样的数据块，Cache的引入与缓存容量的增大，可以大幅提升CPU内部读取数据的命中率，从而提高系统性能

### 5.3.1. 高速缓存存储器的构成
1. 高速缓冲存储器通常由**高速存储器、联想存储器、地址转换部件、替换逻辑**等组成
   1. **联想存储器**：根据内容进行寻址的存储器
   2. 地址转换部件：通过联想存储器建立目录表以实现快速地址转换。命中时直接访问Cache；未命中时从内存读取放入Cache
   3. 替换逻辑部件：在缓存已满时按一定策略进行数据块替换，并修改地址转换部件
2. MMU：硬件，存储管理单元

### 5.3.2. 高速缓存存储器的组织
1. 由于CPU芯片面积和成本，Cache很小
2. 根据成本控制，划分为L1、L2、L3三级

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/4.png)

### 5.3.3. 高速缓存存储器的分级
1. L1 Cache：分为数据缓存和指令缓存；内置；其成本最高，对CPU的性能影响最大；通常在32KB-256KB之间
2. L2 Cache：分内置和外置两种，后者性能低一些；通常在512KB-8MB之间
3. L3 Cache：多为外置，在游戏和服务器领域有效；但对很多应用来说，**总线改善**比**设置L3**更加有利于提升系统性能

### 5.3.4. 早期奔腾处理器架构
1. Intel在最初的奔腾处理器中只包含L1 Cache(含Code Cache和Data Cache)

### 5.3.5. 奔腾4处理器架构
1. 奔腾4的处理器中包含L1 Cache和L2 Cache

### 5.3.6. i5处理器架构

### 5.3.7. i7处理器架构
1. i7处理器中包含L1至L3三级Cache，如果是包含了三级Cache，那么意味着CPU与Cache之间的链接在CPU内部，Core i7处理器方案是将L3 Cache设计为包含在处理中的多个核心Cache

## 5.4. 地址转换/存储保护的硬件支撑
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/5.png)

1. 限长寄存器来检查越界中断
2. 相加体现了动态重定位
3. 比较体现了存储保护

## 5.5. 存储管理与硬件支撑
1. 鉴于程序执行与数据访问的**局部性原理**，存储管理软件使用**Cache**可以大幅度提升程序执行效率
2. **动态重定位、存储保护**等，若无硬件支撑在效率上是无意义的，即无实现价值
3. 无**虚拟地址中断**，虚拟存储器无法实现
4. 无页面替换等硬件支撑机制，虚拟存储器在效率上是无意义的

## 5.6. 虚拟存储与硬件支撑
1. 操作系统的存储管理依靠底层硬件支撑来完成任务，该硬件是存储管理部件(Memory Managment Unit, MMU)，提供地址转换和存储保护并支持虚存管理和多任务管理。
2. MMU由一组集成电路芯片组成，逻辑地址作为输入，物理地址作为输出，直接送达总线，对内存单元进行寻址。

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/86.png)

3. 主要功能：P217
   1. 管理硬件页表基址寄存器
   2. 分解逻辑地址
   3. 管理快表
   4. 访问页表
   5. 发出异常
   6. 管理特征位

# 6. 页式存储管理

## 6.1. 页式存储管理的基本原理
1. 页面：金层逻辑地址空间分成大小相等的区，每个区称为页面或页，页号从0开始编号。比如出版一本书，出版受到页大小影响，最后由若干页组成，一般大小为4KB
2. 页框：又称页帧，把内存物理地址空间分成大小相等的区，其大小与页面大小相等，每个区都是一个页框(物理块)，块号从0开始。
3. 逻辑地址：分页存储器的逻辑地址由页号 + 页面偏移组成(地址总线32位)
   1. 页号：32-12 = 20位，则包含页$2^{20}$位
   2. 页面偏移：页面大小为4KB，则需要12位
4. 内存页框表：该表长度取决于内存划分的物理块数，表项中给出物理块使用情况，0为空闲，1为占用，有的系统还会添加保护位、脏位等等。
5. 页表：将页装入到内存中，**页未必连续**，我们需要为每一个页面设立一个重定向寄存器，这个寄存器的集合就是页表。
   1. 数学角度：$页面号 \rightarrow 页框号$
   2. 系统设置页表基址寄存器，存放当前运行进程的页表起始地址。
   3. $物理地址 = 页框号 * 块长 + 页内偏移$，实际转换时，我们将页内偏移作为低地址，根据页号从页表中查找到页框号并作为高地址即可。
   4. 页表不存储**页号**，只存储页框号和相应标志位
6. 页式存储产生的碎片是内部碎片
   1. 可以类比固定分区
   2. 比如19KB的程序，加载到页大小为4KB中，会产生1KB的内存内零头。

| ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/81.png) | ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/15.png) |
| -------------------- | -------------------- |

> 1. 页号p
> 2. 页内偏移d
> 3. 页框号b

## 6.2. 页式存储管理中的地址
1. 页式存储管理的逻辑地址由两部分组成，**页号和单元号(页内偏移)**，逻辑地址形式：

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/16.png)

2. 页式存储管理的物理地址也有两部分组成：**页架号(页框号)和单元号(页内偏移)**，物理地址形式：

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/17.png)

3. 地址转换可以通过查页表完成
4. 用户不必关心页的具体存储，系统帮助用户完成从页号/页架号映射到物理地址来完成。

## 6.3. 页式存储管理的地址转换例子
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/18.png)

1. 逻辑地址页号，作为偏移量到页表中进行偏移，得到页架号(页框号)。
2. 对页架号(页框号)进行二进制移位操作(补充12个0)，映射到物理空间中本页的首地址，然后根据offset(单元号)在页内进行偏移，从而获取到绝对地址(物理地址：从处理器角度看到的物理内存单元)中的值。

## 6.4. 页式存储管理的内存分配/去配
1. 页式存储管理，系统要建立一张内存物理块表，用来记录页框状态，管理物理内存的而分配，所包含的信息包含内存总块数、哪些为空闲块、哪些已经分配以及分配给哪个进程等。
2. 最简单方法是用一张**位示图**来记录主存分配情况，使用一位来标记一个页框的使用或空闲的状态(压缩的思想)
   1. 如果够，则去查找一个标记为0的装入
   2. 如果不够，采用一定的策略
3. 建立进程页表维护主存逻辑完整性

| ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/19.png) | ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/82.png) |
| -------------------- | -------------------- |

1. 分页存储管理页框分配算法：
   1. 进行内存分配时，先检查空闲块数是否满足用户进行要求
      1. 若不能则使进程等待。
      2. 若能则查位示图，将位0置为占用标志，从空闲块数中减去本次占用快熟，找到对应的页框号，写入页表。
   2. 归还时逆操作。

## 6.5. 页的共享
1. 页式存储管理能够实现多个进程共享程序和数据
2. **数据共享**：不同进程可以使用**不同**页号共享数据页，但是必须解决共享信息保护问题，常用的是在页表中添加标记位。
3. **程序共享**：不同进程必须使用**相同**页号共享代码页，共享代码页中的(**JMP <页内地址>**)指令，使用不同页号是做不到，进程一和二都要跳转到页内的位置，程序共享要求页号相同。

## 6.6. 页式存储管理的地址转换
> 快表TLB，Translation Look_aside Buffer

### 6.6.1. 页式存储管理的地址转换代价
1. **页表放在主存**: 每次地址转换必须访问两次主存
   1. 按页号读出页表中的相应页架号
   2. 按计算出来的绝对地址进行读写
2. **存在问题**：降低了存取速度
3. **解决办法**：利用Cache存放部分页表，即快表

### 6.6.2. 页式存储管理的快表
1. 为提高地址转换速度，设置一个专用的高速存储器，用来存放页表的一部分
2. **快表**：存放在高速存储器中的页表部分，快表表项：**页号+页架号**
4. 这种高速存储器是**联想存储器(TLB)**，即**按照内容寻址**，而非按照地址访问，根据页号进行寻址。

### 6.6.3. 基于快表的地址转换流程
1. 按逻辑地址中的页号查快表
   1. 若该页**已在快表**中，则由页架号和单元号形成绝对地址
   2. 若该页**不在快表**中，则再查主存页表形成绝对地址，同时将该页登记到快表中
2. 当**快表填满**后，又要登记新页时，则需在快表中按一定策略**淘汰**一个旧登记项
3. 快表可以理解为一个简单的账本

### 6.6.4. 引入快表后的地址转换代价
1. 采用**快表**后，可以加快地址转换速度
2. 假定主存访问时间为200毫微秒，快表访问时间为40毫微秒，查快表的命中率是90%，平均地址转换代价为$(200+40)*90\%+(200+200+40)*10\%=260$毫微秒
3. 比两次访问主存的时间(400毫微秒)下降了**36%**

### 6.6.5. 多道程序环境下的进程表
1. 进程表中登记了每个进程的页表
2. 进程占有处理器运行时，其**页表起始地址和长度**送入**页表控制寄存器**

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/20.png)

> 页表长度就是页表项的数量

### 6.6.6. 多道程序环境下的地址转换
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/21.png)

1. 页表控制寄存器存储了当前的页表的地址和长度
2. 页表控制寄存器和进程表是有关联的，所有进程在进程表中都有一项，当这个进程占据CPU时，这个进程就占据页表控制寄存器。
3. 不使用快表:首先从逻辑地址中，提取出页号，比较页号是否出现越界中断，如果没有越界，则根据页表向下偏移到对应的块号，提取出页表信息和页框号，页框号结合单元号，得到物理地址
4. 快表:不是从页表中查找，而是优先从快表中查询块号。

## 6.7. 多级页表

### 6.7.1. 多级页表的概念
1. 现代计算机普遍支持$2^{32}-2^{64}$容量的逻辑地址空间，采用分页存储管理时，页表相当大，以Windows为例，其运行的Intel x86平台具有32位地址，规定页面4KB($2^{12}$)时，那么，4GB($2^{32}$)的逻辑地址空间由1MB($2^{20}$)个页组成，若每个页表项占用4个字节，则需要占用4MB($2^{22}$)连续主存空间存放页表。系统中有许多进程，因此页表存储开销很大。
2. 做法：把整个页表分割成许多小页表，每个称为**页表页**，它的大小与页框长度相同，于是每个页表页含有若干页表表项。
   1. 页表项从0开始编号，允许放到不连续的页框中，为了找到页表页，建立地质索引，称为**页目录表**。
   2. 系统为每一个进程建立一张页目录表，他的每一个表项指出一个页表页，而页表页的每个表项给出页面和页框的对应关系。
3. 逻辑地址结构有三部分组成：页目录、页表页和位移

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/83.png)

4. 解决页表页如何占用内存空间的问题，解决方法：进程运行设计到的页面的页表页放置在内存中，其他页表页使用时动态调入，因此需要添加标志位指示是否调入内存。

### 6.7.2. 多级页表地址转换过程
| ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/71.png) | ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/72.png) |
| -------------------- | -------------------- |
| ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/73.png) | ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/74.png) |

### 6.7.3. 多级页表结构的本质
1. 多级不连续导致多级索引。
2. 以二级页表为例，用户程序的页面不连续存放，要有页面地址索引，该索引是进程页表；进程页表又是不连续存放的多个页表页，故页表页也要页表页地址索引，该索引就是页目录。
3. 页目录项是页表页的索引，而页表页项是进程程序的页面索引。

## 6.8. 反置页表(IPT)
1. 页表设计的一个重要缺陷是页表的大小与虚拟地址空间的大小成正比
2. 对于一个128MB的计算机，如果页面尺寸为4KB，页表项大小为4B，那么其反置页表只占有128KB的内存。
3. 通过这个结构，哈希表和反向表中只有一项对应于一个实存页(面向实存)，而不是虚拟页(面向虚存)。因此，不论由多少进程、支持多少虚拟页，页表都只需要实存中的一个固定部分。
4. 正向页表(名单)、反置页表(现场坐的是谁)
   1. 正向页表:以**页号**为索引(隐含)，完整连续排列，页表项中不含页号，每个进程单独一个页表
   2. 反置页表:以**页框号**为索引(隐含)，完整连续排列，每个页框填入的是哪个进程的哪个页号，索引进程共用一个反置页表。其页表项不包含页框号

### 6.8.1. 反置页表的提出
1. 页表及相关硬件机制在地址转换、存储保护、虚拟地址访问中发挥了**关键作用**，为页式存储管理设置专门硬件机构
3. 内存管理单元MMU：CPU管理虚拟/物理存储器的控制线路，把虚拟地址映射为物理地址，并提供存储保护，必要时确定淘汰页面
4. 反置页表IPT：**MMU使用的数据结构**

### 6.8.2. 反置页表的基本设计思想
1. **针对内存中的每个页架建立一个页表**，按照块号(页架号)排序
2. 表项包含：正在访问该页框的进程标识、页号及特征位，和**哈希链指针**等
3. 用来完成内存页架到访问进程页号的对应，即物理地址到逻辑地址的转换

### 6.8.3. 反置页表的页表项
1. 页号：虚拟地址页号
2. 进程标志符：使用该页的进程号(页号和进程标志符结合起来标志一个特定进程的虚拟地址空间的一页)
3. 标志位：有效、引用、修改、保护和锁定等标志信息
4. 链指针：**哈希链**，如果某个项没有链项，则该域为空(允许用一个单独的位来表示)。

### 6.8.4. 反置页表的逻辑地址
1. 进程标识符：使用该页的进程。
2. 页号：虚拟地址页号部分，页号和进程标志符结合起来标志一个特定的进程的虚拟地址空间的一页。
3. 页内位移

### 6.8.5. 反置页表的地址转换
| ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/75.png) | ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/76.png) |
| -------------------- | -------------------- |

> 上图4-10中，以**页框号**为索引，记录当前页框中存储的是哪个进程的哪个页

1. 反置页表地址转换过程如下:
   1. 需要访问内存地址时，地址转换机制用进程标识符与页号作为输入，由哈希函数先映射到哈希表，哈希表项存放的是指向IPT表项的指针
      1. 此指针可能就是指向匹配的IPT表项
      2. 如果不是则遍历哈希链直至找到进程标识符与页号均匹配的IPT表项：因为多个页号通过哈希值可能得到了相同的哈希值，所以我们选择使用哈希链。
   2. 而此表项的**序号(索引)**就是页框号，通过拼接页内位移便可生成物理地址。
   3. 若在反置页表中未能找到匹配的IPT页表项，说明此页不在内存，触发缺页异常，请求操作系统通过页表调入：发生缺页中断时需要多访问一次磁盘，速度会比较慢。
2. 页框号是根据公式换算出来的:$x_i = x_0 + 4 * i$

### 6.8.6. 反置页表
| 线性反置页表         | 反置页表                   |
| -------------------- | -------------------------- |
| ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/77.png) | ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/78.png)       |
| 哈希线性反置页表     | 主存分配的位示图和链表方法 |
| ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/79.png) | ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/80.png)       |

### 6.8.7. 反置页表下的地址转换示意
1. 未显示选择淘汰页面，同样由MMU完成
2. 使用哈希提高性能->不必遍历

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/26.png)

# 7. 段式存储管理
1. 段式存储管理基于可变分区存储管理原理。

## 7.1. 程序分段结构
1. 高级语言采用模块化程序设计方法。应用程序由若干程序段(模块)组成，如由主程序段(M)、子程序段(X)、数据段(D)和工作区段(W)组成，每一段都从0开始编制，各有各自名字和长度且实现不同功能。

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/27.png)

2. 编译后段间地址是不连续的，段内地址是连续的。

## 7.2. 段式存储逻辑地址
1. 分段存储器的逻辑地址由两部分组成:段号+段内偏移
2. 页式存储管理中页的划分对程序员不可见。
3. 段式存储管理中段的划分对程序员可见。

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/84.png)

## 7.3. 段式存储的段表
1. 存储分配时，应该为进入内存的作业建立段表，各段在内存中的情况有段表来记录，包含了段号、段起始长度和长度。
2. 撤销进程时，回收所占用的内存空间，并清除此进程的段表。
3. 段表表项实际上起到了基址/限长寄存器的作用，设置段表控制寄存器

## 7.4. 段式存储管理的基本思想
1. 段式存储管理基于可变分区存储管理实现，一个进程要占用多个分区
2. 硬件需要增加一组用户可见的段地址寄存器(代码段、数据段、堆栈段，附加段)，供地址转换使用
3. 存储管理需要增加设置一个段表，每个段占用一个段表项，包括：段始址、段限长，以及存储保护、可移动、可扩充等标志位

## 7.5. 段式存储管理的地址转换流程
| ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/28.png) | ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/85.png) |
| -------------------- | -------------------- |

> 使用终端来完成

## 7.6. 段的共享
1. 如果多个进程段表中的某段指向内存相同的地址，内存中以该处为起始地址的某段就可以被共享。
2. 对共享段的信息必须进行保护，如规定只能读出不能写入，不满足保护条件则产生保护中断
3. 为了方便共享，系统中常常建立一张共享段表记录所有共享段，包含段名、共享计数、段长、段首址、保护位等。

# 8. 分页和分段的寻址计算
| ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/37.png) | ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/38.png) |
| -------------------- | -------------------- |

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/39.png)

## 8.1. 分段和分页的比较
1. 分段是信息的**逻辑单位**，由源程序的逻辑结构所决定，**用户可见**
   1. 段长可根据**用户需要**来规定，段起始地址可从**任何主存地址**开始。
   2. 分段方式中，源程序(段号，段内位移)经连结装配后地址仍保持**二维结构**。
2. 分页是信息的物理单位，与源程序的逻辑结构无关，**用户不可见**，
   1. 页长由**系统**确定，页面只能以**页大小的整倍数地址**开始
   2. 分页方式中，源程序(页号，页内位移)经连结装配后地址变成了**一维结构**

## 8.2. 分页:逻辑地址到物理地址
| ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/65.png) | ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/66.png) |
| -------------------- | -------------------- |
| ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/67.png) | ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/68.png) |
| ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/69.png) |                      |

# 9. 段页式存储管理

## 9.1. 段页式存储管理的基本思想
1. **段式存储管理**可以基于**页式存储管理**实现
2. 每一段不必占据连续的存储空间，可存放在不连续的主存页架中
3. 能够扩充为段页式虚拟存储管理
4. 装入部分段，或者装入段中部分页面

## 9.2. 段页式存储管理的段表和页表
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/31.png)

1. 既有段表，也有页表
2. 段表中存储的是页表和页表始址

## 9.3. 段页式存储管理的地址转换
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/32.png)

# 10. 页式虚拟存储管理

## 10.1. 页式虚拟存储管理的基本原理
1. 将进程信息副本存放在外存中，当它被调度投入运行时，程序和数据没有全部装入内存，仅装入当前使用页面，进程执行过程中访问到不再内存的页面时，再由系统自动调入。
2. 页式虚拟存储是现代OS的**主流存储管理技术**
3. 请求页式存储管理：由于页面在需要时是根据进程请求装入内存的
5. 请求页式存储管理
   1. 优点：进程的程序和数据可按页分散存储在内存中，有利于内存利用率和多道程序运行
   2. 缺点：需要硬件支持、处理缺页中断、机器成本增加、系统开销加大，页内存在碎片。

### 10.1.1. 页式虚拟存储管理的页表
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/22.png)

1. 需要扩充页表项，至少包含如上信息，指出：
   1. 主存驻留标志：指出页面是否已经装入内存。1表示在内存中可以被正常访问，0表示不能立即访问，产生缺页异常。
   2. 修改位：被设置后，该页被调出内存前必须先写回磁盘，保障数据一致性
   3. 保护位：限制页面访问权限
   4. 引用位：在页面被引用无论是读写时设置，用来帮助系统进行页面淘汰。
   5. 内存块号：页面对应的页框号，用来地址转换。
2. 32位操作系统:32bit标识一个页表项
3. 页号是隐含信息，不是直接存储的信息。

## 10.2. 页式虚拟存储管理的实现
1. CPU处理地址
   1. 若页驻留，则获得块号形成绝对地址
   2. 若页不在内存，则CPU发出缺页中断
2. OS处理缺页中断
   1. 若有空闲页架，则根据辅存地址(虚存)调入页，更新页表与快表等
   2. 若无空闲页架，则决定淘汰页，调出已修改页，调入页，更新页表与快表

### 10.2.1. 页式虚拟存储管理的地址转换
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/23.png)
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/24.png)

1. 本指令没有被处理完，是在查找地址的时候发生的中断，所以要回退指令执行。
缺页中断完成后要**重新执行被中断指令**。

### 10.2.2. 页式虚拟存储管理的地址转换全过程
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/40.png)

1. 地址转换过程
   1. MMU接收CPU传送来的逻辑地址并自动按页面大小把它从某位起分解成两部分:页号和页内位移。
   2. 以页号为索引快速搜索快表TLB。
   3. 如果命中，立即送出页框号，并与贾内位移拼接成物理地址，然后进行访问权限检查，如获通过，进程就可以访问物理地址。
   4. 如果不命中，由硬件以页号为索引搜索页表，页表基址由硬件页表基址寄存器指出。
   5. 如果页表被命中，说明访问页面已在内存中，可送出页框号，并与页内位移拼接成物理地址，然后进行访问权限检查，如获通过，进程就可以访问物理地址，同时要把这个页面和页框信息装入快表TLB，以备再次访问。
   6. 如果发现页表中的对应页面失效，MMU发出缺页异常，请求操作系统进行处理，MMU工作到此结束。
2. MMU发现缺页并发出缺页异常，存储管理接收控制，进行缺页异常处理的过程如下：
   1. 挂起请求调页的进程。
   2. 根据页号搜索外页表，找到存放此页的磁盘物理地址。
   3. 查看内存是否有空闲页框，如有则分配一个，转(6)。
   4. 如果内存中无空闲页框，按照替换算法选择淘汰页面，检查其是否被写过或修改过，若否则转(6)，若是则转(5)。

### 10.2.3. TLB(快表)
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/35.png)

Note:快表存储正在进行的进程的若干(非连续)的页表项，其意义在于：快表访问速度高于内存，减少访问内存的次数，提高也是寻址效率

# 11. 页面调度
1. 当主存空间已满而又需要装入新页时，页式虚拟存储管理必须按照一定的算法将已在主存的一些页调出去
   1. 选择淘汰页的工作成为**页面调度**
   2. 选择淘汰页的算法称为**页面调度算法**

## 11.1. 交换区
1. 操作系统需要在磁盘上定义一个交换区用来保存临时换出的页面，交换区由磁盘上的一个或多个磁盘分区组成。
2. 简单做法：进程启动时，留出大小和进程一样大的交换分区。
3. 与进程对应的是其交换区的磁盘地址，即进程映像所保存的位置，这一信息记录在进程的外页表中。
4. 问题：进程启动可能会在增大，解决：将正文、数据和堆栈分别保留交换区，并且多留几块。
5. 交换区管理重点是维护交换区映射表，记录所有的呗换出内存的页面在交换区中的位置，以便需要时换入，第二次被换出内存时，当且仅当页面修改过才再次写入，否则直接抛弃。

## 11.2. 页面装入策略和清除策略
1. 页面装入策略用来解决何时将页面装入内存
2. 请页式：当产生缺页异常时调入页面
   1. 在替换时只有发生了更改才写回。
   2. 优点：只有被访问页面才会被调入，节省内存
   3. 缺点：缺页异常处理次数多，系统开销大。
3. 预调式：在使用页面前预先调入内存，操作系统根据某种算法动态预测进程最可能访问的界面，每次调入若干页面。
   1. 在替换前需要将他们都写回磁盘，可成批进行。
   2. 优点：减少磁盘I/O的启动次数，节省寻道和搜索时间。
   3. 缺点：如果调入的大多数界面都没有被使用则效率很低。

## 11.3. 页面分配策略
1. 请求分页虚存管理可能在缺页方面付出很大的代价，需要确定页面调度算法的作用范围是此进程的页面，还是内存中的所有进程的页面。
   1. 全局替换：不考虑进程属主
   2. 局部替换：仅限于进程本身
2. 分配方式
   1. 固定分配：进程生命周期中保持页框数固定不变，有平均分配、比例分配、优先权分配等方式。
   2. 可变分配：进程生命周期中所分得的页框数可变。
      1. 缺页率较高，说明局部性较差，可以适度提高分配的页框数。
      2. 缺页率较低，可以适度降低分配的页框数
3. 工作集(驻留集)：每个进程维护的一组页面。
4. 可变分配配合局部替换可以克服全局替换的缺点。

### 11.3.1. 固定分配，本地范围
1. 分配给进程的帧数是固定的
2. 从分配给过程的框架中选择要替换的页面

### 11.3.2. 变量分配，全局范围
1. 分配给进程的帧数是可变的
2. 从所有框架中选择要替换的页面
3. 最容易实现
4. 被许多操作系统采用
5. 操作系统保留空闲帧列表
6. 发生页面错误时，将空闲帧添加到驻留的进程集
7. 课本224全局页面替换策略和229局部页面替换策略

### 11.3.3. 变量分配，本地范围
1. 分配给进程的帧数是可变的
2. 从分配给过程的框架中选择要替换的页面
3. 添加新流程后，请根据应用程序类型，程序请求或其他条件分配页框数量
4. 发生页面错误时，请从发生故障的进程的常驻集中选择页面。
5. 不时重新评估分配

## 11.4. 缺页中断率
1. 页面调度算法设计不当，会出现(刚淘汰的页面立即又要调入，并如此反复)，这种现象称为**抖动**或**颠簸**，主要原因是内存中同时运行的进程太多，而分配给每个进程的页框太少。
2. 假定进程$P$共有$n$页，系统分配页架数$m$个
3. $P$运行中成功访问次数为$S$，不成功访问次数为$F$，总访问次数$A = S + F$
4. **缺页中断率**定义为: $f = \frac{F}{A}$
5. 缺页中断率是衡量存储管理性能和用户编程水平的重要依据

### 11.4.1. 缺页中断率的影响因素
1. 分配给进程的页框数：可用页框数越多，则缺页中断率就越低
2. 页面的大小：页面尺寸越大，则缺页中断率就越低
3. 页面替换算法：算法的优劣影响缺页异常次数
4. 程序特性：程序局部性要好，它对缺页中断率有很大影响。

### 11.4.2. 用户编程的例子
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/25.png)

> 不同的访问方式会到导致出现缺页情况的。

## 11.5. 全局页面替换策略

### 11.5.1. OPT页面调度算法(Belady算法)
1. 算法描述：当要调入新页面时，首先淘汰以后不再访问的页，然后选择**距现在最长时间后再访问**的页。
2. 该方法由Belady提出，称为BeLady算法，又称最佳算法(OPT)
3. OPT只可以**模拟**，不可以实现，因为永远无法预知之后的事情。
4. 这种算法可以用作衡量其他各种算法的标准。

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/41.png)

### 11.5.2. 先进先出页面调度算法(FIFO)
1. 算法描述：首先淘汰最先调入主存的那一页，或者说主存驻留时间最长的那一页(常驻的除外)
2. 模拟的是程序执行的顺序性，有一定合理性，并不能很好模拟程序的循环性。
3. 根据估计，缺页中断率也是最佳算法的2-3倍。

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/42.png)

4. **FIFO算法的Belady异常**：更多的页框导致了更高的缺页率，页框为3和4的时候

| ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/47.png) | ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/48.png) |
| -------------------- | -------------------- |

### 11.5.3. 页面缓冲算法
页面缓冲算法是对FIFO替换算法的一种改进

> 算法策略

2. 系统维护两个FIFO队列，被替换的页面添加到如下两个队列中
   1. 修改页面队列
   2. 非修改(空闲)页面队列
3. 替换的页面仍保留在内存中
   1. 如果再次引用，则找回的费用很少
   2. 当修改页面队列中的页面到达一定数量后，页面以群集形式写回磁盘，并把空闲页框加入非修改页面队列尾部。

### 11.5.4. 最近最少用LRU页面调度算法
1. 淘汰**最近一段时间较久未被访问**的那一页，即那些刚被使用过的页面，可以马上还要被使用到。
2. 模拟了程序执行的局部属性，既考虑了**循环性**，又兼顾了**顺序性**
3. 严格实现的代价大(需要维持特殊队列——页面淘汰队列)，实现需要硬件支持。

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/43.png)

4. LRU算法得到模拟实现：模拟是相当的不严谨，非常粗粒度的一个模拟。
   1. 引用位法：每页建立一个引用标志，供硬件使用，设置一个时间间隔中断，发生时将页引用标志置0，访问页面时将引用标志置为1，页面置换的时候选择标志为0的页面，在选中淘汰页时，将所有的页的引用为全部置为0
   2. 计数法：每页添加页面引用计数器，根据计数器选择最小的，定时清空页面引用计数器
   3. 计时法：每页添加计时单元，引用时，将绝对时间记录进入计时单元，定时清空计时单元。
   4. 老化算法：设置一个多位寄存器，被访问将最左侧设置为1，定时将寄存器右移，缺页中断时找到最小值的寄存器界面淘汰，被采用较多。

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/87.png)

1. 上图使用老化算法
2. T3时刻替换P2页面，因为和P1他们在3时刻都没有被访问，但是2时刻P1被访问了

### 11.5.5. 第二次机会页面替换算法(SCR，Second Chance Replacement)
1. 将FIFO算法和页表中引用位结合。
2. 算法描述：
   1. 首先检查FIFO页面队列队首
      1. 引用位为0，则淘汰该页面
      2. 引用位为1，将引用位清0，并将该页面移到队列尾部
   2. 如果第一遍全为1，则循环

### 11.5.6. 最不常用LFU的页面调度算法
1. 淘汰最近一段时间内**访问次数较少**的页面，对OPT的模拟性比LRU更好
2. 算法过程：基于时间间隔中断，并给每一页设置一个计数器，时间间隔中断发生后，所有计数器清0，每访问页1次就给计数器加1，选择计数最小的页面淘汰

### 11.5.7. 时钟CLOCK页面调度算法
1. CLOCK就是SCR结合FIFO形成循环，使用页引用标志位。
2. 算法描述：采用循环队列机制构造页面队列，形成了一个类似钟表面的环形表，队列指针则相当于钟表面上的表针，指向可能要淘汰的页面

#### 11.5.7.1. CLOCK算法的工作流程
1. 页面调入主存时，其引用标志位置为1
2. 访问主存页面时，其引用标志位置为1
3. 淘汰页面时，从指针当前指向的页面开始扫描循环队列
   1. 把所遇到的引用标志位是1的页面的引用标志位清0并跳过
   2. 把所遇到的引用标志位是0的页面淘汰，**指针推进一步**

#### 11.5.7.2. CLOCK算法的例子
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/44.png)

> 灰色和星号代表1，蓝色和无星号代表0
> 1. 发生命中，指针不动
> 2. 指针运动是为了寻找替换的页
> 3. 1->5其实是循环一轮，如果为1，指针不替换，但是会将标志位置为0

1. 当一页被替换时，指向下一帧。虽然早就进来，但是最近使用过，所以不急着替换
2. 当需要替换一页时，扫描缓冲区，查找使用位被置为0的一帧。
3. 每当遇到一个使用位为1的帧时，就将该位重新置为0；
4. 如果在这个过程开始时，所有帧的使用位均为0，选择遇到的第一个帧替换；
5. 如果所有帧的使用位为1，则指针在缓冲区中完整地循环一周，把所有使用位都置为0，并且停留在最初的位置上，替换该帧中的页。

| ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/45.png) | ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/46.png) |
| -------------------- | -------------------- |

#### 11.5.7.3. 第三次机会时钟替换算法：结合引用位和修改位
1. 一共四种情况：r是引用位，m是修改位
   1. 最近未被引用，未被修改：r=0，m=0
   2. 最近被引用，未被修改：r=1，m=0
   3. 最近未被引用，被修改：r=0，m=1
   4. 最近被引用，被修改：r=1，m=1
2. 算法描述
   1. 扫描，不修改引用位，找到第一个r=0，m=0的页面替换
   2. 如果1没有找到，则从原位置开始，**修改引用位**，查找r=0，m=1的页面替换写回
   3. 如果2没有找到，重复1或2操作。

### 11.5.8. 不同算法性能比较
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/49.png)

整体上来讲FIFO > CLOCK > LRU > OPT

## 11.6. 局部页面替换算法(不考)
P229-233

### 11.6.1. 局部最佳页面替换算法(MIN)
1. 实现思想：进程在时刻t访问某页面，如果该页面不在主存中，导致一次缺页，把该页面装入一个空闲页框
2. 不论发生缺页与否，算法在每一步要考虑引用串，如果该页面在时间间隔(t, t+τ)内未被再次引用，那么就移出；否则，该页被保留在进程驻留集中
3. t为一个系统常量，间隔(t, t+τ)称作滑动窗口 。例子中τ=3，双闭区间

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/50.png)
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/51.png)

### 11.6.2. 工作集模型和工作集置换算法(WS)
1. 进程工作集指"在某一段时间间隔内进程运行所需访问的页面集合"
2. 实现思想：工作集模型用来对局部最佳页面替换算法进行模拟实现，**不向前查看页面引用串，而是基于程序局部性原理向后看**
3. 任何给定时刻，**进程不久的将来所需主存页框数，可通过考查其过去最近的时间内的主存需求做出估计**

#### 11.6.2.1. 进程工作集
1. 指"在某一段时间间隔内进程运行所需访问的页面集合"，W(t，Δ)表示在时刻t-Δ到时刻t之间( (t-Δ，t))所访问的页面集合，进程在时刻t的工作集
2. Δ是系统定义的一个常量。变量Δ称为"工作集窗口尺寸"，可通过窗口来观察进程行为，还把工作集中所包含的页面数目称为"工作集尺寸"
3. Δ=3

#### 11.6.2.2. 示例
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/52.png)
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/53.png)
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/54.png)
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/55.png)

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/56.png)

> 工作集:程序在运行过程时，程序的局部性是变更的。有的部分是比较陡的，大量调入，然后平稳期，访问替换进来的页们。

### 11.6.3. 模拟工作集替换算法

### 11.6.4. 缺页频率替换算法
1. 定义页面错误率的上限U和下限L。
2. 如果缺页率高于U，则为进程分配更多页框。
3. 如果缺页率低于U，则为进程分配更少页框。
4. 驻留集的大小应该和工作集大小W紧密相关的。
5. 如果PFF(缺页率)>U并且没有更多可用帧，我们将暂停该过程，ROI(Return On Investment)

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/58.png)

> 页框的大小是需要根据程序动态调整的。

### 11.6.5. 通过工作集确定驻留集大小
1. 监视每个进程的工作集，只有属于工作集的页面才能留在主存；
2. 定期地从进程驻留集中删去那些不在工作集中的页面；
3. 仅当一个进程的工作集在主存时，进程才能执行。

# 12. 段式虚拟存储管理

## 12.1. 段式虚拟存储管理的基本思想
1. 把进程的所有分段都存放在辅存中，进程运行时先把当前需要的一段或几段装入主存，在执行过程中访问到不在主存的段时再把它们动态装入
2. 段式虚拟存储管理中段的调进调出是由OS自动实现的，**对用户透明**
3. 与段覆盖技术不同，它是用户控制的主存扩充技术，OS不感知

## 12.2. 段式虚拟存储管理的段表扩充
1. 段表的扩充
2. 特征位: 00(不在内存)01(在内存)11(共享段)
3. 存取权限: 00(可执行)01(可读)11(可写)
4. 扩充位: 0(固定长)1(可扩充)
5. 标志位: 00(未修改)01(已修改)11(不可移动)

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/29.png)

## 12.3. 段式虚拟存储管理的地址转换
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/30.png)

# 13. 段页式虚拟存储管理

## 13.1. 段页式虚拟存储基本原理
1. 虚地址以程序的逻辑结构划分为段，这是段页式的段式特征。
2. 实地址划分层位置固定、大小相同的页框(块)，这是段页式的页式特征。
3. 将每一段的线性地址空间划分成与页框大小相同的页面，段页式的特征
4. 逻辑地址由段号s、段内页号p和页内偏移d组成
   1. 对用户，虚拟地址由段号s和段内位移d'组成
   2. 系统内部将d'分解为p和d，d' = p * 块长 + d
5. 请求段页式虚拟存储管理的数据结构比较复杂，包含作业表、段表和页表三部分。
   1. 作业表：进入系统的作业和作业段表的起始地址
   2. 段表：是否在内存、段页表起始地址
   3. 页表：是否在内存、对应内存块号

## 13.2. 段页式虚拟存储管理的地址转换
| ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/33.png) | ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/88.png) |
| -------------------- | -------------------- |

# 14. 存储管理方案以及虚存页面替换算法小结
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/89.png)

# 15. 补充：关于快表问题
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/1.jpg)

> 有效位为0，不指引

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/2.jpg)

1. Valid并不全表示页表项是否在主存中
2. 发生页面替换的时候，被替换的页如果在快表中，则其的valid位置0或者将该页删除。

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/3.jpg)

# 16. Linux虚拟存储管理

## 16.1. 伙伴系统(一种算法)
1. 伙伴系统(Knuth，1973)，又称buddy算法，是一种**固定分区和可变分区**折中的主存管理算法
2. 基本原理是：任何尺寸为$2^i$的空闲块都可被分为两个尺寸为$2^{i-1}$的空闲块，这两个空闲块称作**伙伴**，它们可以被合并成尺寸为$2^i$的原先空闲块。
3. 伙伴通过对大块的物理主存划分而获得
   1. 假如从第0个页面开始到第3个页面结束的主存
   2. 每次都对半划分，那么第一次划分获得大小为2页的伙伴，如0、1和2、3
   3. 进一步划分，可以获得大小为1页的伙伴，例如0和1，2和3

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/60.png)

### 16.1.1. 例子：类似二叉树的形式进行分配

| ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/61.png) | ![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/62.png) |
| -------------------- | -------------------- |

### 16.1.2. Linux伙伴系统
1. 以page结构为数组元素的`mem_map[]`数组
2. 以free_area_struct结构为数组元素的free_area数组
3. 位图数组(bitmap)

#### 16.1.2.1. Linux基于伙伴的slab分配器
1. 为什么要使用slab分配器?
   1. 伙伴系统以**页框**为基本分配单位，内核在很多情况下，**需要的主存量远远小于页框大小**，如inode、vma、task_struct等，为了更经济地使用内核主存资源，引入**SunOS操作系统中首创的基于伙伴系统的slab分配器**，其基本思想是：为经常使用的小对象建立缓存，小对象的申请与释放都通过slab分配器来管理，仅当缓存不够用时才向伙伴系统申请更多空间。//页内可以按2的幂次拆分。
   2. 优点：**充分利用主存，减少内部碎片**，对象管理局部化，尽可能少地与伙伴系统打交道，从而提高效率。
2. slab的结构

```c++
struct slab{
   struct list_head;          // slab满、半满或空闲链表
   unsigned long colouoff;    //slab着色偏移量
   void * s_mem;              //slab的第一个对象
   unsigned int inuse;        //已分配的对象数
   kmem_bufctl_t free;        //第一个空闲对象
}
```

3. slab的操作

![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/63.png)
![](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-OS/img/lec3/64.png)

#### 16.1.2.2. slab分配器主要操作
1. kmem_cache_create()函数：创建专用cache，规定对象的大小和slab的构成，并加入cache管理队列；
2. kmem_cache_alloc()与kmem_cache_free()函数：分别用于分配和释放一个拥有专用slab队列的对象；
3. kmem_cache_grow()与kmem_cache_reap()函数：
   1. kmem_cache_grow()它向伙伴系统申请向cache增加一个slab
   2. kmem_cache_reap()用于定时回收空闲slab
4. kmem_cache_destroy()与kmem_cache_shrink()：用于cache的销毁和收缩；
5. kmalloc()与kfree()函数：用来从通用的缓冲区队列中申请和释放空间；
6. kmem_getpages()与kmem_freepages()函数：slab与页框级分配器的接口，当slab分配器要创建新的slab或cache时，通过kmem_getpages()向内核提供的伙伴算法来获得一组连续页框。如果释放分配给slab分配器的页框，则调用kmem_freepages()函数。