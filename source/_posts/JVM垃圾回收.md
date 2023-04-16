---
title: JVM垃圾回收
tags: java 开发语言 后端
categories: JVM
abbrlink: 566ec62f
date: 2022-03-02 12:04:24
---

<!--more-->

<p>垃圾回收简介，垃圾回收器介绍，如何调优，调优的基本思路。</p>

<p>主要内容参考：黑马 JVM</p>

<p id="main-toc"><strong>目录</strong></p>

<p id="hzGlE-toc" style="margin-left:0px;"><a href="#hzGlE">垃圾回收</a></p>

<p id="ZgUdd-toc" style="margin-left:40px;"><a href="#ZgUdd">判断（谁被回收）</a></p>

<p id="rZzDp-toc" style="margin-left:40px;"><a href="#rZzDp">四种引用</a></p>

<p id="UTZon-toc" style="margin-left:40px;"><a href="#UTZon">分代垃圾回收工作机制</a></p>

<p id="WcGDU-toc" style="margin-left:80px;"><a href="#WcGDU">简述分代垃圾回收器是怎么工作的？</a></p>

<p id="JHsfT-toc" style="margin-left:80px;"><a href="#JHsfT">你知道一个对象怎么从新生代变成老年代吗</a></p>

<p id="u570561da-toc" style="margin-left:80px;"><a href="#u570561da">如果子线程堆区溢出，会影响主线程的正常运行吗？</a></p>

<p id="WsKNM-toc" style="margin-left:40px;"><a href="#WsKNM">垃圾回收策略（怎么回收）</a></p>

<p id="GmCHM-toc" style="margin-left:40px;"><a href="#GmCHM">新生代垃圾回收器和老年代垃圾回收器都有哪些？有什么区别？</a></p>

<p id="b2nKM-toc" style="margin-left:0px;"><a href="#b2nKM">垃圾回收器</a></p>

<p id="Serial%20%2B%20Serial%20Old%E4%B8%B2%E8%A1%8C-toc" style="margin-left:40px;"><a href="#Serial%20%2B%20Serial%20Old%E4%B8%B2%E8%A1%8C">Serial + Serial Old串行</a></p>

<p id="Parallel%20%2B%20Parallel%20Old%E5%90%9E%E5%90%90%E9%87%8F%E4%BC%98%E5%85%88-toc" style="margin-left:40px;"><a href="#Parallel%20%2B%20Parallel%20Old%E5%90%9E%E5%90%90%E9%87%8F%E4%BC%98%E5%85%88">Parallel + Parallel Old吞吐量优先</a></p>

<p id="CMS%E5%93%8D%E5%BA%94%E6%97%B6%E9%97%B4%E4%BC%98%E5%85%88-toc" style="margin-left:40px;"><a href="#CMS%E5%93%8D%E5%BA%94%E6%97%B6%E9%97%B4%E4%BC%98%E5%85%88">CMS响应时间优先</a></p>

<p id="u5390f54b-toc" style="margin-left:80px;"><a href="#u5390f54b">CMS回收过程可以分为哪些步骤？</a></p>

<p id="CMS%E7%9A%84%E9%97%AE%E9%A2%98-toc" style="margin-left:80px;"><a href="#CMS%E7%9A%84%E9%97%AE%E9%A2%98">CMS的问题</a></p>

<p id="CMS%E7%9A%84%E7%89%B9%E7%82%B9-toc" style="margin-left:80px;"><a href="#CMS%E7%9A%84%E7%89%B9%E7%82%B9">CMS的特点</a></p>

<p id="G1%20GC%EF%BC%88%E7%BB%BC%E5%90%88%EF%BC%89-toc" style="margin-left:40px;"><a href="#G1%20GC%EF%BC%88%E7%BB%BC%E5%90%88%EF%BC%89">G1 GC（综合）</a></p>

<p id="ua2f01c2f-toc" style="margin-left:80px;"><a href="#ua2f01c2f">分区回收过程：</a></p>

<p id="k2bf7-toc" style="margin-left:80px;"><a href="#k2bf7">G1垃圾回收器的改进是什么？相比于CMS突出的地方是什么？</a></p>

