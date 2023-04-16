---
title: P2P应用（BT种子，Skype，洪泛式查询）
tags: 计算机网络 p2p bt
categories: 四大件之计算机网络
abbrlink: 9deba6a5
date: 2021-03-04 20:10:09
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="P2P%E5%BA%94%E7%94%A8-toc" style="margin-left:0px;"><a href="#P2P%E5%BA%94%E7%94%A8">P2P应用</a></p>

<p id="%E6%96%87%E4%BB%B6%E5%88%86%E5%8F%91%E6%97%B6%EF%BC%8C%20%E5%AE%A2%E6%88%B7%E6%9C%BA%2F%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%9E%B6%E6%9E%84%E5%92%8CP2P%E6%9E%B6%E6%9E%84%E7%9A%84%E5%AF%B9%E6%AF%94-toc" style="margin-left:40px;"><a href="#%E6%96%87%E4%BB%B6%E5%88%86%E5%8F%91%E6%97%B6%EF%BC%8C%20%E5%AE%A2%E6%88%B7%E6%9C%BA%2F%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%9E%B6%E6%9E%84%E5%92%8CP2P%E6%9E%B6%E6%9E%84%E7%9A%84%E5%AF%B9%E6%AF%94">文件分发时， 客户机/服务器架构和P2P架构的对比</a></p>

<p id="BT%E7%A7%8D%E5%AD%90-toc" style="margin-left:40px;"><a href="#BT%E7%A7%8D%E5%AD%90">BT种子</a></p>

<p id="Bittorrent%E6%8A%80%E6%9C%AF%E5%AF%B9%E7%BD%91%E7%BB%9C%E6%80%A7%E8%83%BD%E6%9C%89%E5%93%AA%E4%BA%9B%E6%BD%9C%E5%9C%A8%E7%9A%84%E5%8D%B1%E5%AE%B3%EF%BC%9F-toc" style="margin-left:80px;"><a href="#Bittorrent%E6%8A%80%E6%9C%AF%E5%AF%B9%E7%BD%91%E7%BB%9C%E6%80%A7%E8%83%BD%E6%9C%89%E5%93%AA%E4%BA%9B%E6%BD%9C%E5%9C%A8%E7%9A%84%E5%8D%B1%E5%AE%B3%EF%BC%9F">Bittorrent技术对网络性能有哪些潜在的危害？</a></p>

<p id="P2P%E7%B4%A2%E5%BC%95%EF%BC%88P2P%E5%A6%82%E4%BD%95%E6%90%9C%E7%B4%A2%E4%BF%A1%E6%81%AF%E7%9A%84%EF%BC%89-toc" style="margin-left:40px;"><a href="#P2P%E7%B4%A2%E5%BC%95%EF%BC%88P2P%E5%A6%82%E4%BD%95%E6%90%9C%E7%B4%A2%E4%BF%A1%E6%81%AF%E7%9A%84%EF%BC%89">P2P索引（P2P如何搜索信息的）</a></p>

<p id="%E9%9B%86%E4%B8%AD%E5%BC%8F%E7%B4%A2%E5%BC%95-toc" style="margin-left:80px;"><a href="#%E9%9B%86%E4%B8%AD%E5%BC%8F%E7%B4%A2%E5%BC%95">集中式索引</a></p>

<p id="%E6%B4%AA%E6%B3%9B%E5%BC%8F%E6%9F%A5%E8%AF%A2-toc" style="margin-left:80px;"><a href="#%E6%B4%AA%E6%B3%9B%E5%BC%8F%E6%9F%A5%E8%AF%A2">洪泛式查询</a></p>

<p id="%E5%B1%82%E6%AC%A1%E5%BC%8F%E8%A6%86%E7%9B%96%E7%BD%91%E7%BB%9C-toc" style="margin-left:80px;"><a href="#%E5%B1%82%E6%AC%A1%E5%BC%8F%E8%A6%86%E7%9B%96%E7%BD%91%E7%BB%9C">层次式覆盖网络（典型例子：Skype）</a></p>

