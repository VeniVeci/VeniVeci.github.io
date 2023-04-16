---
title: MySQL中的日志
tags: mysql 数据库 database
categories: MySQL 数据库
abbrlink: 937fe6d5
date: 2022-03-12 20:30:59
---

<!--more-->

<p>查询日志，binlog，redo log， undo log介绍。</p>

<p id="main-toc"><strong>目录</strong></p>

<p id="%E6%97%A5%E5%BF%97-toc" style="margin-left:0px;"><a href="#%E6%97%A5%E5%BF%97">日志</a></p>

<p id="MySQL%E4%B8%AD%E7%9A%844%E7%A7%8D%E6%97%A5%E5%BF%97-toc" style="margin-left:40px;"><a href="#MySQL%E4%B8%AD%E7%9A%844%E7%A7%8D%E6%97%A5%E5%BF%97">MySQL中的4种日志</a></p>

<p id="%E9%94%99%E8%AF%AF%E6%97%A5%E5%BF%97-toc" style="margin-left:80px;"><a href="#%E9%94%99%E8%AF%AF%E6%97%A5%E5%BF%97">错误日志</a></p>

<p id="%E6%9F%A5%E8%AF%A2%E6%97%A5%E5%BF%97%E5%92%8C%E6%85%A2%E6%9F%A5%E8%AF%A2%E6%97%A5%E5%BF%97-toc" style="margin-left:80px;"><a href="#%E6%9F%A5%E8%AF%A2%E6%97%A5%E5%BF%97%E5%92%8C%E6%85%A2%E6%9F%A5%E8%AF%A2%E6%97%A5%E5%BF%97">查询日志和慢查询日志</a></p>

<p id="%E4%BA%8C%E8%BF%9B%E5%88%B6%E6%97%A5%E5%BF%97%EF%BC%88binlog%EF%BC%89-toc" style="margin-left:80px;"><a href="#%E4%BA%8C%E8%BF%9B%E5%88%B6%E6%97%A5%E5%BF%97%EF%BC%88binlog%EF%BC%89">二进制日志（binlog）</a></p>

<p id="InnoDB%20%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E%E7%9A%84%E6%97%A5%E5%BF%97-toc" style="margin-left:40px;"><a href="#InnoDB%20%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E%E7%9A%84%E6%97%A5%E5%BF%97">InnoDB 存储引擎的日志</a></p>

<p id="%E9%87%8D%E5%81%9A%E6%97%A5%E5%BF%97%EF%BC%88redo%20log%EF%BC%89-toc" style="margin-left:80px;"><a href="#%E9%87%8D%E5%81%9A%E6%97%A5%E5%BF%97%EF%BC%88redo%20log%EF%BC%89">重做日志（redo log）</a></p>

<p id="%E5%9B%9E%E6%BB%9A%E6%97%A5%E5%BF%97%EF%BC%88undo%20log%EF%BC%89-toc" style="margin-left:80px;"><a href="#%E5%9B%9E%E6%BB%9A%E6%97%A5%E5%BF%97%EF%BC%88undo%20log%EF%BC%89">回滚日志（undo log）</a></p>

<hr id="hr-toc" /><p></p>

<h1 id="%E6%97%A5%E5%BF%97">日志</h1>

<h2 id="MySQL%E4%B8%AD%E7%9A%844%E7%A7%8D%E6%97%A5%E5%BF%97">MySQL中的4种日志</h2>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="MySQL中常见的几种日志 - 知乎" href="https://zhuanlan.zhihu.com/p/150105821?from_voters_page=true" title="MySQL中常见的几种日志 - 知乎">MySQL中常见的几种日志 - 知乎</a></p>

<p>在 MySQL 中，有 4 种不同的日志，分别是错误日志、二进制日志</p>

<p>（BINLOG 日志）、查询日志和慢查询日志，这些日志记录着数据库在不同方面的踪迹</p>

<h3 id="%E9%94%99%E8%AF%AF%E6%97%A5%E5%BF%97">错误日志</h3>

<p>错误日志是 MySQL 中最重要的日志之一，它记录了当 mysqld 启动和停止时，以及服务器在运行过程中发生任何</p>

<p>严重错误时的相关信息。当数据库出现任何故障导致无法正常使用时，可以首先查看此日志。</p>

<h3 id="%E6%9F%A5%E8%AF%A2%E6%97%A5%E5%BF%97%E5%92%8C%E6%85%A2%E6%9F%A5%E8%AF%A2%E6%97%A5%E5%BF%97">查询日志和慢查询日志</h3>

<p>查询日志中记录了客户端的所有操作语句，而二进制日志不包含查询数据的SQL语句。</p>