<p id="Mklqs-toc" style="margin-left:80px;"><a href="#Mklqs">追问3：现在jdk默认使用的是哪种垃圾回收器？</a></p>

<p id="Ib0IN-toc" style="margin-left:0px;"><a href="#Ib0IN">调优</a></p>

<p id="wlOBf-toc" style="margin-left:40px;"><a href="#wlOBf">新生代调优</a></p>

<p id="jqHvi-toc" style="margin-left:40px;"><a href="#jqHvi">老年代调优</a></p>

<hr id="hr-toc" /><p></p>

<h1 id="hzGlE">垃圾回收</h1>

<h2 id="ZgUdd">判断（谁被回收）</h2>

<p id="u7ead21d2"><strong>引用计数法</strong>：早期的python虚拟机使用</p>

<p id="u575419c5"><strong>可达性分析算法</strong>：找到根对象，凡是直接或者间接引用到根对象上，就不被回收</p>

<p id="u96d9152c">从一个被称为 GC Roots 的对象开始向下搜索，如果一个对象到GC Roots没有任何引用链相连时，则说明此对象不可用。</p>

<p>这个算法的实质在于将一系列GC Roots作为初始的存活对象合集（live set），然后从该合集出发，探索所有能够被该合集引用到的对象，并将其加入到该和集中，这个过程称之为标记（mark）。 最终，未被探索到的对象便是死亡的，是可以回收的。</p>

<p>摘自：<a data-link-desc="垃圾回收垃圾回收，就是将已经分配出去的，但却不再使用的内存收回来，以便能搞再次分配。 在Java虚拟机的语境下，垃圾指的是死亡的对象所占据的堆空间。 如何辨别一个对象是存是亡？一种古老的辨别方法： 可以通…" data-link-icon="https://static.zhihu.com/heifetz/assets/apple-touch-icon-152.a53ae37b.png" data-link-title="JVM-可达性分析算法 - 知乎" href="https://zhuanlan.zhihu.com/p/109262576" title="JVM-可达性分析算法 - 知乎">JVM-可达性分析算法 - 知乎</a></p>

<p id="u2d3bd291"><strong>哪些是根对象？</strong></p>

<p>GC Roots可以理解为由堆外指向堆内的引用， 一般而言，GC Roots包括（但不限于）以下几种：</p>

<ol><li>Java 方法栈桢中的局部变量；</li>
	<li><strong>已加载类的静态变量；</strong></li>
	<li>JNI handles；</li>
	<li>已启动且未停止的 <strong>Java 线程</strong>。</li>
</ol><p id="u54d1efdf">比如：系统类的、锁对象、线程相关的 都是根对象，比如 object，string等</p>

<p id="ud652f80e">jmt和 jmap 指令联合可以查看堆区的根对象</p>

<p id="u4d9e8579"></p>

<h2 id="rZzDp">四种引用</h2>

<p id="uaacce899"><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="Java四种引用类型 - Helldorado - 博客园（）" href="https://www.cnblogs.com/liyutian/p/9690974.html" title="Java四种引用类型 - Helldorado - 博客园（）">Java四种引用类型 - Helldorado - 博客园（</a><span style="color:#fe2c24;">附带代码</span><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="Java四种引用类型 - Helldorado - 博客园（）" href="https://www.cnblogs.com/liyutian/p/9690974.html" title="Java四种引用类型 - Helldorado - 博客园（）">Java四种引用类型 - Helldorado - 博客园（</a></p>

<p id="u270b8d3a">强 软 弱 虚</p>

<p id="ubc6673a8"><strong>强引用</strong>：GC root对象可以直接到达的对象就是强引用的对象，一般在程序中创建的都是强引用的，只有当没有引用的时候才会回收。</p>

<p id="uef71cfbd"><strong>软引用</strong>：由一个GC root 关联一个软引用对象，软引用又会关联一个对象，这就是软引用对象，当发生垃圾回收且内存不够时，就会回收。</p>

<p id="u01bccf48">软引用是用来描述一些非必需但仍有用的对象。在内存足够的时候，软引用对象不会被回收，只有在内存不足时，系统则会回收软引用对象，如果回收了软引用对象之后仍然没有足够的内存，才会抛出内存溢出异常。这种特性常常被用来实现缓存技术，比如网页缓存，图片缓存等。</p>