<hr id="hr-toc" /><h1 id="P2P%E5%BA%94%E7%94%A8">P2P应用</h1>

<h2 id="%E6%96%87%E4%BB%B6%E5%88%86%E5%8F%91%E6%97%B6%EF%BC%8C%20%E5%AE%A2%E6%88%B7%E6%9C%BA%2F%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%9E%B6%E6%9E%84%E5%92%8CP2P%E6%9E%B6%E6%9E%84%E7%9A%84%E5%AF%B9%E6%AF%94">文件分发时， 客户机/服务器架构和P2P架构的对比</h2>

<p><img alt="" height="552" src="https://img-blog.csdnimg.cn/20210304192532454.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="965" /><img alt="" height="479" src="https://img-blog.csdnimg.cn/20210304192631911.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1037" /><img alt="" height="562" src="https://img-blog.csdnimg.cn/20210304192814115.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="994" /><img alt="" height="456" src="https://img-blog.csdnimg.cn/20210304192912494.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="787" /></p>

<p><strong>客户端/服务器架构分发文件的时间随着分发文件数量线性增加，而P2P架构所用时间会有一个界限，且较客户端/服务器小很多。</strong></p>

<p>所以下载文件时广泛使用P2P架构。</p>

<h2 id="BT%E7%A7%8D%E5%AD%90">BT种子</h2>

<p><img alt="" height="463" src="https://img-blog.csdnimg.cn/202103041932225.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="864" /></p>

<p><img alt="" height="537" src="https://img-blog.csdnimg.cn/20210304193236999.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="958" /></p>

<p><img alt="" height="458" src="https://img-blog.csdnimg.cn/20210304193438933.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="903" /></p>

<h3 id="Bittorrent%E6%8A%80%E6%9C%AF%E5%AF%B9%E7%BD%91%E7%BB%9C%E6%80%A7%E8%83%BD%E6%9C%89%E5%93%AA%E4%BA%9B%E6%BD%9C%E5%9C%A8%E7%9A%84%E5%8D%B1%E5%AE%B3%EF%BC%9F">Bittorrent技术对网络性能有哪些潜在的危害？</h3>

<p><a href="https://blog.csdn.net/weixin_33971130/article/details/92092163">BT下载的巨大危害究竟有哪些？</a></p>

<h2 id="P2P%E7%B4%A2%E5%BC%95%EF%BC%88P2P%E5%A6%82%E4%BD%95%E6%90%9C%E7%B4%A2%E4%BF%A1%E6%81%AF%E7%9A%84%EF%BC%89">P2P索引（P2P如何搜索信息的）</h2>

<p><img alt="" height="545" src="https://img-blog.csdnimg.cn/20210304194410518.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="944" /></p>

<h3 id="%E9%9B%86%E4%B8%AD%E5%BC%8F%E7%B4%A2%E5%BC%95">集中式索引</h3>

<p><img alt="" height="509" src="https://img-blog.csdnimg.cn/20210304194557390.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="878" /></p>

<h3 id="%E6%B4%AA%E6%B3%9B%E5%BC%8F%E6%9F%A5%E8%AF%A2">洪泛式查询</h3>

<p><img alt="" height="483" src="https://img-blog.csdnimg.cn/20210304194911169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="988" /><img alt="" height="417" src="https://img-blog.csdnimg.cn/20210304194938443.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="890" /></p>

<h3 id="%E5%B1%82%E6%AC%A1%E5%BC%8F%E8%A6%86%E7%9B%96%E7%BD%91%E7%BB%9C">层次式覆盖网络（<strong>典型例子：Skype</strong>）</h3>

<p><img alt="" height="351" src="https://img-blog.csdnimg.cn/20210304195036513.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="912" /></p>

<p><strong>典型例子：Skype</strong></p>

<p><img alt="" height="473" src="https://img-blog.csdnimg.cn/20210304195137453.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="960" /></p>

<p> </p>

<p> </p>

<p> </p>
