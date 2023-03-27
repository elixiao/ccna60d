# 第 32 天 快速生成树协议

**Rapid Spanning Tree Protocol**

___

Gitbook：[ccna60d.xfoss.com](https://ccna60d.xfoss.com/)


你可以在 https://github.com/gnu4cn/ccna60d 上 fork 本项目，并提交你的修正。


本书结合了学习技巧，包括阅读、复习、背书、测试以及 hands-on 实验。

> 本书译者用其业余时间完成本书的翻译工作，并将其公布到网上，以方便你对网络技术的学习掌握，为使译者更有动力改进翻译及完成剩下章节，你可以 [捐赠译者](https://github.com/gnu4cn/buy-me-a-coffee)。

___

#第 32 天任务

- 阅读今天的课文
- 复习昨天的课文
- 完成今天的实验
- 阅读 ICND2 记诵指南
- 在网站 [subnetting.org/](http://subnetting.org/) 花 15 分钟

IEEE 802.1D标准是在连通性从失去到恢复需要一分钟左右，就被认为性能已经可观的时期设计出来的。在IEEE 802.1D STP下，恢复大约需要 50 秒，这其中包括 20 秒的最大老化计时器（the Max Age timer）超时，以及额外的给端口从阻塞状态过渡到转发状态的 30 秒。

随着计算机技术的进化，网络变得更为重要，更为快速的网络收敛显然是人们所需要的。思科通过开发一些包括骨干快速（Backbone Fast）及上行快速（Uplink Fast）等专有的 STP 增强，来满足此需求。

今天你将学到以下知识。

- RSTP的需求, the need for RSTP
- 配置 RSTP ，RSTP configuration

本课对应了以下 CCNA 大纲要求。

+ 认识增强的交换技术，identify enhanced switching technologies
    - RSTP
    - PVSTP

## RSTP的需求

**the Need for RSTP**

随着技术的持续演化，以及在同一物理平台上路由及交换的融合，在诸如 OSPF 及 EIGRP 这样的可以在更短时间内提供出替代路径的路由协议面前，交换网络的延迟就变得明显起来。802.1W标准就被设计出来解决此问题。

IEEE 802.1W标准，或者是快速生成树协议（Rapid Spanning Tree Protocol, RSTP）, 显著地缩短了在某条链路失效时， STP 用于收敛的时间。在 RSTP 下，网络从故障切换到一条替代路径或链路可在亚秒级别完成（with RSTP, network failover to an alternate path or link can occur in a subsecond timeframe）。 RSTP 是802.1D的一个扩展，执行与上行快速及骨干快速类似的功能。**RSTP比传统的 STP 执行得更好，且无需额外配置。此外， RSTP 向后兼容最初的IEEE 802.1D STP标准。**其通过使用一种如下面的截屏中所示的修改的 BPDU ，实现的向后兼容。

![修改的BPDU](images/3201.png)

*图 32.1 -- 修改的BPDU*

RSTP的各种端口状态可如下这样与 STP 端口状态对应起来。

- 关闭 -- 丢弃，Disabled -- Discarding
- 阻塞 -- 丢弃，Blocking -- Discarding
- 侦听 -- 丢弃，Listening -- Discarding
- 学习 -- 学习，Learning -- Learning
- 转发 -- 转发，Forwarding -- Forwarding

RSTP包含了以下的端口角色。

- 根端口（转发状态）, Root(Forwarding state)
- 指定端口（转发状态），Designated(Forwarding state)
- 可变端口（阻塞状态），Alternate(Blocking state)
- 备份端口（阻塞状态），Bakup(Blocking state)

对于考试，掌握上面这些着重号标记的内容是非常重要的，尤其是哪些端口状态转发流量（一旦网络完成收敛）。图32.2及32.3分别演示了一个 RSTP 可变端口及一个 RSTP 备份端口。

![RSTP可变端口](images/3202.png)

*图 32.2 -- RSTP可变端口*

![RSTP备份端口](images/3203.png)

*图 32.3 -- RSTP备份端口*

### 带有PVST+的RSTP

**RSTP with PVST+**

加强版的基于各 VLAN 的生成树允许每个 VLAN 都有一个单独的 STP 实例（Per VLAN Spanning Tree Plus(PVST+) allows for an individual STP instance per VLAN）。传统或普通的PVST+模式在出现某条链路失效时，在网络收敛中，依赖较旧的802.1D STP的使用。

### RPVST+

快速的基于各 VLAN 的生成树加强版，允许与PVST+ 一起使用802.1W（Rapid Per VLAN Spanning Tree Plus(RPVST+) allows for the use of 802.1W with PVST+）。这就允许在每个 VLAN 都有一个单独的 RSTP 实例的同时，提供比起802.1D STP所能提供的更为快速的收敛。**默认情况下，在某台思科交换机上开启 RSTP 时，也就在该交换机上开启了R-PVST+。**

这里有一些可用来记住IEEE STP规格字母命名的记忆窍门。

- 802.1D（“经典的”生成树） -- It's dog-gone slow
- 802.1W(快速生成树) -- Imagine Elmer Fudd saying "rapid" as "wapid"
- 802.1S（多生成树） -- You add the letter "s" to nouns to make them plural(multiple) but this is a CCNP SWITCH subject

## RSTP的配置

**Configuring RSTP**

RSTP的配置只需一个命令！

```console
Switch(config)#spanning-tree mode rapid-pvst
Switch#show spanning-tree summary
Switch is in rapid-pvst mode
Root bridge for: VLAN0050, VLAN0060, VLAN0070
```

## 第 32 天问题

**Day 32 Questions**

1. RSTP is not backward compatible with the original IEEE 802.1D STP standard. True or false?
2. What are the RSTP port states?
3. What are the four RSTP port roles?
4. Which command enables RSTP?
5. By default, when RSTP is enabled on a Cisco switch, R-PVST+ is enabled on the switch. True or false?

## 第 32 天问题答案

**Day 32 Answers**

1. False.
2. Discarding, Learning, and Forwarding.
3. Root, Designated, Alernate, and Backup.
4. The `spanning-tree mode rapid-pvst` command.
5. True.

## 第 32 天实验

**Day 32 Lab**

### RSTP实验

**RSTP Lab**

**拓扑图**

![RSTP实验拓扑图](images/3204.png)

**实验目的**

学习 RSTP 的配置命令。

**实验步骤**

1. 检查交换机上的生成树模式。


    ```console
    SwitchA#show spanning-tree summary
    Switch is in pvst mode
    Root bridge for: VLAN0002 VLAN0003
    ```

2. 将模式改为 RSTP 并再度检查。


    ```console
    SwitchA(config)#spanning-tree mode rapid-pvst
    SwitchA#show spanning-tree summary
    Switch is in rapid-pvst mode
    Root bridge for: VLAN0002 VLAN0003
    ```

3. 用 RSTP 模式来重复第 31 天的实验。

4. 你可以预先预测出那些端口将是根/指定/阻塞端口吗（can you predict which ports will be Root/Designated/Blocking beforehand）？
