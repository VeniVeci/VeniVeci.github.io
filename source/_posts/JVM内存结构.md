---
title: JVM内存结构
tags: java 经验分享
categories: Java基础知识 JVM
abbrlink: e614237b
date: 2022-03-02 11:18:53
---

<!--more-->

<p>JVM内存结构，串池，虚拟机栈，堆区等。</p>

<p id="main-toc"><strong>目录</strong></p>

<p id="XJz2X-toc" style="margin-left:0px;"><a href="#XJz2X">JVM架构图</a></p>

<p id="%E5%86%85%E5%AD%98%E7%BB%93%E6%9E%84-toc" style="margin-left:0px;"><a href="#%E5%86%85%E5%AD%98%E7%BB%93%E6%9E%84">内存结构</a></p>

<p id="Rhk2G-toc" style="margin-left:40px;"><a href="#Rhk2G">1. 程序计数器</a></p>

<p id="KdMVL-toc" style="margin-left:40px;"><a href="#KdMVL">2. 虚拟机栈</a></p>

<p id="ua6ee026e-toc" style="margin-left:80px;"><a href="#ua6ee026e">内存溢出</a></p>

<p id="u8e4576a1-toc" style="margin-left:80px;"><a href="#u8e4576a1">方法内的局部变量是否线程安全？</a></p>

<p id="MMaQD-toc" style="margin-left:80px;"><a href="#MMaQD">线程运行诊断</a></p>

<p id="I8LnM-toc" style="margin-left:40px;"><a href="#I8LnM">3. 本地方法栈</a></p>

<p id="QZVhK-toc" style="margin-left:40px;"><a href="#QZVhK">4. 堆</a></p>

<p id="kFmMB-toc" style="margin-left:40px;"><a href="#kFmMB">5. 方法区</a></p>

<p id="r7g1z-toc" style="margin-left:80px;"><a href="#r7g1z">运行时常量池</a></p>

<p id="mvQ7L-toc" style="margin-left:80px;"><a href="#mvQ7L">stringtable 串池</a></p>

<p id="FBr9A-toc" style="margin-left:40px;"><a href="#FBr9A">6 直接内存</a></p>

<p id="FC5ZZ-toc" style="margin-left:80px;"><a href="#FC5ZZ">直接内存的创建和释放</a></p>

<hr id="hr-toc" /><p> </p>

<h1 id="XJz2X">JVM架构图</h1>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/21732f137709a2446cdd8ad7c5798dd7.png" /></p>

<p><span style="color:#0d0016;"><strong>jvm内存结构主要指的是</strong></span><span style="color:#fe2c24;"><strong>运行时数据区 </strong></span></p>

<p></p>

<h1 id="%E5%86%85%E5%AD%98%E7%BB%93%E6%9E%84"><strong>内存结构</strong></h1>

<h2 id="Rhk2G">1. 程序计数器</h2>

<p id="uaabd4708">寄存器物理实现，记录下一条jvm指令的地址。</p>

<p id="u3d2ac150">每一个线程有一个程序计数器，是线程私有的，内存不会溢出。</p>

<p id="ud1062e8b"></p>

<h2 id="KdMVL">2. 虚拟机栈</h2>

<p id="u2debcb04">虚拟机栈是一个线程调用时的内存，由栈帧构成，每一个方法对应一个栈帧，每一个栈帧由局部变量，返回地址，等等构成。</p>

<p id="u38483fc6"><strong>栈内存 其他操作系统默认大小是1M</strong>，windows会有变动。 所以1GB内存的电脑大概可以启动</p>

<p>1000 000个线程。</p>

<p id="ua80f9bdd"><strong>线程数 = 物理内存/栈内存</strong></p>

<p id="uab2460fe"><strong>物理内存越大 可以有更多的线程同时运行</strong></p>

<p id="ufb27af39"></p>

<h3 id="ua6ee026e"><strong>内存溢出</strong></h3>

<p id="u21b80465">1.递归调用24800多次 就会溢出</p>

<p id="ufb86af74">2.或者使用json包把类转化为相应的字符串时，会出现类之间的循环引用，从而导致栈帧过大，内存溢出。尽管只有一个方法不存在递归调用，但是这个方法中存储的信息过大，甚至大于阈值。</p>

