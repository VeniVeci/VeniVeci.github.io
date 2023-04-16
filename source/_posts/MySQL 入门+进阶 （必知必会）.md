---
title: MySQL 入门+进阶 （必知必会）
tags: mysql 数据库 sql
categories: MySQL 数据库
abbrlink: 33fab2f0
date: 2022-02-28 16:43:20
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E6%9C%89%E6%95%B0%E6%8D%AE%E5%BA%93-toc" style="margin-left:0px;"><a href="#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E6%9C%89%E6%95%B0%E6%8D%AE%E5%BA%93">为什么要有数据库</a></p>

<p id="%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E4%BC%98%E7%82%B9-toc" style="margin-left:40px;"><a href="#%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E4%BC%98%E7%82%B9">数据库的优点</a></p>

<p id="%E6%80%BB%E7%BB%93%E4%B8%80%E4%B8%8B%EF%BC%9A-toc" style="margin-left:40px;"><a href="#%E6%80%BB%E7%BB%93%E4%B8%80%E4%B8%8B%EF%BC%9A">总结一下：</a></p>

<p id="%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E5%88%86%E7%B1%BB-toc" style="margin-left:40px;"><a href="#%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E5%88%86%E7%B1%BB">数据库的分类</a></p>

<p id="MySQL%E5%85%A5%E9%97%A8-toc" style="margin-left:0px;"><a href="#MySQL%E5%85%A5%E9%97%A8">MySQL入门</a></p>

<p id="%E5%AE%89%E8%A3%85%E5%92%8C%E4%BD%BF%E7%94%A8-toc" style="margin-left:0px;"><a href="#%E5%AE%89%E8%A3%85%E5%92%8C%E4%BD%BF%E7%94%A8">安装和使用</a></p>

<p id="SQL%E8%AF%AD%E5%8F%A5%E4%BD%BF%E7%94%A8-toc" style="margin-left:0px;"><a href="#SQL%E8%AF%AD%E5%8F%A5%E4%BD%BF%E7%94%A8">SQL语句使用</a></p>

<p id="%E5%87%A0%E4%B8%AA%E6%B3%A8%E6%84%8F%E7%9A%84%E7%82%B9-toc" style="margin-left:40px;"><a href="#%E5%87%A0%E4%B8%AA%E6%B3%A8%E6%84%8F%E7%9A%84%E7%82%B9">几个注意的点</a></p>

<p id="SQL%E8%AF%AD%E5%8F%A5%E6%89%A7%E8%A1%8C%E9%A1%BA%E5%BA%8F-toc" style="margin-left:80px;"><a href="#SQL%E8%AF%AD%E5%8F%A5%E6%89%A7%E8%A1%8C%E9%A1%BA%E5%BA%8F">SQL语句执行顺序</a></p>

<p id="%E4%B8%89%E7%A7%8D%20join-toc" style="margin-left:80px;"><a href="#%E4%B8%89%E7%A7%8D%20join">三种 join</a></p>

<p id="%E7%B4%A2%E5%BC%95%E5%88%9B%E5%BB%BA-toc" style="margin-left:80px;"><a href="#%E7%B4%A2%E5%BC%95%E5%88%9B%E5%BB%BA">索引创建</a></p>

<p id="%E7%B4%A2%E5%BC%95-toc" style="margin-left:0px;"><a href="#%E7%B4%A2%E5%BC%95">索引</a></p>

<p id="%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E6%9C%89%E7%B4%A2%E5%BC%95-toc" style="margin-left:40px;"><a href="#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E6%9C%89%E7%B4%A2%E5%BC%95">为什么要有索引</a></p>

<p id="%E7%B4%A2%E5%BC%95%E4%BC%98%E7%BC%BA%E7%82%B9-toc" style="margin-left:40px;"><a href="#%E7%B4%A2%E5%BC%95%E4%BC%98%E7%BC%BA%E7%82%B9">索引优缺点</a></p>

<p id="%E4%BC%98%E5%8A%BF-toc" style="margin-left:80px;"><a href="#%E4%BC%98%E5%8A%BF">优势</a></p>

<p id="%E5%8A%A3%E5%8A%BF-toc" style="margin-left:80px;"><a href="#%E5%8A%A3%E5%8A%BF">劣势</a></p>

