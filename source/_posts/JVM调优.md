---
title: JVM调优
tags: java
categories: JVM
abbrlink: 1653795f
date: 2022-03-27 22:10:36
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="JVM%E5%90%AF%E5%8A%A8%E5%8F%82%E6%95%B0-toc" style="margin-left:0px;"><a href="#JVM%E5%90%AF%E5%8A%A8%E5%8F%82%E6%95%B0">JVM启动参数</a></p>

<p id="%C2%A0%E5%AE%9E%E9%99%85%E4%BE%8B%E5%AD%90-toc" style="margin-left:40px;"><a href="#%C2%A0%E5%AE%9E%E9%99%85%E4%BE%8B%E5%AD%90"> 实际例子</a></p>

<p id="%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86-toc" style="margin-left:0px;"><a href="#%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86">基础知识</a></p>

<p id="%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E5%99%A8-toc" style="margin-left:0px;"><a href="#%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E5%99%A8">垃圾回收器</a></p>

<p id="Serial%20%2B%20Serial%20Old%E4%B8%B2%E8%A1%8C-toc" style="margin-left:40px;"><a href="#Serial%20%2B%20Serial%20Old%E4%B8%B2%E8%A1%8C">Serial + Serial Old串行</a></p>

<p id="Parallel%20%2B%20Parallel%20Old%E5%90%9E%E5%90%90%E9%87%8F%E4%BC%98%E5%85%88-toc" style="margin-left:40px;"><a href="#Parallel%20%2B%20Parallel%20Old%E5%90%9E%E5%90%90%E9%87%8F%E4%BC%98%E5%85%88">Parallel + Parallel Old吞吐量优先</a></p>

<p id="CMS%E5%93%8D%E5%BA%94%E6%97%B6%E9%97%B4%E4%BC%98%E5%85%88-toc" style="margin-left:40px;"><a href="#CMS%E5%93%8D%E5%BA%94%E6%97%B6%E9%97%B4%E4%BC%98%E5%85%88">CMS响应时间优先</a></p>

<p id="CMS%E5%9B%9E%E6%94%B6%E8%BF%87%E7%A8%8B%E5%8F%AF%E4%BB%A5%E5%88%86%E4%B8%BA%E5%93%AA%E4%BA%9B%E6%AD%A5%E9%AA%A4%EF%BC%9F-toc" style="margin-left:80px;"><a href="#CMS%E5%9B%9E%E6%94%B6%E8%BF%87%E7%A8%8B%E5%8F%AF%E4%BB%A5%E5%88%86%E4%B8%BA%E5%93%AA%E4%BA%9B%E6%AD%A5%E9%AA%A4%EF%BC%9F">CMS回收过程可以分为哪些步骤？</a></p>

<p id="%E2%80%8BCMS%E7%9A%84%E9%97%AE%E9%A2%98%C2%A0-toc" style="margin-left:80px;"><a href="#%E2%80%8BCMS%E7%9A%84%E9%97%AE%E9%A2%98%C2%A0">​CMS的问题 </a></p>

<p id="%E8%B0%83%E4%BC%98%E6%AD%A5%E9%AA%A4-toc" style="margin-left:0px;"><a href="#%E8%B0%83%E4%BC%98%E6%AD%A5%E9%AA%A4">调优步骤</a></p>

<p id="wlOBf-toc" style="margin-left:0px;"><a href="#wlOBf">新生代调优</a></p>

<p id="%E6%96%B0%E7%94%9F%E4%BB%A3%E6%AF%94%E4%BE%8B%E8%B6%8A%E5%A4%A7%E8%B6%8A%E5%A5%BD%E5%90%97-toc" style="margin-left:40px;"><a href="#%E6%96%B0%E7%94%9F%E4%BB%A3%E6%AF%94%E4%BE%8B%E8%B6%8A%E5%A4%A7%E8%B6%8A%E5%A5%BD%E5%90%97">新生代比例越大越好吗</a></p>

<p id="%E8%80%81%E5%B9%B4%E4%BB%A3%E8%B0%83%E4%BC%98-toc" style="margin-left:0px;"><a href="#%E8%80%81%E5%B9%B4%E4%BB%A3%E8%B0%83%E4%BC%98">老年代调优</a></p>

<hr id="hr-toc" /><p></p>

<p></p>

<p> </p>

<h1 id="JVM%E5%90%AF%E5%8A%A8%E5%8F%82%E6%95%B0">JVM启动参数</h1>

<p><img alt="" height="645" src="https://img-blog.csdnimg.cn/848e21cc04f64683892519de423a235e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="962" /></p>

<h2 id="%C2%A0%E5%AE%9E%E9%99%85%E4%BE%8B%E5%AD%90"> 实际例子</h2>

