---
title: 二叉树 平衡树 搜索树 AVL树 红黑树 B树 B+树
tags: b树 数据结构 算法
categories: 四大件之数据结构和算法 MySQL 数据库
abbrlink: 94447dd0
date: 2022-03-08 15:53:53
---

<!--more-->

<p> 二叉树 平衡树 搜索树 AVL树 红黑树 B树 B+树 的由来和作用，有什么区别。</p>

<p>数据库索引结构为啥必须用B+树？</p>

<p id="main-toc"><strong>目录</strong></p>

<p id="%E5%B9%B3%E8%A1%A1%E6%A0%91-toc" style="margin-left:0px;"><a href="#%E5%B9%B3%E8%A1%A1%E6%A0%91">平衡树</a></p>

<p id="%E6%90%9C%E7%B4%A2%E6%A0%91-toc" style="margin-left:0px;"><a href="#%E6%90%9C%E7%B4%A2%E6%A0%91">搜索树</a></p>

<p id="AVL%E6%A0%91-toc" style="margin-left:0px;"><a href="#AVL%E6%A0%91">AVL树</a></p>

<p id="%E7%BA%A2%E9%BB%91%E6%A0%91-toc" style="margin-left:0px;"><a href="#%E7%BA%A2%E9%BB%91%E6%A0%91">红黑树</a></p>

<p id="B%E6%A0%91-toc" style="margin-left:0px;"><a href="#B%E6%A0%91">B树</a></p>

<p id="B%E6%A0%91%E6%98%AF%E5%A4%9A%E8%B7%AF%E5%B9%B3%E8%A1%A1%E6%90%9C%E7%B4%A2%E6%A0%91-toc" style="margin-left:40px;"><a href="#B%E6%A0%91%E6%98%AF%E5%A4%9A%E8%B7%AF%E5%B9%B3%E8%A1%A1%E6%90%9C%E7%B4%A2%E6%A0%91">B树是多路平衡搜索树</a></p>

<p id="磁盘io与预读-toc" style="margin-left:40px;"><a href="#%E7%A3%81%E7%9B%98io%E4%B8%8E%E9%A2%84%E8%AF%BB">磁盘IO与预读</a></p>

<p id="B%2B%E6%A0%91-toc" style="margin-left:0px;"><a href="#B%2B%E6%A0%91">B+树</a></p>

<p id="%E4%B8%BA%E4%BB%80%E4%B9%88%E8%AF%B4B%2B%E6%A0%91%E6%AF%94B%E6%A0%91%E6%9B%B4%E9%80%82%E5%90%88%E6%95%B0%E6%8D%AE%E5%BA%93%E7%B4%A2%E5%BC%95%EF%BC%9F-toc" style="margin-left:40px;"><a href="#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%AF%B4B%2B%E6%A0%91%E6%AF%94B%E6%A0%91%E6%9B%B4%E9%80%82%E5%90%88%E6%95%B0%E6%8D%AE%E5%BA%93%E7%B4%A2%E5%BC%95%EF%BC%9F">为什么说B+树比B树更适合数据库索引？</a></p>

<hr id="hr-toc" /><p></p>

<h1 id="%E5%B9%B3%E8%A1%A1%E6%A0%91">平衡树</h1>

<p>平衡树就是二叉树的最深节点和最浅节点深度不能大于1，主要目的保证这棵树各个节点深度相差不大，不会退化成链表。</p>

<h1 id="%E6%90%9C%E7%B4%A2%E6%A0%91">搜索树</h1>

<p><img alt="" height="226" src="https://img-blog.csdnimg.cn/cbac463f2c764011acc01bf7d59c09e1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_17,color_FFFFFF,t_70,g_se,x_16" width="394" /></p>

<p></p>

<p>搜索树（BST,Binary search tree），搜索树的中序遍历是一个递增的数组，目的是实现对每一个元素的二分查找，时间复杂度O(logn)。但是最坏情况下二叉树退化为链表，那么时间复杂度就是O(n)。</p>

<h1 id="AVL%E6%A0%91">AVL树</h1>

<p>AVL树是平衡搜索树 = 平衡树+搜索树，算是对搜索树的改进，目的是保证搜索树的搜索效率，不会退化为链表。</p>

