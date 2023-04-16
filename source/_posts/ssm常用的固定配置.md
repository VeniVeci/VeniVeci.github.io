---
title: ssm常用的固定配置
tags: mybatis spring java 数据库 spring 3
categories: Spring框架
abbrlink: c80963cd
date: 2021-04-17 21:42:20
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="pom%E9%85%8D%E7%BD%AE-toc" style="margin-left:0px;"><a href="#pom%E9%85%8D%E7%BD%AE">1.pom配置</a></p>

<p id="%3Cdependencies%3E%E6%A0%87%E5%87%86%E9%85%8D%E7%BD%AE-toc" style="margin-left:40px;"><a href="#%3Cdependencies%3E%E6%A0%87%E5%87%86%E9%85%8D%E7%BD%AE">标准配置</a></p>

<p id="%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E5%AF%BC%E5%87%BA%E9%85%8D%E7%BD%AE-toc" style="margin-left:40px;"><a href="#%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E5%AF%BC%E5%87%BA%E9%85%8D%E7%BD%AE">静态资源导出配置</a></p>

<p id="mybatis%E9%85%8D%E7%BD%AE-toc" style="margin-left:0px;"><a href="#mybatis%E9%85%8D%E7%BD%AE">2.mybatis配置</a></p>

<p id="mapper.xml%C2%A0%20%E7%94%A8%E6%9D%A5%E5%86%99sql%E8%AF%AD%E5%8F%A5-toc" style="margin-left:40px;"><a href="#mapper.xml%C2%A0%20%E7%94%A8%E6%9D%A5%E5%86%99sql%E8%AF%AD%E5%8F%A5">mapper.xml  用来写sql语句</a></p>

<p id="mybatis-config.xml%C2%A0-toc" style="margin-left:40px;"><a href="#mybatis-config.xml%C2%A0">mybatis-config.xml </a></p>

<p id="%C2%A0database.properties-toc" style="margin-left:40px;"><a href="#%C2%A0database.properties">database.properties</a></p>

<p id="spring-dao.xml-toc" style="margin-left:40px;"><a href="#spring-dao.xml">spring-dao.xml</a></p>

<p id="%E6%B5%8B%E8%AF%95%E4%BB%A3%E7%A0%81-toc" style="margin-left:40px;"><a href="#%E6%B5%8B%E8%AF%95%E4%BB%A3%E7%A0%81">mybatis层代码测试</a></p>

<p id="springMVC-toc" style="margin-left:0px;"><a href="#springMVC">3.springMVC</a></p>

<p id="service%E5%B1%82%E7%9A%84%E4%BB%A3%E7%A0%81%2B%E5%A4%84%E7%90%86%E5%99%A8%E7%9A%84%E4%BB%A3%E7%A0%81%EF%BC%88controller%EF%BC%89-toc" style="margin-left:40px;"><a href="#service%E5%B1%82%E7%9A%84%E4%BB%A3%E7%A0%81%2B%E5%A4%84%E7%90%86%E5%99%A8%E7%9A%84%E4%BB%A3%E7%A0%81%EF%BC%88controller%EF%BC%89">service层的代码+处理器的代码（controller）</a></p>

<p id="%E5%BD%93%E7%84%B6%E8%BF%98%E9%9C%80%E8%A6%81%E9%85%8D%E7%BD%AE%E5%A5%BDweb.xml%EF%BC%8C%E5%8F%AA%E6%9C%89%E4%B8%80%E4%B8%AADispatcherServlet-toc" style="margin-left:40px;"><a href="#%E5%BD%93%E7%84%B6%E8%BF%98%E9%9C%80%E8%A6%81%E9%85%8D%E7%BD%AE%E5%A5%BDweb.xml%EF%BC%8C%E5%8F%AA%E6%9C%89%E4%B8%80%E4%B8%AADispatcherServlet">web.xml配置（只有一个DispatcherServlet）</a></p>

<p id="%E5%A4%A7%E4%B8%80%E7%BB%9F%20application.xml-toc" style="margin-left:0px;"><a href="#%E5%A4%A7%E4%B8%80%E7%BB%9F%20application.xml">4.application.xml大一统</a></p>

<p id="5.%E9%85%8D%E7%BD%AEtomcat%E5%8F%91%E5%B8%83-toc" style="margin-left:0px;"><a href="#5.%E9%85%8D%E7%BD%AEtomcat%E5%8F%91%E5%B8%83">5.配置tomcat发布</a></p>

