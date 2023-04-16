---
title: MVCC如何解决不可重复读和读取未提交
tags: MVCC 数据库
categories: MySQL 数据库
abbrlink: c649ac07
date: 2022-04-04 21:15:00
---

<!--more-->

<p>参考：<strong>《MySQL是怎样运行的：从根儿上理解MySQL》 </strong></p>

<p id="main-toc"><strong>目录</strong></p>

<p id="MVCC%E5%8E%9F%E7%90%86-toc" style="margin-left:0px;"><a href="#MVCC%E5%8E%9F%E7%90%86">MVCC原理</a></p>

<p id="%E7%89%88%E6%9C%AC%E9%93%BE-toc" style="margin-left:0px;"><a href="#%E7%89%88%E6%9C%AC%E9%93%BE">版本链</a></p>

<p id="ReadView-toc" style="margin-left:0px;"><a href="#ReadView">ReadView</a></p>

<p id="%E4%B8%BE%E4%BE%8B-toc" style="margin-left:40px;"><a href="#%E4%B8%BE%E4%BE%8B">举例</a></p>

<p id="%E8%AF%BB%E5%8F%96%E5%B7%B2%E6%8F%90%E4%BA%A4-toc" style="margin-left:40px;"><a href="#%E8%AF%BB%E5%8F%96%E5%B7%B2%E6%8F%90%E4%BA%A4">读取已提交</a></p>

<p id="%E5%8F%AF%E9%87%8D%E5%A4%8D%E8%AF%BB-toc" style="margin-left:40px;"><a href="#%E5%8F%AF%E9%87%8D%E5%A4%8D%E8%AF%BB">可重复读</a></p>

<p id="%E6%80%BB%E7%BB%93-toc" style="margin-left:40px;"><a href="#%E6%80%BB%E7%BB%93">总结</a></p>

<hr id="hr-toc" /><div>
<div>
<div>
<div></div>

<div>
<h1 id="MVCC%E5%8E%9F%E7%90%86"><span style="color:#000000;"><strong>MVCC</strong></span><span style="color:#000000;"><strong>原理</strong></span></h1>

<p>MVCC，全称Multi-Version Concurrency Control，<strong>即多版本并发控制</strong>。MVCC是一种并发控制的方法，一般在数据库管理系统中，实现对数据库的并发访问，在编程语言中实现事务内存。MVCC在MySQL InnoDB中的实现主要是为了提高数据库并发性能，用更好的方式去处理读写冲突，做到即<strong><span style="color:#fe2c24;">使有读写冲突时，也能做到不加锁，非阻塞并发读</span></strong>。</p>

<p>那么MVVC是怎么实现的呢，要理解其底层原理，需要知道两个概念，<span style="color:#fe2c24;"><strong>一个是版本链</strong></span>，表示某一个数据被修改的记录，比如某一条数据被事务1、事务2,、事务50依次修改。</p>

<p><strong><span style="color:#fe2c24;">一个是readview</span></strong>，表示在执行查询语句时会生成这样一个readview，里面有当前正在执行的事务有哪些，那么这些事务对应版本的数据我就不读取，这样不加锁我也知道哪些数据是还没有被提交的，顺着版本链我就知道哪些数据我可以读取，哪些不能读取。</p>

<p><span style="color:#fe2c24;"><strong>详解如下</strong></span></p>

<h1 id="%E7%89%88%E6%9C%AC%E9%93%BE"><strong>版本链</strong></h1>

<div>
<p><span style="color:#000000;">对于使用 </span><span style="color:#000000;">InnoDB 存储引擎的表来说，它的聚簇索引记录中都包含两个必要的隐藏列：</span></p>

<div><span style="color:#fe2c24;">trx_id ：</span><span style="color:#000000;">每次一个事务对某条聚簇索引记录进行改动时，都会把该事务的 </span><span style="color:#000000;">事务id </span><span style="color:#000000;">赋值给 </span><span style="color:#000000;">trx_id </span></div>

<div><span style="color:#000000;">隐藏列。 </span></div>

<div><span style="color:#fe2c24;">roll_pointer </span><span style="color:#000000;">：每次对某条聚簇索引记录进行改动时，都会把旧的版本写入到 </span><span style="color:#000000;">undo日志 </span><span style="color:#000000;">中，然后这个隐藏列就相当于一个指针，可以通过它来找到该记录修改前的信息</span></div>

<div></div>

<div><img alt="" height="295" src="https://img-blog.csdnimg.cn/abd35bd7f1d74c75a0e43d9a2e0f82b2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1064" /></div>

<div></div>

<div></div>