<p id="u7255180b"></p>

<h3 id="u8e4576a1">方法内的局部变量是否线程安全？</h3>

<p id="u97df895a">如果方法内局部变量没有逃离方法的作用访问，它是线程安全的</p>

<p id="u5b8afc25">如果是局部变量引用了对象，并逃离方法的作用范围，需要考虑线程安全</p>

<p id="ue0139160">比如一些 返回值 和 调用的参数 把一些对象比如 list 逃离了方法的作用范围</p>

<p id="uc341d718"></p>

<h3 id="MMaQD">线程运行诊断</h3>

<p id="u1b12b450">CPU占用过高，比如 while true 循环</p>

<ul><li id="ue59e0320">Linux环境下运行某些程序的时候，可能导致CPU的占用过高，这时需要定位占用CPU过高的线程</li>
</ul><ul><li><strong>top</strong>命令，查看是哪个<strong>进程</strong>占用CPU过高

	<ul><li id="u65cd74e6"><strong>ps H -eo pid, tid（线程id）, %cpu | grep 刚才通过top查到的进程号</strong> 通过ps命令进一步查看是哪个线程占用CPU过高</li>
	</ul></li>
</ul><ul><li><strong>jstack 进程id</strong> 通过查看进程中的线程的nid，刚才通过ps命令看到的tid来<strong>对比定位</strong>，注意jstack查找出的线程id是<strong>16进制的</strong>，<strong>需要转换</strong></li>
</ul><p id="ub7f9e990">可以在linux下通过jstack去判断 死锁或者其他问题（死循环）程序僵死</p>

<p id="u15b6dd68">看java 的所有运行线程以及 对应的状态</p>

<p id="u68103fd3">windows也可以进行</p>

<p id="ubc8c7dfc"></p>

<h2 id="I8LnM">3. 本地方法栈</h2>

<p id="u378a816b">native方法</p>

<p id="ud168699b">wait noitfy <strong>重量级锁实现 要用 monitor 管程</strong></p>

<p id="ud65f6a0a">clone hashcode 都是 native方法</p>

<p id="u01c671d0">一般是C++ 写的</p>

<p></p>

<h2 id="QZVhK">4. 堆</h2>

<p id="uec5b1664">垃圾回收机制</p>

<p id="u86c7acf9">线程共享</p>

<p id="u1ec04607">创建的对象所在地方</p>

<p id="ub0ad29e6">设置堆内存 大小-Xmx 8m</p>

<p id="u2c5fd511"><strong>jvisualvm 工具 在命令行输入： jvisualvm</strong></p>

<p id="u22caa58e"><strong>查看 堆内存使用情况 以及 堆转储（当前时刻堆的快照，里面有堆的详细信息）</strong></p>

<p id="ue049c84c"></p>

<h2 id="kFmMB">5. 方法区</h2>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/22bd57f105d93650866444f9c2136e00.png" /></p>

<p> 
</p><p id="u596da2a9">总的来说就是，JDK1.7之前，运行时常量池（字符串常量池也在里边）是存放在永久代。<br /><strong>JDK1.7字符串常量池被单独从永久代移到堆中</strong>，运行时常量池剩下的还在永久代（方法区）<br />
JDK1.8，永久代更名为元空间（方法区的新的实现），但字符串常量池池还在堆中，<strong>运行时常量池在元空间（方法区）。</strong></p>


<p id="u97c0f0c9"></p>

<p id="u45a2aae7">实际场景中 会动态加载类 cglib</p>

<p id="ub1d06641">就容易出现 方法区溢出，尤其是 一些框架 比如 spring等</p>

<p id="uc5f9c8c4">不过目前 元空间的内存较大 所以溢出的概率就变小了</p>

<p id="u980922d9"></p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/76d1766a9c1e411de14b68d76fa50e0c.png" /></p>

<p id="u2499a4ee">方法区（Method Area）与上面讲的Java堆一样，都是各个线程共享的，它用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。虽然Java虚拟机规范把方法区描述为堆的一个逻辑部分，但是它却有一个别名叫做Non-Heap（非堆），目的应该是与Java堆区分开来。</p>

<p id="u87e0eac7"></p>

