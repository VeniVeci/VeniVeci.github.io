---
title: 内部类简介 如何创建内部类(Java)
tags: 内部类
categories: Java基础知识
abbrlink: b3e75e6d
date: 2022-04-03 21:15:00
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E4%BD%BF%E7%94%A8%E5%86%85%E9%83%A8%E7%B1%BB%EF%BC%9F-toc" style="margin-left:0px;"><a href="#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E4%BD%BF%E7%94%A8%E5%86%85%E9%83%A8%E7%B1%BB%EF%BC%9F">为什么要使用内部类？</a></p>

<p id="%E5%AE%9E%E9%99%85demo-toc" style="margin-left:0px;"><a href="#%E5%AE%9E%E9%99%85demo">实际demo</a></p>

<p id="%E4%B8%BA%E4%BB%80%E4%B9%88%E5%86%85%E9%83%A8%E7%B1%BB%E5%8F%AF%E4%BB%A5%E8%AE%BF%E9%97%AE%E5%A4%96%E5%9B%B4%E7%B1%BB%E7%9A%84%20private%E6%88%90%E5%91%98-toc" style="margin-left:40px;"><a href="#%E4%B8%BA%E4%BB%80%E4%B9%88%E5%86%85%E9%83%A8%E7%B1%BB%E5%8F%AF%E4%BB%A5%E8%AE%BF%E9%97%AE%E5%A4%96%E5%9B%B4%E7%B1%BB%E7%9A%84%20private%E6%88%90%E5%91%98">为什么内部类可以访问外围类的 private成员</a></p>

<p id="%E5%A6%82%E4%BD%95%E5%88%9B%E5%BB%BA%20%E5%86%85%E9%83%A8%E7%B1%BB-toc" style="margin-left:40px;"><a href="#%E5%A6%82%E4%BD%95%E5%88%9B%E5%BB%BA%20%E5%86%85%E9%83%A8%E7%B1%BB">如何创建 内部类</a></p>

<p id="%E5%86%85%E9%83%A8%E7%B1%BB%E7%9A%84%E5%88%86%E7%B1%BB-toc" style="margin-left:40px;"><a href="#%E5%86%85%E9%83%A8%E7%B1%BB%E7%9A%84%E5%88%86%E7%B1%BB">内部类的分类</a></p>

<hr id="hr-toc" /><p> </p>

<h1 id="%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E4%BD%BF%E7%94%A8%E5%86%85%E9%83%A8%E7%B1%BB%EF%BC%9F">为什么要使用内部类？</h1>

<p>在《Think in java》中有这样一句话：使用内部类最吸引人的原因是：每个内部类都能独立地继承一个（接口的）实现，所以无论外围类是否已经继承了某个（接口的）实现，<strong>对于内部类都没有影响。</strong></p>

<p>在我们程序设计中有时候会存在一些使用接口很难解决的问题，这个时候我们可以利用内部类提供的、可以继承多个具体的或者抽象的类的能力来解决这些程序设计问题。可以这样说，<strong>接口只是解决了部分问题，而<span style="color:#fe2c24;">内部类使得多重继承的解决方案变得更加完整。</span></strong></p>

<h1 id="%E5%A6%82%E4%BD%95%E5%88%9B%E5%BB%BA%20%E5%86%85%E9%83%A8%E7%B1%BB">如何创建 内部类</h1>

<p>其实在这个应用程序中我们还看到了如何来引用内部类：引用内部类我们需要指明这个对象的类型：OuterClasName.InnerClassName。</p>

<p>同时如果我们需要创建某个内部类对象，必须要利用外部类的对象通过.new来创建内部类： OuterClass.InnerClass innerClass = <strong>outerClass.new InnerClass();。</strong></p>

<p>到这里了我们需要明确一点，内部类是个编译时的概念，一旦编译成功后，它就与外围类属于两个完全不同的类（当然他们之间还是有联系的）。对于一个名为OuterClass的外围类和一个名为InnerClass的内部类，在编译成功后，<span style="color:#fe2c24;"><strong>会出现这样两个class文件：OuterClass.class和OuterClass$InnerClass.class。</strong></span></p>

<p>内部类和外部类只是在Java语言层面的一个概念，而不存在于JVM。内部类经过编译后也会生成一个class文件，并记录其外部类的一些信息。<strong>外部类可以看做是一个普通的类，触发初始化和普通类一样，都是在要用到的时候进行初始化。</strong></p>

<h1 id="%E5%AE%9E%E9%99%85demo">实际demo</h1>

<pre>
<code class="language-java">package BasicDemo;


public class OuterClass {
    private String name ;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    /**省略getter和setter方法**/

    public InnerClass getInnerClassInstance(){
        return new InnerClass();
    }

    public class InnerClass{
        public InnerClass(){
            name = "chenssy";
            age = 23;
        }

        public void dosth(){
            name = "kiki";
            System.out.println(name);
        }

        public void display(){
            System.out.println("name：" + getName() +"   ;age：" + getAge());
        }
    }

    public static void main(String[] args) {
        InnerClass innerClass = new OuterClass().getInnerClassInstance();

        innerClass.dosth();
        innerClass.display();
    }
}
</code></pre>

<h1 id="%E4%B8%BA%E4%BB%80%E4%B9%88%E5%86%85%E9%83%A8%E7%B1%BB%E5%8F%AF%E4%BB%A5%E8%AE%BF%E9%97%AE%E5%A4%96%E5%9B%B4%E7%B1%BB%E7%9A%84%20private%E6%88%90%E5%91%98">为什么内部类可以访问外围类的 private成员</h1>

<p>在这个应用程序中，我们可以看到内部了InnerClass可以对外围类OuterClass的属性进行无缝的访问，尽管它是private修饰的。这是因为当我们在创建某个外围类的内部类对象时，此时内部类对象必定会捕获一个指向那个外围类对象的引用，只要我们在访问外围类的成员时，就会用这个引用来选择外围类的成员。</p>

<p></p>

<h1 id="%E5%86%85%E9%83%A8%E7%B1%BB%E7%9A%84%E5%88%86%E7%B1%BB">内部类的分类</h1>

<p>在Java中内部类主要分为<strong>成员内部类、局部内部类、匿名内部类、静态内部类。</strong></p>

<p></p>
