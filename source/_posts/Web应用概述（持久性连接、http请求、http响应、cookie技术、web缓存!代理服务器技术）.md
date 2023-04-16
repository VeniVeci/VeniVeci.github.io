---
title: Web应用概述（持久性连接、http请求、http响应、cookie技术、web缓存/代理服务器技术）
tags: 网络 java
categories: 四大件之计算机网络
abbrlink: 5b7d6721
date: 2021-03-02 16:06:19
---

<!--more-->

<p><strong>web应用就是利用http协议进行互联互通的应用，常用的浏览器都属于web应用。</strong></p>

<p id="main-toc"><strong>目录</strong></p>

<p id="%E9%81%B5%E5%BE%AA%E7%9A%84%E5%8D%8F%E8%AE%AE-toc" style="margin-left:0px;"><a href="#%E9%81%B5%E5%BE%AA%E7%9A%84%E5%8D%8F%E8%AE%AE">遵循的协议</a></p>

<p id="HTTP%E8%BF%9E%E6%8E%A5%E7%9A%84%E4%B8%A4%E7%A7%8D%E7%B1%BB%E5%9E%8B-toc" style="margin-left:0px;"><a href="#HTTP%E8%BF%9E%E6%8E%A5%E7%9A%84%E4%B8%A4%E7%A7%8D%E7%B1%BB%E5%9E%8B">HTTP连接的两种类型</a></p>

<p id="%E9%9D%9E%E6%8C%81%E4%B9%85%E6%80%A7%E8%BF%9E%E6%8E%A5-toc" style="margin-left:40px;"><a href="#%E9%9D%9E%E6%8C%81%E4%B9%85%E6%80%A7%E8%BF%9E%E6%8E%A5">非持久性连接</a></p>

<p id="%E6%8C%81%E4%B9%85%E6%80%A7%E8%BF%9E%E6%8E%A5-toc" style="margin-left:40px;"><a href="#%E6%8C%81%E4%B9%85%E6%80%A7%E8%BF%9E%E6%8E%A5">持久性连接</a></p>

<p id="%E9%87%87%E7%94%A8%E5%B8%A6%E6%9C%89%E6%B5%81%E6%B0%B4%E6%9C%BA%E5%88%B6%E7%9A%84%E6%8C%81%E4%B9%85%E6%80%A7%E8%BF%9E%E6%8E%A5%E4%BD%BF%E5%BE%97http%E9%80%9A%E4%BF%A1%E6%97%B6%E6%95%88%E7%8E%87%E6%9B%B4%E9%AB%98%E3%80%82-toc" style="margin-left:40px;"><a href="#%E9%87%87%E7%94%A8%E5%B8%A6%E6%9C%89%E6%B5%81%E6%B0%B4%E6%9C%BA%E5%88%B6%E7%9A%84%E6%8C%81%E4%B9%85%E6%80%A7%E8%BF%9E%E6%8E%A5%E4%BD%BF%E5%BE%97http%E9%80%9A%E4%BF%A1%E6%97%B6%E6%95%88%E7%8E%87%E6%9B%B4%E9%AB%98%E3%80%82">采用带有流水机制的持久性连接使得http通信时效率更高。</a></p>

<p id="%E5%85%B7%E4%BD%93%E7%9A%84http%E4%BF%A1%E6%81%AF-toc" style="margin-left:0px;"><a href="#%E5%85%B7%E4%BD%93%E7%9A%84http%E4%BF%A1%E6%81%AF">具体的http信息</a></p>

<p id="http%E8%AF%B7%E6%B1%82-toc" style="margin-left:40px;"><a href="#http%E8%AF%B7%E6%B1%82">http请求信息</a></p>

<p id="http%E8%AF%B7%E6%B1%82%E6%B6%88%E6%81%AF%E7%9A%84%E9%80%9A%E7%94%A8%E6%A0%BC%E5%BC%8F-toc" style="margin-left:80px;"><a href="#http%E8%AF%B7%E6%B1%82%E6%B6%88%E6%81%AF%E7%9A%84%E9%80%9A%E7%94%A8%E6%A0%BC%E5%BC%8F">http请求消息的通用格式</a></p>

