---
title: Java线程池简介 参数设置原则
tags: java 开发语言 后端 线程池
categories: Java基础知识 多线程
abbrlink: cb49c142
date: 2022-02-25 17:21:50
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="java%E7%BA%BF%E7%A8%8B%E6%B1%A0%E6%98%AF%E4%BB%80%E4%B9%88%C2%A0%20%E6%9C%89%E4%BB%80%E4%B9%88%E7%94%A8-toc" style="margin-left:0px;"><a href="#java%E7%BA%BF%E7%A8%8B%E6%B1%A0%E6%98%AF%E4%BB%80%E4%B9%88%C2%A0%20%E6%9C%89%E4%BB%80%E4%B9%88%E7%94%A8">java线程池是什么  有什么用</a></p>

<p id="%E9%87%8D%E8%A6%81%E5%8F%82%E6%95%B0-toc" style="margin-left:0px;"><a href="#%E9%87%8D%E8%A6%81%E5%8F%82%E6%95%B0">重要参数</a></p>

<p id="%C2%A0%E9%A5%B1%E5%92%8C%E7%AD%96%E7%95%A5%E2%80%8B-toc" style="margin-left:0px;"><a href="#%C2%A0%E9%A5%B1%E5%92%8C%E7%AD%96%E7%95%A5%E2%80%8B">饱和策略​</a></p>

<p id="%E6%89%A7%E8%A1%8C%E6%B5%81%E7%A8%8B-toc" style="margin-left:0px;"><a href="#%E6%89%A7%E8%A1%8C%E6%B5%81%E7%A8%8B">执行流程</a></p>

<p id="%E6%9C%89%E5%9B%9B%E7%A7%8D%E6%8B%92%E7%BB%9D%E7%AD%96%E7%95%A5-toc" style="margin-left:40px;"><a href="#%E6%9C%89%E5%9B%9B%E7%A7%8D%E6%8B%92%E7%BB%9D%E7%AD%96%E7%95%A5">有四种拒绝策略</a></p>

<p id="%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8%20%E8%A7%81%E4%BB%A3%E7%A0%81-toc" style="margin-left:0px;"><a href="#%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8%20%E8%A7%81%E4%BB%A3%E7%A0%81">如何使用 见代码</a></p>

<p id="%E7%BA%BF%E7%A8%8B%E6%B1%A0%E5%8F%82%E6%95%B0%E8%AE%BE%E7%BD%AE%E5%8E%9F%E5%88%99%EF%BC%88%E5%AE%9E%E6%88%98%EF%BC%89-toc" style="margin-left:0px;"><a href="#%E7%BA%BF%E7%A8%8B%E6%B1%A0%E5%8F%82%E6%95%B0%E8%AE%BE%E7%BD%AE%E5%8E%9F%E5%88%99%EF%BC%88%E5%AE%9E%E6%88%98%EF%BC%89">线程池参数设置原则（实战）</a></p>

<p id="(1)%E3%80%81CPU%E5%AF%86%E9%9B%86%E5%9E%8B-toc" style="margin-left:40px;"><a href="#(1)%E3%80%81CPU%E5%AF%86%E9%9B%86%E5%9E%8B">(1)、CPU密集型</a></p>

<p id="(2)%E3%80%81IO%E5%AF%86%E9%9B%86%E5%9E%8B-toc" style="margin-left:40px;"><a href="#(2)%E3%80%81IO%E5%AF%86%E9%9B%86%E5%9E%8B">(2)、IO密集型</a></p>

<p id="(3)%E3%80%81%E5%85%88%E7%9C%8B%E4%B8%8B%E6%9C%BA%E5%99%A8%E7%9A%84CPU%E6%A0%B8%E6%95%B0%EF%BC%8C%E7%84%B6%E5%90%8E%E5%9C%A8%E8%AE%BE%E5%AE%9A%E5%85%B7%E4%BD%93%E5%8F%82%E6%95%B0%EF%BC%9A-toc" style="margin-left:40px;"><a href="#(3)%E3%80%81%E5%85%88%E7%9C%8B%E4%B8%8B%E6%9C%BA%E5%99%A8%E7%9A%84CPU%E6%A0%B8%E6%95%B0%EF%BC%8C%E7%84%B6%E5%90%8E%E5%9C%A8%E8%AE%BE%E5%AE%9A%E5%85%B7%E4%BD%93%E5%8F%82%E6%95%B0%EF%BC%9A">(3)、先看下机器的CPU核数，然后在设定具体参数：</a></p>

