---
title: ThreadLocal 原理及例子
tags: java
categories: Java基础知识
abbrlink: 89ae2d65
date: 2022-03-27 09:43:05
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="%E9%80%9A%E4%BF%97%E8%A7%A3%E9%87%8A-toc" style="margin-left:0px;"><a href="#%E9%80%9A%E4%BF%97%E8%A7%A3%E9%87%8A">通俗解释</a></p>

<p id="%E4%B8%89%E5%A4%A7%E7%89%B9%E7%82%B9%EF%BC%9A-toc" style="margin-left:0px;"><a href="#%E4%B8%89%E5%A4%A7%E7%89%B9%E7%82%B9%EF%BC%9A">三大特点：</a></p>

<p id="%E7%BB%8F%E5%85%B8%E6%A1%88%E4%BE%8B%EF%BC%9A-toc" style="margin-left:0px;"><a href="#%E7%BB%8F%E5%85%B8%E6%A1%88%E4%BE%8B%EF%BC%9A">经典案例：</a></p>

<p id="%E4%BA%8C%E8%80%85%E5%8C%BA%E5%88%AB-toc" style="margin-left:40px;"><a href="#%E4%BA%8C%E8%80%85%E5%8C%BA%E5%88%AB">和synchronized的区别</a></p>

<p id="%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF-toc" style="margin-left:0px;"><a href="#%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF">应用场景</a></p>

<p id="ThreadLocal%20%E5%9B%BE%E7%A4%BA-toc" style="margin-left:0px;"><a href="#ThreadLocal%20%E5%9B%BE%E7%A4%BA">ThreadLocal 数据结构图示</a></p>

<p id="ThreadLocal%20%E7%9A%84%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F-toc" style="margin-left:0px;"><a href="#ThreadLocal%20%E7%9A%84%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F">ThreadLocal 的内存泄漏</a></p>

<p id="%E9%82%A3%E4%B9%88%E4%B8%BA%E4%BB%80%E4%B9%88key%E8%A6%81%E7%94%A8%E5%BC%B1%E5%BC%95%E7%94%A8%E5%91%A2%EF%BC%9F-toc" style="margin-left:40px;"><a href="#%E9%82%A3%E4%B9%88%E4%B8%BA%E4%BB%80%E4%B9%88key%E8%A6%81%E7%94%A8%E5%BC%B1%E5%BC%95%E7%94%A8%E5%91%A2%EF%BC%9F">那么为什么key要用弱引用呢？</a></p>

<p id="%E5%BC%B1%E5%BC%95%E7%94%A8-toc" style="margin-left:0px;"><a href="#%E5%BC%B1%E5%BC%95%E7%94%A8">弱引用</a></p>

<hr id="hr-toc" /><p></p>

<p><a class="has-card" data-link-desc="刚接触的学员可以直接看零基础java入门av80585971课程全面，包含：ThreadLocal基本介绍，运用场景，源码分析，常见面试问题等结合源码和画图解构ThreadLocal,更加形象源码分析不仅仅停留在表面,有源码为何这样设计的思考覆盖常见的面试问题: 如TheadLocal和synchronized关键字和内存泄漏方面都有深入的分析" data-link-icon="http://i1.hdslb.com/bfs/archive/8a7490c513de7f1e7917898f90da8bcf597f362c.jpg@57w_57h_1c.png" data-link-title="黑马程序员Java基础教程由浅入深全面解析threadlocal_哔哩哔哩_bilibili" href="https://www.bilibili.com/video/BV1N741127FH?p=10&amp;spm_id_from=pageDriver" title="黑马程序员Java基础教程由浅入深全面解析threadlocal_哔哩哔哩_bilibili"><span class="link-card-box"><span class="link-title">黑马程序员Java基础教程由浅入深全面解析threadlocal_哔哩哔哩_bilibili</span><span class="link-desc">刚接触的学员可以直接看零基础java入门av80585971课程全面，包含：ThreadLocal基本介绍，运用场景，源码分析，常见面试问题等结合源码和画图解构ThreadLocal,更加形象源码分析不仅仅停留在表面,有源码为何这样设计的思考覆盖常见的面试问题: 如TheadLocal和synchronized关键字和内存泄漏方面都有深入的分析</span><span class="link-link"><img alt="" class="link-link-icon" src="http://i1.hdslb.com/bfs/archive/8a7490c513de7f1e7917898f90da8bcf597f362c.jpg@57w_57h_1c.png" />https://www.bilibili.com/video/BV1N741127FH?p=10&amp;spm_id_from=pageDriver</span></span></a> <a class="has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="由浅入深，全面解析ThreadLocal_LeslieGuGu的博客-CSDN博客_threadlocal" href="https://blog.csdn.net/weixin_44050144/article/details/113061884" title="由浅入深，全面解析ThreadLocal_LeslieGuGu的博客-CSDN博客_threadlocal"><span class="link-card-box"><span class="link-title">由浅入深，全面解析ThreadLocal_LeslieGuGu的博客-CSDN博客_threadlocal</span><span class="link-link"><img alt="" class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" />https://blog.csdn.net/weixin_44050144/article/details/113061884</span></span></a></p>

