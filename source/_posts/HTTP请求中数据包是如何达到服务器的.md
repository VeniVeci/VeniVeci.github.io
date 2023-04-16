---
title: HTTP请求中数据包是如何达到服务器的
tags: 网络协议
categories: 四大件之计算机网络
abbrlink: 82d84225
date: 2022-03-27 10:40:01
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="%E4%BA%94%E5%B1%82%E6%A8%A1%E5%9E%8B-toc" style="margin-left:0px;"><a href="#%E4%BA%94%E5%B1%82%E6%A8%A1%E5%9E%8B">五层模型</a></p>

<p id="%E5%BA%94%E7%94%A8%E5%B1%82-toc" style="margin-left:40px;"><a href="#%E5%BA%94%E7%94%A8%E5%B1%82">应用层</a></p>

<p id="%E4%BC%A0%E8%BE%93%E5%B1%82-toc" style="margin-left:40px;"><a href="#%E4%BC%A0%E8%BE%93%E5%B1%82">传输层</a></p>

<p id="%E7%BD%91%E7%BB%9C%E5%B1%82-toc" style="margin-left:40px;"><a href="#%E7%BD%91%E7%BB%9C%E5%B1%82">网络层</a></p>

<p id="OSPF%E5%8D%8F%E8%AE%AE-toc" style="margin-left:80px;"><a href="#OSPF%E5%8D%8F%E8%AE%AE">OSPF协议</a></p>

<p id="%E6%95%B0%E6%8D%AE%E9%93%BE%E8%B7%AF%E5%B1%82-toc" style="margin-left:40px;"><a href="#%E6%95%B0%E6%8D%AE%E9%93%BE%E8%B7%AF%E5%B1%82">数据链路层</a></p>

<p id="%E8%B7%AF%E7%94%B1%E5%99%A8%E3%80%81%E4%BA%A4%E6%8D%A2%E6%9C%BA%E3%80%81%E9%9B%86%E7%BA%BF%E5%99%A8%E7%9A%84%E5%8C%BA%E5%88%AB-toc" style="margin-left:40px;"><a href="#%E8%B7%AF%E7%94%B1%E5%99%A8%E3%80%81%E4%BA%A4%E6%8D%A2%E6%9C%BA%E3%80%81%E9%9B%86%E7%BA%BF%E5%99%A8%E7%9A%84%E5%8C%BA%E5%88%AB">路由器、交换机、集线器的区别</a></p>

<p id="%E6%95%B0%E6%8D%AE%E5%8C%85%E5%9C%A8%E4%BA%94%E5%B1%82%E6%A8%A1%E5%9E%8B%E4%B8%AD%E7%9A%84%E4%BC%A0%E8%BE%93-toc" style="margin-left:0px;"><a href="#%E6%95%B0%E6%8D%AE%E5%8C%85%E5%9C%A8%E4%BA%94%E5%B1%82%E6%A8%A1%E5%9E%8B%E4%B8%AD%E7%9A%84%E4%BC%A0%E8%BE%93">数据包在五层模型中的传输</a></p>

<p id="%E4%B8%80%E5%8F%B0%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%9A%84%E7%BD%91%E7%BB%9C%E9%85%8D%E7%BD%AE-toc" style="margin-left:40px;"><a href="#%E4%B8%80%E5%8F%B0%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%9A%84%E7%BD%91%E7%BB%9C%E9%85%8D%E7%BD%AE">一台计算机的网络配置</a></p>

<hr id="hr-toc" /><p> </p>

<p>五层模型每一层的协议和设备， 数据包是如何达到服务器的。</p>

<p> 参考<br /><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="TCP/IP五层协议——数据包是如何到达服务器的_chengzeL的博客-CSDN博客" href="https://blog.csdn.net/asd13518662/article/details/115748435" title="TCP/IP五层协议——数据包是如何到达服务器的_chengzeL的博客-CSDN博客">TCP/IP五层协议——数据包是如何到达服务器的_chengzeL的博客-CSDN博客</a></p>

<p> <a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="客户端请求是如何到达服务器的_无忧杂货铺的博客-CSDN博客_客户端请求服务器的过程" href="https://blog.csdn.net/qq_41816540/article/details/99824657" title="客户端请求是如何到达服务器的_无忧杂货铺的博客-CSDN博客_客户端请求服务器的过程">客户端请求是如何到达服务器的_无忧杂货铺的博客-CSDN博客_客户端请求服务器的过程</a></p>

<h1 id="%E4%BA%94%E5%B1%82%E6%A8%A1%E5%9E%8B">五层模型</h1>

<h2 id="%E5%BA%94%E7%94%A8%E5%B1%82">应用层</h2>

<p>负责进程间的通信，主要的协议有域名系统 DNS，映射</p>

<p>HTTP协议，SMTP协议，交互的数据单元是报文。</p>

<h2 id="%E4%BC%A0%E8%BE%93%E5%B1%82">传输层</h2>

<p>可以分用和复用，应用层都可以使用传输层进行通信，传输数据，这叫复用。</p>

<p>在向应用层传输数据时，分发给不同的应用，这叫做分用。</p>

<p>主要的协议是 TCP UDP</p>

<p>TCP transfer control protocol 传输控制协议，提供可靠的数据连接，</p>

<p>UDP user datagram protocol 用户数据报协议 无连接的 不保证数据传输的可靠性</p>

