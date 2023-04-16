---
title: 设计发红包的API （ 微信红包的算法是怎样的？）
tags: java
categories: 场景设计
abbrlink: 4c392693
date: 2022-04-02 22:13:01
---

<!--more-->

<p> </p>

<p>让你设计一个<strong>微信发红包的</strong>API，你会怎么设计，不能有人领到的红包里面没钱，红包数值精确到分。</p>

<h1>我的想法：</h1>

<p>如果是随机红包，根据发红包的人输入的钱数，默认精确到分，也就是0.01元。</p>

<p>最小的不可再分的单元就是0.01元。</p>

<p>如果红包的分数是10份，那么就把红包比如50元，那么可以生成1-4991的随机整数。</p>

<p>极端案例，一个人分得49.91元，剩余九个人每人分得0.01元。</p>

<p>第一个人分完，剩下的按照这个规则继续。</p>

<p></p>

<p></p>

<h1 id="%E5%BE%AE%E4%BF%A1%E7%BA%A2%E5%8C%85%E7%9A%84%E8%A7%84%E5%88%99">微信红包的规则</h1>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="微信红包算法 - 云+社区 - 腾讯云" href="https://cloud.tencent.com/developer/article/1454083?from=15425" title="微信红包算法 - 云+社区 - 腾讯云">微信红包算法 - 云+社区 - 腾讯云</a></p>

<p>过年很多人会发微信的红包，但是为毛很多人说自己得不到最佳，因此作者写了一个微信红包发送的算法。</p>

<p>首先科普一下，微信红包的 <code>规则</code> 为：</p>

<blockquote>
<p>红包金额的区间为 <code>0.01</code> - <code>平均值的2倍</code></p>
</blockquote>

<p>该规则为 <code>微信团队公布的算法</code> ，读者可自行上网查找相关信息。</p>

<p>这也就是说，假设给10个人发送100元的红包，那么：</p>

<pre>
<code>第一个人得到金额的区间为[0.01,20]</code></pre>

<p>假设 <code>前三个人</code> 领到的红包为50元，那么此时红包还剩下 <code>7个人</code> 没有领取红包，红包还剩下 <code>50元</code> ，那么下一个人可以得到的最大金额为：</p>

<p><code>(100-50)/(10-3)*2=14.29</code></p>

<pre>
<code>第四个人得到的金额的区间为[0.01,14.29]</code></pre>

<p>以此类推，最终可以将红包领完。</p>

<p></p>

<p></p>

<p></p>
