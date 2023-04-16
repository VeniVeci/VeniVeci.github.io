---
title: ConcurrentHashMap 1.8
tags: hashmap hash java
categories: Java基础知识 多线程
abbrlink: f34b2bfc
date: 2022-03-30 22:51:09
---

<!--more-->

<p></p>

<p>参考</p>

<p><a data-link-desc="作者：程序员库森链接：https://www.nowcoder.com/discuss/591527?source_id=profile_create_nctrack&amp;amp;channel=-1来源：牛客网本文汇总了常考的 ConcurrentHashMap 面试题，面试 ConcurrentHashMap，看这一篇就够了！为帮助大家高效复习，专门用&quot;★ &quot;表示面试中出现的频率，&quot;★ &quot;越多，代表越高频！实现原理ConcurrentHashMap 的实现原理是什么？ ★★★★★." data-link-icon="https://g.csdnimg.cn/static/logo/favicon32.ico" data-link-title="ConcurrentHashMap 面试题_学习使我可乐的博客-CSDN博客_concurrenthashmap面试题" href="https://blog.csdn.net/qq_40826814/article/details/115328565" title="ConcurrentHashMap 面试题_学习使我可乐的博客-CSDN博客_concurrenthashmap面试题">ConcurrentHashMap 面试题_学习使我可乐的博客-CSDN博客_concurrenthashmap面试题</a></p>

<p><a data-link-desc="原文：https://my.oschina.net/hosee/blog/675884并发编程实践中，ConcurrentHashMap是一个经常被使用的数据结构，相比于Hashtable以及Collections.synchronizedMap()，ConcurrentHashMap在线程安全的基础上提供了更好的写并发能力，但同时降低了对读一致性的要求（这点好像CAP理论啊 O(∩_∩)O）。ConcurrentHashMap的设计与实现非常精巧，大量的利用了volatile，final，CAS等loc" data-link-icon="https://g.csdnimg.cn/static/logo/favicon32.ico" data-link-title="JDK1.7与JDK1.8中ConcurrentHashMap原理总结_IT狗探求的博客-CSDN博客_concurrenthashmap jdk1.7" href="https://blog.csdn.net/a123demi/article/details/79100534" title="JDK1.7与JDK1.8中ConcurrentHashMap原理总结_IT狗探求的博客-CSDN博客_concurrenthashmap jdk1.7">JDK1.7与JDK1.8中ConcurrentHashMap原理总结_IT狗探求的博客-CSDN博客_concurrenthashmap jdk1.7</a></p>

<div><a data-link-desc="一、简介java8中ConcurrentHashMap的结构是：数组+链表+红黑树。因为在hash冲突严重的情况下，链表的查询效率是O(n)，所以jdk8中改成了单个链表的个数大于8时，数组长度小于64就扩容，数组长度大于等于64，则链表会转换为红黑树，这样以空间换时间，查询效率会变为O(nlogn)。红黑树在Node数组内部存储的不是一个TreeNode对象，而是一个TreeBin对..." data-link-icon="https://g.csdnimg.cn/static/logo/favicon32.ico" data-link-title="【十四】Java集合之ConcurrentHashMap源码分析（1.8）_jy02268879的博客-CSDN博客_concurrenthashmap源码分析1.8" href="https://blog.csdn.net/jy02268879/article/details/88599830" title="【十四】Java集合之ConcurrentHashMap源码分析（1.8）_jy02268879的博客-CSDN博客_concurrenthashmap源码分析1.8">【十四】Java集合之ConcurrentHashMap源码分析（1.8）_jy02268879的博客-CSDN博客_concurrenthashmap源码分析1.8</a></div>

<p>在数据结构上， JDK1.8 中的ConcurrentHashMap 选择了与 HashMap 相同的Node数组+链表+红黑树结构；在锁的实现上，抛弃了原有的 Segment 分段锁，采用CAS + synchronized实现更加细粒度的锁。</p>

<p>将锁的级别控制在了更细粒度的哈希桶数组元素级别，也就是说只需要锁住这个链表头节点（红黑树的根节点），就不会影响其他的哈希桶数组元素的读写，大大提高了并发度。<br />
 </p>

<p></p>

<h1>JDK1.8 中为什么使用内置锁 synchronized替换 可重入锁 ReentrantLock？</h1>

