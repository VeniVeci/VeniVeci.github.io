---
title: filter和threadlocal进行事务管理
tags: java 多线程 数据库 过滤器
categories: Java基础知识
abbrlink: aa1a7222
date: 2021-03-10 22:15:59
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="%E4%BA%8B%E5%8A%A1%E7%AE%A1%E7%90%86%E8%AF%B7%E5%8F%82%E8%80%83%EF%BC%9A%E6%95%B0%E6%8D%AE%E5%BA%93%E4%BA%8B%E5%8A%A1%E7%AE%A1%E7%90%86-toc" style="margin-left:0px;"><a href="#%E4%BA%8B%E5%8A%A1%E7%AE%A1%E7%90%86%E8%AF%B7%E5%8F%82%E8%80%83%EF%BC%9A%E6%95%B0%E6%8D%AE%E5%BA%93%E4%BA%8B%E5%8A%A1%E7%AE%A1%E7%90%86">事务管理请参考：数据库事务管理</a></p>

<p id="Filter%E8%BF%87%E6%BB%A4%E5%99%A8-toc" style="margin-left:0px;"><a href="#Filter%E8%BF%87%E6%BB%A4%E5%99%A8">Filter过滤器</a></p>

<p id="Threadlocal-toc" style="margin-left:0px;"><a href="#Threadlocal">Threadlocal</a></p>

<p id="Filter%E5%92%8CThreadlocal%E8%BF%9B%E8%A1%8C%E4%BA%8B%E5%8A%A1%E7%AE%A1%E7%90%86-toc" style="margin-left:0px;"><a href="#Filter%E5%92%8CThreadlocal%E8%BF%9B%E8%A1%8C%E4%BA%8B%E5%8A%A1%E7%AE%A1%E7%90%86">Filter和Threadlocal进行事务管理</a></p>

<hr id="hr-toc" /><h1 id="%E4%BA%8B%E5%8A%A1%E7%AE%A1%E7%90%86%E8%AF%B7%E5%8F%82%E8%80%83%EF%BC%9A%E6%95%B0%E6%8D%AE%E5%BA%93%E4%BA%8B%E5%8A%A1%E7%AE%A1%E7%90%86">事务管理请参考：<a href="https://blog.csdn.net/weixin_40757930/article/details/114527069">数据库事务管理</a></h1>

<h1 id="Filter%E8%BF%87%E6%BB%A4%E5%99%A8">Filter过滤器</h1>

<p><img alt="" height="334" src="https://img-blog.csdnimg.cn/20210310220540591.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="800" /></p>

<p>比如说你有这样的需求：</p>

<div><span style="color:#000000;">在你的 </span><span style="color:#000000;">web </span><span style="color:#000000;">工程下，有一个 </span><span style="color:#000000;">admin </span><span style="color:#000000;">目录。这个 </span><span style="color:#000000;">admin </span><span style="color:#000000;">目录下的所有资源（</span><span style="color:#000000;">html </span><span style="color:#000000;">页面、</span><span style="color:#000000;">jpg </span><span style="color:#000000;">图片、</span><span style="color:#000000;">jsp </span><span style="color:#000000;">文件、等等）都必须是用户登录之后才允许访问的。</span></div>

<div><span style="color:#000000;">那么利用filter过滤器就可以实现。</span></div>

<div><img alt="" height="460" src="https://img-blog.csdnimg.cn/20210310220750217.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1200" /></div>

<div> </div>

<h1 id="Threadlocal">Threadlocal</h1>

<p>threadlocal就是一个map，key是当前线程，value是自己可以设置的值，类型是object， 语法是：threadLocal.set(o);</p>

<div><span style="color:#000000;">ThreadLocal </span><span style="color:#000000;">的作用，它可以解决多线程的数据安全问题。</span></div>

<div>
<div><span style="color:#000000;">ThreadLocal </span><span style="color:#000000;">的特点： </span></div>

<div><span style="color:#000000;">1</span><span style="color:#000000;">、</span><span style="color:#000000;">ThreadLocal </span><span style="color:#000000;">可以为当前线程关联一个数据。（它可以像 </span><span style="color:#000000;">Map </span><span style="color:#000000;">一样存取数据，</span><span style="color:#000000;">key </span><span style="color:#000000;">为当前线程） </span></div>

<div><span style="color:#000000;">2</span><span style="color:#000000;">、每一个 </span><span style="color:#000000;">ThreadLocal </span><span style="color:#000000;">对象，只能为当前线程关联一个数据，如果要为当前线程关联多个数据，就需要使用多个 </span></div>

<div><span style="color:#000000;">ThreadLocal </span><span style="color:#000000;">对象实例。 </span></div>

<div><span style="color:#000000;">3</span><span style="color:#000000;">、每个 </span><span style="color:#000000;">ThreadLocal </span><span style="color:#000000;">对象实例定义的时候，一般都是 </span><span style="color:#000000;">static </span><span style="color:#000000;">类型 </span></div>

<div><span style="color:#000000;">4</span><span style="color:#000000;">、</span><span style="color:#000000;">ThreadLocal </span><span style="color:#000000;">中保存数据，在线程销毁后。会由 </span><span style="color:#000000;">JVM </span><span style="color:#000000;">虚拟自动释放。</span></div>

<div><span style="color:#000000;">示例代码：</span></div>

<div> </div>
</div>

<blockquote>
<pre>
<code class="language-html hljs">package com.atguigu.threadlocal;

import java.util.HashMap;
import java.util.Map;
import java.util.Random;

public class ThreadLocalTest {

    public static ThreadLocal&lt;Object&gt; threadLocal = new ThreadLocal&lt;Object&gt;(){
};

    private static Random random = new Random();


    public static class Task implements  Runnable {
        @Override
        public void run() {
            String name = Thread.currentThread().getName();

            Integer i = random.nextInt(1000);
            System.out.println("线程["+name+"]生成的随机数是：" + i);

            threadLocal.set(i);

            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            new OrderService().createOrder();
            Object o = threadLocal.get();
            System.out.println("在线程["+name+"]快结束时取出关联的数据是：" + o);

        }
    }

    public static void main(String[] args) {
        for (int i = 0; i &lt; 2; i++){
            new Thread(new Task()).start();
        }

    }


}
</code></pre>
</blockquote>

<h1 id="Filter%E5%92%8CThreadlocal%E8%BF%9B%E8%A1%8C%E4%BA%8B%E5%8A%A1%E7%AE%A1%E7%90%86">Filter和Threadlocal进行事务管理</h1>

<blockquote>
<pre>
@Override
public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
    try {
        filterChain.doFilter(servletRequest,servletResponse);
        JdbcUtils.commitAndClose();// 提交事务
    } catch (Exception e) {
        JdbcUtils.rollbackAndClose();//回滚事务
        e.printStackTrace();
        throw new RuntimeException(e);//把异常抛给Tomcat管理展示友好的错误页面
    }
}</pre>
</blockquote>

<p> </p>
