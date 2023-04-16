---
title: Java创建一个对象的过程是怎样的
tags: java
categories: Java基础知识 JVM
abbrlink: 15dce0bb
date: 2022-04-04 16:53:04
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="%E4%B8%BE%E4%B8%AA%E4%BE%8B%E5%AD%90-toc" style="margin-left:0px;"><a href="#%E4%B8%BE%E4%B8%AA%E4%BE%8B%E5%AD%90">举个例子</a></p>

<p id="%C2%A0%E5%AF%B9%E8%B1%A1%E5%88%9B%E5%BB%BA%E8%BF%87%E7%A8%8B-toc" style="margin-left:0px;"><a href="#%C2%A0%E5%AF%B9%E8%B1%A1%E5%88%9B%E5%BB%BA%E8%BF%87%E7%A8%8B">对象创建过程</a></p>

<p id="1.%E6%A3%80%E6%B5%8B%E7%B1%BB%E6%98%AF%E5%90%A6%E8%A2%AB%E5%8A%A0%E8%BD%BD%EF%BC%9A-toc" style="margin-left:40px;"><a href="#1.%E6%A3%80%E6%B5%8B%E7%B1%BB%E6%98%AF%E5%90%A6%E8%A2%AB%E5%8A%A0%E8%BD%BD%EF%BC%9A">1.检测类是否被加载：</a></p>

<p id="2.%E4%B8%BA%E5%AF%B9%E8%B1%A1%E5%88%86%E9%85%8D%E5%86%85%E5%AD%98%EF%BC%9A-toc" style="margin-left:40px;"><a href="#2.%E4%B8%BA%E5%AF%B9%E8%B1%A1%E5%88%86%E9%85%8D%E5%86%85%E5%AD%98%EF%BC%9A">2.为对象分配内存：</a></p>

<p id="3.%E4%B8%BA%E5%88%86%E9%85%8D%E7%9A%84%E5%86%85%E5%AD%98%E7%A9%BA%E9%97%B4%E5%88%9D%E5%A7%8B%E5%8C%96%E9%9B%B6%E5%80%BC%EF%BC%9A-toc" style="margin-left:40px;"><a href="#3.%E4%B8%BA%E5%88%86%E9%85%8D%E7%9A%84%E5%86%85%E5%AD%98%E7%A9%BA%E9%97%B4%E5%88%9D%E5%A7%8B%E5%8C%96%E9%9B%B6%E5%80%BC%EF%BC%9A">3.为分配的内存空间初始化零值：</a></p>

<p id="4.%E5%AF%B9%E5%AF%B9%E8%B1%A1%E8%BF%9B%E8%A1%8C%E5%85%B6%E4%BB%96%E8%AE%BE%E7%BD%AE%EF%BC%9A-toc" style="margin-left:40px;"><a href="#4.%E5%AF%B9%E5%AF%B9%E8%B1%A1%E8%BF%9B%E8%A1%8C%E5%85%B6%E4%BB%96%E8%AE%BE%E7%BD%AE%EF%BC%9A">4.对对象进行其他设置：</a></p>

<p id="5.%E6%89%A7%E8%A1%8C%20init%20%E6%96%B9%E6%B3%95%EF%BC%9A-toc" style="margin-left:40px;"><a href="#5.%E6%89%A7%E8%A1%8C%20init%20%E6%96%B9%E6%B3%95%EF%BC%9A">5.执行 init 方法：</a></p>

<p id="Java%E5%B1%9E%E6%80%A7%E7%9A%84%E5%88%9D%E5%A7%8B%E5%8C%96%E9%A1%BA%E5%BA%8F-toc" style="margin-left:0px;"><a href="#Java%E5%B1%9E%E6%80%A7%E7%9A%84%E5%88%9D%E5%A7%8B%E5%8C%96%E9%A1%BA%E5%BA%8F">Java属性在类加载过程中的初始化顺序</a></p>

<hr id="hr-toc" /><p></p>

<h1 id="%E4%B8%BE%E4%B8%AA%E4%BE%8B%E5%AD%90">举个例子</h1>

