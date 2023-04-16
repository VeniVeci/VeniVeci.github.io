---
title: Spring的7种事务传播机制整理
tags: spring java 后端
categories: Spring框架 数据库
abbrlink: b25f5a4e
date: 2022-03-06 22:23:24
---

<!--more-->

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="带你读懂Spring 事务——事务的传播机制 - 知乎" href="https://zhuanlan.zhihu.com/p/148504094" title="带你读懂Spring 事务——事务的传播机制 - 知乎">带你读懂Spring 事务——事务的传播机制 - 知乎</a></p>

<p><a data-link-desc="1，Propagation.REQUIRED 如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。详细解释在代码下方。 看下代码员工service 部..." data-link-icon="data:image/vnd.microsoft.icon;base64,AAABAAEAICAAAAEAIACoEAAAFgAAACgAAAAgAAAAQAAAAAEAIAAAAAAAABAAABILAAASCwAAAAAAAAAAAAAAAAAASWTtHEhh5qZIYObmSGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDm5khh5qZJZO0cAAAAAElk7RxIYeXtSGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hh5e1JZO0cSGHmpkhg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hh5qZIYObmSGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDm5khg5f9IYOX/SGDl/0hg5f9IYOX/ipnu/5qn8P9qfen/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/TWTl/5qn8P+grfH/nKnx/3iJ6/9JYeX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f/Cyvb///////T2/f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f99juz//////////////////////8vS9/9LY+X/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/8LK9v///////f3+/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/2l96f+hrfH/o6/x/9HX+P///////////5Gg7/9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/wsr2///////9/f7/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/Vmzn////////////usP1/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f/Cyvb///////39/v9IYOX/SGDl/46d7v/+/v7//v7+//7+/v/+/v7//v7+//7+/v/+/v7/8fP9/8LK9v9keOn/SGDl/0hg5f9JYeX///////////+9xvX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/8LK9v///////f3+/0hg5f9IYOX/j53v////////////ydD3/6ax8v+msfL/prHy/6248//t8Pz//////+/x/P9dcuj/SGDl/0lh5f///////////73G9f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/wsr2///////9/f7/SGDl/0hg5f+Pne////////////+RoO//SGDl/0hg5f9IYOX/SGDl/5Kg7////////////56q8f9IYOX/SWHl////////////vcb1/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f/Cyvb///////39/v9IYOX/SGDl/4+d7////////////5Oh7/9JYeX/SWHl/0lh5f9JYeX/h5fu////////////ucL1/0hg5f9JYeX///////////+9xvX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/8LK9v///////f3+/0hg5f9IYOX/j53v//////////////////////////////////////////////////////+4wfX/SGDl/0lh5f///////////73G9f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/wsr2///////9/f7/SGDl/0hg5f+Pne/////////////g5Pr/xs32/8bN9v/Gzfb/xs32/9jd+f///////////7fB9P9IYOX/SWHl////////////vcb1/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f/Cyvb///////39/v9IYOX/SGDl/4+d7////////////5uo8P9IYOX/SGDl/0hg5f9IYOX/gZHt////////////t8D0/0hg5f9JYeX///////////+9xvX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/8LK9v///////f3+/0hg5f9IYOX/j53v////////////m6jw/0hg5f9IYOX/SGDl/0hg5f+Bke3///////////+2wPT/SGDl/0lh5f///////////73G9f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/wsr2///////9/f7/SGDl/0hg5f+Pne///////////////////////////////////////////////////////7W/9P9IYOX/SWHl////////////vcb1/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f/Cyvb///////39/v9IYOX/SGDl/4WV7f/l6Pv/5ej7/+Xo+//l6Pv/5ej7/+Xo+//l6Pv/5ej7/+Xo+//l6Pv/pbHy/0hg5f9JYeX///////////+9xvX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/2h86f96i+z/eozs/7S+9P/K0ff/p7Ly/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0lh5f///////////73G9f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9gdej///////////+8xfX/bYDq/5il8P+YpfD/mKXw/5il8P+YpfD/mKXw/5il8P+YpfD/mKXw/5il8P+YpfD/mafw////////////vcb1/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/7zF9f//////9fb9/1906P/P1fj///////////////////////////////////////////////////////////////////////////+9xvX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/oK3x/1Rq5v9/kOz///////z9/v99juz/SGDl/6u28//AyPb/wMj2/8DI9v/K0ff/4eX6/8DI9v/AyPb/wMj2/8DI9v/AyPb/wMj2/8DI9v/AyPb/wMj2/5Wj8P9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f/8/f7/9fb9/6248/9Uaub/TWTl/0hg5f9/kOz/6Ov7/+Hl+v9ccuf/SGDl/3WH6///////vMX1/1Fo5v9IYOX/SGDl/0hg5f/L0vf/9/j9/8TL9v9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/6Ov8f/6+/7//////87U+P9PZub/SGDl/7rD9f//////9fb9/05l5f9IYOX/YXXo/+7w/P//////3eL6/1lu5/9IYOX/TWTl//f4/f//////rbjz/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/22A6v/09v3//////9fc+f+Zp/D/9vf9///////Y3fn/j53v/5Si7/+Wo/D/Znrp/93i+v//////3uL6/5mn8P+yvPT///////////+yvPT/mafw/5mn8P96i+z/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/32O7P////////////////////////////////////////////////+cqfH/X3To//r7/v////////////////////////////////////////////Hz/f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/8zT9////////////2l86f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/usP1////////////iJju/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/jJvu////////////kZ/v/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9/kOz///////////+eqvH/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5uZIYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9KYeX/SmHl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYObmSGHmpkhg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hh5qZJZO0cSGHl7Uhg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYeXtSWTtHAAAAABJZO0cSGHmpkhg5uZIYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYOX/SGDl/0hg5f9IYObmSGHmpklk7RwAAAAAgAAAAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIAAAAE=" data-link-title="Spring的事务传播机制实例 - 简书" href="https://www.jianshu.com/p/25c8e5a35ece" title="Spring的事务传播机制实例 - 简书">Spring的事务传播机制实例 - 简书</a></p>

