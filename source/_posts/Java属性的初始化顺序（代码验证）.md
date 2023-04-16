---
title: Java属性的初始化顺序（代码验证）
tags: java 开发语言 后端
categories: Java基础知识
abbrlink: 92d1413f
date: 2022-03-02 14:57:27
---

<!--more-->

<h1 id="d3o1t">Java属性的初始化顺序</h1>

<p id="u2be9c644"><strong>静态优于非静态 &gt; 父类优于子类</strong></p>

<p></p>

<pre>
<code class="language-java">

class Fu {
    {
        System.out.println("这是父类的匿名代码块：父类的非静态属性");
    }
    static {
        System.out.println("这是父类的静态方法");
    }
    public Fu() {
        System.out.println("这是父类的构造方法");
    }
}
class Zi extends Fu {
    {
        System.out.println("这是子类的匿名：子类的非静态属性");
    }
    static {
        System.out.println("这是子类的静态方法");
    }
    public Zi() {
        //super();//一般都是省略的
        System.out.println("这是子类的构造方法");
    }
}
public class Main{
    public static void main(String[] args) throws ClassNotFoundException {
        Zi zi = new Zi();

    }
}</code></pre>

<blockquote>
<pre id="v3O6I">
这是父类的静态方法
这是子类的静态方法
这是父类的匿名代码块：父类的非静态属性
这是父类的构造方法
这是子类的匿名：子类的非静态属性
这是子类的构造方法
</pre>
</blockquote>

<p style="text-align:center;"><img alt="" src="https://img-blog.csdnimg.cn/img_convert/4d16e25f37ec2936c150e38f4f6081fb.png" /></p>

<p>忘记原出处了，侵删 ~</p>
