---
title: 《轻量级分布式 RPC 框架》笔记 (源码+注释)
tags: rpc 分布式 java netty zookeeper
categories: Spring框架 RPC Netty
abbrlink: 1344c5f
date: 2022-02-21 22:43:52
---

<!--more-->

<p>在学习RPC的时候发现了一个很不错的项目，以下是我在学习这个项目的过程中遇到的疑问和问题的答案。</p>

<p id="main-toc"><strong>目录</strong></p>

<p id="%E5%8E%9F%E6%96%87-toc" style="margin-left:0px;"><a href="#%E5%8E%9F%E6%96%87">原文</a></p>

<p id="%E9%A1%B9%E7%9B%AE%E5%9C%B0%E5%9D%80%EF%BC%9A-toc" style="margin-left:40px;"><a href="#%E9%A1%B9%E7%9B%AE%E5%9C%B0%E5%9D%80%EF%BC%9A">项目地址：</a></p>

<p id="%E9%A1%B9%E7%9B%AE%E6%A6%82%E6%8B%AC%EF%BC%9A-toc" style="margin-left:40px;"><a href="#%E9%A1%B9%E7%9B%AE%E6%A6%82%E6%8B%AC%EF%BC%9A">项目概括：</a></p>

<p id="%E5%A6%82%E4%BD%95%E6%8A%8A%E8%BF%99%E4%B8%AA%E9%A1%B9%E7%9B%AE%E8%B7%91%E8%B5%B7%E6%9D%A5%EF%BC%9F-toc" style="margin-left:0px;"><a href="#%E5%A6%82%E4%BD%95%E6%8A%8A%E8%BF%99%E4%B8%AA%E9%A1%B9%E7%9B%AE%E8%B7%91%E8%B5%B7%E6%9D%A5%EF%BC%9F">如何把这个项目跑起来？</a></p>

<p id="%E4%B8%80%E4%B8%AARPC%E8%AF%B7%E6%B1%82%E4%B9%8B%E5%90%8E%E4%BC%9A%E5%8F%91%E7%94%9F%E4%BB%80%E4%B9%88%EF%BC%9F-toc" style="margin-left:0px;"><a href="#%E4%B8%80%E4%B8%AARPC%E8%AF%B7%E6%B1%82%E4%B9%8B%E5%90%8E%E4%BC%9A%E5%8F%91%E7%94%9F%E4%BB%80%E4%B9%88%EF%BC%9F">一个RPC请求之后会发生什么？</a></p>

<p id="%E9%A1%B9%E7%9B%AE%E7%9A%84%E5%85%88%E4%BF%AE%E7%9F%A5%E8%AF%86-toc" style="margin-left:0px;"><a href="#%E9%A1%B9%E7%9B%AE%E7%9A%84%E5%85%88%E4%BF%AE%E7%9F%A5%E8%AF%86">项目的先修知识</a></p>

<p id="spring%E5%A6%82%E4%BD%95%E5%8E%BB%E5%AE%9E%E7%8E%B0%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5%EF%BC%9F-toc" style="margin-left:40px;"><a href="#spring%E5%A6%82%E4%BD%95%E5%8E%BB%E5%AE%9E%E7%8E%B0%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5%EF%BC%9F">spring如何去实现依赖注入？</a></p>

<p id="spring%E7%9A%84%E6%B3%A8%E8%A7%A3%E5%A6%82%E4%BD%95%E5%8F%91%E6%8C%A5%E4%BD%9C%E7%94%A8%EF%BC%8C%E5%A6%82%E4%BD%95%E8%87%AA%E5%AE%9A%E4%B9%89%E6%B3%A8%E8%A7%A3%EF%BC%9F-toc" style="margin-left:40px;"><a href="#spring%E7%9A%84%E6%B3%A8%E8%A7%A3%E5%A6%82%E4%BD%95%E5%8F%91%E6%8C%A5%E4%BD%9C%E7%94%A8%EF%BC%8C%E5%A6%82%E4%BD%95%E8%87%AA%E5%AE%9A%E4%B9%89%E6%B3%A8%E8%A7%A3%EF%BC%9F">spring的注解如何发挥作用，如何自定义注解？</a></p>

