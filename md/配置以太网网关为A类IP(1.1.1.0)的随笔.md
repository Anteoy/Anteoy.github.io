---
date: 2016-11-24 23:25:00
title: 配置以太网网关为A类IP(1.1.1.0)的随笔
categories:
    - 架构
tags:
---

##引子
　　*今天心血来潮，准备把自有网络路由网关修改为1.1.1.0，主机地址修改为1.1.1.1-1.1.1.254（虽然这样部分的公网ip地址不能访问，但对于实用并没有影响，因为这部分网站我没有使用需求），广播地址修改为1.1.1.255，由此扩展出一些东西，在此做一记录，以供后时参考，若有错误或不妥的地方，欢迎大家不吝赐教。*


##过程

    1. 在此之前，我首先去查阅了部分资料，重新温习了下以往认知的ip分类。在此记录一下。

- A 1.0.0.0-126.255.255.255  默认掩码-255.0.0.0

- B 128.0.0.0-191.255.255.255 默认掩码-255.255.0.0

- C 192.0.0.0-223.255.255.255 默认掩码-255.255.255.0

- D 保留地址，主要用于多点广播

- E 一种为将来使用的保留地址

*注：简单地说，ip为32位二进制组成，每一段ip地址的开头如1.0.0.0为当前网段的主机地址（即网关地址），而末尾如1.0.0.255为当前网段的广播地址，这两种地址是不能作为主机地址的。*

    2. 我开始进入路由管理界面配置路由，最开始配置网关为1.1.1.1,掩码配置为255.255.255.0,起止ip配置的1.1.1.2-1.1.1.254,确认，一切正常分发。

    3. 后来把网关修改为1.1.1.0,本来准备只路由6台主机，于是偷了下懒，在网上找了一个计算子网掩码的在线计算程序，配置掩码为255.255.255.248，于是问题就出现了，经过一番折腾，最后发现是因为子网掩码配置有误，最后修改子网掩码为255.0.0.0,重启，正常使用，下面补充下关于子网掩码的问题。

##补充
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这里主要补充下关于子网掩码的问题，ip地址由32位二进制数组成，前面已经提及过，并且ip=网络号+主机号，而在网络路由识别中，因为都是一串二进制数据，如何让机器识别哪一ip部分的数字代表网络号（子网），哪一部分属于主机号，于是便有了子网掩码，子网掩码，也是32位的二进制数据，所谓掩，用1表示，而0则表示暴露，比如1.1.1.1/25,则子网掩码为25，掩为25个1,暴露为32-25=7个0,即为11111111 11111111 11111111 10000000，另外还应记住一个规律，子网掩码与实际ip地址按位与，即可得出当前的网络号，比如1.1.1.0等，同时由此便可得出与之对应的广播地址。关于更加详细地解释可以参考下这位朋友的 [知乎分享](https://www.zhihu.com/question/29723388)





