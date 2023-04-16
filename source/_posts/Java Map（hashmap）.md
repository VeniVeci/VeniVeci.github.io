---
title: Java Map（hashmap）
tags: java 数据结构 hashmap 集合
categories: Java基础知识
abbrlink: e82a34b0
date: 2021-08-03 17:38:47
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="%E4%B8%BA%E4%BB%80%E4%B9%88%E4%BD%BF%E7%94%A8%E9%9B%86%E5%90%88%E6%A1%86%E6%9E%B6%EF%BC%9F-toc" style="margin-left:0px;"><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.5/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=LA92" data-link-title="为什么使用集合框架？" href="#%E4%B8%BA%E4%BB%80%E4%B9%88%E4%BD%BF%E7%94%A8%E9%9B%86%E5%90%88%E6%A1%86%E6%9E%B6%EF%BC%9F" title="为什么使用集合框架？">为什么使用集合框架？</a></p>

<p id="hashmap-toc" style="margin-left:0px;"><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.5/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=LA92" data-link-title="hashmap" href="#hashmap" title="hashmap">hashmap</a></p>

<p id="Hash%3A-toc" style="margin-left:40px;"><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.5/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=LA92" data-link-title="Hash:" href="#Hash%3A" title="Hash:">Hash:</a></p>

<p id="hashmap%E7%9A%84%E6%89%A9%E5%AE%B9%E6%9C%BA%E5%88%B6-toc" style="margin-left:40px;"><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.5/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=LA92" data-link-title="hashmap的扩容机制" href="#hashmap%E7%9A%84%E6%89%A9%E5%AE%B9%E6%9C%BA%E5%88%B6" title="hashmap的扩容机制">hashmap的扩容机制</a></p>

<p id="%E5%85%B6%E4%BB%96-toc" style="margin-left:0px;"><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.5/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=LA92" data-link-title="其他" href="#%E5%85%B6%E4%BB%96" title="其他">其他</a></p>

<p id="Map%3C%3F%20extends%20K%2C%20%3F%20extends%20V%3E%20%E4%B8%AD%E7%9A%84%22%3F%22%E6%98%AF%E4%BB%80%E4%B9%88%E6%84%8F%E6%80%9D%EF%BC%9F-toc" style="margin-left:40px;"><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.5/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=LA92" data-link-title="Map 中的&quot;?&quot;是什么意思？" href="#Map%3C%3F%20extends%20K%2C%20%3F%20extends%20V%3E%20%E4%B8%AD%E7%9A%84%22%3F%22%E6%98%AF%E4%BB%80%E4%B9%88%E6%84%8F%E6%80%9D%EF%BC%9F" title="Map 中的&quot;?&quot;是什么意思？">Map 中的"?"是什么意思？</a></p>

<p id="%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E5%B0%BD%E9%87%8F%E9%81%BF%E5%85%8D%E6%95%B0%E7%BB%84%E6%89%A9%E5%AE%B9-toc" style="margin-left:40px;"><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.5/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=LA92" data-link-title="为什么要尽量避免数组扩容" href="#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E5%B0%BD%E9%87%8F%E9%81%BF%E5%85%8D%E6%95%B0%E7%BB%84%E6%89%A9%E5%AE%B9" title="为什么要尽量避免数组扩容">为什么要尽量避免数组扩容</a></p>

<p id="%C2%A0%E9%80%9A%E8%BF%87%E8%BF%AD%E4%BB%A3%E5%99%A8%E8%AE%BF%E9%97%AE%20hashmap%E4%B8%AD%E7%9A%84value%E2%80%8B-toc" style="margin-left:40px;"><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.5/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=LA92" data-link-title=" 通过迭代器访问 hashmap中的value​" href="#%C2%A0%E9%80%9A%E8%BF%87%E8%BF%AD%E4%BB%A3%E5%99%A8%E8%AE%BF%E9%97%AE%20hashmap%E4%B8%AD%E7%9A%84value%E2%80%8B" title=" 通过迭代器访问 hashmap中的value​"> 通过迭代器访问 hashmap中的value​</a></p>