<p id="http%E5%93%8D%E5%BA%94%E6%B6%88%E6%81%AF-toc" style="margin-left:40px;"><a href="#http%E5%93%8D%E5%BA%94%E6%B6%88%E6%81%AF">http响应消息</a></p>

<p id="Cookie%E6%8A%80%E6%9C%AF-toc" style="margin-left:0px;"><a href="#Cookie%E6%8A%80%E6%9C%AF">Cookie技术</a></p>

<p id="cookie%E5%8E%9F%E7%90%86-toc" style="margin-left:40px;"><a href="#cookie%E5%8E%9F%E7%90%86">cookie原理</a></p>

<p id="cookie%E7%9A%84%E9%97%AE%E9%A2%98%EF%BC%9A%E9%9A%90%E7%A7%81%E9%97%AE%E9%A2%98-toc" style="margin-left:40px;"><a href="#cookie%E7%9A%84%E9%97%AE%E9%A2%98%EF%BC%9A%E9%9A%90%E7%A7%81%E9%97%AE%E9%A2%98">cookie的问题：隐私问题</a></p>

<p id="cookie%E6%97%B6%E4%BB%A3%E5%8D%B3%E5%B0%86%E7%BB%88%E7%BB%93%EF%BC%9F-toc" style="margin-left:40px;"><a href="#cookie%E6%97%B6%E4%BB%A3%E5%8D%B3%E5%B0%86%E7%BB%88%E7%BB%93%EF%BC%9F">cookie时代即将终结？</a></p>

<p id="web%E7%BC%93%E5%AD%98%2F%E4%BB%A3%E7%90%86%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%8A%80%E6%9C%AF-toc" style="margin-left:0px;"><a href="#web%E7%BC%93%E5%AD%98%2F%E4%BB%A3%E7%90%86%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%8A%80%E6%9C%AF">web缓存/代理服务器技术</a></p>

<p id="web%E7%BC%93%E5%AD%98%E7%A4%BA%E4%BE%8B-toc" style="margin-left:40px;"><a href="#web%E7%BC%93%E5%AD%98%E7%A4%BA%E4%BE%8B">web缓存示例</a></p>

<p id="%E9%97%AE%E9%A2%98%EF%BC%9A%E4%BB%A3%E7%90%86%E6%9C%8D%E5%8A%A1%E5%99%A8%E9%87%8C%E9%9D%A2%E7%9A%84%E7%BC%93%E5%AD%98%E7%BD%91%E9%A1%B5%E6%98%AF%E6%9C%80%E6%96%B0%E7%89%88%E7%9A%84%E5%90%97%EF%BC%8C%E5%92%8C%E5%8E%9F%E5%A7%8B%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E9%9D%A2%E7%9A%84%E4%B8%80%E8%87%B4%E5%90%97%EF%BC%9F-toc" style="margin-left:40px;"><a href="#%E9%97%AE%E9%A2%98%EF%BC%9A%E4%BB%A3%E7%90%86%E6%9C%8D%E5%8A%A1%E5%99%A8%E9%87%8C%E9%9D%A2%E7%9A%84%E7%BC%93%E5%AD%98%E7%BD%91%E9%A1%B5%E6%98%AF%E6%9C%80%E6%96%B0%E7%89%88%E7%9A%84%E5%90%97%EF%BC%8C%E5%92%8C%E5%8E%9F%E5%A7%8B%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E9%9D%A2%E7%9A%84%E4%B8%80%E8%87%B4%E5%90%97%EF%BC%9F">问题：代理服务器里面的缓存网页是最新版的吗，和原始服务器上面的一致吗？</a></p>

<p id="%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95%EF%BC%9A%E6%9D%A1%E4%BB%B6%E6%80%A7get%E6%96%B9%E6%B3%95-toc" style="margin-left:80px;"><a href="#%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95%EF%BC%9A%E6%9D%A1%E4%BB%B6%E6%80%A7get%E6%96%B9%E6%B3%95">解决方法：条件性get方法</a></p>

<hr id="hr-toc" /><h1 id="%E9%81%B5%E5%BE%AA%E7%9A%84%E5%8D%8F%E8%AE%AE"><strong>遵循的协议</strong></h1>