<p id="Netty%20%E5%AD%A6%E4%B9%A0-toc" style="margin-left:40px;"><a href="#Netty%20%E5%AD%A6%E4%B9%A0">Netty 学习</a></p>

<p id="Protostuff-toc" style="margin-left:40px;"><a href="#Protostuff">Protostuff</a></p>

<p id="ZooKeeper-toc" style="margin-left:40px;"><a href="#ZooKeeper">ZooKeeper</a></p>

<p id="%E5%85%B6%E4%BB%96%E9%97%AE%E9%A2%98-toc" style="margin-left:0px;"><a href="#%E5%85%B6%E4%BB%96%E9%97%AE%E9%A2%98">其他问题</a></p>

<p id="%E7%BC%96%E7%A0%81%E5%92%8C%E5%BA%8F%E5%88%97%E5%8C%96%E7%9A%84%E5%8C%BA%E5%88%AB%EF%BC%9F-toc" style="margin-left:40px;"><a href="#%E7%BC%96%E7%A0%81%E5%92%8C%E5%BA%8F%E5%88%97%E5%8C%96%E7%9A%84%E5%8C%BA%E5%88%AB%EF%BC%9F">编码和序列化的区别？</a></p>

<p id="%E8%BF%99%E4%B8%AA%E9%A1%B9%E7%9B%AE%E5%92%8C%E4%B9%8B%E5%89%8D%E5%86%99%E7%9A%84RPC%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB%EF%BC%9F-toc" style="margin-left:40px;"><a href="#%E8%BF%99%E4%B8%AA%E9%A1%B9%E7%9B%AE%E5%92%8C%E4%B9%8B%E5%89%8D%E5%86%99%E7%9A%84RPC%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB%EF%BC%9F">这个项目和之前写的RPC有什么区别？</a></p>

<p id="%E6%94%B9%E8%BF%9B%E7%82%B9-toc" style="margin-left:40px;"><a href="#%E6%94%B9%E8%BF%9B%E7%82%B9">改进点</a></p>

<hr id="hr-toc" /><h1 id="%E5%8E%9F%E6%96%87">原文</h1>

<p><a class="has-card" data-link-desc="RPC，即 Remote Procedure Call（远程过程调用），说得通俗一点就是：调用远程计算机上的服务，就像调用本地服务一样。 RPC 可基于 HTTP 或 TCP 协议，Web Service 就是基于 HTTP 协议的 RPC，它具有良好的跨平台性，但其性能却不如基于 TCP 协议的 RPC。会两方面会直接影响 RPC 的性能，一是传输方式，二是序列化。 众所..." data-link-icon="https://static.oschina.net/new-osc/img/favicon.ico" data-link-title="轻量级分布式 RPC 框架 - 黄勇 - OSCHINA - 中文开源技术交流社区" href="https://my.oschina.net/huangyong/blog/361751" title="轻量级分布式 RPC 框架 - 黄勇 - OSCHINA - 中文开源技术交流社区"><span class="link-card-box"><span class="link-title">轻量级分布式 RPC 框架 - 黄勇 - OSCHINA - 中文开源技术交流社区</span><span class="link-desc">RPC，即 Remote Procedure Call（远程过程调用），说得通俗一点就是：调用远程计算机上的服务，就像调用本地服务一样。 RPC 可基于 HTTP 或 TCP 协议，Web Service 就是基于 HTTP 协议的 RPC，它具有良好的跨平台性，但其性能却不如基于 TCP 协议的 RPC。会两方面会直接影响 RPC 的性能，一是传输方式，二是序列化。 众所...</span><span class="link-link"><img alt="" class="link-link-icon" src="https://static.oschina.net/new-osc/img/favicon.ico" />https://my.oschina.net/huangyong/blog/361751</span></span></a></p>

<h2 id="%E9%A1%B9%E7%9B%AE%E5%9C%B0%E5%9D%80%EF%BC%9A">项目地址：</h2>

<p>原项目的地址：</p>

