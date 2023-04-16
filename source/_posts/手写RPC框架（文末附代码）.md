---
title: 手写RPC框架（文末附代码）
tags: rpc 网络 网络协议
categories: Java基础知识 RPC
abbrlink: 6d82a42d
date: 2022-02-14 14:06:51
---

<!--more-->

<p> 代码免费下载地址  <a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" data-link-title="手写RPC框架代码（带注释）-Java文档类资源-CSDN下载" href="https://download.csdn.net/download/weixin_40757930/81142672" title="手写RPC框架代码（带注释）-Java文档类资源-CSDN下载">手写RPC框架代码（带注释）-Java文档类资源-CSDN下载</a></p>

<p>文末有使用 <strong>Spring + Netty + Protostuff + ZooKeeper 做的<span style="color:#fe2c24;">轻量级分布式框架</span>，</strong>是“手写RPC框架”的进阶版。</p>

<p id="main-toc"><strong>目录</strong></p>

<p id="RPC%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F-toc" style="margin-left:0px;"><a href="#RPC%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F">RPC是什么？</a></p>

<p id="%E9%82%A3%E4%B9%88%E4%B8%BA%E4%BB%80%E4%B9%88%E4%B8%8D%E8%83%BD%E6%8A%8A%E8%BF%99%E4%B8%AAfunc%E5%B0%B1%E6%94%BE%E5%9C%A8%E6%9C%AC%E5%9C%B0%E5%91%A2%EF%BC%9F-toc" style="margin-left:40px;"><a href="#%E9%82%A3%E4%B9%88%E4%B8%BA%E4%BB%80%E4%B9%88%E4%B8%8D%E8%83%BD%E6%8A%8A%E8%BF%99%E4%B8%AAfunc%E5%B0%B1%E6%94%BE%E5%9C%A8%E6%9C%AC%E5%9C%B0%E5%91%A2%EF%BC%9F">应用场景</a></p>

<p id="RPC%20%E4%BC%98%E7%82%B9-toc" style="margin-left:40px;"><a href="#RPC%20%E4%BC%98%E7%82%B9">RPC 优点</a></p>

<p id="%E5%86%99RPC%E6%A1%86%E6%9E%B6%E9%9C%80%E8%A6%81%E5%85%B7%E5%A4%87%E5%93%AA%E4%BA%9B%E7%9F%A5%E8%AF%86%EF%BC%9F-toc" style="margin-left:0px;"><a href="#%E5%86%99RPC%E6%A1%86%E6%9E%B6%E9%9C%80%E8%A6%81%E5%85%B7%E5%A4%87%E5%93%AA%E4%BA%9B%E7%9F%A5%E8%AF%86%EF%BC%9F">写RPC框架需要具备哪些知识？</a></p>

<p id="RPC%E5%8E%9F%E7%90%86%EF%BC%88%E6%91%98%E8%87%AA%EF%BC%9A%E4%BB%80%E4%B9%88%E6%83%85%E5%86%B5%E4%B8%8B%E4%BD%BF%E7%94%A8%20RPC%20%EF%BC%9F%20-%20%E7%9F%A5%E4%B9%8E%EF%BC%89-toc" style="margin-left:40px;"><a href="#RPC%E5%8E%9F%E7%90%86%EF%BC%88%E6%91%98%E8%87%AA%EF%BC%9A%E4%BB%80%E4%B9%88%E6%83%85%E5%86%B5%E4%B8%8B%E4%BD%BF%E7%94%A8%20RPC%20%EF%BC%9F%20-%20%E7%9F%A5%E4%B9%8E%EF%BC%89">RPC原理（摘自：什么情况下使用 RPC ？ - 知乎）</a></p>

<p id="Netty%E6%A1%86%E6%9E%B6-toc" style="margin-left:40px;"><a href="#Netty%E6%A1%86%E6%9E%B6">Netty框架</a></p>

