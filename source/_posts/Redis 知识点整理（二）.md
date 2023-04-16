---
title: Redis 知识点整理（二）
tags: redis 数据库 缓存
categories: Redis 数据库
abbrlink: a3a48723
date: 2022-02-28 22:33:58
---

<!--more-->

<p>Redis 持久化机制，主从复制，集群，缓存击穿，缓存穿透，缓存雪崩，分布式锁等内容整理。</p>

<p id="main-toc"><strong>目录</strong></p>

<p id="icTlo-toc" style="margin-left:0px;"><a href="#icTlo">RDB AOF</a></p>

<p id="ClPye-toc" style="margin-left:40px;"><a href="#ClPye">RDB  ( Redis database )</a></p>

<p id="Vt1nu-toc" style="margin-left:40px;"><a href="#Vt1nu">AOF（ Append Only File ）</a></p>

<p id="inemo-toc" style="margin-left:0px;"><a href="#inemo">Redis主从复制</a></p>

<p id="iweYw-toc" style="margin-left:40px;"><a href="#iweYw">主从复制原理</a></p>

<p id="%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E4%BC%98%E7%BC%BA%E7%82%B9-toc" style="margin-left:40px;"><a href="#%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E4%BC%98%E7%BC%BA%E7%82%B9">主从复制优缺点</a></p>

<p id="%E4%BC%98%E7%82%B9-toc" style="margin-left:80px;"><a href="#%E4%BC%98%E7%82%B9">优点</a></p>

<p id="%E7%BC%BA%E7%82%B9-toc" style="margin-left:80px;"><a href="#%E7%BC%BA%E7%82%B9">缺点</a></p>

<p id="rGB38-toc" style="margin-left:0px;"><a href="#rGB38">哨兵模式</a></p>

<p id="%E5%93%A8%E5%85%B5%E6%A8%A1%E5%BC%8F%E7%9A%84%E4%BC%98%E7%BC%BA%E7%82%B9-toc" style="margin-left:40px;"><a href="#%E5%93%A8%E5%85%B5%E6%A8%A1%E5%BC%8F%E7%9A%84%E4%BC%98%E7%BC%BA%E7%82%B9">哨兵模式的优缺点</a></p>

<p id="%E4%BC%98%E7%82%B9-toc" style="margin-left:80px;"><a href="#%E4%BC%98%E7%82%B9">优点</a></p>

<p id="%E7%BC%BA%E7%82%B9-toc" style="margin-left:80px;"><a href="#%E7%BC%BA%E7%82%B9">缺点</a></p>

<p id="Server-toc" style="margin-left:0px;"><a href="#Server">Redis集群</a></p>

<p id="pfgF4-toc" style="margin-left:40px;"><a href="#pfgF4">优点</a></p>

<p id="ciSCU-toc" style="margin-left:40px;"><a href="#ciSCU">缺点</a></p>

<p id="bWXhs-toc" style="margin-left:0px;"><a href="#bWXhs">缓存穿透</a></p>

<p id="%E5%8E%9F%E5%9B%A0-toc" style="margin-left:40px;"><a href="#%E5%8E%9F%E5%9B%A0">原因</a></p>

<p id="u57756163-toc" style="margin-left:40px;"><a href="#u57756163">如何防止</a></p>

<p id="u4035d35e-toc" style="margin-left:80px;"><a href="#u4035d35e">空值缓存</a></p>

<p id="u74624516-toc" style="margin-left:80px;"><a href="#u74624516">布隆过滤器</a></p>

<p id="LXTa5-toc" style="margin-left:0px;"><a href="#LXTa5">缓存击穿</a></p>

<p id="%E5%8E%9F%E5%9B%A0-toc" style="margin-left:40px;"><a href="#%E5%8E%9F%E5%9B%A0">原因</a></p>

<p id="u68a55533-toc" style="margin-left:40px;"><a href="#u68a55533">解决方法</a></p>

<p id="P3lFb-toc" style="margin-left:0px;"><a href="#P3lFb">缓存雪崩</a></p>