<pre>
<code>
import java.util.ArrayList;

/**
 *  演示内存的分配策略
 */
public class Demo2_1 {
    private static final int _512KB = 512 * 1024;
    private static final int _1MB = 1024 * 1024;
    private static final int _6MB = 6 * 1024 * 1024;
    private static final int _7MB = 7 * 1024 * 1024;
    private static final int _8MB = 8 * 1024 * 1024;

    // -Xms20M -Xmx20M -Xmn10M -XX:+UseSerialGC -XX:+PrintGCDetails -verbose:gc -XX:-ScavengeBeforeFullGC

    public static void main(String[] args) throws InterruptedException {
        ArrayList&lt;byte[]&gt; list = new ArrayList&lt;&gt;();
        list.add(new byte[_8MB]);
    }

}
</code></pre>

<p>VM options：</p>

<blockquote>
<p>-Xms20M -Xmx20M -Xmn10M -XX:+UseSerialGC -XX:+PrintGCDetails -verbose:gc -XX:-ScavengeBeforeFullGC</p>
</blockquote>

<p>堆内存初始值 20 M</p>

<p>堆内存最大值 20M</p>

<p>新生代大小 10M</p>

<p>老年代  10M</p>

<p>使用UseSerialGC 串行的回收器</p>

<p>PrintGCDetails 输出GC 的消息</p>

<p><img alt="" height="437" src="https://img-blog.csdnimg.cn/7ae11f26f41844b8891f0a9ca4d63f7c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="768" /></p>

<p><img alt="" height="690" src="https://img-blog.csdnimg.cn/69301d266d8d4e2786c69b89959e25dd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1200" /></p>

<p>调优 调的就是堆空间，但是调优之前先要学会看gc日志，然后工具命令的使用jstat jps jmap jinfo</p>

<pre>
<code class="language-java">new Thread(() -&gt; {
            ArrayList&lt;byte[]&gt; list = new ArrayList&lt;&gt;();
            list.add(new byte[_8MB]);
            list.add(new byte[_8MB]);
            System.out.println("kiki");
        }).start();

        System.out.println("sleep....");
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }</code></pre>

<p><strong>线程的内存溢出 不会导致主线程的结束，线程结束后分配的对象也会随之被回收。</strong></p>

<p></p>

<h1 id="%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86">基础知识</h1>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="JVM垃圾回收_trigger333的博客-CSDN博客" href="https://blog.csdn.net/weixin_40757930/article/details/123225919" title="JVM垃圾回收_trigger333的博客-CSDN博客">JVM垃圾回收_trigger333的博客-CSDN博客</a></p>

<p></p>

<p><img alt="" height="389" src="https://img-blog.csdnimg.cn/5c1c378e44964fe3a41157e611f1b357.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="516" /></p>

<h1 id="%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E5%99%A8">垃圾回收器</h1>

<h2 id="Serial%20%2B%20Serial%20Old%E4%B8%B2%E8%A1%8C"><br />
Serial + Serial Old串行</h2>

<p>堆内存较小，适合个人电脑，不然要耗费很长时间</p>

<p>新生代单线程收集器，标记和清理都是单线程，优点是简单高效；</p>

<p><img alt="" height="459" src="https://img-blog.csdnimg.cn/aacc39aa2c804ffe873ae3ab51d34f0e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1096" /></p>

<p> </p>

<h2 id="Parallel%20%2B%20Parallel%20Old%E5%90%9E%E5%90%90%E9%87%8F%E4%BC%98%E5%85%88">Parallel + Parallel Old吞吐量优先</h2>

<p>新生代收并行集器，实际上是Serial收集器的多线程版本，在多核CPU环境下有着比Serial更好的表现；</p>

<p>堆内存较大 多核cpu 适合 服务器，gc总时间短 适合科学计算等任务</p>

<p>分布式计算</p>

<p>可以设置的参数由 timeratio 一般设置为19  也就是0.05，表示GC暂停的时间不能大于程序运行时间的0.05。垃圾回收器会根据用户自定义的参数动态调整堆的大小。堆越大，一般来说垃圾回收的次数就会变少，不过每次垃圾回收的时间会变多。</p>

<p><img alt="" height="762" src="https://img-blog.csdnimg.cn/9f9ac107047f42769c3dd32848f2f522.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1049" /></p>

<p> </p>

<p></p>

<h2 id="CMS%E5%93%8D%E5%BA%94%E6%97%B6%E9%97%B4%E4%BC%98%E5%85%88">CMS响应时间优先</h2>

<p>Concurrent Mark Sweep。</p>

<p>看名字就知道，CMS是一款并发、使用标记-清除算法的gc。</p>

