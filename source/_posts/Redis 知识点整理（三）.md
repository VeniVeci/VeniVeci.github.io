---
title: Redis 知识点整理（三）
tags: redis
categories: Redis 数据库
abbrlink: eeba2471
date: 2022-03-28 16:25:50
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="Redis%20%E5%92%8C%20Memcached%20%E7%9A%84%E5%8C%BA%E5%88%AB%E5%92%8C%E5%85%B1%E5%90%8C%E7%82%B9-toc" style="margin-left:0px;"><a href="#Redis%20%E5%92%8C%20Memcached%20%E7%9A%84%E5%8C%BA%E5%88%AB%E5%92%8C%E5%85%B1%E5%90%8C%E7%82%B9">Redis 和 Memcached 的区别和共同点</a></p>

<p id="%E5%85%B1%E5%90%8C%E7%82%B9%20%EF%BC%9A-toc" style="margin-left:40px;"><a href="#%E5%85%B1%E5%90%8C%E7%82%B9%20%EF%BC%9A">共同点 ：</a></p>

<p id="%E5%8C%BA%E5%88%AB%20%EF%BC%9A-toc" style="margin-left:40px;"><a href="#%E5%8C%BA%E5%88%AB%20%EF%BC%9A">区别 ：</a></p>

<p id="%E7%BC%93%E5%AD%98%E8%AE%BE%E7%BD%AE%E8%BF%87%E6%9C%9F%E6%97%B6%E9%97%B4-toc" style="margin-left:0px;"><a href="#%E7%BC%93%E5%AD%98%E8%AE%BE%E7%BD%AE%E8%BF%87%E6%9C%9F%E6%97%B6%E9%97%B4">缓存设置过期时间</a></p>

<p id="%E8%BF%87%E6%9C%9F%E7%9A%84%E6%95%B0%E6%8D%AE%E7%9A%84%E5%88%A0%E9%99%A4%E7%AD%96%E7%95%A5-toc" style="margin-left:0px;"><a href="#%E8%BF%87%E6%9C%9F%E7%9A%84%E6%95%B0%E6%8D%AE%E7%9A%84%E5%88%A0%E9%99%A4%E7%AD%96%E7%95%A5">过期的数据的删除策略</a></p>

<p id="Redis%20%E5%86%85%E5%AD%98%E6%B7%98%E6%B1%B0%E6%9C%BA%E5%88%B6-toc" style="margin-left:0px;"><a href="#Redis%20%E5%86%85%E5%AD%98%E6%B7%98%E6%B1%B0%E6%9C%BA%E5%88%B6">Redis 内存淘汰机制</a></p>

<hr id="hr-toc" /><p>参考：Javaguide 4</p>

<h1 id="Redis%20%E5%92%8C%20Memcached%20%E7%9A%84%E5%8C%BA%E5%88%AB%E5%92%8C%E5%85%B1%E5%90%8C%E7%82%B9"><span style="color:#242a31;"><strong>Redis </strong></span><span style="color:#242a31;"><strong>和</strong></span><span style="color:#242a31;"><strong> Memcached </strong></span><span style="color:#242a31;"><strong>的区别和共同点 </strong></span></h1>

<div></div>

<div><span style="color:#3b454e;">现在公司⼀般都是⽤</span><span style="color:#3b454e;"> Redis </span><span style="color:#3b454e;">来实现缓存，⽽且</span><span style="color:#3b454e;"> Redis </span><span style="color:#3b454e;">⾃身也越来越强⼤了！不过，了解</span><span style="color:#3b454e;"> Redis </span><span style="color:#3b454e;">和 </span></div>

<div><span style="color:#3b454e;">Memcached </span><span style="color:#3b454e;">的区别和共同点，有助于我们在做相应的技术选型的时候，能够做到有理有据！ </span></div>

<div></div>

<h2 id="%E5%85%B1%E5%90%8C%E7%82%B9%20%EF%BC%9A"><span style="color:#3b454e;"><strong>共同点 </strong></span><span style="color:#3b454e;">： </span></h2>

<div><span style="color:#3b454e;">1. </span><span style="color:#3b454e;">都是基于内存的数据库，⼀般都⽤来当做缓存使⽤。 </span></div>

<div><span style="color:#3b454e;">2. </span><span style="color:#3b454e;">都有过期策略。 </span></div>

<div><span style="color:#3b454e;">3. </span><span style="color:#3b454e;">两者的性能都⾮常⾼。 </span></div>