<p></p>

<h1 id="%E9%80%9A%E4%BF%97%E8%A7%A3%E9%87%8A">通俗解释</h1>

<p>ThreadLocal 就是每一个线程自己维护的一个map中的key，</p>

<p>比如t1线程有自己的<code>ThreadLocalMap1，里面存放着 </code>ThreadLocal 1， value1</p>

<p>t2线程有自己的<code>ThreadLocalMap2，里面存放着 </code>ThreadLocal 2， value2.</p>

<p>在使用的时候 通过当前线程直接得到自己内部的map，再用键直接取出值即可。</p>

<p></p>

<h1 id="%E4%B8%89%E5%A4%A7%E7%89%B9%E7%82%B9%EF%BC%9A">三大特点：</h1>

<ol><li>线程并发: 在多线程并发的场景下</li>
	<li>传递数据: 我们可以通过ThreadLocal在同一线程，不同组件中传递公共变量</li>
	<li>线程隔离: 每个线程的变量都是独立的，不会互相影响，<strong>各是各的</strong></li>
</ol><p></p>

<h1 id="%E7%BB%8F%E5%85%B8%E6%A1%88%E4%BE%8B%EF%BC%9A">经典案例：</h1>

<p>下面这个例子中 如果不用threadlocal ，那么会出现 线程1设置了content，线程3把content取出来了，因为并发场景且没有加锁，<strong>content只有1份</strong>。 使用threadlocal ，设置值的时候，这个content相当于和这个线程关联上了，所以在取值的时候也和这个线程是对应的，所以其实这个content有5份，<strong>每个线程都有一份</strong>。</p>

<pre>
<code class="language-java">public class MyDemo1 {

    private static ThreadLocal&lt;String&gt; tl = new ThreadLocal&lt;&gt;();

    private String content;

    private String getContent() {
        return tl.get();
    }

    private void setContent(String content) {
         tl.set(content);
    }

    public static void main(String[] args) {
        MyDemo demo = new MyDemo();
        for (int i = 0; i &lt; 5; i++) {
            Thread thread = new Thread(new Runnable() {
                @Override
                public void run() {
                    demo.setContent(Thread.currentThread().getName() + "的数据");
                    System.out.println("-----------------------");
                    System.out.println(Thread.currentThread().getName() + "---&gt;" + demo.getContent());
                }
            });
            thread.setName("线程" + i);
            thread.start();
        }
    }
}
</code></pre>

<p>加锁是可以解决这个问题的，但是在这里我们强调的是线程<strong>数据隔离</strong>的问题，并不是多线程共享数据的问题, 在这个案例中使用synchronized关键字是不合适的。</p>

<h2 id="%E4%BA%8C%E8%80%85%E5%8C%BA%E5%88%AB">和synchronized的区别</h2>

<p><img alt="" height="215" src="https://img-blog.csdnimg.cn/ee529b718cb045a894c0cc13f0a6f56f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1193" /></p>

<p></p>