<p id="%E7%B4%A2%E5%BC%95%E7%9A%84%E5%88%86%E7%B1%BB-toc" style="margin-left:40px;"><a href="#%E7%B4%A2%E5%BC%95%E7%9A%84%E5%88%86%E7%B1%BB">索引的分类</a></p>

<p id="%E6%8C%89%E7%85%A7%E7%B4%A2%E5%BC%95%E7%9A%84%E7%89%B9%E5%BE%81-toc" style="margin-left:80px;"><a href="#%E6%8C%89%E7%85%A7%E7%B4%A2%E5%BC%95%E7%9A%84%E7%89%B9%E5%BE%81">按照索引的特征</a></p>

<p id="%E6%8C%89%E7%85%A7%E7%B4%A2%E5%BC%95%E7%9A%84%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84-toc" style="margin-left:80px;"><a href="#%E6%8C%89%E7%85%A7%E7%B4%A2%E5%BC%95%E7%9A%84%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84">按照索引的数据结构</a></p>

<p id="%E6%8C%89%E7%85%A7%E7%B4%A2%E5%BC%95%E7%9A%84%E7%89%A9%E7%90%86%E5%AD%98%E5%82%A8-toc" style="margin-left:80px;"><a href="#%E6%8C%89%E7%85%A7%E7%B4%A2%E5%BC%95%E7%9A%84%E7%89%A9%E7%90%86%E5%AD%98%E5%82%A8">按照索引的物理存储</a></p>

<p id="%E7%B4%A2%E5%BC%95%E7%9A%84%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%20B%2B%E6%A0%91-toc" style="margin-left:40px;"><a href="#%E7%B4%A2%E5%BC%95%E7%9A%84%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%20B%2B%E6%A0%91">索引的数据结构 B+树</a></p>

<p id="SQL%E4%BA%8B%E5%8A%A1%20%E9%80%9A%E4%BF%97%E7%90%86%E8%A7%A3-toc" style="margin-left:0px;"><a href="#SQL%E4%BA%8B%E5%8A%A1%20%E9%80%9A%E4%BF%97%E7%90%86%E8%A7%A3">SQL事务 通俗理解</a></p>

<p id="%E5%88%86%E5%BA%93%E5%88%86%E8%A1%A8-toc" style="margin-left:0px;"><a href="#%E5%88%86%E5%BA%93%E5%88%86%E8%A1%A8">分库分表</a></p>

<p id="SQL%E4%BC%98%E5%8C%96-toc" style="margin-left:0px;"><a href="#SQL%E4%BC%98%E5%8C%96">SQL优化</a></p>

<hr id="hr-toc" /><p></p>

<p></p>

<h1 id="%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E6%9C%89%E6%95%B0%E6%8D%AE%E5%BA%93">为什么要有数据库</h1>

<p>其实平常的文件比如 txt也可以存储数据，但是读写效率，存储容量等都不能满足需求，所以就出现了数据库。</p>

<h2 id="%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E4%BC%98%E7%82%B9">数据库的优点</h2>

<p>数据库系统主要有以下几点优点：</p>

<p>1.<strong>整体数据结构化</strong>：在数据库系统中，记录的结构和记录之间的联系有数据库管理系统进行维护，从而减轻了程序员的工作量，提高了工作效率。</p>

<p>2.数据的<strong>共享性高、冗余度低且易扩充</strong>：数据共享包括多个用户、多个应用可以同时存取数据库中的数据，也包括用户可以用各种方式通过接口使用数据库中的数据。同时，数据库实现数据共享大大减少了数据冗余，还能够避免数据之间的不相容性和不一致性。（数据的不一致性：指同一数据不同副本的值不一样）</p>

<p>3.数据<strong>独立性高</strong>：数据独立性包括数据的物理独立性和逻辑独立性，即用户的应用程序与数据库中数据的物理存储和数据的逻辑结构均相互独立。</p>

<p>4.数据由<strong>数据库管理系统统一管理和控制</strong>：利用数据库可对数据进行集中控制和管理，并通过数据模型表示各种数据的组织以及数据间的联系，同时数据库管理系统提供了以下几个方面的数据控制功能，以解决数据共享带来的安全隐患。</p>

