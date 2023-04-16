---
title: java中的 jsp是啥
tags: java jsp
categories: Java基础知识
abbrlink: 16690e9f
date: 2021-02-28 16:37:43
---

<!--more-->

<p>jsp是java server pages的缩写，意思是java的服务器页面，和html页面一样，用来描述网页的一种语言，后缀名是.jsp。</p>

<p id="main-toc"><strong>目录</strong></p>

<p id="jsp%E7%AE%80%E4%BB%8B-toc" style="margin-left:40px;"><a href="#jsp%E7%AE%80%E4%BB%8B">jsp简介</a></p>

<p id="jsp%E6%98%AF%E8%B0%81%E5%8F%91%E6%98%8E%E7%9A%84%E5%91%A2%EF%BC%8Chtml%E5%B0%B1%E5%8F%AF%E4%BB%A5%E5%AE%9E%E7%8E%B0%E6%98%BE%E7%A4%BA%E5%8A%9F%E8%83%BD%EF%BC%8C%E4%B8%BA%E4%BB%80%E4%B9%88%E9%9C%80%E8%A6%81jsp%E5%91%A2%EF%BC%9F-toc" style="margin-left:80px;"><a href="#jsp%E6%98%AF%E8%B0%81%E5%8F%91%E6%98%8E%E7%9A%84%E5%91%A2%EF%BC%8Chtml%E5%B0%B1%E5%8F%AF%E4%BB%A5%E5%AE%9E%E7%8E%B0%E6%98%BE%E7%A4%BA%E5%8A%9F%E8%83%BD%EF%BC%8C%E4%B8%BA%E4%BB%80%E4%B9%88%E9%9C%80%E8%A6%81jsp%E5%91%A2%EF%BC%9F">jsp是谁发明的呢，html就可以实现显示功能，为什么需要jsp呢？</a></p>

<p id="jsp%E6%9C%AC%E8%B4%A8-toc" style="margin-left:80px;"><a href="#jsp%E6%9C%AC%E8%B4%A8">jsp本质</a></p>

<p id="jsp%E7%9A%84%E5%85%B7%E4%BD%93%E5%86%85%E5%AE%B9%E5%8F%AF%E5%8F%82%E8%A7%81%EF%BC%9A-toc" style="margin-left:40px;"><a href="#jsp%E7%9A%84%E5%85%B7%E4%BD%93%E5%86%85%E5%AE%B9%E5%8F%AF%E5%8F%82%E8%A7%81%EF%BC%9A">jsp的具体内容可参见：</a></p>

<p id="jsp%E7%9A%84%E4%B9%9D%E5%A4%A7%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1-toc" style="margin-left:80px;"><a href="#jsp%E7%9A%84%E4%B9%9D%E5%A4%A7%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1">jsp的九大内置对象</a></p>

<p id="jsp%E7%9A%84%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93%EF%BC%88jsp%E7%9A%84%E5%A4%B4%E9%83%A8%E6%8C%87%E4%BB%A4%EF%BC%8C%E5%B8%B8%E7%94%A8%E8%84%9A%E6%9C%AC%EF%BC%8C%E5%B8%B8%E7%94%A8%E6%A0%87%E7%AD%BE%EF%BC%89%E4%BB%A5%E5%8F%8A%E7%BB%83%E4%B9%A0%E9%A2%98%EF%BC%88%E9%99%84%E4%BB%A3%E7%A0%81%EF%BC%89-toc" style="margin-left:80px;"><a href="#jsp%E7%9A%84%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93%EF%BC%88jsp%E7%9A%84%E5%A4%B4%E9%83%A8%E6%8C%87%E4%BB%A4%EF%BC%8C%E5%B8%B8%E7%94%A8%E8%84%9A%E6%9C%AC%EF%BC%8C%E5%B8%B8%E7%94%A8%E6%A0%87%E7%AD%BE%EF%BC%89%E4%BB%A5%E5%8F%8A%E7%BB%83%E4%B9%A0%E9%A2%98%EF%BC%88%E9%99%84%E4%BB%A3%E7%A0%81%EF%BC%89">jsp的知识点总结（jsp的头部指令，常用脚本，常用标签）以及练习题（附代码）</a></p>