<div>
<div><span style="color:#000000;">每次对记录进行改动，都会记录一条 </span><span style="color:#000000;">undo日志 </span><span style="color:#000000;">，每条 </span><span style="color:#000000;">undo日志 </span><span style="color:#000000;">也都有一个 </span><span style="color:#000000;">roll_pointer </span><span style="color:#000000;">属性</span><span style="color:#000000;">，可以将这些 </span><span style="color:#000000;">undo日志 </span><span style="color:#000000;">都连起来，串成一个链表，所以现在的情况就像下图一样：</span></div>

<div><img alt="" height="548" src="https://img-blog.csdnimg.cn/36784b6c8fbc4db4a84722905562b088.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1084" /></div>

<p><strong><span style="color:#fe2c24;"> 这个版本链中 有 80， 100 ，200 这三个事务，当然这三个事务80的可能已经提交结束了，200的和100的可能还没有，那么200和100 就属于活跃事务。</span></strong></p>
</div>

<div></div>

<div>
<div><span style="color:#000000;">对该记录每次更新后，都会将旧值放到一条 </span><span style="color:#000000;">undo日志 </span><span style="color:#000000;">中，就算是该记录的一个旧版本，随着更新次数的增多，所有的版本都会被 </span><span style="color:#000000;">roll_pointer </span><span style="color:#000000;">属性连接成一个链表，我们把这个链表称之为 </span><span style="color:#000000;">版本链 </span><span style="color:#000000;">，</span><span style="color:#ff0000;">版本链的头节点就是当前记录最新的值</span><span style="color:#000000;">。另外，每个版本中还包含生成该版本时对应的 </span><span style="color:#000000;">事务id。</span></div>
</div>

<div></div>

<div></div>

<div></div>

<div></div>

<div>
<h1 id="ReadView"><span style="color:#000000;">ReadView </span></h1>

<div><span style="color:#000000;">ReadView </span><span style="color:#000000;">的概念，这个 </span><span style="color:#000000;">ReadView </span><span style="color:#000000;">中主要包含</span><span style="color:#000000;">4</span><span style="color:#000000;">个比较重要的内容：</span><span style="color:#fe2c24;"><strong>直接看下面的例子比较好理解</strong></span></div>

<div></div>

<div>
<div>
<div><strong><span style="color:#000000;">m_ids </span></strong><span style="color:#000000;"><strong>：</strong>表示在生成 </span><span style="color:#000000;">ReadView时当前系统中活跃的读写事务的 </span><span style="color:#000000;">事务id </span><span style="color:#000000;">列表。 </span></div>

<div><span style="color:#000000;"><strong>min_trx_id：</strong>表示在生成 </span><span style="color:#000000;">ReadView </span><span style="color:#000000;">时当前系统中活跃的读写事务中最小的 </span><span style="color:#000000;">事务id </span><span style="color:#000000;">，也就是 </span><span style="color:#000000;">m_ids </span><span style="color:#000000;">中的最小值。 </span></div>

<div><span style="color:#000000;"><strong>max_trx_id：</strong>表示生成 </span><span style="color:#000000;">ReadView </span><span style="color:#000000;">时系统中应该分配给下一个事务的 </span><span style="color:#000000;">id </span><span style="color:#000000;">值。 </span></div>

<div><strong><span style="color:#000000;">creator_trx_id </span></strong><span style="color:#000000;"><strong>：</strong>表示生成该 </span><span style="color:#000000;">ReadView </span><span style="color:#000000;">的事务的 </span><span style="color:#000000;">事务id </span><span style="color:#000000;">。 </span></div>

<div></div>

<div><span style="color:#000000;">注意max_trx_id并不是m_ids中的最大值，事务id是递增分配的。比方说现在有id为1，2，3这三 </span></div>

<div><span style="color:#000000;">个事务，之后id为3的事务提交了。那么一个新的读事务在生成ReadView时，m_ids就包括1和2，min_trx_id的值就是1，max_trx_id的值就是4。 </span></div>

<div></div>

<div>
<div><span style="color:#000000;">只有在对表中的记录做改动时（执行INSERT、DELETE、UPDATE这些语句时）才会 </span></div>

<div><span style="color:#000000;">为事务分配事务id，<strong>否则在一个只读事务中的事务id值都默认为0。</strong></span></div>

<div></div>

<div></div>

<h2 id="%E4%B8%BE%E4%BE%8B">举例</h2>
</div>

<div></div>

<div>假设一个表中，初始一个事务insert一条记录，名字是张一，事务提交，叫它事务A，事务id是1.</div>

<div></div>

<p>又来了事务2，修改名字为张二，但是还没有提交，属于活跃事务，</p>

<p>又来了事务3，修改名字为张三，也没有提交，属于活跃事务。</p>

<h2 id="%E8%AF%BB%E5%8F%96%E5%B7%B2%E6%8F%90%E4%BA%A4">读取已提交</h2>

<p><strong>这时候又一个事务启动，执行了查询语句，因为只有查询，所以事务id为0，</strong></p>

