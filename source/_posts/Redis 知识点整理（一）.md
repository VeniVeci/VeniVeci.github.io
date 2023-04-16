---
title: Redis 知识点整理（一）
tags: redis nosql 数据库
categories: Redis 数据库
abbrlink: 93b26bfb
date: 2022-02-28 21:39:02
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="nt16i-toc" style="margin-left:0px;"><a href="#nt16i">发展简史</a></p>

<p id="oiVxa-toc" style="margin-left:0px;"><a href="#oiVxa">什么是NoSQL？</a></p>

<p id="qu7Zb-toc" style="margin-left:0px;"><a href="#qu7Zb">Redis应用场景</a></p>

<p id="iaZ1M-toc" style="margin-left:0px;"><a href="#iaZ1M">通用命令</a></p>

<p id="zv9eX-toc" style="margin-left:0px;"><a href="#zv9eX">数据类型</a></p>

<p id="skfby-toc" style="margin-left:40px;"><a href="#skfby">string</a></p>

<p id="VbXCM-toc" style="margin-left:40px;"><a href="#VbXCM">列表(List)</a></p>

<p id="mjF7J-toc" style="margin-left:40px;"><a href="#mjF7J">集合(Set)</a></p>

<p id="qA2J5-toc" style="margin-left:40px;"><a href="#qA2J5">哈希 hash</a></p>

<p id="HEp6P-toc" style="margin-left:40px;"><a href="#HEp6P">有序集合zset</a></p>

<p id="NTMvK-toc" style="margin-left:40px;"><a href="#NTMvK">Pub/Sub</a></p>

<p id="YZxz4-toc" style="margin-left:0px;"><a href="#YZxz4">springboot整合</a></p>

<p id="efUpX-toc" style="margin-left:0px;"><a href="#efUpX">Redis在网络io方面的优化你了解吗？使用单线程的话它需要去避免一些怎样的场景（单线程会导致哪些问题）</a></p>

<p id="T2jut-toc" style="margin-left:0px;"><a href="#T2jut">事务的机制</a></p>

<p id="I3vRU-toc" style="margin-left:0px;"><a href="#I3vRU">Redis配置</a></p>

<p id="xirv1-toc" style="margin-left:0px;"><a href="#xirv1">Redis和memcache的区别</a></p>

<p id="u65744e48-toc" style="margin-left:40px;"><a href="#u65744e48">三点不同</a></p>

<hr id="hr-toc" /><p>Redis的由来，特性，重要的5种数据结构（底层实现，应用场景）等。</p>

<p></p>

<p><img alt="" height="374" src="https://img-blog.csdnimg.cn/170e721075794059bb4fbaede19041ad.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_20,color_FFFFFF,t_70,g_se,x_16" width="1041" /></p>

<p> Redis是一种NoSQL，性能相较于传统的SQL数据库有很大提升。</p>

<p></p>

<h1 id="nt16i">发展简史</h1>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/34ac3a60eeba12f474f8ddf473969151.png" /></p>

<p>新的情景，公司有多台服务器，一个用户第一次请求是在服务器A上，第二次路由到了服务器B，那么服务器B如何知道这个用户是否已经登录及其附属的信息？</p>

<p>利用缓存即可，<strong>Redis就可以解决这类问题</strong>。可以大大减少IO的压力。</p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/88b46e93b7aa277cc902624bb372c1cd.png" /></p>

<p id="u2bbec254"></p>

<h1 id="oiVxa">什么是NoSQL？</h1>

<p id="u05a03998">NoSQL(NoSQL = <strong>Not Only SQL</strong> )，意即“不仅仅是SQL”，泛指<strong>非关系型的数据库</strong>。</p>

<p id="uc80500a7">NoSQL 不依赖业务逻辑方式存储，而以简单的key-value模式存储。因此大大的增加了数据库的扩展能力。</p>

<p id="ua000183e">l 不遵循SQL标准。</p>

<p id="u5408d135">l 不支持<strong>ACID</strong>。支持事务</p>

<p id="u1aabbd1d">l 远超于SQL的性能。</p>

