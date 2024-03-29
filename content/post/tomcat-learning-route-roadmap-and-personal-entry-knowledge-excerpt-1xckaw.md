---
title: Tomcat学习路线roadmap和个人入门知识摘录
slug: tomcat-learning-route-roadmap-and-personal-entry-knowledge-excerpt-1xckaw
url: >-
  /post/tomcat-learning-route-roadmap-and-personal-entry-knowledge-excerpt-1xckaw.html
date: '2024-02-18 01:07:08'
lastmod: '2024-03-02 11:44:16'
toc: true
tags:
  - 运维
  - Tomcat
categories:
  - 技术
keywords: 运维,Tomcat
isCJKLanguage: true
---

# Tomcat学习路线roadmap和个人入门知识摘录

#### roadmap

* ### 参考《TOMCAT与JAVA WEB开发技术详解  第3版》，内容非常非常详细，初期入门并不需要学习到那么详细，后面精进学习可按图索骥，或者有需要再看看就行
* 第 1 章 Web 运作原理探析

  读者不妨带着以下问题去阅读本章开头的内容：  
  ●在整个 Web体系中，浏览器和Web服务器各自的功能是什么?  
  ●浏览器和Web服务器采用HTTP协议进行通信，该协议规定了通信的哪些具体细节?  
  本章接着介绍了Web的发展历程。  
  ●第一个阶段: 发布静态HTML文档。  
  ●第二个阶段:发布静态多媒体信息。  
  ●第三个阶段:提供浏览器端与用户的动态交互功能。  
  ●第四个阶段:提供服务器端与用户的动态交互功能。  
  ●第五个阶段:发布基于Web的应用程序，即Web应用。  
  ●第六个阶段:发布Web服务。  
  ●第七个阶段:先后推出Web 2.0以及Web 3.0。Web 2.0是全民共建的Web,用户  
  共同为Web提供丰富的内容。在Web 3.0中，网络为用户提供更智能、更个性化  
  的服务。

  * 1.1 Web 的概念

    * Web 的概念如下：Web 是一种分布式应用架构，旨在共享分布在网络上的各个 Web 服 务器中所有互相连接的信息。采用客户/服务器（B/S结构（Browser/Server））通信模式，客户与服务器之间用 HTTP 协议通信。Web 使用超级文本技术（HTML） 来连接网络上的信息。信息存放在服务器端， 客户机通过浏览器（例如 IE 或 Chrome）就可以查找网络中各个 Web服务器上的信息。  
      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012011723.png)​
    * 概念 WWW（World Wide Web）是指全球范围内的 Web
  * 1.2 HTML 简介

    * HTML ( Hyper Text Markup Language) 是指超文本标记语言。
  * 1.3 URL 简介
  * 1.4 HTTP 简介

    * 1.4.1 HTTP 请求格式

      * HTTP 规定，HTTP 请求由如下 3 部分构成：  
        • 请求方法、URI 和 HTTP 的版本。  
        • 请求头 ( RequestHeader)。  
        • 请求正文 (RequestContent)。  
        ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012010348.png)​

        ```html
        POST /hello.jsp HTTP/1.1   // HTTP 请求的第一行包括请求方式、URI 和协议版本这 3 项内容，以空格分开
        // “POST”为请求方式，“/hello.jsp”为 URI,“HTTP/1.1" 为 HTTP 的 版本
        Accept: image/gifz, image/jpeg, */*
        Referer: http://localhost/login.htm
        Accept-Language: enz,zh-cn;q=0.5   // 浏览器所用的语言
        Content-Type: application/x-www-form-urlencoded   // 正文类型
        Accept-Encoding: gzip, deflate
        User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64;   //浏览器类型
                 Trident/7.0; rv:11.0) like Gecko
        Host: localhost   // 远程主机
        Content-Length: 40   // 正文长度
        Connection: Keep-Alive
        Cache-Control: no-cache

        username=Tom&password=1234&submit=submit   //上面空行必须，HTTP 规定，请求头和请求正文之间必须以空行分隔 (即只有 CRLF 符号的行,(Carriage Return Linefeed ) 是指回车符和行结束符 " \r\n)，这个空行非常重要
        ```
      * 在以上代码中,“POST”为请求方式，“/hello.jsp”为 URI统一资源定位符 (UniversalResource Identifier, URI) 用于标记要访问的网络资源。,“HTTP/1.1" 为 HTTP 的 版本

        HTTP 请求可以使用多种方式，主要包括以下几种。 • GET：这种请求方式最为常见，客户程序通过这种请求方式访问服务器上的一个文 档，服务器把文档发送给客户程序。 • POST： 客户程序可以通过这种方式发送大量信息给服务器。在 HTTP 请求中除了 包含要访问的文档的 URI，还包括大量的请求正文，这些请求正文中通常会包含 HTML 表单数据。 • HEAD： 客户程序和服务器之间交流一些内部数据，服务器不会返回具体的文档。 当使用 GET 和 POST 方法时，服务器最后都将特定的文档返回给客户程序。而 HEAD 请求方式则不同，它仅仅交流一些内部数据，这些数据不会影响用户浏览网 页的过程，可以说对用户是透明的。HEAD 请求方式通常不单独使用，而是对其他 请求方式起辅助作用。一些搜索引擎使用 HEAD 请求方式来获得网页的标志信息， 还有一些 HTTP 服务器进行安全认证时，用这个方式来传递认证信息。 • PUT： 客户程序通过这种方式把文档上传给服务器。 • DELETE：客户程序通过这种方式来删除远程服务器上的某个文档。客户程序可以 利用 PUT 和 DELETE 请求方式来管理远程服务器上的文档。PUT 和 DELETE 请求方式并不常用，因此不少 HTTP 服务器并不支持 PUT 和 DELETE 请求方式
    * 1.4.2 HTTP 响应的格式

      * 和 HTTP 请求相似，HTTP 响应也由 3 部分构成，分别是：  
        • HTTP 的版本、状态代码和描述。  
        • 响应头 (ResponseHeader)。  
        • 响应正文 (ResponseContent)。  
        ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012010594.png)​

        ```html
        HTTP/1.1 200 OK   //服务器使用的HTTP 的版本、状态代码，以及对状态代码的描述
        Server: Apache-Coyote/1.1   // 服务器类型
        Content-type: text/html; charset=GBK   // 正文类型
        Content-length: 102   // 正文长度

        <html> // HTTP 请求头与请求正文之间必须用空行分隔，同样，HTTP 响应头与响应正文之间也必须用空行分隔
        <head>
        	<title>HelloWorld</title>
        </head>
        <body >
        	<hl>hello</hl>
        </body>
        </html>
        ```
        状态代码是一个 3 位整数，以 1、2、3、‘4 或 5 开头： • 1xx： 信息提示，表示临时的响应。 • 2xx： 响应成功，表明服务器成功地接收了客户端请求。 • 3XX： 重定向。 • 4xx： 客户端错误，表明客户端可能有问题。 • 5xx： 服务器错误，表明服务器由于遇到某种错误而不能响应客户请求。 以下是一些常见的状态代码： • 200： 响应成功。 • 400： 错误的请求。客户发送的HTTP 请求不正确。 • 404： 文件不存在。在服务器上没有客户要求访问的文档。 • 405： 服务器不支持客户的请求方式。 • 500： 服务器内部错误。
    * 1.4.3 正文部分的 MIME 类型

      * HTTP 请求以及响应的正文部分可以是任意格式的数据，如何保证接收方能“看懂”发 送方发送的正文数据呢？HTTP 采用 MIME 协议来规范正文的数据格式。MIME 协议由 W3C 组织制定,RFC2045 文档( http://www.ietf.org/rfc/rfc2045.txt)对 MIME 协议做了详细阐述。MIME (Multipurpose Internet Mail Extension) 是指多用途网络邮件扩展 协议，这里的邮件不单纯地指 E-Maib 还可以包括通过各种应用层协议在网络上传输的数据。 因此，也可以将 HTTP 中的请求正文和响应正文看作邮件。MIME 规定了邮件的标准数据格 式，从而使得接收方能“看懂”发送方发送的邮件。 遵守 MIME 协议的数据类型统称为 MIME 类型。在 HTTP 请求头和 HTTP 响应头中都 有一个 Content-type 项，用来指定请求正文部分或响应正文部分的 MIME 类型。表 1-2 列出 了常见的 MIME 类型与文件扩展名之间的对应关系。

        ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012010364.png)​
    * 1.4.4 HTTP 各个版本的特点

      * HTTP/1.1（1999）：持久 TCP 连接（Keep-Alive）、管道机制、更多请求如 PUT 和 DELETE、缺点按照先后顺序来处理 HTTP 请求会导致堵塞
      * HTTP/2.0（2015）：二进制协议、多路复用、头部信息压缩、推送、请求优先级和安全
  * 1.5 用 Java 套接字创建 HTTP 客户与服务器程序

    * 1.5.1 演示异构系统之间用 HTTP 协议通信

      如例HTTPServer 类实现了一个简单的 HTTP 服务器，它接收客户程序发出的 HTTP 请求，把它打印到控制台，然后解析 HTTP 请求，并向客户端发回相应的 HTTP 响应。

      ```java
      package server;
      import java.io.*;
      import java.net.*;

      public class HTTPServer{
        public static void main(String args[]) {
          int port;
          ServerSocket serverSocket;
          
          try { 
             port = Integer.parseInt(args[0]);
           }catch (Exception e) {
             System.out.println("port = 8080 (默认)");
             port = 8080; //默认端口为8080
           }

           try{
             serverSocket = new ServerSocket(port); 
             System.out.println("服务器正在监听端口：" 
                           + serverSocket.getLocalPort());
            
             while(true) { //服务器在一个无限循环中不断接收来自客户的TCP连接请求
               try{
                 //等待客户的TCP连接请求
                 final Socket socket = serverSocket.accept(); 
                 System.out.println("建立了与客户的一个新的TCP连接，"
                      +"该客户的地址为："
                      +socket.getInetAddress()+":" + socket.getPort());
              
                 service(socket);  //响应客户请求
              }catch(Exception e){
                System.out.println("客户端请求的资源不存在");} 
            } //#while
          }catch (Exception e) {e.printStackTrace();}
        }

        /** 响应客户的HTTP请求 */
        public static void service(Socket socket)throws Exception{
        
          /*读取HTTP请求信息*/
          InputStream socketIn=socket.getInputStream(); //获得输入流
          Thread.sleep(500);  //睡眠500毫秒，等待HTTP请求  
          int size=socketIn.available();
          byte[] buffer=new byte[size];
          socketIn.read(buffer);
          String request=new String(buffer);
          System.out.println(request); //打印HTTP请求数据
        
          /*解析HTTP请求*/
          //获得HTTP请求的第一行
          int endIndex=request.indexOf("\r\n");
          if(endIndex==-1)
            endIndex=request.length();
          String firstLineOfRequest=
                     request.substring(0,endIndex);

          //解析HTTP请求的第一行 
          String[] parts=firstLineOfRequest.split(" "); 
          String uri="";
          if(parts.length>=2)
            uri=parts[1]; //获得HTTP请求中的uri    

          /*决定HTTP响应正文的类型，此处作了简化处理*/
          String contentType;
          if(uri.indexOf("html")!=-1 || uri.indexOf("htm")!=-1)
            contentType="text/html";
          else if(uri.indexOf("jpg")!=-1 || uri.indexOf("jpeg")!=-1)
            contentType="image/jpeg";
          else if(uri.indexOf("gif")!=-1) 
            contentType="image/gif";
          else
            contentType="application/octet-stream";  //字节流类型
        
        
          /*创建HTTP响应结果 */
          //HTTP响应的第一行
          String responseFirstLine="HTTP/1.1 200 OK\r\n";
          //HTTP响应头
          String responseHeader="Content-Type:"+contentType+"\r\n\r\n";
          //获得读取响应正文数据的输入流
          InputStream in=HTTPServer
                           .class
                           .getResourceAsStream("root/"+uri);
        
          /*发送HTTP响应结果 */
          OutputStream socketOut=socket.getOutputStream(); //获得输出流
          //发送HTTP响应的第一行
          socketOut.write(responseFirstLine.getBytes());
          //发送HTTP响应的头
          socketOut.write(responseHeader.getBytes());
          //发送HTTP响应的正文
          int len=0;
          buffer=new byte[128];
          while((len=in.read(buffer))!=-1)
          socketOut.write(buffer,0,len);  
        
          Thread.sleep(1000);  //睡眠1秒，等待客户接收HTTP响应结果        
          socket.close(); //关闭TCP连接  
         }
      }
      ```
      下例HTTPClient 类是一个简单的 HTTP 客户程序，它以 GET 方式向 HTTP 服务 器发送 HTTP 请求，然后把接收到的 HTTP 响应结果打印到控制台。

      ```java
      package client;
      import java.net.*;
      import java.io.*;
      import java.util.*;

      public class HTTPClient {
        public static void main(String args[]){
          //确定HTTP请求的uri
          String uri="index.htm";
          if(args.length !=0)uri=args[0]; 
        
          doGet("localhost",8080,uri); //按照GET请求方式访问HTTPServer 
        }
        
        /** 按照GET请求方式访问HTTPServer */
        public static void doGet(String host,int port,String uri){
          Socket socket=null;
        
          try{
            socket=new Socket(host,port); //与HTTPServer建立FTP连接
          }catch(Exception e){e.printStackTrace();}
        
          try{
            /*创建HTTP请求 */
            StringBuffer sb=new StringBuffer("GET "+uri+" HTTP/1.1\r\n");
            sb.append("Accept: */*\r\n");
            sb.append("Accept-Language: zh-cn\r\n");
            sb.append("Accept-Encoding: gzip, deflate\r\n");
            sb.append("User-Agent: HTTPClient\r\n");
            sb.append("Host: localhost:8080\r\n");
            sb.append("Connection: Keep-Alive\r\n\r\n");
        
            /*发送HTTP请求*/
            OutputStream socketOut=socket.getOutputStream(); //获得输出流  
            socketOut.write(sb.toString().getBytes());
        
            Thread.sleep(2000); //睡眠2秒，等待响应结果
        
            /*接收响应结果*/
            InputStream socketIn=socket.getInputStream(); //获得输入流
            int size=socketIn.available();
            byte[] buffer=new byte[size];
            socketIn.read(buffer);
            System.out.println(new String(buffer)); //打印响应结果
          
          }catch(Exception e){ 
            e.printStackTrace();
          }finally{
            try{
              socket.close();  
            }catch(Exception e){e.printStackTrace();}
          }
        } //#doGet()
      }
      ```
      * HTTP 客户程序和服务器分别按 4 种方式运行 HTTP 服务器和客户程序  
        ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012011524.png)  
        （1） HTTPClient 客户程序访问 HTTPServer 程序  
        ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012011487.png)  
        （2）浏览器访问 HTTPServer 程序  
        ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012011576.png)  
        （3）HTTPClient客户程序访问 Tomcat 服务器  
        （4）浏览器访问 Tomcat 服务器  
        ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012011462.png)​
    * 1.5.2 演示对网页中超链接的处理过程

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012011791.png)​

      用鼠标单击超链接 "hellol.htm（纯文本网页）”，此时浏览器会再次与HTTPServer 建立 TCP 连接，然后发出一个要求访问“hellol.htm”文件的 HTTP 请求。
    * 1.5.3 演示对网页中图片的处理过程  
      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012011513.png)​

      在浏览器中输入指向 hello2.htm 文件的 URL 时，浏览器先向 HTTPServer 请求访 问 hello2.htm 文件，浏览器接收到该文件的数据后，再对其解析。在解析文件中的＜img＞标 记时，会根据＜img＞标记的 src属性的值，再次向 HTTPServer 发出一个要求访问 bird.gif文 件的 HTTP 请求，HTTPServer 把本地文件系统中 bird.gif文件的数据发送给浏览器，浏览器 再把它在自己的窗口中展示出来。观察 HTTPServer 端的打印结果，就可以看到浏览器与服务器建立了两次 FTP 连接， 并且浏览器发送了两个 HTTP 请求，这两个 HTTP 请求分别请求访问 hello2.htm 和 bird.gif 文件。
  * 1.6 Web 的发展历程（不分先后顺序）  
    ●第一个阶段:发布静态HTML文档。  
    ●第二个阶段:发布静态多媒体信息。  
    ●第三个阶段:提供浏览器端与用户的动态交互功能。  
    ●第四个阶段:提供服务器端与用户的动态交互功能。  
    ●第五个阶段: 发布基于Web的应用程序，即Web应用。  
    ●第六个阶段:发布Web服务。  
    ●第七个阶段:先后推出Web 2.0和Web 3.0，使得广大用户都可以为Web提供丰富的内容，而Web为用户提供更加便捷和智能的服务。

    * 1.6.1 发布静态 HTML 文档
    * 1.6.2 发布静态多媒体信息
    * 1.6.3 提供浏览器端与用户的动态交互功能

      到了本阶 段，用户不仅可以通过浏览器浏览信息，还可以与浏览器进行交互，当用户在浏览器端输入指向该类的 URL 时，Web 服务器就会运行 HelloServlet类，HelioServlet类生成 HTML 文档（相当于动态生成），并把它发送给浏览器。JavaScript 脚本使得用户与浏览器之间可以进行简单的动态交互。 浏览器能够响应用户单击鼠标的操作，动态切换待展示的图片。  
      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012012804.png)​
    * 1.6.4 提供服务器端与用户的动态交互功能  
      前面的三个阶段的技术发展点都是在客户端，而对 Web 服务器端都没有做 特别要求。到了本阶段，Web 服务器端增加了动态执行特定程序代码的功能，这使得 Web 服务器能利用特定程序代码来动态生成 HTML 文档。第一种方式：完全用编程语言编写的程序，例如 CGI（Common Gateway Interface） 程序和用 Java 编写的 Servlet程序。第二种方式：嵌入了程序代码的 HTML 文档，如 PHP、ASP 和 JSP 文档。JSP 文 档是指嵌入了 Java 程序代码的 HTML 文档。  
      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012012709.png)​
    * 1.6.5 发布 Web 应用  
      本阶段是在上一个阶段的基础上进一步发展起来的。Web 服务器端可以动态执行程序的 功能变得越来越强大，不仅能动态地生成 HTML 文档，而且能处理各种应用领域里的业务 逻辑，还能访问数据库。Web 逐渐被运用到电子财务、电子商务和电子政务等各个领域。随着 Web 应用的规模越来越大，对软件的可维护性和可重用性的要求也越来越高。一 些针对 Web 应用的设计模 式以及 框架软件应运而生。例如 模型一视图一控制器 ( Model-View-Controller, MVC) 设计模式被用到 Java Web 应用的开发中。  
      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012012789.png)​
    * 1.6.6 发布 Web 服务

      什么是 Web 服务呢？简单地理解，Web 服务可看作是被客户端远程调用的各种方法，这些方法能处理特定业务逻辑或者进行复杂的运算等。如图 演示了客户端 请求访问服务器端的一个 Web 服务的过程。  
      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012012418.png)  
      Web 服务架构采用简单对象访问协议 (Simple Object Access Protocol, SOAP) 作为通信 协议。SOAP 规定客户与服务器之间一律用 XML 语言进行通信。可扩展标记语言(Extensible Markup Language, XML) 是一种可扩展的跨平台的标记语言。SOAP 规定了客户端向服务 器端发送的 Web 服务请求的具体数据格式，以及服务器端向客户端发送的 Web 服务响应结 果的具体数据格式。  
      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012028793.png)  
      要建立 Web 服务架构，需要开发基于 SOAP 的服务器来发布、调用和响应 Web 服务，以及创建客户端程序来请求这些服务。全球范围内部署 Web 服务需要大量服务器，这在资源和时间上都是巨大挑战。然而，由于 Web 在 2000 年 Web 服务概念出现时已广泛普及，这为 Web 服务的快速传播提供了便利。  
      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012028027.png)​
    * 1.6.7 Web 2.0: 全民共建的 Web  
      在Web 1.0 中，广大用户主要是 Web 提供的信息的消费者，用户通过浏览器来获取信息。而 Web 2.0 则强调全民织网，发动广大民众来共同为 Web 提供信息来源。Web 2.0注重用户与 Web 的交 互，用户既是 Web 信息的消费者（浏览者），也是 Web 信息的制造者。如：Blog（博客）、站点摘要（Really Simple Syndication, RSS）、WIKI（百科全书）、社交网络软件（SocialNetwork Sofwaret, SNS）、即时通讯（Instant Messenger）
    * 1.6.8 Web3.0: 智能化处理海量信息  
      在 Web 3.0 中，信息变得海量化，人人都参与发布信息，这导致信息质量参差不齐。信 息发布平台提供者必须能对各种信息进行过滤，智能地为用户提供有用的信息，避免用户浪 费大量时间去处理无用的信息。涉及以下技术的流行和发展： • 以博客技术为代表，围绕网民互动及个性体验的互联网应用技术得到完善和发展。 • 虚拟货币得到普及，以及虚拟货币的兑换成为现实。 • 大家开始认同网络财富，并发展出安全的网络财务解决方案。 • 互联网应用变得更加细分、专业和兼容，网络信息内容的管理将由专业的内容管理商来负责。
  * 1.7 处理 HTTP 请求参数以及 HTML 表单  
    在 POST 方式下，浏览器也会把 HTML 表单数据作为请求 参数来处理，只不过此时请求参数位于 HTTP 请求的正文部分；而在 GET 方式下，请求参数则紧跟 HTTP 请求的第一行的 URI 后面  
    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012028212.png)  
    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403012028447.png)​
  * 1.8 客户端向服务器端上传文件  
    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403021109247.png)  
    单击网页中的【upload］按钮，浏览 器会向 HTTPServerl 发送一个包含 FromClient.txt文件数据的 HTTP 请求，HTTPServerl 收到该 HTTP 请求后，会将其打印出来, 内容如下：  
    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403021109086.png)​

    ```html
    POST /servlet/UploadServlet HTTP/1.1
    Accept: image/gif, image/jpeg, */*
    Referer: http://localhost:8080/hello6.htm
    Accept-Language: zh-cn
    Content-Type: multipart/form-data; boundary=----------d82d9a20188   
    //以上HTTP请求的正文部分为复合类型，包含两个子部分：文件部分和提交按钮部分。浏览器会随机产生一个字符串形式的边界 (boundary), 上面代码（Content-Type）设定了边界的取值
    ···
    Cookie: style=default

    ----------7d82d9a20188
    Content-Disposition:form-data;name="filedatan";
    				filename="C:\client\FromClient.txt"
    Content-Type: text/plain

    Datal in FromClient.txt
    Data2 in FromClient.txt
    Data3 in FromClient.txt
    Data4 in FromClient.txt
    ----------7d82d9a20188
    Content-Disposition: form-data; name="submit"

    upload
    ----------7d82d9a20188-
    ```
    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403021109590.png)  
    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403021109731.png)  
    UploadServlet 在处理文件部分的正文部分时，把它按照字节流而不是字符串流写到本地 文件中，因此会把客户端的 FromClient.rar 文件中的数据准确无误地保存到服务器端的文件系统中。  
    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403021109515.png)​
  * 1.9 小结
  * 1.10 思考题