<p id="%E5%85%B7%E4%BD%93%E4%BB%A3%E7%A0%81-toc" style="margin-left:0px;"><a href="#%E5%85%B7%E4%BD%93%E4%BB%A3%E7%A0%81">具体代码</a></p>

<p id="%E4%BB%A3%E7%A0%81%E7%9B%AE%E5%BD%95-toc" style="margin-left:40px;"><a href="#%E4%BB%A3%E7%A0%81%E7%9B%AE%E5%BD%95">代码目录</a></p>

<p id="api-toc" style="margin-left:40px;"><a href="#api">api</a></p>

<p id="provider-toc" style="margin-left:40px;"><a href="#provider">provider</a></p>

<p id="consumer-toc" style="margin-left:40px;"><a href="#consumer">consumer</a></p>

<p id="proxy-toc" style="margin-left:80px;"><a href="#proxy">proxy</a></p>

<p id="registry-toc" style="margin-left:40px;"><a href="#registry">registry</a></p>

<p id="protocol-toc" style="margin-left:40px;"><a href="#protocol">protocol</a></p>

<p id="%E6%9C%AC%E5%9C%B0%E8%B0%83%E7%94%A8%E5%92%8C%E8%BF%9C%E7%A8%8B%E8%B0%83%E7%94%A8%20%E4%BA%A7%E7%94%9F%E7%9A%84%E5%AF%B9%E8%B1%A1%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB%E5%91%A2%EF%BC%9F-toc" style="margin-left:40px;"><a href="#%E6%9C%AC%E5%9C%B0%E8%B0%83%E7%94%A8%E5%92%8C%E8%BF%9C%E7%A8%8B%E8%B0%83%E7%94%A8%20%E4%BA%A7%E7%94%9F%E7%9A%84%E5%AF%B9%E8%B1%A1%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB%E5%91%A2%EF%BC%9F">本地调用和远程调用 产生的对象有什么区别呢？</a></p>

<p id="RPC%E7%9A%84%E8%B0%83%E7%94%A8%E9%80%9F%E5%BA%A6%E5%A6%82%E4%BD%95%EF%BC%9F-toc" style="margin-left:40px;"><a href="#RPC%E7%9A%84%E8%B0%83%E7%94%A8%E9%80%9F%E5%BA%A6%E5%A6%82%E4%BD%95%EF%BC%9F">RPC的调用速度如何？</a></p>

<p id="%E8%BD%BB%E9%87%8F%E7%BA%A7%E5%88%86%E5%B8%83%E5%BC%8FRPC%E6%A1%86%E6%9E%B6-toc" style="margin-left:0px;"><a href="#%E8%BD%BB%E9%87%8F%E7%BA%A7%E5%88%86%E5%B8%83%E5%BC%8FRPC%E6%A1%86%E6%9E%B6">轻量级分布式RPC框架</a></p>

<hr id="hr-toc" /><p> 代码免费下载地址  <a class="has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" data-link-title="手写RPC框架代码（带注释）-Java文档类资源-CSDN下载" href="https://download.csdn.net/download/weixin_40757930/81142672" title="手写RPC框架代码（带注释）-Java文档类资源-CSDN下载"><span class="link-card-box"><span class="link-title">手写RPC框架代码（带注释）-Java文档类资源-CSDN下载</span><span class="link-link"><img alt="" class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" />https://download.csdn.net/download/weixin_40757930/81142672</span></span></a></p>

<h1 id="RPC%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F">RPC是什么？</h1>

<p>RPC是远程过程调用（Remote Procedure Call）的缩写形式。通俗的理解就是我调用了一个函数func(args)， 只不过这个func不在本地，需要远程访问这个函数，把我的args(输入参数)通过网络发送给这个函数所在的机子，然后由这个机子调用func(args)，再将返回值通过网络发送回来。</p>

<p></p>

<p><img alt="" height="428" src="https://img-blog.csdnimg.cn/5af340e433fc49068de8ad73eaae0af2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_11,color_FFFFFF,t_70,g_se,x_16" width="481" /></p>