<p id="ubebffcae"></p>

<p id="u7986f146"><strong>NoSQL适用场景 </strong></p>

<p id="u115a96a9">l 对数据高并发的读写</p>

<p id="ucb824a73">l 海量数据的读写</p>

<p id="u25ef62c7">l 对数据高可扩展性的</p>

<p></p>

<p id="ud0a53d8c"><strong>NoSQL不适用场景 缺点</strong></p>

<p id="u81054702">l 需要事务支持</p>

<p id="ud103b558">l 基于sql的结构化查询存储，处理复杂的关系,需要即席查询。</p>

<p id="u0676b299"></p>

<h1 id="qu7Zb">Redis应用场景</h1>

<p id="ucd144bc5"><strong>配合关系型数据库做高速缓存</strong></p>

<p id="ua6b519ba">Ø 高频次，<strong>热门访问的数据，降低数据库IO</strong></p>

<p id="u0ed94e1c">Ø 分布式架构，<strong>做session共享</strong></p>

<p>做缓存、或者分布式锁，也可以来设计消息队列，同时还支持<strong>持久化、Lua 脚本、多种集群方案。</strong></p>

<p></p>

<p id="u3161dbc9"><strong>多样的数据结构存储持久化数据</strong></p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/6221e70513e8721be6662b071f8b48a9.png" /></p>

<p id="u4cef0c3c">MySQL数据库的端口号是3306，Redis是6379，6379在是手机按键上MERZ对应的号码，而MERZ取自意大利歌女Alessia Merz的名字。MERZ长期以来被Redis作者antirez及其朋友当作愚蠢的代名词。后来Redis作者在开发Redis时就选用了这个端口。</p>

<p id="ue3bc0e52"></p>

<h1 id="iaZ1M">通用命令</h1>

<p id="u0a988a86">keys *查看当前库所有key (匹配：keys *1)</p>

<p id="u730fd7c0">exists key判断某个key是否存在</p>

<p id="ubfca3be7">type key 查看你的key是什么类型</p>

<p id="u41835eaa">del key 删除指定的key数据</p>

<p id="ubac293f5">unlink key 根据value选择非阻塞删除</p>

<p id="u56ff0eb0">仅将keys从keyspace元数据中删除，真正的删除会在后续异步操作。</p>

<p id="ua94794e2">expire key 10 10秒钟：为给定的key设置过期时间</p>

<p id="ucef56261">ttl key 查看还有多少秒过期，-1表示永不过期，-2表示已过期</p>

<p id="ue830bab6"></p>

<p id="ubf98a0ed">select命令切换数据库</p>

<p id="u03c69a2f">dbsize查看当前数据库的key的数量</p>

<p id="ue87ef8b7">flushdb清空当前库</p>

<p id="ueadd9389">flushall通杀全部库</p>

<p></p>

<h1 id="zv9eX">数据类型</h1>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="Redis 键(key) | 菜鸟教程" href="https://www.runoob.com/redis/redis-keys.html" title="Redis 键(key) | 菜鸟教程">Redis 键(key) | 菜鸟教程</a></p>

<p id="u1526354f">Redis的这几种数据类型分别都适用于哪些场景?</p>

<p id="u859c8e6a"><a class="has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="Redis 性能优化思路，写的非常好！_emprere的博客-CSDN博客" href="https://blog.csdn.net/emprere/article/details/115343900" title="Redis 性能优化思路，写的非常好！_emprere的博客-CSDN博客"><span class="link-card-box"><span class="link-title">Redis 性能优化思路，写的非常好！_emprere的博客-CSDN博客</span><span class="link-link"><img alt="" class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" />https://blog.csdn.net/emprere/article/details/115343900</span></span></a></p>

<h2 id="skfby">string</h2>

<p id="uc652b9ef">String的数据结构为简单动态字符串(Simple Dynamic String,缩写SDS)。是<strong>可以修改的字符串</strong>，内部结构实现上类似于Java的ArrayList，采用预分配冗余空间的方式来减少内存的频繁分配.</p>

