---
title: javaweb之文件上传和下载
tags: java javaweb
categories: Java基础知识
abbrlink: c8bb87c6
date: 2021-03-01 20:02:04
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0-toc" style="margin-left:0px;"><a href="#%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0">如何实现文件上传</a></p>

<p id="%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0%E7%9A%844%E4%B8%AA%E6%AD%A5%E9%AA%A4-toc" style="margin-left:80px;"><a href="#%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0%E7%9A%844%E4%B8%AA%E6%AD%A5%E9%AA%A4">文件上传jsp页面的设计要求</a></p>

<p id="1.%E5%86%99%E4%B8%80%E4%B8%AAjsp%E9%A1%B5%E9%9D%A2%EF%BC%8C%E9%87%8C%E9%9D%A2%E6%9C%89%E4%B8%8A%E4%BC%A0%E6%96%87%E4%BB%B6%E7%9A%84%E8%A1%A8%E5%8D%95%E9%A1%B9%EF%BC%9A-toc" style="margin-left:80px;"><a href="#1.%E5%86%99%E4%B8%80%E4%B8%AAjsp%E9%A1%B5%E9%9D%A2%EF%BC%8C%E9%87%8C%E9%9D%A2%E6%9C%89%E4%B8%8A%E4%BC%A0%E6%96%87%E4%BB%B6%E7%9A%84%E8%A1%A8%E5%8D%95%E9%A1%B9%EF%BC%9A">1.写一个jsp页面，里面有上传文件的表单项：</a></p>

<p id="2.%E7%BC%96%E5%86%99servlet%E7%A8%8B%E5%BA%8F%EF%BC%9A-toc" style="margin-left:80px;"><a href="#2.%E7%BC%96%E5%86%99servlet%E7%A8%8B%E5%BA%8F%EF%BC%9A">2.编写servlet程序：</a></p>

<p id="3.%E5%9C%A8web.xml%E4%B8%AD%E9%85%8D%E7%BD%AEservlet%E7%A8%8B%E5%BA%8F%E3%80%82-toc" style="margin-left:80px;"><a href="#3.%E5%9C%A8web.xml%E4%B8%AD%E9%85%8D%E7%BD%AEservlet%E7%A8%8B%E5%BA%8F%E3%80%82">3.在web.xml中配置servlet程序。</a></p>

<p id="%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E6%96%87%E4%BB%B6%E4%B8%8B%E8%BD%BD-toc" style="margin-left:0px;"><a href="#%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E6%96%87%E4%BB%B6%E4%B8%8B%E8%BD%BD">如何实现文件下载</a></p>

<p id="1.%E7%BC%96%E5%86%99%E6%96%87%E4%BB%B6%E4%B8%8B%E8%BD%BD%E7%9A%84servlet%E7%A8%8B%E5%BA%8F-toc" style="margin-left:80px;"><a href="#1.%E7%BC%96%E5%86%99%E6%96%87%E4%BB%B6%E4%B8%8B%E8%BD%BD%E7%9A%84servlet%E7%A8%8B%E5%BA%8F">1.编写文件下载的servlet程序</a></p>

<p id="2.%E5%9C%A8web.xml%E4%B8%AD%E5%81%9A%E7%9B%B8%E5%85%B3%E9%85%8D%E7%BD%AE-toc" style="margin-left:80px;"><a href="#2.%E5%9C%A8web.xml%E4%B8%AD%E5%81%9A%E7%9B%B8%E5%85%B3%E9%85%8D%E7%BD%AE">2.在web.xml中做相关配置</a></p>

<p id="3.%E5%9C%A8%E6%B5%8F%E8%A7%88%E5%99%A8%E4%B8%AD%E8%BD%AC%E5%88%B0%E8%AF%A5servlet%E8%B5%84%E6%BA%90%EF%BC%8C%E5%B0%B1%E5%8F%AF%E4%BB%A5%E5%AE%9E%E7%8E%B0%E4%B8%8B%E8%BD%BD%E3%80%82-toc" style="margin-left:80px;"><a href="#3.%E5%9C%A8%E6%B5%8F%E8%A7%88%E5%99%A8%E4%B8%AD%E8%BD%AC%E5%88%B0%E8%AF%A5servlet%E8%B5%84%E6%BA%90%EF%BC%8C%E5%B0%B1%E5%8F%AF%E4%BB%A5%E5%AE%9E%E7%8E%B0%E4%B8%8B%E8%BD%BD%E3%80%82">3.在浏览器中转到该servlet资源，就可以实现下载。</a></p>

<p id="%E9%81%87%E5%88%B0%E7%9A%84%E9%97%AE%E9%A2%98-toc" style="margin-left:0px;"><a href="#%E9%81%87%E5%88%B0%E7%9A%84%E9%97%AE%E9%A2%98">遇到的问题</a></p>