<blockquote>
<p>数据的安全性保护：保护数据以防止不合法使用造成的数据泄密和破坏；<br />
数据的完整性检查：保证数据的正确性、有效性和相容性（数据中的相容性是指表示同一事实的两个数据应相同，或者满足某一约束关系的一组数据不应发生互斥）；<br />
并发控制：使在同一周期内，允许对数据实现多路存取，又能防止用户之间的不正常交互作用（例如，当多个用户的并发进程同时存取、修改数据库时，可能会发生相互干扰而得到错误的结果或使得数据库的完整性遭到破坏）；<br />
数据库恢复：数据库管理系统能及时发现故障，并将数据库从错误状态恢复到某一已知的正确状态（亦称为完整状态或一致状态）。</p>
</blockquote>

<h2 id="%E6%80%BB%E7%BB%93%E4%B8%80%E4%B8%8B%EF%BC%9A">总结一下：</h2>

<p><strong>存储空间大，读写效率高，支持高并发。</strong></p>

<p><strong>数据的完整性检查 ，安全保证 ，可恢复性。</strong></p>

<p><strong>易于维护，安全性高，通过数据库管理系统，提供了丰富的功能。</strong></p>

<p></p>

<h2 id="%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E5%88%86%E7%B1%BB">数据库的分类</h2>

<p>具体请看博文</p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="关系型数据库和非关系型数据库_trigger的博客-CSDN博客" href="https://blog.csdn.net/weixin_40757930/article/details/123177269" title="关系型数据库和非关系型数据库_trigger的博客-CSDN博客">关系型数据库和非关系型数据库_trigger的博客-CSDN博客</a></p>

<p>关系型数据库就是MySQL之类的，非关系型就是Redis，Redis属于nosql，意思是不仅仅是SQL，在SQL的基础上还有其他功能。</p>

<p>随着<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="云计算" href="https://baike.baidu.com/item/%E4%BA%91%E8%AE%A1%E7%AE%97/9969353" title="云计算">云计算</a>的发展和<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="大数据" href="https://baike.baidu.com/item/%E5%A4%A7%E6%95%B0%E6%8D%AE/1356941" title="大数据">大数据</a>时代的到来，<strong>关系型数据库</strong>越来越无法满足需要，这主要是由于越来越多的半<strong>关系型和非关系型数据</strong>需要用数据库进行存储管理，以此同时，分布式技术等新技术的出现也对数据库的技术提出了新的要求，于是越来越多的非关系型数据库就开始出现，这类数据库与传统的关系型数据库在设计和数据结构有了很大的不同， 它们更强调数据库数据的<strong>高并发读写和存储大数据</strong>，这类数据库一般被称为<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="NoSQL" href="https://baike.baidu.com/item/NoSQL/8828247" title="NoSQL">NoSQL</a>（<strong>Not only SQL</strong>）数据库。 而传统的关系型数据库在一些传统领域依然保持了强大的生命力。</p>

<p></p>

<h1 id="MySQL%E5%85%A5%E9%97%A8">MySQL入门</h1>

<p>MySQL 是一个关系型数据库管理系统，由瑞典 MySQL AB 公司开发，<strong>目前属于 Oracle 公司</strong>。MySQL 是一种关联数据库管理系统，关联数据库将数据保存在不同的表中，而不是将所有数据放在一个大仓库内，这样就增加了速度并提高了灵活性。</p>

<p>MySQL 支持大型数据库，支持 <strong>5000 万条记录</strong>的数据仓库，32 位系统表文件最大可支持 4GB，64 位系统支持最大的表文件为<strong>8TB</strong>。</p>

<p><strong><span style="color:#fe2c24;">可以认为mysql是一个服务器，向外提供一种数据库的服务，可以用客户端或者编程和它进行交互，前提是知道这个服务的网络地址（ip+端口号（MySQL端口号是3306））。 </span></strong></p>

<h1 id="%E5%AE%89%E8%A3%85%E5%92%8C%E4%BD%BF%E7%94%A8">安装和使用</h1>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="超详细MySQL安装及基本使用教程_bobo553443的博客-CSDN博客_mysql安装教程" href="https://blog.csdn.net/bobo553443/article/details/81383194" title="超详细MySQL安装及基本使用教程_bobo553443的博客-CSDN博客_mysql安装教程">超详细MySQL安装及基本使用教程_bobo553443的博客-CSDN博客_mysql安装教程</a></p>

