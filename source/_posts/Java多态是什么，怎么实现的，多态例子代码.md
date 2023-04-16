---
title: Java多态是什么，怎么实现的，多态例子代码
tags: java
categories: Java基础知识
abbrlink: b540b82b
date: 2022-03-22 23:33:39
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="%E5%AD%90%E7%B1%BB%E5%9E%8B%E5%92%8C%E5%AD%90%E7%B1%BB-toc" style="margin-left:0px;"><a href="#%E5%AD%90%E7%B1%BB%E5%9E%8B%E5%92%8C%E5%AD%90%E7%B1%BB">子类型和子类</a></p>

<p id="%E5%A4%9A%E6%80%81%E5%88%86%E4%B8%A4%E7%A7%8D-toc" style="margin-left:0px;"><a href="#%E5%A4%9A%E6%80%81%E5%88%86%E4%B8%A4%E7%A7%8D">多态分两种</a></p>

<p id="%E5%A4%9A%E6%80%81%E7%9A%84%E7%94%A8%E9%80%94-toc" style="margin-left:0px;"><a href="#%E5%A4%9A%E6%80%81%E7%9A%84%E7%94%A8%E9%80%94">多态的用途</a></p>

<p id="%E5%A4%9A%E6%80%81%E7%9A%84%E8%BD%AC%E5%9E%8B-toc" style="margin-left:40px;"><a href="#%E5%A4%9A%E6%80%81%E7%9A%84%E8%BD%AC%E5%9E%8B">多态的转型</a></p>

<p id="%E8%BF%90%E8%A1%8C%E6%97%B6%E5%A4%9A%E6%80%81%E7%9A%84%E4%BE%8B%E5%AD%90-toc" style="margin-left:0px;"><a href="#%E8%BF%90%E8%A1%8C%E6%97%B6%E5%A4%9A%E6%80%81%E7%9A%84%E4%BE%8B%E5%AD%90">运行时多态的例子</a></p>

<p id="%E5%A4%9A%E6%80%81%E5%AE%9E%E7%8E%B0%E7%9A%84%E6%9C%BA%E5%88%B6%20JVM-toc" style="margin-left:0px;"><a href="#%E5%A4%9A%E6%80%81%E5%AE%9E%E7%8E%B0%E7%9A%84%E6%9C%BA%E5%88%B6%20JVM">多态实现的机制 JVM</a></p>

<hr id="hr-toc" /><p>部分内容摘自：</p>

<p><a data-link-desc="作者：crane_practicewww.cnblogs.com/crane-practice/p/3671074.htmlJava多态的实现机制是父类或接口定义的引用变..." data-link-icon="https://g.csdnimg.cn/static/logo/favicon32.ico" data-link-title="Java多态的实现机制是什么，写得非常好！_Java技术栈的博客-CSDN博客" href="https://blog.csdn.net/youanyyou/article/details/91920093" title="Java多态的实现机制是什么，写得非常好！_Java技术栈的博客-CSDN博客">Java多态的实现机制是什么，写得非常好！_Java技术栈的博客-CSDN博客</a></p>

<h1 id="%E5%AD%90%E7%B1%BB%E5%9E%8B%E5%92%8C%E5%AD%90%E7%B1%BB"><strong>子类型和子类</strong></h1>

<p>子类型（Subtype）这个词和子类（Subclass）的区别，简单地说，只要是A类运用了extends关键字实现了对B类的继承，那么我们就可以说Class A是Class B的子类，子类是一个语法层面上的词，只要满足继承的语法，就存在子类关系。</p>

<p>子类型比子类有更严格的要求，它不仅要求有继承的语法，同时要求如果存在子类对父类方法的改写（override），那么改写的内容必须符合父类原本的语义，其被调用后的作用应该和父类实现的效果方向一致。<span style="color:#fe2c24;"><strong>只有保证子类都是子类型，多态才有意义。</strong></span></p>

<p></p>

<h1 id="%E5%A4%9A%E6%80%81%E5%88%86%E4%B8%A4%E7%A7%8D">多态分两种</h1>

<ol><li>
	<p>编译时多态（又称静态多态）</p>
	</li>
	<li>
	<p>运行时多态（又称动态多态）</p>
	</li>
</ol><p>重载（overload）就是编译时多态的一个例子，编译时多态在编译时就已经确定，运行时运行的时候调用的是确定的方法。</p>

<p>我们通常所说的多态指的都是运行时多态，也就是编译时不确定究竟调用哪个具体方法，一直延迟到运行时才能确定。这也是为什么有时候多态方法又被称为延迟方法的原因。</p>

<p>虚拟机会在执行程序时动态调用实际类的方法，它会通过一种名为动态绑定（又称延迟绑定）的机制自动实现。</p>