<p></p>

<h2 id="%E9%82%A3%E4%B9%88%E4%B8%BA%E4%BB%80%E4%B9%88%E4%B8%8D%E8%83%BD%E6%8A%8A%E8%BF%99%E4%B8%AAfunc%E5%B0%B1%E6%94%BE%E5%9C%A8%E6%9C%AC%E5%9C%B0%E5%91%A2%EF%BC%9F">应用场景</h2>

<p> 摘自：<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M0H8" data-link-title="什么情况下使用 RPC ？ - 知乎" href="https://zhuanlan.zhihu.com/p/100199737" title="什么情况下使用 RPC ？ - 知乎">什么情况下使用 RPC ？ - 知乎</a></p>

<p>其实如果我们开发简单的应用，是用不着 RPC的。当我们的应用访问量增加和业务增加时，发现单机已无法承受，此时可以根据不同的业务（划分清楚业务逻辑）拆分成几个互不关联的应用，分别部署在不同的机器上，此时可能也不需要用到 RPC 。</p>

<p>随着我们的业务越来越多，应用也越来越多，应用与应用相互关联调用，发现有些功能已经不能简单划分开，此时可能就需要用到 RPC。</p>

<p>比如，我们开发电商系统，需要拆分出用户服务、商品服务、优惠券服务、支付服务、订单服务、物流服务、售后服务等等，这些服务之间都相互调用，这时内部调用最好使用 RPC ，同时每个服务都可以独立部署，独立上线。</p>

<p>也就说当我们的项目太大，需要解耦服务，扩展性强、部署灵活，这时就要用到 RPC ，主要解决了分布式系统中，服务与服务之间的调用问题。</p>

<h2 id="RPC%20%E4%BC%98%E7%82%B9"><strong>RPC 优点</strong></h2>

<ul><li>跨语言（C++、PHP、Java、Python ...）</li>
	<li>协议私密，安全性较高</li>
	<li>数据传输效率高</li>
	<li>支持动态扩展</li>
</ul><h1 id="%E5%86%99RPC%E6%A1%86%E6%9E%B6%E9%9C%80%E8%A6%81%E5%85%B7%E5%A4%87%E5%93%AA%E4%BA%9B%E7%9F%A5%E8%AF%86%EF%BC%9F">写RPC框架需要具备哪些知识？</h1>

<h2 id="RPC%E5%8E%9F%E7%90%86%EF%BC%88%E6%91%98%E8%87%AA%EF%BC%9A%E4%BB%80%E4%B9%88%E6%83%85%E5%86%B5%E4%B8%8B%E4%BD%BF%E7%94%A8%20RPC%20%EF%BC%9F%20-%20%E7%9F%A5%E4%B9%8E%EF%BC%89">RPC原理（摘自：<a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M0H8" data-link-title="什么情况下使用 RPC ？ - 知乎" href="https://zhuanlan.zhihu.com/p/100199737" title="什么情况下使用 RPC ？ - 知乎">什么情况下使用 RPC ？ - 知乎</a>）</h2>

<p><img alt="" height="453" src="https://img-blog.csdnimg.cn/b57e3a922c324201973b12f222d85144.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_20,color_FFFFFF,t_70,g_se,x_16" width="905" /></p>

<p></p>

<p>RPC 架构主要包括三部分：</p>

<ul><li>服务注册中心（Registry），负责将本地服务发布成远程服务，管理远程服务，提供给服务消费者使用。</li>
	<li>服务提供者（Server），提供服务接口定义与服务实现类。</li>
	<li>服务消费者（Client），通过远程代理对象调用远程服务。</li>
</ul><p><strong>服务提供者</strong>启动后主动向服务注册中心（Registry）注册机器IP、端口以及提供的服务列表；</p>

<p><strong>服务消费者</strong>启动时向服务注册中心（Registry）获取服务提供方地址列表。</p>

