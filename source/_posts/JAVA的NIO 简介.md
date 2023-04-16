---
title: JAVA的NIO 简介
tags: 网络
categories: Netty 多线程
abbrlink: f3815d8e
date: 2022-03-28 19:49:32
---

<!--more-->

<p id="%E5%8F%82%E8%80%83%EF%BC%9A%20Netty%20%E9%BB%91%E9%A9%AC">参考： Netty 黑马</p>

<p id="main-toc"><strong>目录</strong></p>

<p id="%E9%98%BB%E5%A1%9E-toc" style="margin-left:0px;"><a href="#%E9%98%BB%E5%A1%9E">阻塞</a></p>

<p id="%E9%9D%9E%E9%98%BB%E5%A1%9E-toc" style="margin-left:0px;"><a href="#%E9%9D%9E%E9%98%BB%E5%A1%9E">非阻塞</a></p>

<p id="%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8-toc" style="margin-left:0px;"><a href="#%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8">多路复用</a></p>

<hr id="hr-toc" /><p></p>

<h1 id="%E9%98%BB%E5%A1%9E">阻塞</h1>

<ul><li>
	<p>阻塞模式下，相关方法都会导致线程暂停</p>

	<ul><li>
		<p>ServerSocketChannel.accept 会在没有连接建立时让线程暂停</p>
		</li>
		<li>
		<p>SocketChannel.read 会在没有数据可读时让线程暂停</p>
		</li>
		<li>
		<p>阻塞的表现其实就是线程暂停了，暂停期间不会占用 cpu，但线程相当于闲置</p>
		</li>
	</ul></li>
	<li>
	<p>单线程下，<span style="color:#fe2c24;">阻塞方法之间相互影响</span>，<strong>几乎不能正常工作，需要多线程支持</strong></p>
	</li>
	<li>
	<p>但多线程下，有新的问题，体现在以下方面</p>

	<ul><li>
		<p>32 位 jvm 一个线程 320k，64 位 jvm 一个线程 1024k，如果连接数过多，必然导致 OOM，并且线程太多，反而会因为频繁上下文切换导致性能降低</p>
		</li>
		<li>
		<p>可以采用线程池技术来减少线程数和线程上下文切换，但治标不治本，如果有很多连接建立，但长时间 inactive，会阻塞线程池中所有线程，因此不适合长连接，只适合短连接</p>
		</li>
	</ul></li>
</ul><p><strong>所以阻塞模式的IO比较适合</strong><strong>短连接，一次连接很快就可以完成读写，立马关闭，进行下一个连接和读写。</strong></p>

<p></p>

<p></p>

<h1 id="%E9%9D%9E%E9%98%BB%E5%A1%9E">非阻塞</h1>

<ul><li>
	<p>非阻塞模式下，相关方法都会不会让线程暂停</p>

	<ul><li>
		<p>在 ServerSocketChannel.accept 在没有连接建立时，<strong>会返回 null，继续运行</strong></p>
		</li>
		<li>
		<p>SocketChannel.read 在没有数据可读时，会返回 0，<strong>但线程不必阻塞，可以去执行其它</strong> SocketChannel 的 read 或是去执行 ServerSocketChannel.accept</p>
		</li>
		<li>
		<p>写数据时，线程只是等待数据写入 Channel 即可，无需等 Channel 通过网络把数据发送出去</p>
		</li>
	</ul></li>
	<li>
	<p>但非阻塞模式下，即使没有连接建立，<strong>和可读数据，<span style="color:#fe2c24;">线程仍然在不断运行，白白浪费了 cpu</span></strong></p>
	</li>
	<li>
	<p>数据复制过程中，线程实际还是阻塞的（AIO 改进的地方）</p>
	</li>
</ul><p></p>

<h1 id="%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8">多路复用</h1>

<p><img alt="" height="472" src="https://img-blog.csdnimg.cn/3df8b511029f4e1f8759c42fdfb9fd87.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_19,color_FFFFFF,t_70,g_se,x_16" width="630" /></p>

<p> </p>

<p><span style="color:#fe2c24;">单线程可以配合 Selector 完成对多个 Channel 可读写事件的监控，这称之为多路复用</span></p>

<ul><li>
	<p>多路复用仅针对网络 IO、普通文件 IO 没法利用多路复用</p>
	</li>
	<li>
	<p><strong>如果不用 Selector 的非阻塞模式，线程大部分时间都在做无用功，而 Selector 能够保证</strong></p>

	<ul><li>
		<p><strong>有可连接事件时才去连接</strong></p>
		</li>
		<li>
		<p><strong>有可读事件才去读取</strong></p>
		</li>
		<li>
		<p><strong>有可写事件才去写入</strong></p>

		<ul><li>
			<p>限于网络传输能力，Channel 未必时时可写，一旦 Channel 可写，<strong>会触发 Selector 的可写事件</strong></p>
			</li>
		</ul></li>
	</ul></li>
</ul><ul><li>
	<p><strong>channel 必须工作在非阻塞模式</strong></p>
	</li>
	<li>
	<p>FileChannel 没有非阻塞模式，因此不能配合 selector 一起使用</p>
	</li>
	<li>
	<p>绑定的事件类型可以有</p>

	<ul><li>
		<p>connect - 客户端连接成功时触发</p>
		</li>
		<li>
		<p>accept - 服务器端成功接受连接时触发</p>
		</li>
		<li>
		<p>read - 数据可读入时触发，有因为接收能力弱，数据暂不能读入的情况</p>
		</li>
		<li>
		<p>write - 数据可写出时触发，有因为发送能力弱，数据暂不能写出的情况</p>
		</li>
	</ul></li>
</ul><p></p>

<p></p>