* 第 2 章 Tomcat 简介  
  <span style="font-weight: bold;" class="bold">Web 应用</span>由 <span style="font-weight: bold;" class="bold">Web 服务器</span>来发布和运行，以可交互的 HTML 网页作为客户端界面由浏览器来展示。浏览器与 Web 服务器之间的远程数据交换遵循 HTTP 协议。由一个中介方制定 Web 应用与 Web 服务器进行协作的标准接口，Servlet 就是其中最主要的一个接口。中介方规定： • Web 服务器可以访问任意一个 Web 应用中所有实现 Servlet 接口的类。 • Web 应用中用于被 Web 服务器动态调用的程序代码位于 Servlet 接口的实现类中。  
  ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403021110416.png)  
  Oracle 公司制定了 Web 应用与 Web 服务器进行协作的一系列标准 Java 接口（统称为 Java Servlet API）,这一系列标准Java 接口和规约统称为 Servlet 规范。Servlet 规范的官方网址为： https://www.oracle.com/technetwork/java/javaee/documentation/index.html  
  由 Apache 开源软件组织创建的 <span style="font-weight: bold;" class="bold">Tomcat</span> 是一个符合 Servlet 规范的优秀 Servlet 容器。以 下图 2-2 演示了 Tomcat 与 Java Web 应用之间通过 Servlet 接口来协作的过程。  
  ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403021110789.png)​

  * 2.1 Tomcat 概述  
    Tomcat 是在 Oracle 公司的 JSWDK（JavaServer Web DevelopmentKit, 是 Oracle 公司推 出的小型 Servlet/JSP 调试工具）的基础上发展起来的一个优秀的 Servlet 容器，Tomcat 本身 完全用 Java 语言编写。可以从 Tomcat 的官方网址（http://tomcat.apache.org）来获取关于 Tomcat 的最新信息。
  * 2.2 Tomcat 作为 Servlet 容器的基本功能  
    Servlet是一种运行在服务器上的小插件。Servlet 最常见的用途是扩展 Web 服务器的功能，可作为非常安全的、可移植的、易于使用的 CGI 替代品。具有以下 特点： • 提供了可被服务器动态加载并执行的程序代码，为来自客户的请求提供相应服务。 • Servlet 完全用 Java 语言编写，因此要求运行 Servlet 的服务器必须支持 Java 语言。 • Servlet 完全在服务器端运行，因此它的运行不依赖于浏览器。不管浏览器是否支持 Java 语言, 都能请求访问服务器端的 Servlet  
    Tomcat 作为运行 Servlet 的容器，其基本功能是负责接收和解析来自客 户的请求，把客户的请求传送给相应的 Servlet, 并把 Servlet 的响应结果返回给客户。  
    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403021110841.png)  
    Servlet 规范规定，Servlet 容器响应客户请求访问特定 Servlet 的流程 如下:  
    （1）客户发出要求访问特定 Servlet的请求。  
    （2）Servlet容器接收到客户请求，对其解析。  
    （3）Servlet容器创建一个 ServletRequest对象，在 ServletRequest对象中包含了客户请求信息以及其他关于客户的相关信息，如请求头、请求正文，以及客户机的 IP 地址等。  
    （4）Servlet容器创建一个 ServletResponse对象。  
    （5）Servlet容器调用客户所请求的 Servlet的 service （）服务方法，并且把 ServletRequest 对象和 ServletResponse对象作为参数传给该服务方法。  
    （6）Servlet从 ServletRequest对象中可获得客户的请求信息。  
    （7）Servlet利用 ServletResponse对象来生成响应结果。  
    （8）Servlet容器把 Servlet生成的响应结果发送给客户。
  * 2.3 Tomcat 的组成结构  
    Tomcat 本身由一系列可配置的组件构成，其中核心组件是 Servlet容器组件，它是所有 其他 Tomcat 组件的顶层容器，用＜CATALINA_HOME＞ 表示 Tomcat 的安装根目录。Tomcat 的各个组件可以在/conf7server.xml 文件中进行配 置，每个 Tomcat 组件在 server.xml 文件中对应一种配置元素。以下代码以 XML 的形式展示 了各种 Tomcat 组件之间的关系：

    ```html
    	<Server>
    		<Service>
    			<Connector />
    			<Engine>
    				<Host>
    					<Context>
    					</Context>
    				</Host>
    			</Engine>
    		</Service>
    	</Server>
    ```
    在以上 XML 代码中，每个元素都代表一种 Tomcat 组件。这些元素可分为四类：  
    • 顶层类元素 包括〈Server〉元素和元素，它们位于整个配置文件的顶层。  
    • 连接器类元素 为〈Connector〉元素，代表介于客户与服务器之间的通信接口，负责将客户的请求发送 给服务器，并将服务器的响应结果发送给客户。  
    • 容器类元素，代表处理客户请求并生成响应结果的组件，有四种容器类元素，分别为〈Engine〉、<Host>、<Context> 和<Cluster>元素。Engine 组件为特定的 Service 组 件处理所有客户请求， Host 组件为特定的虚拟主机处理所有客户请求，Context 组件为特定的 Web 应用处理所有客 户请求。Cluster 组件负责为 Tomcat 集群系统进行会话复制、Context 组件的属性的复制，以 及集群范围内 WAR 文件的发布。本书第 26 章的 26.5 节（Tomat 集群）对配置 Tomcat 集群 系统做了进一步介绍。  
    • 嵌套类元素 代表可以嵌入到容器中的组件，如<Valve>元素和<Realm>元素等，这些元素的作用将在 后面的章节做介绍

    Tomcat 的组成结构是由 自身的实现决定的，与 Servlet 规 范无关。不同的服务器开发商可以用不同的方 式来实现符合 Servlet 规范的 Servlet 容器。

    下面，再对一些基本的 Tomcat 元素进行介绍。具体属性，可以 参照本书附录 A（server.xml 文件）：  
    • 元素 〈Server〉元素代表整个 Servlet 容器组件，它是 Tomcat 的顶层元素。〈Server〉元素中可 包含一个或多个元素。  
    • <Service>元素＜Service＞元素中包含一'个＜Engine＞元素，以及一个或多个＜（201111©以01＞元素，这些 ＜Connector＞元素共享同一个＜Engine＞元素  
    • ＜Connector＞元素 ＜Connector＞元素代表和客户程序实际交互的组件，它负责接收客户请求，以及向客户 返回响应结果。  
    • ＜Engine＞元素 每个＜Service＞元素只能包含一个〈Engine〉元素。＜Engine＞元素处理在同一个＜Service＞ 中所有＜Connector＞元素接收到的客户请求。  
    • ＜Host＞元素 一个〈Engine〉元素中可以包含多个＜Host＞元素。每个＜Host＞元素定义了一个虚拟主机， 它可以包含一个或多个 Web 应用。  
    • ＜Context＞元素 ＜Context＞元素是使用最频繁的元素。每个＜Context＞元素代表了运行在虚拟主机上的单 个 Web 应用。一个＜Host＞元素中可以包含多个＜Context＞元素。  
    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403021110228.png)  
    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403021110418.png)​
  * 2.4 Tomcat 的工作模式  
    Tomcat 作为 Servlet 容器，有以下三种工作模式

    * （1） 独立的 Servlet 容器  
      在这种模式下，Tomcat 是一个独立运行的 Java 程序。和运行其他 Java 程序一样，运行 Tomcat 需要启动一个 Java 虚拟机(JVM.Java Virtual Machine)进程，由该进程来运行 Tomcat  
      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403021110016.png)​
    * （2） 其他 Web 服务器进程内的 Servlet 容器  
      这种模式下，Tomcat 分为 Web 服务器插件和 Servlet 容器组件两部分。Web 服务器插件在其他 Web 服务器进程的内部地址空间启动一个 Java 虚拟机，Servlet 容器组件在此 Java 虚拟机中运行。如有客户端发出调用 Servlet 的请求，Web 服务器插件获得对此请求的控制并将它转发 (使用 JNI 通信机制) 给 Servlet 容器组件。  
      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403021109118.png)​
    * （3）其他 Web 服务器进程外的 Servlet容器  
      在这种模式下，Tomcat 分为 Web 服务器插件和 Servlet容器组件两部分。如图 2-9 所示, Web 服务器插件在其他 Web 服务器的外部地址空间启动一个 Java 虚拟机进程，Servlet容器 组件在此 Java 虚拟机中运行。如有客户端发出调用 Servlet的请求，Web 服务器插件获得对 此请求的控制并将它转发（采用 IPC 通信机制）给 Servlet容器。  
      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403021109405.png)​

    从 Tomcat 的三种工作模式可以看出，当 Tomcat 作为独立的 Servlet 容器来运行时，此 时 Tomcat 是能运行 Java Servlet的独立 Web 服务器。此外，Tomcat 还可作为其他 Web 服务 器进程内或者进程外的 Servlet容器，从而与其他 Web 服务器集成（如 Apache 和 IIS 服务 器等）。集成的意义在于：对于不支持运行 Java Servlet 的其他 Web 服务器，可通过集成 Tomcat 来提供运行 Servlet的功能。第 26 章将进一步介绍 Tomcat 和其他 Web 服务器的 集成方法。
  * 2.5 Tomcat 的版本

    * Tomcat 9.x 是目前比较稳定和成熟的版本建立在 Tomcat 8.x 的基础上，前者实现了 Servlet 4.0 和 JSP2.3 规范。此外， 它还提供了如下新特性： • 支持 HTTP/2 协议。 • 使用 OpenSSL（Open Secure Socket Layer, 开放安全套接字层密码库）来支持 TLS （Transport Layer Security, 安全传输层协议），在实现中使用了 JSSE（Java Secure Socket Extension, Java 安全套接字扩展）连接器。 • 支持 TLS 虚拟主机。
    * Tomcat 8.x 建立在 Tomcat 7.x 的基础上，前者实现了 Servlet 3.1 和 JSP 2.3 规范。此外， 它还提供了如下新特性： • 采用单个公共资源的实现机制，来替代早期版本中提供多个可扩展资源的特征。 • Tomcat 8.5.x 实现了 JASPIC1.1（Java Authentication Service Provider, Java 认证服务 提供者接口）规范。
    * Tomcat 7.x 建立在 Tomcat 6.x 的基础上，前者实现了 Servlet 3.0 和 JSP 2.3 规范，此外， 它还提供了如下新特性： • 针对 Web 应用的内存泄漏，进行检测和预防。 • 提高了 Tomcat 自带的两个 Web 应用的安全性能，该自带的两个 Web 应用分别是 Tomcat的控制平台和管理平台，参见本书第 24 章（Tomcat 的控制平台和管理平台）。 • 采取安全措施，防止 CSRF（Cross-Site Request Forgery, 跨站请求伪造）攻击网站。 • 允许在 Web 应用中直接包含外部内容。 • 重构连接器，对 Tomcat 内部实现代码进行清理和优化。
    * Tomcat 6.x 是基于 Tomcat 5.5.x 的升级版本，前者实现了 Servlet 2.5 和 JSP 2.1 规范。此 外，它还提供了如下新特性： • 优化对内存的使用。 • 先进的 IO（输入输出）功能，利用 JDK 的 java.nio 包中的接口与类来提高输入输 出的性能，并且增加了对异步通信的支持。 • 对 Tomcat 服务器的集群功能进行了重建。
    * Tomcat 5.5.x.与 Tomcat 5.0.x 一样支持 Servlet 2.4 和 JSP 2.0 规范。此外，在其底层实现 中，Tomcat 5.5.x 在原有版本的基础上进行了巨大改动。这些改动大大提高了服务器的运行 性能和稳定性，并且降低了运行服务器的成本
    * Tomcat 5.0.x 在 Tomcat 4.1 的基础上做了许多扩展和改进，它提供的新功能和新特性 包括： • 对服务器性能进一步优化，提高垃圾回收的效率。 • 采用 JMX 技术来监控服务器的运行。 • 提高了服务器的可扩展性和可靠性。 • 增强了对标签库（Tag Library）的支持。 • 利用 Windows wrapper 和 UNIX wrapper 技术改进了与操作系统平台的集成。 • 采用 JMX 技术来实现嵌入式的 Tomcat o • 提供了更完善的 Tomcat 文档
    * Tomcat 4.1.x 是 Tomcat 4.0.x 的升级版本。Tomcat 4.0.x 完全废弃了 Tomcat 3.x 的架构， 采用新的体系结构实现了 Servlet 容器。Tomcat 4.1.x 在 Tomcat 4.0.x 的基础上又进一步升级， 它提供的新功能和新特性包括： • 基于 JMX 的管理控制功能。 • 实现了新的 Coyote Connector（支持 HTTP/1.1,AJP 1.3 和 JNI）。 • 重写了 Jasper JSP 编译器。 • 提高了 Web 管理应用与开发工具的集成。 • 提供客户化的 Ant 任务，使 Ant 程序根据 build.xml 脚本直接和 Web 管理应用交互。
  * 2.6 安装和配置 Tomcat 所需的资源
  * 2.7 安装 Tomcat
  * 2.8 启动 Tomcat 并测试 Tomcat 的安装
  * 2.9 Tomcat 的运行脚本
  * 2.10 小结
  * 2.11 思考题
