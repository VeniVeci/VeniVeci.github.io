---
title: Netty面试题整理（基础知识点）
tags: java 面试 netty
categories: RPC Netty
abbrlink: e68fa4d
date: 2022-02-16 11:38:21
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="Netty%20%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F-toc" style="margin-left:0px;"><a href="#Netty%20%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F">Netty 是什么？</a></p>

<p id="%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E2%BD%A4%20Netty%EF%BC%9F-toc" style="margin-left:0px;"><a href="#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E2%BD%A4%20Netty%EF%BC%9F">为什么要⽤ Netty？</a></p>

<p id="Netty%20%E5%BA%94%E2%BD%A4%E5%9C%BA%E6%99%AF%E6%9C%89%E5%93%AA%E4%BA%9B%EF%BC%9F-toc" style="margin-left:0px;"><a href="#Netty%20%E5%BA%94%E2%BD%A4%E5%9C%BA%E6%99%AF%E6%9C%89%E5%93%AA%E4%BA%9B%EF%BC%9F">Netty 应⽤场景有哪些？</a></p>

<p id="Netty%20%E6%A0%B8%E2%BC%BC%E7%BB%84%E4%BB%B6%E6%9C%89%E5%93%AA%E4%BA%9B%EF%BC%9F%E5%88%86%E5%88%AB%E6%9C%89%E4%BB%80%E4%B9%88%E4%BD%9C%E2%BD%A4%EF%BC%9F-toc" style="margin-left:0px;"><a href="#Netty%20%E6%A0%B8%E2%BC%BC%E7%BB%84%E4%BB%B6%E6%9C%89%E5%93%AA%E4%BA%9B%EF%BC%9F%E5%88%86%E5%88%AB%E6%9C%89%E4%BB%80%E4%B9%88%E4%BD%9C%E2%BD%A4%EF%BC%9F">Netty 核⼼组件有哪些？分别有什么作⽤？</a></p>

<p id="1.Channel-toc" style="margin-left:40px;"><a href="#1.Channel">1.Channel</a></p>

<p id="2.EventLoop-toc" style="margin-left:40px;"><a href="#2.EventLoop">2.EventLoop</a></p>

<p id="3.Channel%E7%9A%84Handler%20%E5%92%8CPipeline-toc" style="margin-left:40px;"><a href="#3.Channel%E7%9A%84Handler%20%E5%92%8CPipeline">3.Channel的Handler 和Pipeline</a></p>

<p id="EventloopGroup%20%E4%BA%86%E8%A7%A3%E4%B9%88%3F%E5%92%8C%20EventLoop%20%E5%95%A5%E5%85%B3%E7%B3%BB%3F-toc" style="margin-left:0px;"><a href="#EventloopGroup%20%E4%BA%86%E8%A7%A3%E4%B9%88%3F%E5%92%8C%20EventLoop%20%E5%95%A5%E5%85%B3%E7%B3%BB%3F">EventloopGroup 了解么? 和 EventLoop 啥关系?</a></p>

<p id="Bootstrap%20%E5%92%8C%20ServerBootstrap%20%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F-toc" style="margin-left:0px;"><a href="#Bootstrap%20%E5%92%8C%20ServerBootstrap%20%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F">Bootstrap 和 ServerBootstrap 是什么？</a></p>

<p id="NioEventLoopGroup%20%E9%BB%98%E8%AE%A4%E7%9A%84%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0%E4%BC%9A%E8%B5%B7%E5%A4%9A%E5%B0%91%E7%BA%BF%E7%A8%8B%EF%BC%9F-toc" style="margin-left:0px;"><a href="#NioEventLoopGroup%20%E9%BB%98%E8%AE%A4%E7%9A%84%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0%E4%BC%9A%E8%B5%B7%E5%A4%9A%E5%B0%91%E7%BA%BF%E7%A8%8B%EF%BC%9F">NioEventLoopGroup 默认的构造函数会起多少线程？</a></p>

<p id="Netty%20%E7%BA%BF%E7%A8%8B%E6%A8%A1%E5%9E%8B%E4%BA%86%E8%A7%A3%E4%B9%88%EF%BC%9F-toc" style="margin-left:0px;"><a href="#Netty%20%E7%BA%BF%E7%A8%8B%E6%A8%A1%E5%9E%8B%E4%BA%86%E8%A7%A3%E4%B9%88%EF%BC%9F">Netty 线程模型了解么？</a></p>