<p>服务注册中心（Registry）可实现负载均衡和故障切换。</p>

<h2 id="Netty%E6%A1%86%E6%9E%B6">Netty框架</h2>

<p>原理图中的注册中心+生产者可以理解为一个 服务器，而消费者就是一个客户端。</p>

<p>在代码中需要先启动注册中心并注册服务，然后等待客户端请求对应的服务，所以需要用到通信框架，这里采用Netty。</p>

<p></p>

<h1 id="%E5%85%B7%E4%BD%93%E4%BB%A3%E7%A0%81">具体代码</h1>

<p>代码主要是基于RPC调用了两个类的函数，一个是<strong>RpcHelloServiceImpl</strong>的hello函数，另一个是<strong>RpcServiceImpl</strong>的加减乘除函数。</p>

<pre>
<code class="language-java">public class RpcConsumer {
    public static void main(String [] args){
        // 如果想要使用RpcHelloServiceImpl中的hello方法
        //         和 RpcServiceImpl中的4个算术方法
        // 可以直接新建对应的对象并使用  也可以是远程调用

        // 本地调用示例
        System.out.println("本地调用示例");
        IRpcHelloService LpcHello = new RpcHelloServiceImpl();
        System.out.println(LpcHello.hello("Tom老师"));

        IRpcService LpcService = new RpcServiceImpl();
        System.out.println("8 + 2 = " + LpcService.add(8, 2));
        System.out.println("8 - 2 = " + LpcService.sub(8, 2));
        System.out.println("8 * 2 = " + LpcService.mult(8, 2));
        System.out.println("8 / 2 = " + LpcService.div(8, 2));

        System.out.println("=======================");
        // 远程调用示例
        System.out.println("远程调用示例");
        IRpcHelloService RpcHello = RpcProxy.create(IRpcHelloService.class);
        System.out.println(RpcHello.hello("Tom老师"));

        IRpcService RpcService = RpcProxy.create(IRpcService.class);
        System.out.println("8 + 2 = " + RpcService.add(8, 2));
        System.out.println("8 - 2 = " + RpcService.sub(8, 2));
        System.out.println("8 * 2 = " + RpcService.mult(8, 2));
        System.out.println("8 / 2 = " + RpcService.div(8, 2));
    }

}
</code></pre>

<h2 id="%E4%BB%A3%E7%A0%81%E7%9B%AE%E5%BD%95">代码目录</h2>

<p>参考书籍：《Netty4核心原理与手写rpc实战》</p>

<p><img alt="" height="427" src="https://img-blog.csdnimg.cn/f2d413d889d84d3a81a84df1cecf6c00.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_11,color_FFFFFF,t_70,g_se,x_16" width="415" /></p>

<h2 id="api">api</h2>

<pre>
<code class="language-java">public interface IRpcHelloService {
    String hello(String name);  
}  </code></pre>

<pre>
<code class="language-java">public interface IRpcService {
	/** 加 */
	public int add(int a,int b);
	/** 减 */
	public int sub(int a,int b);
	/** 乘 */
	public int mult(int a,int b);
	/** 除 */
	public int div(int a,int b);
}
</code></pre>

<h2 id="provider">provider</h2>

<pre>
<code class="language-java">// 类名 RpcHelloServiceImpl 函数名 hello
// 形参列表 name 实参列表 张三
public class RpcHelloServiceImpl implements IRpcHelloService {

    public String hello(String name) {
        return "Hello " + name + "!";
    }

}
</code></pre>

<pre>
<code class="language-java">public class RpcServiceImpl implements IRpcService {

	public int add(int a, int b) {
		return a + b;
	}

	public int sub(int a, int b) {
		return a - b;
	}

	public int mult(int a, int b) {
		return a * b;
	}

	public int div(int a, int b) {
		return a / b;
	}

}</code></pre>

<h2 id="consumer">consumer</h2>