<pre>
<code class="language-java">    class Fu {
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

<p> 在main函数中new Zi() 会先去方法区的常量池中找有没有Zi这个类，发现没有，执行类的加载，链接，初始化。</p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="Java类加载机制_trigger333的博客-CSDN博客_java类加载的机制" href="https://blog.csdn.net/weixin_40757930/article/details/123229959" title="Java类加载机制_trigger333的博客-CSDN博客_java类加载的机制">Java类加载机制_trigger333的博客-CSDN博客_java类加载的机制</a></p>

<p>之后在使用这个类创建对象，具体如下。</p>

<p></p>

<h1 id="%C2%A0%E5%AF%B9%E8%B1%A1%E5%88%9B%E5%BB%BA%E8%BF%87%E7%A8%8B">对象创建过程</h1>

<p><img alt="" height="525" src="https://img-blog.csdnimg.cn/2060bf527351443793ceddcf0d8de77e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_8,color_FFFFFF,t_70,g_se,x_16" width="295" /></p>

<h2 id="1.%E6%A3%80%E6%B5%8B%E7%B1%BB%E6%98%AF%E5%90%A6%E8%A2%AB%E5%8A%A0%E8%BD%BD%EF%BC%9A"><strong>1.检测类是否被加载：</strong></h2>

<p>　　当虚拟机执行<strong>到new时</strong>，会先<strong>去常量池中查找这个类的符号引用</strong>。如果能找到符号引用，说明此类已经被加载到方法区（方法区存储虚拟机已经加载的类的信息），可以继续执行；如果找<strong>不到符号引用，就会使用类加载器执行类的加载过程，类加载完成后继续执</strong>行。</p>

<h2 id="2.%E4%B8%BA%E5%AF%B9%E8%B1%A1%E5%88%86%E9%85%8D%E5%86%85%E5%AD%98%EF%BC%9A"><strong>2.为对象分配内存：</strong></h2>

<p>　　<strong>类加载完成以后</strong>，虚<strong>拟机就开始为对象分配内存</strong>，此时所需内存的<strong>大小就已经确定</strong>了。只需要在堆上分配所需要的内存即可。</p>

<p>　　具体的<strong>分配内存有两种情况</strong>：第一种情况<strong>是内存空间绝对规整</strong>，第二种情况是<strong>内存空间是不连续的</strong>。</p>

<ul><li>对<strong>于内存绝对规整的</strong>情况相对简单一些，虚拟机只需要在被占用的内存和可用空间之间移动指针即可，这<strong>种方式被称为指针碰撞。</strong></li>
	<li>对于<strong>内存不规整的情</strong>况稍微复杂一点，这时候虚拟机<strong>需要维护一个列表</strong>，来<strong>记录哪些内存是可用</strong>的。分配内存的时候需要找到一个可用的内存空间，然后在列表上<strong>记录下已被分配</strong>，这种方式成<strong>为空闲列表</strong>。</li>
</ul><p>　　分配内存的时候也需要考<strong>虑线程安全问题</strong>，有两种解决方案：</p>

<ul><li>第一种是<strong>采用同步的办</strong>法，使用CAS来<strong>保证操作的原子性</strong>。</li>
	<li>另<strong>一种是每个线程分配内存都在自己的空间内进</strong>行，即是每个线程都<strong>在堆中</strong>预先分配一小块内存，称为本<strong>地线程分配缓冲</strong>（TLAB），分配内存的时候再TLAB上分配，互不干扰。</li>
</ul><h2 id="3.%E4%B8%BA%E5%88%86%E9%85%8D%E7%9A%84%E5%86%85%E5%AD%98%E7%A9%BA%E9%97%B4%E5%88%9D%E5%A7%8B%E5%8C%96%E9%9B%B6%E5%80%BC%EF%BC%9A"><strong>3.为分配的内存空间初始化零值：</strong></h2>

<p>　　对象的内存分配完成后，还需要将对象的内存空间都初<strong>始化为零值</strong>，这样能<strong>保证对象即使没有赋初值，也可以直接使用</strong></p>

<h2 id="4.%E5%AF%B9%E5%AF%B9%E8%B1%A1%E8%BF%9B%E8%A1%8C%E5%85%B6%E4%BB%96%E8%AE%BE%E7%BD%AE%EF%BC%9A"><strong>4.对对象进行其他设置：</strong></h2>

<p>　　分配完内存空间，初始化零值之后，虚拟机还需要对对象进行其他必要的设置，<strong>设置的地方都在对象头中</strong>，包括这个对<strong>象所属的类，类的元数据信息，对象的hashcode，GC分代年龄</strong>等信息。</p>

<h2 id="5.%E6%89%A7%E8%A1%8C%20init%20%E6%96%B9%E6%B3%95%EF%BC%9A"><strong>5.执行 init 方法：</strong></h2>

<p>　　执行完<strong>上面的步骤之</strong>后，在虚拟机里这个对象就<strong>算创建成功了</strong>，但是对于Java程序来说还需要<strong>执行init方法才算真正的创建完成</strong>，因为这个时候对象只是被初始化零值了，还没有真正的去根据程序中的代码分配初始值，调用了init方法之后，这个对象才真正能使用。</p>

<p></p>

<h1 id="Java%E5%B1%9E%E6%80%A7%E7%9A%84%E5%88%9D%E5%A7%8B%E5%8C%96%E9%A1%BA%E5%BA%8F">Java属性在类加载过程中的初始化顺序</h1>

<p>静态优于非静态 &gt; 父类优于子类</p>

<pre>
<code class="language-java">    class Fu {
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
<p>    这是父类的静态方法<br />
    这是子类的静态方法<br />
    这是父类的匿名代码块：父类的非静态属性<br />
    这是父类的构造方法<br />
    这是子类的匿名：子类的非静态属性<br />
    这是子类的构造方法</p>
</blockquote>

<p><br />
 </p>