<p id="(4)%E3%80%81%E5%88%86%E6%9E%90%E4%B8%8B%E7%BA%BF%E7%A8%8B%E6%B1%A0%E5%A4%84%E7%90%86%E7%9A%84%E7%A8%8B%E5%BA%8F%E6%98%AFCPU%E5%AF%86%E9%9B%86%E5%9E%8B%E8%BF%98%E6%98%AFIO%E5%AF%86%E9%9B%86%E5%9E%8B-toc" style="margin-left:40px;"><a href="#(4)%E3%80%81%E5%88%86%E6%9E%90%E4%B8%8B%E7%BA%BF%E7%A8%8B%E6%B1%A0%E5%A4%84%E7%90%86%E7%9A%84%E7%A8%8B%E5%BA%8F%E6%98%AFCPU%E5%AF%86%E9%9B%86%E5%9E%8B%E8%BF%98%E6%98%AFIO%E5%AF%86%E9%9B%86%E5%9E%8B">(4)、分析下线程池处理的程序是CPU密集型还是IO密集型</a></p>

<p id="%E5%85%B7%E4%BD%93%E5%8F%82%E6%95%B0%E8%AE%BE%E7%BD%AE-toc" style="margin-left:40px;"><a href="#%E5%85%B7%E4%BD%93%E5%8F%82%E6%95%B0%E8%AE%BE%E7%BD%AE">具体参数设置</a></p>

<p id="%E5%8D%9A%E5%AE%A2%E4%B8%AD%E6%8F%90%E5%88%B0%E7%9A%84%E6%96%B9%E6%B3%95%E6%98%AF%EF%BC%9A-toc" style="margin-left:80px;"><a href="#%E5%8D%9A%E5%AE%A2%E4%B8%AD%E6%8F%90%E5%88%B0%E7%9A%84%E6%96%B9%E6%B3%95%E6%98%AF%EF%BC%9A">博客中提到的方法是：</a></p>

<hr id="hr-toc" /><p></p>

<p></p>

<p></p>

<p>资料来源  javaguide 4</p>

<p><a data-link-desc="一、前言在开发过程中，好多场景要用到线程池。每次都是自己根据业务场景来设置线程池中的各个参数。这两天又有需求碰到了，索性总结一下方便以后再遇到可以直接看着用。虽说根据业务场景来设置各个参数的值，但有些万变不离其宗，掌握它的原理对如何用好线程池起了至关重要的作用。那我们接下来就来进行线程池的分析。二、ThreadPoolExecutor的重要参数我们先来看下ThreadPoolExecutor..." data-link-icon="https://g.csdnimg.cn/static/logo/favicon32.ico" data-link-title="线程池中各个参数如何合理设置_老周聊架构的博客-CSDN博客_线程池参数设置原则" href="https://blog.csdn.net/riemann_/article/details/104704197" title="线程池中各个参数如何合理设置_老周聊架构的博客-CSDN博客_线程池参数设置原则">线程池中各个参数如何合理设置_老周聊架构的博客-CSDN博客_线程池参数设置原则</a></p>

<h1 id="java%E7%BA%BF%E7%A8%8B%E6%B1%A0%E6%98%AF%E4%BB%80%E4%B9%88%C2%A0%20%E6%9C%89%E4%BB%80%E4%B9%88%E7%94%A8">java线程池是什么  有什么用</h1>

<p><img alt="" height="262" src="https://img-blog.csdnimg.cn/83b07b264f53482ab71de873d8f22d92.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_20,color_FFFFFF,t_70,g_se,x_16" width="1142" /></p>

<p></p>

