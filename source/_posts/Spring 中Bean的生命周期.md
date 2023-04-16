---
title: Spring 中Bean的生命周期
tags: spring java 后端
categories: Java基础知识 Spring框架
abbrlink: 12ee38c0
date: 2022-03-28 20:47:59
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="Bean%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F-toc" style="margin-left:0px;"><a href="#Bean%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F">Bean的生命周期</a></p>

<p id="%E4%BA%94%E4%B8%AA%E9%98%B6%E6%AE%B5-toc" style="margin-left:40px;"><a href="#%E4%BA%94%E4%B8%AA%E9%98%B6%E6%AE%B5">五个阶段</a></p>

<p id="%E4%B8%8B%E9%9D%A2%E6%98%AF%E4%B8%80%E4%B8%AAbean%E5%AF%B9%E8%B1%A1%E5%88%9B%E5%BB%BA%E5%88%B0%E9%94%80%E6%AF%81%E7%BB%8F%E5%8E%86%E8%BF%87%E7%9A%84%E6%96%B9%E6%B3%95%E3%80%82-toc" style="margin-left:40px;"><a href="#%E4%B8%8B%E9%9D%A2%E6%98%AF%E4%B8%80%E4%B8%AAbean%E5%AF%B9%E8%B1%A1%E5%88%9B%E5%BB%BA%E5%88%B0%E9%94%80%E6%AF%81%E7%BB%8F%E5%8E%86%E8%BF%87%E7%9A%84%E6%96%B9%E6%B3%95%E3%80%82">下面是一个bean对象创建到销毁经历过的方法。</a></p>

<p id="%E5%9B%BE%E7%A4%BA%E2%80%8B-toc" style="margin-left:40px;"><a href="#%E5%9B%BE%E7%A4%BA%E2%80%8B">图示​</a></p>

<p id="%E9%97%AE%E7%AD%94-toc" style="margin-left:0px;"><a href="#%E9%97%AE%E7%AD%94">问答</a></p>

<p id="%E6%99%AE%E9%80%9AJava%E7%B1%BB%E6%98%AF%E5%9C%A8%E5%93%AA%E4%B8%80%E6%AD%A5%E5%8F%98%E6%88%90beanDefinition%E7%9A%84-toc" style="margin-left:40px;"><a href="#%E6%99%AE%E9%80%9AJava%E7%B1%BB%E6%98%AF%E5%9C%A8%E5%93%AA%E4%B8%80%E6%AD%A5%E5%8F%98%E6%88%90beanDefinition%E7%9A%84">普通Java类是在哪一步变成beanDefinition的</a></p>

<hr id="hr-toc" /><p></p>

<h1 id="Bean%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F">Bean的生命周期</h1>

<h2 id="%E4%BA%94%E4%B8%AA%E9%98%B6%E6%AE%B5">五个阶段</h2>

<p><strong>1.创建前准备</strong></p>

<p><strong>2.正式创建bean</strong></p>

<p><strong>3.依赖注入</strong></p>

<p><strong>4.容器缓存</strong></p>

<p>    使用bean</p>

<p><strong>5.bean的销毁</strong></p>

<p></p>

<h2 id="%E4%B8%8B%E9%9D%A2%E6%98%AF%E4%B8%80%E4%B8%AAbean%E5%AF%B9%E8%B1%A1%E5%88%9B%E5%BB%BA%E5%88%B0%E9%94%80%E6%AF%81%E7%BB%8F%E5%8E%86%E8%BF%87%E7%9A%84%E6%96%B9%E6%B3%95%E3%80%82"><strong>下面是一个bean对象创建到销毁经历过的方法。</strong></h2>

<p><strong>==================现在开始初始化容器==================</strong><br /><span style="color:#fe2c24;">1.1</span> [BeanFactoryPostProcessor]这是BeanFactoryPostProcessor实现类构造器！！<br /><span style="color:#fe2c24;">1.2</span> [BeanFactoryPostProcessor]BeanFactoryPostProcessor调用postProcessBeanFactory方法<br /><span style="color:#fe2c24;">1.3</span> [BeanPostProcessor]这是BeanPostProcessor实现类构造器！！<br /><span style="color:#fe2c24;">1.4</span> [InstantiationAwareBeanPostProcessorAdapter]这是InstantiationAwareBeanPostProcessorAdapter实现类构造器！！<br /><span style="color:#fe2c24;">1.5</span> [InstantiationAwareBeanPostProcessorAdapter]InstantiationAwareBeanPostProcessor调用postProcessBeforeInstantiation方法</p>

<p><br /><span style="color:#4da8ee;">2.1</span> [Person]【构造器】调用Person的构造器实例化<br /><span style="color:#4da8ee;">2.2</span> [InstantiationAwareBeanPostProcessorAdapter]InstantiationAwareBeanPostProcessor调用<strong>postProcessPropertyValues</strong>方法</p>

<p><br /><strong><span style="color:#0d0016;">3.1</span></strong> [Person]【注入属性】注入属性address<br /><strong>3.1</strong> [Person]【注入属性】注入属性name<br /><strong>3.1</strong> [Person]【注入属性】注入属性phone<br /><strong>3.2</strong> [Person]【BeanNameAware接口】调用<strong>BeanNameAware.setBeanName()</strong><br /><strong>3.3</strong> [Person]【BeanFactoryAware接口】调用<strong>BeanFactoryAware.setBeanFactory()</strong><br /><strong>3.4</strong> [BeanPostProcessor]BeanPostProcessor接口方法postProcessBeforeInitialization对属性进行更改！<br /><strong>3.5</strong> [Person]【InitializingBean接口】调用InitializingBean.afterPropertiesSet()</p>

