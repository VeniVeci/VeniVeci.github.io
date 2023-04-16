---
title: 分布式多个机器生成id，如何保证不重复?
tags: 分布式
categories: 分布式系统设计
abbrlink: dc6b09c8
date: 2022-04-02 18:29:21
---

<!--more-->

<p>参考：<br /><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="分布式多个机器生成id，如何保证不重复? - 简书" href="https://www.jianshu.com/p/2e7d8de2c90d" title="分布式多个机器生成id，如何保证不重复? - 简书">分布式多个机器生成id，如何保证不重复? - 简书</a><br />
 </p>

<p> 更多场景设计题参看：</p>

<p><span><a href="https://blog.csdn.net/weixin_40757930/article/details/123925150" title="场景设计题 汇总 (一)_trigger333的博客-CSDN博客">场景设计题 汇总 (一)_trigger333的博客-CSDN博客</a></span></p>

<p></p>

<p id="main-toc"><strong>目录</strong></p>

<p id="1.snowflake%E6%96%B9%E6%A1%88%EF%BC%9A-toc" style="margin-left:0px;"><a href="#1.snowflake%E6%96%B9%E6%A1%88%EF%BC%9A">1.snowflake方案：</a></p>

<p id="%E4%BC%98%E7%82%B9%EF%BC%9A-toc" style="margin-left:40px;"><a href="#%E4%BC%98%E7%82%B9%EF%BC%9A">优点：</a></p>

<p id="%E7%BC%BA%E7%82%B9%EF%BC%9A-toc" style="margin-left:40px;"><a href="#%E7%BC%BA%E7%82%B9%EF%BC%9A">缺点：</a></p>

<p id="2.%E7%94%A8Redis%E7%94%9F%E6%88%90ID%EF%BC%9A-toc" style="margin-left:0px;"><a href="#2.%E7%94%A8Redis%E7%94%9F%E6%88%90ID%EF%BC%9A">2.用Redis生成ID：</a></p>

<p id="%E4%BC%98%E7%82%B9%EF%BC%9A-toc" style="margin-left:40px;"><a href="#%E4%BC%98%E7%82%B9%EF%BC%9A">优点：</a></p>

<p id="%E7%BC%BA%E7%82%B9%EF%BC%9A-toc" style="margin-left:40px;"><a href="#%E7%BC%BA%E7%82%B9%EF%BC%9A">缺点：</a></p>

<p id="3.%20UUID-toc" style="margin-left:0px;"><a href="#3.%20UUID">3. UUID</a></p>

<p id="%E4%BC%98%E7%82%B9%EF%BC%9A-toc" style="margin-left:40px;"><a href="#%E4%BC%98%E7%82%B9%EF%BC%9A">优点：</a></p>

<p id="%E7%BC%BA%E7%82%B9%EF%BC%9A-toc" style="margin-left:40px;"><a href="#%E7%BC%BA%E7%82%B9%EF%BC%9A">缺点：</a></p>

<p id="4%20%E7%BE%8E%E5%9B%A2%20Leaf-toc" style="margin-left:0px;"><a href="#4%20%E7%BE%8E%E5%9B%A2%20Leaf">4 美团 Leaf</a></p>

<hr id="hr-toc" /><h1 id="%E7%AE%80%E5%8D%95%E7%9A%84%E6%83%B3%E6%B3%95">简单的想法</h1>

<p>设计一个全局唯一的ID，需要唯一性，时间可以作为一个区分点，时间尽可能细化，精确到ms。</p>

<p>如果机器很多，并行生成ID，比如一毫秒内50台机器都被调用这个API生成ID，那么可以根据机器的不同再进行标识和区分。</p>

<p>一台机器如果运算速度快，可能1ms应该会有大量ID 生成，这种情况可以结合实际问题，限制一台机器1ms内生成的ID数量，比如至多m个。</p>

<p>如果N台机器都去一个叫<strong>ID生成服务器</strong>的服务器去得到全局ID，是很容易保证全局唯一且自增的，但是存在单点失效的问题，<strong>不满足高可用。</strong></p>

<p></p>

<p></p>

<h1 id="1.snowflake%E6%96%B9%E6%A1%88%EF%BC%9A">1.snowflake方案：</h1>

<p><a data-link-desc="分布式id生成算法的有很多种，Twitter的SnowFlake就是其中经典的一种。 概述 SnowFlake算法生成id的结果是一个64bit大小的整数，它的结构如下图： 1位，不用。二进制中最高位为1的都是负数，但是我们生成的id一般都使用整数，所以这个最高位固定是0 41位，用来记录时间戳（毫秒）。 41位可以表示$2^{41}-1$个数字， 如果..." data-link-icon="https://cdn.segmentfault.com/r-6af7c327/favicon.ico" data-link-title="理解分布式id生成算法SnowFlake - SegmentFault 思否" href="https://segmentfault.com/a/1190000011282426" title="理解分布式id生成算法SnowFlake - SegmentFault 思否">理解分布式id生成算法SnowFlake - SegmentFault 思否</a></p>

