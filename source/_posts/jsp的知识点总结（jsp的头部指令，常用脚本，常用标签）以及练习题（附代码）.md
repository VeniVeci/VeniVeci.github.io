---
title: jsp的知识点总结（jsp的头部指令，常用脚本，常用标签）以及练习题（附代码）
tags: java jsp
categories: Java基础知识
abbrlink: b772a0ea
date: 2021-02-28 16:19:52
---

<!--more-->

<h1>jsp独特的语法、常用方法和应用场景。</h1>

<p id="main-toc"><strong>目录</strong></p>

<p id="jsp%E7%9A%84%E5%A4%B4%E9%83%A8%E6%8C%87%E4%BB%A4-toc" style="margin-left:40px;"><a href="#jsp%E7%9A%84%E5%A4%B4%E9%83%A8%E6%8C%87%E4%BB%A4">jsp的头部指令</a></p>

<p id="jsp%E7%9A%84%E5%B8%B8%E7%94%A8%E8%84%9A%E6%9C%AC-toc" style="margin-left:40px;"><a href="#jsp%E7%9A%84%E5%B8%B8%E7%94%A8%E8%84%9A%E6%9C%AC">jsp的常用脚本</a></p>

<p id="i.%E5%A3%B0%E6%98%8E%E8%84%9A%E6%9C%AC%EF%BC%88%E6%9E%81%E5%B0%91%E4%BD%BF%E7%94%A8%EF%BC%89-toc" style="margin-left:80px;"><a href="#i.%E5%A3%B0%E6%98%8E%E8%84%9A%E6%9C%AC%EF%BC%88%E6%9E%81%E5%B0%91%E4%BD%BF%E7%94%A8%EF%BC%89">i.声明脚本（极少使用）</a></p>

<p id="i.%E8%A1%A8%E8%BE%BE%E5%BC%8F%E8%84%9A%E6%9C%AC%EF%BC%88%E5%B8%B8%E7%94%A8%EF%BC%89-toc" style="margin-left:80px;"><a href="#i.%E8%A1%A8%E8%BE%BE%E5%BC%8F%E8%84%9A%E6%9C%AC%EF%BC%88%E5%B8%B8%E7%94%A8%EF%BC%89">i.表达式脚本（常用）</a></p>

<p id="iii.%E4%BB%A3%E7%A0%81%E8%84%9A%E6%9C%AC-toc" style="margin-left:80px;"><a href="#iii.%E4%BB%A3%E7%A0%81%E8%84%9A%E6%9C%AC">iii.代码脚本（常用）</a></p>

<p id="%E5%B8%B8%E7%94%A8%E6%A0%87%E7%AD%BE-toc" style="margin-left:40px;"><a href="#%E5%B8%B8%E7%94%A8%E6%A0%87%E7%AD%BE">常用标签</a></p>

<p id="%E9%9D%99%E6%80%81%E5%8C%85%E5%90%AB%3A-toc" style="margin-left:80px;"><a href="#%E9%9D%99%E6%80%81%E5%8C%85%E5%90%AB%3A">静态包含</a></p>

<p id="%E5%8A%A8%E6%80%81%E5%8C%85%E5%90%AB%EF%BC%9A-toc" style="margin-left:80px;"><a href="#%E5%8A%A8%E6%80%81%E5%8C%85%E5%90%AB%EF%BC%9A">动态包含</a></p>

<p id="%E8%AF%B7%E6%B1%82%E8%BD%AC%E5%8F%91-toc" style="margin-left:80px;"><a href="#%E8%AF%B7%E6%B1%82%E8%BD%AC%E5%8F%91">请求转发标签</a></p>

<p id="jsp%E7%9A%84%E7%BB%83%E4%B9%A0%E9%A2%98%EF%BC%9A-toc" style="margin-left:40px;"><a href="#jsp%E7%9A%84%E7%BB%83%E4%B9%A0%E9%A2%98%EF%BC%9A">jsp的练习题：</a></p>

