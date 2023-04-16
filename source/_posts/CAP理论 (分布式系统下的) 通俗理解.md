---
title: CAP理论 (分布式系统下的) 通俗理解
tags: 分布式 数据库 java
categories: 分布式系统设计
abbrlink: 47f9e04c
date: 2022-03-08 17:25:14
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="%E4%B8%80%E8%87%B4%E6%80%A7%EF%BC%88C%EF%BC%89-toc" style="margin-left:40px;"><a href="#%E4%B8%80%E8%87%B4%E6%80%A7%EF%BC%88C%EF%BC%89">一致性（C）</a></p>

<p id="%E5%8F%AF%E7%94%A8%E6%80%A7%EF%BC%88A%EF%BC%89-toc" style="margin-left:40px;"><a href="#%E5%8F%AF%E7%94%A8%E6%80%A7%EF%BC%88A%EF%BC%89">可用性（A）</a></p>

<p id="%E5%88%86%E5%8C%BA%E5%AE%B9%E9%94%99%E6%80%A7%EF%BC%88P%EF%BC%89-toc" style="margin-left:40px;"><a href="#%E5%88%86%E5%8C%BA%E5%AE%B9%E9%94%99%E6%80%A7%EF%BC%88P%EF%BC%89">分区容错性（P）</a></p>

<p id="CAP%E5%AE%9A%E7%90%86%EF%BC%88%E6%A0%B8%E5%BF%83%EF%BC%89-toc" style="margin-left:0px;"><a href="#CAP%E5%AE%9A%E7%90%86%EF%BC%88%E6%A0%B8%E5%BF%83%EF%BC%89">CAP定理（核心）</a></p>

<p id="%E7%9B%B8%E4%BA%92%E5%85%B3%E7%B3%BB-toc" style="margin-left:40px;"><a href="#%E7%9B%B8%E4%BA%92%E5%85%B3%E7%B3%BB">相互关系</a></p>

<hr id="hr-toc" /><p></p>

<p>CAP 理论是针对分布式数据库而言的，它是指在一个分布式系统中，一致性（Consistency, C）、可用性（Availability, A）、分区容错性（Partition Tolerance, P）三者不可兼得。</p>

<h1 id="%E4%B8%80%E8%87%B4%E6%80%A7%EF%BC%88C%EF%BC%89">一致性（C）</h1>

<p>一致性是指“all nodes see the same data at the same time”，即更新操作成功后，所有节点在同一时间的数据完全一致。<br /><br />
一致性可以分为客户端和服务端两个不同的视角：</p>

<ul><li>从客户端角度来看，一致性主要指多个用户并发访问时更新的数据如何被其他用户获取的问题；</li>
	<li>从服务端来看，一致性则是用户进行数据更新时如何将数据复制到整个系统，以保证数据的一致。</li>
</ul><p><br />
一致性是在并发读写时才会出现的问题，因此在理解一致性的问题时，一定要注意结合考虑并发读写的场景。</p>

<p></p>

<h1 id="%E5%8F%AF%E7%94%A8%E6%80%A7%EF%BC%88A%EF%BC%89">可用性（A）</h1>

<p>可用性是指“reads and writes always succeed”，即用户访问数据时，系统是否能在正常响应时间返回结果。<br /><br />
好的可用性主要是指系统能够很好地为用户服务，不出现用户操作失败或者访问超时等用户体验不好的情况。在通常情况下，<strong>可用性与分布式数据冗余、负载均衡等有着很大的关联</strong>。</p>

<p></p>

<h1 id="%E5%88%86%E5%8C%BA%E5%AE%B9%E9%94%99%E6%80%A7%EF%BC%88P%EF%BC%89">分区容错性（P）</h1>

<p>分区容忍性(Partition Tolerance，简称P)只有两个可选参数：不容忍和容忍。</p>

<p></p>

<h1 id="CAP%E5%AE%9A%E7%90%86%EF%BC%88%E6%A0%B8%E5%BF%83%EF%BC%89"><span style="color:#fe2c24;">CAP定理（核心）</span></h1>

<p><strong>C、A、P三者最多得二，因为是分布式系统，所以P必须满足，所以CA不可能同时满足。</strong></p>

<p>比如两台机器A，B存放着一样的数据，满足P（分区容忍）。</p>

<p>如果要满足一致性，假如多个请求同时访问一个数据，其中一个请求修改了A中的数据data，此时有请求访问机器B，那么必然要通过信息传输将A机器中的data同步到机器B，这样就不是完全的可用性（访问B的时延为0）。</p>

<p>如果满足时延为0（可用性），那么返回的data一定不是最新的data。</p>

<p>所以一般会根据不同的场景做折中。比如要求你在一定时延内返回正确的数据。</p>

<p></p>

<h1 id="%E7%9B%B8%E4%BA%92%E5%85%B3%E7%B3%BB">相互关系</h1>

<p>CAP 理论认为分布式系统只能兼顾其中的两个特性，即出现 CA、CP、AP 三种情况，如图所示。<br />
 </p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/dff6273be1df40e579d4aebac29e431e.png" /></p>

<p></p>

<p>CA without P</p>

<p>如果不要求 Partition Tolerance，即不允许分区，则强一致性和可用性是可以保证的。其实分区是始终存在的问题，因此 CA 的分布式系统更多的是允许分区后各子系统依然保持 CA。</p>

<p>CP without A</p>

<p>如果不要求可用性，相当于每个请求都需要在各服务器之间强一致，而分区容错性会导致同步时间无限延长，如此 CP 也是可以保证的。<strong>很多传统的数据库分布式事务都属于这种模式。</strong></p>

<p>AP without C</p>

<p>如果要可用性高并允许分区，则需放弃一致性。一旦分区发生，节点之间可能会失去联系，为了实现高可用，每个节点只能用本地数据提供服务，<strong>而这样会导致全局数据的不一致性。</strong></p>

<p> </p>

<p>参考博文</p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="cap理论具体含义_什么是CAP定理？_知乎职场的博客-CSDN博客" href="https://blog.csdn.net/weixin_27565831/article/details/112844587" title="cap理论具体含义_什么是CAP定理？_知乎职场的博客-CSDN博客">cap理论具体含义_什么是CAP定理？_知乎职场的博客-CSDN博客</a></p>