<p id="u44bc64dc">如图中所示，内部为当前字符串实际分配的空间capacity一般要高于实际字符串长度len。当字符串长度小于1M时，扩容都是加倍现有的空间，如果超过1M，扩容时一次只会多扩1M的空间。<strong>需要注意的是字符串最大长度为512M。</strong></p>

<p id="ucdb3f355">Strings 数据结构是简单的key-value类型，value其实不仅是String，也可以是数字.</p>

<p id="u037ec877">常用命令: set,get,decr,incr,mget 等。</p>

<p id="ufd162497"><strong>应用场景：</strong>String是最常用的一种数据类型，普通的key/ value 存储都可以归为此类.即可以完全实现目前 Memcached 的功能，并且效率更高。还可以享受Redis的定时持久化，操作日志及 Replication等功能,提供与 Memcached 一样的get、set、incr、decr 等操作.</p>

<p id="u5f6e7ac8"><strong>实现方式：</strong>String在redis内部存储<strong>默认就是一个字符串，被redisObject所引用</strong>，当遇到incr,decr等操作时会转成数值型进行计算，此时redisObject的encoding字段为int。</p>

<p id="u97ed7dcf"></p>

<h2 id="VbXCM">列表(List)</h2>

<p id="u8ff63a94">lpush/rpush &lt;key&gt;&lt;value1&gt;&lt;value2&gt;&lt;value3&gt; .... 从左边/右边插入一个或多个值。</p>

<p id="u0bacaaef">lpop/rpop &lt;key&gt;从左边/右边吐出一个值。值在键在，值光键亡。</p>

<p id="u672c71cd"></p>

<p id="u882b2eaf">它的底层实际是个<strong>双向链表，对两端的操作性能很高</strong>，通过索引下标的操作中间的节点性能会较差。</p>

<p id="u0af63b9f">List的数据结构为快速链表quickList。</p>

<p id="u84404b88">首先在列表元素较少的情况下会使用一块连续的内存存储，这个结构是ziplist，也即是压缩列表。</p>

<p id="u39871ece">它将所有的元素紧挨着一起存储，分配的是一块连续的内存。</p>

<p id="u443ee51c">当数据量比较多的时候才会改成quicklist。</p>

<p id="u4f2d2914">因为普通的链表需要的附加指针空间太大，会比较浪费空间。比如这个列表里存的只是int类型的数据，结构上还需要两个额外的指针prev和next。</p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/8edf92212ab936d20d63b24d2dcb9195.png" /></p>

<p id="u5514296b">Redis将链表和ziplist结合起来组成了quicklist。也就是将多个ziplist使用双向指针串起来使用。这样既满足了快速的插入删除性能，又不会出现太大的空间冗余。</p>

<p id="udee4f9e2"><strong>应用场景：</strong></p>

<p id="uc3b83d93"><strong>Redis list的应用场景非常多，也是Redis最重要的数据结构之一，比如<span style="color:#ff9900;">twitter的关注列表</span>，粉丝列表等都可以用Redis的list结构来实现。</strong></p>

<p id="ud8f14401">Lists 就是链表，相信略有数据结构知识的人都应该能理解其结构。使用Lists结构，我们可以轻松地实现最新消息排行等功能。Lists的另一个应用就是消息队列，<br />
可以利用Lists的PUSH操作，将任务存在Lists中，然后工作线程再用POP操作将任务取出进行执行。Redis还提供了操作Lists中某一段的api，你可以直接查询，删除Lists中某一段的元素。</p>

<p id="u39fbc701"><strong>实现方式：</strong></p>

<p id="u6f2f7b40">Redis list的实现为一个双向链表，即可以支持反向查找和遍历，更方便操作，不过带来了部分额外的内存开销，Redis内部的很多实现，包括发送缓冲队列等也都是用的这个数据结构。</p>

<p id="u928df519"></p>

<h2 id="mjF7J">集合(Set)</h2>

<p id="u4a1d8cc7"><strong>常用命令：</strong></p>

<p id="u51569117">sadd,spop,smembers,sunion 等。</p>

