---
title: Synchronized和Lock的实现原理与区别 附代码
tags: java 开发语言 锁
categories: Java基础知识 多线程
abbrlink: f2872bf8
date: 2022-02-25 19:55:37
---

<!--more-->

<h1>用Synchronized和Lock实现交替打印奇偶数。</h1>

<h2>Synchronized</h2>

<pre>
<code class="language-java">    private static void testSynchronized() {
        // 实现 synchronized 多线程交替打印奇数偶数
        Object obj = new Object();
        Thread thread1 = new Thread(() -&gt; {
            for (int i = 0; i &lt;= 100; i++) {
                synchronized (obj){
                    if(i%2==1) System.out.println(Thread.currentThread().getName() + ": " +i);
                    obj.notifyAll();
                    try {
                        obj.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        },"奇数");
        Thread thread2 = new Thread(() -&gt; {
            for (int i = 0; i &lt;= 100; i++) {
                synchronized (obj){
                    if(i%2==0) System.out.println(Thread.currentThread().getName() + ": " +i);
                    obj.notifyAll();
                    try {
                        obj.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        },"偶数");
        thread1.start();
        thread2.start();
    }</code></pre>

<h2>lock</h2>

<pre>
<code class="language-java">public class demo2 {
    static volatile boolean flag = false;
    private static int count = 0;
    private static int timewait = 10;
    public static void main(String[] args) {
        testLock();
        // 实现 lock 多线程交替打印奇数偶数
    }
    private static void testLock() {
        ReentrantLock lock = new ReentrantLock();
        Thread thread1 = new Thread(() -&gt; {
            while(count &lt;= 100) {
                if(!flag) {
                    lock.lock();
                    try{
                        if(count%2==1) System.out.println(Thread.currentThread().getName() + ": " +count++);
                        flag = true;
                    }finally {
                        lock.unlock();
                    }
                }else {
                    // 防止线程空转
                    try {
                        Thread.sleep(timewait);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        },"奇数");


        Thread thread2 = new Thread(() -&gt; {
            while(count &lt;= 100) {
                if(flag) {
                    lock.lock();
                    try{
                        if(count%2==0) System.out.println(Thread.currentThread().getName() + ": " +count++);
                        flag = false;
                    }finally {
                        lock.unlock();
                    }
                }else {
                    // 防止线程空转
                    try {
                        Thread.sleep(timewait);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        },"偶数");
        thread1.start();
        thread2.start();
        System.out.println();
    }

}
</code></pre>

<h1 id="JUQvt">为什么会有lock</h1>

<h2 id="rvHLv">一.synchronized的缺陷</h2>

<p id="u7faef49e">synchronized是java中的一个关键字，也就是说是Java语言内置的特性。那么为什么会出现Lock呢？</p>

<p id="ucb96f990">在上面一篇文章中，我们了解到如果一个代码块被synchronized修饰了，当一个线程获取了对应的锁，并执行该代码块时，其他线程便只能一直等待，等待获取锁的线程释放锁，而这里获取锁的线程释放锁只会有两种情况：</p>

<p id="ue6c89069">1）获取锁的线程执行完了该代码块，然后线程释放对锁的占有；</p>

<p id="u26677710">2）线程执行发生异常，此时JVM会让线程自动释放锁。</p>

<p id="u5a969601">那么如果这个获取锁的线程<strong>由于要等待IO或者其他原因（比如调用sleep方法）被阻塞了，但是又没有释放锁，</strong>其他线程便只能干巴巴地等待，试想一下，这多么影响程序执行效率。</p>

<p id="u8f614eb7">因此就需要有一种机制可以不让等待的线程一直无期限地等待下去（比如只等待一定的时间或者能够响应中断），通过Lock就可以办到。</p>

<p id="u21f3c62a"></p>

<p id="uc6246204">再举个例子：当有多个线程读写文件时，读操作和写操作会发生冲突现象，写操作和写操作会发生冲突现象，但是读操作和读操作不会发生冲突现象。</p>

<p id="ucf61abf0">但是采用synchronized关键字来实现同步的话，就会导致一个问题：</p>

<p id="uf6e290d4">如果多个线程都只是进行读操作，所以当一个线程在进行读操作时，其他线程只能等待无法进行读操作。</p>

<p id="u269abb36">因此就需要一种机制来使得多个线程都只是进行读操作时，线程之间不会发生冲突，<strong>通过Lock就可以办到。</strong></p>

<p id="u704ae482">另外，通过Lock可以知道线程有没有成功获取到锁。这个是synchronized无法办到的。</p>

<p></p>

<h1>相同点 不同点</h1>

<p><img alt="" height="675" src="https://img-blog.csdnimg.cn/37206ee5b7394c9b82717af47a97ff94.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_20,color_FFFFFF,t_70,g_se,x_16" width="1016" /></p>

<p></p>

<p> </p>

<p></p>

<p></p>

<p></p>

<p></p>

<p></p>

<p></p>

<p></p>

<p></p>

<p></p>

<p></p>

<p></p>

<p></p>

<p></p>

<p> </p>

<p> </p>