<p id="%E4%BB%80%E4%B9%88%E6%98%AF%20TCP%20%E7%B2%98%E5%8C%85%2F%E5%8D%8A%E5%8C%85%3F%E6%9C%89%E4%BB%80%E4%B9%88%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95%E5%91%A2%EF%BC%9F-toc" style="margin-left:0px;"><a href="#%E4%BB%80%E4%B9%88%E6%98%AF%20TCP%20%E7%B2%98%E5%8C%85%2F%E5%8D%8A%E5%8C%85%3F%E6%9C%89%E4%BB%80%E4%B9%88%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95%E5%91%A2%EF%BC%9F">什么是 TCP 粘包/半包?有什么解决办法呢？</a></p>

<p id="Netty%20%E2%BB%93%E8%BF%9E%E6%8E%A5%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F-toc" style="margin-left:0px;"><a href="#Netty%20%E2%BB%93%E8%BF%9E%E6%8E%A5%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F">Netty 长连接是什么，应用场景有哪些？</a></p>

<p id="%E9%95%BF%E8%BF%9E%E6%8E%A5%E5%92%8C%E7%9F%AD%E8%BF%9E%E6%8E%A5%E7%9A%84%E5%8C%BA%E5%88%AB%EF%BC%9A-toc" style="margin-left:40px;"><a href="#%E9%95%BF%E8%BF%9E%E6%8E%A5%E5%92%8C%E7%9F%AD%E8%BF%9E%E6%8E%A5%E7%9A%84%E5%8C%BA%E5%88%AB%EF%BC%9A">长连接和短连接的区别：</a></p>

<p id="Netty%E2%BC%BC%E8%B7%B3%E6%9C%BA%E5%88%B6%E4%BA%86%E8%A7%A3%E4%B9%88%EF%BC%9F-toc" style="margin-left:0px;"><a href="#Netty%E2%BC%BC%E8%B7%B3%E6%9C%BA%E5%88%B6%E4%BA%86%E8%A7%A3%E4%B9%88%EF%BC%9F">Netty心跳机制了解么？</a></p>

<p id="%E7%AE%80%E5%8D%95%E7%9A%84%E5%BF%83%E8%B7%B3%E6%9C%BA%E5%88%B6%E5%AE%9E%E7%8E%B0%EF%BC%9A-toc" style="margin-left:40px;"><a href="#%E7%AE%80%E5%8D%95%E7%9A%84%E5%BF%83%E8%B7%B3%E6%9C%BA%E5%88%B6%E5%AE%9E%E7%8E%B0%EF%BC%9A">简单的心跳机制实现：</a></p>

<p id="Netty%20%E7%9A%84%E9%9B%B6%E6%8B%B7%E8%B4%9D-toc" style="margin-left:0px;"><a href="#Netty%20%E7%9A%84%E9%9B%B6%E6%8B%B7%E8%B4%9D">Netty 的零拷贝是什么，有什么用？</a></p>

<p id="%E4%BC%A0%E7%BB%9F%E7%9A%84%E6%8B%B7%E8%B4%9D-toc" style="margin-left:40px;"><a href="#%E4%BC%A0%E7%BB%9F%E7%9A%84%E6%8B%B7%E8%B4%9D">传统的拷贝</a></p>

<p id="%E9%9B%B6%E6%8B%B7%E8%B4%9D-toc" style="margin-left:40px;"><a href="#%E9%9B%B6%E6%8B%B7%E8%B4%9D">零拷贝</a></p>

<hr id="hr-toc" /><p style="margin-left:0;text-align:left;">参考：1.《v4.0-JavaGuide面试突击版》</p>

<p style="margin-left:0;text-align:left;">           2.黑马课程</p>