<p id="Collections%E5%B7%A5%E5%85%B7%E7%B1%BB-toc" style="margin-left:40px;"><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.5/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=LA92" data-link-title="Collections工具类" href="#Collections%E5%B7%A5%E5%85%B7%E7%B1%BB" title="Collections工具类">Collections工具类</a></p>

<p id="SHA%E7%AE%97%E6%B3%95-toc" style="margin-left:0px;"><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.5/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=LA92" data-link-title="SHA算法" href="#SHA%E7%AE%97%E6%B3%95" title="SHA算法">SHA算法</a></p>

<p id="%E7%94%A8%E6%9D%A5%E6%A3%80%E6%9F%A5%E5%AF%86%E7%A0%81-toc" style="margin-left:40px;"><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.5/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=LA92" data-link-title="用来检查密码" href="#%E7%94%A8%E6%9D%A5%E6%A3%80%E6%9F%A5%E5%AF%86%E7%A0%81" title="用来检查密码">用来检查密码</a></p>

<p id="%C2%A0%E5%B1%80%E9%83%A8%E6%95%8F%E6%84%9F%E7%9A%84%E6%95%A3%E5%88%97%E7%AE%97%E6%B3%95-toc" style="margin-left:40px;"><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.5/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=LA92" data-link-title=" 局部敏感的散列算法" href="#%C2%A0%E5%B1%80%E9%83%A8%E6%95%8F%E6%84%9F%E7%9A%84%E6%95%A3%E5%88%97%E7%AE%97%E6%B3%95" title=" 局部敏感的散列算法"> 局部敏感的散列算法</a></p>

<hr id="hr-toc" /><h1 id="%E4%B8%BA%E4%BB%80%E4%B9%88%E4%BD%BF%E7%94%A8%E9%9B%86%E5%90%88%E6%A1%86%E6%9E%B6%EF%BC%9F"><strong>为什么使用集合框架？</strong></h1>

<p><strong>变量多了，集合方便管理。集合就是一种数据结构+算法（API），提升开发效率。</strong></p>

<p><img alt="" height="541" src="https://img-blog.csdnimg.cn/20210801162631577.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1200" /></p>

<p><img alt="" height="207" src="https://img-blog.csdnimg.cn/20210801165608621.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="708" /></p>

<h1 id="hashmap">hashmap</h1>

<p>JDK1.8之前hashmap的数据结构是链表散列，也就是链表和数组的结合体。模型图：</p>

<p><img alt="" height="565" src="https://img-blog.csdnimg.cn/20210803093118429.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1136" /></p>

<p> <img alt="" height="245" src="https://img-blog.csdnimg.cn/20210803093216627.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1098" /></p>

<p>  hash值就是通过hash算法计算出来的值，这个值就是一个内存地址（不是实际的物理地址）。</p>

<h2 id="Hash%3A"><strong>Hash:</strong></h2>

<ol><li>一般翻译做“散列”，也有直接音译为“哈希”的，就是把任意长度的输入（又叫做预映射， pre-image），通过散列算法，<span style="color:#fe2c24;">变换成固定长度的输出</span>，该输出就是散列值。</li>
	<li>这种转换是一种压缩映射，也就是，散列值的空间通常远小于输入的空间，不同的输入可能会散列成相同的输出，所以不可能从散列值来唯一的确定输入值。简单的说就是一种将任意长度的消息压缩到某一固定长度的消息摘要的函数。</li>
	<li><span style="color:#fe2c24;">文末有一种hash算法的介绍  SHA算法</span></li>
</ol><p>为什么hash表查找数据时间复杂度是O(1)</p>

<p><strong>原理：</strong></p>

<ol><li>Hash表采用一个映射函数 f : key —&gt; address 将关键字映射到该记录在表中的存储位置，从而在想要查找该记录时，可以<span style="color:#fe2c24;">直接根据关键字和映射关系计算出该记录在表中的存储位置</span>.</li>
	<li>通常情况下，这种映射关系称作为Hash函数，而通过Hash函数和关键字计算出来的存储位置(<span style="color:#fe2c24;"><strong>注意这里的存储位置只是表中的存储位置，并不是实际的物理地址</strong></span>)称作为Hash地址.</li>
	<li>比如上述例子中，假如联系人信息采用Hash表存储，则当想要找到“李四”的信息时，直接根据“李四”和Hash函数计算出Hash地址即可</li>