<p id="%E7%BB%83%E4%B9%A0%E4%B8%80%EF%BC%9A%E5%9C%A8%20jsp%20%E9%A1%B5%E9%9D%A2%E4%B8%AD%E8%BE%93%E5%87%BA%E4%B9%9D%E4%B9%9D%E4%B9%98%E6%B3%95%E5%8F%A3%E8%AF%80%E8%A1%A8-toc" style="margin-left:80px;"><a href="#%E7%BB%83%E4%B9%A0%E4%B8%80%EF%BC%9A%E5%9C%A8%20jsp%20%E9%A1%B5%E9%9D%A2%E4%B8%AD%E8%BE%93%E5%87%BA%E4%B9%9D%E4%B9%9D%E4%B9%98%E6%B3%95%E5%8F%A3%E8%AF%80%E8%A1%A8">练习一：在 jsp 页面中输出九九乘法口诀表</a></p>

<p id="%E7%BB%83%E4%B9%A0%E4%BA%8C%EF%BC%9Ajsp%20%E8%BE%93%E5%87%BA%E4%B8%80%E4%B8%AA%E8%A1%A8%E6%A0%BC%EF%BC%8C%E9%87%8C%E9%9D%A2%E6%9C%89%2010%20%E4%B8%AA%E5%AD%A6%E7%94%9F%E4%BF%A1%E6%81%AF%E3%80%82-toc" style="margin-left:80px;"><a href="#%E7%BB%83%E4%B9%A0%E4%BA%8C%EF%BC%9Ajsp%20%E8%BE%93%E5%87%BA%E4%B8%80%E4%B8%AA%E8%A1%A8%E6%A0%BC%EF%BC%8C%E9%87%8C%E9%9D%A2%E6%9C%89%2010%20%E4%B8%AA%E5%AD%A6%E7%94%9F%E4%BF%A1%E6%81%AF%E3%80%82">练习二：jsp 输出一个表格，里面有 10 个学生信息。</a></p>

<p id="%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF%EF%BC%9A-toc" style="margin-left:80px;"><a href="#%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF%EF%BC%9A">应用场景：</a></p>

<hr id="hr-toc" /><h2 id="jsp%E7%9A%84%E5%A4%B4%E9%83%A8%E6%8C%87%E4%BB%A4">jsp的头部指令</h2>

<p>就是包含在&lt;%@ page    %&gt;的内容，很少使用，可忽略。</p>

<p><img alt="" height="418" src="https://img-blog.csdnimg.cn/20210227210033103.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="957" /></p>

<p><img alt="" height="312" src="https://img-blog.csdnimg.cn/20210227210737152.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="842" /></p>

<p><img alt="" height="54" src="https://img-blog.csdnimg.cn/20210227211028618.png" width="942" /></p>

<p>这个比较常用，可以在运行的时候提示用户，转到错误页面，常见的找不到服务器就是这样做的。</p>

<h2 id="jsp%E7%9A%84%E5%B8%B8%E7%94%A8%E8%84%9A%E6%9C%AC">jsp的常用脚本</h2>

<p><em>脚本</em>（Script），是使用一种特定的描述性语言，依据一定的格式编写的可执行文件。</p>

<h3 id="i.%E5%A3%B0%E6%98%8E%E8%84%9A%E6%9C%AC%EF%BC%88%E6%9E%81%E5%B0%91%E4%BD%BF%E7%94%A8%EF%BC%89">i.声明脚本（极少使用）</h3>

<p>声明脚本的格式是：&lt;%!声明java代码%〉<br />
作用：可以给jsp翻译出来的java类定义属性和方法甚至是静态代码块。内部类等。</p>

<p>意思就是在jsp里面用Java来写一些属性和方法，比如：</p>

<p><img alt="" height="200" src="https://img-blog.csdnimg.cn/20210228161103813.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="562" /></p>

