---
title: java反射机制
tags: java 反射
categories: Java基础知识
abbrlink: 3ecb78ee
date: 2021-03-17 21:25:54
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="-toc" style="margin-left:0px;"> </p>

<p id="java%E5%8F%8D%E5%B0%84%E6%9C%BA%E5%88%B6%E6%A6%82%E8%BF%B0-toc" style="margin-left:0px;"><a href="#java%E5%8F%8D%E5%B0%84%E6%9C%BA%E5%88%B6%E6%A6%82%E8%BF%B0">java反射机制概述</a></p>

<p id="java%E5%8F%8D%E5%B0%84%E6%9C%BA%E5%88%B6%E5%B0%B1%E6%98%AF%E4%B8%80%E5%A5%97API%EF%BC%8C%E6%98%AFjava%E5%9F%BA%E7%A1%80%E7%9A%84%E9%87%8D%E9%9A%BE%E7%82%B9%E3%80%82-toc" style="margin-left:40px;"><a href="#java%E5%8F%8D%E5%B0%84%E6%9C%BA%E5%88%B6%E5%B0%B1%E6%98%AF%E4%B8%80%E5%A5%97API%EF%BC%8C%E6%98%AFjava%E5%9F%BA%E7%A1%80%E7%9A%84%E9%87%8D%E9%9A%BE%E7%82%B9%E3%80%82">java反射机制就是一套API，是java基础的重难点。</a></p>

<p id="%E5%85%B3%E4%BA%8Ejava.lang.Class%E7%B1%BB%E7%9A%84%E7%90%86%E8%A7%A3%E5%B9%B6%E8%8E%B7%E5%8F%96class%E5%AE%9E%E4%BE%8B-toc" style="margin-left:0px;"><a href="#%E5%85%B3%E4%BA%8Ejava.lang.Class%E7%B1%BB%E7%9A%84%E7%90%86%E8%A7%A3%E5%B9%B6%E8%8E%B7%E5%8F%96class%E5%AE%9E%E4%BE%8B">关于java.lang.Class类的理解并获取class实例</a></p>

<p id="%E8%8E%B7%E5%8F%96%E6%AD%A4%E8%BF%90%E8%A1%8C%E6%97%B6%E7%B1%BB%E7%9A%84%E6%96%B9%E6%B3%95-toc" style="margin-left:40px;"><a href="#%E8%8E%B7%E5%8F%96%E6%AD%A4%E8%BF%90%E8%A1%8C%E6%97%B6%E7%B1%BB%E7%9A%84%E6%96%B9%E6%B3%95">获取此运行时类的方法</a></p>

<p id="%E8%8E%B7%E5%8F%96%E4%B9%8B%E5%90%8E%E5%8F%AF%E4%BB%A5%E8%B0%83%E7%94%A8Class%E7%9A%84%E6%96%B9%E6%B3%95%EF%BC%9A-toc" style="margin-left:80px;"><a href="#%E8%8E%B7%E5%8F%96%E4%B9%8B%E5%90%8E%E5%8F%AF%E4%BB%A5%E8%B0%83%E7%94%A8Class%E7%9A%84%E6%96%B9%E6%B3%95%EF%BC%9A">获取之后可以调用Class的方法：</a></p>

<p id="%E7%B1%BB%E7%9A%84%E5%8A%A0%E8%BD%BD%E4%B8%8EClassloader%E7%9A%84%E7%90%86%E8%A7%A3-toc" style="margin-left:0px;"><a href="#%E7%B1%BB%E7%9A%84%E5%8A%A0%E8%BD%BD%E4%B8%8EClassloader%E7%9A%84%E7%90%86%E8%A7%A3">类的加载与Classloader的理解</a></p>

<p id="%E7%A4%BA%E4%BE%8B%EF%BC%9A-toc" style="margin-left:80px;"><a href="#%E7%A4%BA%E4%BE%8B%EF%BC%9A">示例：</a></p>

<p id="%E7%B1%BB%E5%8A%A0%E8%BD%BD%E5%99%A8-toc" style="margin-left:40px;"><a href="#%E7%B1%BB%E5%8A%A0%E8%BD%BD%E5%99%A8">类加载器</a></p>