</ol><p><img alt="" height="471" src="https://img-blog.csdnimg.cn/20210803093950594.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="848" /></p>

<p>entry对象的定义为<img alt="" height="224" src="https://img-blog.csdnimg.cn/20210803093251149.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="918" /><br /><img alt="" height="571" src="https://img-blog.csdnimg.cn/20210803094054893.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="854" /> JDK1.8之后就hashmap的数据结构变成了  数组+链表+红黑树</p>

<p> <img alt="" height="612" src="https://img-blog.csdnimg.cn/20210803094503301.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="804" /></p>

<h2 id="hashmap%E7%9A%84%E6%89%A9%E5%AE%B9%E6%9C%BA%E5%88%B6">hashmap的扩容机制</h2>

<p>首先介绍几个概念</p>

<p>【bucket】桶：</p>

<div><span style="color:#333333;">数组中每一个位置上都放有一个桶，</span><span style="color:#fe2c24;">每个桶里就是装一个链表或是红黑树</span><span style="color:#333333;">，链表或是红黑树中可以有很多个元素(entry)</span><span style="color:#333333;">，这就是桶的意思。也就相当于把元素都放在桶中。</span></div>

<div></div>

<div>
<div><span style="color:#333333;">【</span><span style="color:#333333;">capacity</span><span style="color:#333333;">】容量</span></div>

<div></div>

<div><span style="color:#333333;">capacity</span><span style="color:#333333;">译为容量代表的数组的容量，也就是数组的长度，同时也是</span><span style="color:#333333;">HashMap</span><span style="color:#333333;">中桶的个数。默认值是16</span><span style="color:#333333;">。 </span></div>

<div></div>

<div>
<div><span style="color:#333333;">【</span><span style="color:#333333;">size</span><span style="color:#333333;">】 大小</span></div>

<div></div>

<div><span style="color:#333333;">size</span><span style="color:#333333;">就是在该</span><span style="color:#333333;">HashMap</span><span style="color:#333333;">的实例中实际存储的元素的个数。</span></div>

<div></div>
</div>

<div><span style="color:#333333;">【loadFactor】加载因子：</span></div>

<div></div>

<div>
<div><span style="color:#333333;">装载因子用来衡量</span><span style="color:#333333;">HashMap</span><span style="color:#333333;">满的程度。</span><span style="color:#333333;">loadFactor</span><span style="color:#333333;">的默认值为0.75f</span><span style="color:#333333;">。计算</span><span style="color:#333333;">HashMap</span><span style="color:#333333;">的实时装载因子的方法为：</span><span style="color:#333333;">size/capacity</span><span style="color:#333333;">，而不是占用桶的数量去除以</span><span style="color:#333333;">capacity</span><span style="color:#333333;">。</span></div>

<div></div>
</div>
</div>

<p>hashmap初始化默认是有16个桶，当添加一个元素时，通过散列函数生成hash值，也就是地址值，查看当前地址有没有元素，如果没有，就存在该地址的内存中，如果有，使用equals函数判断值是否相等，如果不相等，说明key不一样，那么就以链表的形式挂载在当前地址所存储的元素下，如果相等，则更新value即可。</p>

<p>当添加的元素数量size/<span style="color:#333333;">capacity达到加载因子并且还要在对应数组位置上有元素时，便进行扩容，桶的数量变为原来的2倍。</span></p>

<p><span style="color:#333333;">loadFactor的大小一般为0.75，</span><span style="color:#fe2c24;">过大查找起来耗费时间多，过小空间耗费多。</span></p>

