---
title: jsp的九大内置对象
tags: java jsp
categories: Java基础知识
abbrlink: df3b574
date: 2021-02-28 16:29:49
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="jsp%E7%9A%84%E4%B9%9D%E5%A4%A7%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1-toc" style="margin-left:0px;"><a href="#jsp%E7%9A%84%E4%B9%9D%E5%A4%A7%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1">jsp的九大内置对象</a></p>

<p id="%E5%9B%9B%E4%B8%AA%E5%9F%9F%E5%AF%B9%E8%B1%A1-toc" style="margin-left:40px;"><a href="#%E5%9B%9B%E4%B8%AA%E5%9F%9F%E5%AF%B9%E8%B1%A1">四个域对象</a></p>

<p id="out%E5%AF%B9%E8%B1%A1-toc" style="margin-left:40px;"><a href="#out%E5%AF%B9%E8%B1%A1">out对象</a></p>

<p id="out%E5%92%8Cresponse.getWriter%E8%BE%93%E5%87%BA%E7%9A%84%E7%89%B9%E7%82%B9-toc" style="margin-left:80px;"><a href="#out%E5%92%8Cresponse.getWriter%E8%BE%93%E5%87%BA%E7%9A%84%E7%89%B9%E7%82%B9">out和response.getWriter输出的特点</a></p>

<hr id="hr-toc" /><h1 id="jsp%E7%9A%84%E4%B9%9D%E5%A4%A7%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1">jsp的九大内置对象</h1>

<p><strong>jsp中的内置对象，是指 Tomcat在翻译jsp页面成为 Servlet源代码后，内部提供的九大对象，叫内置对象，内置的意思就是本身就有，我们可以直接用。</strong></p>

<p><strong>特别request对象，可以帮助我们很便捷的处理servlet请求转发。</strong></p>

<p><strong>具体可参见 </strong><a href="https://blog.csdn.net/weixin_40757930/article/details/114191905">jsp的知识点总结（jsp的头部指令，常用脚本，常用标签）以及练习题（附代码） </a>中的练习题2</p>

<p><img alt="" height="271" src="https://img-blog.csdnimg.cn/20210228094250271.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="515" /></p>

<h2 id="%E5%9B%9B%E4%B8%AA%E5%9F%9F%E5%AF%B9%E8%B1%A1">四个域对象</h2>

<p><img alt="" height="565" src="https://img-blog.csdnimg.cn/20210228094938880.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1200" /></p>

<p>request是一次请求内有效，比如我写一个请求转发的语句，转发跳到另一个界面后，该域还可以访问，但是再请求一次就不能访问该域了。</p>

<p>session重启浏览器后不能访问，可以理解为该域保存在浏览器中。</p>

<p>application可以认为是保存在本地web工程中，重启或者重新部署都不能再访问，当然这个时候session、request、pagecontext也会刷新。</p>

<p> </p>

<h2 id="out%E5%AF%B9%E8%B1%A1">out对象</h2>

<h3 id="out%E5%92%8Cresponse.getWriter%E8%BE%93%E5%87%BA%E7%9A%84%E7%89%B9%E7%82%B9">out和response.<span style="color:#000000;"><strong>getWriter</strong></span>输出的特点</h3>

<p><img alt="" height="490" src="https://img-blog.csdnimg.cn/20210228101000862.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1200" /></p>

<p><strong>不管out语句在response语句的上面还是下面，输出时总是response语句的内容在上面，因为在向客户端输出时，out缓冲区中的内容会追加到response缓冲区中，统一输出。</strong></p>

<p>由于jsp翻译之后，底层源代码都是使用out来进行输出，所以一般情况下。我们在jsp页面中统一使用out来进行输出。避免打乱页面输出内容的顺序。<br />
out. write()输出字符串没有问题<br />
out. print()输出任意数据都没有问题（都转换成为字符串后调用的wite输出）<br /><strong>结论：在jsp页面中，可以统一使用out. print()来进行输出</strong></p>

<p>资料来源：B站尚硅谷</p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>
