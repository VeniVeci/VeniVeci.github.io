---
title: JVM的构成 (类加载子系统、执行引擎、运行时数据区)
tags: JVM
categories: Java基础知识 JVM
abbrlink: 8c5bb365
date: 2022-04-03 17:25:04
---

<!--more-->

<p> </p>

<p id="main-toc"><strong>目录</strong></p>

<p id="JVM%E7%94%B1%E4%B8%89%E9%83%A8%E5%88%86%E7%BB%84%E6%88%90-toc" style="margin-left:0px;"><a href="#JVM%E7%94%B1%E4%B8%89%E9%83%A8%E5%88%86%E7%BB%84%E6%88%90">JVM由三部分组成</a></p>

<p id="1.%20%E7%B1%BB%E5%8A%A0%E8%BD%BD%E5%AD%90%E7%B3%BB%E7%BB%9F%EF%BC%8C%E5%8F%AF%E4%BB%A5%E6%A0%B9%E6%8D%AE%E6%8C%87%E5%AE%9A%E7%9A%84%E5%85%A8%E9%99%90%E5%AE%9A%E5%90%8D%E6%9D%A5%E8%BD%BD%E5%85%A5%E7%B1%BB%E6%88%96%E6%8E%A5%E5%8F%A3%E3%80%82-toc" style="margin-left:40px;"><a href="#1.%20%E7%B1%BB%E5%8A%A0%E8%BD%BD%E5%AD%90%E7%B3%BB%E7%BB%9F%EF%BC%8C%E5%8F%AF%E4%BB%A5%E6%A0%B9%E6%8D%AE%E6%8C%87%E5%AE%9A%E7%9A%84%E5%85%A8%E9%99%90%E5%AE%9A%E5%90%8D%E6%9D%A5%E8%BD%BD%E5%85%A5%E7%B1%BB%E6%88%96%E6%8E%A5%E5%8F%A3%E3%80%82">1. 类加载子系统，可以根据指定的全限定名来载入类或接口。</a></p>

<p id="Java%E7%B1%BB%E5%8A%A0%E8%BD%BD%E6%9C%BA%E5%88%B6_trigger333%E7%9A%84%E5%8D%9A%E5%AE%A2-CSDN%E5%8D%9A%E5%AE%A2_java%E7%B1%BB%E5%8A%A0%E8%BD%BD%E7%9A%84%E6%9C%BA%E5%88%B6-toc" style="margin-left:80px;"><a href="#Java%E7%B1%BB%E5%8A%A0%E8%BD%BD%E6%9C%BA%E5%88%B6_trigger333%E7%9A%84%E5%8D%9A%E5%AE%A2-CSDN%E5%8D%9A%E5%AE%A2_java%E7%B1%BB%E5%8A%A0%E8%BD%BD%E7%9A%84%E6%9C%BA%E5%88%B6">Java类加载机制_trigger333的博客-CSDN博客_java类加载的机制</a></p>

<p id="2.%20%E6%89%A7%E8%A1%8C%E5%BC%95%E6%93%8E%EF%BC%8C%E8%B4%9F%E8%B4%A3%E6%89%A7%E8%A1%8C%E9%82%A3%E4%BA%9B%E5%8C%85%E5%90%AB%E5%9C%A8%E8%A2%AB%E8%BD%BD%E5%85%A5%E7%B1%BB%E7%9A%84%E6%96%B9%E6%B3%95%E4%B8%AD%E7%9A%84%E6%8C%87%E4%BB%A4%E3%80%82-toc" style="margin-left:40px;"><a href="#2.%20%E6%89%A7%E8%A1%8C%E5%BC%95%E6%93%8E%EF%BC%8C%E8%B4%9F%E8%B4%A3%E6%89%A7%E8%A1%8C%E9%82%A3%E4%BA%9B%E5%8C%85%E5%90%AB%E5%9C%A8%E8%A2%AB%E8%BD%BD%E5%85%A5%E7%B1%BB%E7%9A%84%E6%96%B9%E6%B3%95%E4%B8%AD%E7%9A%84%E6%8C%87%E4%BB%A4%E3%80%82">2. 执行引擎，负责执行那些包含在被载入类的方法中的指令。</a></p>