* 第 3 章 第一个 Java Web 应用

  * 3.1 Java Web 应用简介  
    Java Web 应用由一组 Servlet/JSP、HTML 文件、相关 Java 类，以及其他可以被绑定的资源构成。它可以在由各种 供应商提供的符合 Servlet 规范的 Servlet 容器中运行。在 Java Web 应用中可以包含如下内容：  
    • Servlet 组件 标准 Servlet 接口的实现类，运行在服务器端，包含了被 Servlet 容器动态调用的程序 代码。  
    • JSP 组件 包含 Java 程序代码的 HTML 文档。运行在服务器端，当客户端请求访问 JSP 文件时，Servlet 容器先把它编译成 Servlet 类，然后动态调用它的程序代码  
    • 相关的 Java 类 开发人员自定义的与 Web 应用相关的 Java 类。  
    • 静态文档 存放在服务器端的文件系统中，如 HTML 文件、图片文件和声音文件等。当客户端请求访 问这些文件时，Servlet容器从本地文件系统中读取这些文件的数据，再把它发送到客户端。  
     • 客户端脚本程序 是由客户端来运行的程序，JavaScript是典型的客户端脚本程序，第 1 章的 163 节 （提供浏览器端与用户的动态交互功能）己经介绍了它的运行机制。  
     • web.xml 文件 Java Web 应用的配置文件，该文件采用 XML 格式。该文件必须位于 Web 应用的 WEB-INF 子目录下。
  * 3.2 创建 Java Web 应用  
    Servlet规范规定，Java Web 应用必须采用 固定的目录结构，每种类型的组件在 Web 应用中都有固定的存放目录。Servlet规范还规定， Java Web 应用的配置信息存放在 WEB-INF/web.xml 文件中，Servlet容器从该文件中读取配 置信息。在发布某些 Web 组件（如 Servlet）时，需要在 web.xml 文件中添加相应的关于这 些 Web 组件的配置信息

    * 3.2.1 Java Web 应用的目录结构  
      假定开发一个名为 helloapp的 Java Web 应用。首 先，应该创建这个 Web 应用的目录结构，参见表 3-1  
      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403021109089.png)  
      在 WEB-INF 目录的 classes 以及 lib 子目录下，都可以存放 Java 类 文件。在运行时,Servlet 容器的类加载器先加载 classes 目录下的类，再加载 lib 目录下的 JAR 文件（Java类库的打包文件）中的类。因此，如果两个目录下存在同名的类，classes 目录下 的类具有优先权。另外，浏览器端不可以直接请求访问 WEB-INF 目录下的文件，这些文件 只能被服务器端的组件访问。  
      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403021112632.png)  
      图 3-1 中有一个 src 目录，这是在开发 helloapp 应用阶段，开发人员自定义的目录，该 目录用来存放所有 Java 类的源文件。到了 Web 应用产品正式发布阶段，一般都不希望对外 公开 Java 源代码，所以届时应该将 src 目录转移到其他地方。 在 helloapp 应用中包含如下组件。  
      • HTML 组件：login.htm  
      • Servlet 组件：DispatcherServlet 类  
      • JSP 组件： hello.jsp  
      这些组件之间的关系如图 3-2 所示。login.htm 与 DispatcherServlet 类之间为超级链接关 系，DispatcherServlet 类与 hello.jsp 之间为请求转发关系，本书第 5 章的 5.6.1 节（请求转发） 对 Web组件之间的请求转发关系做了深入介绍。  
      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403021132229.png)​
    * 3.2.2 创建 HTML 文件
    * 3.2.3 创建 Servlet 类
    * 3.2.4 创建 JSP 文件
    * 3.2.5 创建 web.xml 文件
  * 3.3 在 Tomcat 中发布 Java Web 应用  
    发布的具体细节依赖于 Tomcat 本身的实现，但是以下关 于发布 Java Web 应用的基本思想适用于所有 Servlet容器：  
    • 把 Web 应用的所有文件复制到 Servlet容器的特定目录下，这是发布 Web 应用的最 快捷的一种方式。  
    • 各种 Servlet容器实现都会从 Web 应用的 web.xml 配置文件中读取有关 Web 组件的 配置信息。  
    • 为了使用户能更加灵活自如地控制 Servlet容器发布和运行 Web 应用的行为，并且 为了使 Servlet容器与 Web 应用能进行更紧密地协作，许多 Servlet容器还允许用户 使用额外的配置文件及配置元素，这些配置文件及配置元素的语法由 Servlet 容器 的实现决定，与 Oracle 公司的 Servlet规范无关

    * 3.3.1 Tomcat 的目录结构  
      Tomcat9.x 的目录结构参见 表 3-3, 表中的目录都是＜CATALINA_HOME＞ 的子目录。  
      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403021144200.png)​
    * 3.3.2 按照默认方式发布 Java Web 应用
    * 3.3.3 Web 组件的 URL
    * 3.3.4 配置 Tomcat 的 <Context> 元素
    * 3.3.5 配置 Tomcat 的虚拟主机
  * 3.4 创建、配置和使用自定义 JSP 标签
  * 3.5 用批处理文件或 ANT 编译范例
  * 3.6 小结
  * 3.7 思考题
