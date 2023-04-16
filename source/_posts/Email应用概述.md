---
title: Email应用概述
tags: 计算机网络 http smtp imap
categories: 四大件之计算机网络
abbrlink: 4e67e76a
date: 2021-03-03 16:21:01
---

<!--more-->

<p><strong>传输信息为什么不采用点对点传输，因为不能保证两台设备同时在线，所以利用邮件服务器做一个中转。</strong></p>

<p id="main-toc"><strong>目录</strong></p>

<p id="Email%E5%BA%94%E7%94%A8%E7%9A%84%E6%9E%84%E6%88%90%E7%BB%84%E4%BB%B6-toc" style="margin-left:0px;"><a href="#Email%E5%BA%94%E7%94%A8%E7%9A%84%E6%9E%84%E6%88%90%E7%BB%84%E4%BB%B6">Email应用的构成组件</a></p>

<p id="email%E7%9A%84%E7%BC%BA%E7%82%B9-toc" style="margin-left:40px;"><a href="#email%E7%9A%84%E7%BC%BA%E7%82%B9">email的缺点</a></p>

<p id="SMTP%E5%8D%8F%E8%AE%AE-toc" style="margin-left:0px;"><a href="#SMTP%E5%8D%8F%E8%AE%AE">SMTP协议</a></p>

<p id="SMTP%E4%BA%A4%E4%BA%92%E5%AE%9E%E4%BE%8B-toc" style="margin-left:40px;"><a href="#SMTP%E4%BA%A4%E4%BA%92%E5%AE%9E%E4%BE%8B">SMTP交互实例</a></p>

<p id="%E4%B8%8Ehttp%E5%8D%8F%E8%AE%AE%E7%9A%84%E5%AF%B9%E6%AF%94-toc" style="margin-left:40px;"><a href="#%E4%B8%8Ehttp%E5%8D%8F%E8%AE%AE%E7%9A%84%E5%AF%B9%E6%AF%94">与http协议的对比</a></p>

<p id="SMTP%E5%8D%8F%E8%AE%AE%E5%A6%82%E4%BD%95%E4%BC%A0%E8%BE%93%E8%A7%86%E9%A2%91%E3%80%81%E9%9F%B3%E9%A2%91%E3%80%81%E5%9B%BE%E7%89%87%E7%AD%89%E4%BF%A1%E6%81%AF%E5%91%A2%EF%BC%9F-toc" style="margin-left:40px;"><a href="#SMTP%E5%8D%8F%E8%AE%AE%E5%A6%82%E4%BD%95%E4%BC%A0%E8%BE%93%E8%A7%86%E9%A2%91%E3%80%81%E9%9F%B3%E9%A2%91%E3%80%81%E5%9B%BE%E7%89%87%E7%AD%89%E4%BF%A1%E6%81%AF%E5%91%A2%EF%BC%9F">SMTP协议如何传输视频、音频、图片等信息呢？</a></p>

<p id="%E9%82%AE%E4%BB%B6%E8%AE%BF%E9%97%AE%E5%8D%8F%E8%AE%AE-toc" style="margin-left:0px;"><a href="#%E9%82%AE%E4%BB%B6%E8%AE%BF%E9%97%AE%E5%8D%8F%E8%AE%AE">邮件访问协议</a></p>

<p id="POP3%E5%92%8CIMAP%E7%9A%84%E5%8C%BA%E5%88%AB%EF%BC%9A-toc" style="margin-left:40px;"><a href="#POP3%E5%92%8CIMAP%E7%9A%84%E5%8C%BA%E5%88%AB%EF%BC%9A">POP3和IMAP的区别：</a></p>

<hr id="hr-toc" /><h1 id="Email%E5%BA%94%E7%94%A8%E7%9A%84%E6%9E%84%E6%88%90%E7%BB%84%E4%BB%B6">Email应用的构成组件</h1>

<p><img alt="" height="436" src="https://img-blog.csdnimg.cn/20210303153931283.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="537" /></p>

<p><img alt="" height="515" src="https://img-blog.csdnimg.cn/20210303153946487.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1008" /></p>

<h2 id="email%E7%9A%84%E7%BC%BA%E7%82%B9">email的缺点</h2>

