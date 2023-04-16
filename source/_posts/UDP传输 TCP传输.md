---
title: UDP传输 TCP传输
tags: 网络 网络安全
categories: 四大件之计算机网络
abbrlink: '1519e094'
date: 2022-03-27 11:07:39
---

<!--more-->

<h1>UDP TCP对比</h1>

<p><img alt="" height="398" src="https://img-blog.csdnimg.cn/56105588f7cb4667af3ba3d3969883e0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1062" /></p>

<p></p>

<h1>有TCP为什么还要有UDP？</h1>

<p>UDP有时比TCP更有优势。UDP以其简单、传输快的优势，在越来越多场景下取代了TCP，如实时游戏。</p>

<p>（1）网速的提升给UDP的稳定性提供可靠网络保障，丢包率很低，如果使用应用层重传，能够确保传输的可靠性。</p>

<p>（2）TCP为了实现网络通信的可靠性，使用了复杂的拥塞控制算法，建立了繁琐的握手过程，由于TCP内置于系统协议栈中，极难对其进行改进。</p>

<p>采用TCP，一旦发生丢包，TCP会将后续的包缓存起来，等前面的包重传并接收到后再继续发送，延时会越来越大，基于UDP对实时性要求较为严格的情况下，采用自定义重传机制，能够把丢包产生的延迟降到最低，<strong>尽量减少网络问题对游戏性造成影响。</strong><br />
 <br />
比如现在的一些开会软件应该大都采用的是RUDP这样一种应用层协议，reliable UDP.</p>

<p></p>

<h1>UDP单播 广播 组播</h1>

<p>单播就是点对点、多播是给一组设备发、广播就是在自己所在的网段发送信息（比如局域网游戏）。</p>

<p>现在的<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="路由器" href="https://so.csdn.net/so/search?q=%E8%B7%AF%E7%94%B1%E5%99%A8&amp;spm=1001.2101.3001.7020" title="路由器">路由器</a>都有个拒绝发送广播的策略，广播一般来说就是在你的路由器内部进行广播，</p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="UDP 单播、广播和多播 - DoubleLi - 博客园" href="https://www.cnblogs.com/lidabo/p/5865045.html" title="UDP 单播、广播和多播 - DoubleLi - 博客园">UDP 单播、广播和多播 - DoubleLi - 博客园</a></p>

<h2>组播（多播）</h2>

<p>　　多播，也称为“组播”，将网络中同一业务类型主机进行了逻辑上的分组，进行数据收发的时候其数据仅仅在同一分组中进行，<strong>其他的主机没有加入此分组不能收发对应的数据。</strong></p>

<p>　　在广域网上广播的时候，其中的交换机和路由器只向需要获取数据的主机复制并转发数据。主机可以向路由器请求加入或退出某个组，网络中的路由器和交换机有选择地复制并传输数据，将数据仅仅传输给组内的主机。</p>

<p>      <span style="color:#fe2c24;">  多播的这种功能，可以一次将数据发送到多个主机，又能保证不影响其他不需要（未加入组）的主机的其他通 信。</span></p>

<p><strong>相对于传统的一对一的单播，多播具有如下的优点</strong>：</p>

<p>　　1、具有同种业务的主机加入同一数据流，共享同一通道，节省了带宽和服务器的优点，具有广播的优点而又没有广播所需要的带宽。</p>

<p>　　2、服务器的总带宽不受客户端带宽的限制。由于组播协议由接收者的需求来确定是否进行数据流的转发，所以服务器端的带宽是常量，与客户端的数量无关。</p>

<p>　　3、与单播一样，多播是允许在广域网即Internet上进行传输的，而广播仅仅在同一局域网上才能进行。</p>

<p><strong>组播的缺点：</strong></p>

<p>　　1、多播与单播相比没有纠错机制，当发生错误的时候难以弥补，但是可以在应用层来实现此种功能。</p>

<p>　　2、多播的网络支持存在缺陷，需要路由器及网络协议栈的支持。</p>

<p>　　3、多播的应用主要有网上视频、网上会议等。</p>