<p style="margin-left:0;text-align:left;"><a class="has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M0H8" data-link-title="黑马程序员Netty全套教程，全网最全Netty深入浅出教程，Java网络编程的王者_哔哩哔哩_bilibili现在的互联网环境下，分布式系统大行其道，而分布式系统的根基在于网络编程，而 Netty 恰恰是 Java 领域网络编程的王者。如果要致力于开发高性能的服务器程序、高性能的客户端程序，必须掌握 Netty，而本课程的目的就是带领你进入基于 Netty 的网络编程世界。本课程从基础 NIO 编程开始，到 Netty 入门及进阶，参数优化到源码分析，由浅入深，为 Netty 学习打下坚实基础。本课程的目正在上传…重新上传取消https://www.bilibili.com/video/BV1py4y1E7oA?p=1" href="https://www.bilibili.com/video/BV1py4y1E7oA?p=1" title="黑马程序员Netty全套教程，全网最全Netty深入浅出教程，Java网络编程的王者_哔哩哔哩_bilibili现在的互联网环境下，分布式系统大行其道，而分布式系统的根基在于网络编程，而 Netty 恰恰是 Java 领域网络编程的王者。如果要致力于开发高性能的服务器程序、高性能的客户端程序，必须掌握 Netty，而本课程的目的就是带领你进入基于 Netty 的网络编程世界。本课程从基础 NIO 编程开始，到 Netty 入门及进阶，参数优化到源码分析，由浅入深，为 Netty 学习打下坚实基础。本课程的目正在上传…重新上传取消https://www.bilibili.com/video/BV1py4y1E7oA?p=1"><span class="link-card-box"><span class="link-title">黑马程序员Netty全套教程，全网最全Netty深入浅出教程，Java网络编程的王者_哔哩哔哩_bilibili现在的互联网环境下，分布式系统大行其道，而分布式系统的根基在于网络编程，而 Netty 恰恰是 Java 领域网络编程的王者。如果要致力于开发高性能的服务器程序、高性能的客户端程序，必须掌握 Netty，而本课程的目的就是带领你进入基于 Netty 的网络编程世界。本课程从基础 NIO 编程开始，到 Netty 入门及进阶，参数优化到源码分析，由浅入深，为 Netty 学习打下坚实基础。本课程的目正在上传…重新上传取消https://www.bilibili.com/video/BV1py4y1E7oA?p=1</span><span class="link-link"><img alt="" class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release1.9.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M0H8" />https://www.bilibili.com/video/BV1py4y1E7oA?p=1</span></span></a></p>

<h1 id="Netty%20%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F" style="text-align:left;"><strong><strong><span style="color:#242a31;">Netty 是什么？</span></strong></strong></h1>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">1. Netty 是⼀个 </span><strong><span style="color:#3b454e;">基于 NIO </span></strong><span style="color:#3b454e;">的 client-server(客户端服务器)框架，使⽤它可以快速简单地开发⽹ </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">络应⽤程序。 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">2. 它极⼤地简化并优化了 TCP 和 UDP 套接字服务器等⽹络编程,并且性能以及安全性等都更好。</span></p>

<p style="margin-left:.0001pt;text-align:left;"><strong>Spring之于javaEE，Netty之于网络通信。</strong></p>

<p style="margin-left:.0001pt;text-align:left;"></p>

<h1 id="%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E2%BD%A4%20Netty%EF%BC%9F" style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#242a31;">为什么要⽤ Netty？</span></strong></h1>

<p style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#242a31;">性能上优于原生的Java API，开发效率高，网络吞吐量大。</span></strong></p>

<p style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#242a31;">很多人都在用，很多项目都在用，可靠。</span></strong></p>

<p style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#242a31;">具体的：</span></strong></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">1.统⼀的 API，⽀持多种传输类型，阻塞和⾮阻塞的。</span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">2.简单⽽强⼤的线程模型。 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">3.⾃带编解码器解决 TCP 粘包/拆包（半包）问题。 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">4.⾃带各种协议栈。</span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">5.⽐直接使⽤ Java 核⼼ API 有更⾼的吞吐量、更低的延迟、更低的资源消耗和更少的内存复 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">制。</span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">6.成熟稳定，经历了⼤型项⽬的使⽤和考验，⽽且很多开源项⽬都使⽤到了 Netty， ⽐如我们 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">经常接触的 </span><strong><span style="color:#3b454e;">Dubbo</span></strong><span style="color:#3b454e;">、</span><strong><span style="color:#3b454e;">RocketMQ </span></strong><span style="color:#3b454e;">等等。</span></p>

