---
title: Spring5之AOP详解
tags: spring java aop
categories: Java基础知识 Spring框架
abbrlink: e4fcddc2
date: 2021-03-19 22:10:07
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="%E7%AE%80%E5%8D%95%E4%BB%8B%E7%BB%8D-toc" style="margin-left:0px;"><a href="#%E7%AE%80%E5%8D%95%E4%BB%8B%E7%BB%8D">简单介绍</a></p>

<p id="%E4%B8%BE%E4%BE%8B%E5%AD%90%E8%AF%B4%E6%98%8E-toc" style="margin-left:40px;"><a href="#%E4%B8%BE%E4%BE%8B%E5%AD%90%E8%AF%B4%E6%98%8E">举例子说明</a></p>

<p id="%E5%BA%95%E5%B1%82%E5%8E%9F%E7%90%86-toc" style="margin-left:0px;"><a href="#%E5%BA%95%E5%B1%82%E5%8E%9F%E7%90%86">底层原理</a></p>

<p id="%E5%AE%9E%E9%99%85%E5%BC%80%E5%8F%91-toc" style="margin-left:0px;"><a href="#%E5%AE%9E%E9%99%85%E5%BC%80%E5%8F%91">实际开发</a></p>

<p id="%E4%B8%80%E8%88%AC%E5%9F%BA%E4%BA%8E%E6%B3%A8%E8%A7%A3%E6%96%B9%E5%BC%8F%E4%BD%BF%E7%94%A8-toc" style="margin-left:40px;"><a href="#%E4%B8%80%E8%88%AC%E5%9F%BA%E4%BA%8E%E6%B3%A8%E8%A7%A3%E6%96%B9%E5%BC%8F%E4%BD%BF%E7%94%A8">一般基于注解方式使用</a></p>

<hr id="hr-toc" /><h1 id="%E7%AE%80%E5%8D%95%E4%BB%8B%E7%BB%8D">简单介绍</h1>

<p>AOP为Aspect Oriented Programming的缩写，意为：面向切面编程，可以理解为一个类中函数之间存在切面，我可以在在这个切面中加入一些功能，使得整个类原有的功能得到增强，同时不改动原先的代码。AOP可以说是对OOP的补充和完善。</p>

<p><em>oop</em>一般指面向对象程序设计---&gt;(Object Oriented Programming)</p>

<div>利用 AOP 可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。</div>

<p></p>

<h2 id="%E4%B8%BE%E4%BE%8B%E5%AD%90%E8%AF%B4%E6%98%8E">举例子说明</h2>

<p><img alt="" height="497" src="https://img-blog.csdnimg.cn/20210319211517662.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="937" /></p>

<p><strong>或者是 日志、监控等功能都可以采用AOP这种侵入性很小的方式加入。</strong></p>

<h1 id="%E5%BA%95%E5%B1%82%E5%8E%9F%E7%90%86">底层原理</h1>

<p>aop的底层原理是动态代理。</p>

<p>代理模式详解：</p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="设计模式之代理模式_trigger333的博客-CSDN博客" href="https://blog.csdn.net/weixin_40757930/article/details/123310990" title="设计模式之代理模式_trigger333的博客-CSDN博客">设计模式之代理模式_trigger333的博客-CSDN博客</a></p>

<pre>
<code class="language-java">package com.atguigu.spring5;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.util.Arrays;

public class JDKProxy { //代理类，就是他增强了原先userDao 的add 和 update功能
    public static void main(String[] args) {

        UserDao userDao = new UserDaoImpl();//创建原先的类的实例，new UserDaoProxy(userDao)实现功能增强，再得到增强后的实例proxyInstance 

        Class[] interfaces = {UserDao.class};
        UserDao proxyInstance = (UserDao)Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces, new UserDaoProxy(userDao));
        int add = proxyInstance.add(1, 2);
        System.out.println(add);
        System.out.println("-------------------------");
        System.out.println(proxyInstance.update("kiki"));

    }


}
class UserDaoProxy implements InvocationHandler{

