---
title: Spring5之IOC详解
tags: spring java bean ioc
categories: Java基础知识 Spring框架
abbrlink: 912f989
date: 2021-03-18 21:53:06
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="%E7%AE%80%E5%8D%95%E4%BB%8B%E7%BB%8D-toc" style="margin-left:0px;"><a href="#%E7%AE%80%E5%8D%95%E4%BB%8B%E7%BB%8D">简单介绍</a></p>

<p id="%E5%B0%8F%E7%9F%A5%E8%AF%86%EF%BC%9A%E6%80%8E%E4%B9%88%E8%A7%A3%E8%80%A6%E5%91%A2%EF%BC%8C%E5%8F%AF%E4%BB%A5%E5%88%A9%E7%94%A8%E5%B7%A5%E5%8E%82%E6%9D%A5%E9%99%8D%E4%BD%8E%E8%80%A6%E5%90%88%E5%BA%A6%E3%80%82-toc" style="margin-left:40px;"><a href="#%E5%B0%8F%E7%9F%A5%E8%AF%86%EF%BC%9A%E6%80%8E%E4%B9%88%E8%A7%A3%E8%80%A6%E5%91%A2%EF%BC%8C%E5%8F%AF%E4%BB%A5%E5%88%A9%E7%94%A8%E5%B7%A5%E5%8E%82%E6%9D%A5%E9%99%8D%E4%BD%8E%E8%80%A6%E5%90%88%E5%BA%A6%E3%80%82">小知识：怎么解耦呢，可以利用工厂来降低耦合度。</a></p>

<p id="%E5%BA%95%E5%B1%82%E5%8E%9F%E7%90%86-toc" style="margin-left:0px;"><a href="#%E5%BA%95%E5%B1%82%E5%8E%9F%E7%90%86">底层原理</a></p>

<p id="%E7%A4%BA%E4%BE%8B%EF%BC%88%E5%88%9B%E5%BB%BA%E5%AF%B9%E8%B1%A1%E5%92%8C%E5%B1%9E%E6%80%A7%E6%B3%A8%E5%85%A5%EF%BC%89%E5%9F%BA%E4%BA%8Exml%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9A%84%E6%96%B9%E5%BC%8F-toc" style="margin-left:0px;"><a href="#%E7%A4%BA%E4%BE%8B%EF%BC%88%E5%88%9B%E5%BB%BA%E5%AF%B9%E8%B1%A1%E5%92%8C%E5%B1%9E%E6%80%A7%E6%B3%A8%E5%85%A5%EF%BC%89%E5%9F%BA%E4%BA%8Exml%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9A%84%E6%96%B9%E5%BC%8F">基于xml配置文件的方式</a></p>

<p id="%E5%9C%A8xml%E6%96%87%E4%BB%B6%E4%B8%AD%E5%88%A9%E7%94%A8bean%E6%A0%87%E7%AD%BE%E8%BF%9B%E8%A1%8Cuser%E5%AF%B9%E8%B1%A1%E5%88%9B%E5%BB%BA%EF%BC%8C%E5%88%A9%E7%94%A8p%E6%A0%87%E7%AD%BE%E8%BF%9B%E8%A1%8C%E5%B1%9E%E6%80%A7%E7%9A%84%E6%B3%A8%E5%85%A5%E3%80%82-toc" style="margin-left:40px;"><a href="#%E5%9C%A8xml%E6%96%87%E4%BB%B6%E4%B8%AD%E5%88%A9%E7%94%A8bean%E6%A0%87%E7%AD%BE%E8%BF%9B%E8%A1%8Cuser%E5%AF%B9%E8%B1%A1%E5%88%9B%E5%BB%BA%EF%BC%8C%E5%88%A9%E7%94%A8p%E6%A0%87%E7%AD%BE%E8%BF%9B%E8%A1%8C%E5%B1%9E%E6%80%A7%E7%9A%84%E6%B3%A8%E5%85%A5%E3%80%82">在xml文件中利用bean标签进行user对象创建，利用p标签进行属性的注入。</a></p>