<p style="margin-left:.0001pt;text-align:left;"></p>

<h1 id="Netty%20%E5%BA%94%E2%BD%A4%E5%9C%BA%E6%99%AF%E6%9C%89%E5%93%AA%E4%BA%9B%EF%BC%9F" style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#242a31;">Netty 应⽤场景有哪些？</span></strong></h1>

<p style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#3b454e;">作为 RPC 框架的⽹络通信⼯具 (分布式应用会用到）</span></strong></p>

<p style="margin-left:.0001pt;text-align:left;"><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M0H8" data-link-title="手写RPC框架（文末附代码）_trigger的博客-CSDN博客" href="https://blog.csdn.net/weixin_40757930/article/details/122908918" title="手写RPC框架（文末附代码）_trigger的博客-CSDN博客">手写RPC框架（文末附代码）_trigger的博客-CSDN博客</a></p>

<p style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#3b454e;">实现⼀个 HTTP 服务器</span></strong></p>

<p style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#3b454e;">实现⼀个即时通讯系统</span></strong></p>

<p style="margin-left:.0001pt;text-align:left;"><strong>实现消息推送系统</strong></p>

<p style="margin-left:.0001pt;text-align:left;"></p>

<h1 id="Netty%20%E6%A0%B8%E2%BC%BC%E7%BB%84%E4%BB%B6%E6%9C%89%E5%93%AA%E4%BA%9B%EF%BC%9F%E5%88%86%E5%88%AB%E6%9C%89%E4%BB%80%E4%B9%88%E4%BD%9C%E2%BD%A4%EF%BC%9F" style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#242a31;">Netty 核⼼组件有哪些？分别有什么作⽤？</span></strong></h1>

<p style="margin-left:.0001pt;text-align:left;">比如现在有服务器和客户端，场景是PRC调用。</p>

<p style="margin-left:.0001pt;text-align:left;">服务器已经启动，有一个核心组件正在运行，这个组件就是事件循环<strong><span style="color:#242a31;">EventLoop</span></strong>，它不断的监听当前端口是否有请求（连接请求）。</p>

<p style="margin-left:.0001pt;text-align:left;">客户端通过<strong>channel</strong>去bind对应的ip+port，建立连接。</p>

<p style="margin-left:.0001pt;text-align:left;">然后客户端就可以发送请求，发送的时候涉及到核心组件<strong>pipeline</strong>和<strong>handler</strong>，最简单的，需要将自己的请求通过编码器这个<strong>handler</strong>编码后传输（<strong>handler</strong>在<strong>pipeline</strong>中），编码后的信息来到服务器的<strong>channel</strong>后，又进入服务器的<strong>pipeline</strong>，通过解码器这个<strong>handler</strong>解码，交由<strong><span style="color:#242a31;">EventLoop</span></strong>处理请求，得到结果后再编码返回。</p>

<p style="margin-left:.0001pt;text-align:left;"><strong>具体的：</strong></p>

<h2 id="1.Channel" style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#242a31;">1.Channel </span></strong></h2>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">Channel 接⼝是 Netty 对⽹络操作抽象类，它除了包括基本的 I/O 操作，如 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">bind() 、 connect() 、 read() 、 write() 等。</span></p>

<h2 id="2.EventLoop" style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#242a31;">2.EventLoop</span></strong></h2>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">EventLoop 的主要作⽤实际就是负责监听⽹络事件并调⽤事件处理器进⾏相关 I/O 操作的处理。</span></p>

<h2 id="3.Channel%E7%9A%84Handler%20%E5%92%8CPipeline" style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#242a31;">3.Channel的Handler 和Pipeline</span></strong></h2>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">Handler 是消息的具体处理器。他负责处理读写操作、客户端连接等事情。 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">Pipeline 为Handler 的链，提供了⼀个容器并定义了⽤于沿着链传播⼊站和出站 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">事件流的 API 。当Channel 被创建时，它会被⾃动地分配到它专属的 Pipeline 。 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">我们可以在 Pipeline 上通过 addLast() ⽅法添加⼀个或者多个 Handler ，因为⼀ </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">个数据或者事件可能会被多个 Handler 处理。当⼀个 Handler 处理完之后就将数据交给 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">下⼀个 Handler 。</span></p>