<p id="u1d3270b6">Redis的Set是string类型的无序集合。它底层其实是一个value为null的hash表，所以添加，删除，查找的<strong>复杂度都是O(1)</strong>。</p>

<p id="u0ef1691b">Set数据结构是dict字典，字典是用哈希表实现的。</p>

<p id="u583a9897">Java中HashSet的内部实现使用的是HashMap，只不过所有的value都指向同一个对象。Redis的set结构也是一样，它的内部也使用hash结构，所有的value都指向同一个内部值。</p>

<p id="u3106e082">利用Redis提供的Sets数据结构，可以存储一些集合性的数据，比如<span style="color:#fe2c24;"><strong>在微博应用中，可以将一个用户所有的关注人存在一个集合中，将其所有粉丝存在一个集合。Redis还为集合提供了求交集、并集、差集等操作，可以非常方便的实现如共同关注、共同喜好、二度好友等功能</strong></span>，对上面的所有集合操作，你还可以使用不同的命令选择将结果返回给客户端还是存集到一个新的集合中。</p>

<p></p>

<h2 id="qA2J5">哈希 hash</h2>

<p id="u84f78d1e">Redis hash 是一个键值对集合。</p>

<p id="u7ba3f0ef">Redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象。</p>

<p id="u1f91033b">Hash类型对应的数据结构是两种：</p>

<p id="u4845ff23"><strong>ziplist（压缩列表），hashtable（哈希表）。</strong></p>

<p id="u367903e4"><strong>当field-value长度较短且个数较少时，使用ziplist，否则使用hashtable。</strong></p>

<p id="uef48c03a"><strong>常用命令：</strong>hget,hset,hgetall 等。</p>

<p id="u257b6c1c"><strong>应用场景：</strong>在Memcached中，我们经常将一些结构化的信息打包成HashMap，在客户端序列化后存储为一个字符串的值，比如用户的昵称、年龄、性别、积分等，这时候在需要修改其中某一项时，通常需要将所有值取出反序列化后，修改某一项的值，再序列化存储回去。<strong>这样不仅增大了开销，也不适用于一些可能并发操作的场合</strong>（比如两个并发的操作都需要修改积分）。而Redis的Hash结构可以使你像在数据库中Update一个属性一样只修改某一项属性值。</p>

<p id="uf1585b32">我们简单举个实例来描述下Hash的应用场景，比如我们要存储一个用户信息对象数据，包含以下信息：</p>

<p id="u61ee917d">用户ID为查找的key，存储的value用户对象包含姓名，年龄，生日等信息，如果用普通的key/value结构来存储，主要有以下2种存储方式：</p>

<p id="ucafbd2a2"><strong>第一种方式</strong>将用户ID作为查找key,把其他信息封装成一个对象以序列化的方式存储，这种方式的缺点是，增加了序列化/反序列化的开销，并且在需要修改其中一项信息时，需要把整个对象取回，并且修改操作需要对并发进行保护，引入CAS等复杂问题。</p>

<p id="ue3337455"><strong>第二种方法</strong>是这个用户信息对象有多少成员就存成多少个key-value对儿，用用户ID+对应属性的名称作为唯一标识来取得对应属性的值，虽然省去了序列化开销和并发问题，但是用户ID为重复存储，如果存在大量这样的数据，内存浪费还是非常可观的。</p>

<p id="u01cf6535">那么Redis提供的Hash很好的解决了这个问题，Redis的Hash实际是内部存储的Value为一个HashMap，并提供了直接存取这个Map成员的接口.</p>

<p id="u62969192">也就是说，Key仍然是用户ID, value是一个Map，这个Map的key是成员的属性名，value是属性值，这样对数据的修改和存取都可以直接通过其内部Map的Key(Redis里称内部Map的key为field), 也就是通过 key(用户ID) + field(属性标签) 就可以操作对应属性数据了，既不需要重复存储数据，也不会带来序列化和并发修改控制的问题。很好的解决了问题。</p>