<h3 id="i.%E8%A1%A8%E8%BE%BE%E5%BC%8F%E8%84%9A%E6%9C%AC%EF%BC%88%E5%B8%B8%E7%94%A8%EF%BC%89">i.表达式脚本（常用）</h3>

<p>表达式脚本的格式是：&lt;%=表达式%&gt;<br />
表达式脚本的作用是：在jsp页面上输出数据。<br />
表达式脚本的特点：<br />
1、所有的表达式脚本都会被翻译到 jspservice()方法中<br />
2、表达式脚本都会被翻译成为out. print)输出到页面上<br />
3、由于表达式脚本翻译的内容都在_ jspservice()方法中，所以 jspservice)方法中的对象都可以直接使用。<br />
4、表达式脚本中的表达式不能以分号结束</p>

<p>&lt;%=12&gt;就会在浏览器中输出12这个数。</p>

<p><img alt="" height="259" src="https://img-blog.csdnimg.cn/20210228090708940.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="940" /></p>

<h3 id="iii.%E4%BB%A3%E7%A0%81%E8%84%9A%E6%9C%AC">iii.代码脚本（常用）</h3>

<p>代码脚本的格式是：&lt;% %&gt;java语句<br />
代码脚本的作用是：可以在jsp页面中，编写我们自己需要的功能（写的是java语句）。<br />
代码脚本的特点是：<br />
1、代码脚本翻译之后都在 jspservice方法中<br />
代码脚本由于翻译到jsp Service()方法中，所以在 jspservice0方法中的现有对象都可以直接使用<br />
3、<strong>还可以由多个代码脚本块组合完成一个完整的java语句。</strong><br />
4、代码脚本还可以和表达式脚本一起组合使用，在jsp页面上输出数据</p>

<p><strong>和表达式脚本差不多，可以结合起来使用，<span style="color:#f33b45;">这样相当于在jsp中进行java编程。</span></strong></p>

<blockquote>
<p>&lt;%!<br />
    int a = 10;                          //声明脚本  可以用java中的单行注释  //<br />
%&gt;<br /><strong>&lt;%<br />
    if(1&gt;0){                             //代码脚本，一个if语句<br />
%&gt;<br />
&lt;div&gt; 1&gt;0 &lt;/div&gt;<br />
&lt;%<br />
    }else{<br />
%&gt;<br />
&lt;div&gt; 1&lt;0 &lt;/div&gt;                &lt;%--普通的html语句--%&gt;<br />
&lt;%<br />
    }<br />
%&gt;</strong></p>

<p>&lt;%= 1&gt;0 %&gt;                      &lt;%--表达式脚本--%&gt;<br />
&lt;div&gt; &lt;/div&gt;<br />
&lt;%= a %&gt;</p>
</blockquote>

<blockquote>
<p>上面的加粗部分就翻译成了下面这段 ，验证了：<strong>jsp代码语句可以由多个代码脚本块组合完成一个完整的java语句。</strong></p>

<p>if(1&gt;0){</p>

<p>      out.write("\r\n");<br />
      out.write("&lt;div&gt; 1&gt;0 &lt;/div&gt;\r\n");</p>

<p>    }else{</p>

<p>      out.write("\r\n");<br />
      out.write("&lt;div&gt; 1&lt;0 &lt;/div&gt;\r\n");</p>

<p>    }</p>
</blockquote>

<p id="%E9%82%A3%E4%B9%88%E5%9C%A8jsp%E4%B8%AD%E6%80%8E%E4%B9%88%E6%B3%A8%E9%87%8A%E5%91%A2%EF%BC%8C%3C%25--%20%E8%BF%99%E6%98%AF%20jsp%20%E6%B3%A8%E9%87%8A%20--%25%3E">那么在jsp中怎么注释呢，<span style="color:#808080;"><em>&lt;%-- </em></span><span style="color:#808080;"><em>这是 </em></span><span style="color:#808080;"><em>jsp </em></span><span style="color:#808080;"><em>注释 </em></span><span style="color:#808080;"><em>--%&gt;</em></span></p>