<p id="ucaa45276">Java虚拟机规范中是这样定义方法区的：<br />
它存储了每个类的结构信息，例如<strong>运行时常量池、字段、方法数据、构造函数和普通方法的字节码内容</strong>，还包括一些在类、实例、接口初始化时用到的特殊方法。</p>

<h3 id="r7g1z">运行时常量池</h3>

<p id="ufc09d0ac">概念上是在 方法区中，<strong>常量池</strong>，就是一张表，虚拟机指令根据这张常量表找到要执行的类名、方法名、参数类型、<strong>字面量</strong>等信息，之后转到解释器中解释为机器码指令 然后交给cpu执行。</p>

<p id="ua80776a4"></p>

<h3 id="mvQ7L">stringtable 串池</h3>

<p id="u341e3ae7">常量池中的字符串仅是符号，第一次用到时才变为字符串对象</p>

<p id="u14ee6208">利用串池的机制，来避免重复创建字符串对象 ，如果两个字符串（作为key）一样，那么就会使用同一份。</p>

<p id="u425aabed">字符串常量拼接的原理是编译期优化</p>

<p id="ud60fa99b">可以使用 intern 方法，主动将串池中还没有的字符串对象放入串池</p>

<p>1.6 将这个字符串对象尝试放入串池，如果有则并不会放入，如果没有会把此对象复制一份（原来的对象没有变，只是在常量池中多了一个字符串）， 放入串池， 会把串池中的对象返回。</p>

<p id="u7f54ae90">1.8 将这个字符串对象尝试放入串池，如果有则并不会放入，如果没有则放入串池（移动而不是复制，也就是说堆中的对象消失了）， 会把串池中的对象返回 。</p>

<p id="u5547e71d">只有一份 是移动不是复制</p>

<p id="u8d026402">stringtable在1.6的时候是在永久代中，但是table使用比较频繁，而永久代的垃圾回收比较晚，所以效率较低。</p>

<p id="ucdc75efe">String table又称为String pool，字符串常量池，其存在于堆中(jdk1.7之后改的)。最重要的一点，String table中存储的并不是String类型的对象，存储的而是指向String对象的索引，<strong>真实对象还是存储在堆中。</strong></p>

<p id="u2c0a19ba">此外String table还存在一个hash表的特性，里面<strong>不存在相同的两个字符串。</strong></p>

<p id="ucf4d714d">面试题</p>

<p><a data-link-desc="package com.jt.test;public class StringTable {    public static void main(String[] args) {        String s1=&quot;a&quot;;        String s2=&quot;b&quot;;        String s3=&quot;a&quot;+&quot;b&quot;;        String s4=s1+s2;        String s5=&quot;ab&quot;;        String s6=s4.intern();" data-link-icon="https://g.csdnimg.cn/static/logo/favicon32.ico" data-link-title="串池StringTable基本了解_zhanlijuan-CSDN博客" href="https://blog.csdn.net/zhanlijuan2015/article/details/118638421" title="串池StringTable基本了解_zhanlijuan-CSDN博客">串池StringTable基本了解_zhanlijuan-CSDN博客</a></p>

<p id="u97ed330b"><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="java-方法区 (二) -  StringTable串池_404QAQ的博客-CSDN博客" href="https://blog.csdn.net/qq_43539962/article/details/113918168" title="java-方法区 (二) -  StringTable串池_404QAQ的博客-CSDN博客">java-方法区 (二) - StringTable串池_404QAQ的博客-CSDN博客</a><br /><br /><a data-link-desc="Stringtable串池简单介绍常量池中的字符串仅是符号，第一次用到时才变为字符串对象利用串池的机制，来避免重复创建字符串对象 ，如果两个字符串（作为key）一样，那么就会使用同一份。可以使用 intern 方法，主动将串池中还没有的字符串对象放入串池1.6 将这个字符串对象尝试放入串池，如果有则并不会放入，如果没有会把此对象复制一份（原来的对象没有变，只是在常量池中多了一个字符串）， 放入串池， 会把串池中的对象返回。1.8 将这个字符串对象尝试放入串池，如果有则并不会放入，如果" data-link-icon="https://g.csdnimg.cn/static/logo/favicon32.ico" data-link-title="Stringtable 串池经典面试题_trigger的博客-CSDN博客" href="https://blog.csdn.net/weixin_40757930/article/details/123225164" title="Stringtable 串池经典面试题_trigger的博客-CSDN博客">Stringtable 串池经典面试题_trigger的博客-CSDN博客</a></p>