<p id="u7a0db4d0"><strong>弱引用</strong>：由一个GC root 关联一个弱引用对象，弱引用又会关联一个对象，这就是弱引用对象，当发生垃圾回收就会回收。</p>

<p id="u4ae7cf2f">软引用和弱引用会进入引用队列，线程会在后台 扫描 引用队列，定期释放这两个引用自身。</p>

<p id="ua3a7917f"><strong>虚引用</strong>：必须配合引用队列使用，主要配合 <strong>ByteBuffer </strong>使用，被引用对象回收时，会将虚引用入队， 由 Reference Handler 线程调用虚引用相关方法释放直接内存</p>

<p id="u9868065b">NIO的时候会用到，因为是直接内存，所以gc 管不到，只能通过虚引用，再加一个线程去手动释放</p>

<p id="u71bb1ac3"><strong>终接器引用</strong>：</p>

<p id="u4feb71b3">无需手动编码，但其内部配合引用队列使用，在<strong>第一次垃圾回收</strong>时，<strong>终结器引用</strong>入队（被引用对象</p>

<p id="u205f8f75">暂时没有被回收），再由 Finalizer 线程通过终结器引用找到被引用对象并调用它的 finalize</p>

<p id="u714aea8f">方法，<strong>第二次 GC 时</strong>才能回收被引用对象</p>

<p id="ue5903a3b"></p>

<p id="u7cbfd3e4">强 &gt; 软 &gt; 弱 &gt; 虚引用</p>

<p> 
</p><h2 id="UTZon">分代垃圾回收工作机制</h2>


<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/7fbb729f9275eede5030185886b0a46f.png" /></p>

<h3 id="WcGDU"><strong>简述分代垃圾回收器是怎么工作的？</strong></h3>

<p id="u3298a235">分代回收器有两个分区：老生代和新生代，新生代默认的空间占比总空间的 1/3，老生代的默认占比是 2/3。</p>

<p id="u029b5f08">新生代使用的是复制算法，新生代里有 3 个分区：Eden、To Survivor、From Survivor，它们的默认占比是 8:1:1，它的执行流程如下：</p>

<ul><li id="ub6658ee9"><strong>把 Eden + From Survivor 存活的对象放入 To Survivor 区；</strong></li>
	<li id="u1f52eae8"><strong>清空 Eden 和 From Survivor 分区</strong><strong>；</strong></li>
</ul><ul><li id="u64c9decd"><strong>From Survivor 和 To Survivor 分区交换，From Survivor 变 To Survivor，To Survivor 变 From Survivor。</strong></li>
</ul><p id="u5a330e7d">每次在 <strong>From Survivor 到 To Survivor</strong> 移动时都存活的对象，年龄就 +1，当年龄到达 15（默认配置是 15）时，升级为老生代。大对象也会直接进入老生代。</p>

<p id="u58ce66f8">老生代当空间占用到达某个值之后就会触发全局垃圾收回，一般使用<strong>标记整理</strong>的执行算法。以上这些循环往复就构成了整个分代垃圾回收的整体执行流程。</p>

<p id="fqwEB"></p>

<h3 id="JHsfT">你知道一个对象怎么从新生代变成老年代吗</h3>

<p id="u34cccf4a">一个对象初始分配在伊甸园，新生代空间不足 触发minor gc 如果该对象存活 就会复制到 to区</p>

<p id="uc91fa608">交换from to，年龄加一，几次新生代空间不足 进行gc后 年龄达到阈值就会进入老年代。</p>

<p id="u6fab0036"></p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/30ffef716867d4148e3a3b637d253b20.png" /></p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/4ea3a2015726e840ab4005d03aa956f4.png" /></p>

<p id="u579c90d4"><strong>通过调试程序可以看到 垃圾回收的具体过程。</strong></p>

<p id="u6e211766">特例： 堆区存储大对象时 如果新生代不够 老年代够 那么会直接晋升</p>

<p id="ub68903e5">如果都不够，直接堆区溢出</p>

<h3 id="u570561da">如果子线程堆区溢出，会影响主线程的正常运行吗？</h3>