<p><a data-link-desc="AVL树又称为高度平衡的二叉搜索树。它能保持二叉树的高度平衡，尽量降低二叉树的高度，减少树的平均搜索长度。AVL树的性质左子树和右子树的高度之差的绝对值不超过1树中的每一个左子树和右子树都是AVL树每个节点都有一个平衡因子，任一节点的平衡因子是-1或0或1.（每个节点的平衡因子等于右子树的高度减去左子树的高度，即：bf = rightHeigh - leftHeight）..." data-link-icon="https://g.csdnimg.cn/static/logo/favicon32.ico" data-link-title="平衡搜索树-AVLTree_YAIMZA的博客-CSDN博客_平衡搜索树" href="https://blog.csdn.net/qq_37941471/article/details/79706523" title="平衡搜索树-AVLTree_YAIMZA的博客-CSDN博客_平衡搜索树">平衡搜索树-AVLTree_YAIMZA的博客-CSDN博客_平衡搜索树</a></p>

<h1 id="%E7%BA%A2%E9%BB%91%E6%A0%91">红黑树</h1>

<p>红黑树是AVL树的进阶版，因为AVL树在频繁插入删除数据的时候，为了保证平衡，需要频繁进行调整，这样使得AVL树的效率变低，红黑树保持平衡的算法比AVL树更有效率。</p>

<h1 id="B%E6%A0%91">B树</h1>

<h2 id="B%E6%A0%91%E6%98%AF%E5%A4%9A%E8%B7%AF%E5%B9%B3%E8%A1%A1%E6%90%9C%E7%B4%A2%E6%A0%91">B树是多路平衡搜索树</h2>

<p><img alt="" height="458" src="https://img-blog.csdnimg.cn/d9c0912e0e014fd986bc5eab47093fbe.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1155" /></p>

<p></p>

<p>B树是多路平衡搜索树，应用在数据库中，因为数据量很庞大，如果是二叉，那么深度很大，使得搜索效率降低（<strong>数据库的每次搜索需要去磁盘加载到内存里面，而平常我们写的二分搜索都是在内存中，前者耗费的时间是后者的十万倍</strong>）。所以目的是减少磁盘IO的次数。</p>

<p>不仅如此，计算机从磁盘读取数据时以页(4KB)为单位的，每次读取4096byte。平衡二叉树每个节点只保存了一个关键字（如int即4byte），浪费了4092byte，极大的浪费了读取空间。</p>

<h2 id="磁盘io与预读"><strong>磁盘IO与预读</strong></h2>

<p><a data-link-desc="B树 前言 首先，为什么要总结B树、B+树的知识呢？最近在学习数据库索引调优相关知识，数据库系统普遍采用B-/+Tree作为索引结构（例如mysql的InnoDB引擎使用的B+树），理解不透彻B树，则" data-link-icon="https://common.cnblogs.com/favicon.svg" data-link-title="B树、B+树详解 - Assassinの - 博客园" href="https://www.cnblogs.com/lianzhilei/p/11250589.html" title="B树、B+树详解 - Assassinの - 博客园">B树、B+树详解 - Assassinの - 博客园</a></p>

<p><strong>　　</strong>计算机存储设备一般分为两种：内存储器(main memory)和外存储器(external memory)。 </p>

<p>　　内存储器为内存，内存存取速度快，但容量小，价格昂贵，而且不能长期保存数据(在不通电情况下数据会消失)。</p>

<p>　　外存储器即为磁盘读取，磁盘读取数据靠的是机械运动，每次读取数据花费的时间可以分为<strong>寻道时间、旋转延迟、传输时间</strong>三个部分，寻道时间指的是磁臂移动到指定磁道所需要的时间，主流磁盘一般在5ms以下；旋转延迟就是我们经常听说的磁盘转速，比如一个磁盘7200转，表示每分钟能转7200次，也就是说1秒钟能转120次，旋转延迟就是1/120/2 = 4.17ms；传输时间指的是从磁盘读出或将数据写入磁盘的时间，一般在零点几毫秒，相对于前两个时间可以忽略不计。那么访问一次磁盘的时间，<strong>即一次磁盘IO的时间约等于5+4.17 = 9ms左右</strong>，听起来还挺不错的，但要知道一台500 -MIPS的机器每秒可以执行5亿条指令，因为指令依靠的是电的性质，换句话说<strong>执行一次IO的时间可以执行40万条指令</strong>，数据库动辄十万百万乃至千万级数据，每次9毫秒的时间，显然是个灾难。</p>

<p>      考虑到磁盘IO是非常高昂的操作，计算机操作系统做了一些优化，当一次IO时，不光把当前磁盘地址的数据，而是把相邻的数据也都读取到内存缓冲区内，因为<strong>局部预读性原理</strong>告诉我们，当计算机访问一个地址的数据的时候，与其相邻的数据也会很快被访问到。每一次IO读取的数据我们称之为一页(page)。具体一页有多大数据跟操作系统有关，一般为4k或8k，<strong>也就是我们读取一页内的数据时候，实际上才发生了一次IO</strong>，这个理论对于索引的数据结构设计非常有帮助。</p>