<hr id="hr-toc" /><h1 id="pom%E9%85%8D%E7%BD%AE">1.pom配置</h1>

<h2 id="%3Cdependencies%3E%E6%A0%87%E5%87%86%E9%85%8D%E7%BD%AE">&lt;dependencies&gt;标准配置</h2>

<blockquote>
<pre>
&lt;!--标准模板--&gt;
    &lt;dependencies&gt;
        &lt;!--Junit--&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;junit&lt;/groupId&gt;
            &lt;artifactId&gt;junit&lt;/artifactId&gt;
            &lt;version&gt;4.12&lt;/version&gt;
        &lt;/dependency&gt;
        &lt;!--数据库驱动--&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;mysql&lt;/groupId&gt;
            &lt;artifactId&gt;mysql-connector-java&lt;/artifactId&gt;
            &lt;version&gt;5.1.47&lt;/version&gt;
        &lt;/dependency&gt;
        &lt;!-- 数据库连接池 dbcp--&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;com.mchange&lt;/groupId&gt;
            &lt;artifactId&gt;c3p0&lt;/artifactId&gt;
            &lt;version&gt;0.9.5.2&lt;/version&gt;
        &lt;/dependency&gt;

        &lt;!--Servlet - JSP --&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;javax.servlet&lt;/groupId&gt;
            &lt;artifactId&gt;servlet-api&lt;/artifactId&gt;
            &lt;version&gt;2.5&lt;/version&gt;
        &lt;/dependency&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;javax.servlet.jsp&lt;/groupId&gt;
            &lt;artifactId&gt;jsp-api&lt;/artifactId&gt;
            &lt;version&gt;2.2&lt;/version&gt;
        &lt;/dependency&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;javax.servlet&lt;/groupId&gt;
            &lt;artifactId&gt;jstl&lt;/artifactId&gt;
            &lt;version&gt;1.2&lt;/version&gt;
        &lt;/dependency&gt;

        &lt;!--Mybatis--&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;org.mybatis&lt;/groupId&gt;
            &lt;artifactId&gt;mybatis&lt;/artifactId&gt;
            &lt;version&gt;3.5.2&lt;/version&gt;
        &lt;/dependency&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;org.mybatis&lt;/groupId&gt;
            &lt;artifactId&gt;mybatis-spring&lt;/artifactId&gt;
            &lt;version&gt;2.0.2&lt;/version&gt;
        &lt;/dependency&gt;

        &lt;!--Spring--&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;org.springframework&lt;/groupId&gt;
            &lt;artifactId&gt;spring-webmvc&lt;/artifactId&gt;
            &lt;version&gt;5.1.9.RELEASE&lt;/version&gt;
        &lt;/dependency&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;org.springframework&lt;/groupId&gt;
            &lt;artifactId&gt;spring-jdbc&lt;/artifactId&gt;
            &lt;version&gt;5.1.9.RELEASE&lt;/version&gt;
        &lt;/dependency&gt;

        &lt;dependency&gt;
            &lt;groupId&gt;org.projectlombok&lt;/groupId&gt;
            &lt;artifactId&gt;lombok&lt;/artifactId&gt;
            &lt;version&gt;1.16.10&lt;/version&gt;
        &lt;/dependency&gt;
    &lt;/dependencies&gt;</pre>
</blockquote>

<h2 id="%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E5%AF%BC%E5%87%BA%E9%85%8D%E7%BD%AE">静态资源导出配置</h2>

<blockquote>
<p> &lt;build&gt;</p>

<pre>
    &lt;resources&gt;
        &lt;resource&gt;
            &lt;directory&gt;src/main/java&lt;/directory&gt;
            &lt;includes&gt;
                &lt;include&gt;**/*.properties&lt;/include&gt;
                &lt;include&gt;**/*.xml&lt;/include&gt;
            &lt;/includes&gt;
            &lt;filtering&gt;false&lt;/filtering&gt;
        &lt;/resource&gt;
        &lt;resource&gt;
            &lt;directory&gt;src/main/resources&lt;/directory&gt;
            &lt;includes&gt;
                &lt;include&gt;**/*.properties&lt;/include&gt;
                &lt;include&gt;**/*.xml&lt;/include&gt;
            &lt;/includes&gt;
            &lt;filtering&gt;false&lt;/filtering&gt;
        &lt;/resource&gt;
    &lt;/resources&gt;
&lt;/build&gt;</pre>
</blockquote>