* 第 4 章 Servlet技术（上）

  * 4.1 ServletAPI

    * 4.1.1 Servlet接口
    * 4.1.2 GenericServlet抽象类
    * 4.1.3 HttpServlet抽象类
    * 4.1.4 ServletRequest接口
    * 4.1.5 HttpServletRequest接口
    * 4.1.6 ServletResponse接口
    * 4.1.7 HttpServletResponse接口
    * 4.1.8 ServletConfig 接口
    * 4.1.9 ServletContext接口
  * 4.2 JavaWeb 应用的生命周期

    * 4.2.1 启动阶段
    * 4.2.2 运行时阶段
    * 4.2.3 终止阶段
    * 4.2.4 用 Tomcat 的管理平台管理 Web 应用的生命周期
  * 4.3 Servlet的 生命周期

    * 4.3.1 初始化阶段
    * 4.3.2 运行时阶段
    * 4.3.3 销毁阶段
    * 4.3.4 演示 Servlet 的生命周期的范例
  * 4.4 ServletContext与 Web 应用范围

    * 4.4.1 在 Web 应用范围内存放共享数据的范例
  * 4.5 Servlet的服务方法抛出异常
  * 4.6 防止页面被客户端缓存
  * 4.7 使用 Annotation 标注配置 Servlet
  * 4.8 处理 HTTP 请求参数中的中文字符编码
  * 4.9 小结
  * 4.10 思考题