<p>事实1 ： 不同容量的存储器，访问速度差异悬殊。</p>

<ul><li>磁盘(ms级别) &lt;&lt; 内存(ns级别)， 100000倍</li>
	<li>若内存访问需要1s，则一次外存访问需要一天</li>
	<li>为了避免1次外存访问，宁愿访问内存100次...所以将<code>最常用</code>的数据存储在最快的存储器中</li>
</ul><p>事实2 ： <strong>从磁盘中读 1 B，与读写 1KB 的时间成本几乎一样</strong></p>

<p>从以上数据中可以总结出一个道理，索引查询的数据主要受限于硬盘的I/O速度，查询I/O次数越少，速度越快，所以B树的结构才应需求而生；B树的每个节点的元素可以视为一次I/O读取，树的高度表示最多的I/O次数，在相同数量的总元素个数下，每个节点的元素个数越多，高度越低，查询所需的I/O次数越少；假设，一次硬盘一次I/O数据为8K，索引用int(4字节)类型数据建立，理论上一个节点最多可以为2000个元素，2000*2000*2000=8000000000，<strong><span style="color:#fe2c24;">80亿条的数据只需3次IO（理论值）</span></strong>，可想而知，B树做为索引的查询效率有多高；</p>

<h1 id="B%2B%E6%A0%91"><strong>B+树</strong></h1>

<p><img alt="" height="432" src="https://img-blog.csdnimg.cn/d4f94e74134745c2a6ff18af64a22d9f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1148" /></p>

<p></p>

<h2 id="%E4%B8%BA%E4%BB%80%E4%B9%88%E8%AF%B4B%2B%E6%A0%91%E6%AF%94B%E6%A0%91%E6%9B%B4%E9%80%82%E5%90%88%E6%95%B0%E6%8D%AE%E5%BA%93%E7%B4%A2%E5%BC%95%EF%BC%9F"><strong>为什么说B+树比B树更适合数据库索引？</strong></h2>

<p>1）B+树的磁盘读写代价更低</p>

<p>　　B+树的内部结点并没有指向关键字具体信息的指针。因此其内部结点相对B 树更小。如果把所有同一内部结点的关键字存放在同一盘块中，那么盘块所能容纳的关键字数量也越多。一次性读入内存中的需要查找的关键字也就越多。相对来说IO读写次数也就降低了；</p>

<p>2）B+树查询效率更加稳定</p>

<p>　　由于非终结点并不是最终指向文件内容的结点，而只是叶子结点中关键字的索引。所以任何关键字的查找必须走一条从根结点到叶子结点的路。<strong>所有关键字查询的路径长度相同，导致每一个数据的查询效率相当；</strong></p>

<p>3）B+树便于范围查询（<span style="color:#fe2c24;"><strong>最重要的原因，范围查找是数据库的常态</strong></span>）</p>

<p>　　B树在提高了IO性能的同时并没有<strong>解决元素遍历的效率低下</strong>的问题，正是为了解决这个问题，B+树应用而生。B+树只需要去遍历叶子节点就可以实现整棵树的遍历。而且在数据库中<strong>基于范围的查询</strong>是非常频繁的，而B树不支持这样的操作或者说效率太低；不懂可以看看这篇解读-》<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="范围查找" href="https://zhuanlan.zhihu.com/p/54102723" title="范围查找">范围查找</a></p>

<p>参考：</p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="二叉树、平衡二叉树、红黑树、B树、B+树与B*树 - 简书" href="https://www.jianshu.com/p/b597aa97c9de" title="二叉树、平衡二叉树、红黑树、B树、B+树与B*树 - 简书">二叉树、平衡二叉树、红黑树、B树、B+树与B*树 - 简书</a></p>

<p><a data-link-desc="B树 前言 首先，为什么要总结B树、B+树的知识呢？最近在学习数据库索引调优相关知识，数据库系统普遍采用B-/+Tree作为索引结构（例如mysql的InnoDB引擎使用的B+树），理解不透彻B树，则" data-link-icon="https://common.cnblogs.com/favicon.svg" data-link-title="B树、B+树详解 - Assassinの - 博客园" href="https://www.cnblogs.com/lianzhilei/p/11250589.html" title="B树、B+树详解 - Assassinの - 博客园">B树、B+树详解 - Assassinの - 博客园</a><br /><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="平衡二叉树、B树、B+树、B*树 理解其中一种你就都明白了 - 知乎" href="https://zhuanlan.zhihu.com/p/27700617" title="平衡二叉树、B树、B+树、B*树 理解其中一种你就都明白了 - 知乎">平衡二叉树、B树、B+树、B*树 理解其中一种你就都明白了 - 知乎</a></p>