<p><img alt="" height="468" src="https://img-blog.csdnimg.cn/20210302093028112.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="982" /></p>

<p><img alt="" height="460" src="https://img-blog.csdnimg.cn/20210302093138419.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="982" /></p>

<p>无状态机制：不会记录谁访问过我，只要访问就响应。</p>

<p>有状态：有状态的协议更复杂，需维护状态历史信息，如果客户或服务器失效，会产生状态的不一致，解决这种不一致代价高。</p>

<h1 id="HTTP%E8%BF%9E%E6%8E%A5%E7%9A%84%E4%B8%A4%E7%A7%8D%E7%B1%BB%E5%9E%8B">HTTP连接的两种类型</h1>

<p><img alt="" height="335" src="https://img-blog.csdnimg.cn/20210302130228197.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="958" /></p>

<h2 id="%E9%9D%9E%E6%8C%81%E4%B9%85%E6%80%A7%E8%BF%9E%E6%8E%A5">非持久性连接</h2>

<p><img alt="" height="543" src="https://img-blog.csdnimg.cn/20210302130301465.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="861" /><img alt="" height="302" src="https://img-blog.csdnimg.cn/20210302130321275.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="822" /><img alt="" height="551" src="https://img-blog.csdnimg.cn/20210302130541127.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1025" /></p>

<h2 id="%E6%8C%81%E4%B9%85%E6%80%A7%E8%BF%9E%E6%8E%A5">持久性连接</h2>

<p><img alt="" height="565" src="https://img-blog.csdnimg.cn/20210302130858618.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="973" /></p>

<h2 id="%E9%87%87%E7%94%A8%E5%B8%A6%E6%9C%89%E6%B5%81%E6%B0%B4%E6%9C%BA%E5%88%B6%E7%9A%84%E6%8C%81%E4%B9%85%E6%80%A7%E8%BF%9E%E6%8E%A5%E4%BD%BF%E5%BE%97http%E9%80%9A%E4%BF%A1%E6%97%B6%E6%95%88%E7%8E%87%E6%9B%B4%E9%AB%98%E3%80%82"><strong>采用带有流水机制的持久性连接使得http通信时效率更高。</strong></h2>

<h1 id="%E5%85%B7%E4%BD%93%E7%9A%84http%E4%BF%A1%E6%81%AF"><strong>具体的http信息</strong></h1>

<h2 id="http%E8%AF%B7%E6%B1%82"><strong>http请求信息</strong></h2>

<p><img alt="" height="429" src="https://img-blog.csdnimg.cn/20210302131741147.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1040" /></p>

<h3 id="http%E8%AF%B7%E6%B1%82%E6%B6%88%E6%81%AF%E7%9A%84%E9%80%9A%E7%94%A8%E6%A0%BC%E5%BC%8F">http请求消息的通用格式</h3>

<p><img alt="" height="449" src="https://img-blog.csdnimg.cn/20210302131830269.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="816" /></p>

<p>SP：space 空格的意思</p>

<p>CR：Carriage Return，对应ASCII中转义字符\r，表示回车（回到改行最左边）</p>

<p>LF：Linefeed，对应ASCII中转义字符\n，表示换行（另起一行）</p>

<p>CRLF：Carriage Return &amp; Linefeed，\r\n，表示回车并换行</p>

<p>Windows操作系统采用两个字符来进行换行，即CRLF；Unix/Linux/Mac OS X操作系统采用单个字符LF来进行换行；另外，MacIntosh操作系统（即早期的Mac操作系统）采用单个字符CR来进行换行。</p>

<p><strong>在很久以前的机械打字机时代</strong>，CR和LF分别具有不同的作用：LF会将打印纸张上移一行位置，但是保持当前打字的水平位置不变；CR则会将“Carriage”（打字机上的滚动托架）滚回到打印纸张的最左侧，但是保持当前打字的垂直位置不变，即还是在同一行。</p>

<h2 id="http%E5%93%8D%E5%BA%94%E6%B6%88%E6%81%AF">http响应消息</h2>

<p><img alt="" height="357" src="https://img-blog.csdnimg.cn/20210302132637363.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="801" /></p>

<p> </p>