<pre>
<code class="language-java">public class RpcConsumer {
    public static void main(String [] args){
        // 如果想要使用RpcHelloServiceImpl中的hello方法
        //         和 RpcServiceImpl中的4个算术方法
        // 可以直接新建对应的对象并使用  也可以是远程调用

        // 本地调用示例
        System.out.println("本地调用示例");
        IRpcHelloService LpcHello = new RpcHelloServiceImpl();
        System.out.println(LpcHello.hello("Tom老师"));

        IRpcService LpcService = new RpcServiceImpl();
        System.out.println("8 + 2 = " + LpcService.add(8, 2));
        System.out.println("8 - 2 = " + LpcService.sub(8, 2));
        System.out.println("8 * 2 = " + LpcService.mult(8, 2));
        System.out.println("8 / 2 = " + LpcService.div(8, 2));

        System.out.println("=======================");
        // 远程调用示例
        System.out.println("远程调用示例");
        IRpcHelloService RpcHello = RpcProxy.create(IRpcHelloService.class);
        System.out.println(RpcHello.hello("Tom老师"));

        IRpcService RpcService = RpcProxy.create(IRpcService.class);
        System.out.println("8 + 2 = " + RpcService.add(8, 2));
        System.out.println("8 - 2 = " + RpcService.sub(8, 2));
        System.out.println("8 * 2 = " + RpcService.mult(8, 2));
        System.out.println("8 / 2 = " + RpcService.div(8, 2));
    }

}
</code></pre>

<h3 id="proxy">proxy</h3>

<pre>
<code class="language-java">public class RpcProxyHandler extends ChannelInboundHandlerAdapter {
    // 入站处理器
    private Object response;
    public Object getResponse() {
	    return response;
	}

    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        response = msg;
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        System.out.println("client exception is general");
    }
}</code></pre>

<pre>
<code class="language-java">public class RpcProxy {
	// PRC代理类
	public static &lt;T&gt; T create(Class&lt;?&gt; clazz){
		// 返回这个实例 方便客户端使用
        MethodProxy proxy = new MethodProxy(clazz);
        Class&lt;?&gt; [] interfaces = clazz.isInterface() ? new Class[]{clazz} : clazz.getInterfaces();// 如果clazz.isInterface() 就找到所有的实现类 否则就直接使用该接口
        T result = (T) Proxy.newProxyInstance(clazz.getClassLoader(),interfaces,proxy);
        return result;
    }
	// new MethodProxy(IRpcHelloService.class);
	private static class MethodProxy implements InvocationHandler {
		private Class&lt;?&gt; clazz;  //clazz = IRpcHelloService.class
		public MethodProxy(Class&lt;?&gt; clazz){
			this.clazz = clazz;//IRpcHelloService.class
		}
		// method:hello
		// args: Tom老师
		public Object invoke(Object proxy, Method method, Object[] args)  throws Throwable {
			//如果传进来是一个已实现的具体类（本次演示略过此逻辑)
			if (Object.class.equals(method.getDeclaringClass())) {
				try {
					System.out.println("LPC");
					return method.invoke(this, args);
				} catch (Throwable t) {
					t.printStackTrace();
				}
				//如果传进来的是一个接口（核心)
			} else {
				System.out.println("RPC");
				return rpcInvoke(proxy, method, args);
			}
			return null;
		}
		/**
		 * 实现接口的核心方法
		 *  把调用的方法封装为消息  与服务器建立连接  发送消息得  在服务器上调用该方法  返回对应的值
		 * @param method
		 * @param args
		 * @return
		 */
		public Object rpcInvoke(Object proxy,Method method,Object[] args){
			//传输协议封装 调用的方法 参数 成一个InvokerProtocol 对象 方便后面编解码
			InvokerProtocol msg = new InvokerProtocol();
			msg.setClassName(this.clazz.getName()); //IRpcHelloService
			msg.setMethodName(method.getName());    //hello
			msg.setParames(method.getParameterTypes()); // string
			msg.setValues(args);  // zhangsan
			final RpcProxyHandler consumerHandler = new RpcProxyHandler();

			EventLoopGroup group = new NioEventLoopGroup();
			try {
				Bootstrap b = new Bootstrap();
				b.group(group)
						.channel(NioSocketChannel.class)
						.option(ChannelOption.TCP_NODELAY, true)
						.handler(new ChannelInitializer&lt;SocketChannel&gt;() {
							@Override
							public void initChannel(SocketChannel ch) throws Exception {
								ChannelPipeline pipeline = ch.pipeline();
								//对象参数类型编码器
								pipeline.addLast("encoder", new ObjectEncoder());
								//对象参数类型解码器
								pipeline.addLast("decoder", new ObjectDecoder(Integer.MAX_VALUE, ClassResolvers.cacheDisabled(null)));
								pipeline.addLast("handler",consumerHandler);
							}
						});

				ChannelFuture future = b.connect("localhost", 8080).sync();
				future.channel().writeAndFlush(msg).sync();// 建立连接之后进行编解码 然后传输 等待结果
				future.channel().closeFuture().sync();// 在这里等待closeFuture
			} catch(Exception e){
				e.printStackTrace();
			}finally {
				group.shutdownGracefully();// 最后关闭这个请求连接
			}
			return consumerHandler.getResponse();// 在建立连接的时候已经将响应存储在了consumerHandler中
		}

	}
}
</code></pre>