<p id="rpc%3A%20%E8%BD%BB%E9%87%8F%E7%BA%A7%E5%88%86%E5%B8%83%E5%BC%8F%20RPC%20%E6%A1%86%E6%9E%B6"><a class="has-card" data-link-icon="https://assets.gitee.com/assets/favicon-9007bd527d8a7851c8330e783151df58.ico" data-link-title="rpc: 轻量级分布式 RPC 框架" href="http://git.oschina.net/huangyong/rpc" title="rpc: 轻量级分布式 RPC 框架"><span class="link-card-box"><span class="link-title">rpc: 轻量级分布式 RPC 框架</span><span class="link-link"><img alt="" class="link-link-icon" src="https://assets.gitee.com/assets/favicon-9007bd527d8a7851c8330e783151df58.ico" />http://git.oschina.net/huangyong/rpc</span></span></a>加了注释的项目地址：</p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" data-link-title="RPCfromHY: 这个项目主要来自https://my.oschina.net/huangyong/blog/361751 http://git.oschina.net/huangyong/rpc （原版项目地址）在此基础上加了一些注释，方便理解。" href="https://gitee.com/trigger333/rpcfrom-hy" title="RPCfromHY: 这个项目主要来自https://my.oschina.net/huangyong/blog/361751 http://git.oschina.net/huangyong/rpc （原版项目地址）在此基础上加了一些注释，方便理解。">RPCfromHY: 这个项目主要来自https://my.oschina.net/huangyong/blog/361751 http://git.oschina.net/huangyong/rpc （原版项目地址）在此基础上加了一些注释，方便理解。</a></p>

<p></p>

<h2 id="%E9%A1%B9%E7%9B%AE%E6%A6%82%E6%8B%AC%EF%BC%9A">项目概括：</h2>

<p>本文通过<strong> Spring + Netty + Protostuff + ZooKeeper</strong> 实现了一个轻量级 RPC 框架，使用 Spring 提供依赖注入与参数配置，使用 Netty 实现 NIO 方式的数据传输，使用 Protostuff 实现对象序列化，使用 ZooKeeper 实现服务注册与发现。使用该框架，可将服务部署到分布式环境中的任意节点上，客户端通过远程接口来调用服务端的具体实现，让服务端与客户端的开发完全分离，为实现大规模分布式应用提供了基础支持。</p>

<p></p>

<h1 id="%E5%A6%82%E4%BD%95%E6%8A%8A%E8%BF%99%E4%B8%AA%E9%A1%B9%E7%9B%AE%E8%B7%91%E8%B5%B7%E6%9D%A5%EF%BC%9F">如何把这个项目跑起来？</h1>

<p>首先下载和学习ZooKeeper ，参考这篇文章</p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" data-link-title="Zookeeper基本原理+通俗理解+安装使用_trigger的博客-CSDN博客" href="https://blog.csdn.net/weixin_40757930/article/details/122993927" title="Zookeeper基本原理+通俗理解+安装使用_trigger的博客-CSDN博客">Zookeeper基本原理+通俗理解+安装使用_trigger的博客-CSDN博客</a></p>

<p>1.启动ZooKeeper 的服务器，这样服务注册和发现中心就启动了，如果要使用该服务，只需要访问127.0.0.1:2181即可。</p>

<p><img alt="" height="455" src="https://img-blog.csdnimg.cn/824fc92958b746dd951b41901d0a01f8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_15,color_FFFFFF,t_70,g_se,x_16" width="535" /></p>

<p>2.启动红字所标识的 rpc-sample-server，命令行窗口会出现：</p>

<pre>
<code>start server
connect zookeeper
create address node: /registry/com.xxx.rpc.sample.api.HelloService/address-0000000001
register service: com.xxx.rpc.sample.api.HelloService =&gt; 127.0.0.1:8000
create address node: /registry/com.xxx.rpc.sample.api.HelloService-sample.hello2/address-0000000001
register service: com.xxx.rpc.sample.api.HelloService-sample.hello2 =&gt; 127.0.0.1:8000
server started on port 8000
</code></pre>

<p>3.启动rpc-sample-client中的第一个测试类即可。</p>

<p><img alt="" height="331" src="https://img-blog.csdnimg.cn/821221c8a34646a3af8fcdd3b8f11045.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_20,color_FFFFFF,t_70,g_se,x_16" width="896" /></p>

<p> 会出现：</p>