<h2 id="%E5%B8%B8%E7%94%A8%E6%A0%87%E7%AD%BE">常用标签</h2>

<h3 id="%E9%9D%99%E6%80%81%E5%8C%85%E5%90%AB%3A"><strong>静态包含</strong></h3>

<p>使用场景：一般的网站最下面的内容基本都是一些友情链接，邮箱，联系方式等等，这份数据在这个网站的所有页面都一样，所以没有必要每个jsp页面都重复写这段代码，只需要维护一份就够了。</p>

<blockquote>
<pre>
<code class="language-html hljs">&lt;%@include file="/footer.jsp %&gt; 可以在footer.jsp 中写好描述尾部的代码，再利用这句话来包含就行了</code></pre>

<p>具体实现方式：在浏览器翻译的时候找到footer.jsp文件，然后翻译到浏览器页面。</p>
</blockquote>

<h3 id="%E5%8A%A8%E6%80%81%E5%8C%85%E5%90%AB%EF%BC%9A">动态包含</h3>

<pre>
<code class="language-html hljs">&lt;jsp:include page="footer.jsp"&gt;&lt;/jsp:include&gt;</code></pre>

<blockquote>
<p>动态包含的特点<br />
1动态包含会把包含的jsp页面地翻译成为java代码<br />
2动态包含底层代码使用如下代码去週用被包含的jsp页面进行输出。<br />
org.apache.jasper.runtime.JspRuntimeLibrary.include(request, response, "footer.jsp", out, false);<br />
动态包含，还可以传递参数，比如<br /><br />
&lt;jsp:include page="footer.jsp"&gt;</p>

<p>&lt;jsp: param name="username "value="bb"/&gt;<br />
&lt;jsp: param name="password"value="root"/&gt;</p>

<p>&lt;/jsp:include&gt;<br />
 </p>
</blockquote>

<p><img alt="" height="653" src="https://img-blog.csdnimg.cn/20210228114255193.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1195" /></p>

<p><strong>动态包含的实现是在包含语句的地方调用一个函数去输出页脚信息，所以它可以利用jsp中的内置对象传递参数。</strong></p>

<h3 id="%E8%AF%B7%E6%B1%82%E8%BD%AC%E5%8F%91">请求转发标签</h3>

<p><img alt="" height="99" src="https://img-blog.csdnimg.cn/20210228114851722.png" width="907" /></p>

<p>可以转化为：</p>

<p><img alt="" height="37" src="https://img-blog.csdnimg.cn/20210228114907828.png" width="577" /></p>

<p>程序变得很简洁。</p>

<h2 id="jsp%E7%9A%84%E7%BB%83%E4%B9%A0%E9%A2%98%EF%BC%9A">jsp的练习题：</h2>

<h3 id="%E7%BB%83%E4%B9%A0%E4%B8%80%EF%BC%9A%E5%9C%A8%20jsp%20%E9%A1%B5%E9%9D%A2%E4%B8%AD%E8%BE%93%E5%87%BA%E4%B9%9D%E4%B9%9D%E4%B9%98%E6%B3%95%E5%8F%A3%E8%AF%80%E8%A1%A8"><span style="color:#000000;"><strong>练习一：在 </strong></span><span style="color:#000000;"><strong>jsp </strong></span><span style="color:#000000;"><strong>页面中输出九九乘法口诀表</strong></span></h3>

<div>思路：使用代码脚本for循环、表达式脚本和常用的html语句实现。</div>

<blockquote>
<div>
<pre>
<code class="language-html hljs">&lt;body&gt;
&lt;h1 align="center"&gt;九九乘法口诀表&lt;/h1&gt;
&lt;table align="center"&gt; 居中表格
    &lt;%for (int i = 1; i &lt; 10; i++) {%&gt;
    &lt;tr&gt; 循环显示i行
        &lt;%for (int j = 1; j &lt; i+1; j++) { %&gt;
            &lt;td&gt;&lt;%=j%&gt; x &lt;%=i%&gt; = &lt;%=j*i%&gt;&lt;/td&gt; 每i行是j个单元格
        &lt;%}%&gt;
    &lt;/tr&gt;

    &lt;%}%&gt;