<p>    在 JDK1.6 中，对 synchronized 锁的实现引入了大量的优化，并且 synchronized 有多种锁状态，会从无锁 -&gt; 偏向锁 -&gt; 轻量级锁 -&gt; 重量级锁一步步转换。</p>

<p>    减少内存开销 。假设使用可重入锁来获得同步支持，<strong>那么每个节点都需要通过继承 AQS 来获得同步支持。</strong>但并不是每个节点都需要获得同步支持的，只有链表的头节点（红黑树的根节点）需要同步，这无疑带来了巨大内存浪费。<br />
 </p>

<h1>put</h1>

<p>大致可以分为以下步骤：</p>

<p>    根据 key 计算出 hash 值；</p>

<p>    判断是否需要进行初始化；</p>

<p>    定位到 Node，拿到首节点 f，判断首节点 f：</p>

<p>        如果为 null ，则通过 <strong>CAS 的方式尝试添加</strong>；</p>

<p>        如果为 f.hash = MOVED = -1 ，说明其他线程在扩容，参与一起扩容；</p>

<p>        如果都不满足 ，<strong>synchronized 锁住 f 节点</strong>，判断是链表还是红黑树，遍历插入；</p>

<p>    当在链表长度达到 8 的时候，数组扩容或者将链表转换为红黑树。<br />
 </p>

<p></p>

<h1>get</h1>

<p>大致可以分为以下步骤：</p>

<ol><li>
	<p>根据 key 计算出 hash 值，判断数组是否为空；</p>
	</li>
	<li>
	<p>如果是首节点，就直接返回；</p>
	</li>
	<li>
	<p>如果是<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="红黑树" href="https://blog.csdn.net/jump/super-jump/word?word=%E7%BA%A2%E9%BB%91%E6%A0%91" title="红黑树">红黑树</a>结构，就从<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="红黑树" href="https://blog.csdn.net/jump/super-jump/word?word=%E7%BA%A2%E9%BB%91%E6%A0%91" title="红黑树">红黑树</a>里面查询；</p>
	</li>
	<li>
	<p>如果是<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="链表" href="https://blog.csdn.net/jump/super-jump/word?word=%E9%93%BE%E8%A1%A8" title="链表">链表</a>结构，循环遍历判断。</p>
	</li>
</ol><p></p>

<h1>ConcurrentHashMap 的 get 方法是否要加锁，为什么？</h1>

<p>get 方法不需要加锁。因为 <strong>Node 的元素 value 和指针 next </strong>是用<strong> volatile 修饰的，在多线程环境下线程A修改节点的 value 或者新增节点的时候是对线程B可见的。</strong></p>

<p></p>

<h1>ConcurrentHashMap 不支持 key 或者 value 为 null 的原因</h1>

<p>我们先来说value 为什么不能为 null。因为 ConcurrentHashMap 是用于多线程的 ，如果ConcurrentHashMap.get(key)得到了 null ，这就无法判断，是映射的value是 null ，还是没有找到对应的key而为 null ，<strong>就有了二义性。</strong></p>

<p>而用于单线程状态的 HashMap 却可以用containsKey(key) 去判断到底是否包含了这个 null 。<br />
 </p>

<p></p>

<h1>JDK1.7 与 JDK1.8 中ConcurrentHashMap 的区别？</h1>

<p><strong>数据结构</strong>：取消了 Segment 分段锁的数据结构，取而代之的是数组+链表+红黑树的结构。</p>

<p><strong>保证线程安全机制</strong>：JDK1.7 采用 Segment 的分段锁机制实现线程安全，其中 Segment 继承自 ReentrantLock 。JDK1.8 采用CAS+synchronized保证线程安全。</p>

<p><strong>锁的粒度</strong>：JDK1.7 是对需要进行数据操作的 Segment 加锁，JDK1.8 调整为对每个数组元素加锁（Node）。</p>

<p><strong>链表转化为红黑树</strong>：定位节点的 hash 算法简化会带来弊端，hash 冲突加剧，因此在链表节点数量大于 8（且数据总量大于等于 64）时，会将链表转化为红黑树进行存储。</p>

<p>查询时间复杂度：从 JDK1.7的遍历链表O(n)， JDK1.8 变成遍历红黑树O(logN)。<br />
 </p>

<p> </p>
