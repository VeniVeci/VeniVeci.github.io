---
title: EL表达式
tags: java javaweb
categories: Java基础知识
abbrlink: e940c1fa
date: 2021-03-01 20:22:05
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="%E4%BB%80%E4%B9%88%E6%98%AFEL%E8%A1%A8%E8%BE%BE%E5%BC%8F-toc" style="margin-left:0px;"><a href="#%E4%BB%80%E4%B9%88%E6%98%AFEL%E8%A1%A8%E8%BE%BE%E5%BC%8F">什么是EL表达式</a></p>

<p id="%E7%A4%BA%E4%BE%8B-toc" style="margin-left:40px;"><a href="#%E7%A4%BA%E4%BE%8B">示例</a></p>

<p id="EL%20%E8%A1%A8%E8%BE%BE%E5%BC%8F%E8%BE%93%E5%87%BA%20Bean%20%E7%9A%84%E6%99%AE%E9%80%9A%E5%B1%9E%E6%80%A7%EF%BC%8C%E6%95%B0%E7%BB%84%E5%B1%9E%E6%80%A7%EF%BC%8CList%20%E9%9B%86%E5%90%88%E5%B1%9E%E6%80%A7%EF%BC%8Cmap%20%E9%9B%86%E5%90%88%E5%B1%9E%E6%80%A7-toc" style="margin-left:40px;"><a href="#EL%20%E8%A1%A8%E8%BE%BE%E5%BC%8F%E8%BE%93%E5%87%BA%20Bean%20%E7%9A%84%E6%99%AE%E9%80%9A%E5%B1%9E%E6%80%A7%EF%BC%8C%E6%95%B0%E7%BB%84%E5%B1%9E%E6%80%A7%EF%BC%8CList%20%E9%9B%86%E5%90%88%E5%B1%9E%E6%80%A7%EF%BC%8Cmap%20%E9%9B%86%E5%90%88%E5%B1%9E%E6%80%A7">EL 表达式输出 Bean 的普通属性，数组属性，List 集合属性，map 集合属性</a></p>

<p id="javaBean%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9A-toc" style="margin-left:80px;"><a href="#javaBean%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9A">javaBean是什么：</a></p>

<p id="EL%E8%A1%A8%E8%BE%BE%E5%BC%8F%E5%A6%82%E4%BD%95%E8%BF%9B%E8%A1%8C%E8%BF%90%E7%AE%97-toc" style="margin-left:0px;"><a href="#EL%E8%A1%A8%E8%BE%BE%E5%BC%8F%E5%A6%82%E4%BD%95%E8%BF%9B%E8%A1%8C%E8%BF%90%E7%AE%97">EL表达式如何进行运算</a></p>

<p id="%E5%85%B3%E7%B3%BB%E8%BF%90%E7%AE%97%E5%92%8C%E9%80%BB%E8%BE%91%E8%BF%90%E7%AE%97-toc" style="margin-left:40px;"><a href="#%E5%85%B3%E7%B3%BB%E8%BF%90%E7%AE%97%E5%92%8C%E9%80%BB%E8%BE%91%E8%BF%90%E7%AE%97">关系运算和逻辑运算</a></p>

<p id="%E7%AE%97%E6%9C%AF%E8%BF%90%E7%AE%97-toc" style="margin-left:40px;"><a href="#%E7%AE%97%E6%9C%AF%E8%BF%90%E7%AE%97">算术运算</a></p>

<p id="EL%20%E8%A1%A8%E8%BE%BE%E5%BC%8F%E7%9A%84%2011%20%E4%B8%AA%E9%9A%90%E5%90%AB%E5%AF%B9%E8%B1%A1-toc" style="margin-left:0px;"><a href="#EL%20%E8%A1%A8%E8%BE%BE%E5%BC%8F%E7%9A%84%2011%20%E4%B8%AA%E9%9A%90%E5%90%AB%E5%AF%B9%E8%B1%A1">EL 表达式的 11 个隐含对象</a></p>

<p id="%E8%AE%BF%E9%97%AE%E5%9B%9B%E4%B8%AA%E7%89%B9%E5%AE%9A%E5%9F%9F%E4%B8%AD%E7%9A%84%E5%B1%9E%E6%80%A7-toc" style="margin-left:40px;"><a href="#%E8%AE%BF%E9%97%AE%E5%9B%9B%E4%B8%AA%E7%89%B9%E5%AE%9A%E5%9F%9F%E4%B8%AD%E7%9A%84%E5%B1%9E%E6%80%A7">访问四个特定域中的属性</a></p>