<p id="%E8%A6%81%E6%83%B3%E6%B3%A8%E5%85%A5%E5%A4%96%E9%83%A8Bean%E6%80%8E%E4%B9%88%E5%8A%9E%E5%91%A2%EF%BC%88%E4%B9%9F%E5%B0%B1%E6%98%AF%E7%BB%99%E8%BF%99%E4%B8%AA%E5%AF%B9%E8%B1%A1%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E6%97%B6%E5%80%99%E6%B3%A8%E5%85%A5%E5%8F%A6%E4%B8%80%E4%B8%AA%E5%AF%B9%E8%B1%A1%EF%BC%89%E7%94%A8ref%E6%A0%87%E7%AD%BE%E5%B0%B1%E5%8F%AF%E4%BB%A5-toc" style="margin-left:40px;"><a href="#%E8%A6%81%E6%83%B3%E6%B3%A8%E5%85%A5%E5%A4%96%E9%83%A8Bean%E6%80%8E%E4%B9%88%E5%8A%9E%E5%91%A2%EF%BC%88%E4%B9%9F%E5%B0%B1%E6%98%AF%E7%BB%99%E8%BF%99%E4%B8%AA%E5%AF%B9%E8%B1%A1%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E6%97%B6%E5%80%99%E6%B3%A8%E5%85%A5%E5%8F%A6%E4%B8%80%E4%B8%AA%E5%AF%B9%E8%B1%A1%EF%BC%89%E7%94%A8ref%E6%A0%87%E7%AD%BE%E5%B0%B1%E5%8F%AF%E4%BB%A5">要想注入外部Bean怎么办呢（也就是给这个对象初始化的时候注入另一个对象）用ref标签就可以</a></p>

<p id="%E5%86%85%E9%83%A8Bean%E5%92%8C%E7%BA%A7%E8%81%94%E8%B5%8B%E5%80%BC-toc" style="margin-left:40px;"><a href="#%E5%86%85%E9%83%A8Bean%E5%92%8C%E7%BA%A7%E8%81%94%E8%B5%8B%E5%80%BC">内部Bean和级联赋值</a></p>

<p id="%E5%9F%BA%E4%BA%8E%E6%B3%A8%E8%A7%A3%E6%96%B9%E5%BC%8F-toc" style="margin-left:0px;"><a href="#%E5%9F%BA%E4%BA%8E%E6%B3%A8%E8%A7%A3%E6%96%B9%E5%BC%8F">基于注解的方式</a></p>

<p id="%E5%AE%9E%E7%8E%B0%E6%B3%A8%E8%A7%A3%E7%9A%84%E5%B1%9E%E6%80%A7%E6%B3%A8%E5%85%A5%EF%BC%8C%E5%8F%AF%E4%BB%A5%E4%BD%BF%E7%94%A8-toc" style="margin-left:40px;"><a href="#%E5%AE%9E%E7%8E%B0%E6%B3%A8%E8%A7%A3%E7%9A%84%E5%B1%9E%E6%80%A7%E6%B3%A8%E5%85%A5%EF%BC%8C%E5%8F%AF%E4%BB%A5%E4%BD%BF%E7%94%A8">实现注解的属性注入，可以使用</a></p>

<p id="%E5%AE%8C%E5%85%A8%E6%B3%A8%E8%A7%A3%E5%BC%80%E5%8F%91-toc" style="margin-left:40px;"><a href="#%E5%AE%8C%E5%85%A8%E6%B3%A8%E8%A7%A3%E5%BC%80%E5%8F%91">完全注解开发</a></p>

<hr id="hr-toc" /><h1 id="%E7%AE%80%E5%8D%95%E4%BB%8B%E7%BB%8D">简单介绍</h1>

<p>IOC，英文名是inversion of control</p>

<p>其中最常见的方式叫做<strong><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="依赖注入" href="https://baike.baidu.com/item/%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5/5177233" title="依赖注入">依赖注入</a></strong>（Dependency Injection，简称<strong>DI</strong>），还有一种方式叫“依赖查找”（Dependency Lookup）。</p>