<h2 id="registry">registry</h2>

<pre>
<code class="language-java">public class RegistryHandler  extends ChannelInboundHandlerAdapter {
	//用来保存所有可用的服务
    public static ConcurrentHashMap&lt;String, Object&gt; registryMap = new ConcurrentHashMap&lt;String,Object&gt;();
    //保存所有相关的服务类
    private List&lt;String&gt; classNames = new ArrayList&lt;&gt;();

    public RegistryHandler(){
    	//递归扫描provider文件夹，其中所有服务的全类名都被保存在了classNames这个链表中
		// 比如com.gupaoedu.vip.netty.rpc.provider.RpcHelloServiceImpl
    	scannerClass("com.gupaoedu.vip.netty.rpc.provider");
    	//将classNames这个链表中的服务 注册到registryMap中
    	doRegister();
    }

    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
    	Object result = new Object();
        InvokerProtocol request = (InvokerProtocol)msg;
        //当客户端建立连接时，需要从自定义协议中获取信息，拿到具体的服务和实参
		//使用反射调用
        if(registryMap.containsKey(request.getClassName())){
        	Object clazz = registryMap.get(request.getClassName());
        	Method method = clazz.getClass().getMethod(request.getMethodName(), request.getParames());
        	result = method.invoke(clazz, request.getValues());
        	// clazz.method( request.getParames()(type)  request.getValues())
        }
        ctx.write(result);
        ctx.flush();
        ctx.close();// 关闭该通道
    }
    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
         cause.printStackTrace();
         ctx.close();
    }
    /*
     * 递归扫描
     */
	private void scannerClass(String packageName){
		URL url = this.getClass().getClassLoader().getResource(packageName.replaceAll("\\.", "/"));
		File dir = new File(url.getFile());
		for (File file : dir.listFiles()) {
			//如果是一个文件夹，继续递归
			if(file.isDirectory()){
				scannerClass(packageName + "." + file.getName());
			}else{
				classNames.add(packageName + "." + file.getName().replace(".class", "").trim());
			}
		}
	}
	/**
	 * 完成注册
	 */
	private void doRegister(){
		if(classNames.size() == 0){ return; }
		for (String className : classNames) {
			try {
				Class&lt;?&gt; clazz = Class.forName(className); // 实现类
				Class&lt;?&gt; i = clazz.getInterfaces()[0];    // 上层接口
				registryMap.put(i.getName(), clazz.newInstance());// 类名 服务名 和 对应的服务实例
			} catch (Exception e) {
				e.printStackTrace();
			}
		}
	}

}
</code></pre>