<h2 id="%E5%8C%BA%E5%88%AB%20%EF%BC%9A"><span style="color:#3b454e;"><strong>区别 </strong></span><span style="color:#3b454e;">： </span></h2>

<div><span style="color:#3b454e;">1. </span><span style="color:#3b454e;"><strong>Redis </strong></span><span style="color:#fe2c24;"><strong>⽀持更丰富的数据类型</strong></span><span style="color:#3b454e;"><strong>（⽀持更复杂的应⽤场景）</strong></span><span style="color:#3b454e;">。</span><span style="color:#3b454e;">Redis </span><span style="color:#3b454e;">不仅仅⽀持简单的</span><span style="color:#3b454e;"> k/v </span><span style="color:#3b454e;">类 </span></div>

<div><span style="color:#3b454e;">型的数据，同时还提供</span><span style="color:#3b454e;"> list</span><span style="color:#3b454e;">，</span><span style="color:#3b454e;">set</span><span style="color:#3b454e;">，</span><span style="color:#3b454e;">zset</span><span style="color:#3b454e;">，</span><span style="color:#3b454e;">hash </span><span style="color:#3b454e;">等数据结构的存储。</span><span style="color:#3b454e;">Memcached </span><span style="color:#3b454e;">只⽀持最简 </span></div>

<div><span style="color:#3b454e;">单的</span><span style="color:#3b454e;"> k/v </span><span style="color:#3b454e;">数据类型。 </span></div>

<div><span style="color:#3b454e;">2. </span><span style="color:#3b454e;"><strong>Redis </strong></span><span style="color:#fe2c24;"><strong>⽀持数据的持久化</strong></span><span style="color:#3b454e;"><strong>，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进 </strong></span></div>

<div><span style="color:#3b454e;"><strong>⾏使⽤</strong></span><span style="color:#3b454e;"><strong>,</strong></span><span style="color:#3b454e;"><strong>⽽</strong></span><span style="color:#3b454e;"><strong> Memecache </strong></span><span style="color:#3b454e;"><strong>把数据全部存在内存之中。 </strong></span></div>

<div><span style="color:#3b454e;">3. </span><span style="color:#3b454e;"><strong>Redis </strong></span><span style="color:#fe2c24;"><strong>有灾难恢复机制</strong></span><span style="color:#3b454e;"><strong>。 </strong></span><span style="color:#3b454e;">因为可以把缓存中的数据持久化到磁盘上。 </span></div>

<div><span style="color:#3b454e;">4. </span><span style="color:#3b454e;"><strong>Redis </strong></span><span style="color:#3b454e;"><strong>在服务器内存使⽤完之后，可以将不⽤的数据放到磁盘上。但是，</strong></span><span style="color:#3b454e;"><strong>Memcached </strong></span><span style="color:#3b454e;"><strong>在服 </strong></span></div>

<div><span style="color:#3b454e;"><strong>务器内存使⽤完之后，就会直接报异常。 </strong></span></div>

<div><span style="color:#3b454e;">5. </span><span style="color:#3b454e;"><strong>Memcached </strong></span><span style="color:#3b454e;"><strong>没有原⽣的集群模式，需要依靠客户端来实现往集群中分⽚写⼊数据；但是 </strong></span></div>

<div><span style="color:#3b454e;"><strong>Redis </strong></span><span style="color:#3b454e;"><strong>⽬前是原⽣⽀持</strong></span><span style="color:#3b454e;"><strong> cluster </strong></span><span style="color:#3b454e;"><strong>模式的</strong></span><span style="color:#3b454e;"><strong>. </strong></span></div>

<div><span style="color:#3b454e;">6. </span><span style="color:#3b454e;"><strong>Memcached </strong></span><span style="color:#3b454e;"><strong>是多线程，⾮阻塞</strong></span><span style="color:#3b454e;"><strong> IO </strong></span><span style="color:#3b454e;"><strong>复⽤的⽹络模型；</strong></span><span style="color:#3b454e;"><strong>Redis </strong></span><span style="color:#3b454e;"><strong>使⽤</strong></span><span style="color:#fe2c24;"><strong>单线程的多路 IO 复⽤模 </strong></span></div>

<div><span style="color:#fe2c24;"><strong>型</strong></span><span style="color:#3b454e;"><strong>。 </strong></span><span style="color:#3b454e;">（</span><span style="color:#3b454e;">Redis 6.0 </span><span style="color:#3b454e;">引⼊了多线程</span><span style="color:#3b454e;"> IO </span><span style="color:#3b454e;">） </span></div>

