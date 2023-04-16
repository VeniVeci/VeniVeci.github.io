---
title: java 枚举类介绍
tags: java enum 枚举类
categories: Java基础知识
abbrlink: 717b3e82
date: 2021-02-09 16:31:25
---

<!--more-->

<p>java枚举类一般用来定义有限个，确定的常量。</p>

<p>和 static final 是一样的效果</p>

<p>     static表明这个属性是类层面的，只有一个，所有的实例化对象都共用这一个，但是可以修改。</p>

<p>     final属性不能修改，是一个常量。</p>

<p>枚举类更加直观，类型安全。使用常量则会有以下几个缺陷：</p>

<p>　　1. 类型不安全。若一个方法中要求传入季节这个参数，用常量的话，形参就是int类型，开发者传入任意类型的int类型值就行，但是如果是枚举类型的话，就只能传入枚举类中包含的对象。</p>

<p>　　2. 没有命名空间。开发者要在命名的时候以SEASON_开头，这样另外一个开发者再看这段代码的时候，才知道这四个常量分别代表季节。</p>

<p>语法规则</p>

<p><strong>所有的枚举都继承自java.lang.Enum类</strong></p>

<p><strong>示例</strong></p>

<pre>
<code class="language-java">public enum Color {
    RED, GREEN, BLANK, YELLOW
}</code></pre>

<pre>
<code class="language-java">    public static void main(String[] args) {

        System.out.println(Color.GREEN);//输出为：GREEN
        System.out.println( isRed( Color.BLANK ) ) ;  //结果： false
        System.out.println( isRed( Color.RED ) ) ;    //结果： true

    }


    static boolean isRed( Color color ){
        if ( Color.RED.equals( color )) {
            return true ;
        }
        return false ;
    }</code></pre>

<p>常用的函数</p>

<pre>
<code class="language-java">Color[] colors = Color.values();//.values()返回对象数组，方便遍历所有的枚举类
for (int i = 0; i &lt; colors.length; i++) {
     System.out.println(colors[i]);
}
//输出：
RED
BLUE
......</code></pre>

<pre>
<code class="language-java">Color color1 = Color.valueOf("YELLOW");
// 创建一个color枚举类对象，去找Color枚举类中名字是YELLOW的属性，找到的话就令
// color1 = Color.YELLOW;
//找不到就抛异常 Exception in thread "main" java.lang.IllegalArgumentException: No enum constant com.Color.YELLOW
//</code></pre>

<p>部分内容摘自</p>

<p><a href="https://www.cnblogs.com/sister/p/4700702.html">【JAVA】浅谈java枚举类</a></p>

<p> </p>