<p>那么首先会创建一个readview，里面有几项信息，比如当前活跃事务id表 ，这个表里面有2,3，</p>

<p>通过这个就知道2,3还没有提交，那么就不能读取这两个版本对应的名字，说不定后来它们又改成什么样子，而是按照版本链找，知道找到1，因为1不在活跃的事务id表中，所以读取这个版本对应的记录，读到“张一”。</p>

<p>如果之后事务2提交了，那么还是这个事务在读取的时候，<span style="color:#fe2c24;"><strong>就会新创建一个readview，这次里面的活跃id表中就没有事务2了，那么读到的就是“张二”。</strong></span></p>

<h2 id="%E5%8F%AF%E9%87%8D%E5%A4%8D%E8%AF%BB">可重复读</h2>

<p><strong>这时候又一个事务启动，执行了查询语句，因为只有查询，所以事务id为0，</strong></p>

<p>那么首先会创建一个readview，里面有几项信息，比如当前活跃事务id表 ，这个表里面有2,3，</p>

<p>通过这个就知道2,3还没有提交，那么就不能读取这两个版本对应的名字，说不定后来它们又改成什么样子，而是按照版本链找，知道找到1，因为1不在活跃的事务id表中，所以读取这个版本对应的记录，读到“张一”。</p>

<p>如果之后事务2提交了，那么还是这个事务在读取的时候，<span style="color:#fe2c24;"><strong>不会新创建一个readview，这次里面的活跃id表中依旧是事务2,3，那么读到的依然是是“张一”，这样就实现了可重复读。</strong></span></p>

<h2 id="%E6%80%BB%E7%BB%93">总结</h2>

<p><span style="color:#000000;">READ COMMITTD </span><span style="color:#000000;">、REPEATABLE READ </span><span style="color:#000000;">这两个隔离级别的一个很大不同就是：</span><span style="color:#ff0000;">生成</span><span style="color:#ff0000;">ReadView</span><span style="color:#ff0000;">的时机不同，</span><span style="color:#ff0000;">READ COMMITTD</span><span style="color:#ff0000;">在每一次进行普通</span><span style="color:#ff0000;">SELECT</span><span style="color:#ff0000;">操作前都会生成一个</span><span style="color:#ff0000;">ReadView</span><span style="color:#ff0000;">，而</span><span style="color:#ff0000;">REPEATABLE READ</span><span style="color:#ff0000;">只在第一次进行普通</span><span style="color:#ff0000;">SELECT</span><span style="color:#ff0000;">操作前生成一个</span><span style="color:#ff0000;">ReadView</span><span style="color:#ff0000;">，之后的查询操作都重复使用这个</span><span style="color:#ff0000;">ReadView</span><span style="color:#ff0000;">就好了</span><span style="color:#000000;">。</span></p>

<p></p>

<div>
<div>
<p style="margin-left:.0001pt;text-align:left;"><span style="color:#000000;">有了这个 ReadView ，这样在访问某条记录时，只需要按照下边的步骤判断记录的某个版本是否可见： </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#000000;">如果被访问版本的 trx_id 属性值与 ReadView 中的 creator_trx_id 值相同，意味着当前事务在访问它自己修改过的记录，</span><span style="color:#511b78;">所以该版本可以被当前事务访问。</span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#000000;">如果被访问版本的 trx_id 属性值小于 ReadView 中的 min_trx_id值，表明生成该版本的事务在当前事务生成 ReadView 前已经提交，</span><span style="color:#511b78;">所以该版本可以被当前事务访问。</span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#000000;">如果被访问版本的 trx_id 属性值大于 ReadView 中的 max_trx_id值，表明生成该版本的事务在当前事务生成 ReadView 后才开启，</span><span style="color:#511b78;">所以该版本不可以被当前事务访问。 </span></p>
</div>
</div>
</div>
</div>
</div>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#000000;">如果被访问版本的 trx_id 属性值在 ReadView 的 min_trx_id 和 max_trx_id 之间，那就需要判断一下trx_id 属性值是不是在 m_ids 列表中，</span><span style="color:#fe2c24;">如果在，说明创建 ReadView 时生成该版本的事务还是活跃的，该版本不可以被访问；如果不在，说明创建 ReadView 时生成该版本的事务已经被提交，该版本可以被访问。 </span></p>

<p style="margin-left:.0001pt;text-align:left;"><span style="color:#000000;">如果某个版本的数据对当前事务不可见的话，那就顺着版本链找到下一个版本的数据，继续按照上边的步骤判断可见性，依此类推，直到版本链中的最后一个版本。如果最后一个版本也不可见的话，</span><span style="color:#fe2c24;">那么就意味着该条记录对该事务完全不可见，查询结果就不包含该记录。 </span></p>

<p style="margin-left:.0001pt;text-align:justify;"></p>
</div>
</div>
</div>
</div>
</div>