* 第 5 章 Servlet技术（下）

  * 5.1 下载文件
  * 5.2 上传文件

    * 5.2.1 利用 Apache 开源类库实现文件上传
    * 5.2.2 利用 ServletAPI 中的 Part 接口实现文件上传
  * 5.3 动态生成图像
  * 5.4 读写 Cookie
  * 5.5 访问 Web 应用的工作目录
  * 5.6 转发和包含

    * 5.6.1 请求转发
    * 5.6.2 包含
    * 5.6.3 请求范围
  * 5.7 重定向
  * 5.8 访问 Servlet 容器内的其他 Web 应用
  * 5.9 避免并发问题

    * 5.9.1 合理决定在 Servlet 中定义的变量的作用域类型
    * 5.9.2 使用 Java 同步机制对多线程同步
    * 5.9.3 被废弃的 SingleThreadModel 接口
  * 5.10 对客户请求的异步处理

    * 5.10.1 异步处理的流程
    * 5.10.2 异步处理的范例
    * 5.10.3 异步监听器
    * 5.10.4 非阻塞 I/O
    * 5.10.5 服务器端推送
  * 5.11 小结
  * 5.12 思考题
* 第 6 章 JSP 技术

  * 6.1 比较 HTML、Servlet和 JSP

    * 6.1.1 静态 HTML 文件
    * 6.1.2 用 Servlet 动态生成 HTML 页面
    * 6.1.3 用 JSP 动态生成 HTML 页面
  * 6.2 JSP 语法

    * 6.2.1 JSP 指令 (Directive)
    * 6.2.2 JSP 声明
    * 6.2.3 Java程序片段 (Scriptlet)
    * 6.2.4 Java 表达式
    * 6.2.5 隐含对象
  * 6.3 JSP 的生命周期
  * 6.4 请求转发
  * 6.5 包含

    * 6.5.1 静态包含
    * 6.5.2 动态包含
    * 6.5.3 混合使用静态包含和动态包含
  * 6.6 JSP 异常处理
  * 6.7 再谈发布 JSP
  * 6.8 预编译 JSP
  * 6.9 PageContext类的用法
  * 6.10 在 web.xml 中配置 JSP
  * 6.11 JSP 技术的发展趋势
  * 6.12 小结
  * 6.13 思考题