<p id="%E5%8E%9F%E5%9B%A0-toc" style="margin-left:40px;"><a href="#%E5%8E%9F%E5%9B%A0">原因</a></p>

<p id="%E8%A7%A3%E5%86%B3%E6%8E%AA%E6%96%BD-toc" style="margin-left:40px;"><a href="#%E8%A7%A3%E5%86%B3%E6%8E%AA%E6%96%BD">解决措施</a></p>

<p id="Z9MIB-toc" style="margin-left:0px;"><a href="#Z9MIB">分布式锁</a></p>

<p id="v4gFQ-toc" style="margin-left:40px;"><a href="#v4gFQ">为啥用</a></p>

<p id="XO2BK-toc" style="margin-left:40px;"><a href="#XO2BK">如何实现和优化</a></p>

<p id="zm9tH-toc" style="margin-left:40px;"><a href="#zm9tH">问题：setnx刚好获取到锁，业务逻辑出现异常，导致锁无法释放</a></p>

<p id="ZnTqc-toc" style="margin-left:40px;"><a href="#ZnTqc">问题：可能会释放其他服务器的锁。</a></p>

<p id="soCOk-toc" style="margin-left:40px;"><a href="#soCOk">问题：删除操作缺乏原子性。</a></p>

<p id="ua9bbaf02-toc" style="margin-left:40px;"><a href="#ua9bbaf02">最后这块很可能存在一种异常场景：</a></p>

<p id="uaaa8eeaa-toc" style="margin-left:0px;"><a href="#uaaa8eeaa">如果Redis内存满了的话会发生什么？</a></p>

<hr id="hr-toc" /><p></p>

<h1 id="icTlo">RDB AOF</h1>

<p id="u700ee319">官方推荐两个持久化机制都启用。</p>

<p id="u7aecdb5e"><strong>如果对数据不敏感，可以选单独用RDB。</strong></p>

<p id="u47aab730">不建议单独用AOF，因为可能会出现Bug。</p>

<p id="u28eacbaf">如果只是做纯内存缓存，可以都不用。</p>

<p id="u3c9070a1"><strong>AOF和RDB同时开启，Redis听谁的？</strong></p>

<p id="u4dc3839a">AOF和RDB同时开启，系统默认取AOF的数据（数据不会存在丢失）</p>

<h2 id="ClPye">RDB  ( Redis database )</h2>

<p id="u6b18898d">在指定的时间间隔内将内存中的数据集快照写入磁盘，也就是行话讲的Snapshot快照，它恢复时是将快照文件直接读到内存里</p>

<p id="u7f801c25"><strong>备份是如何执行的</strong></p>

<p id="u02470715">Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何IO操作的，这就确保了极高的性能 如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。<strong>RDB的缺点是</strong><strong>最后一次持久化后的数据可能丢失</strong>。</p>

<p></p>

<p></p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/7cd337399f7b953e01511ab6e4bd22d3.png" /></p>

<h2 id="Vt1nu">AOF（ Append Only File ）</h2>

<p id="uc44698be">以<strong>日志</strong>的形式来记录每个写操作（增量保存），将Redis执行过的所有写指令记录下来(<strong>读操作不记录</strong>)（类似于mysql中的redo log）， <strong>只许追加文件但不可以改写文件</strong>，Redis启动之初会读取该文件重新构建数据，换言之，Redis 重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作</p>

<p id="u1bc407c3"><strong>AOF默认不开启</strong></p>

<p id="u1625fc8a">可以在Redis.conf中配置文件名称，默认为 appendonly.aof</p>

<p id="ucc767d21">AOF文件的保存路径，同RDB的路径一致。</p>

<p id="u71a71d98"></p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/f69b2fef008ef7e3fb90f216b56401ff.png" /></p>

<p id="ud246d713"></p>

<h1 id="inemo">Redis主从复制</h1>

<p id="u15c37921">主机数据更新后根据配置和策略，自动同步到备机的master/slaver机制，<strong>Master以写为主，Slave以读为主</strong></p>

<p></p>