<p></p>

<h1 id="%E5%A4%9A%E6%80%81%E7%9A%84%E7%94%A8%E9%80%94"><strong>多态的用途</strong></h1>

<p>多态最大的用途我认为在于对设计和架构的复用，更进一步来说，《设计模式》中提倡的针对接口编程而不是针对实现编程就是充分利用多态的典型例子。</p>

<h2 id="%E5%A4%9A%E6%80%81%E7%9A%84%E8%BD%AC%E5%9E%8B">多态的转型</h2>

<p><img alt="" height="483" src="https://img-blog.csdnimg.cn/bcb48ad2fb8341f89a89a6f4133607b2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1183" /></p>

<h1 id="%E8%BF%90%E8%A1%8C%E6%97%B6%E5%A4%9A%E6%80%81%E7%9A%84%E4%BE%8B%E5%AD%90">运行时多态的例子</h1>

<pre>
<code class="language-java">public class Duotai {
    public static void main(String[] args) {
        People p=new Stu();
        p.eat();//调用子类重写过的方法 就是运行时多态  向上转型
//        p.study();  错误 p没有study 方法
        Stu s=(Stu)p; //调用子类型特有的方法  向下转型
        s.study();
    }
}
class People{
    public void eat(){
        System.out.println("吃饭");
    }
}
class Stu extends People{
    @Override
    public void eat(){
        System.out.println("吃水煮肉片");
    }
    public void study(){
        System.out.println("好好学习");
    }
}</code></pre>

<h1 id="%E5%A4%9A%E6%80%81%E5%AE%9E%E7%8E%B0%E7%9A%84%E6%9C%BA%E5%88%B6%20JVM"><br />
多态实现的机制 JVM</h1>

<p>拿上述例子来说，父类和子类都有自己的方法表，方法表的构造如下：</p>

<p>由于Java的单继承机制，一个类只能继承一个父类，而所有的类又都继承自Object类。方法表中最先存放的是Object类的方法，接下来是该类的父类People的方法，最后是该类Stu本身的方法。这里关键的地方在于，如果子类改写了父类的方法，那么子类和父类的那些同名方法共享一个方法表项，都被认作是父类的方法。</p>

<p>注意这里只有非私有的实例方法才会出现，并且静态方法也不会出现在这里，原因很容易理解：静态方法跟对象无关，可以将方法地址直接引用，而不像实例方法需要间接引用。</p>

<p>由于以上方法的排列特性（Object——父类——子类），使得方法表的偏移量总是固定的。例如，对于任何类来说，其方法表中equals方法的偏移量总是一个定值，所有继承某父类的子类的方法表中，其父类所定义的方法的偏移量也总是一个定值。</p>

<p>前面说过，方法表中的表项都是指向该类对应方法的指针，<strong>这里就开始了多态的实现</strong>：</p>

<p>假设Class Stu是Class People的子类，并且Stu改写了People的方法eat()，那么在People的方法表中，eat方法的指针指向的就是B的People方法入口。</p>

<p>而对于Stu来说，它的方法表中的eat() 方法则会指向其自身的eat() 方法而非其父类People的（这在类加载器载入该类时已经保证，同时JVM会保证总是能从对象引用指向正确的类型信息）。</p>

<p>结合方法指针偏移量是固定的以及指针总是指向实际类的方法域，<strong>我们不难发现多态的机制就在这里：</strong></p>

<p>在调用方法时，实际上必须首先完成实例方法的符号引用解析，结果是该符号引用被解析为方法表的偏移量。</p>

<p>虚拟机通过对象引用得到方法区中类型信息的入口，查询类的方法表，当将子类对象声明为父类类型时，形式上调用的是父类方法，<strong>此时虚拟机会从实际类的方法表（虽然声明的是父类，但是实际上这里的类型信息中存放的是子类的信息）中查找该方法名对应的指针</strong>（这里用“查找”实际上是不合适的，前面提到过，方法的偏移量是固定的，所以只需根据偏移量就能获得指针），进而就能指向实际类的方法了。</p>

<p>概括一下，就是运行时虽然这个对象的声明是父类的，但是是  Stu类的对象，所以eat()方法绑定的是Stu重写后的eat()方法的地址。</p>

<p> 更深入的内容：</p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="Java多态的实现机制是什么，写得非常好！_Java技术栈的博客-CSDN博客" href="https://blog.csdn.net/youanyyou/article/details/91920093" title="Java多态的实现机制是什么，写得非常好！_Java技术栈的博客-CSDN博客">Java多态的实现机制是什么，写得非常好！_Java技术栈的博客-CSDN博客</a></p>