* 第 7 章 bookstore应用简介

  * 7.1 bookstore应用的软件结构

    * 7.1.1 Web 服务器层
    * 7.1.2 数据库层
  * 7.2 浏览 bookstore 应用的 JSP 网页
  * 7.3 JavaBean 和实用类

    * 7.3.1 实体类
    * 7.3.2 购物车的实现
  * 7.4 发布 bookstore 应用
  * 7.5 小结
* 第 8 章 访问数据库

  * 8.1 安装和配置 MySQL 数据库
  * 8.2 JDBC 简介

    * 8.2.1 java.sql包中的接口和类
    * 8.2.2 编写访问数据库程序的步骤
    * 8.2.3 事务处理
  * 8.3 通过 JDBC API 访问数据库的 JSP 范例程序
  * 8.4 bookstore应用通过 JDBC API 访问数据库
  * 8.5 数据源 (DataSource) 简介
  * 8.6 配置数据源

    * 8.6.1 在 context.xml 中加入 <Resource> 元素
    * 8.6.2 在 web.xml 中加入 <resource-ref> 元素
  * 8.7 程序中访问数据源

    * 8.7.1 通过数据源连接数据库的 JSP 范例程序
    * 8.7.2 bookstore 应用通过数据源连接数据库
  * 8.8 处理数据库中数据的中文字符编码
  * 8.9 分页显示批量数据
  * 8.10 用可滚动结果集分页显示批量数据
  * 8.11 小结
  * 8.12 思考题
* 第 9 章 HTTP 会话的使用与管理

  * 9.1 会话简介
  * 9.2 HttpSession的生命周期及会话范围
  * 9.3 使用会话的 JSP 范例程序
  * 9.4 使用会话的 Servlet范例程序
  * 9.5 通过重写 URL 来跟踪会话
  * 9.6 会话的持久化

    * 9.6.1 标准会话管理器 StandardManager
    * 9.6.2 持久化会话管理器 PersistentManager
  * 9.7 会话的监听

    * 9.7.1 用 HttpSessionListener 统计在线用户人数
    * 9.7.2 用 HttpSessionBindingListener 统计在线用户人数
  * 9.8 小结
  * 9.9 思考题
* 第 10 章 JSP 访问 JavaBean

  * 10.1 JavaBean 简介
  * 10.2 JSP 访问 JavaBean 的语法
  * 10.3 JavaBean 的范围

    * 10.3.1 JavaBean 在页面（page）范围内
    * 10.3.2 JavaBean 在请求（request）范围内
    * 10.3.3 JavaBean 在会话（session）范围内
    * 10.3.4 JavaBean 在 Web 应用（application）范围内
  * 10.4 在 bookstore 应用中访问 JavaBean

    * 10.4.1 访问 BookDB 类
    * 10.4.2 访问 ShoppingCart 类
* 第 11 章 开发 JavaMail Web 应用

  * 11.1 E-Mail 协议简介

    * 11.1.1 SMTP 简单邮件传输协议
    * 11.1.2 POP3 邮局协议
    * 11.1.3 接收邮件的新协议 IMAP
  * 11.2 JavaMail API 简介
  * 11.3 建立 JavaMail 应用程序的开发环境

    * 11.3.1 获得 JavaMail API 的类库
    * 11.3.2 安装和配置邮件服务器
  * 11.4 创建 JavaMail 应用程序
  * 11.5 JavaMail Web 应用简介
  * 11.6 JavaMail Web 应用的程序结构

    * 11.6.1 重新封装 Message 数据
    * 11.6.2 用于保存邮件账号信息的 JavaBean
    * 11.6.3 定义所有 JSP 文件的相同内容
    * 11.6.4 登录 IMAP 服务器上的邮件账号
    * 11.6.5 管理邮件夹
    * 11.6.6 查看邮件中的邮件信息
    * 11.6.7 查看邮件内容
    * 11.6.8 创建和发送邮件
    * 11.6.9 退出邮件系统
  * 11.7 在 Tomcat 中配置邮件会话 (Mail Session)

    * 11.7.1 在 context.xml 中配置 Mail Session 资源
    * 11.7.2 在 web.xml 中加入对 JNDI Mail Session 资源的引用
    * 11.7.3 在 JavaMail 应用中获取 JNDI Mail Session 资源
  * 11.8 发布和运行 JavaMail 应用
  * 11.9 小结
  * 11.10 思考题