<h2 id="%E7%BD%91%E7%BB%9C%E5%B1%82">网络层</h2>

<p>因为有不同的通信子网存在，所以选择合适的网间路由 交换节点 怎么把数据通过路由传递到应用层</p>

<p>数据单元是 数据报文分成的分组和包</p>

<p>具体功能包括<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="寻址" href="https://baike.baidu.com/item/%E5%AF%BB%E5%9D%80" title="寻址">寻址</a>和<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="路由选择" href="https://baike.baidu.com/item/%E8%B7%AF%E7%94%B1%E9%80%89%E6%8B%A9" title="路由选择">路由选择</a>、连接的建立、保持和终止等。它提供的服务使<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="传输层" href="https://baike.baidu.com/item/%E4%BC%A0%E8%BE%93%E5%B1%82" title="传输层">传输层</a>不需要了解网络中的数据传输和<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="交换技术" href="https://baike.baidu.com/item/%E4%BA%A4%E6%8D%A2%E6%8A%80%E6%9C%AF" title="交换技术">交换技术</a>。</p>

<h3 id="OSPF%E5%8D%8F%E8%AE%AE">OSPF协议</h3>

<p><img alt="" height="668" src="https://img-blog.csdnimg.cn/32d289c8a2464dcebbcd18827b2d4762.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="952" /></p>

<p> </p>

<h2 id="%E6%95%B0%E6%8D%AE%E9%93%BE%E8%B7%AF%E5%B1%82">数据链路层</h2>

<p>数据链路层的主要作用是负责解决两个直接相邻节点之间的通信，但并不负责解决数据经过通信子网中多个转接节点时的通信问题</p>

<p></p>

<h2 id="%E8%B7%AF%E7%94%B1%E5%99%A8%E3%80%81%E4%BA%A4%E6%8D%A2%E6%9C%BA%E3%80%81%E9%9B%86%E7%BA%BF%E5%99%A8%E7%9A%84%E5%8C%BA%E5%88%AB">路由器、交换机、集线器的区别</h2>

<p>集线器是<span style="color:#fe2c24;">物理层</span>设备。</p>

<p>交换机是一种基于MAC地址识别，能完成封装转发数据包功能的网络设备。交换机可以“学习”MAC地址，并把其存放在内部地址表中 ，属于<span style="color:#fe2c24;">数据链路层</span>设备。</p>

<p>路由器是<span style="color:#fe2c24;">网络层</span>设备，基于IP地址寻址。</p>

<p></p>

<p>从物理上划分网段的交换机不同，路由器使用专门的软件协议从<strong>逻辑</strong>上对整个网络进行划分。</p>

<p>路由器是产生于交换机之后，就像交换机产生于集线器之后。</p>

<p>传统的<strong>交换机只能分割冲突域，不能分割广播域</strong>；而路由器可以分割广播域 ，由交换机连接的网段仍属于同一个广播域，广播数据包会在交换机连接的所有网段上传播，在某些情况下会导致通信拥挤和安全漏洞。连接到路由器上的网段会被分配成不同的广播域，广播数据不会穿过路由器。虽然第三层以上交换机具有VLAN功能，也可以分割广播域，但是各子广播域之间是不能通信交流的，它们之间的交流仍然需要路由器，同时路由器<strong>提供了防火墙的服务 </strong>，路由器仅仅转发特定地址的数据包，不传送不支持路由协议的数据包传送和未知目标网络数据包的传送，从而可以防止广播风暴。</p>

<p></p>

<h1 id="%E6%95%B0%E6%8D%AE%E5%8C%85%E5%9C%A8%E4%BA%94%E5%B1%82%E6%A8%A1%E5%9E%8B%E4%B8%AD%E7%9A%84%E4%BC%A0%E8%BE%93">数据包在五层模型中的传输</h1>

<p><img alt="" height="461" src="https://img-blog.csdnimg.cn/bcea2d47bb4e4f6e958dc17f929a8a1d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="902" /></p>

<p></p>

<p>每一层负责自己的工作，共同合作完成了一次网络传输。</p>

<p>从上面的图中就能看出，当客户端的应用程序要发起一次信息传输，会由上到下被每一层进行一次信息加工。而这个数据包到达服务端后，再由下到上每一层对信息进行反加工，从而能完整的还原客户端发送的信息。</p>

<p>从传输控制层，通过三次握手和四次挥手建立连接。</p>

<p>从网络层，通过<strong>IP协议找到要连接的目标主机</strong>。</p>

<p>从链路层，分析<strong>下一跳的是如何定位下一台机器的</strong>，从而定位到目标IP的主机的物理位置。</p>

<p></p>

<h2 id="%E4%B8%80%E5%8F%B0%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%9A%84%E7%BD%91%E7%BB%9C%E9%85%8D%E7%BD%AE">一台计算机的网络配置</h2>

<p>一台计算机的网络配置要关注四个维度：</p>

<p>IP地址：就是当前计算机的IP地址，在上图中，本机的IP地址是172.16.40.128。</p>

<p>掩码：主要是用来决定目标IP应该从哪个路由走。</p>

<p>网关：网关就是另一个网络的关口，实质上就是一个网络通向另一个网络的IP地址。比如在上图中，本机是属于172.16.40.0这个局域网，但这个局域网的网关地址是172.16.40.2。</p>

<p></p>

<p><br />
 </p>

<p></p>

<p></p>

<p></p>
