---
title: Stringtable 串池经典面试题
tags: java 开发语言 后端
categories: Java基础知识 JVM
abbrlink: b9b7c063
date: 2022-03-02 11:02:43
---

<!--more-->

<h1 id="mvQ7L">Stringtable串池</h1>

<h2>简单介绍</h2>

<p id="u341e3ae7">常量池中的字符串仅是符号，第一次用到时才变为字符串对象</p>

<p id="u14ee6208">利用串池的机制，来避免重复创建字符串对象 ，如果两个字符串（作为key）一样，那么就会使用同一份。</p>

<p id="ud60fa99b">可以使用 intern 方法，主动将串池中还没有的字符串对象放入串池</p>

<p>1.6 将这个字符串对象尝试放入串池，如果有则并不会放入，如果没有会把此对象复制一份（原来的对象没有变，只是在常量池中多了一个字符串）， 放入串池， 会把串池中的对象返回。</p>

<p id="u7f54ae90">1.8 将这个字符串对象尝试放入串池，如果有则并不会放入，如果没有则放入串池（移动而不是复制，也就是说堆中的对象消失了）， 会把串池中的对象返回 。</p>

<p id="ucdc75efe">String table又称为String pool，字符串常量池，其存在于堆中(jdk1.7之后改的)。最重要的一点，String table中存储的并不是String类型的对象，存储的而是指向String对象的索引，<strong>真实对象还是存储在堆中。</strong></p>

<p id="u2c0a19ba">此外String table还存在一个hash表的特性，里面<strong>不存在相同的两个字符串。</strong></p>

<h2 id="ucf4d714d">面试题</h2>

<p><span><a href="https://blog.csdn.net/zhanlijuan2015/article/details/118638421" title="串池StringTable基本了解_zhanlijuan-CSDN博客">串池StringTable基本了解_zhanlijuan-CSDN博客</a></span></p>

<p id="u97ed330b"><span><a href="https://blog.csdn.net/qq_43539962/article/details/113918168" title="java-方法区 (二) -  StringTable串池_404QAQ的博客-CSDN博客">java-方法区 (二) - StringTable串池_404QAQ的博客-CSDN博客</a></span></p>

<h2>代码解析</h2>

<pre>
<code class="language-java">
public class StringTable {
    public static void main(String[] args) {
        String s1="a";
        String s2="b";
        String s3="a"+"b"; // 相当于 "ab"  在串池中 串池在堆区中
        String s4= s1+s2;   // 在堆中 但不在串池中
        String s5="ab"; // 在串池中 因为串池的hash属性  只有一份 ab 所以和s3一样
        String s6=s4.intern();
        // java1.8 将这个字符串对象尝试放入串池，如果有则并不会放入，如果没有则放入串池（移动而不是复制，也就是说堆中的对象消失了），
        // 会把串池中的对象返回  返回的就是 “ab”所在的位置
// 具体的 串池中有"ab" 所以并不会把s4 从堆区移动到串池中，但是返回的是 串池中的对象 s3

        System.out.println(s3==s4);//false  s3就是"ab" 在串池中  s4在堆中
        System.out.println(s3==s5);// true
        System.out.println(s3==s6);// true 入池后返回引用 

        String x2=new String("c")+new String("d");
        String x1="cd";
        String x3 = x2.intern();
        //问，如果调换了【最后两行代码】位置呢？ true  
        //如果是jdk1.6呢  如果是1.6 都是false
        System.out.println(x1==x2);// false x2无法入池  所以x2的的指向还是在堆区中
        System.out.println(x1 == x3); // true 返回串池中的对象

    }
}
</code></pre>

<p></p>