<p>其实发生OOM的线程一般情况下会死亡，也就是会被终结掉，该线程持有的对象占用的heap都会被gc了，释放内存。因为发生OOM之前要进行gc，<strong>就算其他线程能够正常工作</strong>，也会因为频繁gc产生较大的影响。</p>

<p>一般不会影响其他线程工作。</p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="高德面试官问我：JVM内存溢出后服务还能运行吗，我一顿操作行云流水_wj1314250的博客-CSDN博客_oom程序还能运行吗" href="https://blog.csdn.net/wj1314250/article/details/119145609" title="高德面试官问我：JVM内存溢出后服务还能运行吗，我一顿操作行云流水_wj1314250的博客-CSDN博客_oom程序还能运行吗">高德面试官问我：JVM内存溢出后服务还能运行吗，我一顿操作行云流水_wj1314250的博客-CSDN博客_oom程序还能运行吗</a></p>

<p id="u15476faf"><strong>一个进程一个堆 一个线程一个栈</strong></p>

<p></p>

<h2 id="WsKNM">垃圾回收策略（怎么回收）</h2>

<p id="ud832dff7"><strong>标记清除</strong></p>

<p id="u65faf821">优点 速度快</p>

<p id="u9f6f5670">缺点 空间不连续 有内存碎片</p>

<p id="uc0b14fc4"><strong>复制算法</strong></p>

<p id="u27890d07">双倍内存空间 效率较低</p>

<p id="u37260d46"><strong>标记整理</strong></p>

<p id="ua9b64e2e">没有内存碎片 效率较低</p>

<p id="u6d8ae90b"><strong>在新生代中可以使用复制算法，但是在老年代就不能选择复制算法了</strong>，因为老年代的对象存活率会较高，这样会有较多的复制操作，导致效率变低。</p>

<p id="u0ba57448">标记-清除算法可以应用在老年代中，但是它效率不高，在内存回收后容易产生大量内存碎片。因此就出现了一种标记-整理算法（Mark-Compact）算法，与标记-清除算法不同的是，在标记可回收的对象后将所有存活的对象压缩到内存的一端，使他们紧凑的排列在一起，然后对端边界以外的内存进行回收。回收后，已用和未用的内存都各自一边。</p>

<p id="ud194a3e4">优点：解决了标记-清除算法存在的内存碎片问题。</p>

<p id="u5dc48a58">缺点：仍需要进行局部对象移动，一定程度上降低了效率。</p>

<p id="u76f8bdb1"></p>

<p id="u9139f0d7">分代算法：根据对象存活周期的不同将内存划分为几块，一般是新生代和老年代，</p>

<p id="uef518369"><strong><span style="color:#fe2c24;">新生代基本采用复制算法，老年代采用标记整理算法。</span></strong></p>

<p id="u95724d6f"></p>

<h2 id="GmCHM">新生代垃圾回收器和老年代垃圾回收器都有哪些？有什么区别？</h2>

<ul><li id="u6cfd3ecc">新生代回收器：Serial、ParNew、Parallel Scavenge</li>
	<li id="u7f238a71">老年代回收器：<strong>Serial Old、Parallel Old、CMS</strong></li>
</ul><ul><li id="u80e24e25">整堆回收器：G1</li>
</ul><h1 id="b2nKM">垃圾回收器</h1>

<h2 id="Serial%20%2B%20Serial%20Old%E4%B8%B2%E8%A1%8C"><span style="color:#fe2c24;"><strong>Serial +</strong> <strong>Serial Old</strong>串行</span></h2>

<p id="u83f6a5be">堆内存较小，适合个人电脑。</p>

<p id="u3d349463">不然要耗费很长时间</p>

<p id="u79ae8ab0">新生代单线程收集器，标记和清理都是单线程，优点是简单高效；</p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/d315a0109773ca985ee76f512c7e6a95.png" /></p>

<h2 id="Parallel%20%2B%20Parallel%20Old%E5%90%9E%E5%90%90%E9%87%8F%E4%BC%98%E5%85%88"><span style="color:#fe2c24;"><strong>Parallel + Parallel Old</strong>吞吐量优先</span></h2>

