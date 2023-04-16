---
title: MySQL的主从复制
tags: mysql 数据库 database
categories: MySQL 数据库
abbrlink: 9ee789ff
date: 2022-03-12 20:47:44
---

<!--more-->

<p> MySQL的主从复制，中继日志，主从复制步骤和作用。</p>

<p id="main-toc"><strong>目录</strong></p>

<p id="mysql%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E8%AF%A6%E7%BB%86%E8%BF%87%E7%A8%8B-toc" style="margin-left:0px;"><a href="#mysql%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E8%AF%A6%E7%BB%86%E8%BF%87%E7%A8%8B">mysql主从复制详细过程</a></p>

<p id="%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E6%A6%82%E8%BF%B0-toc" style="margin-left:40px;"><a href="#%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E6%A6%82%E8%BF%B0">主从复制概述</a></p>

<p id="%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E7%9A%84%E6%AD%A5%E9%AA%A4-toc" style="margin-left:40px;"><a href="#%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E7%9A%84%E6%AD%A5%E9%AA%A4">主从复制的步骤</a></p>

<p id="%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E7%9A%84%E4%BD%9C%E7%94%A8-toc" style="margin-left:40px;"><a href="#%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E7%9A%84%E4%BD%9C%E7%94%A8">主从复制的作用</a></p>

<hr id="hr-toc" /><p></p>

<p></p>

<h1 id="mysql%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E8%AF%A6%E7%BB%86%E8%BF%87%E7%A8%8B">mysql主从复制详细过程</h1>

<h2 id="%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E6%A6%82%E8%BF%B0">主从复制概述</h2>

<p>复制是指将主数据库的DDL 和 DML 操作通过二进制日志传到从库服务器中，然后在从库上对这些日志重新执行（也叫重做），从而使得从库和主库的数据保持同步。</p>

<p>MySQL支持一台主库同时向多台从库进行复制， 从库同时也可以作为其他从服务器的主库，实现链状复制。</p>

<p></p>

<h2 id="%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E7%9A%84%E6%AD%A5%E9%AA%A4">主从复制的步骤</h2>

<p><img alt="" height="325" src="https://img-blog.csdnimg.cn/c1ada1ed5cdf46149fdf60c4a0fb4ab5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_16,color_FFFFFF,t_70,g_se,x_16" width="535" /></p>

<p></p>

<p>主从复制是 MySQL 最重要的功能之一，MySQL 集群的<strong>高可用、负载均衡和读写分离</strong>都是基于复制来实现。复制步骤如下：</p>

<ol><li>
	<p>Master 将数据改变记录到二进制日志(binary log)中。</p>
	</li>
	<li>
	<p>Slave 上面的 IO 进程连接上 Master，并请求从指定日志文件的指定位置（或者从最开始的日志）之后的日志内容。</p>
	</li>
	<li>
	<p>Master 接收到来自 Slave 的 IO 进程的请求后，负责复制的 IO 进程会根据请求信息读取<strong>日志指定位置之后的日志信息</strong>，返回给 Slave 的 IO 进程。返回信息中除了日志所包含的信息之外，还包括本次返回的信息已经到 Master 端的 binlog 文件的名称以及 binlog 的位置。</p>
	</li>
	<li>
	<p>Slave 的 IO 进程接收到信息后，将接收到的日志内容依次添加到 <strong>Slave 端的 relay log 文件的最末端</strong>，并将读取到的 Master 端的 binlog 的文件名和位置记录到 masterinfo 文件中，以便在下一次读取的时候能够清楚的告诉 Master 从某个 binlog 的哪个位置开始往后的日志内容</p>
	</li>
	<li>
	<p>Slave 的 SQL 进程检测到 relaylog 中新增加了内容后，会马上解析 relaylog 的内容成为在 Master 端真实执行时候的那些可执行的内容，<strong>并在自身执行。</strong></p>
	</li>
</ol><p></p>

<h2 id="%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E7%9A%84%E4%BD%9C%E7%94%A8">主从复制的作用</h2>

<p>主库出现问题，可以快速切换到从库提供服务，保证集群的<span style="color:#fe2c24;"><strong>高可用</strong></span>。</p>

<p>可以在从库上执行查询操作，从主库中更新，<strong>实现读写分离，降低主库的访问压力，实现<span style="color:#fe2c24;">高并发、高性能</span></strong><span style="color:#fe2c24;">。</span></p>

<p>可以在从库中执行备份，以避免备份期间影响主库的服务。</p>

<p></p>