<p> 
</p><div><span style="color:#3b454e;">7. </span><span style="color:#3b454e;"><strong>Memcached</strong></span><span style="color:#3b454e;"><strong>过期数据的删除策略只⽤了惰性删除，⽽</strong></span><span style="color:#3b454e;"><strong> Redis </strong></span><span style="color:#3b454e;"><strong>同时使⽤了惰性删除与定期删删除</strong></span></div>


<div></div>

<div></div>

<div></div>

<h1 id="%E7%BC%93%E5%AD%98%E8%AE%BE%E7%BD%AE%E8%BF%87%E6%9C%9F%E6%97%B6%E9%97%B4">缓存设置过期时间</h1>

<div>
<div><span style="color:#3b454e;">注意：</span><span style="color:#3b454e;"><strong>Redis</strong></span><span style="color:#3b454e;"><strong>中除了字符串类型有⾃⼰独有设置过期时间的命令 </strong></span><span style="color:#3b454e;"><strong>setex </strong></span><span style="color:#3b454e;"><strong>外，其他⽅法都需要依靠 </strong></span></div>

<div><span style="color:#3b454e;"><strong>expire </strong></span><span style="color:#3b454e;"><strong>命令来设置过期时间 。另外， </strong></span><span style="color:#3b454e;"><strong>persist </strong></span><span style="color:#3b454e;"><strong>命令可以移除⼀个键的过期时间： </strong></span></div>

<div><span style="color:#3b454e;"><strong>过期时间除了有助于缓解内存的消耗，还有什么其他⽤么？ </strong></span></div>

<div><span style="color:#3b454e;">很多时候，我们的业务场景就是需要某个数据只在某⼀时间段内存在，⽐如我们的短信验证码可 </span></div>

<div><span style="color:#3b454e;">能只在</span><span style="color:#3b454e;">1</span><span style="color:#3b454e;">分钟内有效，⽤户登录的</span><span style="color:#3b454e;"> token </span><span style="color:#3b454e;">可能只在</span><span style="color:#3b454e;"> 1 </span><span style="color:#3b454e;">天内有效。 </span></div>

<div><span style="color:#3b454e;">如果使⽤传统的数据库来处理的话，⼀般都是⾃⼰判断过期，这样更麻烦并且性能要差很多。</span></div>
</div>

<div></div>

<div></div>

<div>
<h1 id="%E8%BF%87%E6%9C%9F%E7%9A%84%E6%95%B0%E6%8D%AE%E7%9A%84%E5%88%A0%E9%99%A4%E7%AD%96%E7%95%A5"><span style="color:#242a31;"><strong>过期的数据的删除策略</strong></span></h1>

<div><span style="color:#3b454e;">1. </span><span style="color:#3b454e;"><strong>惰性删除 </strong></span><span style="color:#3b454e;">：只会在取出</span><span style="color:#3b454e;">key</span><span style="color:#3b454e;">的时候才对数据进⾏过期检查。这样对</span><span style="color:#3b454e;">CPU</span><span style="color:#3b454e;">最友好，但是可能会 </span></div>

<div><span style="color:#3b454e;">造成太多过期</span><span style="color:#3b454e;"> key </span><span style="color:#3b454e;">没有被删除。 </span></div>

<div><span style="color:#3b454e;">2. </span><span style="color:#3b454e;"><strong>定期删除 </strong></span><span style="color:#3b454e;">： 每隔⼀段时间抽取⼀批</span><span style="color:#3b454e;"> key </span><span style="color:#3b454e;">执⾏删除过期</span><span style="color:#3b454e;">key</span><span style="color:#3b454e;">操作。并且，</span><span style="color:#3b454e;">Redis </span><span style="color:#3b454e;">底层会通过限 </span></div>

<div><span style="color:#3b454e;">制删除操作执⾏的时⻓和频率来减少删除操作对</span><span style="color:#3b454e;">CPU</span><span style="color:#3b454e;">时间的影响。</span></div>

<div>
<div><span style="color:#3b454e;">定期删除对内存更加友好，惰性删除对</span><span style="color:#3b454e;">CPU</span><span style="color:#3b454e;">更加友好。两者各有千秋，所以</span><span style="color:#3b454e;">Redis </span><span style="color:#3b454e;">采⽤的是</span><span style="color:#fe2c24;"> <strong>定期 </strong></span></div>