<hr id="hr-toc" /><h2 id="jsp%E7%AE%80%E4%BB%8B">jsp简介</h2>

<h3 id="jsp%E6%98%AF%E8%B0%81%E5%8F%91%E6%98%8E%E7%9A%84%E5%91%A2%EF%BC%8Chtml%E5%B0%B1%E5%8F%AF%E4%BB%A5%E5%AE%9E%E7%8E%B0%E6%98%BE%E7%A4%BA%E5%8A%9F%E8%83%BD%EF%BC%8C%E4%B8%BA%E4%BB%80%E4%B9%88%E9%9C%80%E8%A6%81jsp%E5%91%A2%EF%BC%9F">jsp是谁发明的呢，html就可以实现显示功能，为什么需要jsp呢？</h3>

<p>jsp是sun公司（已被甲骨文公司收购）发明的一种动态网页技术标准，可以代替servlet程序回传html界面的数据，就比如说我点击登录，完后这个页面就需要跳转到index页面，那么这个index页面怎么显示在浏览器上呢，一种方法是把这个页面都用java程序写在doGet函数中，这样就很繁琐，比如下面这种。</p>

<p><img alt="" height="626" src="https://img-blog.csdnimg.cn/20210227204609923.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="961" /></p>

<p>执行servlet程序后，页面刷新到客户端，这样的话要在servlet程序中写出所有的html页面的程序，所以很麻烦。如果用jsp的话就像html语言一样简单，还有语法纠错功能。</p>

<p> </p>

<h3 id="jsp%E6%9C%AC%E8%B4%A8">jsp本质</h3>

<p>jsp本质就是servlet程序，因为jsp文件反编译成的.class文件和servlet程序反编译的结果一样的。</p>

<p><strong>怎么看jsp页面对应的java和.class文件呢？</strong></p>

<p>启动服务器后去这个地址下看</p>

<p><img alt="" height="248" src="https://img-blog.csdnimg.cn/20210227204726147.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1200" /></p>

<p> </p>

<p><img alt="" height="126" src="https://img-blog.csdnimg.cn/20210227204812277.png" width="1200" /></p>

<p>因为这个index_jsp类（index.jsp对应的类）是继承于org.apache.jasper.runtime.HttpJspBase的，而这个玩意继承于HttpServlet，所以jsp和servlet是一样的，具体看反编译的语句都是一样的。</p>

<h2 id="jsp%E7%9A%84%E5%85%B7%E4%BD%93%E5%86%85%E5%AE%B9%E5%8F%AF%E5%8F%82%E8%A7%81%EF%BC%9A"><strong>jsp的具体内容可参见：</strong></h2>

<h3 id="jsp%E7%9A%84%E4%B9%9D%E5%A4%A7%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1"><a href="https://blog.csdn.net/weixin_40757930/article/details/114208924">jsp的九大内置对象</a></h3>

<h3 id="jsp%E7%9A%84%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93%EF%BC%88jsp%E7%9A%84%E5%A4%B4%E9%83%A8%E6%8C%87%E4%BB%A4%EF%BC%8C%E5%B8%B8%E7%94%A8%E8%84%9A%E6%9C%AC%EF%BC%8C%E5%B8%B8%E7%94%A8%E6%A0%87%E7%AD%BE%EF%BC%89%E4%BB%A5%E5%8F%8A%E7%BB%83%E4%B9%A0%E9%A2%98%EF%BC%88%E9%99%84%E4%BB%A3%E7%A0%81%EF%BC%89"><a href="https://blog.csdn.net/weixin_40757930/article/details/114191905">jsp的知识点总结（jsp的头部指令，常用脚本，常用标签）以及练习题（附代码）</a></h3>

<p>资料来源：B站尚硅谷</p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>

<pre>

 </pre>

<p> </p>
