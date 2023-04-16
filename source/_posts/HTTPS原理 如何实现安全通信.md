---
title: HTTPS原理 如何实现安全通信
tags: 网络安全 网络 网络协议
categories: 四大件之计算机网络
abbrlink: 77ab3ab4
date: 2022-03-26 22:38:23
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="HTTP%E5%AD%98%E5%9C%A8%E7%9A%84%E9%97%AE%E9%A2%98-toc" style="margin-left:0px;"><a href="#HTTP%E5%AD%98%E5%9C%A8%E7%9A%84%E9%97%AE%E9%A2%98">HTTP存在的问题</a></p>

<p id="HTTPS%E5%8E%9F%E7%90%86-toc" style="margin-left:0px;"><a href="#HTTPS%E5%8E%9F%E7%90%86">HTTPS原理</a></p>

<p id="%E6%95%B0%E5%AD%97%E8%AF%81%E4%B9%A6-toc" style="margin-left:0px;"><a href="#%E6%95%B0%E5%AD%97%E8%AF%81%E4%B9%A6">数字证书</a></p>

<p id="CA%E5%8F%AF%E4%B8%8D%E5%8F%AF%E4%BB%A5%E7%94%A8%E5%85%AC%E9%92%A5%E5%8A%A0%E5%AF%86%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%9A%84%E5%85%AC%E9%92%A5%EF%BC%9F-toc" style="margin-left:0px;"><a href="#CA%E5%8F%AF%E4%B8%8D%E5%8F%AF%E4%BB%A5%E7%94%A8%E5%85%AC%E9%92%A5%E5%8A%A0%E5%AF%86%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%9A%84%E5%85%AC%E9%92%A5%EF%BC%9F">CA可不可以用公钥加密服务器的公钥？</a></p>

<hr id="hr-toc" /><p></p>

<p>参考：</p>

<p><a class="has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="HTTPS理论基础及其在Android中的最佳实践_孙群的博客-CSDN博客_android https" href="https://blog.csdn.net/iispring/article/details/51615631" title="HTTPS理论基础及其在Android中的最佳实践_孙群的博客-CSDN博客_android https"><span class="link-card-box"><span class="link-title">HTTPS理论基础及其在Android中的最佳实践_孙群的博客-CSDN博客_android https</span><span class="link-link"><img alt="" class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" />https://blog.csdn.net/iispring/article/details/51615631</span></span></a></p>

<p><a data-link-desc="HTTPS是什么？加密原理和证书。SSL/TLS握手过程" data-link-icon="http://i2.hdslb.com/bfs/archive/62612ecd372a0e5e1812e9da2617c72ba15fc15c.jpg@57w_57h_1c.png" data-link-title="HTTPS是什么？加密原理和证书。SSL/TLS握手过程_哔哩哔哩_bilibili" href="https://www.bilibili.com/video/BV1KY411x7Jp?spm_id_from=333.337.search-card.all.click" title="HTTPS是什么？加密原理和证书。SSL/TLS握手过程_哔哩哔哩_bilibili">HTTPS是什么？加密原理和证书。SSL/TLS握手过程_哔哩哔哩_bilibili</a></p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.8/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M276" data-link-title="HTTPS有什么用？比起HTTP来说有什么区别？如何实现？ - 知乎" href="https://www.zhihu.com/question/356721077" title="HTTPS有什么用？比起HTTP来说有什么区别？如何实现？ - 知乎">HTTPS有什么用？比起HTTP来说有什么区别？如何实现？ - 知乎</a></p>

<h1 id="HTTP%E5%AD%98%E5%9C%A8%E7%9A%84%E9%97%AE%E9%A2%98">HTTP存在的问题</h1>

<p>HTTP 由于是明文传输，所以安全上存在以下三个风险：</p>