<p id="u3d33f476">这里同时需要注意，Redis提供了接口(hgetall)可以直接取到全部的属性数据,但是如果内部Map的成员很多，那么涉及到遍历整个内部Map的操作，由于Redis单线程模型的缘故，这个遍历操作可能会比较耗时，而另其它客户端的请求完全不响应，这点需要格外注意。</p>

<p id="u4ec116cc"><strong>实现方式：</strong></p>

<p id="u9a7462eb">上面已经说到Redis Hash对应Value内部实际就是一个HashMap，实际这里会有2种不同实现，这个Hash的成员比较少时Redis为了节省内存会采用类似一维数组的方式来紧凑存储，而不会采用真正的HashMap结构，对应的value redisObject的encoding为zipmap,当成员数量增大时会自动转成真正的HashMap,此时encoding为ht。</p>

<p id="u84bb8183"><img alt="" height="416" src="https://img-blog.csdnimg.cn/d8d4352132eb47e38943955d84d95698.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="716" /></p>

<p></p>

<h2 id="HEp6P">有序集合zset</h2>

<p id="uaedb1d5f"><strong>常用命令：</strong></p>

<p id="u65150199">zadd,zrange,zrem,zcard等</p>

<p id="u89107899">SortedSet(zset)是Redis提供的一个非常特别的数据结构，一方面它等价于Java的数据结构<strong>Map&lt;String, Double&gt;</strong>，可以给每一个元素value赋予一个权重score，另一方面它又类似于TreeSet，内部的元素会按照权重score进行排序，可以得到每个元素的名次，还可以通过score的<strong>范围来获取元素的列表。</strong></p>

<p id="ucee06a4b">zset底层使用了两个数据结构</p>

<p id="u18189a1e">（1）hash，hash的作用就是关联元素value和权重score，保障元素value的唯一性，可以通过元素value找到相应的score值。</p>

<p id="u78620cd3">（2）跳跃表，跳跃表的目的在于给元素value排序，根据score的范围获取元素列表。</p>

<p id="u8de7e804"></p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/9c7c6c940b8496c372d4eeabe9e9b438.png" /></p>

<h2 id="NTMvK">Pub/Sub</h2>

<p id="uead3f5c0">Pub/Sub 从字面上理解就是发布（Publish）与订阅（Subscribe），在Redis中，你可以设定对某一个key值进行消息发布及消息订阅，当一个key值上进行了消息发布后，所有订阅它的客户端都会收到相应的消息。这一功能最明显的用法就是用作实时消息系统，比如普通的即时聊天，群聊等功能。</p>

<p></p>

<h1 id="YZxz4">springboot整合</h1>

<p><strong>三步走</strong></p>

<p id="u6b5a4955">引入依赖，编写redis配置类，使用相应的对象即可调用。</p>

<p>redis相应的包</p>

<p><img alt="" height="202" src="https://img-blog.csdnimg.cn/329a55475bdd4caca04a06dc7d43c794.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="660" /></p>

<p></p>

<p></p>

<h1 id="efUpX">Redis在网络io方面的优化你了解吗？使用单线程的话它需要去避免一些怎样的场景（单线程会导致哪些问题）</h1>

<p id="u2bd16c8d">单线程+多路复用</p>

<p id="ua963fb2b"><strong>单线程的问题 任务耗时较长 影响其他任务的运行</strong></p>

<p id="u3686fd88">Redis 大多数时候是单线程运行的（single-threaded)，即同一时间只占用一个 CPU，只能有一个指令在运行，并行读写是不存在的。很多操作带来的延迟问题，都可以在这里找到答案。<br /><br />
关于最后这个特性，为什么 Redis 是单线程的，却能有很好的性能:</p>

<p id="uf2472889">两句话概括是：Redis <strong>利用了多路 I/O 复用机制</strong>，处理客户端请求时，不会阻塞主线程；</p>

<p id="u4f1498c7">Redis 单纯执行（大多数指令）一个指令不到 1 微秒，如此，单核 CPU 一秒就能处理 1 百万个指令（大概对应着<strong>几十万个请求</strong>），用不着实现多线程（网络才是瓶颈）。</p>

