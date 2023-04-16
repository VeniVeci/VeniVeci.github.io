---
title: Java基本数据类型知识点总结
tags: java 开发语言 后端
categories: Java基础知识
abbrlink: f3265bc9
date: 2022-03-06 15:13:53
---

<!--more-->

<p><em>参考：</em><a data-link-desc="一、8种基本数据类型(4整,2浮,1符,1布) ​ 整型：byte（最小的数据类型）、short（短整型）、int（整型）、long（长整型）； ​ 浮点型：float（浮点型）、double（双精度" data-link-icon="https://common.cnblogs.com/favicon.svg" data-link-title="Java基本数据类型和Integer缓存机制 - 之石先生 - 博客园" href="https://www.cnblogs.com/brewin/p/12681625.html" title="Java基本数据类型和Integer缓存机制 - 之石先生 - 博客园">Java基本数据类型和Integer缓存机制 - 之石先生 - 博客园</a></p>

<p><em>文章链接: </em> <a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="Java中的基本数据类型 | 学习笔记" href="https://wjhub.gitee.io/object-object-java-zhong-de-ji-ben-shu-ju-lei-xing/" title="Java中的基本数据类型 | 学习笔记">Java中的基本数据类型 | 学习笔记</a></p>

<p id="main-toc"><strong>目录</strong></p>

<p id="字符型常量和字符串常量的区别-toc" style="margin-left:0px;"><a href="#%E5%AD%97%E7%AC%A6%E5%9E%8B%E5%B8%B8%E9%87%8F%E5%92%8C%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%B8%B8%E9%87%8F%E7%9A%84%E5%8C%BA%E5%88%AB">字符型常量和字符串常量的区别?</a></p>

<p id="8%E7%A7%8D%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B-toc" style="margin-left:0px;"><a href="#8%E7%A7%8D%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B">8种数据类型</a></p>

<p id="整型（byte、short、int、long）-toc" style="margin-left:40px;"><a href="#%E6%95%B4%E5%9E%8B%EF%BC%88byte%E3%80%81short%E3%80%81int%E3%80%81long%EF%BC%89">整型（byte、short、int、long）</a></p>

<p id="浮点型（float、double）-toc" style="margin-left:40px;"><a href="#%E6%B5%AE%E7%82%B9%E5%9E%8B%EF%BC%88float%E3%80%81double%EF%BC%89">浮点型（float、double）</a></p>

<p id="字面值赋值-toc" style="margin-left:0px;"><a href="#%E5%AD%97%E9%9D%A2%E5%80%BC%E8%B5%8B%E5%80%BC">字面值赋值</a></p>

<p id="%E4%BE%8B%E5%AD%90%EF%BC%9A-toc" style="margin-left:40px;"><a href="#%E4%BE%8B%E5%AD%90%EF%BC%9A">例子：</a></p>

<p id="%C2%A0(byte)%20156%20%E7%AD%89%E4%BA%8E%E5%A4%9A%E5%B0%91%EF%BC%9A-toc" style="margin-left:40px;"><a href="#%C2%A0(byte)%20156%20%E7%AD%89%E4%BA%8E%E5%A4%9A%E5%B0%91%EF%BC%9A"> (byte) 156 等于多少：</a></p>

<p id="h1--integer--toc" style="margin-left:0px;"><a href="#h1--integer-">Integer 的缓存机制</a></p>

<p id="%C2%A0%E5%85%B6%E4%BB%96%E7%BC%93%E5%AD%98%E7%9A%84%E5%AF%B9%E8%B1%A1-toc" style="margin-left:40px;"><a href="#%C2%A0%E5%85%B6%E4%BB%96%E7%BC%93%E5%AD%98%E7%9A%84%E5%AF%B9%E8%B1%A1"> 其他缓存的对象</a></p>

<p id="%E6%98%AF%E5%90%A6%E5%AD%98%E5%9C%A8%20x%3Ex%2B1%3F%E4%B8%BA%E4%BB%80%E4%B9%88%EF%BC%9F-toc" style="margin-left:0px;"><a href="#%E6%98%AF%E5%90%A6%E5%AD%98%E5%9C%A8%20x%3Ex%2B1%3F%E4%B8%BA%E4%BB%80%E4%B9%88%EF%BC%9F">是否存在 x&gt;x+1?为什么？</a></p>

<p id="switch%E8%AF%AD%E5%8F%A5%E8%83%BD%E5%90%A6%E4%BD%9C%E7%94%A8%E5%9C%A8byte%E4%B8%8A%EF%BC%8C%E8%83%BD%E5%90%A6%E4%BD%9C%E7%94%A8%E5%9C%A8long%E4%B8%8A%EF%BC%8C%E8%83%BD%E5%90%A6%E4%BD%9C%E7%94%A8%E5%9C%A8string%E4%B8%8A%EF%BC%9F-toc" style="margin-left:0px;"><a href="#switch%E8%AF%AD%E5%8F%A5%E8%83%BD%E5%90%A6%E4%BD%9C%E7%94%A8%E5%9C%A8byte%E4%B8%8A%EF%BC%8C%E8%83%BD%E5%90%A6%E4%BD%9C%E7%94%A8%E5%9C%A8long%E4%B8%8A%EF%BC%8C%E8%83%BD%E5%90%A6%E4%BD%9C%E7%94%A8%E5%9C%A8string%E4%B8%8A%EF%BC%9F">switch语句能否作用在byte上，能否作用在long上，能否作用在string上？</a></p>