<ul><li><strong>窃听风险</strong>，比如通信链路上可以获取通信内容，用户号容易没。</li>
	<li><strong>篡改风险</strong>，比如强制入垃圾广告，视觉污染，用户眼容易瞎。</li>
	<li><strong>冒充风险</strong>，比如冒充淘宝网站，用户钱容易没。</li>
</ul><p>HTTPS做的改进：</p>

<ul><li><strong>信息加密</strong>：交互信息无法被窃取。</li>
	<li><strong>校验机制</strong>：无法篡改通信内容，篡改了就不能正常显示。</li>
	<li><strong>身份证书</strong>：证明淘宝是真的。</li>
</ul><h1 id="HTTPS%E5%8E%9F%E7%90%86">HTTPS原理</h1>

<p>我们知道，HTTP请求都是明文传输的，所谓的明文指的是没有经过加密的信息，如果HTTP请求被黑客拦截，并且里面含有银行卡密码等敏感数据的话，会非常危险。为了解决这个问题，Netscape 公司制定了HTTPS协议，HTTPS可以将数据加密传输，也就是传输的是密文，即便黑客在传输过程中拦截到数据也无法破译，这就保证了网络通信的安全。</p>

<p><strong>HTTPS协议 = HTTP协议 + SSL/TLS协议</strong>，在HTTPS数据传输的过程中，需要用SSL/TLS对数据进行加密和解密，需要用HTTP对加密后的数据进行传输，由此可以看出HTTPS是由HTTP和SSL/TLS一起合作完成的，现在大都支持TLS。</p>

<p>SSL的全称是Secure Sockets Layer，即安全套接层协议，是为网络通信提供安全及数据完整性的一种安全协议。SSL协议在1994年被Netscape发明，后来各个浏览器均支持SSL，其最新的版本是3.0</p>

<p>TLS的全称是Transport Layer Security，即安全传输层协议，最新版本的TLS（Transport Layer Security，传输层安全协议）是IETF（Internet Engineering Task Force，Internet工程任务组）制定的一种新的协议，它建立在SSL 3.0协议规范之上，是SSL 3.0的后续版本。在TLS与SSL3.0之间存在着显著的差别，主要是它们所支持的加密算法不同，所以TLS与SSL3.0不能互操作。虽然TLS与SSL3.0在加密算法上不同，但是在我们理解HTTPS的过程中，<strong>我们可以把SSL和TLS看做是同一个协议。</strong></p>

<p>HTTPS为了兼顾安全与效率，同时使用了对称加密和非对称加密。数据是被对称加密传输的，对称加密过程需要客户端的一个密钥，为了确保能把该密钥安全传输到服务器端，采用非对称加密对该密钥进行加密传输。</p>

<p><span style="color:#fe2c24;"><strong>总的来说，对数据进行对称加密，对称加密所要使用的密钥通过非对称加密传输。</strong></span></p>

<p>一个HTTPS请求实际上包含了<strong>两次HTTP传输</strong>，可以细分为8步。</p>

<p><img alt="" height="808" src="https://img-blog.csdnimg.cn/1709bc9c67ee4bd08ed21f6401057507.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1200" /><br />
1.客户端向服务器发起HTTPS请求，连接到服务器的443端口</p>

<p>2.服务器端有一个密钥对，即公钥和私钥，是用来进行非对称加密使用的，服务器端保存着私钥，不能将其泄露，公钥可以发送给任何人。</p>

<p>3.服务器将自己的<strong>公钥发送给客户端</strong>。</p>

<p>4.客户端收到服务器端的证书之后，<strong>会对证书进行检查，验证其合法性</strong>，如果发现发现证书有问题，那么HTTPS传输就无法继续。严格的说，这里应该是验证服务器发送的数字证书的合法性，关于客户端如何验证数字证书的合法性，下文会进行说明。如果公钥合格，那么客户端会生成一个随机值，这个<span style="color:#fe2c24;"><strong>随机值就是用于进行对称加密的密钥</strong></span>，我们将该密钥称之为client key，即客户端密钥，这样在概念上和服务器端的密钥容易进行区分。然后用服务器的公钥对客户端密钥进行非对称加密，这样客户端密钥就变成密文了，至此，HTTPS中的第一次HTTP请求结束。</p>