<p>传输信息时间延迟较长，交互体验不好。</p>

<p>在传递一些情绪反应、需要同情理解或社会支持的信息时,电子邮件就不能传递出这些情感。</p>

<h1 id="SMTP%E5%8D%8F%E8%AE%AE">SMTP协议</h1>

<p>传输层采用TCP协议</p>

<p>端口25</p>

<p>传输过程的三个阶段：握手 消息传输  关闭</p>

<p>采用的模式：命令响应交互模式</p>

<p>属于异步应用</p>

<p> </p>

<h2 id="SMTP%E4%BA%A4%E4%BA%92%E5%AE%9E%E4%BE%8B">SMTP交互实例</h2>

<p><img alt="" height="433" src="https://img-blog.csdnimg.cn/20210303154823205.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="738" /></p>

<h2 id="%E4%B8%8Ehttp%E5%8D%8F%E8%AE%AE%E7%9A%84%E5%AF%B9%E6%AF%94">与http协议的对比</h2>

<p><img alt="" height="539" src="https://img-blog.csdnimg.cn/20210303154805954.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1005" /></p>

<p> 
</p><h2 id="SMTP%E5%8D%8F%E8%AE%AE%E5%A6%82%E4%BD%95%E4%BC%A0%E8%BE%93%E8%A7%86%E9%A2%91%E3%80%81%E9%9F%B3%E9%A2%91%E3%80%81%E5%9B%BE%E7%89%87%E7%AD%89%E4%BF%A1%E6%81%AF%E5%91%A2%EF%BC%9F">SMTP协议如何传输视频、音频、图片等信息呢？</h2>


<p><span style="color:#f33b45;"><strong>进行多媒体扩展</strong></span></p>

<p><span><strong>文本消息格式</strong></span></p>

<p><img alt="" height="433" src="https://img-blog.csdnimg.cn/20210303155737788.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="986" /></p>

<p><strong>多媒体扩展</strong></p>

<p><img alt="" height="540" src="https://img-blog.csdnimg.cn/20210303155850920.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="867" /></p>

<h1 id="%E9%82%AE%E4%BB%B6%E8%AE%BF%E9%97%AE%E5%8D%8F%E8%AE%AE">邮件访问协议</h1>

<p><img alt="" height="549" src="https://img-blog.csdnimg.cn/20210303160117914.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="878" /></p>

<p>我们通常用的QQ邮箱就是用的http协议，因为一般是在浏览器中使用的，不是客户端。</p>

<p>POP3 是无状态协议 IMAP是有状态的协议</p>

<h2 id="POP3%E5%92%8CIMAP%E7%9A%84%E5%8C%BA%E5%88%AB%EF%BC%9A">POP3和IMAP的区别：</h2>

<p><strong><a href="http://help.163.com/09/1223/14/5R7P6CJ600753VB8.html?servCode=6010376">POP3</a></strong>协议允许电子邮件客户端下载服务器上的邮件，但是在客户端的操作（如移动邮件、标记已读等），不会反馈到服务器上，比如通过客户端收取了邮箱中的3封邮件并移动到其他文件夹，邮箱服务器上的这些邮件是没有同时被移动的 。</p>

<p>而<strong><a href="http://help.163.com/09/1223/14/5R7P6CJ600753VB8.html?servCode=6010376">IMAP</a></strong>提供webmail 与电子邮件客户端之间的双向通信，客户端的操作都会反馈到服务器上，对邮件进行的操作，服务器上的邮件也会做相应的动作。</p>

<p><img alt="" height="263" src="https://img-blog.csdnimg.cn/2021030316085863.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="519" /></p>

<p>POP3的邮箱模式是独立于服务器的，也就是当你使用了POP3设置，一旦下载到了你的系统，服务器会自动删除已经下载的邮件。<br />
IMAP的邮箱模式是和服务器同步的。也就是当你使用了IMAP设置，尽管你已经下载到了你的系统，服务器是不会自动删除的。只有当你在你的系统上删除了，服务器才会自动删除。也就是服务器上面的邮件和你本地的邮件是一模一样的。<strong>所以一般选择IMAP协议。</strong></p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>