<div><span style="color:#333333;">loadFactor</span><span style="color:#333333;">加载因子是控制数组存放数据的疏密程度，</span><span style="color:#333333;">loadFactor</span><span style="color:#333333;">越趋近于</span><span style="color:#333333;">1</span><span style="color:#333333;">，那么数组中存放的数据(entry)</span><span style="color:#333333;">也就越多，也就越密，也就是会让链表的长度增加，</span><span style="color:#333333;">loadFactor</span><span style="color:#333333;">越小，也就是趋近于</span><span style="color:#333333;">0</span><span style="color:#333333;">，那么数组中存放的数据也就越稀，也就是可能数组中每个位置上就放一个元素。</span></div>

<div></div>

<div><span style="color:#333333;">那有人说，就把</span><span style="color:#333333;">loadFactor</span><span style="color:#333333;">变为</span><span style="color:#333333;">1最好吗，存的数据很多，但是这样会有一个问题，就是我们在通过</span><span style="color:#333333;">key</span><span style="color:#333333;">拿到我们的</span><span style="color:#333333;">value</span><span style="color:#333333;">时，是先通过</span><span style="color:#333333;">key的</span><span style="color:#333333;">hashcode</span><span style="color:#333333;">值，找到对应数组中的位置，如果该位置中有很多元素，则需要通过</span><span style="color:#333333;">equals来依次比较链表</span><span style="color:#333333;">中的元素，拿到我们的</span><span style="color:#333333;">value</span><span style="color:#333333;">值，这样花费的性能就很高，如果能让数组上的每个位置尽量只有一个元素</span><span style="color:#333333;">最好，我们就能直接得到</span><span style="color:#333333;">value</span><span style="color:#333333;">值了。</span></div>

<div></div>

<div><span style="color:#333333;">所以有人又会说，那把</span><span style="color:#333333;">loadFactor</span><span style="color:#333333;">变得很小不就好了，但是如果变</span><span style="color:#333333;">得太小，在数组中的位置就会太稀，也就是分散的太开，浪费很多空间，这样也不好，所以在</span><span style="color:#333333;">hashMap</span><span style="color:#333333;">中</span><span style="color:#333333;">loadFactor</span><span style="color:#333333;">的初始值就是</span><span style="color:#333333;">0.75</span><span style="color:#333333;">，一般情况下不需要更改它。</span></div>

<p></p>

<p></p>

<h1 id="%E5%85%B6%E4%BB%96">其他</h1>

<p>hashmap有很多初始化函数，其中</p>

<blockquote>
<div><span style="color:#770088;">public </span><span style="color:#000000;">HashMap</span><span style="color:#333333;">(</span><span style="color:#000000;">Map</span><span style="color:#981a1a;">&lt;? </span><span style="color:#770088;">extends </span><span style="color:#000000;">K</span><span style="color:#333333;">, </span><span style="color:#981a1a;">? </span><span style="color:#770088;">extends </span><span style="color:#000000;">V</span><span style="color:#981a1a;">&gt; </span><span style="color:#000000;">m</span><span style="color:#333333;">) { </span></div>

<div><span style="color:#aa5500;">// </span><span style="color:#aa5500;">初始化填充因子 </span></div>

<div><span style="color:#770088;">this</span><span style="color:#333333;">.</span><span style="color:#000000;">loadFactor </span><span style="color:#981a1a;">= </span><span style="color:#000000;">DEFAULT_LOAD_FACTOR</span><span style="color:#333333;">; </span></div>

<div><span style="color:#aa5500;">// </span><span style="color:#aa5500;">将</span><span style="color:#aa5500;">m</span><span style="color:#aa5500;">中的所有元素添加至</span><span style="color:#aa5500;">HashMap</span><span style="color:#aa5500;">中 </span></div>

<div><span style="color:#000000;">putMapEntries</span><span style="color:#333333;">(</span><span style="color:#000000;">m</span><span style="color:#333333;">, </span><span style="color:#221199;">false</span><span style="color:#333333;">); </span></div>

<div><span style="color:#333333;">} </span></div>
</blockquote>