<p id="u332ad111">新生代收并行集器，实际上是Serial收集器的多线程版本，在多核CPU环境下有着比Serial更好的表现；</p>

<p id="u55ae3022">堆内存较大 多核cpu 适合 服务器，gc总时间短 适合科学计算等任务</p>

<p id="ub3d57ec1">分布式计算</p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/33b141770bd85646936bed850cd332db.png" /></p>

<h2 id="CMS%E5%93%8D%E5%BA%94%E6%97%B6%E9%97%B4%E4%BC%98%E5%85%88"><span style="color:#fe2c24;"><strong>CMS</strong>响应时间优先</span></h2>

<p id="u86612d5a">Concurrent Mark Sweep。</p>

<p id="u936c3a0d">看名字就知道，CMS是一款<strong>并发、使用标记-清除算法</strong>的gc。</p>

<p id="u1fb8ee02">CMS是针对老年代进行回收的GC。</p>

<p id="u1a918f4c">在一些对响应时间有很高要求的应用或网站中，用户程序不能有长时间的停顿，CMS 可以用于此场景。</p>

<p id="u2215e3b6">好文</p>

<p id="u2f4eff1d"><a class="has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="CMS垃圾收集器执行过程_lz710117239的博客-CSDN博客_cms垃圾收集器过程" href="https://blog.csdn.net/lz710117239/article/details/78565926" title="CMS垃圾收集器执行过程_lz710117239的博客-CSDN博客_cms垃圾收集器过程"><span class="link-card-box"><span class="link-title">CMS垃圾收集器执行过程_lz710117239的博客-CSDN博客_cms垃圾收集器过程</span><span class="link-link"><img class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" alt="icon-default.png?t=M1L8" />https://blog.csdn.net/lz710117239/article/details/78565926</span></span></a></p>

<p id="u694318ca">多线程</p>

<p id="u8bbafa6a">堆内存较大 多核CPU</p>

<p id="ub9d52093">gc 单次时间短</p>

<p id="u10fadb79">适合互联网的低延迟需求</p>

<p id="u7b13a35e"></p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/7c974a43534b1685a8af6c6ded676fcf.png" /></p>

<h3 id="u5390f54b">CMS回收过程可以分为哪些步骤？</h3>

<p id="u432207a4">（1）<strong>初试标记</strong>：初试标记仅仅只是标记一下GC Roots能直接关联到的对象，速度很快，但需要<strong>暂停</strong>所有其他的工作线程。</p>

<p id="uef6c05e8">（2）<strong>并发标记</strong>：GC 和用户线程一起工作，执行GC Roots跟踪标记过程，<strong>不需要暂停</strong>工作线程。</p>

<p id="u9011adc4">（3）<strong>重新标记</strong>：在并发标记过程中用户线程继续运作，导致在垃圾回收过程中部分对象的状态发生了变化，未来确保这部分对象的状态的正确性，需要对其重新标记并<strong>暂停</strong>工作线程。</p>

<p id="u0882491c">（4）<strong>并发清除</strong>：清理删除掉标记阶段判断的已经死亡的对象，这个过程用户线程和垃圾回收线程<strong>同时</strong>发生。</p>

<p><strong>需要重新标记</strong> 是因为在并发的过程中待回收的对象可能会变多，或者原来被判定为垃圾的现在被强引用了。</p>

<h3 id="CMS%E7%9A%84%E9%97%AE%E9%A2%98">CMS的问题</h3>

<p id="uf28d14f5">（1）CMS收集器对处理器资源非常敏感。</p>

<p id="ubff87a0b">（2）CMS是基于标记-清除算，产生大量的空间碎片。</p>

<p>（3）如果并发失败，响应时间就会变得很长。</p>

<p>具体的：</p>

<p id="u56bc6fe0">cms 并发失败 （因为碎片太多，需要整理） 就会退化为 串行parallel 的， 响应时间就会变的很长</p>

<h3 id="CMS%E7%9A%84%E7%89%B9%E7%82%B9">CMS的特点</h3>

<p id="u72450c4b"><strong>特点一</strong></p>

