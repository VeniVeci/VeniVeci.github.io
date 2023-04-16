---
title: Maven
tags: java maven 大数据 spring
categories: Java基础知识
abbrlink: 7273cdc
date: 2021-03-28 15:53:08
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="Maven%20%E7%BF%BB%E8%AF%91%E4%B8%BA%22%E4%B8%93%E5%AE%B6%22%E3%80%81%22%E5%86%85%E8%A1%8C%22%EF%BC%8C%E6%98%AF%20Apache%20%E4%B8%8B%E7%9A%84%E4%B8%80%E4%B8%AA%E7%BA%AF%20Java%20%E5%BC%80%E5%8F%91%E7%9A%84%E5%BC%80%E6%BA%90%E9%A1%B9%E7%9B%AE%E3%80%82-toc" style="margin-left:0px;"><a href="#Maven%20%E7%BF%BB%E8%AF%91%E4%B8%BA%22%E4%B8%93%E5%AE%B6%22%E3%80%81%22%E5%86%85%E8%A1%8C%22%EF%BC%8C%E6%98%AF%20Apache%20%E4%B8%8B%E7%9A%84%E4%B8%80%E4%B8%AA%E7%BA%AF%20Java%20%E5%BC%80%E5%8F%91%E7%9A%84%E5%BC%80%E6%BA%90%E9%A1%B9%E7%9B%AE%E3%80%82">Maven 翻译为"专家"、"内行"，是 Apache 下的一个纯 Java 开发的开源项目。</a></p>

<p id="%E4%B8%BA%E4%BB%80%E4%B9%88%E9%9C%80%E8%A6%81maven-toc" style="margin-left:40px;"><a href="#%E4%B8%BA%E4%BB%80%E4%B9%88%E9%9C%80%E8%A6%81maven">为什么需要maven</a></p>

<p id="maven%E7%9A%84%E6%9E%84%E5%BB%BA%E8%BF%87%E7%A8%8B-toc" style="margin-left:40px;"><a href="#maven%E7%9A%84%E6%9E%84%E5%BB%BA%E8%BF%87%E7%A8%8B">maven的构建过程</a></p>

<p id="%E7%BA%A6%E5%AE%9A%E5%A4%A7%E4%BA%8E%E9%85%8D%E7%BD%AE-toc" style="margin-left:40px;"><a href="#%E7%BA%A6%E5%AE%9A%E5%A4%A7%E4%BA%8E%E9%85%8D%E7%BD%AE">约定大于配置</a></p>

<p id="POM-toc" style="margin-left:40px;"><a href="#POM">POM</a></p>

<p id="%E4%BE%9D%E8%B5%96-toc" style="margin-left:40px;"><a href="#%E4%BE%9D%E8%B5%96">依赖</a></p>

<p id="%E9%81%87%E5%88%B0%E7%9A%84%E9%97%AE%E9%A2%98%EF%BC%9A-toc" style="margin-left:40px;"><a href="#%E9%81%87%E5%88%B0%E7%9A%84%E9%97%AE%E9%A2%98%EF%BC%9A">遇到的问题：</a></p>

<hr id="hr-toc" /><h1 id="Maven%20%E7%BF%BB%E8%AF%91%E4%B8%BA%22%E4%B8%93%E5%AE%B6%22%E3%80%81%22%E5%86%85%E8%A1%8C%22%EF%BC%8C%E6%98%AF%20Apache%20%E4%B8%8B%E7%9A%84%E4%B8%80%E4%B8%AA%E7%BA%AF%20Java%20%E5%BC%80%E5%8F%91%E7%9A%84%E5%BC%80%E6%BA%90%E9%A1%B9%E7%9B%AE%E3%80%82">Maven 翻译为"专家"、"内行"，是 Apache 下的一个纯 Java 开发的开源项目。</h1>

<h2 id="%E4%B8%BA%E4%BB%80%E4%B9%88%E9%9C%80%E8%A6%81maven">为什么需要maven</h2>

<div><span style="color:#000000;">生产环境下开发不再是一个项目一个工程，而是每一个模块创建一个工程，而多个模块整合在一起就需要 </span></div>

<div><span style="color:#000000;">使用到像 </span><span style="color:#000000;">Maven </span><span style="color:#000000;">这样的构建工具</span></div>

<div><span style="color:#000000;">如果没有maven，照样可以开发项目，那么maven的必要性体现在哪里呢？</span></div>

<div>用了maven，可以省掉很多重复的步骤，大大提高开发人员的效率。</div>

<div> </div>

<h2 id="maven%E7%9A%84%E6%9E%84%E5%BB%BA%E8%BF%87%E7%A8%8B">maven的构建过程</h2>

<p><img alt="" height="320" src="https://img-blog.csdnimg.cn/20210326202348959.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1200" /></p>

<div> </div>

<p>基于项目对象模型（缩写：POM）概念，Maven利用一个中央信息片断能管理一个项目的构建、报告和文档等步骤。</p>

<h2 id="%E7%BA%A6%E5%AE%9A%E5%A4%A7%E4%BA%8E%E9%85%8D%E7%BD%AE">约定大于配置</h2>

<p><img alt="" height="710" src="https://img-blog.csdnimg.cn/202103262024447.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1148" /></p>

<p><img alt="" height="692" src="https://img-blog.csdnimg.cn/20210325220756775.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1071" /></p>

<p>意思就是maven这样规定目录结构，你就把对应的文件按照约定放进去就好了，不要乱放，这样maven就可以很精确的进行构建了。</p>

<h2 id="POM">POM</h2>

<div><span style="color:#000000;">Project Object Model</span><span style="color:#000000;">：项目对象模型。将 </span><span style="color:#000000;">Java </span><span style="color:#0000ff;"><strong>工程</strong></span><span style="color:#000000;">的相关信息封装为</span><span style="color:#0000ff;"><strong>对象</strong></span><span style="color:#000000;">作为便于操作和管理的</span><span style="color:#0000ff;"><strong>模型</strong></span><span style="color:#000000;">。 </span></div>

<div><span style="color:#000000;">Maven </span><span style="color:#000000;">工程的核心配置。可以说学习 </span><span style="color:#000000;">Maven </span><span style="color:#000000;">就是学习 </span><span style="color:#000000;">pom.xml </span><span style="color:#000000;">文件中的配置</span></div>

<div> </div>

<h2 id="%E4%BE%9D%E8%B5%96"><span style="color:#000000;">依赖</span></h2>

<p><img alt="" height="440" src="https://img-blog.csdnimg.cn/20210326203423404.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1156" /></p>

<p><img alt="" height="213" src="https://img-blog.csdnimg.cn/20210326203519655.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1169" /></p>

<h2 id="%E9%81%87%E5%88%B0%E7%9A%84%E9%97%AE%E9%A2%98%EF%BC%9A">遇到的问题：</h2>

<p>没有办法导入对应的依赖，结果发现自己的maven版本是3.6.3，过高了，采用idea自带的那个版本就ok了，还不用自己配置。</p>

<p>com.google.inject.CreationException: Unable to create injector</p>

<p><img alt="" height="226" src="https://img-blog.csdnimg.cn/20210328155104265.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="950" /></p>

<p> </p>

<p>资料来源  B站尚硅谷</p>

<p> </p>