<h1 id="%E9%87%8D%E8%A6%81%E5%8F%82%E6%95%B0">重要参数</h1>

<p><img alt="" height="533" src="https://img-blog.csdnimg.cn/266e467ceb724777a7afca7281319005.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_20,color_FFFFFF,t_70,g_se,x_16" width="842" /></p>

<h1 id="%C2%A0%E9%A5%B1%E5%92%8C%E7%AD%96%E7%95%A5%E2%80%8B">饱和策略<img alt="" height="411" src="https://img-blog.csdnimg.cn/ca7ac7d6dd954e018781ad351580c07b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_20,color_FFFFFF,t_70,g_se,x_16" width="835" /></h1>

<p></p>

<p></p>

<h1 id="%E6%89%A7%E8%A1%8C%E6%B5%81%E7%A8%8B">执行流程</h1>

<p><img alt="" height="523" src="https://img-blog.csdnimg.cn/c620448a9bef41baaad369aa449c4c69.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_20,color_FFFFFF,t_70,g_se,x_16" width="1143" /></p>

<p></p>

<div><br /><p>线程池中的执行流程：</p>

<p>（1）当线程数小于核心线程数的时候，使用核心线程数。</p>

<p>（2）如果核心线程数小于线程数，就<strong>将多余的线程放入任务队列（阻塞队列）中</strong></p>

<p>（3）当任务队列（阻塞队列）满的时候，就启动最大线程数.</p>

<p>（4）当最大线程数也达到后，就将启动拒绝策略。</p>

<div><span style="color:#333333;">当高峰过去后，超过</span><span style="color:#333333;">corePoolSize </span><span style="color:#333333;">的救急线程如果一段时间没有任务做，需要结束节省资源，这个时间由 </span></div>

<div><span style="color:#333333;">keepAliveTime </span><span style="color:#333333;">和</span><span style="color:#333333;"> unit </span><span style="color:#333333;">来控制。 </span></div>

<div></div>

<div><br /><h2 id="%E6%9C%89%E5%9B%9B%E7%A7%8D%E6%8B%92%E7%BB%9D%E7%AD%96%E7%95%A5">有四种拒绝策略</h2>

<p>1.ThreadPoolExecutor.AbortPolicy</p>

<p>线程池的<strong>默认拒绝策略</strong>为AbortPolicy，即丢弃任务并抛出RejectedExecutionException异常（即后面提交的请求不会放入队列也不会直接消费并抛出异常）；</p>

<p>2.ThreadPoolExecutor.DiscardPolicy</p>

<p>丢弃任务，但是不抛出异常。如果线程队列已满，则后续提交的任务都会被丢弃，且是静默丢弃（也不会抛出任何异常，任务直接就丢弃了）。</p>

<p>3.ThreadPoolExecutor.DiscardOldestPolicy</p>

<p>丢弃队列最前面的任务，然后重新提交被拒绝的任务（丢弃掉了队列最前的任务，并不抛出异常，直接丢弃了）。</p>

<p>4.ThreadPoolExecutor.CallerRunsPolicy</p>

<p>由调用线程处理该任务（不会丢弃任务，最后所有的任务都执行了，并不会抛出异常）</p>
</div>
</div>

<h1 id="%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8%20%E8%A7%81%E4%BB%A3%E7%A0%81">如何使用 见代码</h1>

<pre>
<code class="language-java">package cn.itcast.n8;

import cn.itcast.n2.util.Sleeper;
import lombok.extern.slf4j.Slf4j;

import java.util.concurrent.*;
import java.util.concurrent.atomic.AtomicInteger;