<p id="ub5e99f04">老年代并行收集器，以获取最短回收停顿时间为目标的收集器，具有<strong>高并发、低停顿</strong>的特点，追求<strong>最短GC回收停顿</strong>时间。</p>

<p id="uda49822f"><strong>特点二</strong></p>

<p id="u2d1b03fb">CMS 使用的是标记-清除的算法实现的，所以在 gc 的时候回产生大量的内存碎片，当剩余内存不能满足程序运行要求时，系统将会出现 Concurrent Mode Failure，临时 CMS 会采用 Serial Old 回收器（标记-整理）进行垃圾清除，此时的性能将会被降低。</p>

<p id="u3c2860b5"></p>

<h2 id="G1%20GC%EF%BC%88%E7%BB%BC%E5%90%88%EF%BC%89">G1 GC（综合）</h2>

<p id="u2f6c3723">Java堆并行收集器，<strong>G1收集器是JDK1.7提供的一个新收集器</strong>，G1收集器基于“标记-整理”算法实现，也就是说不会产生内存碎片。此外，G1收集器不同于之前的收集器的一个重要特点是：<strong>G1回收的范围是整个Java堆(包括新生代，老年代)，而前六种收集器回收的范围仅限于新生代或老年代。</strong></p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/c31367f0a85c3744b73494205b59d2fb.png" /></p>

<p id="u0caa89ca">g1 比cms更强，没有内存碎片 <strong>兼顾吞吐量和响应时间</strong></p>

<h3 id="ua2f01c2f">分区回收过程：</h3>

<p id="ud0048f56"></p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/3c13b28311dee171bb3761d36f5b20ca.png" /></p>

<p id="ud0f35011"></p>

<h3 id="k2bf7">G1垃圾回收器的改进是什么？相比于CMS突出的地方是什么？</h3>

<p id="u2cff8c12">回答：G1垃圾回收器<strong>抛弃了分代的概念，将堆内存划分为大小固定的几个独立区域</strong>，并维护一个优先级列表，在垃圾回收过程中根据<strong>系统允许的最长垃圾回收时间，优先回收垃圾最多的区域</strong>。（G1<a href="/jump/super-jump/word?word=%E7%AE%97%E6%B3%95">算法</a>是可控STW的一种<a href="/jump/super-jump/word?word=%E7%AE%97%E6%B3%95">算法</a>，GC收集器和我们GC调优的目标就是尽可能的减少STW的时间和次数。）</p>

<p id="u83660586">G1突出的地方：</p>

<p id="u5d998d0d">基于标记整理算法，<strong>不产生垃圾碎片。</strong></p>

<p id="u25ca3ee3"><span style="color:#fe2c24;"><strong>可以精确的控制停顿时间</strong></span>，在不牺牲吞吐量的前提下实现短停顿垃圾回收。</p>

<p id="uafc32d0f"></p>

<h3 id="Mklqs">追问3：现在jdk默认使用的是哪种垃圾回收器？</h3>

<p id="ud75b6c1c">回答：</p>

<p id="u490e700f">jdk1.7 默认垃圾收集器<strong>Parallel Scavenge</strong>（新生代）+Parallel Old（老年代）</p>

<p id="ua1cef92e">jdk1.8 默认垃圾收集器Parallel Scavenge（新生代）+Parallel Old（老年代）</p>

<p id="ue9081279">jdk1.9 默认垃圾收集器G1</p>

<p></p>

<h1 id="Ib0IN">调优</h1>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/fe492aad0c47e0172af4d7d2553dc7a8.png" /></p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/e3a9087dfd73b7ef8c8ed0665938c48a.png" /></p>

<p id="u22af3d8d"></p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/49df57bfb5db78a857ca20b4edd65a8d.png" /></p>

<p id="u174bee98"></p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/4c80786d1cfd7a1de4e77bf5684aa956.png" /></p>

<p id="u1cdf9913">jvm调优应该放在最后，代码 调优优先</p>

<h2 id="wlOBf">新生代调优</h2>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/663ae919581dd696a30a4131dd2c2ce7.png" /></p>

<h2 id="jqHvi">老年代调优</h2>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/12702b9c382ec33df559f09caa737f02.png" /></p>

<p id="uaab6f42b"></p>