<h2 id="iweYw">主从复制原理</h2>

<p id="u61e12de7">l Slave启动成功连接到master后会发送一个sync命令</p>

<p id="u4b6e88cd">l Master接到命令启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令， 在后台进程执行完毕之后，master将传送整个数据文件到slave,以完成一次全量复制</p>

<p id="ubefc5e99">l 全量复制：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。</p>

<p id="ufb0a8779">l 增量复制：Master继续将新的所有收集到的修改命令依次传给slave,完成同步</p>

<p id="u24428167">l 但是只要是重新连接master,<strong>一次完全同步（全量复制)</strong>将被自动执行</p>

<h2 id="%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E4%BC%98%E7%BC%BA%E7%82%B9">主从复制优缺点</h2>

<p>摘自：<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="Redis集群的 3 种方式，各自优缺点分析！ - 知乎" href="https://zhuanlan.zhihu.com/p/257845445?utm_source=wechat_session" title="Redis集群的 3 种方式，各自优缺点分析！ - 知乎">Redis集群的 3 种方式，各自优缺点分析！ - 知乎</a></p>

<h3 id="%E4%BC%98%E7%82%B9">优点</h3>

<ul><li>支持主从复制，主机会自动将数据同步到从机，可以进行读写分离</li>
	<li>Slave同样可以接受其它Slaves的连接和同步请求，这样可以有效的分载Master的同步压力。</li>
	<li>Master Server是以非阻塞的方式为Slaves提供服务。所以在Master-Slave同步期间，客户端仍然可以提交查询或修改请求。</li>
	<li>Slave Server同样是以非阻塞的方式完成数据同步。在同步期间，如果有客户端提交查询请求，Redis则返回同步之前的数据</li>
</ul><h3 id="%E7%BC%BA%E7%82%B9">缺点</h3>

<ul><li>Redis不具备自动容错和恢复功能，主机从机的宕机都会导致前端部分读写请求失败，需要等待机器重启或者手动切换前端的IP才能恢复。</li>
	<li>主机宕机，宕机前有部分数据未能及时同步到从机，切换IP后还会引入数据不一致的问题，降低了系统的可用性。</li>
	<li>Redis较难支持在线扩容，在集群容量达到上限时在线扩容会变得很复杂。</li>
</ul><p></p>

<h1 id="rGB38">哨兵模式</h1>

<p id="ua6657529">主从复制 设置哨兵，如果主机down掉那么从机上位。</p>

<p id="u9c73d7e7">Redis-server.exe sentinel.conf --sentinel</p>

<p id="u931ca614">能够后台监控主机是否故障，如果故障了根据投票数自动将从库转换为主库</p>

<p id="u11fc4a35"><img alt="" height="439" src="https://img-blog.csdnimg.cn/792f728360a04cb191f2c9e230563517.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="822" /></p>

<p></p>

<h2 id="%E5%93%A8%E5%85%B5%E6%A8%A1%E5%BC%8F%E7%9A%84%E4%BC%98%E7%BC%BA%E7%82%B9">哨兵模式的优缺点</h2>

<h3>优点</h3>

<ul><li>哨兵模式是基于主从模式的，所有主从的优点，哨兵模式都具有。</li>
	<li>主从可以自动切换，系统更健壮，可用性更高。</li>
</ul><h3>缺点</h3>

<ul><li>Redis较难支持在线扩容，在集群容量达到上限时在线扩容会变得很复杂。</li>
</ul><p></p>

<h1 id="Server">Redis集群</h1>

<p>redis的哨兵模式基本已经可以实现高可用，读写分离 ，但是在这种模式下每台redis服务器都存储相同的数据，很浪费内存，所以在redis3.0上加入了cluster模式，实现的redis的分布式存储，也就是说每台redis节点上存储不同的内容。</p>

<p></p>

<p id="u928bab68">Redis 集群实现了对Redis的水平扩容，即启动N个Redis节点，将整个数据库分布存储在这N个节点中，每个节点存储总数据的1/N。</p>