<p><strong>就是创建对象的时候，直接给对象赋初值，不用我们去set。</strong></p>

<p>使用IOC的目的就是为了降低耦合度</p>

<h2 id="%E5%B0%8F%E7%9F%A5%E8%AF%86%EF%BC%9A%E6%80%8E%E4%B9%88%E8%A7%A3%E8%80%A6%E5%91%A2%EF%BC%8C%E5%8F%AF%E4%BB%A5%E5%88%A9%E7%94%A8%E5%B7%A5%E5%8E%82%E6%9D%A5%E9%99%8D%E4%BD%8E%E8%80%A6%E5%90%88%E5%BA%A6%E3%80%82">小知识：怎么解耦呢，可以利用工厂来降低耦合度。</h2>

<p><img alt="" height="311" src="https://img-blog.csdnimg.cn/20210317215144107.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1043" /></p>

<p></p>

<h1 id="%E5%BA%95%E5%B1%82%E5%8E%9F%E7%90%86">底层原理</h1>

<p><strong>xml解析+工厂模式+反射</strong></p>

<p></p>

<h1 id="%E7%A4%BA%E4%BE%8B%EF%BC%88%E5%88%9B%E5%BB%BA%E5%AF%B9%E8%B1%A1%E5%92%8C%E5%B1%9E%E6%80%A7%E6%B3%A8%E5%85%A5%EF%BC%89%E5%9F%BA%E4%BA%8Exml%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9A%84%E6%96%B9%E5%BC%8F">基于xml配置文件的方式</h1>

<h2 id="%E5%9C%A8xml%E6%96%87%E4%BB%B6%E4%B8%AD%E5%88%A9%E7%94%A8bean%E6%A0%87%E7%AD%BE%E8%BF%9B%E8%A1%8Cuser%E5%AF%B9%E8%B1%A1%E5%88%9B%E5%BB%BA%EF%BC%8C%E5%88%A9%E7%94%A8p%E6%A0%87%E7%AD%BE%E8%BF%9B%E8%A1%8C%E5%B1%9E%E6%80%A7%E7%9A%84%E6%B3%A8%E5%85%A5%E3%80%82">在xml文件中利用bean标签进行user对象创建，利用p标签进行属性的注入。</h2>

<blockquote>
<pre>
<code class="language-html">&lt;!--1 配置User对象创建--&gt;
&lt;bean id="user" class="com.atguigu.spring5.User" p:userName="kiki"&gt;&lt;/bean&gt;</code></pre>
</blockquote>

<h2 id="%E8%A6%81%E6%83%B3%E6%B3%A8%E5%85%A5%E5%A4%96%E9%83%A8Bean%E6%80%8E%E4%B9%88%E5%8A%9E%E5%91%A2%EF%BC%88%E4%B9%9F%E5%B0%B1%E6%98%AF%E7%BB%99%E8%BF%99%E4%B8%AA%E5%AF%B9%E8%B1%A1%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E6%97%B6%E5%80%99%E6%B3%A8%E5%85%A5%E5%8F%A6%E4%B8%80%E4%B8%AA%E5%AF%B9%E8%B1%A1%EF%BC%89%E7%94%A8ref%E6%A0%87%E7%AD%BE%E5%B0%B1%E5%8F%AF%E4%BB%A5">要想注入外部Bean怎么办呢（也就是给这个对象初始化的时候注入另一个对象）用ref标签就可以</h2>

<blockquote>
<pre>
<code class="language-html">&lt;!--1 service和dao对象创建--&gt;
&lt;bean id="userService" class="com.atguigu.spring5.service.UserService"&gt;
    &lt;!--注入userDao对象
        name属性：类里面属性名称
        ref属性：创建userDao对象bean标签id值
    --&gt;
    &lt;property name="userDao" ref="userDaoImpl"&gt;&lt;/property&gt;
&lt;/bean&gt;

&lt;bean id="userDaoImpl" class="com.atguigu.spring5.dao.UserDaoImpl"&gt;&lt;/bean&gt;</code></pre>
</blockquote>