<h1 id="Cookie%E6%8A%80%E6%9C%AF">Cookie技术</h1>

<p>对于无状态的请求，产生了cookie技术，用于身份认证，购物车，推荐等等</p>

<p><img alt="" height="450" src="https://img-blog.csdnimg.cn/2021030215234550.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="953" /></p>

<h2 id="cookie%E5%8E%9F%E7%90%86">cookie原理</h2>

<p><img alt="" height="446" src="https://img-blog.csdnimg.cn/2021030215270016.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="834" /></p>

<h2 id="cookie%E7%9A%84%E9%97%AE%E9%A2%98%EF%BC%9A%E9%9A%90%E7%A7%81%E9%97%AE%E9%A2%98">cookie的问题：隐私问题</h2>

<h2 id="cookie%E6%97%B6%E4%BB%A3%E5%8D%B3%E5%B0%86%E7%BB%88%E7%BB%93%EF%BC%9F">cookie时代即将终结？</h2>

<p>可参见<a href="https://www.zhihu.com/question/21915543">浏览器 Cookies 时代要终结了吗，为何网络巨头纷纷开发自有系统，对广告业和网络应用有何影响？</a></p>

<p> </p>

<h1 id="web%E7%BC%93%E5%AD%98%2F%E4%BB%A3%E7%90%86%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%8A%80%E6%9C%AF">web缓存/代理服务器技术</h1>

<p>主要用来提升客户端访问资源的速度。</p>

<p><img alt="" height="333" src="https://img-blog.csdnimg.cn/20210302153418576.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="717" /></p>

<p><img alt="" height="405" src="https://img-blog.csdnimg.cn/20210302153533442.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="598" /></p>

<h2 id="web%E7%BC%93%E5%AD%98%E7%A4%BA%E4%BE%8B">web缓存示例</h2>

<p> </p>

<p><img alt="" height="516" src="https://img-blog.csdnimg.cn/20210302154236274.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="976" /></p>

<p><img alt="" height="532" src="https://img-blog.csdnimg.cn/20210302154349190.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="987" /><img alt="" height="508" src="https://img-blog.csdnimg.cn/20210302154251303.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="936" /></p>

<p>加一个代理服务器的效果比提升带宽还要好。</p>

<h2 id="%E9%97%AE%E9%A2%98%EF%BC%9A%E4%BB%A3%E7%90%86%E6%9C%8D%E5%8A%A1%E5%99%A8%E9%87%8C%E9%9D%A2%E7%9A%84%E7%BC%93%E5%AD%98%E7%BD%91%E9%A1%B5%E6%98%AF%E6%9C%80%E6%96%B0%E7%89%88%E7%9A%84%E5%90%97%EF%BC%8C%E5%92%8C%E5%8E%9F%E5%A7%8B%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E9%9D%A2%E7%9A%84%E4%B8%80%E8%87%B4%E5%90%97%EF%BC%9F">问题：代理服务器里面的缓存网页是最新版的吗，和原始服务器上面的一致吗？</h2>

<h3 id="%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95%EF%BC%9A%E6%9D%A1%E4%BB%B6%E6%80%A7get%E6%96%B9%E6%B3%95">解决方法：条件性get方法</h3>

<p><img alt="" height="482" src="https://img-blog.csdnimg.cn/20210302155334821.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1010" /></p>

<p>工作原理<br />
①代理缓存器（proxy cache）发送请求报文给Web服务器。<br />
②Web服务器发送具有被请求对象的响应报文给缓存器，报文带有Last-Modified:最后修改时间；缓存器存储被请求对象和最后修改时间。<br />
③当用户经过该缓存器请求同一个对象时，缓存器发送一个条件GET执行最新检查，该报文包含首部行“If-Modified-Since:最后修改时间”。<br />
④Web服务器向缓存器发送响应报文。如果该对象没有更新，则返回一个带有304状态码 Not Modified的报文，没有包含对象；如果对象已更新，则返回一个带有被请求对象的报文。<br /><br />
参考：<a href="https://blog.csdn.net/jorson2000a/article/details/81461436">《HTTP教程四》条件GET方法</a></p>

<p> </p>

<p> </p>

<p> </p>