<p style="margin-left:.0001pt;text-align:left;"></p>

<h1 id="EventloopGroup%20%E4%BA%86%E8%A7%A3%E4%B9%88%3F%E5%92%8C%20EventLoop%20%E5%95%A5%E5%85%B3%E7%B3%BB%3F" style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#242a31;">EventloopGroup 了解么? 和 EventLoop 啥关系?</span></strong></h1>

<p style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#242a31;">EventloopGroup </span></strong>是<strong><span style="color:#242a31;">EventLoop </span></strong>的集合</p>

<p style="margin-left:.0001pt;text-align:left;">参见下图</p>

<p style="margin-left:.0001pt;text-align:left;">​<img alt="" height="829" src="https://img-blog.csdnimg.cn/40227a8ffbaa4ecb8c63074171469917.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_20,color_FFFFFF,t_70,g_se,x_16" width="1200" /></p>

<p></p>

<h1 id="Bootstrap%20%E5%92%8C%20ServerBootstrap%20%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F" style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#242a31;">Bootstrap 和 ServerBootstrap 是什么？</span></strong></h1>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;"><strong>Bootstrap </strong>、</span><strong><span style="color:#242a31;">ServerBootstrap </span></strong><span style="color:#3b454e;">是客户端和服务器端的启动引导类，通常ServerBootstrap 通常使⽤ bind()⽅法绑定本地的端⼝上，然后等待客户端的连接。 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">Bootstrap 只需要配置⼀个线程组 EventLoopGroup ,⽽ ServerBootstrap 需要配置两个线 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">程组EventLoopGroup ，⼀个⽤于接收连接（bossGroup），⼀个⽤于具体的处理(workerGroup)。</span></p>

<p style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#242a31;">Bootstrap和 ServerBootstrap</span></strong><span style="color:#242a31;">的使用步骤大致包括</span>设置信道，确定线程模型，添加ChannelInitializer ，绑定端口，设置一些参数等。</p>

<p style="margin-left:.0001pt;text-align:left;"></p>

<h1 id="NioEventLoopGroup%20%E9%BB%98%E8%AE%A4%E7%9A%84%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0%E4%BC%9A%E8%B5%B7%E5%A4%9A%E5%B0%91%E7%BA%BF%E7%A8%8B%EF%BC%9F" style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#242a31;">NioEventLoopGroup 默认的构造函数会起多少线程？</span></strong></h1>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">NioEventLoopGroup 默认的构造函数实际会起的线程数为 </span><strong><span style="color:#3b454e;">CPU</span></strong><strong><span style="color:#3b454e;">*2</span></strong></p>

<p style="margin-left:.0001pt;text-align:left;"></p>

<h1 id="Netty%20%E7%BA%BF%E7%A8%8B%E6%A8%A1%E5%9E%8B%E4%BA%86%E8%A7%A3%E4%B9%88%EF%BC%9F" style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#242a31;">Netty 线程模型了解么？</span></strong></h1>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">设计模式：Reactor 模式</span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">从⼀个 主线程 NIO 线程池中选择⼀个线程作为 Acceptor 线程，绑定监听端⼝，接收客户端连接 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">的连接，其他线程负责后续的接⼊认证等⼯作。连接建⽴完成后，从属 NIO 线程池负责具体处理 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">I/O 读写。</span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">BossGroup负责 accept， 连接后交由 WorkerGroup负责读写处理等。</span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">BossGroup监听客户端的连接，WorkerGroup监听读写事件。</span></p>

<p style="margin-left:.0001pt;text-align:left;"></p>

<h1 id="%E4%BB%80%E4%B9%88%E6%98%AF%20TCP%20%E7%B2%98%E5%8C%85%2F%E5%8D%8A%E5%8C%85%3F%E6%9C%89%E4%BB%80%E4%B9%88%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95%E5%91%A2%EF%BC%9F" style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#242a31;">什么是 TCP 粘包/半包?有什么解决办法呢？</span></strong></h1>