<p id="%E4%BB%A3%E7%A0%81%E7%A4%BA%E4%BE%8B-toc" style="margin-left:80px;"><a href="#%E4%BB%A3%E7%A0%81%E7%A4%BA%E4%BE%8B">代码示例</a></p>

<p id="classloader%E6%9C%89%E4%BB%80%E4%B9%88%E7%94%A8%EF%BC%9F%EF%BC%9F%EF%BC%9F%EF%BC%9F%E8%AF%BB%E5%8F%96%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6-toc" style="margin-left:80px;"><a href="#classloader%E6%9C%89%E4%BB%80%E4%B9%88%E7%94%A8%EF%BC%9F%EF%BC%9F%EF%BC%9F%EF%BC%9F%E8%AF%BB%E5%8F%96%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6">classloader有什么用？？？？读取配置文件</a></p>

<p id="%E5%88%9B%E5%BB%BA%E8%BF%90%E8%A1%8C%E6%97%B6%E7%B1%BB%E7%9A%84%E5%AF%B9%E8%B1%A1-toc" style="margin-left:0px;"><a href="#%E5%88%9B%E5%BB%BA%E8%BF%90%E8%A1%8C%E6%97%B6%E7%B1%BB%E7%9A%84%E5%AF%B9%E8%B1%A1">创建运行时类的对象</a></p>

<p id="%E8%B0%83%E7%94%A8%E8%BF%90%E8%A1%8C%E6%97%B6%E7%B1%BB%E7%9A%84%E6%8C%87%E5%AE%9A%E7%BB%93%E6%9E%84-toc" style="margin-left:0px;"><a href="#%E8%B0%83%E7%94%A8%E8%BF%90%E8%A1%8C%E6%97%B6%E7%B1%BB%E7%9A%84%E6%8C%87%E5%AE%9A%E7%BB%93%E6%9E%84">调用运行时类的指定结构</a></p>

<hr id="hr-toc" /><h1 id="java%E5%8F%8D%E5%B0%84%E6%9C%BA%E5%88%B6%E6%A6%82%E8%BF%B0">java反射机制概述</h1>

<h2 id="java%E5%8F%8D%E5%B0%84%E6%9C%BA%E5%88%B6%E5%B0%B1%E6%98%AF%E4%B8%80%E5%A5%97API%EF%BC%8C%E6%98%AFjava%E5%9F%BA%E7%A1%80%E7%9A%84%E9%87%8D%E9%9A%BE%E7%82%B9%E3%80%82">java反射机制就是一套API，是java基础的重难点。</h2>

<p><img alt="" height="499" src="https://img-blog.csdnimg.cn/20210314222143242.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1105" /></p>

<blockquote>
<p><code class="language-html">疑问1：通过直接new的方式或反射的方式都可以调用公共的结构，开发中到底用那个？<br />
建议：直接new的方式。</code></p>

<p><br /><code class="language-html">疑问2：反射机制与面向对象中的封装性是不是矛盾的？如何看待两个技术？<br />
不矛盾。面向对象中的封装性指的是对于private属性的方法。属性，你最好不要碰，尽管你可以采用反射机制去调用。</code></p>

<p><code class="language-html">疑问3：什么时候会使用反射的方式？</code></p>

<p><code class="language-html">比如说在web开发的时候，有若干个功能比如注册和登录（这些功能需要特定的类进行功能实现），当前端页面返回功能请求时，此刻正在运行的服务器可以运用反射的方法生成这些类的实例，从而完成注册或者是登录功能，或者是其他的功能。</code></p>
</blockquote>

<h1 id="%E5%85%B3%E4%BA%8Ejava.lang.Class%E7%B1%BB%E7%9A%84%E7%90%86%E8%A7%A3%E5%B9%B6%E8%8E%B7%E5%8F%96class%E5%AE%9E%E4%BE%8B">关于java.lang.Class类的理解并获取class实例</h1>

<p>1.类的加载过程：<br />
程序经过javac.exe命令以后，会生成一个或多个字节码文件(.class结尾，此时字节码文件还没有进入运行状态)，</p>