<p id="uf67c0235">Redis 集群通过分区（partition）来提供一定程度的可用性（availability）：即使集群中有一部分节点失效或者无法进行通讯，集群也可以继续处理命令请求。</p>

<p id="u39c5e2d4"></p>

<h2 id="pfgF4">优点</h2>

<p id="uf27c43bc">扩容，容灾，分摊压力</p>

<h2 id="ciSCU">缺点</h2>

<p id="u4c425fa9">方案没有被目前的公司广泛部署，因为大都采用 代理模式</p>

<p id="ua4267f05"></p>

<h1 id="bWXhs">缓存穿透</h1>

<p id="ud3ec3c7f">我们使用Redis大部分情况都是通过Key查询对应的值，假如发送的请求传进来的key是不存在Redis中的，那么就查不到缓存，查不到缓存就会去数据库查询。</p>

<p id="u670c4ec7">穿过Redis 直接访问数据库</p>

<h2 id="%E5%8E%9F%E5%9B%A0">原因</h2>

<p id="u0d3107b1">1.黑客攻击</p>

<p id="u76005230">2.访问不存在的url</p>

<p></p>

<h2 id="u57756163">如何防止</h2>

<h3 id="u4035d35e">空值缓存</h3>

<p id="ufb089d60">如果一个查询返回的数据为空（不管是数据是否不存在），我们仍然把这个空结果（null）进行缓存，设置空结果的过期时间会很短，最长不超过五分钟。</p>

<p id="u324d95ff">假如传进来的这个不存在的Key值每次都是随机的，那存进Redis空值也没有意义。</p>

<p id="ua59d2318"></p>

<h3 id="u74624516">布隆过滤器</h3>

<p id="u91f76432">把有效的key经过k次hash的bitmap置位1</p>

<p id="uf6acb439">来了一个url 先进行 k次hash 如果有一次是0 说明它不是有效的 直接返回</p>

<p id="uada9a4d5">这样的话，如果全都是1，就访问redis ，当然也会有一定的误判率。</p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/e8ad56c95b9011ff2fe258386e33ffc1.png" /></p>

<h1 id="LXTa5">缓存击穿</h1>

<h2>原因</h2>

<p id="udce83cdb">一个热门key过期导致穿过Redis 直接访问数据库</p>

<p id="u93fb80af">key对应的数据存在，但在Redis中过期，此时若有大量并发请求过来，这些请求发现缓存过期一般都会从后端DB加载数据并回设到缓存，这个时候大并发的请求可能会瞬间把后端DB压垮。</p>

<p id="ud4e28855"><strong>和缓存穿透的区别是 <span style="color:#fe2c24;">这个key在数据库中有 Redis中也有 但是过期了</span></strong></p>

<h2 id="u68a55533">解决方法</h2>

<p id="u60049211"><strong>（1）预先设置热门数据：</strong>在Redis高峰访问之前，把一些热门数据提前存入到Redis里面，加大这些热门数据key的时长</p>

<p id="u5f0571d9"><strong>（2）实时调整：</strong>现场监控哪些数据热门，实时调整key的过期时长</p>

<p id="u241a90ed"><strong>（3）使用锁：</strong></p>

<p id="u3bc9fb94">1、上面说过了，如果业务允许的话，对于热点的key可以设置永不过期的key。</p>

<p id="u36c8009f">2、使用互斥锁。如果缓存失效的情况，只有拿到锁才可以查询数据库，降低了在同一时刻打在数据库上的请求，防止数据库打死。当然这样会导致系统的性能变差。</p>

<p id="u61e28b72"></p>

<h1 id="P3lFb">缓存雪崩</h1>

<h2>原因</h2>

<p id="u4966e116">大量key同时过期，造成数据库瘫痪。</p>

<p id="ua60d3830">为什么会出现这个问题呢，有几种可能，第一种可能是Redis宕机，第二种可能是采用了相同的过期时间。</p>

<h2 id="%E8%A7%A3%E5%86%B3%E6%8E%AA%E6%96%BD">解决措施</h2>