<p id="PageContext%E7%9A%84%E4%BD%BF%E7%94%A8-toc" style="margin-left:40px;"><a href="#PageContext%E7%9A%84%E4%BD%BF%E7%94%A8">PageContext的使用</a></p>

<p id="%E5%85%B6%E4%BB%96%E9%9A%90%E5%90%AB%E5%AF%B9%E8%B1%A1%E7%9A%84%E4%BD%BF%E7%94%A8-toc" style="margin-left:40px;"><a href="#%E5%85%B6%E4%BB%96%E9%9A%90%E5%90%AB%E5%AF%B9%E8%B1%A1%E7%9A%84%E4%BD%BF%E7%94%A8">其他隐含对象的使用</a></p>

<hr id="hr-toc" /><h1 id="%E4%BB%80%E4%B9%88%E6%98%AFEL%E8%A1%A8%E8%BE%BE%E5%BC%8F">什么是EL表达式</h1>

<p>EL表达式的全称是： Expression Language，是表达式语言。<br />
EL表达式的什么作用：EL表达式主要是代替<span style="color:#f33b45;"><strong>jsp页面中的表达式脚本</strong></span>在jsp页面中进行数据的输出<br />
因为EL表达式在输出数据的时候，要比jsp的表达式脚本要简洁很多。</p>

<h2 id="%E7%A4%BA%E4%BE%8B">示例</h2>

<blockquote>
<div><span style="color:#000000;">&lt;</span><span style="color:#000080;"><strong>body</strong></span><span style="color:#000000;">&gt; </span></div>