<h1 id="%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF">应用场景</h1>

<p>需要在项目中进行<strong>数据传递</strong>和<strong>线程隔离</strong>的场景，</p>

<p></p>

<p>在一些特定场景下，ThreadLocal方案有两个突出的优势：</p>

<ol><li>
	<p>传递数据 ： 保存每个线程绑定的数据，在需要的地方可以直接获取, 避免参数直接传递带来的代码耦合问题</p>
	</li>
	<li>
	<p>线程隔离 ： 各线程之间的数据相互隔离却又具备并发性，避免同步方式带来的性能损失</p>
	</li>
</ol><p></p>

<h1 id="ThreadLocal%20%E5%9B%BE%E7%A4%BA">ThreadLocal 数据结构图示</h1>

<p><img alt="" height="421" src="https://img-blog.csdnimg.cn/963dd7fb1b5443789a0a6869c790b587.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="900" /></p>

<p></p>

<p>​ 在ThreadLocalMap中，也是用Entry来保存K-V结构数据的。不过Entry中的key只能是ThreadLocal对象，这点在构造方法中已经限定死了。</p>

<p>​ 另外，Entry继承WeakReference，也就是key（ThreadLocal）是弱引用，其目的是将ThreadLocal对象的生命周期和线程生命周期解绑。</p>

<p></p>

<h1 id="ThreadLocal%20%E7%9A%84%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F"><strong>ThreadLocal 的内存泄漏</strong></h1>

<p><strong>ThreadLocal内存泄漏的根源是</strong>：由于ThreadLocalMap的生命周期跟Thread一样长，如果没有手动删除对应key就会导致内存泄漏。 </p>

<p><img alt="" height="556" src="https://img-blog.csdnimg.cn/cea79ac328d04cd29ff62549ab33c8ac.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="913" /></p>

<p></p>

<p> 同样假设在业务代码中使用完ThreadLocal ，栈区的threadLocal Ref被回收了。</p>

<p>​ 由于ThreadLocalMap只持有ThreadLocal的弱引用，没有任何强引用指向threadlocal实例, 所以threadlocal就可以顺利被gc回收，此时Entry中的key=null。</p>

<p>​ 但是在没有手动删除这个Entry以及CurrentThread依然运行的前提下，也存在有强引用链 threadRef-&gt;currentThread-&gt;threadLocalMap-&gt;entry -&gt; value ，<strong><span style="color:#fe2c24;">value不会被回收</span></strong>， 而这块value永远不会被访问到了，导致value内存泄漏。</p>

<p>​ 也就是说，ThreadLocalMap中的key使用了弱引用， 也有可能内存泄漏。<br />
 </p>

<h2 id="%E9%82%A3%E4%B9%88%E4%B8%BA%E4%BB%80%E4%B9%88key%E8%A6%81%E7%94%A8%E5%BC%B1%E5%BC%95%E7%94%A8%E5%91%A2%EF%BC%9F">那么为什么key要用弱引用呢？</h2>

<p>​ 事实上，在<strong> ThreadLocalMap </strong>中的  set/getEntry 方法中，会对key为null（也即是ThreadLocal为null）进行判断，如果为null的话，那么是会对value置为null的。</p>

<p>​ 这就意味着使用完ThreadLocal，CurrentThread依然运行的前提下，就算忘记调用remove方法，弱引用比强引用可以多一层保障：弱引用的ThreadLocal会被回收，对应的value在下一次ThreadLocalMap调用set,get,remove中的任一方法的时候会被清除，从而<strong>避免内存泄漏</strong>。<br />
 </p>

<p></p>

<h1 id="%E5%BC%B1%E5%BC%95%E7%94%A8">弱引用</h1>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="【Java虚拟机】弱引用引发的一些思考_Jungle_的博客-CSDN博客" href="https://blog.csdn.net/Jungle_/article/details/109133610" title="【Java虚拟机】弱引用引发的一些思考_Jungle_的博客-CSDN博客">【Java虚拟机】弱引用引发的一些思考_Jungle_的博客-CSDN博客</a></p>