<p id="u08d2354b"></p>

<h1 id="T2jut">事务的机制</h1>

<p id="ufd5743e2">Redis事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。</p>

<p id="ucaf9ef69">Redis事务的主要作用就是串联多个命令防止别的命令插队。</p>

<p id="u70952661">组队中某个命令出现了报告错误，执行时整个的所有队列都会被取消。</p>

<p id="uaad2db20">如果执行阶段某个命令报出了错误，则只有报错的命令不会被执行，而其他的命令都会执行，<strong>不会回滚。</strong></p>

<p id="u14b37d74"></p>

<h1 id="I3vRU">Redis配置</h1>

<p id="u59837c21">Warning: no config file specified, using the default config. In order to specify a config file use D:\JAVA\Redis\Redis-server.exe /path/to/Redis.conf</p>

<p id="u677c3211"><span style="color:#fe2c24;"><strong>注意：直接点击exe 没办法加载自定义的conf</strong></span></p>

<p id="ua988a6b2">需要在命令行中指定conf文件，比如</p>

<p id="u6efe185d">Redis-server.exe Redis.windows.conf</p>

<p id="u94615ccb">启动指定的配置文件</p>

<p id="u752a7bd0">Redis-server --service-start Redis6379.conf</p>

<p></p>

<h1 id="xirv1">Redis和memcache的区别</h1>

<p id="ub768b06c">Redis是单线程 +多路io复用</p>

<p id="ube4a8dd8">memcache是多线程+锁</p>

<h2 id="u65744e48"><strong>三点不同</strong></h2>

<p id="u92e692e6">支持<strong>多数据类型</strong>，支持<strong>持久化</strong>，单线程+多路IO复用</p>

<p id="u527a6f36">Redis单命令的原子性主要得益于Redis的单线程。</p>

<p id="u8ff0d82a"></p>

<p id="u7b3cd54a">1、Redis和Memcache都是将数据存放在内存中，都是内存数据库。不过memcache还可用于缓存<strong>其他东西，例如图片、视频等等</strong>；</p>

<p id="ue4ffeff0">2、Redis不仅仅支持简单的k/v类型的数据，同时还提供list，set，hash等数据结构的存储；</p>

<p id="u9307eaad">3、<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="虚拟内存" href="https://www.baidu.com/s?wd=%E8%99%9A%E6%8B%9F%E5%86%85%E5%AD%98&amp;tn=44039180_cpr&amp;fenlei=mv6quAkxTZn0IZRqIHcvrjTdrH00T1Y4rjDYnju-njb1nycLmW-b0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EnHbzPHfYP10LPjDznWnkn1T3Ps" title="虚拟内存">虚拟内存</a>--Redis当物理内存用完时，可以将一些很久没用到的value 交换到磁盘；</p>

<p id="u1547cfe6">4、过期策略--memcache在set时就指定，例如set key1 0 0 8,即永不过期。Redis可以通过例如expire 设定，例如expire name 10；</p>

<p id="u8a67dde5">5、分布式--设定memcache集群，利用magent做一主多从;Redis可以做一主多从。都可以一主一从；</p>

<p id="u2e97c02b">6、存储数据安全--memcache挂掉后，数据没了；Redis可以定期保存到磁盘（持久化）；</p>

<p id="u13f4dcb3">7、<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="灾难恢复" href="https://www.baidu.com/s?wd=%E7%81%BE%E9%9A%BE%E6%81%A2%E5%A4%8D&amp;tn=44039180_cpr&amp;fenlei=mv6quAkxTZn0IZRqIHcvrjTdrH00T1Y4rjDYnju-njb1nycLmW-b0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EnHbzPHfYP10LPjDznWnkn1T3Ps" title="灾难恢复">灾难恢复</a>--memcache挂掉后，<strong>数据不可恢复</strong>; Redis数据丢失后可以<strong>通过aof/rdb恢复</strong>；</p>

<p id="uf2b1450b">8、Redis支持数据的备份，即master-slave模式的数据备份；</p>

<p></p>

<p>主要内容来自 尚硅谷。</p>
