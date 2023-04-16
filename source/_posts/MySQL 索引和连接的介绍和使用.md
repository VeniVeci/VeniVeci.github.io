---
title: MySQL 索引和连接的介绍和使用
tags: 数据库开发
categories: MySQL 数据库
abbrlink: c176adc7
date: 2022-03-28 16:03:25
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="%E5%BB%BA%E7%B4%A2%E5%BC%95%E7%9A%84%E5%8E%9F%E5%88%99-toc" style="margin-left:0px;"><a href="#%E5%BB%BA%E7%B4%A2%E5%BC%95%E7%9A%84%E5%8E%9F%E5%88%99">建索引的原则</a></p>

<p id="%E7%B4%A2%E5%BC%95%E8%A6%86%E7%9B%96-toc" style="margin-left:0px;"><a href="#%E7%B4%A2%E5%BC%95%E8%A6%86%E7%9B%96">索引覆盖</a></p>

<p id="%E7%B4%A2%E5%BC%95%E5%A4%B1%E6%95%88-toc" style="margin-left:0px;"><a href="#%E7%B4%A2%E5%BC%95%E5%A4%B1%E6%95%88">索引失效</a></p>

<p id="%E5%B7%A6%E8%BF%9E%E6%8E%A5%20%E5%86%85%E8%BF%9E%E6%8E%A5-toc" style="margin-left:0px;"><a href="#%E5%B7%A6%E8%BF%9E%E6%8E%A5%20%E5%86%85%E8%BF%9E%E6%8E%A5">左连接 内连接 右连接的区别</a></p>

<hr id="hr-toc" /><p></p>

<h1 id="%E5%BB%BA%E7%B4%A2%E5%BC%95%E7%9A%84%E5%8E%9F%E5%88%99">建索引的原则</h1>

<p>字段没有大量相同取值，区分性好，比如身份证号优于性别。</p>

<p>字段占用空间小。</p>

<p>考虑使用索引覆盖。对数据很少被更新的表，如果用户经常只查询其中的几个字段，可以考虑在这几个字段上建立索引，从而将表的扫描改变为索引的扫描。</p>

<p>除了以上原则，在创建索引时，我们还应当注意以下的限制：</p>

<p>（1）限制表上的索引数目。</p>

<p>对一个存在大量更新操作的表，所建索引的数目一般不要超过3个，最多不要超过5个。索引虽说提高了访问速度，但太多索引会影响数据的更新操作。</p>

<p>（2）不要在有大量相同取值的字段上，建立索引。</p>

<p>在这样的字段（例如：性别）上建立索引，字段作为选择条件时将返回大量满足条件的记录，优化器不会使用该索引作为访问路径。</p>

<p></p>

<h1 id="%E7%B4%A2%E5%BC%95%E8%A6%86%E7%9B%96">索引覆盖</h1>

<p>假如有这样一张表:</p>