<p style="margin-left:.0001pt;text-align:left;">​粘包：好多句子  (包)   拼成了一个段落（流）</p>

<p style="margin-left:.0001pt;text-align:left;">半包：一个完整的句子（包）被拆分成了两半（流）</p>

<p style="margin-left:.0001pt;text-align:left;"><strong>主要是因为TCP当中, 只有流的概念, 没有包的概念。</strong></p>

<p style="margin-left:.0001pt;text-align:left;"></p>

<div>
<p style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#3b454e;">解决办法：使⽤ Netty ⾃带的解码器 </span></strong></p>

<p style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#3b454e;">LineBasedFrameDecoder</span></strong><span style="color:#3b454e;"> : 发送端发送数据包的时候，每个数据包之间以<strong>换⾏符</strong>作为分 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">隔， LineBasedFrameDecoder 的⼯作原理是它依次遍历 ByteBuf 中的可读字节，判断是否 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">有换⾏符，然后进⾏相应的截取。 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#3b454e;">DelimiterBasedFrameDecoder</span></strong><span style="color:#3b454e;"> : 可以<strong>⾃定义分隔符</strong>解码器， </span><strong><span style="color:#3b454e;">LineBasedFrameDecoder </span></strong><span style="color:#3b454e;">实际 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">上是⼀种特殊的 DelimiterBasedFrameDecoder 解码器。 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><strong><span style="color:#3b454e;">FixedLengthFrameDecoder </span></strong><span style="color:#3b454e;">: 固定⻓度解码器，它能够按照指定的⻓度对消息进⾏相应的拆 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#3b454e;">包。 </span></p>

<p style="margin-left:.0001pt;text-align:left;">比较常用的是<strong><span style="color:#3b454e;">LengthFieldBasedFrameDecoder </span></strong></p>
<strong><span style="color:#3b454e;">LengthFieldBasedFrameDecoder </span></strong>介绍：</div>

<div></div>

<div>
<blockquote>
<p>ch.pipeline().addLast(new LengthFieldBasedFrameDecoder(1024, 0, 1, 0, 1));</p>

<p>//自定义协议解码器<br />
/** 入参有5个，分别解释如下</p>

<p>maxFrameLength：框架的最大长度。如果帧的长度大于此值，则将抛出TooLongFrameException。</p>

<p>lengthFieldOffset：长度字段的偏移量：即对应的长度字段在整个消息数据中得位置</p>

<p>lengthFieldLength：长度字段的长度。如：长度字段是int型表示，那么这个值就是4（long型就是8）</p>

<p>lengthAdjustment：要添加到长度字段值的补偿值</p>

<p>initialBytesToStrip：从解码帧中去除的第一个字节数</p>

<p>*/</p>
</blockquote>
</div>

<div></div>

<div>
<h1 id="Netty%20%E2%BB%93%E8%BF%9E%E6%8E%A5%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F"><span style="color:#242a31;"><strong>Netty </strong></span><span style="color:#242a31;">长<strong>连接是什么，应用场景有哪些？</strong></span></h1>

<p style="margin-left:.0001pt;text-align:justify;">Netty长连接和http长连接都是基于<strong><span style="color:#3b454e;">TCP长连接 </span></strong>的。</p>

<h2 id="%E9%95%BF%E8%BF%9E%E6%8E%A5%E5%92%8C%E7%9F%AD%E8%BF%9E%E6%8E%A5%E7%9A%84%E5%8C%BA%E5%88%AB%EF%BC%9A" style="margin-left:.0001pt;text-align:justify;">长连接和短连接的区别：</h2>

<p style="margin-left:.0001pt;text-align:justify;"><br /><strong>长连接相较于短连接，节约时间和网络资源。</strong></p>

<p style="margin-left:.0001pt;text-align:justify;">TCP 在进⾏读写之前，server 与 client 之间必须提前建⽴⼀个连接。建⽴连接的过程，需要三次握⼿，释放/关闭连接的话需要四次挥⼿。这个过程是比较消耗⽹络资源并且有时间延迟的。</p>

<p style="margin-left:.0001pt;text-align:justify;">短连接说的就是 server 端 与 client 端建⽴连接之后，读写完成之后就关闭掉连接，如果</p>