<pre>
<code>connect zookeeper
get only address node: address-0000000001
discover service: com.xxx.rpc.sample.api.HelloService =&gt; 127.0.0.1:8000
time: 653ms
Hello! World
connect zookeeper
get only address node: address-0000000001
discover service: com.xxx.rpc.sample.api.HelloService-sample.hello2 =&gt; 127.0.0.1:8000
time: 17ms
你好! 世界</code></pre>

<p>这样就启动了这个项目。</p>

<p></p>

<h1 id="%E4%B8%80%E4%B8%AARPC%E8%AF%B7%E6%B1%82%E4%B9%8B%E5%90%8E%E4%BC%9A%E5%8F%91%E7%94%9F%E4%BB%80%E4%B9%88%EF%BC%9F">一个RPC请求之后会发生什么？</h1>

<p>这个项目总的流程是：</p>

<p>ZooKeeper启动，rpcServer启动，<strong>把特定目录下的远程服务加入到handlerMap</strong>（本地缓存）中，同时利用zk客户端向zk中注册这些服务，之后监听对应的端口8000。</p>

<p>客户端发起请求之前，首先<strong>需要获取动态代理对象</strong>，这个对象里面有<strong>zk客户端</strong>（可以通过它获得服务的地址）。</p>

<p>在调用这个动态代理对象的某一个方法时，会先利用zk客户端<strong>去zk中查找是否有这个服务</strong>，如果有，得到其地址端口比如127.0.0.1:8000，<strong>新建一个rpcClient</strong>，绑定对应的地址端口，连接rpcServer，发送相应的信息请求服务。</p>

<p></p>

<p>rpc-sample-client包下的HelloClient类是一个客户端类，用来测试。</p>

<p>具体测试代码为</p>

<pre>
<code class="language-java">public class HelloClient {

    public static void main(String[] args) throws Exception {

        ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
        RpcProxy rpcProxy = context.getBean(RpcProxy.class);

        HelloService helloService = rpcProxy.create(HelloService.class);
        String result = helloService.hello("World");
        System.out.println(result);

    }
}</code></pre>

<p>前两行代码获得了rpcProxy，debug如下，运行完create对象后，知道了服务的地址在127.0.0.1:8000端口。</p>

<p><img alt="" height="538" src="https://img-blog.csdnimg.cn/c234a3e27ef84dc0a9a5bf5ac70ae920.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_20,color_FFFFFF,t_70,g_se,x_16" width="1137" /></p>

<blockquote>
<p>String result = helloService.hello("World");</p>
</blockquote>

<p>这句代码调用了invoke函数，<br />
函数中生成了一个rpcClient，首先把要访问的函数和输入参数等信息包装成RpcRequest“请求消息”对象，序列化之后发送给rpcServer并等待结果。</p>

<p>rpcserver始终在监听着8000端口，有消息过来，先进行解码（反序列化），得到RpcRequest“请求消息”对象，将其中的信息（函数，输入参数等）取出，进行反射调用得到结果，再把这个结果包装成RpcResponse“响应消息”对象，发回给客户端。</p>

<p></p>

<h1 id="%E9%A1%B9%E7%9B%AE%E7%9A%84%E5%85%88%E4%BF%AE%E7%9F%A5%E8%AF%86">项目的先修知识</h1>

<p><strong>这个项目的技术选型是Spring + Netty + Protostuff + ZooKeeper，所以这四个是需要掌握的，其中Protostuff </strong>不需要很多时间，大概了解一下即可。</p>

<h2 id="spring%E5%A6%82%E4%BD%95%E5%8E%BB%E5%AE%9E%E7%8E%B0%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5%EF%BC%9F">spring如何去实现依赖注入？</h2>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" data-link-title="Spring5之IOC详解_trigger的博客-CSDN博客" href="https://blog.csdn.net/weixin_40757930/article/details/114806640" title="Spring5之IOC详解_trigger的博客-CSDN博客">Spring5之IOC详解_trigger的博客-CSDN博客</a></p>

<p>代码中主要的依赖注入有两处，一处是测试类获得rpcClient，另一处是rpcServer启动的时候，在启动时采用rpcClient、rpcServer中的属性都是通过xml文件配置的方式注入的。</p>

<pre>
<code class="language-XML">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd"&gt;

    &lt;context:property-placeholder location="classpath:rpc.properties"/&gt;
