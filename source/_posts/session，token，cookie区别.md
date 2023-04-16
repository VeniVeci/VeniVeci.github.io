---
title: session，token，cookie区别
tags: 服务器 前端 session
categories: 四大件之计算机网络
abbrlink: d290e541
date: 2022-03-01 15:26:21
---

<!--more-->

<p>计算机网络知识点整理见</p>

<p><a class="link-info has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="计算机网络知识点整理（一）" href="https://blog.csdn.net/weixin_40757930/article/details/123199271" title="计算机网络知识点整理（一）"><span class="link-card-box"><span class="link-title">计算机网络知识点整理（一）</span><span class="link-link"><img class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" alt="icon-default.png?t=M1L8" />https://blog.csdn.net/weixin_40757930/article/details/123199271</span></span></a><a class="link-info has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="计算机网络知识点整理（二）" href="https://blog.csdn.net/weixin_40757930/article/details/123205548" title="计算机网络知识点整理（二）"><span class="link-card-box"><span class="link-title">计算机网络知识点整理（二）</span><span class="link-link"><img class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" alt="icon-default.png?t=M1L8" />https://blog.csdn.net/weixin_40757930/article/details/123205548</span></span></a></p>

<p id="main-toc"><strong>目录</strong></p>

<p id="cookie-toc" style="margin-left:0px;"><a href="#cookie">cookie</a></p>

<p id="session-toc" style="margin-left:0px;"><a href="#session">session</a></p>

<p id="token-toc" style="margin-left:0px;"><a href="#token">token</a></p>

<p id="%E6%80%BB%E7%BB%93-toc" style="margin-left:0px;"><a href="#%E6%80%BB%E7%BB%93">总结</a></p>

<hr id="hr-toc" /><h1 id="cookie">cookie</h1>

<p><strong>cookie在http请求的头部 headers</strong></p>

<p>cookie 是一个非常具体的东西，指的就是浏览器里面能永久存储的一种数据，仅仅是浏览器实现的一种数据存储功能。</p>

<p>cookie由服务器生成，发送给浏览器，浏览器把cookie以kv形式保存到某个目录下的文本文件内，下一次请求同一网站时会把该cookie发送给服务器。由于cookie是存在客户端上的，所以浏览器加入了一些限制确保cookie不会被恶意使用，同时不会占据太多磁盘空间，所以每个域的cookie数量是有限的。</p>

<p>但问题是，我们也知道现在的很多网站功能很复杂，而且涉及很多的数据交互，比如说电商网站的购物车功能，信息量大，而且结构也比较复杂，无法通过简单的 cookie 机制传递这么多信息，而且要知道 cookie 字段是存储在 HTTP header 中的，就算能够承载这些信息，也会消耗很多的带宽，比较消耗网络资源。</p>

<h1 id="session">session</h1>

<p>session 就可以配合 cookie 解决这一问题，比如说一个 cookie 存储这样一个变量<code>sessionID=xxxx</code>，仅仅把这一个 cookie 传给服务器，然后服务器通过这个 ID 找到对应的 session，这个 session 是一个数据结构，里面存储着该用户的购物车等详细信息，服务器可以通过这些信息返回该用户的定制化网页，有效解决了追踪用户的问题。</p>

<p><strong>session 是一个数据结构，由网站的开发者设计，所以可以承载各种数据</strong>，只要客户端的 cookie 传来一个唯一的 session ID，服务器就可以找到对应的 session，认出这个客户。</p>

<p>服务器使用session把用户的信息临时保存在了服务器上，用户离开网站后session会被销毁。这种用户信息存储方式相对cookie来说更安全，可是session有一个缺陷：如果web服务器做了负载均衡，那么下一个操作请求到了另一台服务器的时候session会丢失。 <strong>需要session复制 才能使得不同的服务器都能响应这个请求。</strong></p>

<p>这样每个人只需要保存自己的session id，而服务器要保存所有人的session id ！如果访问服务器多了， 就得由成千上万，甚至几十万个。</p>

<p>这对服务器说是一个巨大的开销 ， 严重的限制了服务器扩展能力， 比如说我用两个机器组成了一个集群， 小F通过机器A登录了系统， 那session id会保存在机器A上， 假设小F的下一次请求被转发到机器B怎么办？机器B可没有小F的 session id啊</p>

<p>后来有个叫Memcached的支了招：把session id 集中存储到一个地方， 所有的机器都来访问这个地方的数据， 这样一来，就不用复制了， 这就叫分布式session，但是增加了单点失败的可能性。</p>

<p>可以尝试把这个单点的机器也搞出redis集群，增加可靠性， 但不管如何， <strong>这小小的session 对我来说是一个沉重的负担</strong></p>

<p><strong>还有没有更好的办法呢？</strong></p>

<h1 id="token">token</h1>

<p>比如说， 小F已经登录了系统， 我给他发一个令牌(token)， 里边包含了小F的 user id， 下一次小F 再次通过Http 请求访问我的时候， 把这个token 通过Http header 带过来不就可以了。</p>

<p>不过这和session id没有本质区别啊， 任何人都可以可以伪造， 所以我得想点儿办法， 让别人伪造不了。</p>

<p>那就对数据做一个签名吧， 比如说我用HMAC-SHA256 算法，加上一个只有我才知道的密钥， 对数据做一个签名， 把这个签名和数据一起作为token ， 由于密钥别人不知道， 就无法伪造token了。</p>

<p>Token 中的数据是明文保存的(虽然我会用Base64做下编码， 但那不是加密)， 还是可以被别人看到的， 所以我不能在其中保存像密码这样的敏感信息。</p>

<p>当然， 如果一个人的token 被别人偷走了， 那我也没办法， 我也会认为小偷就是合法用户， 这其实和一个人的session id 被别人偷走是一样的。</p>

<p>这样一来， 我就不保存session id 了， 我只是生成token , 然后验证token ， <strong>我用我的CPU计算时间获取了我的session 存储空间 ！</strong></p>

<h1 id="%E6%80%BB%E7%BB%93">总结</h1>

<p>  token就是令牌，比如你授权（登录）一个程序时，他就是个依据，判断你是否已经授权该软件；cookie就是写在<strong>客户端</strong>的一个txt文件，里面包括你登录信息之类的，这样你下次在登录某个网站，就会自动调用cookie自动登录用户名；session和cookie差不多，只是session是写在服务器端的文件，也需要在客户端写入cookie文件，但是文件里是你的浏览器编号.Session的状态是存储在<strong>服务器端</strong>，客户端只有<strong>session id</strong>；而Token的状态是存储在<strong>客户端</strong>。</p>

<p><a class="has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="session 、cookie、token的区别及联系 - 溪洋 - 博客园" href="https://www.cnblogs.com/wxinyu/p/9154178.html" title="session 、cookie、token的区别及联系 - 溪洋 - 博客园"><span class="link-card-box"><span class="link-title">session 、cookie、token的区别及联系 - 溪洋 - 博客园</span><span class="link-link"><img alt="" class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" />https://www.cnblogs.com/wxinyu/p/9154178.html</span></span></a></p>