<div><span style="color:#fe2c24;"><strong>删除+惰性/懒汉式删除 </strong>。</span></div>

<div></div>

<div>
<h1 id="Redis%20%E5%86%85%E5%AD%98%E6%B7%98%E6%B1%B0%E6%9C%BA%E5%88%B6"><span style="color:#242a31;"><strong>Redis </strong></span><span style="color:#242a31;"><strong>内存淘汰机制</strong></span></h1>

<div><span style="color:#3b454e;">Redis </span><span style="color:#3b454e;">提供</span><span style="color:#3b454e;"> 8 </span><span style="color:#3b454e;">种数据淘汰策略</span></div>

<div><span style="color:#fe2c24;">两种LRU 不同是删除数据的集合不同 第一种是已设置过期时间的数据集</span></div>

<div><span style="color:#fe2c24;">第二种是当前的键空间，包含所有的键。</span></div>

<div></div>

<div>两种LFU的区别和LRU一样。</div>

<div></div>

<div></div>

<div>
<div><span style="color:#3b454e;">1. </span><span style="color:#3b454e;"><strong>volatile-lru</strong></span><span style="color:#3b454e;"><strong>（</strong></span><span style="color:#3b454e;"><strong>least recently used</strong></span><span style="color:#3b454e;"><strong>）</strong></span><span style="color:#3b454e;">：从已设置过期时间的数据集（</span><span style="color:#3b454e;">server.db[i].expires</span><span style="color:#3b454e;">） </span></div>

<div><span style="color:#3b454e;">中挑选最近最少使⽤的数据淘汰 </span></div>

<div><span style="color:#3b454e;">2. </span><span style="color:#3b454e;"><strong>volatile-ttl</strong></span><span style="color:#3b454e;">：从已设置过期时间的数据集（</span><span style="color:#3b454e;">server.db[i].expires</span><span style="color:#3b454e;">）中挑选将要过期的数据淘汰 </span></div>

<div><span style="color:#3b454e;">3. </span><span style="color:#3b454e;"><strong>volatile-random</strong></span><span style="color:#3b454e;">：从已设置过期时间的数据集（</span><span style="color:#3b454e;">server.db[i].expires</span><span style="color:#3b454e;">）中任意选择数据淘汰 </span></div>

<div><span style="color:#fe2c24;">4. <strong>allkeys-lru（least recently used）</strong>：当内存不⾜以容纳新写⼊数据时，在键空间中，移除 </span></div>

<div><span style="color:#fe2c24;">最近最少使⽤的 key（这个是最常⽤的） </span></div>

<div><span style="color:#3b454e;">5. </span><span style="color:#3b454e;"><strong>allkeys-random</strong></span><span style="color:#3b454e;">：从数据集（</span><span style="color:#3b454e;">server.db[i].dict</span><span style="color:#3b454e;">）中任意选择数据淘汰 </span></div>

<div><span style="color:#3b454e;">6. </span><span style="color:#3b454e;"><strong>no-eviction</strong></span><span style="color:#3b454e;">：禁⽌驱逐数据，也就是说当内存不⾜以容纳新写⼊数据时，新写⼊操作会报 </span></div>

<div><span style="color:#3b454e;">错。</span></div>

<div>
<div><span style="color:#3b454e;">7. </span><span style="color:#3b454e;"><strong>volatile-lfu</strong></span><span style="color:#3b454e;"><strong>（</strong></span><span style="color:#3b454e;"><strong>least frequently used</strong></span><span style="color:#3b454e;"><strong>）</strong></span><span style="color:#3b454e;">：从已设置过期时间的数据集</span><span style="color:#3b454e;">(server.db[i].expires)</span><span style="color:#3b454e;">中 </span></div>

<div><span style="color:#3b454e;">挑选最不经常使⽤的数据淘汰 </span></div>

<div><span style="color:#3b454e;">8. </span><span style="color:#3b454e;"><strong>allkeys-lfu</strong></span><span style="color:#3b454e;"><strong>（</strong></span><span style="color:#3b454e;"><strong>least frequently used</strong></span><span style="color:#3b454e;"><strong>）</strong></span><span style="color:#3b454e;">：当内存不⾜以容纳新写⼊数据时，在键空间中，移 </span></div>

<div><span style="color:#3b454e;">除最不经常使⽤的</span><span style="color:#3b454e;"> key</span></div>

<div></div>
</div>
</div>
</div>
</div>
</div>