<p id="u24b135ef">（1） <strong>构建多级缓存架构：</strong>nginx缓存 + Redis缓存 +其他缓存（ehcache等）</p>

<p id="ua71756cd">（2） <strong>使用锁或队列：</strong></p>

<p id="u7b1d2c24">用加锁或者队列的方式保证来保证不会有大量的线程对数据库一次性进行读写，从而避免失效时大量的并发请求落到底层存储系统上。不适用高并发情况</p>

<p id="u3f543622">（3） <strong>设置过期标志更新缓存：</strong></p>

<p id="u84194949">记录缓存数据是否过期（设置提前量），如果过期会触发通知另外的线程在后台去更新实际key的缓存。</p>

<p id="ua57f191e">（4） <strong>将缓存失效时间分散开：</strong></p>

<p id="u2189cd7d">比如我们可以在原有的失效时间基础上增加一个随机值，比如1-5分钟随机，这样每一个缓存的过期时间的重复率就会降低，<strong>就很难引发集体失效的事件</strong>。</p>

<p id="u319b5221"></p>

<h1 id="Z9MIB">分布式锁</h1>

<p id="uc6ae6ed6">跨JVM进行锁的相关操作，适用于数据库集群</p>

<p id="u6cc440ec">好文：<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="一文搞懂分布式锁及其业务场景_chunqiu3351的博客-CSDN博客" href="https://blog.csdn.net/chunqiu3351/article/details/100762270" title="一文搞懂分布式锁及其业务场景_chunqiu3351的博客-CSDN博客">一文搞懂分布式锁及其业务场景_chunqiu3351的博客-CSDN博客</a></p>

<h2 id="v4gFQ">为啥用</h2>

<p id="u23480875">因为单机上锁其他java api也能识别，但是集群就不行了，</p>

<p id="ud99a33b2">所以需要实现分布式锁 让一台服务器上的锁 在别的服务器上也能识别到。</p>

<p id="u00690cee"></p>

<p id="uae970e0e">具体有几种方法</p>

<p id="uce9a848c">zookeeper</p>

<p id="u322e0968">Redis实现</p>

<p id="u13b6aa46">数据库实现</p>

<p id="u55f414aa"></p>

<p id="u85afede2">性能比较高的是Redis</p>

<p id="ua63c122c">1. 性能：Redis最高</p>

<p id="ue15bda1b">2. 可靠性：zookeeper最高</p>

<p id="u4c8cdd89"></p>

<h2 id="XO2BK">如何实现和优化</h2>

<p id="ua2507e93">Redis如何实现呢？</p>

<p id="u99ee9e74">set lock uuid nx ex 12</p>

<p id="u802991e8">设置lock 的值为uuid nx 表示为不允许再设置</p>

<p id="ubd065792">ex 表示过期时间 12s</p>

<p id="ufc456b2e">在java中也能实现</p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/79fa55af7bd034dbb24e85327ebba479.png" /></p>

<p id="u3fd829ba">比如在一台机子上 运行时 一个线程 在Redis中设置了 lock</p>

<p id="u9c9d7ee1">其他机子如果想要执行程序 但是无法拿到这个锁</p>

<p id="ud6bd99d3">set 成功就会返回1 不成功就会返回0</p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/32af75f073f9b2dcb15d9316a7897809.png" /></p>

<p id="u219cfc9c"></p>

<h2 id="zm9tH">问题：setnx刚好获取到锁，业务逻辑出现异常，导致锁无法释放</h2>

<p id="u2e52a453">解决：设置过期时间，自动释放锁。</p>

<p id="ub07f3b4b">所以必须设置过期时间 否则别的线程无法拿到锁了</p>

<p id="ud3e530c1"></p>

<h2 id="ZnTqc">问题：可能会释放其他服务器的锁。</h2>

<p id="u7864c80d">一台服务器做完需要同步的操作后就可以 del 锁了 让别的服务器去用</p>

<p id="ub8533826">所须在删除锁之前先进行判断</p>

<p id="u19a8d11b">通过 uuid进行锁的标识，看是不是自己的锁</p>

