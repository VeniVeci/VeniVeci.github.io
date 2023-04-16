---
title: DNS服务（域名解析服务）
tags: 计算机网络 dns服务器
categories: 四大件之计算机网络
abbrlink: 2567b11
date: 2021-03-04 20:03:00
---

<!--more-->

<p><strong>DNS服务（域名解析服务），提供ip地址和域名之间的映射关系，这样我们在访问www.baidu.com的时候DNS就知道它的ip地址是202.108.22.5。</strong></p>

<p id="main-toc"><strong>目录</strong></p>

<p id="DNS%E6%9C%8D%E5%8A%A1-toc" style="margin-left:0px;"><a href="#DNS%E6%9C%8D%E5%8A%A1">DNS服务</a></p>

<p id="DNS%E6%9C%8D%E5%8A%A1%E9%87%87%E7%94%A8%E7%9A%84%E6%98%AF%E5%88%86%E5%B8%83%E5%BC%8F%E5%B1%82%E6%AC%A1%E5%BC%8F%E7%9A%84%E6%95%B0%E6%8D%AE%E5%BA%93-toc" style="margin-left:40px;"><a href="#DNS%E6%9C%8D%E5%8A%A1%E9%87%87%E7%94%A8%E7%9A%84%E6%98%AF%E5%88%86%E5%B8%83%E5%BC%8F%E5%B1%82%E6%AC%A1%E5%BC%8F%E7%9A%84%E6%95%B0%E6%8D%AE%E5%BA%93">DNS服务采用的是分布式层次式的数据库</a></p>

<p id="%E5%85%A8%E7%90%8313%E4%B8%AAipv4%E6%A0%B9%E5%9F%9F%E5%90%8D%E6%9C%8D%E5%8A%A1%E5%99%A8%E9%83%BD%E5%9C%A8%E5%A4%96%E5%9B%BD%EF%BC%8C%E4%BC%9A%E5%AF%B9%E6%88%91%E5%9B%BD%E7%9A%84%E7%BD%91%E7%BB%9C%E5%AE%89%E5%85%A8%E9%80%A0%E6%88%90%E4%BB%80%E4%B9%88%E5%BD%B1%E5%93%8D%EF%BC%9A-toc" style="margin-left:40px;"><a href="#%E5%85%A8%E7%90%8313%E4%B8%AAipv4%E6%A0%B9%E5%9F%9F%E5%90%8D%E6%9C%8D%E5%8A%A1%E5%99%A8%E9%83%BD%E5%9C%A8%E5%A4%96%E5%9B%BD%EF%BC%8C%E4%BC%9A%E5%AF%B9%E6%88%91%E5%9B%BD%E7%9A%84%E7%BD%91%E7%BB%9C%E5%AE%89%E5%85%A8%E9%80%A0%E6%88%90%E4%BB%80%E4%B9%88%E5%BD%B1%E5%93%8D%EF%BC%9A">全球13个ipv4根域名服务器都在外国，会对我国的网络安全造成什么影响：</a></p>

<p id="%E5%9F%9F%E5%90%8D%E6%9F%A5%E8%AF%A2%E6%96%B9%E6%B3%95-toc" style="margin-left:40px;"><a href="#%E5%9F%9F%E5%90%8D%E6%9F%A5%E8%AF%A2%E6%96%B9%E6%B3%95">域名查询方法</a></p>

<p id="%E8%BF%AD%E4%BB%A3%E6%9F%A5%E8%AF%A2-toc" style="margin-left:80px;"><a href="#%E8%BF%AD%E4%BB%A3%E6%9F%A5%E8%AF%A2">迭代查询</a></p>

<p id="%E9%80%92%E5%BD%92%E6%9F%A5%E8%AF%A2-toc" style="margin-left:80px;"><a href="#%E9%80%92%E5%BD%92%E6%9F%A5%E8%AF%A2">递归查询</a></p>

<hr id="hr-toc" /><h1 id="DNS%E6%9C%8D%E5%8A%A1"><strong>DNS服务</strong></h1>

