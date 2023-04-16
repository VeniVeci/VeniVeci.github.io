---
title: Spring 循环依赖 以及解决方式
tags: spring 循环依赖
categories: Spring框架
abbrlink: 2950035f
date: 2022-03-28 20:50:19
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="%E4%BB%80%E4%B9%88%E6%98%AF%E5%BE%AA%E7%8E%AF%E4%BE%9D%E8%B5%96-toc" style="margin-left:0px;"><a href="#%E4%BB%80%E4%B9%88%E6%98%AF%E5%BE%AA%E7%8E%AF%E4%BE%9D%E8%B5%96">什么是循环依赖</a></p>

<p id="%E5%A6%82%E4%BD%95%E8%A7%A3%E5%86%B3%E5%BE%AA%E7%8E%AF%E4%BE%9D%E8%B5%96-toc" style="margin-left:0px;"><a href="#%E5%A6%82%E4%BD%95%E8%A7%A3%E5%86%B3%E5%BE%AA%E7%8E%AF%E4%BE%9D%E8%B5%96">如何解决循环依赖</a></p>

<p id="%E4%B8%A4%E5%B1%82%E5%8F%AF%E4%B8%8D%E5%8F%AF%E4%BB%A5%EF%BC%9F-toc" style="margin-left:40px;"><a href="#%E4%B8%A4%E5%B1%82%E5%8F%AF%E4%B8%8D%E5%8F%AF%E4%BB%A5%EF%BC%9F">两层可不可以？</a></p>

<hr id="hr-toc" /><p><a data-link-desc="1.循环依赖首先我们要知道什么是Spring的循环依赖问题。假如我们有两个bean，A 和 B。他们的代码简单如下：@Beanpublic class A {    @Autowire    private B b;}@Beanpublic class B {    @Autowire    private A a;}也就是需要在A中注入B，在B中注入A，那么Spring在创建A的时候会出现这种现象：创建A实例后，在依赖注入时需要B，然后就去创建B，这时候发现又需要..." data-link-icon="https://g.csdnimg.cn/static/logo/favicon32.ico" data-link-title="Spring 是如何解决循环依赖问题的_从菜鸟到放弃的博客-CSDN博客_spring 循环依赖怎么解决" href="https://blog.csdn.net/linzherong/article/details/116094843" title="Spring 是如何解决循环依赖问题的_从菜鸟到放弃的博客-CSDN博客_spring 循环依赖怎么解决">Spring 是如何解决循环依赖问题的_从菜鸟到放弃的博客-CSDN博客_spring 循环依赖怎么解决</a></p>

<h1 id="%E4%BB%80%E4%B9%88%E6%98%AF%E5%BE%AA%E7%8E%AF%E4%BE%9D%E8%B5%96">什么是循环依赖</h1>

<p>多个bean之间相互依赖，形成了一个闭环。 比如:A依赖于B、B依赖于c、c依赖于A</p>

<p>通常来说，如果问spring容器内部如何解决循环依赖， 一定是指默认的单例Bean中，属性互相引用的场景。也就是说，Spring的循环依赖，是Spring容器注入时候出现的问题。</p>

<p>比如</p>

<pre>
<code class="language-java">@Bean
public class A {
    @Autowire
    private B b;
}
 
 
@Bean
public class B {
    @Autowire
    private A a;
}</code></pre>

<h1 id="%E5%A6%82%E4%BD%95%E8%A7%A3%E5%86%B3%E5%BE%AA%E7%8E%AF%E4%BE%9D%E8%B5%96">如何解决循环依赖</h1>

<p>Spring对循环依赖的解决方法可以概括为 <strong>用三级缓存方式达到Bean提前曝光的目的</strong></p>

<p>A创建过程中需要B，于是A将自己放到三级缓存里面，去实例化B实例化的时候发现需要A，于是B先查一级缓存，没有，再查二级缓存，还是没有，再查三级缓存，找到了A然后把三级缓存里面的这个A放到二级缓存里面，并删除三级缓存里面的A，B顺利初始化完毕，将自己放到一级缓存里面（此时B里面的A依然是创建中状态）然后回来接着创建A，此时B已经创建结束，直接从一级缓存里面拿到B，然后完成创建，并将A放到一级缓存中。</p>

<p><img alt="" height="309" src="https://img-blog.csdnimg.cn/408ac0b010404a8e82189e08081d7efb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="720" /></p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="Spring 是如何解决循环依赖的？ - 知乎" href="https://www.zhihu.com/question/438247718/answer/1730527725" title="Spring 是如何解决循环依赖的？ - 知乎">Spring 是如何解决循环依赖的？ - 知乎</a></p>

<p>spring内部有三级缓存：</p>

<ul><li>singletonObjects 一级缓存，用于保存实例化、注入、初始化完成的bean实例</li>
	<li>earlySingletonObjects 二级缓存，<strong>用于保存实例化完成的bean实例</strong></li>
	<li>singletonFactories 三级缓存，用于保存bean创建工厂，以便于后面扩展有机会创建代理对象。</li>
</ul><p></p>

<h2 id="%E4%B8%A4%E5%B1%82%E5%8F%AF%E4%B8%8D%E5%8F%AF%E4%BB%A5%EF%BC%9F">两层可不可以？</h2>

<p><strong>二级缓存不能保证多个依赖中实例化对象的唯一性。</strong></p>

<p>如果是这种情况：TestService1依赖于TestService2和TestService3，而TestService2依赖于TestService1，同时TestService3也依赖于TestService1。</p>

<p>按照上图的流程可以把TestService1注入到TestService2，并且TestService1的实例是从第三级缓存中获取的。</p>

<p>假设不用第二级缓存，TestService1注入到TestService3又需要从第三级缓存中获取实例，而第三级缓存里保存的并非真正的实例对象，而是<code>ObjectFactory</code>对象。说白了，两次从三级缓存中获取都是<code>ObjectFactory</code>对象，<span style="color:#fe2c24;"><strong>而通过它创建的实例对象每次可能都不一样的。</strong></span></p>

<p>为了解决这个问题，spring引入的第二级缓存。上面图1其实TestService1对象的实例已经被添加到第二级缓存中了，而在TestService1注入到TestService3时，只用从第二级缓存中获取该对象即可。</p>

<p><br />
 </p>

<p><br />
 </p>