    private Object obj;
    public UserDaoProxy(Object obj) {
        this.obj = obj;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("方法之前执行...."+"    "+method.getName()+" :传递的参数..."+ Arrays.toString(args));


        Object res = method.invoke(obj, args);
        //方法之后
        System.out.println("方法之后执行...."+obj+"  ");
        return res;

    }
}</code></pre>

<blockquote>
<pre>
package com.atguigu.spring5;

public class UserDaoImpl implements UserDao {
    @Override
    public int add(int a, int b) {
        System.out.println("add方法执行了.....");
        return a+b;
    }

    @Override
    public String update(String id) {
        System.out.println("update方法执行了.....");
        return id;
    }
}
</pre>
</blockquote>

<blockquote>
<p>方法之前执行....    add :传递的参数...[1, 2]<br />
add方法执行了.....<br />
方法之后执行....com.atguigu.spring5.UserDaoImpl@2b193f2d  <br />
3<br />
-------------------------<br />
方法之前执行....    update :传递的参数...[kiki]<br />
update方法执行了.....<br />
方法之后执行....com.atguigu.spring5.UserDaoImpl@2b193f2d  <br />
kiki </p>
</blockquote>

<h1 id="%E5%AE%9E%E9%99%85%E5%BC%80%E5%8F%91">实际开发</h1>

<p>实际开发时直接用现有的框架来做。</p>

<p><img alt="" height="207" src="https://img-blog.csdnimg.cn/20210319213527724.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="895" /></p>

<h2 id="%E4%B8%80%E8%88%AC%E5%9F%BA%E4%BA%8E%E6%B3%A8%E8%A7%A3%E6%96%B9%E5%BC%8F%E4%BD%BF%E7%94%A8">一般基于注解方式使用</h2>

<p>首先引入相关的依赖，再配置xml文件<strong>（扫描所有的注解）</strong>，自动生成代理对象，这样在调用函数时就可以使用代理去进行功能增强。</p>

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd"&gt;
    &lt;!-- 开启注解扫描 --&gt;
    &lt;context:component-scan base-package="com.atguigu.spring5.aopanno"&gt;&lt;/context:component-scan&gt;

    &lt;!-- 开启Aspect生成代理对象--&gt;
    &lt;aop:aspectj-autoproxy&gt;&lt;/aop:aspectj-autoproxy&gt;
&lt;/beans&gt;</pre>
</blockquote>

<p></p>

<pre>
<code class="language-java">package com.atguigu.spring5.aopanno;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.core.annotation.Order;
import org.springframework.stereotype.Component;

//增强的类
@Component
@Aspect  //生成代理对象
@Order(3)  //如果有多个代理都对同一个函数进行增强，那么顺序可以通过注解@order来标识 
public class UserProxy {

    //相同切入点抽取
    @Pointcut(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))")
    public void pointdemo() {

    }

    //前置通知
    //@Before注解表示作为前置通知
    @Before(value = "pointdemo()")
    public void before() {
        System.out.println("before.........");
    }

    //后置通知（返回通知） 有异常不执行
    @AfterReturning(value = "pointdemo()")
    public void afterReturning() {
        System.out.println("afterReturning.........");
    }

    //最终通知 不管有没有异常都会执行
    @After(value = "pointdemo()")
    public void after() {
        System.out.println("after.........");
    }

    //异常通知
    @AfterThrowing(value = "pointdemo()")
    public void afterThrowing() {
        System.out.println("afterThrowing.........");
    }

    //环绕通知
    @Around(value = "pointdemo()")
    public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("环绕之前.........");

        //被增强的方法执行
        proceedingJoinPoint.proceed();

        System.out.println("环绕之后.........");
    }
}
</code></pre>

<p></p>

<p>资料来源：B站尚硅谷</p>

<p> </p>