<hr id="hr-toc" /><p></p>

<h1 id="字符型常量和字符串常量的区别">字符型常量和字符串常量的区别?</h1>

<p>比较：</p>

<ol><li>字符常量用单引号，而字符串是双引号；</li>
	<li>字符常量相当于一个整形值（ASCII值，可以参加表达式运算），而字符串常量代表一个地址值（该字符在内存中的地址）；</li>
	<li>字符常量在Java中占两个字节；而字符串常量占若干个字节大小（没有大小规定）；</li>
</ol><h1 id="8%E7%A7%8D%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B">8种数据类型</h1>

<p><img alt="" height="483" src="https://img-blog.csdnimg.cn/9d58ec77399249b1806f2aa595cb8023.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1136" /></p>

<h2 id="整型（byte、short、int、long）">整型（byte、short、int、long）</h2>

<p>虽然byte、short、int、long 数据类型都是表示整数的，但是它们的取值范围可不一样。</p>

<p>byte 的取值范围：-128～127（-2的7次方到2的7次方-1）</p>

<p>short 的取值范围：-32768～32767（-2的15次方到2的15次方-1）</p>

<p>int 的取值范围：-2147483648～2147483647（-2的31次方到2的31次方-1）</p>

<p>long 的取值范围：-9223372036854774808～9223372036854774807（-2的63次方到2的63次方-1）</p>

<h2 id="浮点型（float、double）">浮点型（float、double）</h2>

<p>float 和 double 都是表示浮点型的数据类型，它们之间的区别在于精确度的不同。</p>

<p>float（单精度浮点型）取值范围：3.402823e+38～1.401298e-45（e+38 表示乘以10的38次方，而e-45 表示乘以10的负45次方）</p>

<p>double（双精度浮点型）取值范围：1.797693e+308～4.9000000e-324（同上）</p>

<p>double 类型比float 类型存储范围更大，精度更高。</p>

<blockquote>
<p>通常的浮点型数据在不声明的情况下都是double型的，如果要表示一个数据时float 型的，可以在数据后面加上 “F” 。浮点型的数据是不能完全精确的，有时候在计算时可能出现小数点最后几位出现浮动，这时正常的。</p>
</blockquote>

<h1 id="字面值赋值">字面值赋值</h1>

<p>在使用字面值对整数赋值的过程中，可以将int 赋值给byte short char int，只要不超出范围。这个过程中的类型转换时自动完成的，但是如果你试图将long literal赋给byte，即使没有超出范围，也必须进行强制类型转换。例如 byte b = 10L；是错的，要进行强制转换。<br />
 </p>

<blockquote>
<p>十进制转成十六进制： Integer.toHexString(int i) <br />
十进制转成八进制        Integer.toOctalString(int i) <br />
十进制转成二进制        Integer.toBinaryString(int i) </p>

<p><br />
十六进制转成十进制   Integer.valueOf("FFFF",16).toString() <br />
八进制转成十进制       Integer.valueOf("876",8).toString() <br />
二进制转十进制           Integer.valueOf("0101",2).toString()</p>
</blockquote>

<h2 id="%E4%BE%8B%E5%AD%90%EF%BC%9A">例子：</h2>

<pre>
<code class="language-java">    public static void main(String[] args) {
        byte a = 1;
        short b = 18;
        short c = 128;// 1000 0000  -256+128 = -128
        short d = 200; //1100 1000  减一取反  1100 0111 -- &gt; 0011 1000 8+16+32 = 56
        // -128+(200-127)           -256+200 = -56
        short e = 257;// 1 0000 0001   -256+257 = 1

//        System.out.println("200的二进制: "+ Integer.toBinaryString(200) );
//        System.out.println(Integer.valueOf("01001000",2));
        
        a = (byte) b;
        System.out.println("a = " + a);

        a = (byte) c;
        System.out.println("a = " + a);

        a = (byte) d;
        System.out.println("a = " + a);

        a = (byte) e;
        System.out.println("a = " + a);
    }</code></pre>

<h2 id="%C2%A0(byte)%20156%20%E7%AD%89%E4%BA%8E%E5%A4%9A%E5%B0%91%EF%BC%9A"> (byte) 156 等于多少：</h2>

<p>156；二进制表示为：1001 1100，进行强制转换为byte后，因为byte是有符号的，取值范围为：-128-127；1001 1100是一byte数的补码，我们将它转为原码，即减一后再取反，但符号位不能变，得到：1110 0100，这个数也就是-100了。</p>