<h1 id="SQL%E8%AF%AD%E5%8F%A5%E4%BD%BF%E7%94%A8">SQL语句使用</h1>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="mysql常用语句大全 - MrSmartLin - 博客园" href="https://www.cnblogs.com/mrlwc/p/12079149.html" title="mysql常用语句大全 - MrSmartLin - 博客园">mysql常用语句大全 - MrSmartLin - 博客园</a></p>

<h2 id="%E5%87%A0%E4%B8%AA%E6%B3%A8%E6%84%8F%E7%9A%84%E7%82%B9">几个注意的点</h2>

<h3 id="SQL%E8%AF%AD%E5%8F%A5%E6%89%A7%E8%A1%8C%E9%A1%BA%E5%BA%8F">SQL语句执行顺序</h3>

<p>开始-&gt;FROM子句-&gt;WHERE子句-&gt;GROUP BY子句-&gt;HAVING子句-&gt;ORDER BY子句-&gt;SELECT子句-&gt;LIMIT子句-&gt;最终结果 </p>

<p><img alt="" height="462" src="https://img-blog.csdnimg.cn/40eecf4658c3416184eb972a080dca3a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_17,color_FFFFFF,t_70,g_se,x_16" width="622" /></p>

<h3 id="%E4%B8%89%E7%A7%8D%20join">三种 join</h3>

<h3 id="%E7%B4%A2%E5%BC%95%E5%88%9B%E5%BB%BA">索引创建</h3>

<p></p>

<p></p>

<h1 id="%E7%B4%A2%E5%BC%95">索引</h1>

<h2 id="%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E6%9C%89%E7%B4%A2%E5%BC%95">为什么要有索引</h2>

<p>加快数据查询，索引（index）是帮助MySQL高效获取数据的数据结构（有序）</p>

<h2 id="%E7%B4%A2%E5%BC%95%E4%BC%98%E7%BC%BA%E7%82%B9">索引优缺点</h2>

<h3 id="%E4%BC%98%E5%8A%BF">优势</h3>

<p>1） 类似于书籍的目录索引，提高数据检索的效率，<strong>降低数据库的IO成本</strong>。</p>

<p>2） 通过索引列对数据进行排序，<strong>降低数据排序的成本</strong>，降低CPU的消耗。</p>

<h3 id="%E5%8A%A3%E5%8A%BF">劣势</h3>

<p>1） 实际上索引也是一张表，该表中保存了主键与索引字段，并指向实体类的记录，所以索引列也是要占用空间</p>

<p>的。</p>

<p>2） 虽然索引大大提高了查询效率，<strong>同时却也降低更新表的速度</strong>，如对表进行INSERT、UPDATE、DELETE。因为</p>

<p>更新表时，MySQL 不仅要保存数据，还要保存一下索引文件每次更新添加了索引列的字段，都会调整因为更新所</p>

<p>带来的键值变化后的索引信息。</p>

<h2 id="%E7%B4%A2%E5%BC%95%E7%9A%84%E5%88%86%E7%B1%BB">索引的分类</h2>

<h3 id="%E6%8C%89%E7%85%A7%E7%B4%A2%E5%BC%95%E7%9A%84%E7%89%B9%E5%BE%81">按照索引的特征</h3>

<p>1） 单值索引 ：即一个索引只包含单个列，一个表可以有多个单列索引</p>

<p>2） 唯一索引 ：索引列的值必须唯一，但允许有空值</p>

<p>3） 复合索引 ：即一个索引包含多个列</p>

<p></p>

<h3 id="%E6%8C%89%E7%85%A7%E7%B4%A2%E5%BC%95%E7%9A%84%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84">按照索引的数据结构</h3>

<p>MySQL目前提供了以下4种索引：</p>

<ul><li>
	<p>BTREE 索引 ： 最常见的索引类型，大部分索引都支持 B 树索引。</p>
	</li>
	<li>
	<p>HASH 索引：只有Memory引擎支持 ， 使用场景简单 。</p>
	</li>
	<li>
	<p>R-tree 索引（空间索引）：空间索引是MyISAM引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少，不做特别介绍。</p>
	</li>
	<li>
	<p>Full-text （全文索引） ：全文索引也是MyISAM的一个特殊索引类型，主要用于全文索引，InnoDB从Mysql5.6版本开始支持全文索引。</p>
	</li>
</ul><h3 id="%E6%8C%89%E7%85%A7%E7%B4%A2%E5%BC%95%E7%9A%84%E7%89%A9%E7%90%86%E5%AD%98%E5%82%A8">按照索引的物理存储</h3>