<p style="margin-left:.0001pt;text-align:justify;">下⼀次再要互相发送消息，就要重新连接。短连接的优点很明显，就是管理和实现都比较简单，</p>

<p style="margin-left:.0001pt;text-align:justify;">缺点也很明显，每⼀次的读写都要建⽴连接必然会带来⼤量⽹络资源的消耗，并且连接的建⽴也</p>

<p style="margin-left:.0001pt;text-align:justify;">需要耗费时间。</p>

<p style="margin-left:.0001pt;text-align:justify;">长连接说的就是 client 向 server 双⽅建⽴连接之后，即使 client 与 server 完成⼀次读写，它们</p>

<p style="margin-left:.0001pt;text-align:justify;">之间的连接并不会主动关闭，后续的读写操作会继续使⽤这个连接。<strong>⻓</strong>连接的可以省去较多的</p>

<p style="margin-left:.0001pt;text-align:justify;">TCP 建⽴和关闭的操作，降低对⽹络资源的依赖，节约时间。对于频繁请求资源的客户来说，⾮</p>

<p style="margin-left:.0001pt;text-align:justify;">常适⽤⻓连接。</p>

<p style="margin-left:.0001pt;text-align:justify;"></p>

<div>
<p id="%E9%95%BF%E8%BF%9E%E6%8E%A5%E7%9A%84%E6%93%8D%E4%BD%9C%E6%AD%A5%E9%AA%A4%E6%98%AF%EF%BC%9A"><strong>长连接的操作步骤是</strong>：</p>

<p>建立连接-&gt;数据传输…（保持连接）…数据传输-&gt;关闭连接。</p>

<p id="%E7%9F%AD%E8%BF%9E%E6%8E%A5%E7%9A%84%E6%AD%A5%E9%AA%A4%E6%98%AF%EF%BC%9A"><a name="t2"></a><strong>短连接的步骤是：</strong></p>

<p>建立连接-&gt;数据传输-&gt;关闭连接…建立连接-&gt;数据传输-&gt;关闭连接。</p>

<p>TCP长/短连接的应用场景 转载：</p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M0H8" data-link-title="TCP长连接和短链接的区别及应用场景_haikuotiankongdong的博客-CSDN博客_tcp长连接和短连接的区别" href="https://blog.csdn.net/weixin_41563161/article/details/105529382" title="TCP长连接和短链接的区别及应用场景_haikuotiankongdong的博客-CSDN博客_tcp长连接和短连接的区别">TCP长连接和短链接的区别及应用场景_haikuotiankongdong的博客-CSDN博客_tcp长连接和短连接的区别</a></p>

<p style="margin-left:0;text-align:left;">1、长连接<strong>多用于操作频繁，点对点的通讯，而且连接数不能太多情况</strong>。每个TCP连接都需要三次握手，这需要时间，如果每个操作都是先连接，再操作的话那么处理速度会降低很多，所以每个操作完后都不断开，再次处理时直接发送数据包就OK了，不用建立TCP连接。例如：数据库的连接用长连接，如果用短连接频繁的通信会造成socket错误，而且频繁的socket 创建也是对资源的浪费。</p>

<p style="margin-left:0;text-align:left;">2、而像WEB网站的http服务一般都用短连接，因为长连接对于服务端来说会耗费一定的资源，而像WEB网站这么频繁的成千上万甚至上亿客户端的连接用短连接会更省一些资源，如果用长连接，而且同时有成千上万的用户，如果每个用户都占用一个连接的话，那可想而知吧。所以<strong>并发量大，但每个用户无需频繁操作情况下需用短连接好</strong>。</p>

<p style="margin-left:.0001pt;text-align:justify;">总结：</p>

<p style="margin-left:.0001pt;text-align:justify;">长连接<strong>多用于操作频繁，点对点的通讯，而且连接数不能太多情况</strong>。 例子： 数据库</p>

<p style="margin-left:.0001pt;text-align:justify;">短连接<strong>多用于并发量大，但每个用户无需频繁操作情况。</strong> 例子：WEB网站的http服务</p>

<p style="margin-left:.0001pt;text-align:justify;"></p>
</div>