<p>snowflake是Twitter开源的分布式ID生成算法，结果是一个<strong>long型的ID（64位）</strong>。其核心思想是：使用41bit作为毫秒数，10bit作为机器的ID（5个bit是数据中心，5个bit的机器ID），12bit作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是0。</p>

<p><img alt="" height="248" src="https://img-blog.csdnimg.cn/4feadcc4fc50403680803a8e2526b217.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="732" /></p>

<p>可以表示的时间长度是69年，表示1024台机器，一个毫秒内可以有4096个ID。</p>

<p></p>

<h2 id="%E4%BC%98%E7%82%B9%EF%BC%9A">优点：</h2>

<p>1.毫秒数在高位，自增序列在低位，整个ID都是趋势递增的。</p>

<p>2.不依赖数据库等第三方系统，以服务的方式部署，稳定性更高，生成ID的性能也是非常高的。</p>

<p>3.可以根据自身业务特性分配bit位，非常灵活。</p>

<h2 id="%E7%BC%BA%E7%82%B9%EF%BC%9A">缺点：</h2>

<p><strong>强依赖机器时钟，如果机器上时钟回拨</strong>，会导致发号重复或者服务会处于不可用状态。</p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="Twitter的分布式雪花分片ID算法（SnowFlake）每秒自增生成26个万个可排序的ID (Java版)_HD243608836的博客-CSDN博客" href="https://blog.csdn.net/HD243608836/article/details/102632523" title="Twitter的分布式雪花分片ID算法（SnowFlake）每秒自增生成26个万个可排序的ID (Java版)_HD243608836的博客-CSDN博客">Twitter的分布式雪花分片ID算法（SnowFlake）每秒自增生成26个万个可排序的ID (Java版)_HD243608836的博客-CSDN博客</a></p>

<p></p>

<h1 id="2.%E7%94%A8Redis%E7%94%9F%E6%88%90ID%EF%BC%9A">2.用Redis生成ID：</h1>

<p>因为<strong>Redis是单线程的</strong>，也可以用来生成全局唯一ID。可以用Redis的<strong>原子操作INCR和INCRBY来实现。</strong></p>

<p>此外，可以使用Redis集群来获取更高的吞吐量。假如一个集群中有5台Redis，可以初始化每台Redis的值分别是1,2,3,4,5，步长都是5，各Redis生成的ID如下：</p>

<p>A：1,6,11,16</p>

<p>B：2,7,12,17</p>

<p>C：3,8,13,18</p>

<p>D：4,9,14,19</p>

<p>E：5,10,15,20</p>

<p>这种方式是负载到哪台机器提前定好，未来很难做修改。3~5台服务器基本能够满足需求，都可以获得不同的ID，但步长和初始值一定需要事先确定<strong>，使用Redis集群也可以解决单点故障问题</strong>。</p>

<p>另外，比较适合使用Redis来生成每天从0开始的流水号，如订单号=日期+当日自增长号。可以每天在Redis中生成一个Key，使用INCR进行累加。</p>

<h2>优点：</h2>

<p>1）不依赖于数据库，灵活方便，且性能优于数据库。</p>

<p>2）数字ID天然排序，对分页或需要排序的结果很有帮助。</p>

<h2>缺点：</h2>

<p>1）如果系统中没有Redis，需要引入新的组件，增加系统复杂度。</p>

<p>2）需要编码和配置的工作量较大。</p>

<p></p>

<h1 id="3.%20UUID"><strong>3. UUID</strong></h1>

<p>常见的方式。可以利用数据库也可以利用程序生成，一般来说全球唯一。UUID是由32个的16进制数字组成，所以每个UUID的长度是<strong>128位</strong>（16^32 = 2^128）。UUID作为一种广泛使用标准，有多个实现版本，影响它的因素包括时间、网卡MAC地址、自定义Namesapce等等。</p>

<h2>优点：</h2>

<p>1）简单，代码方便。</p>

<p>2）生成ID性能非常好，基本不会有性能问题。</p>

<p>3）全球唯一，在遇见数据迁移，<strong>系统数据合并，或者数据库变更等情况下</strong>，可以从容应对。</p>

<p></p>

<h2>缺点：</h2>

<p>1）没有排序，<strong>无法保证趋势递增</strong>。</p>

<p>2）UUID往往是使用字符串存储，<strong>查询的效率比较低</strong>。</p>

<p>3）存储空间比较大，如果是海量数据库，<strong>就需要考虑存储量的问题</strong>。</p>

<p>4）传输数据量大</p>

<p>5）不可读。</p>

<p></p>

<p></p>

<h1 id="4%20%E7%BE%8E%E5%9B%A2%20Leaf">4 美团 Leaf</h1>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="美团（Leaf）分布式ID算法(实战)_靈熙雲的博客-CSDN博客_leaf算法" href="https://blog.csdn.net/minkeyto/article/details/104943883" title="美团（Leaf）分布式ID算法(实战)_靈熙雲的博客-CSDN博客_leaf算法">美团（Leaf）分布式ID算法(实战)_靈熙雲的博客-CSDN博客_leaf算法</a></p>