<p>聚集索引 ，索引本身就是数据，在物理上是聚集的</p>

<p>非聚集索引有单独的目录文件</p>

<p>区别：</p>

<p>聚集索引一个表只能有一个，而非聚集索引一个表可以存在多个 聚集索引存储记录是物理上连续存在，而非聚集索引是逻辑上的连续，物理存储并不连续</p>

<p>聚集索引:物理存储按照索引排序；聚集索引是一种索引组织形式，索引的键值逻辑顺序决定了表数据行的物理存储顺序。</p>

<p><strong>比如 主键就是聚集索引 因为物理上是连续的存储</strong></p>

<p>非聚集索引:物理存储不按照索引排序；非聚集索引则就是普通索引了，仅仅只是对数据列创建相应的索引，不影响整个表的物理存储顺序。</p>

<p><strong>对年龄建立的索引就是 非聚集索引</strong></p>

<p>索引是通过二叉树的数据结构来描述的，我们可以这么理解聚簇索引：索引的叶节点就是数据节点。而非聚簇索引的叶节点仍然是索引节点，只不过有一个指针指向对应的数据块。</p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="聚集索引和非聚集索引的区别_riemann_的博客-CSDN博客_聚集索引和非聚集索引的区别" href="https://blog.csdn.net/riemann_/article/details/90324846" title="聚集索引和非聚集索引的区别_riemann_的博客-CSDN博客_聚集索引和非聚集索引的区别">聚集索引和非聚集索引的区别_riemann_的博客-CSDN博客_聚集索引和非聚集索引的区别</a></p>

<h2 id="%E7%B4%A2%E5%BC%95%E7%9A%84%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%20B%2B%E6%A0%91">索引的数据结构 B+树</h2>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="【最通俗易懂】MySQL索引原理 - 知乎" href="https://zhuanlan.zhihu.com/p/348482791" title="【最通俗易懂】MySQL索引原理 - 知乎">【最通俗易懂】MySQL索引原理 - 知乎</a></p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="一步步分析为什么B+树适合作为索引的结构 以及索引原理 (阿里面试) - aspirant - 博客园" href="https://www.cnblogs.com/aspirant/p/9214485.html" title="一步步分析为什么B+树适合作为索引的结构 以及索引原理 (阿里面试) - aspirant - 博客园">一步步分析为什么B+树适合作为索引的结构 以及索引原理 (阿里面试) - aspirant - 博客园</a></p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="redis为何单线程 效率还这么高 为何使用跳表不使用B+树做索引(阿里) - aspirant - 博客园" href="https://www.cnblogs.com/aspirant/p/11704530.html" title="redis为何单线程 效率还这么高 为何使用跳表不使用B+树做索引(阿里) - aspirant - 博客园">redis为何单线程 效率还这么高 为何使用跳表不使用B+树做索引(阿里) - aspirant - 博客园</a></p>

<h1 id="SQL%E4%BA%8B%E5%8A%A1%20%E9%80%9A%E4%BF%97%E7%90%86%E8%A7%A3">SQL事务 通俗理解</h1>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="https://blog.csdn.net/weixin_40757930/article/details/123182984" href="https://blog.csdn.net/weixin_40757930/article/details/123182984" title="https://blog.csdn.net/weixin_40757930/article/details/123182984">https://blog.csdn.net/weixin_40757930/article/details/123182984</a></p>

<p></p>

<h1 id="%E5%88%86%E5%BA%93%E5%88%86%E8%A1%A8">分库分表</h1>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="什么是分库分表，为什么要分库分表？ - 知乎" href="https://www.zhihu.com/question/448775613" title="什么是分库分表，为什么要分库分表？ - 知乎">什么是分库分表，为什么要分库分表？ - 知乎</a></p>

<p>阿里云和华为云都写了回答。</p>

<p></p>

<h1 id="SQL%E4%BC%98%E5%8C%96">SQL优化</h1>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="MySQL优化之推荐使用规范_猪哥-CSDN博客" href="https://blog.csdn.net/u014044812/article/details/78931044" title="MySQL优化之推荐使用规范_猪哥-CSDN博客">MySQL优化之推荐使用规范_猪哥-CSDN博客</a></p>

<p></p>

<p></p>

<p></p>
