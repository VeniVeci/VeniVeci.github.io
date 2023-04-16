---
title: Java中堆 链表 队列 栈的实现汇总
tags: java 链表 堆栈 队列
categories: Java基础知识
abbrlink: 8a30b826
date: 2022-02-15 17:10:35
---

<!--more-->

<pre>
<code class="language-java">public void test1(){
        // 堆
        PriorityQueue&lt;Integer&gt; pq = new PriorityQueue&lt;&gt;((o1, o2) -&gt; {
            return o1 - o2;//小根堆
        });
        PriorityQueue&lt;Integer&gt; pq2 = new PriorityQueue&lt;&gt;((o1, o2) -&gt; {
            return o2 - o1;//大根堆
        });
        // 队列  双端队列 的创建 主要在于前面的接口  Queue还是Deque
        Queue&lt;Integer&gt; queueSingle = new ArrayDeque&lt;&gt;();
        Deque&lt;Integer&gt; queueDouble = new ArrayDeque&lt;&gt;();
        queueSingle.add(1);
        queueDouble.add(2);
        queueSingle.offer(3);
        queueDouble.offerFirst(2);
        // 链表
        List&lt;Integer&gt; linkedlist = new LinkedList&lt;&gt;();
        // 栈
        Stack&lt;Integer&gt; stack = new Stack&lt;&gt;();
//        A more complete and consistent set of LIFO stack operations is provided by the Deque interface and its implementations, which should be used in preference to this class. For example:
//        Deque&lt;Integer&gt; stack = new ArrayDeque&lt;Integer&gt;();
//        Since:  JDK1.0
//        Author:  Jonathan Payne
        // stack的源码中 推荐使用双端队列 来代替栈
        Deque&lt;Integer&gt; s = new ArrayDeque&lt;&gt;();
        s.addLast(1);// 代替 push
        s.pollLast();// 代替 pop
        Integer last = s.peekLast();// 代替 peek
    }</code></pre>

<p></p>