<p></p>

<p>byte的最小值是-128，最大值是127，就好像一杯水的容量是有限的，当你杯子的水装满了，自然也就会溢出，127就好像是杯子最上面的那一层水，你只要加上一滴，就会溢出，流到杯子底部，而杯子的最底部就是-128。按照这种逻辑，你的156,也就是有28流到了底部，最底部是-128,被28覆盖了28，所以最后自然也就是-100了。</p>

<h1 id="h1--integer-">Integer 的缓存机制</h1>

<p>Integer 缓存是 Java 5 中引入的一个有助于节省内存、提高性能的特性。</p>

<p>Integer的缓存机制： Integer是对小数据（-128~127）是有缓存的，再jvm初始化的时候，数据-128~127之间的数字便被缓存到了本地内存中，如果初始化-128~127之间的数字，会直接从内存中取出，不需要新建一个对象.</p>

<p>首先看一个使用 Integer 的示例代码，展示了 Integer 的缓存行为。</p>

<pre>
<code class="language-java">public class testIntegerCache {
    public static void main(String[] args) {
        System.out.println("---int---");
        int a = 127, b = 127;
        System.out.println(a == b); //true
        a = 128;
        b = 128;
        System.out.println(a == b); //true
        System.out.println("---Integer---");
        Integer aa = 127, bb = 127;
        System.out.println(aa == bb); //true
        aa = 128;
        bb = 128;
        System.out.println(aa == bb); //false
        System.out.println(aa.equals(bb)); //true
    }
}
</code></pre>

<h2 id="%C2%A0%E5%85%B6%E4%BB%96%E7%BC%93%E5%AD%98%E7%9A%84%E5%AF%B9%E8%B1%A1"> 其他缓存的对象</h2>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="Java基本数据类型和Integer缓存机制 - 之石先生 - 博客园" href="https://www.cnblogs.com/brewin/p/12681625.html" title="Java基本数据类型和Integer缓存机制 - 之石先生 - 博客园">Java基本数据类型和Integer缓存机制 - 之石先生 - 博客园</a><br />
这种缓存行为不仅适用于Integer对象。我们针对所有整数类型的类都有类似的缓存机制。<br />
有 ByteCache 用于缓存 Byte 对象<br />
有 ShortCache 用于缓存 Short 对象<br />
有 LongCache 用于缓存 Long 对象<br />
有 CharacterCache 用于缓存 Character 对象<br />
Byte，Short，Long 有固定范围: -128 到 127。对于 Character, 范围是 0 到 127。除了 Integer 可以通过参数改变范围外，其它的都不行。</p>

<p></p>

<pre>
<code class="language-java">    public void test2(){
        short s1 = 1;
        s1 = (short) (s1 + 1);// 没有 (short)  编译报错
        s1 += 2;
        System.out.println(s1);
    }</code></pre>

<p>答：对于short s1=1;s1=s1+1来说，在s1+1运算时会自动提升表达式的类型为int，那么将int赋予给short类型的变量s1会出现类型转换错误。</p>

<p>对于short s1=1;s1+=1来说 +=是java语言规定的运算符，java编译器会对它进行特殊处理，因此可以正确编译。</p>

<h1 id="%E6%98%AF%E5%90%A6%E5%AD%98%E5%9C%A8%20x%3Ex%2B1%3F%E4%B8%BA%E4%BB%80%E4%B9%88%EF%BC%9F">是否存在 x&gt;x+1?为什么？</h1>

<p>这就是临界值，当x=最大值 时； 再加1（根据二进制运算+1）就超过了它的临界值，刚好会是它最小值。</p>

<p>举个例子吧，byte 8位， -128 ~ 127</p>

<p>127 二进制： 0111 1111</p>

<p>1 二进制 ： 0000 0001</p>

<p>相加结果： 1000 0000</p>

<p>byte 8位 有符号， 1000 0000 刚好 为 -128</p>

<p></p>

<h1 id="switch%E8%AF%AD%E5%8F%A5%E8%83%BD%E5%90%A6%E4%BD%9C%E7%94%A8%E5%9C%A8byte%E4%B8%8A%EF%BC%8C%E8%83%BD%E5%90%A6%E4%BD%9C%E7%94%A8%E5%9C%A8long%E4%B8%8A%EF%BC%8C%E8%83%BD%E5%90%A6%E4%BD%9C%E7%94%A8%E5%9C%A8string%E4%B8%8A%EF%BC%9F">switch语句能否作用在byte上，能否作用在long上，能否作用在string上？</h1>

<p>byte的存储范围小于int，可以向int类型进行隐式转换，所以switch可以作用在byte上</p>

<p>long的存储范围大于int，不能向int进行隐式转换，只能强制转换，所以switch不可以作用在long上</p>

<p>string在1.7版本之前不可以，1.7版本之后switch就可以作用在string上了</p>