<p id="%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0%E6%97%B6-toc" style="margin-left:40px;"><a href="#%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0%E6%97%B6">文件上传时</a></p>

<p id="%E6%96%87%E4%BB%B6%E4%B8%8B%E8%BD%BD%E6%97%B6-toc" style="margin-left:40px;"><a href="#%E6%96%87%E4%BB%B6%E4%B8%8B%E8%BD%BD%E6%97%B6">文件下载时</a></p>

<hr id="hr-toc" /><h1 id="%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0">如何实现文件上传</h1>

<p><strong>文件上传：在浏览器的页面点击上传文件，提交后转向servlet程序，然后servlet程序读取后保存在本地文件夹下。</strong></p>

<h3 id="%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0%E7%9A%844%E4%B8%AA%E6%AD%A5%E9%AA%A4">文件上传jsp页面的设计要求</h3>

<p><img alt="" height="275" src="https://img-blog.csdnimg.cn/20210301185415229.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="1200" /></p>

<h3 id="1.%E5%86%99%E4%B8%80%E4%B8%AAjsp%E9%A1%B5%E9%9D%A2%EF%BC%8C%E9%87%8C%E9%9D%A2%E6%9C%89%E4%B8%8A%E4%BC%A0%E6%96%87%E4%BB%B6%E7%9A%84%E8%A1%A8%E5%8D%95%E9%A1%B9%EF%BC%9A">1.写一个jsp页面，里面有上传文件的表单项：</h3>

<blockquote>
<pre>
<code class="language-html hljs">&lt;%@ page contentType="text/html;charset=UTF-8" language="java" %&gt;
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;Title&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;form action="http://localhost:8080/jsp/uploadServlet" method="post" enctype="multipart/form-data"&gt; 
        用户名：&lt;input type="text" name="username" /&gt; &lt;br&gt;
        头像：&lt;input type="file" name="photo" &gt; &lt;br&gt;
        &lt;input type="submit" value="上传"&gt;
    &lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
</blockquote>

<p><img alt="" height="79" src="https://img-blog.csdnimg.cn/20210301195808682.png" width="248" /></p>

<p>get和post的区别：</p>

<p>对于浏览器中<strong>非</strong>Ajax的HTTP请求，即从HTML和浏览器诞生就一直使用的HTTP协议中的GET/POST。浏览器用GET请求来获取一个html页面/图片/css/js等资源；用POST来提交一个&lt;form&gt;表单，并得到一个结果的网页。</p>

<h3 id="2.%E7%BC%96%E5%86%99servlet%E7%A8%8B%E5%BA%8F%EF%BC%9A"><strong>2.编写servlet程序：</strong></h3>

<blockquote>
<pre>
<code class="language-html hljs">package servlet;


import org.apache.commons.fileupload.FileItem;
import org.apache.commons.fileupload.FileItemFactory;
import org.apache.commons.fileupload.disk.DiskFileItemFactory;
import org.apache.commons.fileupload.servlet.ServletFileUpload;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.IOException;
import java.util.List;