<h2 id="%E5%86%85%E9%83%A8Bean%E5%92%8C%E7%BA%A7%E8%81%94%E8%B5%8B%E5%80%BC">内部Bean和级联赋值</h2>

<blockquote>
<pre>
<code class="language-html">&lt;!--内部bean--&gt;
&lt;bean id="emp" class="com.atguigu.spring5.bean.Emp"&gt;
    &lt;!--设置两个普通属性--&gt;
    &lt;property name="ename" value="lucy"&gt;&lt;/property&gt;
    &lt;property name="gender" value="女"&gt;&lt;/property&gt;
    &lt;!--设置对象类型属性--&gt;
内部bean
    &lt;property name="dept"&gt;
        &lt;bean id="dept" class="com.atguigu.spring5.bean.Dept"&gt;
            &lt;property name="dname" value="安保部"&gt;&lt;/property&gt;
        &lt;/bean&gt;
    &lt;/property&gt;
&lt;/bean&gt;</code></pre>
</blockquote>

<p>Bean在创建时默认是单实例，也就是多次通过xml获得对象，只能获得一个对象。</p>

<p>如果想要获取多实例，就需要在bean标签中设置。</p>

<p><img alt="" height="625" src="https://img-blog.csdnimg.cn/20210318204923407.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1068" /></p>

<p>bean有两种，一种是我们自己用bean标签做的，这种bean在获取对象时和它申明的类型是一致的，也就是说我bean标签中设置了什么类，那就会获得什么类。</p>

<p>还有一种是factorybean，工厂bean，可以调用它可以获得任意类型的对象。</p>

<p><img alt="" height="626" src="https://img-blog.csdnimg.cn/20210318205323272.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="661" /></p>

<p></p>

<h1 id="%E5%9F%BA%E4%BA%8E%E6%B3%A8%E8%A7%A3%E6%96%B9%E5%BC%8F">基于注解的方式</h1>

<p>注解的方式更加简便。</p>

<p>首先需要在bean.xml中配置下面这段话，表示用注解的时候去哪些地方搜索</p>

<blockquote>
<pre>
&lt;context:component-scan base-package="包名"&gt;&lt;/context:component-scan&gt;</pre>
</blockquote>

<p><img alt="" height="487" src="https://img-blog.csdnimg.cn/20210318213726632.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1200" /></p>

<p><img alt="" height="274" src="https://img-blog.csdnimg.cn/20210318213801881.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1165" /></p>

<h2 id="%E5%AE%9E%E7%8E%B0%E6%B3%A8%E8%A7%A3%E7%9A%84%E5%B1%9E%E6%80%A7%E6%B3%A8%E5%85%A5%EF%BC%8C%E5%8F%AF%E4%BB%A5%E4%BD%BF%E7%94%A8">实现注解的属性注入，可以使用</h2>

<p>注入普通类型的：用@value</p>

<p>注入对象：</p>

<blockquote>
<pre>
@Autowired //byType 
@Qualifier(value = "userDaoImpl") //byName 
@Resource //byType or Name
</pre>
</blockquote>

<p>注意 尽量使用上面两种，因为它们属于spring框架中的，而Resource 不属于。 </p>

<blockquote>
<pre>
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import javax.annotation.Resource;</pre>
</blockquote>

<h2 id="%E5%AE%8C%E5%85%A8%E6%B3%A8%E8%A7%A3%E5%BC%80%E5%8F%91">完全注解开发</h2>

<p>利用下面这两个注解实现之前xml配置文件一样的功能。</p>

<p>实际开发中一般直接用springboot。</p>

<blockquote>
<pre>
@Configuration //作为配置类，替代xml配置文件 
@ComponentScan(basePackages = {"com.atguigu"}) 
</pre>
</blockquote>

<p><img alt="" height="674" src="https://img-blog.csdnimg.cn/20210318214611213.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1176" /></p>

<p>资料来源：B站尚硅谷</p>

<p></p>

<p></p>
