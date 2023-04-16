---
title: Zookeeper基本原理+通俗理解+安装使用
tags: zookeeper 分布式 rpc
categories: RPC
abbrlink: c8e2efea
date: 2022-02-20 10:33:41
---

<!--more-->

<h1>通俗理解 参考</h1>

<p><a class="has-card" data-link-desc="很多中间件，比如Kafka、Hadoop、HBase，都用到了 Zookeeper，于是很多人就会去了解这个 Zookeeper 到底是什么，为什么它在分布式系统里有着如此无可替代的地位。 在踩了很多坑之后，我决定来回答下这个问题。 其…" data-link-icon="https://static.zhihu.com/heifetz/assets/apple-touch-icon-152.a53ae37b.png" data-link-title="为什么需要 Zookeeper - 知乎" href="https://zhuanlan.zhihu.com/p/69114539" title="为什么需要 Zookeeper - 知乎"><span class="link-card-box"><span class="link-title">为什么需要 Zookeeper - 知乎</span><span class="link-desc">很多中间件，比如Kafka、Hadoop、HBase，都用到了 Zookeeper，于是很多人就会去了解这个 Zookeeper 到底是什么，为什么它在分布式系统里有着如此无可替代的地位。 在踩了很多坑之后，我决定来回答下这个问题。 其…</span><span class="link-link"><img alt="" class="link-link-icon" src="https://static.zhihu.com/heifetz/assets/apple-touch-icon-152.a53ae37b.png" />https://zhuanlan.zhihu.com/p/69114539</span></span></a></p>

<p><a class="has-card" data-link-desc="ZooKeeper简介 ZooKeeper是一个开放源码的分布式应用程序协调服务，它包含一个简单的原语集，分布式应用程序可以基于它实现同步服务，配置维护和命名服务等。 ZooKeeper设计目的 1." data-link-icon="https://common.cnblogs.com/favicon.svg" data-link-title="ZooKeeper基本原理 - 阿凡卢 - 博客园" href="https://www.cnblogs.com/luxiaoxun/p/4887452.html" title="ZooKeeper基本原理 - 阿凡卢 - 博客园"><span class="link-card-box"><span class="link-title">ZooKeeper基本原理 - 阿凡卢 - 博客园</span><span class="link-desc">ZooKeeper简介 ZooKeeper是一个开放源码的分布式应用程序协调服务，它包含一个简单的原语集，分布式应用程序可以基于它实现同步服务，配置维护和命名服务等。 ZooKeeper设计目的 1.</span><span class="link-link"><img alt="" class="link-link-icon" src="https://common.cnblogs.com/favicon.svg" />https://www.cnblogs.com/luxiaoxun/p/4887452.html</span></span></a></p>

<p>粗暴一点的理解，可以认为zookeeper可以进行服务注册和发现，就像电话簿一样，我把一个电话先注册到电话簿中（<strong>姓名：电话</strong>），在有需要的时候进行<strong>“服务发现”</strong>——找到<strong>相应姓名的电话</strong>。</p>

<p>相当于中介，之前的<a data-link-desc="目录RPC是什么？应用场景RPC 优点写RPC框架需要具备哪些知识？RPC原理（摘自：什么情况下使用 RPC ？ - 知乎）Netty框架具体代码代码目录apiproviderconsumerproxyregistryprotocol本地调用和远程调用 产生的对象有什么区别呢？RPC的调用速度如何？RPC是什么？RPC是远程过程调用（Remote Procedure Call）的缩写形式。通俗的理解就是我调用了一个函数func(args" data-link-icon="https://g.csdnimg.cn/static/logo/favicon32.ico" data-link-title="手写RPC框架（文末附代码）_trigger的博客-CSDN博客" href="https://blog.csdn.net/weixin_40757930/article/details/122908918" title="手写RPC框架（文末附代码）_trigger的博客-CSDN博客">手写RPC框架（文末附代码）_trigger的博客-CSDN博客</a>这个博客中是采用的<strong>map</strong>存储 <strong><span style="color:#fe2c24;">“  服务名：服务地址  ”</span></strong> 信息的，现在把这个map换成了zk。</p>

<p>简单的图示</p>

<p><img alt="" height="301" src="https://img-blog.csdnimg.cn/7a2debe9f4474461bdc0fafe68a05425.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_12,color_FFFFFF,t_70,g_se,x_16" width="556" /></p>

<p></p>

<h1>安装参考</h1>

<p><a class="has-card" data-link-desc="第一步（下载安装包）先准备安装包，这里我推荐在Apache官网下载（地址：https://zookeeper.apache.org/releases.html）。因为这篇文章是为后续dubbo+zk+mybatis+springBoot的教程做铺垫，故选用windows版本做讲解方便各位读者快速上手，后面我会写linux环境下的安装配置及使用。关联dubbo请看https://blog.csd..." data-link-icon="https://g.csdnimg.cn/static/logo/favicon32.ico" data-link-title="windows环境下安装zookeeper教程详解（单机版）_风轩雨墨的博客-CSDN博客_zookeeper安装教程" href="https://blog.csdn.net/qq_33316784/article/details/88563482" title="windows环境下安装zookeeper教程详解（单机版）_风轩雨墨的博客-CSDN博客_zookeeper安装教程"><span class="link-card-box"><span class="link-title">windows环境下安装zookeeper教程详解（单机版）_风轩雨墨的博客-CSDN博客_zookeeper安装教程</span><span class="link-desc">第一步（下载安装包）先准备安装包，这里我推荐在Apache官网下载（地址：https://zookeeper.apache.org/releases.html）。因为这篇文章是为后续dubbo+zk+mybatis+springBoot的教程做铺垫，故选用windows版本做讲解方便各位读者快速上手，后面我会写linux环境下的安装配置及使用。关联dubbo请看https://blog.csd...</span><span class="link-link"><img alt="" class="link-link-icon" src="https://g.csdnimg.cn/static/logo/favicon32.ico" />https://blog.csdn.net/qq_33316784/article/details/88563482</span></span></a></p>

<p>安装后自动服务器即可启动zk服务，可以通过客户端或者编程（创建zkClient对象）去使用zk。</p>

<p></p>

<p></p>

<p></p>

<p></p>