<pre>
<code class="language-java">public class RpcRegistry {
    /*
    新建一个注册中心并启动
     */
    private int port; // 服务发布的端口  例如8080
    public RpcRegistry(int port){
        this.port = port;
    }
    /*
          利用netty新建一个服务器（注册中心） 启动该服务器并监听port端口
     */
    public void start(){
        EventLoopGroup bossGroup = new NioEventLoopGroup();
        // 非阻塞的事件循环组 里面有多个事件循环  bossGroup负责
        EventLoopGroup workerGroup = new NioEventLoopGroup();
        ChannelInitializer&lt;SocketChannel&gt; channelInitializer = new ChannelInitializer&lt;SocketChannel&gt;() {
            @Override
            protected void initChannel(SocketChannel ch) throws Exception {// 信道初始化 主要是在该信道中添加一些处理器
                ChannelPipeline pipeline = ch.pipeline();
                //对象参数类型编码器
                pipeline.addLast("encoder", new ObjectEncoder()); // netty中的编码器 把对象转化为Byte 然后进行网络传输
                //对象参数类型解码器  ObjectDecoder extends LengthFieldBasedFrameDecoder
                pipeline.addLast("decoder", new ObjectDecoder(Integer.MAX_VALUE, ClassResolvers.cacheDisabled(null)));// netty中的解码器 把Byte转化为对象
                // 在信道初始化时就 进行服务的注册
                RegistryHandler registryHandler = new RegistryHandler();
                pipeline.addLast(registryHandler);
            }
        };
        try {
            ServerBootstrap b = new ServerBootstrap();
            b.group(bossGroup, workerGroup)
            		.channel(NioServerSocketChannel.class)
                    .childHandler(channelInitializer)
                    .option(ChannelOption.SO_BACKLOG, 128)
                    .childOption(ChannelOption.SO_KEEPALIVE, true);
            ChannelFuture future = b.bind(port).sync(); // 绑定端口
            System.out.println("RPC Registry has started and is listening at " + port );
            future.channel().closeFuture().sync();
        } catch (Exception e) {
             bossGroup.shutdownGracefully();
             workerGroup.shutdownGracefully();
        }
    }

    public static void main(String[] args) throws Exception {
        new RpcRegistry(8080).start();// 新建一个端口号为8080的注册中心 并启动
    }
}
</code></pre>

<p><img alt="" height="736" src="https://img-blog.csdnimg.cn/a2470f0a8f0d4db7a4bf328be597a50a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_20,color_FFFFFF,t_70,g_se,x_16" width="1200" /></p>

<h2 id="protocol">protocol</h2>

<pre>
<code class="language-java">/**
 * 自定义传输协议
 */
@Data
public class InvokerProtocol implements Serializable {

    private String className;//类名
    private String methodName;//函数名称
    private Class&lt;?&gt;[] parames;//形参类型？列表
    private Object[] values;//实参列表
// 类名 RpcHelloServiceImpl 函数名 hello
// 形参列表 name 实参列表 张三
}
</code></pre>

<p></p>

<h2 id="%E6%9C%AC%E5%9C%B0%E8%B0%83%E7%94%A8%E5%92%8C%E8%BF%9C%E7%A8%8B%E8%B0%83%E7%94%A8%20%E4%BA%A7%E7%94%9F%E7%9A%84%E5%AF%B9%E8%B1%A1%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB%E5%91%A2%EF%BC%9F">本地调用和远程调用 产生的对象有什么区别呢？</h2>

<p>远程调用产生的是代理对象，本地调用产生</p>

<p><img alt="" height="593" src="https://img-blog.csdnimg.cn/b92902f5791640d9be1ab9657f7ff91a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_20,color_FFFFFF,t_70,g_se,x_16" width="1170" /></p>

<p></p>