public class UploadServlet extends HttpServlet {
    /**
     * 用来处理上传的数据
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

//        System.out.println("transfer");
        //1 先判断上传的数据是否多段数据（只有是多段的数据，才是文件上传的）
        if (ServletFileUpload.isMultipartContent(req)) {
//           创建FileItemFactory工厂实现类
            FileItemFactory fileItemFactory = new DiskFileItemFactory();
            // 创建用于解析上传数据的工具类ServletFileUpload类
            ServletFileUpload servletFileUpload = new ServletFileUpload(fileItemFactory);
            try {
                // 解析上传的数据，得到每一个表单项FileItem
                List&lt;FileItem&gt; list = servletFileUpload.parseRequest(req);
                // 循环判断，每一个表单项，是普通类型，还是上传的文件
                for (FileItem fileItem : list) {

                    if (fileItem.isFormField()) {
                        // 普通表单项

                        System.out.println("表单项的name属性值：" + fileItem.getFieldName());
                        // 参数UTF-8.解决乱码问题
                        System.out.println("表单项的value属性值：" + fileItem.getString("UTF-8"));

                    } else {
                        // 上传的文件
                        System.out.println("表单项的name属性值：" + fileItem.getFieldName());
                        System.out.println("上传的文件名：" + fileItem.getName());

                        fileItem.write(new File("此处填写地址\\" + fileItem.getName()));
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }

    }

}
</code></pre>
</blockquote>

<h3 id="3.%E5%9C%A8web.xml%E4%B8%AD%E9%85%8D%E7%BD%AEservlet%E7%A8%8B%E5%BA%8F%E3%80%82"><strong>3.在web.xml中配置servlet程序</strong>。</h3>

<h1 id="%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E6%96%87%E4%BB%B6%E4%B8%8B%E8%BD%BD">如何实现文件下载</h1>

<h3 id="1.%E7%BC%96%E5%86%99%E6%96%87%E4%BB%B6%E4%B8%8B%E8%BD%BD%E7%9A%84servlet%E7%A8%8B%E5%BA%8F">1.编写文件下载的servlet程序</h3>

<blockquote>
<pre>
<code class="language-html hljs">package servlet;

import org.apache.commons.io.IOUtils;
import sun.misc.BASE64Encoder;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.URLEncoder;

public class Download extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
//        1、获取要下载的文件名
        String downloadFileName = "kiki.jpg";
//        2、读取要下载的文件内容 (通过ServletContext对象可以读取)
        ServletContext servletContext = getServletContext();
        // 获取要下载的文件类型
        String mimeType = servletContext.getMimeType("/file/" + downloadFileName);
        System.out.println("下载的文件类型：" + mimeType);
//        4、在回传前，通过响应头告诉客户端返回的数据类型
        resp.setContentType(mimeType);
//        5、还要告诉客户端收到的数据是用于下载使用（还是使用响应头）
        // Content-Disposition响应头，表示收到的数据怎么处理
        // attachment表示附件，表示下载使用
        // filename= 表示指定下载的文件名
        // url编码是把汉字转换成为%xx%xx的格式

//        resp.setHeader("Content-Disposition", "attachment; filename=康康.jpg" );


        if (req.getHeader("User-Agent").contains("Firefox")) {
            // 如果是火狐浏览器使用Base64编码
            resp.setHeader("Content-Disposition", "attachment; filename==?UTF-8?B?" + new BASE64Encoder().encode("康康.jpg".getBytes("UTF-8")) + "?=");
        } else {
            // 如果不是火狐，是IE或谷歌，使用URL编码操作
            resp.setHeader("Content-Disposition", "attachment; filename=" + URLEncoder.encode("中国.jpg", "UTF-8"));
        }


        /**
         * /斜杠被服务器解析表示地址为http://ip:port/工程名/  映射 到代码的Web目录
         */
        InputStream resourceAsStream = servletContext.getResourceAsStream("/file/" + downloadFileName);
        // 获取响应的输出流
        OutputStream outputStream = resp.getOutputStream();
        //        3、把下载的文件内容回传给客户端
        // 读取输入流中全部的数据，复制给输出流，输出给客户端
        IOUtils.copy(resourceAsStream, outputStream);
    }
}
</code></pre>
</blockquote>

<h3 id="2.%E5%9C%A8web.xml%E4%B8%AD%E5%81%9A%E7%9B%B8%E5%85%B3%E9%85%8D%E7%BD%AE">2.在web.xml中做相关配置</h3>

<h3 id="3.%E5%9C%A8%E6%B5%8F%E8%A7%88%E5%99%A8%E4%B8%AD%E8%BD%AC%E5%88%B0%E8%AF%A5servlet%E8%B5%84%E6%BA%90%EF%BC%8C%E5%B0%B1%E5%8F%AF%E4%BB%A5%E5%AE%9E%E7%8E%B0%E4%B8%8B%E8%BD%BD%E3%80%82">3.在浏览器中转到该servlet资源，就可以实现下载。</h3>

<h1 id="%E9%81%87%E5%88%B0%E7%9A%84%E9%97%AE%E9%A2%98">遇到的问题</h1>

<h2 id="%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0%E6%97%B6">文件上传时</h2>

<p> </p>

<p>Artifact jsp:war exploded: Error during artifact deployment.</p>

<p>Caused by: java.lang.NoClassDefFoundError: org/apache/commons/fileupload/FileItemFactory</p>

<p>找不到这个类，是因为要在这个地方加入支持的库</p>

<p><img alt="" height="590" src="https://img-blog.csdnimg.cn/20210301171743732.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="943" /></p>

<p><img alt="" height="330" src="https://img-blog.csdnimg.cn/20210301171714250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="714" /></p>

<h2 id="%E6%96%87%E4%BB%B6%E4%B8%8B%E8%BD%BD%E6%97%B6">文件下载时</h2>

<p><img alt="" height="355" src="https://img-blog.csdnimg.cn/20210301192624248.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NzkzMA==,size_16,color_FFFFFF,t_70" width="697" /></p>

<p>意思是download.java中的最后一条语句有错：IOUtils.copy(resourceAsStream, outputStream);但实际没错</p>

<p>重新启动工程即可</p>

<p> </p>

<p> </p>

<p> </p>