<div><span style="color:#000080;"><strong>&lt;%</strong></span><span style="color:#000000;">request.setAttribute(</span><span style="color:#008000;"><strong>"key"</strong></span><span style="color:#000000;">,</span><span style="color:#008000;"><strong>"</strong></span><span style="color:#008000;"><strong>值</strong></span><span style="color:#008000;"><strong>"</strong></span><span style="color:#000000;">);</span><span style="color:#000080;"><strong>%&gt;</strong></span></div>

<div> </div>

<div><span style="color:#000000;">表达式脚本输出 </span><span style="color:#000000;">key </span><span style="color:#000000;">的值是：</span><span style="color:#000080;"><strong>&lt;%=</strong></span><span style="color:#000000;">request.getAttribute(</span><span style="color:#008000;"><strong>"key1"</strong></span><span style="color:#000000;">)==</span><span style="color:#000080;"><strong>null</strong></span><span style="color:#000000;">?</span><span style="color:#008000;"><strong>""</strong></span><span style="color:#000000;">:request.getAttribute(</span><span style="color:#008000;"><strong>"key1"</strong></span><span style="color:#000000;">)</span><span style="color:#000080;"><strong>%&gt;</strong></span><span style="color:#000000;">&lt;</span><span style="color:#000080;"><strong>br</strong></span><span style="color:#000000;">/&gt; </span></div>

<div> </div>

<div><span style="color:#f33b45;">EL 表达式输出 key 的值是：<strong>${</strong>key1<strong>} </strong></span></div>

<div><span style="color:#000000;">&lt;/</span><span style="color:#000080;"><strong>body</strong></span><span style="color:#000000;">&gt; </span></div>
</blockquote>

<p>EL表达式在输出nul值的时候，输出的是空串。jsp表达式脚本输出null值的时候，输出的是null字符串。</p>

<p>EL表达式主要是在jsp页面中输出数据，主要是输出域对象中的数据。<br />
当四个域中都有相同的key的数据的时候，EL表达式会按照四个域的从小到大的顺序去进行搜素，找到就输出。</p>

<h2 id="EL%20%E8%A1%A8%E8%BE%BE%E5%BC%8F%E8%BE%93%E5%87%BA%20Bean%20%E7%9A%84%E6%99%AE%E9%80%9A%E5%B1%9E%E6%80%A7%EF%BC%8C%E6%95%B0%E7%BB%84%E5%B1%9E%E6%80%A7%EF%BC%8CList%20%E9%9B%86%E5%90%88%E5%B1%9E%E6%80%A7%EF%BC%8Cmap%20%E9%9B%86%E5%90%88%E5%B1%9E%E6%80%A7"><span style="color:#000000;"><strong>EL </strong></span><span style="color:#000000;"><strong>表达式输出 </strong></span><span style="color:#000000;"><strong>Bean </strong></span><span style="color:#000000;"><strong>的普通属性，数组属性，</strong></span><span style="color:#000000;"><strong>List </strong></span><span style="color:#000000;"><strong>集合属性，</strong></span><span style="color:#000000;"><strong>map </strong></span><span style="color:#000000;"><strong>集合属性</strong></span></h2>

<div><img alt="" height="422" src="https://img-blog.csdnimg.cn/20210301121212624.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="699" /></div>

<h3 id="javaBean%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9A"><span style="color:#000000;"><strong>javaBean是什么：</strong></span></h3>

<p><span style="color:#000000;"><strong>就是通常意义上的类，提供了一些方法可以访问类的属性。</strong></span></p>

<div>1、所有属性为private<br />
2、提供默认构造方法<br />
3、提供getter和setter<br />
4、实现serializable接口</div>

<div><a href="https://www.zhihu.com/question/19773379/answer/31625054">Java bean 是个什么概念？ - 杨博的回答 - 知乎</a></div>

<div> </div>

<h1 id="EL%E8%A1%A8%E8%BE%BE%E5%BC%8F%E5%A6%82%E4%BD%95%E8%BF%9B%E8%A1%8C%E8%BF%90%E7%AE%97">EL表达式如何进行运算</h1>

<h2 id="%E5%85%B3%E7%B3%BB%E8%BF%90%E7%AE%97%E5%92%8C%E9%80%BB%E8%BE%91%E8%BF%90%E7%AE%97">关系运算和逻辑运算</h2>

<p><img alt="" height="550" src="https://img-blog.csdnimg.cn/20210301121549303.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="924" /></p>

<h2 id="%E7%AE%97%E6%9C%AF%E8%BF%90%E7%AE%97"><strong>算术运算</strong></h2>

<p><img alt="" height="159" src="https://img-blog.csdnimg.cn/20210301121603531.png" width="843" /></p>

<h1 id="EL%20%E8%A1%A8%E8%BE%BE%E5%BC%8F%E7%9A%84%2011%20%E4%B8%AA%E9%9A%90%E5%90%AB%E5%AF%B9%E8%B1%A1"><span style="color:#000000;"><strong>EL </strong></span><span style="color:#000000;"><strong>表达式的 </strong></span><span style="color:#000000;"><strong>11 </strong></span><span style="color:#000000;"><strong>个隐含对象</strong></span></h1>

<p><img alt="" height="457" src="https://img-blog.csdnimg.cn/20210301123741877.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="887" /></p>

<p><img alt="" height="37" src="https://img-blog.csdnimg.cn/20210301123753189.png" width="891" /></p>

<h2 id="%E8%AE%BF%E9%97%AE%E5%9B%9B%E4%B8%AA%E7%89%B9%E5%AE%9A%E5%9F%9F%E4%B8%AD%E7%9A%84%E5%B1%9E%E6%80%A7">访问四个特定域中的属性</h2>

<p><img alt="" height="491" src="https://img-blog.csdnimg.cn/20210301123811323.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="687" /></p>

<h2 id="PageContext%E7%9A%84%E4%BD%BF%E7%94%A8">PageContext的使用</h2>

<p>因为它可以获取九大内置对象，所以功能强大：</p>

<p><img alt="" height="375" src="https://img-blog.csdnimg.cn/20210301124224896.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="594" /></p>

<p><img alt="" height="240" src="https://img-blog.csdnimg.cn/20210301124236362.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="637" /></p>

<p>直接用.就可以得到协议、服务器IP等信息。</p>

<h2 id="%E5%85%B6%E4%BB%96%E9%9A%90%E5%90%AB%E5%AF%B9%E8%B1%A1%E7%9A%84%E4%BD%BF%E7%94%A8">其他隐含对象的使用</h2>

<p>重点是param和paramvalues</p>

<p><img alt="" height="435" src="https://img-blog.csdnimg.cn/20210301124618666.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="987" /></p>

<p> </p>

<p> </p>