<p><br /><span style="color:#956fe7;">4.1</span> [Person]【init-method】调用&lt;bean&gt;的init-method属性指定的初始化方法<br /><span style="color:#956fe7;">4.2</span> [BeanPostProcessor]BeanPostProcessor接口方法postProcessAfterInitialization对属性进行更改！<br /><span style="color:#956fe7;">4.3</span> [InstantiationAwareBeanPostProcessorAdapter]调用postProcessAfterInitialization方法</p>

<p><br /><strong>==================容器初始化成功==================</strong></p>

<p><span style="color:#fe2c24;"><strong>使用bean</strong></span><br />
Person [address=广州, name=张三, phone=110]</p>

<p><br /><strong>==================现在开始关闭容器！==================</strong><br /><span style="color:#ff9900;">5.1</span> [Person]【DiposibleBean接口】调用DiposibleBean.destory()<br /><span style="color:#ff9900;">5.2 </span>[Person]【destroy-method】调用&lt;bean&gt;的destroy-method属性指定的初始化方法</p>

<p> </p>

<h2 id="%E5%9B%BE%E7%A4%BA%E2%80%8B">图示<img alt="" height="749" src="https://img-blog.csdnimg.cn/968e9bb333794270aff223dc04bdf732.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1197" /></h2>

<p> <img alt="" height="528" src="https://img-blog.csdnimg.cn/0ecb92592eb44758b82b7b29732d088d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1102" /></p>

<p></p>

<p><img alt="" height="333" src="https://img-blog.csdnimg.cn/3bb7bf9291b24ebb8ec6ac1185dd6db6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1022" /></p>

<p></p>

<p><img alt="" height="264" src="https://img-blog.csdnimg.cn/88376110b7bb42cba6144bf7a451eb76.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="947" /></p>

<p> <img alt="" height="164" src="https://img-blog.csdnimg.cn/07d90fefee524d648585d87627f5ee05.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="769" /></p>

<p></p>

<p><img alt="" height="1200" src="https://img-blog.csdnimg.cn/355481dfd210449aa7984dcc01c87fb1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1113" /></p>

<h1 id="%E9%97%AE%E7%AD%94">问答</h1>

<h2 id="%E6%99%AE%E9%80%9AJava%E7%B1%BB%E6%98%AF%E5%9C%A8%E5%93%AA%E4%B8%80%E6%AD%A5%E5%8F%98%E6%88%90beanDefinition%E7%9A%84">普通Java类是在哪一步变成beanDefinition的</h2>

<p>不用spring的时候 普通Java类对象的创建：</p>

<p><img alt="" height="354" src="https://img-blog.csdnimg.cn/4a8e9f7c201046bc966b50a586184581.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="767" /></p>

<p></p>

<p>而在spring中是所有的对象都是bean，普通Java类在创建前准备后就会正式成为bean，之后就是依赖注入，最后初始化。</p>

<p></p>

<p>参考</p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="Spring 了解Bean的一生(生命周期)_浅然的专栏-CSDN博客_简述bean的生命周期" href="https://blog.csdn.net/w_linux/article/details/80086950?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164673932016780269820761%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&amp;request_id=164673932016780269820761&amp;biz_id=0&amp;utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-3-80086950.pc_search_result_cache&amp;utm_term=Bean%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F&amp;spm=1018.2226.3001.4187" title="Spring 了解Bean的一生(生命周期)_浅然的专栏-CSDN博客_简述bean的生命周期">Spring 了解Bean的一生(生命周期)_浅然的专栏-CSDN博客_简述bean的生命周期</a></p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="Spring Bean的生命周期（非常详细） - Chandler Qian - 博客园" href="https://www.cnblogs.com/zrtqsk/p/3735273.html" title="Spring Bean的生命周期（非常详细） - Chandler Qian - 博客园">Spring Bean的生命周期（非常详细） - Chandler Qian - 博客园</a></p>

<p><a class="has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="Spring面试7连问|Bean的生命周期|AOP底层原理|@Autowired、@Resource、Configuration底层原理|事务底层原理|启动步骤_哔哩哔哩_bilibili" href="https://www.bilibili.com/video/BV1hp4y1h7Di/?spm_id_from=333.788.recommend_more_video.0" title="Spring面试7连问|Bean的生命周期|AOP底层原理|@Autowired、@Resource、Configuration底层原理|事务底层原理|启动步骤_哔哩哔哩_bilibili"><span class="link-card-box"><span class="link-title">Spring面试7连问|Bean的生命周期|AOP底层原理|@Autowired、@Resource、Configuration底层原理|事务底层原理|启动步骤_哔哩哔哩_bilibili</span><span class="link-link"><img alt="" class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" />https://www.bilibili.com/video/BV1hp4y1h7Di/?spm_id_from=333.788.recommend_more_video.0</span></span></a><a class="has-card" data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="细说Spring——IOC详解(Bean的生命周期）_哔哩哔哩_bilibili" href="https://www.bilibili.com/video/BV1Pv411q7XM?from=search&amp;seid=15511048826781431632&amp;spm_id_from=333.337.0.0" title="细说Spring——IOC详解(Bean的生命周期）_哔哩哔哩_bilibili"><span class="link-card-box"><span class="link-title">细说Spring——IOC详解(Bean的生命周期）_哔哩哔哩_bilibili</span><span class="link-link"><img alt="" class="link-link-icon" src="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" />https://www.bilibili.com/video/BV1Pv411q7XM?from=search&amp;seid=15511048826781431632&amp;spm_id_from=333.337.0.0</span></span></a></p>