<p>接着我们使用java.exe命令对某个字节码文件进行解释运行，相当于将某个字节码文件<br />
加载到内存中，此过程就称为类的加载，加载到内存中的类，我们就称为运行时类，此运行时类，就作为Class的一个实例。<br />
2.加载到内存中的运行时类，会缓存一定的时间。在此时间之内，我们可以通过不同的方式来获取此运行时类。</p>

<p>一句话：加载到内存中的运行时类就可以作为class的实例。</p>

<h2 id="%E8%8E%B7%E5%8F%96%E6%AD%A4%E8%BF%90%E8%A1%8C%E6%97%B6%E7%B1%BB%E7%9A%84%E6%96%B9%E6%B3%95">获取此运行时类的方法</h2>

<blockquote>
<pre>
<code class="language-html hljs">//方式一：调用运行时类的属性：.class
        Class clazz1 = Person.class;
        System.out.println(clazz1);
//方式二：通过运行时类的对象,调用getClass()
        Person p1 = new Person();
        Class clazz2 = p1.getClass();
        System.out.println(clazz2);

//方式三：调用Class的静态方法：forName(String classPath)
<strong>        Class clazz3 = Class.forName("com.atguigu.java.Person");
        System.out.println(clazz3);</strong></code></pre>

<pre>
<code class="language-html hljs">//方式四：使用类的加载器：ClassLoader  (了解)
        ClassLoader classLoader = ReflectionTest.class.getClassLoader();
        Class clazz4 = classLoader.loadClass("com.atguigu.java.Person");
        System.out.println(clazz4);
</code></pre>
</blockquote>

<h3 id="%E8%8E%B7%E5%8F%96%E4%B9%8B%E5%90%8E%E5%8F%AF%E4%BB%A5%E8%B0%83%E7%94%A8Class%E7%9A%84%E6%96%B9%E6%B3%95%EF%BC%9A"><code class="language-html">获取之后可以调用Class的方法：</code></h3>

<p><img alt="" height="568" src="https://img-blog.csdnimg.cn/20210315195920969.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1126" /></p>

<h1 id="%E7%B1%BB%E7%9A%84%E5%8A%A0%E8%BD%BD%E4%B8%8EClassloader%E7%9A%84%E7%90%86%E8%A7%A3">类的加载与Classloader的理解</h1>

<p><img alt="" height="606" src="https://img-blog.csdnimg.cn/20210315200101879.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1113" /></p>

<p> 
</p><h2 id="%E7%B1%BB%E5%8A%A0%E8%BD%BD%E5%99%A8">类加载器</h2>


<p><img alt="" height="571" src="https://img-blog.csdnimg.cn/20210315200432379.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1066" /></p>

<p><strong>string核心库 就是用引导类加载器的。</strong></p>

<p><strong>jar包是用扩展类加载器加载的。</strong></p>

<p>系统类加载器一般用来加载我们自己写的类</p>

<h3 id="%E4%BB%A3%E7%A0%81%E7%A4%BA%E4%BE%8B">代码示例</h3>

<blockquote>
<pre>
<code class="language-html hljs">//对于自定义类，使用系统类加载器进行加载
ClassLoader classLoader = ClassLoaderTest.class.getClassLoader();
System.out.println(classLoader);

//调用系统类加载器的getParent()：获取扩展类加载器
ClassLoader classLoader1 = classLoader.getParent();
System.out.println(classLoader1);

//调用扩展类加载器的getParent()：无法获取引导类加载器
//引导类加载器主要负责加载java的核心类库，无法加载自定义类的。
ClassLoader classLoader2 = classLoader1.getParent();
System.out.println(classLoader2);</code></pre>
</blockquote>

<blockquote>
<p><strong>返回的结果</strong></p>

<p>sun.misc.Launcher$AppClassLoader@18b4aac2<br />
sun.misc.Launcher$ExtClassLoader@54bedef2<br />
null<br />
null </p>
</blockquote>

<h3 id="classloader%E6%9C%89%E4%BB%80%E4%B9%88%E7%94%A8%EF%BC%9F%EF%BC%9F%EF%BC%9F%EF%BC%9F%E8%AF%BB%E5%8F%96%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6">classloader有什么用？？？？读取配置文件</h3>