<p id="3.%20%E8%BF%90%E8%A1%8C%E6%97%B6%E6%95%B0%E6%8D%AE%E5%8C%BA.%C2%A0-toc" style="margin-left:40px;"><a href="#3.%20%E8%BF%90%E8%A1%8C%E6%97%B6%E6%95%B0%E6%8D%AE%E5%8C%BA.%C2%A0">3. 运行时数据区. </a></p>

<p id="JVM%E5%86%85%E5%AD%98%E7%BB%93%E6%9E%84_trigger333%E7%9A%84%E5%8D%9A%E5%AE%A2-CSDN%E5%8D%9A%E5%AE%A2_jvm%E5%86%85%E5%AD%98%E7%BB%93%E6%9E%84-toc" style="margin-left:80px;"><a href="#JVM%E5%86%85%E5%AD%98%E7%BB%93%E6%9E%84_trigger333%E7%9A%84%E5%8D%9A%E5%AE%A2-CSDN%E5%8D%9A%E5%AE%A2_jvm%E5%86%85%E5%AD%98%E7%BB%93%E6%9E%84">JVM内存结构_trigger333的博客-CSDN博客_jvm内存结构</a></p>

<p id="%E5%85%B7%E4%BD%93%E7%9A%84-toc" style="margin-left:0px;"><a href="#%E5%85%B7%E4%BD%93%E7%9A%84">具体的</a></p>

<hr id="hr-toc" /><p></p>

<h1 id="JVM%E7%94%B1%E4%B8%89%E9%83%A8%E5%88%86%E7%BB%84%E6%88%90">JVM由三部分组成</h1>

<p><strong>类加载子系统、执行引擎、运行时数据区。</strong></p>

<h2 id="1.%20%E7%B1%BB%E5%8A%A0%E8%BD%BD%E5%AD%90%E7%B3%BB%E7%BB%9F%EF%BC%8C%E5%8F%AF%E4%BB%A5%E6%A0%B9%E6%8D%AE%E6%8C%87%E5%AE%9A%E7%9A%84%E5%85%A8%E9%99%90%E5%AE%9A%E5%90%8D%E6%9D%A5%E8%BD%BD%E5%85%A5%E7%B1%BB%E6%88%96%E6%8E%A5%E5%8F%A3%E3%80%82">1. 类加载子系统，可以根据指定的全限定名来载入类或接口。</h2>

<h3 id="Java%E7%B1%BB%E5%8A%A0%E8%BD%BD%E6%9C%BA%E5%88%B6_trigger333%E7%9A%84%E5%8D%9A%E5%AE%A2-CSDN%E5%8D%9A%E5%AE%A2_java%E7%B1%BB%E5%8A%A0%E8%BD%BD%E7%9A%84%E6%9C%BA%E5%88%B6"><a data-link-desc="类加载器，双亲委派机制，类加载的三个阶段。目录类加载过程加载阶段链接阶段验证准备解析初始化类加载机制类加载器双亲委派机制类加载过程我们编写的java文件都是保存着业务逻辑代码。java编译器将 .java 文件编译成扩展名为 .class 的文件。.class 文件中保存着java转换后，虚拟机将要执行的指令。当需要某个类的时候，java虚拟机会加载 .class 文件，并创建对应的class对象，将class文件加载到虚拟机的内存，这个过程被称为类的" data-link-icon="https://g.csdnimg.cn/static/logo/favicon32.ico" data-link-title="Java类加载机制_trigger333的博客-CSDN博客_java类加载的机制" href="https://blog.csdn.net/weixin_40757930/article/details/123229959" title="Java类加载机制_trigger333的博客-CSDN博客_java类加载的机制">Java类加载机制_trigger333的博客-CSDN博客_java类加载的机制</a></h3>

<p><img alt="" height="502" src="https://img-blog.csdnimg.cn/48aecc6ae05f4a0a944347cc827ac43d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_9,color_FFFFFF,t_70,g_se,x_16" width="305" /></p>

<p> </p>

<h2 id="2.%20%E6%89%A7%E8%A1%8C%E5%BC%95%E6%93%8E%EF%BC%8C%E8%B4%9F%E8%B4%A3%E6%89%A7%E8%A1%8C%E9%82%A3%E4%BA%9B%E5%8C%85%E5%90%AB%E5%9C%A8%E8%A2%AB%E8%BD%BD%E5%85%A5%E7%B1%BB%E7%9A%84%E6%96%B9%E6%B3%95%E4%B8%AD%E7%9A%84%E6%8C%87%E4%BB%A4%E3%80%82">2. 执行引擎，负责执行那些包含在被载入类的方法中的指令。</h2>