<h1 id="mybatis%E9%85%8D%E7%BD%AE">2.mybatis配置</h1>

<p>首先需要一个pojo类以及对应的mapper接口和mapper.xml</p>

<h2 id="mapper.xml%C2%A0%20%E7%94%A8%E6%9D%A5%E5%86%99sql%E8%AF%AD%E5%8F%A5">mapper.xml  用来写sql语句</h2>

<p>mapper接口类需要和它对应 </p>

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="UTF-8" ?&gt;
&lt;!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd"&gt;


&lt;mapper namespace="com.alibaba.dao.BookMapper"&gt;
    &lt;select id="queryAllBook" resultType="Books"&gt;
        select * from books;
    &lt;/select&gt;
&lt;/mapper&gt;</pre>
</blockquote>

<h2 id="mybatis-config.xml%C2%A0">mybatis-config.xml </h2>

<p>这个文件可以直接整合到spring的配置文件spring-dao.xml中，删掉也没有关系</p>

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="UTF-8" ?&gt;
&lt;!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd"&gt;
&lt;configuration&gt;
    &lt;typeAliases&gt;
        &lt;package name="com.alibaba.pojo"/&gt;
    &lt;/typeAliases&gt;
    &lt;mappers&gt;
        &lt;mapper class="com.alibaba.dao.BookMapper"/&gt;
    &lt;/mappers&gt;
&lt;/configuration&gt;</pre>
</blockquote>

<h2 id="%C2%A0database.properties"> database.properties</h2>

<blockquote>
<pre>
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssmbuild?useSSL=true&amp;useUnicode=true&amp;characterEncoding=utf8
jdbc.username=root
jdbc.password=root</pre>
</blockquote>

<h2 id="spring-dao.xml">spring-dao.xml</h2>

<p>spring-dao.xml就实现了 dao层的功能  读取这个配置文件即可拿到 mapper去访问数据库</p>

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd"&gt;

    &lt;context:property-placeholder location="db.properties"/&gt;


    &lt;bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"&gt;
        &lt;property name="jdbcUrl" value="${jdbc.url}"/&gt;
        &lt;property name="driverClass" value="${jdbc.driver}"/&gt;
        &lt;property name="user" value="${jdbc.username}"/&gt;
        &lt;property name="password" value="${jdbc.password}"/&gt;

    &lt;/bean&gt;

    &lt;bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean"&gt;
        &lt;property name="dataSource" ref="dataSource"/&gt;
        &lt;property name="typeAliases" value="com.alibaba.pojo.Books"/&gt;
        &lt;property name="mapperLocations" value="com/alibaba/dao/BookMapper.xml"/&gt;
    &lt;/bean&gt;


    &lt;!-- 4.配置扫描Dao接口包，动态实现Dao接口注入到spring容器中 --&gt;
    &lt;bean class="org.mybatis.spring.mapper.MapperScannerConfigurer"&gt;
        &lt;!-- 注入sqlSessionFactory --&gt;
        &lt;property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/&gt;
        &lt;!-- 给出需要扫描Dao接口包 --&gt;
        &lt;property name="basePackage" value="com.alibaba.dao"/&gt;
    &lt;/bean&gt;

&lt;/beans&gt;</pre>
</blockquote>

<p>到现在dao层的代码基本实现，也就是说我们可以拿到mapper去访问数据库，实现基本的增删改查了。</p>

<h2 id="%E6%B5%8B%E8%AF%95%E4%BB%A3%E7%A0%81">mybatis层代码测试</h2>

<blockquote>
<pre>
@Test
public void test(){
    ApplicationContext context = new ClassPathXmlApplicationContext("spring-dao.xml");
    BookMapper mapper = (BookMapper) context.getBean("bookMapper");
    List&lt;Books&gt; books = mapper.queryAllBook();

    System.out.println(books);
}</pre>
</blockquote>

<h1 id="springMVC">3.springMVC</h1>

<h2 id="service%E5%B1%82%E7%9A%84%E4%BB%A3%E7%A0%81%2B%E5%A4%84%E7%90%86%E5%99%A8%E7%9A%84%E4%BB%A3%E7%A0%81%EF%BC%88controller%EF%BC%89">service层的代码+处理器的代码（controller）</h2>

<p>controller就可以处理前端的请求，调用service层，service层再调用dao层的mapper去访问数据库，</p>

<p>那么这就需要写一个配置文件，将mapper注入到service层，使得业务层可以调用mapper访问数据库</p>