<blockquote>
<p><code class="language-html">Properties pros = new Properties();<br />
//此时的文件默认在当前的module下。<br />
//读取配置文件的方式一：<br />
// FileInputStream fis = new FileInputStream("jdbc.properties");<br />
// pros.load(fis);<br /><br />
//读取配置文件的方式二：使用ClassLoader<br />
//配置文件默认识别为：当前module的src下<br />
ClassLoader classLoader = ClassLoaderTest.class.getClassLoader();<br />
InputStream is = classLoader.getResourceAsStream("jdbc1.properties");<br />
pros.load(is);<br /><br /><br />
String user = pros.getProperty("user");<br />
String password = pros.getProperty("password");<br />
System.out.println("user = " + user + ",password = " + password);</code></p>
</blockquote>

<h1 id="%E5%88%9B%E5%BB%BA%E8%BF%90%E8%A1%8C%E6%97%B6%E7%B1%BB%E7%9A%84%E5%AF%B9%E8%B1%A1">创建运行时类的对象</h1>

<p>只需要知道想创建的类的全类名，就可以调用getInstance（）函数来创建一个运行时类。</p>

<blockquote>
<pre>
public Object getInstance(String classPath) throws Exception {
   Class clazz =  Class.forName(classPath);
   return clazz.newInstance();
}
</pre>
</blockquote>

<h1 id="%E8%B0%83%E7%94%A8%E8%BF%90%E8%A1%8C%E6%97%B6%E7%B1%BB%E7%9A%84%E6%8C%87%E5%AE%9A%E7%BB%93%E6%9E%84">调用运行时类的指定结构</h1>

<p>主要是调用运行时类的一些方法，因为实际开发中用的比较多</p>

<blockquote>
<pre>
public void testMethod() throws Exception {

        Class clazz = Person.class;

        //创建运行时类的对象
        Person p = (Person) clazz.newInstance();

        /*
        1.获取指定的某个方法
        getDeclaredMethod():参数1 ：指明获取的方法的名称  参数2：指明获取的方法的形参列表
         */
        Method show = clazz.getDeclaredMethod("show", String.class);
        //2.保证当前方法是可访问的  <strong>因为有的方法是私有的</strong>
        show.setAccessible(true);

        /*
        3. <strong>调用方法的invoke():参数1：方法的调用者  参数2：给方法形参赋值的实参</strong>
        invoke()的返回值即为对应类中调用的方法的返回值。
         */
        Object returnValue = show.invoke(p,"CHN"); <strong>//String nation = p.show("CHN");</strong>
        System.out.println(returnValue);

        System.out.println("*************如何调用静态方法*****************");

        // private static void showDesc()

        Method showDesc = clazz.getDeclaredMethod("showDesc");
        showDesc.setAccessible(true);
        
//        Object returnVal = showDesc.invoke(null);//因为是静态方法，所以填谁都一样，null也可以。
        Object returnVal = showDesc.invoke(Person.class);  //Person.class是调用者

//如果调用的运行时类中的方法没有返回值，则此invoke()返回null，
        System.out.println(returnVal);//null

    }
</pre>
</blockquote>

<div><span style="color:#ffffff;">与</span><span style="color:#ffffff;">ClassLoader</span><span style="color:#ffffff;">的理解</span></div>

<p>person类的java文件</p>

<blockquote>
<pre>
package com.atguigu.java1;

/**
 * @author shkstart
 * @create 2019 下午 3:12
 */
@MyAnnotation(value="hi")
public class Person extends Creature&lt;String&gt; implements Comparable&lt;String&gt;,MyInterface{

    private String name;
    int age;
    public int id;

    public Person(){}

    @MyAnnotation(value="abc")
    private Person(String name){
        this.name = name;
    }

     Person(String name,int age){
        this.name = name;
        this.age = age;
    }
    @MyAnnotation
    private String show(String nation){
        System.out.println("我的国籍是：" + nation);
        return nation;
    }

    public String display(String interests,int age) throws NullPointerException,ClassCastException{
        return interests + age;
    }


    @Override
    public void info() {
        System.out.println("我是一个人");
    }

    @Override
    public int compareTo(String o) {
        return 0;
    }

    private static void showDesc(){
        System.out.println("我是一个可爱的人");
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", id=" + id +
                '}';
    }
}
</pre>
</blockquote>

<p>资料来源：B站尚硅谷</p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>