<h2 id="Map%3C%3F%20extends%20K%2C%20%3F%20extends%20V%3E%20%E4%B8%AD%E7%9A%84%22%3F%22%E6%98%AF%E4%BB%80%E4%B9%88%E6%84%8F%E6%80%9D%EF%BC%9F"><strong>Map&lt;? extends K, ? extends V&gt; 中的"?"是什么意思？</strong></h2>

<p>map的key要是K类或者其子类的对象,value要是类V或者其子类的对象</p>

<p></p>

<p></p>

<h2 id="%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E5%B0%BD%E9%87%8F%E9%81%BF%E5%85%8D%E6%95%B0%E7%BB%84%E6%89%A9%E5%AE%B9">为什么要尽量避免数组扩容</h2>

<p><img alt="" height="329" src="https://img-blog.csdnimg.cn/20210803165926498.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="969" /></p>

<h2 id="%C2%A0%E9%80%9A%E8%BF%87%E8%BF%AD%E4%BB%A3%E5%99%A8%E8%AE%BF%E9%97%AE%20hashmap%E4%B8%AD%E7%9A%84value%E2%80%8B"> 通过迭代器访问 hashmap中的value<img alt="" height="188" src="https://img-blog.csdnimg.cn/20210803170702426.png" width="1200" /></h2>

<p> 可以看到  所有的key使用set来维护的，也就是说map中的key具有唯一性。</p>

<p></p>

<h2 id="Collections%E5%B7%A5%E5%85%B7%E7%B1%BB">Collections工具类</h2>

<p>Collections是一个工具类，里面提供了操作集合的很多方法，比如排序，查找，替换，直接调用即可。</p>

<div><span style="color:#333333;">Collectons</span><span style="color:#333333;">提供了多个</span><span style="color:#333333;">synchronizedXxx()</span><span style="color:#333333;">方法</span><span style="color:#333333;">·</span><span style="color:#333333;">，该方法可以将指定集合包装成线程同步的集合，从而解决多线程并发访问集合时的线程安全问题。 </span></div>

<div></div>

<div><span style="color:#333333;">正如前面介绍的</span><span style="color:#333333;">HashSet</span><span style="color:#333333;">，</span><span style="color:#333333;">TreeSet</span><span style="color:#333333;">，</span><span style="color:#333333;">arrayList,LinkedList,HashMap,TreeMap</span><span style="color:#333333;">都是线程不安全的。 </span></div>

<div><span style="color:#333333;">Collections</span><span style="color:#333333;">提供了多个静态方法可以把他们包装成线程同步的集合。</span></div>

<div></div>

<div></div>

<h1 id="SHA%E7%AE%97%E6%B3%95"><span style="color:#333333;">SHA算法</span></h1>

<div>
<div><span style="color:#000000;">用于创建散列表的散列函数根据字符串生成数组索引，</span><span style="color:#fe2c24;"><strong>而SHA根据字符串生成另一个字符串</strong></span><span style="color:#000000;">。</span></div>

<div></div>
</div>

<h2 id="%E7%94%A8%E6%9D%A5%E6%A3%80%E6%9F%A5%E5%AF%86%E7%A0%81">用来检查密码</h2>

<div>数据库中存着你密码的散列值，你登录时系统计算密码的散列值然后比较，而<span style="color:#fe2c24;">不是直接判断密码是否相等。</span></div>

<div><img alt="" height="444" src="https://img-blog.csdnimg.cn/20210804152443376.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="982" /></div>

<p> <img alt="" height="172" src="https://img-blog.csdnimg.cn/20210804152558911.png" width="952" /></p>

<h2 id="%C2%A0%E5%B1%80%E9%83%A8%E6%95%8F%E6%84%9F%E7%9A%84%E6%95%A3%E5%88%97%E7%AE%97%E6%B3%95"> <span style="color:#000000;">局部敏感的散列算法</span></h2>

<p><img alt="" height="666" src="https://img-blog.csdnimg.cn/20210804152755478.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="980" /></p>

<p><img alt="" height="346" src="https://img-blog.csdnimg.cn/20210804152802354.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="996" /></p>

<div><span style="color:#333333;">部分资料来源：狂神说 、《算法图解》</span></div>