<h1 id="Netty%E2%BC%BC%E8%B7%B3%E6%9C%BA%E5%88%B6%E4%BA%86%E8%A7%A3%E4%B9%88%EF%BC%9F"><span style="color:#242a31;"><strong>Netty心跳机制了解么？</strong></span></h1>
</div>

<div>
<p>在 TCP 保持⻓连接的过程中，可能会出现断⽹等⽹络异常出现，异常发⽣的时候， client 与</p>

<p>server 之间如果没有交互的话，它们是⽆法发现对⽅已经掉线的。为了解决这个问题, 我们就需</p>

<p>要引⼊ <strong>⼼跳机制</strong> 。</p>

<p>⼼跳机制的⼯作原理是: 在 client 与 server 之间在⼀定时间内没有数据交互时, 即处于 idle 状态</p>

<p>时, 客户端或服务器就会发送⼀个特殊的数据包给对⽅, 当接收⽅收到这个数据报⽂后, 也⽴即发</p>

<p>送⼀个特殊的数据报⽂, 回应发送⽅, 此即⼀个 PING-PONG 交互。所以, 当某⼀端收到⼼跳消息</p>

<p>后, 就知道了对⽅仍然在线, 这就确保 TCP 连接的有效性。</p>

<p></p>

<h2 id="%E7%AE%80%E5%8D%95%E7%9A%84%E5%BF%83%E8%B7%B3%E6%9C%BA%E5%88%B6%E5%AE%9E%E7%8E%B0%EF%BC%9A"><strong>简单的心跳机制实现：</strong></h2>

<p>服务器监听信道，如果<span style="color:#fe2c24;"><strong>5s</strong></span>内没有客户端发过来的包，就认为客户端挂掉了，释放该信道。</p>

<p>客户端和服务器建立连接后，在空闲时间（没有读写请求）每隔<span style="color:#fe2c24;"><strong>4s</strong></span>向服务器发送一个数据包，证明自己还“活着”。</p>
</div>

<div></div>

<div></div>

<div>
<h1 id="Netty%20%E7%9A%84%E9%9B%B6%E6%8B%B7%E8%B4%9D"><span style="color:#242a31;"><strong>Netty </strong></span><span style="color:#242a31;"><strong>的零拷贝是什么，有什么用？</strong></span></h1>

<h2 id="%E4%BC%A0%E7%BB%9F%E7%9A%84%E6%8B%B7%E8%B4%9D"><span style="color:#242a31;"><strong>传统的拷贝</strong></span></h2>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/9822c00078ac4467a437a498bcfb96f4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_14,color_FFFFFF,t_70,g_se,x_16" /></p>

<p></p>

<ul><li>
	<p>用户态与内核态的切换发生了 3 次，这个操作比较重量级</p>
	</li>
	<li>
	<p>数据拷贝了共 4 次</p>
	</li>
</ul><h2 id="%E9%9B%B6%E6%8B%B7%E8%B4%9D"><span style="color:#242a31;"><strong>零拷贝</strong></span></h2>
</div>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#fe2c24;"><strong>零拷贝指的不是不拷贝 而是减少拷贝的次数 </strong></span></p>

<p style="margin-left:.0001pt;text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/1ee4f64fae4c4af18b3d6c87a2e397a7.png" /></p>

<p></p>

<ol><li>
	<p>java 调用 transferTo 方法后，要从 java 程序的<strong>用户态</strong>切换至<strong>内核态</strong>，使用 DMA将数据读入<strong>内核缓冲区</strong>，不会使用 cpu</p>
	</li>
	<li>
	<p>只会将一些 offset 和 length 信息拷入 <strong>socket 缓冲区</strong>，几乎无消耗</p>
	</li>
	<li>
	<p>使用 DMA 将 <strong>内核缓冲区</strong>的数据写入网卡，不会使用 cpu</p>
	</li>
</ol><p>整个过程仅只发生了一次用户态与内核态的切换，数据拷贝了 <strong>2 次</strong>。所谓的【零拷贝】，<strong>并不是真正无拷贝，而是不会拷贝重复数据到 jvm 内存中。</strong></p>

<p style="margin-left:.0001pt;text-align:left;"></p>
