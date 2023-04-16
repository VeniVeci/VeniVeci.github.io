---
title: volatile 的实现原理
tags: volatile jvm
categories: 多线程 JVM
abbrlink: e1ce8e7e
date: 2022-04-04 16:15:02
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="volatile%E7%9A%84%E4%BD%9C%E7%94%A8-toc" style="margin-left:0px;"><a href="#volatile%E7%9A%84%E4%BD%9C%E7%94%A8">volatile的作用</a></p>

<p id="%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86-toc" style="margin-left:0px;"><a href="#%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86">实现原理</a></p>

<p id="%E5%A6%82%E4%BD%95%E4%BF%9D%E8%AF%81%E5%8F%AF%E8%A7%81%E6%80%A7-toc" style="margin-left:40px;"><a href="#%E5%A6%82%E4%BD%95%E4%BF%9D%E8%AF%81%E5%8F%AF%E8%A7%81%E6%80%A7">如何保证可见性</a></p>

<p id="Java%E5%8F%98%E9%87%8F%E7%9A%84%E8%AF%BB%E5%86%99-toc" style="margin-left:80px;"><a href="#Java%E5%8F%98%E9%87%8F%E7%9A%84%E8%AF%BB%E5%86%99">Java变量的读写</a></p>

<p id="volatile%E5%A6%82%E4%BD%95%E4%BF%9D%E6%8C%81%E5%86%85%E5%AD%98%E5%8F%AF%E8%A7%81%E6%80%A7-toc" style="margin-left:80px;"><a href="#volatile%E5%A6%82%E4%BD%95%E4%BF%9D%E6%8C%81%E5%86%85%E5%AD%98%E5%8F%AF%E8%A7%81%E6%80%A7">volatile如何保持内存可见性</a></p>

<p id="%E9%98%B2%E6%AD%A2%E6%8C%87%E4%BB%A4%E9%87%8D%E6%8E%92-toc" style="margin-left:40px;"><a href="#%E9%98%B2%E6%AD%A2%E6%8C%87%E4%BB%A4%E9%87%8D%E6%8E%92">防止指令重排</a></p>

<p id="%E5%8F%82%E8%80%83-toc" style="margin-left:0px;"><a href="#%E5%8F%82%E8%80%83">参考</a></p>

<hr id="hr-toc" /><p></p>

<h1 id="volatile%E7%9A%84%E4%BD%9C%E7%94%A8">volatile的作用</h1>

<div>保证变量可见性</div>

<div>防止指令重排序 ： 例子：双重校验锁的懒汉式单例模式</div>

<div></div>

<div></div>

<div></div>

<h1 id="%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86">实现原理</h1>

<h2 id="%E5%A6%82%E4%BD%95%E4%BF%9D%E8%AF%81%E5%8F%AF%E8%A7%81%E6%80%A7">如何保证可见性</h2>

<p>java通过几种原子操作完成工作内存和主内存的交互，volatile规定 load之前必须read，这样保证每次读的时候会从主存中读取。</p>

<div>
<h3 id="Java%E5%8F%98%E9%87%8F%E7%9A%84%E8%AF%BB%E5%86%99"><strong>Java变量的读写</strong></h3>

<p>Java通过几种原子操作完成<code>工作内存</code>和<code>主内存</code>的交互：</p>

<ol><li>lock：作用于主内存，把变量标识为线程独占状态。</li>
	<li>unlock：作用于主内存，解除独占状态。</li>
	<li><span style="color:#fe2c24;">read：作用主内存，把一个变量的值从主内存传输到线程的工作内存。</span></li>
	<li><span style="color:#fe2c24;">load：作用于工作内存，把read操作传过来的变量值放入工作内存的变量副本中。</span></li>
	<li><span style="color:#fe2c24;">use：作用工作内存，把工作内存当中的一个变量值传给执行引擎。</span></li>
	<li><span style="color:#956fe7;">assign：作用工作内存，把一个从执行引擎接收到的值赋值给工作内存的变量。</span></li>
	<li><span style="color:#956fe7;">store：作用于工作内存的变量，把工作内存的一个变量的值传送到主内存中。</span></li>
	<li><span style="color:#956fe7;">write：作用于主内存的变量，把store操作传来的变量的值放入主内存的变量中。</span></li>
</ol><h3 id="volatile%E5%A6%82%E4%BD%95%E4%BF%9D%E6%8C%81%E5%86%85%E5%AD%98%E5%8F%AF%E8%A7%81%E6%80%A7"><strong>volatile如何保持内存可见性</strong></h3>

<p>volatile的特殊规则就是：</p>

<ul><li>read、load、use动作必须<strong>连续出现</strong>。</li>
	<li>assign、store、write动作必须<strong>连续出现</strong>。</li>
</ul><p>所以，使用volatile变量能够保证:</p>

<ul><li>每次<code>读取前</code>必须先从主内存刷新最新的值。</li>
	<li>每次<code>写入后</code>必须立即同步回主内存当中。</li>
</ul><p><img alt="" height="362" src="https://img-blog.csdnimg.cn/1b6180ba4ee941cba56dec35927221bd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="734" /></p>

<p> </p>

<p>也就是说，<strong>volatile关键字修饰的变量看到的随时是自己的最新值</strong>。线程1中对变量v的最新修改，对线程2是可见的。</p>
</div>

<div></div>

<div>
<h2 id="%E9%98%B2%E6%AD%A2%E6%8C%87%E4%BB%A4%E9%87%8D%E6%8E%92"><strong>防止指令重排</strong></h2>
</div>

<div>编译器在生成字节码时，会在指令序列中插入<strong>内存屏障</strong>来禁止特定类型的处理器重排序。</div>

<div><span style="color:#333333;">volatile </span><span style="color:#333333;">的底层实现原理是内存屏障，</span><span style="color:#333333;">Memory Barrier</span></div>

<div></div>

<div><span style="color:#333333;">对</span><span style="color:#333333;"> volatile </span><span style="color:#333333;">变量的写指令后会加入写屏障 </span></div>

<div><span style="color:#333333;">对</span><span style="color:#333333;"> volatile </span><span style="color:#333333;">变量的读指令前会加入读屏障 </span></div>

<div></div>

<div>内存屏障之前的所有写操作都要写入内存；内存屏障之后的读操作都可以获得同步屏障之前的写操作的结果。</div>

<div><br />
 </div>

<div></div>

<h1 id="%E5%8F%82%E8%80%83">参考</h1>

<div><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="volatile关键字的作用以及原理 - 知乎" href="https://zhuanlan.zhihu.com/p/151289085" title="volatile关键字的作用以及原理 - 知乎">volatile关键字的作用以及原理 - 知乎</a></div>

<div><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="volatile关键字的作用、原理 - 猴子007 - 博客园" href="https://www.cnblogs.com/monkeysayhi/p/7654460.html" title="volatile关键字的作用、原理 - 猴子007 - 博客园">volatile关键字的作用、原理 - 猴子007 - 博客园</a></div>

<p></p>