<blockquote>
<pre>
<code class="language-sql">CREATE TABLE `test` (
  `id` int(11) NOT NULL,
  `age` int(11) NOT NULL,
  `name` varchar(255) NOT NULL DEFAULT '',
  PRIMARY KEY (`id`),
  KEY `idx_age_name` (`age`,`name`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;</code></pre>
</blockquote>

<p>两种查询语句</p>

<blockquote>
<p>SELECT * FROM test WHERE age = 10;<br />
SELECT age,NAME FROM test WHERE age = 10;</p>
</blockquote>

<p> 下面的这个走的是覆盖索引，比上面的快，是因为下面这个没有回表的操作，所以速度更快。</p>

<p>在实际应用中，如果select 多个字段比较慢的话，可以尝试将这多个字段建立联合索引，这样只需要扫描一遍联合索引树即可得到数据。</p>

<p></p>

<h1 id="%E7%B4%A2%E5%BC%95%E5%A4%B1%E6%95%88">索引失效</h1>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="MySQL 索引失效的几种类型以及解决方式 - 知乎" href="https://zhuanlan.zhihu.com/p/166247445" title="MySQL 索引失效的几种类型以及解决方式 - 知乎">MySQL 索引失效的几种类型以及解决方式 - 知乎</a></p>

<p><strong>索引列不独立是指 被索引的这列不能是表达式的一部分，不能是函数的参数，比如下面的这种情况</strong></p>

<pre>
<code>select id,name,age,salary from table_name where salary + 1000 = 6000;</code></pre>

<p>salary 列被用户表达式的计算了，这种情况下索引就会失效，解决方式就是提前计算好条件值，不要让索引列参与表达式计算，修改后 sql 如下</p>

<pre>
<code>select id,name,age,salary from table_name where salary = 5000;</code></pre>

<p><strong>索引字段作为函数的参数</strong></p>

<pre>
<code>select id,name,age,salary from table_name where substring(name,1,3)= 'luc';</code></pre>

<p>解决方式是什么呢，可以提前计算好条件，不要使用索引，或者可以使用其他的 sql 替换上面的，比如，上面的sql 可以使用 like 来代替</p>

<pre>
<code>select id,name,age,salary from table_name where name like 'luc%';</code></pre>

<p><strong>使用了左模糊</strong></p>

<pre>
<code>select id,name,age,salary from table_name where name like '%lucs%';</code></pre>

<p>平时尽可能避免用到左模糊，可以这样写</p>

<pre>
<code>select id,name,age,salary from table_name where name like 'lucs%';</code></pre>

<p>如果实在避免不了左模糊查询的话，考虑一下搜索引擎 比如 ES</p>

<p><strong>不符合最左前缀原则的查询</strong></p>

<p>例如有这样一个组合索引 index(a,b,c)</p>

<pre>
<code>select * from table_name where b='1'and c='2'
select * from table_name where c='2'

// 上面这两条 SQL 都是无法走索引执行的</code></pre>

<p>最左原则，就是要最左边的优先存在。</p>

<p></p>

<p><strong>查询的数量是大表的大部分，全表扫描比走索引更快。</strong></p>

<p></p>

<h1 id="%E5%B7%A6%E8%BF%9E%E6%8E%A5%20%E5%86%85%E8%BF%9E%E6%8E%A5">左连接 内连接 右连接的区别</h1>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="左连接，右连接，全外连接的区别是什么？ - 知乎" href="https://www.zhihu.com/question/288207258" title="左连接，右连接，全外连接的区别是什么？ - 知乎">左连接，右连接，全外连接的区别是什么？ - 知乎</a></p>

<p id="%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E7%94%A8%E8%BF%9E%E6%8E%A5%EF%BC%88join%EF%BC%89"><strong>为什么要用连接（join）</strong></p>

<p>因为大部分情况下，要符合数据库设计规范，数据不可能集中在同一张表里，那样的话会产生数据冗余，但是分成多张表会造成取数比较麻烦，join（连接）就是为解决上述问题的一种语法。</p>

<p id="left%20join%20%EF%BC%88%E5%B7%A6%E8%BF%9E%E6%8E%A5%EF%BC%89%EF%BC%9A%E8%BF%94%E5%9B%9E%E5%8C%85%E6%8B%AC%E5%B7%A6%E8%A1%A8%E4%B8%AD%E7%9A%84%E6%89%80%E6%9C%89%E8%AE%B0%E5%BD%95%E5%92%8C%E5%8F%B3%E8%A1%A8%E4%B8%AD%E8%BF%9E%E6%8E%A5%E5%AD%97%E6%AE%B5%E7%9B%B8%E7%AD%89%E7%9A%84%E8%AE%B0%E5%BD%95%EF%BC%8C%E5%8F%B3%E8%A1%A8%E6%B2%A1%E6%9C%89%E5%B0%B1%E5%A1%AB%E5%85%85%E4%B8%BAnull%EF%BC%9B">left join （左连接）：返回包括左表中的所有记录和右表中连接字段相等的记录，右表没有就填充为null；</p>

<p id="right%20join%20%EF%BC%88%E5%8F%B3%E8%BF%9E%E6%8E%A5%EF%BC%89%EF%BC%9A%E8%BF%94%E5%9B%9E%E5%8C%85%E6%8B%AC%E5%8F%B3%E8%A1%A8%E4%B8%AD%E7%9A%84%E6%89%80%E6%9C%89%E8%AE%B0%E5%BD%95%E5%92%8C%E5%B7%A6%E8%A1%A8%E4%B8%AD%E8%BF%9E%E6%8E%A5%E5%AD%97%E6%AE%B5%E7%9B%B8%E7%AD%89%E7%9A%84%E8%AE%B0%E5%BD%95%EF%BC%8C%E5%B7%A6%E8%A1%A8%E6%B2%A1%E6%9C%89%E5%B0%B1%E5%A1%AB%E5%85%85%E4%B8%BAnull%EF%BC%9B">right join （右连接）：返回包括右表中的所有记录和左表中连接字段相等的记录，左表没有就填充为null；</p>

<p id="inner%20join%20%EF%BC%88%E7%AD%89%E5%80%BC%E8%BF%9E%E6%8E%A5%E6%88%96%E8%80%85%E5%8F%AB%E5%86%85%E8%BF%9E%E6%8E%A5%EF%BC%89%EF%BC%9A%E5%8F%AA%E8%BF%94%E5%9B%9E%E4%B8%A4%E4%B8%AA%E8%A1%A8%E4%B8%AD%E8%BF%9E%E6%8E%A5%E5%AD%97%E6%AE%B5%E7%9B%B8%E7%AD%89%E7%9A%84%E8%A1%8C%EF%BC%8C%E4%B8%A4%E8%BE%B9%E9%83%BD%E6%9C%89%E6%89%8D%E8%BF%94%E5%9B%9E%E3%80%82">inner join （等值连接或者叫内连接）：只返回两个表中连接字段相等的行，两边都有才返回。</p>

<p id="full%20join%20%EF%BC%88%E5%85%A8%E5%A4%96%E8%BF%9E%E6%8E%A5%EF%BC%89%EF%BC%9A%E8%BF%94%E5%9B%9E%E5%B7%A6%E5%8F%B3%E8%A1%A8%E4%B8%AD%E6%89%80%E6%9C%89%E7%9A%84%E8%AE%B0%E5%BD%95%E5%92%8C%E5%B7%A6%E5%8F%B3%E8%A1%A8%E4%B8%AD%E8%BF%9E%E6%8E%A5%E5%AD%97%E6%AE%B5%E7%9B%B8%E7%AD%89%E7%9A%84%E8%AE%B0%E5%BD%95%E3%80%82">full join （全外连接）：返回左右表中所有的记录和左右表中连接字段相等的记录。</p>

<p></p>