<pre>
<code class="language-java">public class StringTable {
    public static void main(String[] args) {
        String s1="a";
        String s2="b";
        String s3="a"+"b"; // 相当于 "ab"
        String s4= s1+s2;   // 在堆中
        String s5="ab";
        String s6=s4.intern();
        // 1.8 将这个字符串对象尝试放入串池，如果有则并不会放入，如果没有则放入串池（移动而不是复制，也就是说堆中的对象消失了），
        // 会把串池中的对象返回  返回的就是 “ab”所在的位置
        System.out.println(s3==s4);//false  s3就是"ab" 在串池中  s4在堆中
        System.out.println(s3==s5);// true
        System.out.println(s3==s6);// true 入池后返回引用

        String x2=new String("c")+new String("d");
        String x1="cd";
        String x3 = x2.intern();
        //问，如果调换了【最后两行代码】位置呢？ true  如果是jdk1.6呢  如果是1.6 都是false
        System.out.println(x1==x2);// false x2无法入池  所以x2的的指向还是在堆区中
        System.out.println(x1 == x3); // true 返回串池中的对象

    }
}
</code></pre>

<p id="ub1dbc861">stringtable 会发生gc，怎么调优：</p>

<p id="ub55a4d7e">如果你的程序中会用到很多的常量，stringtable的桶的个数可以设置的大一点，hash查找的速度会变快</p>

<p id="u3d8b806b">那么存入的时间就会变快。</p>

<p id="u02c79922">例子：</p>

<p id="u677e3d2c">twitter在存储用户地址的时候本来是要放到堆内存的，使用intern后放到常量池中，内存占用由30G变为了上百M。因为地址有重复 所以串池实现了去重</p>

<p id="u4946d4a8"></p>

<h2 id="FBr9A">6 直接内存</h2>

<p id="ue254c46d">直接内存是java和os都可以调用 使用的内存，</p>

<p id="uc6523af0">分配回收成本较高，但读写性能高</p>

<p id="ubcb1f969"><strong>应用场景：</strong></p>

<p id="u008f4247">在传统的NIO操作的buffer 中，如果进行大文件的读写，</p>

<p id="u795e17b9">会建立一个系统缓冲区，这个缓冲区不属于jvm管控的，而是 os的</p>

<p id="uaf023c7c">在读写时，效率就会比较低。</p>

<p id="uadfa8f68">如果使用直接内存，那么就可以减少一次复制拷贝，效率会提升。</p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/eac90a825d7be71dca62432cd260894a.png" /></p>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/b874ca012cc796628d0761030c28b444.png" /></p>

<p id="uc15a0284"></p>

<h3 id="FC5ZZ">直接内存的创建和释放</h3>

<p id="u2c201acc">直接内存可以由代码显示的创建，但是回收并不直接受到垃圾回收机制的管理，而是通过虚引用对象cleaner间接的回收。</p>

<p id="uf9acfb91"></p>

<p id="u61e608b6"><strong>比如bytebuffer</strong></p>

<p id="u978f5269">使用了 Unsafe 对象完成直接内存的分配回收，并且回收需要主动调用 freeMemory 方法</p>

<p id="u6666285a">ByteBuffer 的实现类内部，使用了 Cleaner （虚引用）来监测 ByteBuffer 对象，一旦</p>

<p id="u4eeca954">ByteBuffer 对象被垃圾回收，那么就会由 ReferenceHandler 线程通过 Cleaner 的 clean 方法调</p>

<p id="u4a0e07de">用 freeMemory 来释放直接内存</p>

<p id="u0dc2e839">在进行jvm调优的时候，通常会禁用显式的系统gc，也就是程序员自己 gc。</p>

<p id="u6e374ce1">那么直接内存这时就不会被释放，如何解决这个问题呢？</p>

<p id="u995efa09"><strong>通常来讲还是通过显式的调用 freememory函数去释放直接内存。</strong></p>