&lt;!--一个zk的服务发现对象serviceDiscovery，该对象是ZooKeeperServiceDiscovery的实例化，可以实现 服务发现功能
构造函数需要传入 zk的地址和端口 用字符串传入 也就是127.0.0.1:2181
这个值是在 rpc.properties配置好了 --&gt;
    &lt;bean id="serviceDiscovery" class="com.xxx.rpc.registry.zookeeper.ZooKeeperServiceDiscovery"&gt;
        &lt;constructor-arg name="zkAddress" value="${rpc.registry_address}"/&gt;
    &lt;/bean&gt;
&lt;!-- 生成rpcProxy，用来远程调用对应的服务，构造函数的输入参数是 一个zk的服务发现对象serviceDiscovery
调用serviceDiscovery的discover方法就能远程获取相应的服务--&gt;
    &lt;bean id="rpcProxy" class="com.xxx.rpc.client.RpcProxy"&gt;
        &lt;constructor-arg name="serviceDiscovery" ref="serviceDiscovery"/&gt;
    &lt;/bean&gt;

&lt;/beans&gt;</code></pre>

<p id=""></p>

<pre>
<code class="language-XML">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd"&gt;

    &lt;context:component-scan base-package="com.xxx.rpc.sample.server"/&gt;

    &lt;context:property-placeholder location="classpath:rpc.properties"/&gt;
&lt;!--导入 一个serviceRegistry进行服务注册 构造函数的输入参数是 zk的ip + port --&gt;
    &lt;bean id="serviceRegistry" class="com.xxx.rpc.registry.zookeeper.ZooKeeperServiceRegistry"&gt;
        &lt;constructor-arg name="zkAddress" value="${rpc.registry_address}"/&gt;
    &lt;/bean&gt;
&lt;!--根据serviceAddress 8000 serviceRegistry 2181 构建RpcServer
如果客户端有请求  只需要访问 ip+8000 就能获得服务--&gt;
    &lt;bean id="rpcServer" class="com.xxx.rpc.server.RpcServer"&gt;
        &lt;constructor-arg name="serviceAddress" value="${rpc.service_address}"/&gt;
        &lt;constructor-arg name="serviceRegistry" ref="serviceRegistry"/&gt;
    &lt;/bean&gt;

&lt;/beans&gt;</code></pre>

<p id=""></p>

<h2 id="spring%E7%9A%84%E6%B3%A8%E8%A7%A3%E5%A6%82%E4%BD%95%E5%8F%91%E6%8C%A5%E4%BD%9C%E7%94%A8%EF%BC%8C%E5%A6%82%E4%BD%95%E8%87%AA%E5%AE%9A%E4%B9%89%E6%B3%A8%E8%A7%A3%EF%BC%9F">spring的注解如何发挥作用，如何自定义注解？</h2>

<p><a class="has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" data-link-title="java注解详解和自定义注解_不积跬步，无以至千里-CSDN博客" href="https://blog.csdn.net/u010902721/article/details/52576624/" title="java注解详解和自定义注解_不积跬步，无以至千里-CSDN博客"><span class="link-card-box"><span class="link-title">java注解详解和自定义注解_不积跬步，无以至千里-CSDN博客</span><span class="link-link"><img alt="" class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" />https://blog.csdn.net/u010902721/article/details/52576624/</span></span></a>在这里定义了RpcService 注解。</p>

<pre>
<code class="language-java">@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Component
public @interface RpcService {
    /**
     * 服务接口类
     */
    Class&lt;?&gt; value();
    /**
     * 服务版本号
     */
    String version() default "";
}
</code></pre>

<p>在服务器启动之后就会扫描这样的注解进行注册。debug如下。</p>

<p><br /><img alt="" height="826" src="https://img-blog.csdnimg.cn/10fb3a93e7ac4dd18cde92e05d40dd7b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_20,color_FFFFFF,t_70,g_se,x_16" width="1200" /></p>

<p> </p>

<p><img alt="" height="610" src="https://img-blog.csdnimg.cn/bfa5fc4016e94bc4879a640cceda34b9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_20,color_FFFFFF,t_70,g_se,x_16" width="976" /></p>

<p></p>

<h2 id="Netty%20%E5%AD%A6%E4%B9%A0"><strong>Netty </strong>学习</h2>

<p>参考 B站黑马的视频</p>