<h2 id="3.%20%E8%BF%90%E8%A1%8C%E6%97%B6%E6%95%B0%E6%8D%AE%E5%8C%BA.%C2%A0">3. <span style="color:#fe2c24;"><strong>运行时数据区</strong></span>. </h2>

<p>当程序运行时，JVM需要内存来存储许多内容，例如：<strong>字节码、对象、参数、返回值、局部变量、运算的中间结果</strong>，等等，JVM会把这些东西都存储到运行时数据区中，以便于管理。而运行时数据区又可以分为方法区、堆、虚拟机栈、本地方法栈、程序计数器。</p>

<h3 id="JVM%E5%86%85%E5%AD%98%E7%BB%93%E6%9E%84_trigger333%E7%9A%84%E5%8D%9A%E5%AE%A2-CSDN%E5%8D%9A%E5%AE%A2_jvm%E5%86%85%E5%AD%98%E7%BB%93%E6%9E%84"><a data-link-desc="问题：OOM,GC问题怎么解决，JVM调优。JVM参数设置。java api 还是运行在jvm虚拟机上。内功就是jvm 相当于公式的推导过程。底层问题，高工资。架构师关心的两个问题：如何让我的系统更快？ 如何避免系统出现瓶颈。如何精进自己的技术?知乎上有条帖子：应该如何看招聘信息，直通年薪50万+?参与现有系统的性能优化，重构，保证平台性能和稳定性根据业务场景和需求，决定技术方向，做技术选型能够独立架构和设计海量数据下高并发分布式解决方案，满足功能和非功能需求解决..." data-link-icon="https://g.csdnimg.cn/static/logo/favicon32.ico" data-link-title="JVM内存结构_trigger333的博客-CSDN博客_jvm内存结构" href="https://blog.csdn.net/weixin_40757930/article/details/113520634" title="JVM内存结构_trigger333的博客-CSDN博客_jvm内存结构">JVM内存结构_trigger333的博客-CSDN博客_jvm内存结构</a></h3>

<p><img alt="" height="691" src="https://img-blog.csdnimg.cn/7291421df62646ad966088a66ec55f2b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_16,color_FFFFFF,t_70,g_se,x_16" width="545" /></p>

<p> </p>

<h1 id="%E5%85%B7%E4%BD%93%E7%9A%84">具体的</h1>

<p>运行时数据区是开发者重点要关注的部分，因为程序的运行与它密不可分，很多错误的排查也需要基于对运行时数据区的理解。在运行时数据区所包含的几块内存空间中，方法区和堆是线程之间共享的内存区域，而虚拟机栈、本地方法栈、程序计数器则是线程私有的区域，就是说每个线程都有自己的这个区域。</p>

<p class="img-center"><img alt="" height="540" src="https://img-blog.csdnimg.cn/img_convert/d0f219cf6d8a0faccd6d67b5b50cddba.png" width="512" /></p>

<p> </p>

<p><strong>虚拟机栈</strong>描述的是Java方法执行的<strong>线程内存模型</strong>：每个方法被执行的时候，Java虚拟机都会同步创建一个栈帧用于存储局部变量表、操作数栈、动态连接、方法出口等信息。</p>

<p>每一个方法被调用直至执行完毕的过程，就对应着一个栈帧在虚拟机栈中从入栈到出栈的过程。</p>

<p> 
</p><p><strong>方法区</strong>与堆一样，是各个线程共享的内存区域，它用于存储已被虚拟机加载的<strong>类型信息、常量、静态变量、即时编译器编译后的代码缓存</strong>等数据。虽然Java虚拟机规范中把方法区描述为堆的一个逻辑部分，但是它却有一个别名叫作“非堆”，目的是与堆区分开来。</p>


<p><strong>运行时常量池</strong>是方法区的一部分。Class文件中除了有类的版本、字段、方法、接口等描述信息外，还有一项信息是常量池表，用于存放编译期生成的各种字面量与符号引用。</p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="Stringtable 串池经典面试题_trigger333的博客-CSDN博客" href="https://blog.csdn.net/weixin_40757930/article/details/123225164" title="Stringtable 串池经典面试题_trigger333的博客-CSDN博客">Stringtable 串池经典面试题_trigger333的博客-CSDN博客</a></p>

<p></p>

<p></p>