&lt;/table&gt;
&lt;/body&gt;</code></pre>
</div>
</blockquote>

<p><img alt="" height="317" src="https://img-blog.csdnimg.cn/20210228152922681.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="752" /></p>

<h3 id="%E7%BB%83%E4%B9%A0%E4%BA%8C%EF%BC%9Ajsp%20%E8%BE%93%E5%87%BA%E4%B8%80%E4%B8%AA%E8%A1%A8%E6%A0%BC%EF%BC%8C%E9%87%8C%E9%9D%A2%E6%9C%89%2010%20%E4%B8%AA%E5%AD%A6%E7%94%9F%E4%BF%A1%E6%81%AF%E3%80%82"><span style="color:#000000;"><strong>练习二：</strong></span><span style="color:#000000;"><strong>jsp </strong></span><span style="color:#000000;"><strong>输出一个表格，里面有 </strong></span><span style="color:#000000;"><strong>10 </strong></span><span style="color:#000000;"><strong>个学生信息。</strong></span></h3>

<div><img alt="" height="301" src="https://img-blog.csdnimg.cn/20210228155727201.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="661" /></div>

<div>student类的构造</div>

<blockquote>
<div>
<pre>
<code class="language-html hljs">package pojo;

public class Student {
    private String name;
    private int age;
    private String phone;

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

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }

    public Student(String name, int age, String phone) {
        this.name = name;
        this.age = age;
        this.phone = phone;
    }

    public Student() {
    }
}
</code></pre>
</div>
</blockquote>

<div>test2.jsp先生成一个学生的列表，再显示到浏览器中。</div>

<blockquote>
<div>
<pre>
<code class="language-html hljs">-------------------格式---------------------
&lt;head&gt;
    &lt;title&gt;Title&lt;/title&gt;
    &lt;style&gt;
        table{
            border: 1px black solid;
            width: 600px;
            border-collapse: collapse; /*边框合并*/
        }
        td,th{
            border: 1px black solid;
            text-align: center;
        }

    &lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
-------------------生成学生列表---------------------
&lt;%

    ArrayList&lt;Student&gt; list = new ArrayList&lt;Student&gt;();
    for (int i = 0; i &lt; 10; i++) {
        list.add(new Student("name" + i + 1, i + 18, "phone" + i + 1));
    }

%&gt;
-------------------生成表格显示---------------------
&lt;table align="center"&gt;


        &lt;tr&gt;
            &lt;td&gt;姓名&lt;/td&gt;
            &lt;td&gt;年龄&lt;/td&gt;
            &lt;td&gt;电话&lt;/td&gt;
        &lt;/tr&gt;
    &lt;%
        for (Student student : list) {
    %&gt;
    &lt;tr&gt;
        &lt;td&gt;&lt;%=student.getName()%&gt;
        &lt;/td&gt;
        &lt;td&gt;&lt;%=student.getAge()%&gt;
        &lt;/td&gt;
        &lt;td&gt;&lt;%=student.getPhone()%&gt;
        &lt;/td&gt;

    &lt;/tr&gt;
    &lt;%
        }
    %&gt;
&lt;/table&gt;

&lt;/body&gt;</code></pre>
</div>
</blockquote>

<h3 id="%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF%EF%BC%9A"><strong>应用场景：</strong></h3>

<p><img alt="" height="423" src="https://img-blog.csdnimg.cn/20210228160046512.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1191" /></p>

<p><strong>当我们点击搜索后，servlet程序获得搜索的参数，找到结果后保存到request域中，转到jsp中，jsp在显示的时候可以从req中得到搜索到的结果，显示出来。</strong></p>

<p><strong>比起直接用servlet程序去显示要方便的多。</strong></p>

<p><strong>资料来源：B站尚硅谷。</strong></p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>

<p> </p>