<p><img alt="" height="449" src="https://img-blog.csdnimg.cn/20210304182000915.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="609" /></p>

<h2 id="DNS%E6%9C%8D%E5%8A%A1%E9%87%87%E7%94%A8%E7%9A%84%E6%98%AF%E5%88%86%E5%B8%83%E5%BC%8F%E5%B1%82%E6%AC%A1%E5%BC%8F%E7%9A%84%E6%95%B0%E6%8D%AE%E5%BA%93">DNS服务采用的是分布式层次式的数据库</h2>

<p><img alt="" height="514" src="https://img-blog.csdnimg.cn/20210304182227379.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="969" /><img alt="" height="466" src="https://img-blog.csdnimg.cn/20210304182559826.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="902" /></p>

<h2 id="%E5%85%A8%E7%90%8313%E4%B8%AAipv4%E6%A0%B9%E5%9F%9F%E5%90%8D%E6%9C%8D%E5%8A%A1%E5%99%A8%E9%83%BD%E5%9C%A8%E5%A4%96%E5%9B%BD%EF%BC%8C%E4%BC%9A%E5%AF%B9%E6%88%91%E5%9B%BD%E7%9A%84%E7%BD%91%E7%BB%9C%E5%AE%89%E5%85%A8%E9%80%A0%E6%88%90%E4%BB%80%E4%B9%88%E5%BD%B1%E5%93%8D%EF%BC%9A">全球13个ipv4根域名服务器都在外国，会对我国的网络安全造成什么影响：</h2>

<p><strong>其实问题是有依据的，2003年伊拉克战争期间，伊拉克断网，20004年让利比亚断网3天。</strong>在2010年的伊朗的核泄漏事件，就是因为没有掌握自己本国的网络主导权，进而被攻击的。缺乏根服务器使得相关国家抵御大规模“分布式拒绝服务”攻击能力不足，为互联网安全带来隐患，</p>

<p>我们大可不必担心这个问题，我国早在03至04年就已经镜像了全球的根服务器。<strong>通俗地讲，就是中国把所有互联网服务器的数据都抄了一份。</strong>这样，我们在访问世界网络时，可以使用自己服务器，并不需要通过美国的服务器！在少数极端情况下(比如全球互联网出现大面积瘫痪、或者中国互联网国际出口堵塞)，至少能保证国内的站点由国内的域名服务器来解析。虽然国外的用户连接到我国的网络会出现问题，但是我国可以自己解决中国境内的域名解析问题，保证国内网络正常使用。<br /><br />
　　在IPv4时代美国还是挺强大的，基本上是“美国说了算”，但是到了IPv6时代就不同了；<br /><br />
　　”雪人计划“是由中国领衔发起的，是基于IPv6的根服务器测试和运营实验项目，用来打破现有的根服务器困局，为IPv6提供根服务器解决方案；<br /><br />
　　目前在全球已经部署了25台IPv6根服务器，其中中国部署了4台，1台主根服务器，3台辅根服务器，打破了中国没有根服务器的局面。</p>

<p><img alt="" height="405" src="https://img-blog.csdnimg.cn/20210304190422419.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="983" /></p>

<h2 id="%E5%9F%9F%E5%90%8D%E6%9F%A5%E8%AF%A2%E6%96%B9%E6%B3%95">域名查询方法</h2>

<h3 id="%E8%BF%AD%E4%BB%A3%E6%9F%A5%E8%AF%A2">迭代查询</h3>

<p><img alt="" height="504" src="https://img-blog.csdnimg.cn/20210304190853763.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="955" /></p>

<h3 id="%E9%80%92%E5%BD%92%E6%9F%A5%E8%AF%A2">递归查询</h3>

<p><img alt="" height="495" src="https://img-blog.csdnimg.cn/20210304190941604.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1003" /></p>

<p> </p>

<p><img alt="" height="377" src="https://img-blog.csdnimg.cn/20210304191155999.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="937" /><br />
 </p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>