<p>先把spring-mvc的配置写好，这个主要是为了让controller正常工作</p>

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans.xsd
   http://www.springframework.org/schema/context
   http://www.springframework.org/schema/context/spring-context.xsd
   http://www.springframework.org/schema/mvc
   https://www.springframework.org/schema/mvc/spring-mvc.xsd"&gt;

    &lt;!-- 配置SpringMVC --&gt;
    &lt;!-- 1.开启SpringMVC注解驱动 --&gt;
    &lt;mvc:annotation-driven /&gt;
    &lt;!-- 2.静态资源默认servlet配置--&gt;
    &lt;mvc:default-servlet-handler/&gt;

    &lt;!-- 3.配置jsp 显示ViewResolver视图解析器 --&gt;
    &lt;bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"&gt;
        &lt;property name="viewClass" value="org.springframework.web.servlet.view.JstlView" /&gt;
        &lt;property name="prefix" value="/WEB-INF/jsp/" /&gt;
        &lt;property name="suffix" value=".jsp" /&gt;
    &lt;/bean&gt;

    &lt;!-- 4.扫描web相关的bean --&gt;
    &lt;context:component-scan base-package="com.alibaba.controller" /&gt;

&lt;/beans&gt;</pre>
</blockquote>

<p> 接着写好 spring-service.xml，是为了把dao层和service层连接到一起</p>

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans.xsd
   http://www.springframework.org/schema/context
   http://www.springframework.org/schema/context/spring-context.xsd"&gt;

    &lt;import resource="spring-dao.xml"/&gt;
    &lt;!-- 扫描service相关的bean --&gt;
    &lt;context:component-scan base-package="com.alibaba.service" /&gt;

    &lt;!--BookServiceImpl注入到IOC容器中--&gt;
    &lt;bean id="BookServiceImpl" class="com.alibaba.service.BookServiceImpl"&gt;
        &lt;property name="bookMapper" ref="bookMapper"/&gt;
    &lt;/bean&gt;

    &lt;!-- 配置事务管理器 --&gt;
    &lt;bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager"&gt;
        &lt;!-- 注入数据库连接池 --&gt;
        &lt;property name="dataSource" ref="dataSource" /&gt;
    &lt;/bean&gt;

&lt;/beans&gt;</pre>
</blockquote>

<h2 id="%E5%BD%93%E7%84%B6%E8%BF%98%E9%9C%80%E8%A6%81%E9%85%8D%E7%BD%AE%E5%A5%BDweb.xml%EF%BC%8C%E5%8F%AA%E6%9C%89%E4%B8%80%E4%B8%AADispatcherServlet">web.xml配置（只有一个DispatcherServlet）</h2>

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"&gt;
    &lt;!--DispatcherServlet--&gt;
    &lt;servlet&gt;
        &lt;servlet-name&gt;DispatcherServlet&lt;/servlet-name&gt;
        &lt;servlet-class&gt;org.springframework.web.servlet.DispatcherServlet&lt;/servlet-class&gt;
        &lt;init-param&gt;
            &lt;param-name&gt;contextConfigLocation&lt;/param-name&gt;
          <u><strong><span style="color:#f33b45;">  &lt;!--一定要注意:我们这里加载的是总的配置文件，之前被这里坑了！--&gt;</span></strong></u>
            &lt;param-value&gt;classpath:application.xml&lt;/param-value&gt;
        &lt;/init-param&gt;
        &lt;load-on-startup&gt;1&lt;/load-on-startup&gt;
    &lt;/servlet&gt;
    &lt;servlet-mapping&gt;
        &lt;servlet-name&gt;DispatcherServlet&lt;/servlet-name&gt;
        &lt;url-pattern&gt;/&lt;/url-pattern&gt;
    &lt;/servlet-mapping&gt;

&lt;/web-app&gt;</pre>
</blockquote>

<h1 id="%E5%A4%A7%E4%B8%80%E7%BB%9F%20application.xml">4.application.xml大一统</h1>

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd"&gt;

    &lt;import resource="spring-dao.xml"/&gt;
    &lt;import resource="spring-mvc.xml"/&gt;
    &lt;import resource="spring-service.xml"/&gt;

&lt;/beans&gt;</pre>
</blockquote>

<h1 id="5.%E9%85%8D%E7%BD%AEtomcat%E5%8F%91%E5%B8%83">5.配置tomcat发布</h1>

<p>资料借鉴自 B站狂神说</p>