<h1>什么是传播机制？</h1>

<p>所谓传播机制,指的就是 方法A带有事务，去调用带有事务的方法B，方法A，B事务设置的事务属性，会对方法A，B的事务产生什么影响。</p>

<p>前提：A调用B  A是父事务，B是子事务。</p>

<table align="center" border="1" cellpadding="1" cellspacing="1" style="width:500px;"><thead><tr><th scope="col" style="width:156px;"></th>
			<th scope="col" style="width:132px;">通俗理解</th>
			<th scope="col" style="width:215px;">具体介绍</th>
		</tr></thead><tbody><tr><td style="width:156px;">
			<pre>
REQUIRED</pre>
			</td>
			<td style="width:132px;">需要一个事务</td>
			<td style="width:215px;">如果当前B没有事务，则B自己新建一个事务，如果当前有事务，就加入当前事务。如果B中抛出异常，那么A中已经执行的操作也会回滚。</td>
		</tr><tr><td style="width:156px;">
			<pre>
REQUIRES_NEW</pre>
			</td>
			<td style="width:132px;">自己的事务必须是新的，而且只能有一个</td>
			<td style="width:215px;">B创建一个新事务，如果A存在当前事务，则挂起A的事务。相当于只有B新建的事务了。</td>
		</tr><tr><td style="width:156px;">
			<pre>
MANDATORY</pre>
			</td>
			<td style="width:132px;">
			<p></p>

			<p>强制性的必须有事务</p>
			</td>
			<td style="width:215px;">当前A存在事务，则加入当前事务，如果当前A事务不存在，则抛出异常。</td>
		</tr><tr><td style="width:156px;">
			<pre>
SUPPORTS</pre>
			</td>
			<td style="width:132px;">B支持当前A(父事务)的事务</td>
			<td style="width:215px;">当前A存在事务，则加入当前事务，如果当前没有事务，B就以非事务方法执行。</td>
		</tr><tr><td style="width:156px;">
			<pre>
NOT_SUPPORTED</pre>
			</td>
			<td style="width:132px;">B不支持当前A(父事务)的事务</td>
			<td style="width:215px;">始终以非事务方式执行,如果当前存在事务，则挂起当前事务。</td>
		</tr><tr><td style="width:156px;">
			<pre>
NEVER</pre>
			</td>
			<td style="width:132px;">不能有</td>
			<td style="width:215px;">A不能有事务，否则抛异常</td>
		</tr><tr><td style="width:156px;">
			<pre>
NESTED</pre>
			</td>
			<td style="width:132px;">嵌套在A父事务中</td>
			<td style="width:215px;">如果当前A事务存在，则在嵌套事务中执行。如果不存在，就自己新建一个事务。父事务抛异常，子事务回滚，子事务抛异常，父事务不受影响。</td>
		</tr><tr><td style="width:156px;"></td>
			<td style="width:132px;"></td>
			<td style="width:215px;"></td>
		</tr></tbody></table><p></p>