<p id="u7e18c3a5"></p>

<p id="udd96101a">如果是自己的锁 那么删除 别人在set lock 为自己的uuid</p>

<p id="udb4d9fc7">不是自己的 说明 已经过了过期时间 自动释放</p>

<p id="u6651377e">别的线程已经拿到了，那么就不用管了</p>

<p id="u66fbac43"></p>

<h2 id="soCOk">问题：删除操作缺乏原子性。</h2>

<p id="u5cd8871f">在判断是自己的uuid 后准备删除 这时候正好过期，被另一个线程拿到了这把锁。</p>

<p id="u10dc767e">也会误删 所以需要使得判断和删除时原子性的</p>

<p id="uab70088f">使用lua脚本保证删除操作的原子性。</p>

<p id="u641d231d"></p>

<p id="u6897d38b">为了确保分布式锁可用，我们至少要确保锁的实现同时<strong>满足以下四个条件</strong>：</p>

<p id="u6fe482d5">- 互斥性。在任意时刻，只有一个客户端能持有锁。</p>

<p id="udf697733">- 不会发生死锁。即使有一个客户端在持有锁的期间崩溃而没有主动解锁，也能保证后续其他客户端能加锁。</p>

<p id="u1315f2d0">- 解铃还须系铃人。加锁和解锁必须是同一个客户端，客户端自己不能把别人加的锁给解了。</p>

<p id="u70774176">- 加锁和解锁必须具有原子性。</p>

<p></p>

<h2 id="ua9bbaf02"><strong>最后这块很可能存在一种异常场景：</strong></h2>

<p id="u7a6a5945">实例 a 抢占到分布式锁，设置 value 及过期时长</p>

<p id="u815022c7">此时该 key-value 保存在 Redis master 节点，突然 master 节点挂了</p>

<p id="u703a84d6">Redis 集群从 slave 节点中推选出新的 master</p>

<p id="u3e5a5c5f">由于异步同步的原因，新的 master 此时不包含该分布式锁 key-value</p>

<p id="u686844bd">实例 b 抢占到分布式锁，设置 value 及过期时长</p>

<p id="uc8fea35b"><strong>此时实例 a 和 实例 b 都抢占到 Redis 锁，就有可能造成同步问题</strong></p>

<p id="u317bbf29">实际该问题的本质是<strong> Redis 异步同步可能丢数据的问题</strong>，只是用到分布式锁这个模块比较重要，影响范围更大。此时一般有两种解决方式：</p>

<p id="u326c6ae4">RedLock 获取分布式锁</p>

<p id="u0e17cf0e">Zookeeper 实现分布式锁</p>

<p id="u8265a52d">这里简单介绍下 RedLock 的逻辑：</p>

<p id="u6826e880">获取当前时间，以毫秒为单位</p>

<p id="u06fee741">客户端依次从5个毫无关系的 Redis 实例请求分布式锁</p>

<p id="ucf8d307d">通过当前时间减去步骤1的时间计算获取锁的时长，当且仅当 （N / 2 + 1）个 Redis 实例获取锁成功并且获取锁的时间小于锁失效的时间才算获取锁成功</p>

<p id="uc7ac98c2">此时如果获取锁成功，锁真正有效时间等于初始有效时间（根据步骤1时间 + 过期时长算出的时间）减去真正获取到锁的时间。如果获取锁失败执行解锁逻辑</p>

<p id="u6de2f396">总的来说 RedLock 实施难度太大，成本高是一方面，各个 Redis 实例时间还得对其，还得防止不出现重启等情况。一般很少使用，了解即可。</p>

<p id="u55c24a78"></p>

<h1 id="uaaa8eeaa">如果Redis内存满了的话会发生什么？</h1>

<p id="u2ca745c5">Redis所在服务器内存满了可能导致访问变慢，更严重的会直接宕机。</p>

<p id="u036cc1dd">如果不设置最大内存大小或者设置最大内存大小为0，在64位操作系统下不限制内存大小，在32位操作系统下最多使用3GB内存。</p>