* 第 12 章 EL 表达式语言

  * 12.1 基本语法

    * 12.1.1 访问对象的属性及数组的元素
    * 12.1.2 EL 运算符
    * 12.1.3 隐含对象
    * 12.1.4 命名变量
  * 12.2 使用 EL 表达式的 JSP 范例

    * 12.2.1 关于基本语法的例子
    * 12.2.2 读取 HTML 表单数据的例子
    * 12.2.3 访问命名变量的例子
  * 12.3 定义和使用 EL 函数
  * 12.4 小结
  * 12.5 思考题
* 第 13 章 自定义 JSP 标签

  * 13.1 自定义 JSP 标签简介
  * 13.2 JSP Tag API

    * 13.2.1 JspTag 接口
    * 13.2.2 Tag 接口
    * 13.2.3 IterationTag 接口
    * 13.2.4 BodyTag 接口
    * 13.2.5 TagSupport 类和 BodyTagSupport 类
  * 13.3 message 标签范例

    * 13.3.1 创建 message 标签的处理类 MessageTag
    * 13.3.2 创建标签库描述文件
    * 13.3.3 在 Web 应用中使用标签
    * 13.3.4 发布支持中、英文版本的 helloapp 应用
  * 13.4 iterate标签范例（重复执行标签主体）
  * 13.5 greet标签范例（访问标签主体内容）
  * 13.6 小结
  * 13.7 思考题
* 第 14 章 采用模板设计网上书店应用

  * 14.1 如何设计网站的模板
  * 14.2 创建负责流程控制的 Servlet
  * 14.3 创建模板标签和模板 JSP 文件

    * 14.3.1 <parameter> 标签及其处理类
    * 14.3.2 <screen> 标签和处理类
    * 14.3.3 <definition> 标签和处理类
    * 14.3.4 <insert> 标签和处理类
  * 14.4 修改 JSP 文件
  * 14.5 发布采用模板设计的 bookstore 应用
  * 14.6 小结
  * 14.7 思考题
* 第 15 章 JSTL Core 标签库

  * 15.1 使用第三方提供的标签库的步骤
  * 15.2 JSTL 标签库简介
  * 15.3 一般用途的标签

    * 15.3.1 <c:out> 标签
    * 15.3.2 <c:set> 标签
    * 15.3.3 <c:remove> 标签
    * 15.3.4 <c:catch> 标签
  * 15.4 条件标签

    * 15.4.1 <c:if> 标签
    * 15.4.2 <c:choose>、<c:when> 和 <c:otherwise> 标签
  * 15.5 迭代标签

    * 15.5.1 <c:forEach> 标签
    * 15.5.2 <c:forTokens> 标签
  * 15.6 URL 相关的标签

    * 15.6.1 <c:import> 标签
    * 15.6.2 <c:url> 标签
    * 15.6.3 <c:redirect> 标签
  * 15.7 小结
  * 15.8 思考题
* 第 16 章 JSTL I18N 标签库

  * 16.1 国际化的概念
  * 16.2 Java 语言对 I18N 的支持

    * 16.2.1 Locale 类
    * 16.2.2 ResourceBundle 类
    * 16.2.3 MessageFormat 类和复合消息
  * 16.3 国际化标签

    * 16.3.1 [fmt:setLocale]() 标签
    * 16.3.2 [fmt:setBundle]() 标签
    * 16.3.3 [fmt:bundle]() 标签
    * 16.3.4 [fmt:message]() 标签
    * 16.3.5 [fmt:param]() 标签
    * 16.3.6 [fmt:requestEncoding]() 标签
  * 16.4 创建国际化的 Web 应用

    * 16.4.1 创建支持国际化的网页
    * 16.4.2 创建资源文件
  * 16.5 格式化标签

    * 16.5.1 [fmt:setTimeZone]() 标签
    * 16.5.2 [fmt:timeZone]() 标签
    * 16.5.3 [fmt:formatNumber]() 标签
    * 16.5.4 [fmt:parseNumber]() 标签
    * 16.5.5 [fmt:formatDate]() 标签
    * 16.5.6 [fmt:parseDate]() 标签
  * 16.6 小结
  * 16.7 思考题
* 第 17 章 JSTL SQL 标签库

  * 17.1 [sql:setDataSource]() 标签
  * 17.2 [sql:query]() 标签

    * 17.2.1 设置数据源
    * 17.2.2 设置 select 查询语句
    * 17.2.3 控制实际取出的记录
    * 17.2.4 访问查询结果
    * 17.2.5 使用 [sql:query]() 标签的范例
  * 17.3 [sql:param]() 标签
  * 17.4 [sql:dateParam]() 标签
  * 17.5 [sql:update]() 标签
  * 17.6 [sql:transaction]() 标签
  * 17.7 小结
  * 17.8 思考题
* 第 18 章 JSTL Functions标签库

  * 18.1 fh:contains函数
  * 18.2 fh:containsIgnoreCase函数
  * 18.3 fh:startsWith 函数
  * 18.4 fh:endsWith 函数
  * 18.5 fti:indexOf函数
  * 18.6 fh:replace函数
  * 18.7 fh:substring函数
  * 18.8 fo:substringBefore函数
  * 18.9 fh:substringAfter函数
  * 18.10 fo:split函数
  * 18.11 firjoin函数
  * 18.12 fiirtoLowerCase 函数
  * 18.13 fh:toUpperCase 函数
  * 18.14 fh:trim函数
  * 18.15 fii:escapeXml函数
  * 18.16 fh:length 函数
  * 18.17 小结
  * 18.18 思考题
* 第 19 章 简单标签和标签文件

  * 19.1 实现 SimpleTag 接口

    * 19.1.1 创建和使用 `<hello>`​​ 简单标签
    * 19.1.2 创建和使用带属性和标签主体的 `<welcome>`​​ 简单标签
  * 19.2 使用标签文件

    * 19.2.1 标签文件的隐含对象
    * 19.2.2 标签文件的指令
    * 19.2.3 标签文件的` <jsp:invoke> `​​  和 ` <jsp:doBody>`​​ 动作元素
    * 19.2.4 创建和使用带属性和标签主体的 `display`​​ 标签文件
    * 19.2.5 创建和使用带属性和标签主体的 `welcome`​​ 标签文件
    * 19.2.6 创建和使用带变量的 `precode`​​ 标签文件
  * 19.3 小结
  * 19.4 思考题
* 第 20 章 过滤器

  * 20.1 过滤器简介
  * 20.2 创建过滤器
  * 20.3 发布过滤器

    * 20.3.1 在 web.xml 文件中配置过滤器
    * 20.3.2 用 @WebFilter 注解来配置过滤器
    * 20.3.3 用 NoteFilter 来过滤 NoteServlet 的范例
  * 20.4 串联过滤器

    * 20.4.1 包装设计模式简介
    * 20.4.2 ServletOutputStream 的包装类
    * 20.4.3 HttpServletResponse 的包装类
    * 20.4.4 创建对响应结果进行字符串替换的过滤器
    * 20.4.5 ReplaceTextFilter 过滤器工作的 UML 时序图
    * 20.4.6 发布和运行包含 ReplaceTextFilter 过滤器的 Web 应用
  * 20.5 异步处理过滤器
  * 20.6 小结
  * 20.7 思考题
* 第 21 章 在 Web 应用中访问 EJB 组件

  * 21.1 JavaEE 体系结构简介
  * 21.2 安装和配置 WildFly 服务器
  * 21.3 创建 EJB 组件

    * 21.3.1 编写 Remote 接口
    * 21.3.2 编写 Enterprise Java Bean 类
  * 21.4 在 Web 应用中访问 EJB 组件
  * 21.5 发布 JavaEE 应用

    * 21.5.1 在 WildFly 上发布 EJB 组件
    * 21.5.2 在 WildFly 上发布 Web 应用
    * 21.5.3 在 WildFly 上发布 JavaEE 应用
  * 21.6 小结
  * 21.7 思考题