<h2 id="RPC%E7%9A%84%E8%B0%83%E7%94%A8%E9%80%9F%E5%BA%A6%E5%A6%82%E4%BD%95%EF%BC%9F">RPC的调用速度如何？</h2>

<p>由于RPC涉及到数据的序列化和网络传输，性能不如本地调用LPC，不过瑕不掩瑜。</p>

<p>速度对比：</p>

<pre>
<code class="language-java">public class TestLpcRpc {
    public static void main(String[] args) {
        int num = 100;
        Lpc(num);
        Rpc(num);
    }
    public static void Lpc(int num){
        long startTime = System.currentTimeMillis(); //获取开始时间
        for (int i = 0; i &lt; num; i++) {
            IRpcHelloService LpcHello = new RpcHelloServiceImpl();
            LpcHello.hello("Tom老师");
        }
        long endTime = System.currentTimeMillis(); //获取结束时间
        System.out.println("Lpc程序运行时间：" + (endTime - startTime) + "ms");
    }
    public static void Rpc(int num){
        long startTime = System.currentTimeMillis(); //获取开始时间
        for (int i = 0; i &lt; num; i++) {
            IRpcHelloService RpcHello = RpcProxy.create(IRpcHelloService.class);
            RpcHello.hello("Tom老师");
        }
        long endTime = System.currentTimeMillis(); //获取结束时间
        System.out.println("Rpc程序运行时间：" + (endTime - startTime) + "ms");
    }
}
</code></pre>

<p><img alt="" height="257" src="https://img-blog.csdnimg.cn/29b1423257e8456890ab564793d35b9e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_20,color_FFFFFF,t_70,g_se,x_16" width="712" /></p>

<p>代码的github地址为 <a class="has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release1.9.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M0H8" data-link-title="GitHub - VeniVeci/RpcLearn" href="https://github.com/VeniVeci/RpcLearn" title="GitHub - VeniVeci/RpcLearn"><span class="link-card-box"><span class="link-title">GitHub - VeniVeci/RpcLearn</span><span class="link-link"><img alt="" class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release1.9.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M0H8" />https://github.com/VeniVeci/RpcLearn</span></span></a></p>

<p>代码免费下载地址  <a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.6/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1H3" data-link-title="手写RPC框架代码（带注释）-Java文档类资源-CSDN下载" href="https://download.csdn.net/download/weixin_40757930/81142672" title="手写RPC框架代码（带注释）-Java文档类资源-CSDN下载">手写RPC框架代码（带注释）-Java文档类资源-CSDN下载</a></p>

<p>严格来说，这只能叫做RPC调用，RPC框架除了RPC调用之外还应该有更丰富的功能，比如 像dubbo的高性能NIO通讯及多协议集成，服务动态寻址与路由，软负载均衡与容错，依赖分析与降级等等。</p>

<p>在学习RPC框架的时候发现了一个很不错的项目，以下是我在学习这个项目的过程中遇到的疑问和问题的答案。</p>

<h1 id="%E8%BD%BB%E9%87%8F%E7%BA%A7%E5%88%86%E5%B8%83%E5%BC%8FRPC%E6%A1%86%E6%9E%B6">轻量级分布式RPC框架</h1>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="《轻量级分布式 RPC 框架》笔记 (源码+注释)_trigger的博客-CSDN博客" href="https://blog.csdn.net/weixin_40757930/article/details/123019144?spm=1001.2014.3001.5502" title="《轻量级分布式 RPC 框架》笔记 (源码+注释)_trigger的博客-CSDN博客">《轻量级分布式 RPC 框架》笔记 (源码+注释)_trigger的博客-CSDN博客</a></p>

<p><strong>Spring + Netty + Protostuff + ZooKeeper</strong></p>

<p>使用Netty构建RPC服务器和客户端；</p>

<p>使用Protostuff将请求和响应消息序列化；</p>

<p>使用ZooKeeper做服务注册和服务发现。</p>

<p> </p>
