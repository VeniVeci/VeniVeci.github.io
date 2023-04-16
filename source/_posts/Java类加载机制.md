---
title: Java类加载机制
tags: java jar 开发语言
categories: Java基础知识 JVM
abbrlink: 6d27f500
date: 2022-03-02 14:59:43
---

<!--more-->

<p>类加载器，双亲委派机制，类加载的三个阶段。</p>

<p id="main-toc"><strong>目录</strong></p>

<p id="u87693a76-toc" style="margin-left:0px;"><a href="#u87693a76">类加载过程</a></p>

<p id="u352b23d7-toc" style="margin-left:40px;"><a href="#u352b23d7">加载阶段</a></p>

<p id="u351332fb-toc" style="margin-left:40px;"><a href="#u351332fb">链接阶段</a></p>

<p id="%E9%AA%8C%E8%AF%81-toc" style="margin-left:80px;"><a href="#%E9%AA%8C%E8%AF%81">验证</a></p>

<p id="u71793c4e-toc" style="margin-left:80px;"><a href="#u71793c4e">准备</a></p>

<p id="u9b3834e7-toc" style="margin-left:80px;"><a href="#u9b3834e7">解析</a></p>

<p id="ue483fb52-toc" style="margin-left:40px;"><a href="#ue483fb52">初始化</a></p>

<p id="%E7%B1%BB%E5%8A%A0%E8%BD%BD%E6%9C%BA%E5%88%B6-toc" style="margin-left:0px;"><a href="#%E7%B1%BB%E5%8A%A0%E8%BD%BD%E6%9C%BA%E5%88%B6">类加载机制</a></p>

<p id="%E7%B1%BB%E5%8A%A0%E8%BD%BD%E5%99%A8-toc" style="margin-left:40px;"><a href="#%E7%B1%BB%E5%8A%A0%E8%BD%BD%E5%99%A8">类加载器</a></p>

<p id="%E5%8F%8C%E4%BA%B2%E5%A7%94%E6%B4%BE%E6%9C%BA%E5%88%B6-toc" style="margin-left:40px;"><a href="#%E5%8F%8C%E4%BA%B2%E5%A7%94%E6%B4%BE%E6%9C%BA%E5%88%B6">双亲委派机制</a></p>

<hr id="hr-toc" /><p></p>

<h1 id="u87693a76">类加载过程</h1>

<p id="u9634c197">我们编写的java文件都是保存着业务逻辑代码。java编译器将 .java 文件编译成扩展名为 .class 的文件。.class 文件中保存着java转换后，虚拟机将要执行的指令。当需要某个类的时候，java虚拟机会加载 .class 文件，并创建对应的class对象，将class文件加载到虚拟机的内存，这个过程被称为类的加载。</p>

<h2 id="u352b23d7"><strong>加载阶段</strong></h2>

<p id="udde2d2c9">类加载过程的一个阶段，ClassLoader通过一个类的完全限定名查找此类字节码文件，并利用字节码文件创建一个class对象。</p>

<p id="u2734734a"></p>

<h2 id="u351332fb"><strong>链接阶段 </strong></h2>

<h3 id="%E9%AA%8C%E8%AF%81">验证</h3>

<p id="u423d39fb">目的在于确保class文件的字节流中包含信息符合当前虚拟机要求，不会危害虚拟机自身的安全，主要包括四种验证：文件格式的验证，元数据的验证，字节码验证，符号引用验证。</p>

<h3 id="u71793c4e">准备</h3>

<p id="u0471ee14">为类变量（static修饰的字段变量）分配内存并且设置该类变量的初始值，（如static int i = 5 这里只是将 i 赋值为0，在初始化的阶段再把 i 赋值为5)，这里不包含final修饰的static ，因为final在<strong>编译的时候就已经分配了</strong>。这里不会为实例变量分配初始化，类变量会分配在方法区中，实例变量会随着对象分配到Java堆中。</p>

<h3 id="u9b3834e7">解析</h3>

<p id="u8e2e06b8">这里主要的任务是把<strong>常量池中的符号引用替换成直接引用</strong></p>

<p></p>

<h2 id="ue483fb52"><strong>初始化</strong></h2>

<p id="u2688b45e">这里是类记载的最后阶段，<strong>如果该类具有父类就进行对父类进行初始化，执行其静态初始化器（静态代码块）和静态初始化成员变量</strong>。（前面已经对static 初始化了默认值，这里我们对它进行赋值，成员变量也将被初始化）</p>

<p>详情见：</p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="Java中对象属性的初始化顺序_超越Gatsby的博客-CSDN博客_java属性的初始化顺序" href="https://blog.csdn.net/weixin_45436474/article/details/117234230" title="Java中对象属性的初始化顺序_超越Gatsby的博客-CSDN博客_java属性的初始化顺序">Java中对象属性的初始化顺序_超越Gatsby的博客-CSDN博客_java属性的初始化顺序</a></p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="Java属性的初始化顺序（代码验证）_trigger的博客-CSDN博客" href="https://blog.csdn.net/weixin_40757930/article/details/123229211" title="Java属性的初始化顺序（代码验证）_trigger的博客-CSDN博客">Java属性的初始化顺序（代码验证）_trigger的博客-CSDN博客</a></p>

<h1 id="%E7%B1%BB%E5%8A%A0%E8%BD%BD%E6%9C%BA%E5%88%B6">类加载机制</h1>

<h2 id="%E7%B1%BB%E5%8A%A0%E8%BD%BD%E5%99%A8">类加载器</h2>

<p>类加载器自下而上分为</p>

<p>应用程序加载器AppClassLoader：主要负责加载应用程序的主函数类（Person类等），</p>

<p>扩展类加载器ExtClassLoader：主要负责加载jre/lib/ext目录下的一些扩展的jar，</p>

<p>启动类加载器Bootstrap classLoader:主要负责加载核心的类库(java.lang.*等)，构造ExtClassLoader和APPClassLoader。</p>

<p></p>

<h2 id="%E5%8F%8C%E4%BA%B2%E5%A7%94%E6%B4%BE%E6%9C%BA%E5%88%B6"><strong>双亲委派机制</strong></h2>

<p><img alt="" height="386" src="https://img-blog.csdnimg.cn/e4555717019a47f9996a404c6e72e61c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1189" /></p>

<p></p>

<p></p>

<p>当.class文件被加载时 ，jvm会调用依次app.ex.boot三个加载器检查是否已经加载该类，直到启动类加载器，然后开始加载该类，如果在boot找不到，那就由子加载器加载。</p>

<p>一层一层往下加载，如果都找不到 抛出异常classnotfound 。</p>

<p></p>

<p>侵删~</p>

<p></p>