<p>5.客户端会发起HTTPS中的第二个HTTP请求，将加密之后的客户端密钥发送给服务器。</p>

<p>6.服务器接收到客户端发来的密文之后，会用自己的私钥对其进行非对称解密，解密之后的明文就是客户端密钥，然后用客户端密钥对数据进行对称加密，这样数据就变成了密文。</p>

<p>7.然后服务器将加密后的密文发送给客户端。</p>

<p>8.客户端收到服务器发送来的密文，用客户端密钥对其进行对称解密，得到服务器发送的数据。这样HTTPS中的第二个HTTP请求结束，整个HTTPS传输完成。</p>

<h1 id="%E6%95%B0%E5%AD%97%E8%AF%81%E4%B9%A6"><br /><br />
数字证书</h1>

<p>通过观察HTTPS的传输过程，我们知道，当服务器接收到客户端发来的请求时，会向客户端发送服务器自己的公钥，但是黑客有可能中途篡改公钥，将其改成黑客自己的，所以有个问题，客户端怎么信赖这个公钥是自己想要访问的服务器的公钥而不是黑客的呢？ <strong>这时候就需要用到数字证书。</strong></p>

<p>要想让客户端信赖公钥，公钥也要找一个担保人，这个担保人的就是证书认证中心（Certificate Authority），简称CA。 也就是说CA是专门对公钥进行认证，进行担保的，也就是专门给公钥做担保的担保公司。 全球知名的CA也就100多个，这些CA都是全球都认可的，比如VeriSign、GlobalSign等，国内知名的CA有WoSign。</p>

<p>那CA怎么对公钥做担保认证呢？<strong>CA本身也有一对公钥和私钥</strong>，CA会用CA自己的私钥对要进行认证的公钥进行非对称加密，此处待认证的公钥就相当于是明文，加密完之后，得到的密文再加上证书的过期时间、颁发给、颁发者等信息，就组成了数字证书。</p>

<p>不论什么平台，设备的操作系统中都会内置100多个全球公认的CA，说具体点就是设备中存储了这些知名CA的公钥。<strong>当客户端接收到服务器的数字证书的时候，会进行如下验证：</strong></p>

<p>    首先客户端会用设备中内置的CA的公钥尝试解密数字证书，如果所有内置的CA的公钥都无法解密该数字证书，说明该数字证书不是由一个全球知名的CA签发的，这样客户端就无法信任该服务器的数字证书。</p>

<p><strong>    如果有一个CA的公钥能够成功解密该数字证书，说明该数字证书就是由该CA的私钥签发的，因为被私钥加密的密文只能被与其成对的公钥解密。</strong></p>

<p>    除此之外，还需要检查客户端当前访问的服务器的域名是与数字证书中提供的“颁发给”这一项吻合，还要检查数字证书是否过期等。<br />
 </p>

<p></p>

<h1 id="CA%E5%8F%AF%E4%B8%8D%E5%8F%AF%E4%BB%A5%E7%94%A8%E5%85%AC%E9%92%A5%E5%8A%A0%E5%AF%86%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%9A%84%E5%85%AC%E9%92%A5%EF%BC%9F">CA可不可以用公钥加密服务器的公钥？</h1>

<p>（我的理解）</p>

<p>不行，如果CA用CA公钥加密 待认证的服务器公钥，那么其他人也有一样的CA公钥，任何人都可以对服务器公钥进行CA认证。</p>

<p>这样一些危险的网站也就有了自己的CA证书，而且这个证书客户端收到后，在用设备中内置的私钥解锁时都能正确解锁，这样客户端就误以为这是一个经过CA认证的机构。</p>