@Slf4j(topic = "c.TestExecutors")
public class TestExecutors {
    public static void main(String[] args) throws InterruptedException {
//        test1();
//        test2();
//        test3();
        test4();

    }
    public static void test4() {
        // 线程池的拒绝策略 默认 抛出异常
        ArrayBlockingQueue&lt;Runnable&gt; queue = new ArrayBlockingQueue&lt;Runnable&gt;(20);
        ThreadFactory factory = r -&gt; new Thread(r, "test-thread-pool");
        ThreadPoolExecutor executor = new ThreadPoolExecutor(1, 2, 0L, TimeUnit.SECONDS, queue);
        while (true) {
            executor.submit(() -&gt; {
                try {
                    System.out.println(queue.size());
                    Thread.sleep(10000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            });
        }
    }
    public static void test2() {
        // test newSingleThreadExecutor
        ExecutorService pool = Executors.newSingleThreadExecutor();
        pool.execute(() -&gt; {
            log.debug("1");
            int i = 1 / 0;
        });

        pool.execute(() -&gt; {//executor.execute(worker) 来提交⼀个任务到线程池中去
            log.debug("2");
        });

        pool.execute(() -&gt; {
            log.debug("3");
        });
    }
    private static void test3() {
        ExecutorService pool = Executors.newSingleThreadExecutor();

        pool.execute(() -&gt; {
            log.debug("1start");
            Sleeper.sleep(2);
            log.debug("1end");
        });

        pool.execute(() -&gt; {
            log.debug("2start");
            Sleeper.sleep(2);
            log.debug("2end");
        });

        pool.execute(() -&gt; {
            log.debug("3start");
            Sleeper.sleep(2);
            log.debug("3end");
        });
    }
    private static void test1() {
        // test newFixedThreadPool
        ExecutorService pool = Executors.newFixedThreadPool(2, new ThreadFactory() {
            private AtomicInteger t = new AtomicInteger(1);
            @Override
            public Thread newThread(Runnable r) {
                return new Thread(r, "mypool_t" + t.getAndIncrement());
            }
        });

        pool.execute(() -&gt; {
            log.debug("1");
        });

        pool.execute(() -&gt; {
            log.debug("2");
        });

        pool.execute(() -&gt; {
            log.debug("3");
        });
    }
}
</code></pre>

<p></p>

<h1 id="%E7%BA%BF%E7%A8%8B%E6%B1%A0%E5%8F%82%E6%95%B0%E8%AE%BE%E7%BD%AE%E5%8E%9F%E5%88%99%EF%BC%88%E5%AE%9E%E6%88%98%EF%BC%89">线程池参数设置原则（实战）</h1>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="线程池中各个参数如何合理设置_老周聊架构的博客-CSDN博客_线程池参数设置原则" href="https://blog.csdn.net/riemann_/article/details/104704197" title="线程池中各个参数如何合理设置_老周聊架构的博客-CSDN博客_线程池参数设置原则">线程池中各个参数如何合理设置_老周聊架构的博客-CSDN博客_线程池参数设置原则</a></p>

<p>如何设置好的前提我们要很清楚的知道CPU密集型和IO密集型的区别。</p>

<h2 id="(1)%E3%80%81CPU%E5%AF%86%E9%9B%86%E5%9E%8B">(1)、CPU密集型</h2>

<p>CPU密集型也叫计算密集型，指的是系统的硬盘、内存性能相对CPU要好很多，此时，系统运作大部分的状况是<strong>CPU Loading 100%</strong>，CPU要读/写I/O(硬盘/内存)，I/O在<strong>很短</strong>的时间就可以完成，而CPU还有许多运算要处理，CPU Loading 很高。</p>

<p>在多重程序系统中，大部分时间用来做计算、逻辑判断等CPU动作的程序称之CPU bound。例如一个计算圆周率至小数点一千位以下的程序，在执行的过程当中绝大部分时间用在三角函数和开根号的计算，便是属于CPU bound的程序。</p>

<p>CPU bound的程序一般而言CPU占用率相当高。这可能是因为任务本身不太需要访问I/O设备，也可能是因为程序是多线程实现因此屏蔽掉了等待I/O的时间。</p>

<h2 id="(2)%E3%80%81IO%E5%AF%86%E9%9B%86%E5%9E%8B">(2)、IO密集型</h2>

<p>IO密集型指的是系统的CPU性能相对硬盘、内存要好很多，此时，系统运作，大部分的状况是CPU在等I/O (硬盘/内存) 的读/写操作，此时CPU Loading并不高。</p>

<p>I/O bound的程序一般在达到性能极限时，CPU占用率仍然较低。这可能是因为任务本身需要大量I/O操作，而pipeline做得不是很好，没有充分利用处理器能力。</p>

<p></p>

<h2 id="(3)%E3%80%81%E5%85%88%E7%9C%8B%E4%B8%8B%E6%9C%BA%E5%99%A8%E7%9A%84CPU%E6%A0%B8%E6%95%B0%EF%BC%8C%E7%84%B6%E5%90%8E%E5%9C%A8%E8%AE%BE%E5%AE%9A%E5%85%B7%E4%BD%93%E5%8F%82%E6%95%B0%EF%BC%9A">(3)、先看下机器的CPU核数，然后在设定具体参数：</h2>

<p>自己测一下自己机器的核数</p>

<blockquote>
<p>System.out.println(Runtime.getRuntime().availableProcessors());</p>
</blockquote>

<p>即CPU核数 = Runtime.getRuntime().availableProcessors()</p>

<h2 id="(4)%E3%80%81%E5%88%86%E6%9E%90%E4%B8%8B%E7%BA%BF%E7%A8%8B%E6%B1%A0%E5%A4%84%E7%90%86%E7%9A%84%E7%A8%8B%E5%BA%8F%E6%98%AFCPU%E5%AF%86%E9%9B%86%E5%9E%8B%E8%BF%98%E6%98%AFIO%E5%AF%86%E9%9B%86%E5%9E%8B">(4)、分析下线程池处理的程序是CPU密集型还是IO密集型</h2>

<p>CPU密集型：corePoolSize = CPU核数 + 1</p>

<p>IO密集型：corePoolSize = CPU核数 * 2</p>

<p></p>

<h2 id="%E5%85%B7%E4%BD%93%E5%8F%82%E6%95%B0%E8%AE%BE%E7%BD%AE">具体参数设置</h2>

<p>根据每秒的任务数 每个任务花费时间，允许等待时间设置</p>

<p>比如每秒1000个任务，每个时间0.1s，但是要求这1000个任务要在1s中处理完，这样后续的任务才能被线程池拿到处理，所以需要100个线程处理，100个线程在1s内可以完成1000个任务。</p>

<p>队列可以设置为1000，因为下一秒，这1000个任务都会进入到线程池被执行。</p>

<p></p>

<h3 id="%E5%8D%9A%E5%AE%A2%E4%B8%AD%E6%8F%90%E5%88%B0%E7%9A%84%E6%96%B9%E6%B3%95%E6%98%AF%EF%BC%9A">博客中提到的方法是：</h3>

<p>tasks ：每秒的任务数，假设为500~1000</p>

<p>taskcost：每个任务花费时间，假设为0.1s</p>

<p>responsetime：系统允许容忍的最大响应时间，假设为1s</p>

<p>做几个计算</p>

<p>corePoolSize = 每秒需要多少个线程处理？</p>

<p>threadcount = tasks/(1/taskcost) = tasks*taskcout = (500 ~ 1000)*0.1 = 50~100 个线程。</p>

<p>corePoolSize设置应该大于50。</p>

<p>根据8020原则，如果80%的每秒任务数小于800，那么corePoolSize设置为80即可。</p>

<p>queueCapacity = (coreSizePool/taskcost)*responsetime</p>

<p>计算可得 queueCapacity = 80/0.1*1 = 800。意思是队列里的线程可以等待1s，超过了的需要新开线程来执行。</p>

<p>切记不能设置为Integer.MAX_VALUE，这样队列会很大，线程数只会保持在corePoolSize大小，当任务陡增时，不能新开线程来执行，响应时间会随之陡增。</p>

<p>maxPoolSize 最大线程数在生产环境上我们往往设置成corePoolSize一样，这样可以减少在处理过程中创建线程的开销。</p>

<p>rejectedExecutionHandler：根据具体情况来决定，任务不重要可丢弃，任务重要则要利用一些缓冲机制来处理。</p>

<p>keepAliveTime和allowCoreThreadTimeout采用默认通常能满足。<br />
 </p>

<p></p>

<p></p>

<p></p>

<p></p>