<p></p>

<h2 id="Protostuff"><strong>Protostuff </strong></h2>

<p><a class="has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" data-link-title="初探Protostuff的使用_LuoLiangDSGA的博客-CSDN博客_protostuff" href="https://blog.csdn.net/oppo5630/article/details/80173520" title="初探Protostuff的使用_LuoLiangDSGA的博客-CSDN博客_protostuff"><span class="link-card-box"><span class="link-title">初探Protostuff的使用_LuoLiangDSGA的博客-CSDN博客_protostuff</span><span class="link-link"><img alt="" class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" />https://blog.csdn.net/oppo5630/article/details/80173520</span></span></a></p>

<h2 id="ZooKeeper"><strong>ZooKeeper</strong></h2>

<p><a class="has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" data-link-title="Zookeeper基本原理+通俗理解+安装使用_trigger的博客-CSDN博客" href="https://blog.csdn.net/weixin_40757930/article/details/122993927" title="Zookeeper基本原理+通俗理解+安装使用_trigger的博客-CSDN博客"><span class="link-card-box"><span class="link-title">Zookeeper基本原理+通俗理解+安装使用_trigger的博客-CSDN博客</span><span class="link-link"><img alt="" class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" />https://blog.csdn.net/weixin_40757930/article/details/122993927</span></span></a></p>

<p></p>

<h1 id="%E5%85%B6%E4%BB%96%E9%97%AE%E9%A2%98">其他问题</h1>

<h2 id="%E7%BC%96%E7%A0%81%E5%92%8C%E5%BA%8F%E5%88%97%E5%8C%96%E7%9A%84%E5%8C%BA%E5%88%AB%EF%BC%9F">编码和序列化的区别？</h2>

<p>序列化是编码过程的一个示例;它将复杂的数据结构转换为单个文本字符序列。 然后可以将该字符序列进一步编码成某种二进制表示，以实现更紧凑的存储或更快的传输。</p>

<p>参见</p>

<p><a class="has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" data-link-title="编码和序列化_小豆角的博客-CSDN博客_序列化和编码的区别" href="https://blog.csdn.net/u013755520/article/details/99938337" title="编码和序列化_小豆角的博客-CSDN博客_序列化和编码的区别"><span class="link-card-box"><span class="link-title">编码和序列化_小豆角的博客-CSDN博客_序列化和编码的区别</span><span class="link-link"><img alt="" class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" />https://blog.csdn.net/u013755520/article/details/99938337</span></span></a></p>

<p></p>

<h2 id="%E8%BF%99%E4%B8%AA%E9%A1%B9%E7%9B%AE%E5%92%8C%E4%B9%8B%E5%89%8D%E5%86%99%E7%9A%84RPC%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB%EF%BC%9F">这个项目和之前写的RPC有什么区别？</h2>

<p>之前写的RPC：</p>

<p><a class="has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" data-link-title="手写RPC框架（文末附代码）_trigger的博客-CSDN博客" href="https://blog.csdn.net/weixin_40757930/article/details/122908918" title="手写RPC框架（文末附代码）_trigger的博客-CSDN博客"><span class="link-card-box"><span class="link-title">手写RPC框架（文末附代码）_trigger的博客-CSDN博客</span><span class="link-link"><img alt="" class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" />https://blog.csdn.net/weixin_40757930/article/details/122908918</span></span></a></p>

<p>之前的项目只能算作一个RPC调用，不能称之为RPC框架，因为服务注册、服务发现是基于一个map实现的。</p>

<p></p>

<h2 id="%E6%94%B9%E8%BF%9B%E7%82%B9">改进点</h2>

<p><a class="has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" data-link-title="一个轻量级分布式RPC框架--NettyRpc - 阿凡卢 - 博客园" href="https://www.cnblogs.com/luxiaoxun/p/5272384.html" title="一个轻量级分布式RPC框架--NettyRpc - 阿凡卢 - 博客园"><span class="link-card-box"><span class="link-title">一个轻量级分布式RPC框架--NettyRpc - 阿凡卢 - 博客园</span><span class="link-link"><img alt="" class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" />https://www.cnblogs.com/luxiaoxun/p/5272384.html</span></span></a></p>

<p></p>

<p></p>

<p></p>

<p></p>