<p>慢查询日志记录了所有执行时间超过参数 long_query_time 设置值并且扫描记录数不小于</p>

<p>min_examined_row_limit 的所有的SQL语句的日志。long_query_time 默认为 10 秒，最小为 0， 精度可以到微</p>

<p>秒。</p>

<h3 id="%E4%BA%8C%E8%BF%9B%E5%88%B6%E6%97%A5%E5%BF%97%EF%BC%88binlog%EF%BC%89">二进制日志（binlog）</h3>

<p>二进制日志（BINLOG）记录了所有的 DDL（data define language数据定义语言）语句和 DML（data manage language数据操纵语言 insert 等）语句，但是<strong>不包括数据查询语句</strong>。此日志对于灾难时的数据恢复起着极其重要的作用，MySQL的主从复制， 就是通过该binlog实现的。</p>

<p></p>

<h2 id="InnoDB%20%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E%E7%9A%84%E6%97%A5%E5%BF%97">InnoDB 存储引擎的日志</h2>

<p>undo log 和 redo log 其实都不是 MySQL 数据库层面的日志，<strong>而是 InnoDB 存储引擎的日志</strong>。二者的作用联系紧密，事务的<strong>隔离性</strong>由锁来实现，<strong>原子性、一致性、持久性</strong>通过数据库的 redo log 或 undo log 来完成。</p>

<p>redo log 又称为重做日志，用来保证<strong>事务的持久性</strong>，undo log 用来保证<strong>事务的原子性和 MVCC。</strong></p>

<h3 id="%E9%87%8D%E5%81%9A%E6%97%A5%E5%BF%97%EF%BC%88redo%20log%EF%BC%89">重做日志（redo log）</h3>

<p><img alt="" height="365" src="https://img-blog.csdnimg.cn/b6cac92f30e34cd6b67dabe3b015563c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_13,color_FFFFFF,t_70,g_se,x_16" width="442" /></p>

<p> <a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="详细分析MySQL事务日志(redo log和undo log) - 骏马金龙 - 博客园" href="https://www.cnblogs.com/f-ck-need-u/archive/2018/05/08/9010872.html" title="详细分析MySQL事务日志(redo log和undo log) - 骏马金龙 - 博客园">详细分析MySQL事务日志(redo log和undo log) - 骏马金龙 - 博客园</a></p>

<p>redo 记录的是<strong><span style="color:#fe2c24;">物理上 值的修改</span></strong> 不像 binlog 是记录的是逻辑语句</p>

<p>和大多数关系型数据库一样，InnoDB 记录了对数据文件的物理更改，并保证总是<strong>日志先行</strong>，也就是所谓的 WAL，即在持久化数据文件前，保证之前的 redo 日志已经写到磁盘。由于 redo log 是顺序整块写入，所以性能要更好。</p>

<p>重做日志两部分组成：一是内存中的重做日志缓冲(redo log buffer)，是易失的；二是重做日志文件(redo log file)，是持久的。redo log 记录事务操作的变化，记录的是数据修改之后的值，不管事务是否提交都会记录下来。</p>

<p><strong>在一条语句进行执行的时候，InnoDB 引擎会把新记录写到<span style="color:#fe2c24;"> redo log 日志中，然后更新内存，更新完成后就算是语句执行完了</span>，然后在空闲的时候或者是按照设定的更新策略将 redo log 中的内容更新到磁盘中。</strong></p>

<p><strong>实现持久性</strong>，避免事务提交之后每次都要刷新到磁盘。</p>

<p></p>

<h3 id="%E5%9B%9E%E6%BB%9A%E6%97%A5%E5%BF%97%EF%BC%88undo%20log%EF%BC%89">回滚日志（undo log）</h3>

<p>undo log 有两个作用：<strong>提供回滚和多版本并发控制下的读</strong>(MVCC)，也即非锁定读</p>

<p>在数据修改的时候，不仅记录了redo，还记录了相对应的 undo，如果因为某些原因导致事务失败或回滚了，可以借助该 undo 进行回滚。</p>

<p>undo log 和 redo log 记录物理日志不一样，<strong><span style="color:#fe2c24;">它是逻辑日志</span></strong>。可以认为当 delete 一条记录时，undo log 中会记录一条对应的 insert 记录，反之亦然，当 update 一条记录时，它记录一条对应相反的 update 记录。</p>

<p>有时候应用到行版本控制的时候，也是通过 undo log 来实现的：当读取的某一行被其他事务锁定时，它可以从 undo log 中分析出该行记录以前的数据是什么，从而提供该行版本信息，让用户实现非锁定一致性读取。</p>

<p></p>

<p></p>