<p>CMS是<strong>针对老年代进行回收</strong>的GC。</p>

<p>在一些对响应时间有很高要求的应用或网站中，用户程序不能有长时间的停顿，CMS 可以用于此场景。</p>

<p> 
</p><p> 
</p><h3 id="CMS%E5%9B%9E%E6%94%B6%E8%BF%87%E7%A8%8B%E5%8F%AF%E4%BB%A5%E5%88%86%E4%B8%BA%E5%93%AA%E4%BA%9B%E6%AD%A5%E9%AA%A4%EF%BC%9F">CMS回收过程可以分为哪些步骤？</h3>



<p>（1）初试标记：初试标记仅仅只是标记一下GC Roots能直接关联到的对象，速度很快，但需要暂停所有其他的工作线程。</p>

<p>（2）并发标记：GC 和用户线程一起工作，执行GC Roots跟踪标记过程，不需要暂停工作线程。</p>

<p>（3）重新标记：在并发标记过程中用户线程继续运作，导致在垃圾回收过程中部分对象的状态发生了变化，未来确保这部分对象的状态的正确性，需要对其重新标记并暂停工作线程。</p>

<p>（4）并发清除：清理删除掉标记阶段判断的已经死亡的对象，这个过程用户线程和垃圾回收线程同时发生。</p>

<p>需要重新标记 <strong>是因为在并发的过程中待回收的对象可能会变多，或者原来被判定为垃圾的现在被强引用了。</strong></p>

<h3 id="%E2%80%8BCMS%E7%9A%84%E9%97%AE%E9%A2%98%C2%A0"><img alt="" height="562" src="https://img-blog.csdnimg.cn/93ebae73826e47599afeba79bba8b8c1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1091" /><br />
CMS的问题 </h3>

<p>（1）CMS收集器对处理器资源非常敏感。</p>

<p>（2）CMS是基于标记-清除算，产生大量的空间碎片。</p>

<p>（3）如果并发失败，响应时间就会变得很长。CMS并发失败 （因为碎片太多，需要整理） 就会退化为 serialold的， 响应时间就会变的很长。</p>

<p>使用如下命令查看当前jvm使用的垃圾回收器</p>

<p>"C:\Program Files\Java\jdk1.8.0_231\bin\java" -XX:+PrintFlagsFinal -version |  findstr "GC"</p>

<p> <img alt="" height="323" src="https://img-blog.csdnimg.cn/4d00a2692c704531819d512e9b30742d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1200" /></p>

<p>默认使用 并行的垃圾回收器 新生代老年代都是。</p>

<p></p>

<p></p>

<h1 id="%E8%B0%83%E4%BC%98%E6%AD%A5%E9%AA%A4">调优步骤</h1>

<p><img alt="" height="257" src="https://img-blog.csdnimg.cn/3b33e5a74b744967ae431a67af0ec4e3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="976" /></p>

<p> </p>

<p><img alt="" height="526" src="https://img-blog.csdnimg.cn/3130705f06e54c939b5a3d0375eda724.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="850" /></p>

<p> 在查询数据库时不要select * ,使用数据类型能用占用空间小的尽量用，实现缓存 不要自己建map，尽量使用第三方缓存，比如redis，这样和堆区没有关系。</p>

<h1 id="wlOBf">新生代调优</h1>

<p><img alt="" height="351" src="https://img-blog.csdnimg.cn/28508539a5c94bbbbc918aefe8498c9d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="927" /></p>

<p> TLAB 就和ThreadLocal差不多，每一个线程在堆区（伊甸园）都有自己的一块区域供他们分配内存。这样就各自的数据就隔离了，增加并发度。</p>

<p></p>

<h2 id="%E6%96%B0%E7%94%9F%E4%BB%A3%E6%AF%94%E4%BE%8B%E8%B6%8A%E5%A4%A7%E8%B6%8A%E5%A5%BD%E5%90%97">新生代比例越大越好吗</h2>

<p><img alt="" height="227" src="https://img-blog.csdnimg.cn/4318ba0b10f6496599928f078f9e5b40.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1085" /></p>

<p> 不是的，新生代越大，minor gc 的次数越少，在一定条件下，吞吐量也越大，但随着新生代比例进一步增大，每次gc 的时间就会边长，吞吐量反而下降，大致可以认为是钟形的。</p>

<p>新生代能容纳所有[并发量*(请求-响应)]的数据，这样尽可能的减少或者不发生新生代gc</p>

<p>幸存区大到能保留[挡前活跃对象+需要晋升对象</p>

<p></p>

<h1 id="%E8%80%81%E5%B9%B4%E4%BB%A3%E8%B0%83%E4%BC%98">老年代调优</h1>

<p>待续</p>

<p></p>

<p></p>