* 第 22 章 在 Web 应用中访问 SOAP 服务

  * 22.1 SOAP 简介
  * 22.2 在 Tomcat 上发布 AxisWeb 应用
  * 22.3 创建 SOAP 服务

    * 22.3.1 创建提供 SOAP 服务的 Java 类
    * 22.3.2 创建 SOAP 服务的发布描述文件
  * 22.4 发布和管理 SOAP 服务

    * 22.4.1 发布 SOAP 服务
    * 22.4.2 管理 SOAP 服务
  * 22.5 创建和运行 SOAP 客户端程序
  * 22.6 在 bookstore 应用中访问 SOAP 服务

    * 22.6.1 对 SOAP 服务方法的参数和返回值的限制
    * 22.6.2 创建 BookDB 服务类及 BookDBDelegate 代理类
    * 22.6.3 发布 BookDBService 服务和 bookstore 应用
  * 22.7 小结
  * 22.8 思考题
* 第 23 章 Web 应用的 MVC 设计模式

  * 23.1 MVC 设计模式简介
  * 23.2 JSP Model1 和 JSP Model2
  * 23.3 Spring MVC 概述

    * 23.3.1 Spring MVC 的框架结构
    * 23.3.2 Spring MVC 的工作流程
  * 23.4 创建采用 Spring MVC 的 Web 应用

    * 23.4.1 建立 Spring MVC 的环境
    * 23.4.2 创建视图
    * 23.4.3 创建模型
    * 23.4.4 创建 Controller 组件
    * 23.4.5 创建 web.xml 文件和 Spring MVC 配置文件
  * 23.5 运行 helloapp 应用
  * 23.6 小结
  * 23.7 思考题
* 第 24 章 Tomcat的管理平台

  * 24.1 访问 Tomcat 的管理平台
  * 24.2 Tomcat 的管理平台

    * 24.2.1 管理 Web 应用
    * 24.2.2 管理 HTTP 会话
    * 24.2.3 查看 Tomcat 服务器信息
  * 24.3 小结
* 第 25 章 安全域

  * 25.1 安全域概述
  * 25.2 为 Web 资源设置安全约束

    * 25.2.1 在 web.xml 中加入 <security-constraint> 元素
    * 25.2.2 在 web.xml 中加入 <login-config> 元素
    * 25.2.3 在 web.xml 中加入 <security-role> 元素
  * 25.3 内存域
  * 25.4 JDBC 域

    * 25.4.1 用户数据库的结构
    * 25.4.2 在 MySQL 中创建和配置用户数据库
    * 25.4.3 配置 <Realm> 元素
  * 25.5 在 Web 应用中访问用户信息
  * 25.6 小结
  * 25.7 思考题
* 第 26 章 Tomcat与其他HTTP服务器集成

  * 26.1 Tomcat与HTTP服务器集成的原理

    * 26.1.1 JK插件
    * 26.1.2 AJP协议
  * 26.2 在Windows下Tomcat与Apache服务器集成
  * 26.3 在Linux下Tomcat与Apache服务器集成

    * 26.3.1 安装和启动Apache服务器
    * 26.3.2 准备相关文件
    * 26.3.3 编辑注册表
    * 26.3.4 在IIS中加入"Jakarta"虚拟目录
    * 26.3.5 把JK插件作为ISAPI筛选器加入到IIS
    * 26.3.6 测试配置
  * 26.4 Tomcat集群

    * 26.4.1 配置集群系统的负载平衡器
    * 26.4.2 配置集群管理器
  * 26.5 小结
  * 26.6 思考题
* 第 27 章 在 Tomcat 中配置 SSI

  * 27.1 SSI 简介

    * 27.1.1 #echo 指令
    * 27.1.2 #include 指令
    * 27.1.3 #flastmod 指令
    * 27.1.4 #fsize 指令
    * 27.1.5 #exec 指令
    * 27.1.6 #config 指令
    * 27.1.7 `#if`​​、`#elif`​​、#endif 指令
  * 27.2 在 Tomcat 中配置对 SSI 的支持
  * 27.3 小结
  * 27.4 思考题
* 第 28 章 Tomcat阀

  * 28.1 Tomcat 阀简介
  * 28.2 客户访问日志阀
  * 28.3 远程地址过滤阀
  * 28.4 远程主机过滤阀
  * 28.5 错误报告阀
  * 28.6 小结
  * 28.7 思考题
* 第 29 章 在 Tomcat 中配置 SSL

  * 29.1 SSL 简介
  * 29.2 在 Tomcat 中使用 SSL

    * 29.2.1 准备安全证书
    * 29.2.2 配置 SSL 连接器
    * 29.2.3 访问支持 SSL 的 Web 站点
  * 29.3 小结
  * 29.4 思考题
* 第 30 章 用 ANT 工具管理 Web 应用

  * 30.1 安装配置 ANT
  * 30.2 创建 build.xml 文件
  * 30.3 运行 ANT
  * 30.4 小结
  * 30.5 思考题
* 第 31 章 使用 Log4J 进行日志操作

  * 31.1 Log4J 简介

    * 31.1.1 Logger 组件
    * 31.1.2 Appender 组件
    * 31.1.3 Layout 组件
    * 31.1.4 Logger 组件的继承性
  * 31.2 Log4J 的基本使用方法

    * 31.2.1 创建 Log4J 的配置文件
    * 31.2.2 在程序中使用 Log4J
  * 31.3 在 helloapp 应用中使用 Log4J
  * 31.4 小结
  * 31.5 思考题
* 第 32 章 Velocity模板语言

  * 32.1 获得与 Velocity 相关的类库
  * 32.2 Velocity 的简单例子

    * 32.2.1 创建 Velocity 模板
    * 32.2.2 创建扩展 VelocityViewServlet 的 Servlet 类
    * 32.2.3 发布和运行基于 Velocity 的 Web 应用
  * 32.3 注释
  * 32.4 引用

    * 32.4.1 变量引用
    * 32.4.2 属性引用
    * 32.4.3 方法引用
    * 32.4.4 正式引用符
    * 32.4.5 安静引用符
  * 32.5 指令

    * 32.5.1 #set 指令
    * 32.5.2 字面字符串
    * 32.5.3 #if 指令
    * 32.5.4 比较运算
    * 32.5.5 #foreach 循环指令
    * 32.5.6 #include 指令
    * 32.5.7 #parse 指令
    * 32.5.8 #macro 指令
    * 32.5.9 转义 VTL 指令
    * 32.5.10 VTL 的格式
  * 32.6 其他特征

    * 32.6.1 数学运算
    * 32.6.2 范围操作符
    * 32.6.3 字符串的连接
  * 32.7 小结
  * 32.8 思考题
* 第 33 章 创建嵌入式 Tomcat 服务器

  * 33.1 将 Tomcat 嵌入 Java 应用
  * 33.2 创建嵌入了 Tomcat 的 Java 示范程序
  * 33.3 终止嵌入式 Tomcat 服务器
  * 33.4 引用

    * 33.4.1 变量引用
    * 33.4.2 属性引用
    * 33.4.3 方法引用
    * 33.4.4 正式引用符
    * 33.4.5 安静引用符
  * 33.5 小结
  * 33.6 思考题
* 附录 A: server.xml 文件

  * A.1 配置 Server 元素
  * A.2 配置 Service 元素
  * A.3 配置 Engine 元素
  * A.4 配置 Host 元素
  * A.5 配置 Context 元素
  * A.6 配置 Connector 元素
  * A.7 配置 Executor 元素
* 附录 B: web.xml 文件

  * B.1 配置过滤器
  * B.2 配置 Servlet
  * B.3 配置 Servlet 映射
  * B.4 配置 Session
  * B.5 配置 Welcome 文件清单
  * B.6 配置 Tag Library
  * B.7 配置资源引用
  * B.8 配置安全约束
  * B.9 配置对安全验证角色的引用
  * B.10 配置对安全验证登录界面的引用
* 附录 C: XML 简介

  * C.1 SGML、HTML 与 XML 的比较
  * C.2 DTD 文档类型定义
  * C.3 有效 XML 文档以及简化格式的 XML 文档
  * C.4 URL、URN 和 URI
  * C.4.1 URL、URN 和 URI
  * C.4.2 XML 命名空间
* 附录 D: 书中涉及软件获取途径
