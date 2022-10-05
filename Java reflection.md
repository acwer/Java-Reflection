## web的基本概念

web是网页的意思,例如www.baidu.com(百度)。

web又分成两种

* ### 静态web

  1. 由html,css所构成的网页.

  2. 网页的数据始终不会发生变化.



* ### 动态web

1. 几乎所有的网站都是动态web,例如淘宝.
2. 动态web在不同的情况下,相关的数据会发生变化.
3. 技术栈:jsp,servlet等



**在java中，动态web开发技术统称为javaweb**


## web应用程序


* ### 基本概念

web应用程序是可以通过浏览器访问的程序，web应用程序例如index.html,index2.html.....,外界可以通过浏览器来对这些web应用程序进行访问。

* ### 构成

一个web应用程序由

1. java程序
2. html css js
3. jsp,servlet
4. 相关的配置文件(properties)
5. jar包

所构成.



**web应用程序编写完成后,需要有一个服务器来进行同一管理。**


### 静态web


* 由html，css所构成的web页面叫做静态web。

* 静态web的通信过程

客户端通过网络(network)来对服务器中的web服务(web service)进行第一次请求(Request)，然后web服务根据相应的请求去找相应的网页，找到了相应的网页然后再响应到web服务，然后继续响应(Response)到客户端

![-1.png](https://img-blog.csdnimg.cn/img_convert/eee10952b3bb5bf2196b68b6147dcabc.png) 



* 在f12的网络中可以找到请求和响应的相关文件和信息

![-2.png](https://img-blog.csdnimg.cn/img_convert/40eace3938522de0cdd28c8d67ee7733.png) 



* 静态web存在的缺点

1. web页面无法动态更新，所有用户看到的都是同一页面。
2. 无法与数据库进行交互。


### 动态web


* 在不同的情况下会发生相应的变化的网页叫做动态web。

* 动态web的通信过程。

![在这里插入图片描述](https://img-blog.csdnimg.cn/cefc47042ff64d4ea4011dd69ab7d046.png)

* 动态web的缺点

假如服务器的动态资源出现了问题，我们需要更改后台的程序，然后重新发布。


## web服务器


* 基本概念：

web服务器是一种被动操作，处理用户给服务器的一些请求和给用户一些响应信息。



* 常用的web服务器

tomcat

## tomcat

* 安装tomcat

* 启动和关闭tomcat

![在这里插入图片描述](https://img-blog.csdnimg.cn/51bbd671729144519e50f5186cbb944f.png)

访问测试:localhost:8080，不过我把8080端口改成了80端口。

* 各个文件夹的作用

![在这里插入图片描述](https://img-blog.csdnimg.cn/6c8cdb4e921d4a89a993972a4d86bb6e.png)



* 端口的修改

![在这里插入图片描述](https://img-blog.csdnimg.cn/7e869e7e640940ba8980e3a36585efd9.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/1c31959e541c4429bf585d5b0fec7ba3.png)

* 解决乱码问题

1. 进入logging.xml文件

   ![在这里插入图片描述](https://img-blog.csdnimg.cn/c43c5c12147342108f8b3ba99be94c1b.png)

2. 把utf-8修改成gbk

![在这里插入图片描述](https://img-blog.csdnimg.cn/d48c6409644545f98aa9f7ad76d244a1.png)

### 发布一个web网站

将自己写的网站放入到webapps文件中，就可以访问了。

```java
--webapps：tomcat服务器的web目录
	-ROOT：根用户的网站目录
	-kuangstudy: 自定义的网站目录
		-WEB-INF：
			-classes：存放Java程序
			-lib：web应用所依赖的jar包
			-web.xml：网站的配置文件
		-index.html:默认的首页
		-css
		-js
		-img
```





## Socket

### TCP和UDP

#### 1.TCP:

​		当一台计算机和另一台计算机相互通讯时，在这种协议下，两台计算机之间会比较畅通

和可靠(因为在连接之前会进行三次握手，在断开的时候会四次挥手)。

#### 2.UDP:

​		它是一种无连接的协议，数据想发就发，所以不会建立可靠的数据传输，也就是说数据

在传输的过程中可能会导致部分数据丢失，但是比TCP更加高效，适合视频直播类的。



#### 补充：

​		我们的电脑上可能存在大量的应用程序，我们可以通过ip地址来访问目标计算机，然后

通过端口号来访问特定的应用程序。

![6.png](https://cdn.acwing.com/media/article/image/2022/08/01/136759_577c675a11-6.png) 



### Socket技术

​		通过Socket技术，我们可以实现两台计算机之间的通信，Socket也被翻译成套接字，它

支持TCP和UDP协议。java对于socket技术有一套完整的封装，我们可以通过Java来实现一

套完整的通信，

​		要实现Socket通信，我们必须创建一个数据发送者和一个数据接收者，也就是客户端和

服务器端，我们需要提前开启服务器端，等待客户端对服务器端的连接。

下面展示的是**数据从客户端到服务端的过程**,通过一串代码来进行演示。

#### 相关代码：

```java
/*server类*/
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;

public class Server {
    public static void main(String[] args){
        try {
            ServerSocket server = new ServerSocket(1000);

            System.out.println("---正在等待客户端的连接---");
            //阻塞该线程，等待客户端的连接，当客户端对服务端发起连接时，此时服务端会恢复成正常状态。
            /*  返回的对象是客户端的连接。即为socket，socket中存储了有关于客户端的一些数据
                所以，我们可以通过某些方法来得到客户端的ip地址。
            */
            Socket socket = server.accept();

            //获取主机，也就是服务端的地址。
            System.out.println("连接已建立，客户端端的ip地址为"+socket.getInetAddress().getHostAddress());

            //读取从客户端发来的数据
            System.out.println("读取客户端的数据:");
            BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(socket.getInputStream()));

            //输出读取的数据
            String str = null;
            while((str = bufferedReader.readLine())!=null){
                System.out.println(str);
            }

            socket.close();
            //关闭服务端
            server.close();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}


```

```java
/*client类*/
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.OutputStreamWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class Client {

    public static void main(String[] args){
        try {
            //此处的localhost是服务器端的地址。1000是服务器端的端口号。
            Socket socket = new Socket("localhost",1000);

            System.out.println("已经连接到服务器端!");

            //写入数据
            BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
            bufferedWriter.write("我叫刘德华\n");
            bufferedWriter.write("我是非常的帅气");


            //将字符流中的数据进行刷新
            bufferedWriter.flush();
            System.out.println("数据已经发送！");
            //关闭客户端
            socket.close();
        } catch (IOException e) {
            System.out.println("服务器连接失败!");
            throw new RuntimeException(e);
        }

    }
}

```



## maven的基础知识

### maven环境搭建和配置

* 为什么要学习这个技术？

  	在Javaweb中，有大量的jar包需要我们去导入，如果一个一个导入，则会浪费大量的时间。由此，maven诞生了。maven项目管理工具的目的就是为了方便导入和配置jar包。

* maven会规定你该如何地去编写Java代码，必须按照这个规范来。

* 下载解压maven压缩文件

* 配置环境变量

  在系统变量中新建和配置path和MAVEN_HOME。

  * MAVEN_HOME            D:\apache-maven-3.6.1-bin\apache-maven-3.6.1
  * path                 %MAVEN_HOME%\bin

* 通过mvn -version命令来判断maven是否安装成功。

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/204f25db7ab54c8397a069f4a0a6f3b5.png)

* 配置文件

  * 通过阿里云镜像(mirror)来加快下载和导入jar包。

  * 国内尽量使用阿里云镜像

  * 将下面这段代码导入到/conf/setting.xml文件中。

    ```xml
    <mirror>
        <id>aliyunmaven</id>
        <mirrorOf>*</mirrorOf>
        <name>阿里云公共仓库</name>
        <url>https://maven.aliyun.com/repository/public</url>
    </mirror>
    ```

    

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/cf6cea3a39404a1fb61a0e328ea39994.png)

* 建立一个本地仓库(localRepository)

  * 进入cof/setting.xml

  * 在localRepository标签中加入一串代码

    ```xml
    <localRepository>D:\apache-maven-3.6.1-bin\apache-maven-3.6.1\mvn_resp</localRepository>
    ```

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/9495f96f96cc48eba4ccabbd0ac013d3.png)

### maven在idea中的使用

1. 启动idea

2. 创建一个maven项目

   ![在这里插入图片描述](https://img-blog.csdnimg.cn/7f60f7d6841d4f418abb27124913b2f4.png) 

![在这里插入图片描述](https://img-blog.csdnimg.cn/5de21f80cae24acca38ab1ae9aea8f35.png) 

3. 等待项目初始化完毕

4. 观察maven项目中多了什么东西？

多了一些maven项目需要的jar包。

5. 记住Maven的文件地址、配置文件、本地仓库需要配置正确，这样才不会爆红。

6. 创建一个普通的maven项目

![11.png](https://cdn.acwing.com/media/article/image/2022/08/30/136759_f078e3d028-11.png) 
然后将maven的文件地址、配置文件、本地仓库配置正确。

7. 普通的maven项目的目录

![22.png](https://cdn.acwing.com/media/article/image/2022/08/30/136759_1e5fc93a28-22.png) 

8. 模板创建maven项目的目录

![44.png](https://cdn.acwing.com/media/article/image/2022/08/30/136759_3fad347728-44.png) 



9. maven项目的标记文件夹功能

创建java和resources文件夹，可能不能创建class文件。则我们需要对文件夹进行标记。

![21.png](https://cdn.acwing.com/media/article/image/2022/08/30/136759_57916ca528-21.png) 



10. 在idea中配置tomcat

![33.png](https://cdn.acwing.com/media/article/image/2022/08/30/136759_730c37f928-33.png) 

![44.png](https://cdn.acwing.com/media/article/image/2022/08/30/136759_886bc2f128-44.png) 

11. 解决警告问题

![77.png](https://cdn.acwing.com/media/article/image/2022/08/30/136759_9ae88fe728-77.png) 

12. 运行该项目

![66.png](https://cdn.acwing.com/media/article/image/2022/08/30/136759_b5dde08028-66.png) 

13. war包

一般Javaweb项目会打包成war包。

14. maven的生命周期

启动compile，首先不是从中央仓库中寻找jar包，而是先到阿里云镜像仓库中寻找jar包，把这些jar包放入到本地仓库中。

compile：编译

clean：清理target

package：打成一个jar包

test：执行test目录项目

install：将该项目打成jar包放到本地仓库内。






## helloServlet

1. 什么是servlet？

   * servlet是sun公司开发动态web的一项技术。
   * sun公司在这些api中提供了一个接口，即为servlet。

2. 开发servlet的两个小步骤

   * 编写一个类实现servlet接口。
   * 把开发好的类部署到web服务器中。

   我们把实现了servlet接口的Java程序叫做servlet。

3. helloServlet

   servlet接口有两个默认的实现类，一个是HttpServlet

   1. 创建一个maven项目

   2. 导入servlet相关依赖

   ```
   <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>javax.servlet-api</artifactId>
         <version>3.1.0</version>
       </dependency>
       <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->
       <dependency>
         <groupId>javax.servlet.jsp</groupId>
         <artifactId>javax.servlet.jsp-api</artifactId>
         <version>2.3.3</version>
       </dependency>
   ```

   3. 编写一个servlet程序

      * 编写一个普通类

      * 实现HttpServlet接口，也就是直接继承HttpServlet接口

      * 编写servlet注解

      在类中加上@WebServlet(“/Demo01”)注解，意思就是通过Demo01可以访问servlet这个页面。

      * 配置tomcat服务器

![在这里插入图片描述](https://img-blog.csdnimg.cn/1732c8dc1fbe4fd8b8d519f73f33fc3e.png)

将该项目打包成一个war包，通过war包可以访问该项目。

![在这里插入图片描述](https://img-blog.csdnimg.cn/0484e3bd6a444f0f8833f280f82718b3.png)

				6. 运行程序

![在这里插入图片描述](https://img-blog.csdnimg.cn/f602cc2cb8e94cb6b1e61473e3ae1059.png)





## 开发servlet有三种方法

1. 实现servlet接口
2. 继承GenericServlet
3. 继承HttpServlet

### servlet接口的方式来开发。

```java
import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/demo01")
public class HelloServlet implements Servlet {

    //该函数用于初始化Servlet
    //类似于类的构造函数，该函数只会调用一次。
    //当用户第一次访问servlet时，该函数会被调用。
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("init it...");
    }

    //得到servlet的配置文件，这个地方不是重点。
    public ServletConfig getServletConfig() {
        return null;
    }

    //这个函数用户处理业务逻辑
    //程序员应当把业务逻辑写到这里
    //当用户每次访问servlet时，servlet每次都会调用service()方法
    //req  用户获取客户端(浏览器)的信息
    //res  用于向客户端(浏览器)返回信息
    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        System.out.println("service it");

        //从res中获得writer
        PrintWriter printWriter = res.getWriter();

        printWriter.println("hello world!");
    }

    //该函数没有什么意义
    public String getServletInfo() {
        return "";
    }

    //销毁servlet实例(释放内存)
    //当关闭tomcat服务器时，则会调用该方法。
    public void destroy() {
        System.out.println("destroy it...");
    }
}
```



### 使用GenericServlet实现类的方式来开发

通过GenericServlet去开发servlet，只需要重写service()方法，相对于servlet接口开发

比较简单一点。

```java
import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/demo01")
public class HelloServlet extends GenericServlet {

    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        PrintWriter printWriter = res.getWriter();
        printWriter.println("hello world generic!");
    }
}
```

### 使用HttpServlet实现类的方式来开发

通过HttpServlet实现类来开发servlet，我们需要重写doGet()方法和doPost()方法,这是目前使用最多的

一种方法。

首先我们需要对表单form的get方法和post方法进行一些了解，表单form的get和post方法和http的get和post方法

是类似的。



* form的get和post方法

  ```
  <!DOCTYPE html>
  <html>
  <head>
      <title></title>
  </head>
  <body>
  <center>
      <form method="post">
          <p>name:<input type="text" name="name"/></p>
          <p>password:<input type="password" name="pwd"/></p>
          <input type="submit" value="submit">
  
      </form>
  </center>
  </body>
  </html>
  
  ```

  ​	

**form的get和post方法的结论**

1. 从安全性看，get<post，get提交的数据会在地址栏中显示，而post提交的数据在地址栏中不会显示。

2. 从提交的内容看，get<post，get提交的数据不能大于2k，而post提交的数据大小建议不要大于64k。

3. 从请求响应速度看，get>post,get要求服务器立刻处理请求或响应请求，而post请求可能会形成一个队列

   的请求，响应可能会形成一个队列的相应。

```
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/Demo01")
public class HelloServlet extends HttpServlet {

    //处理get请求
    //req:用于获得客户端提交的信息
    //res:用于获得向客户端返回的信息
    //req对象实例和res对象实例是由tomcat容器来创建的。
    public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        //业务逻辑
        PrintWriter printWriter = res.getWriter();
        printWriter.println("hello httpServlet!");

    }

    //处理post请求
    //req:用于获得客户端提交的信息
    //res:用于获得向客户端返回的信息

    //很多情况下，我们都是将doGet和doPost方法合二为一
    //即在doPost方法下运用doGet方法
    public void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        this.doGet(req,res);
    }
}
```



## servlet的生命周期

servlet部署在容器里(tomcat)，他的生命周期由容器(tomcat)来管理

servlet生命周期一般分成几个阶段:

1. tomcat容器创建servlet实例。

2. 实例调用servlet的init方法，该方法只会在第一次访问servlet时

   被调用一次。

3. 实例调用servlet的service方法，一般逻辑代码在这里处理，在

   servlet实例时，会被调用一次。

4. 当关闭tomcat服务器时，servlet实例会调用一次destroy方法。

   

## 简单用户登录网站实例

* 框架图

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/1cc6e1c86b3644cbb168480449d1b44b.png)





* 登录界面Login.java

  ```
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  import java.io.PrintWriter;
  
  @WebServlet("/Login")
  public class Login extends HttpServlet {
  
      public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
          //将页面内容改成gbk编码模式，防止中文乱码。
          res.setContentType("text/html;charset=gbk");
  
          PrintWriter printWriter = res.getWriter();
          /*
          	form标签中 action=t 
          	意思是当登录成功时，会自动跳转到t页面中，这里的t是LoginCheck,
          	LoginCheck是验证用户界面的注解名。
          
          */
          printWriter.println("<!DOCTYPE html>\n" +
                  "<html>\n" +
                  "<head>\n" +
                  "    <title></title>\n" +
                  "</head>\n" +
                  "<body>\n" +
                  "<center>\n" +
                  "    <form action=LoginCheck method=\"post\">\n" +
                  "        <p>name:<input type=\"text\" name=\"name\"/></p>\n" +
                  "        <p>password:<input type=\"password\" name=\"pwd\"/></p>\n" +
                  "        <input type=\"submit\" value=\"submit\">\n" +
                  "\n" +
                  "    </form>\n" +
                  "</center>\n" +
                  "</body>\n" +
                  "</html>");
      }
      public void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
          this.doGet(req,res);
      }
  }
  
  ```



* 验证用户界面LoginCheck.java

```
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

//用户验证Servlet
@WebServlet("/LoginCheck")
public class LoginCheck extends HttpServlet {
    public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {

        //接收登录界面的账号和密码
        //参数是Login.java的对应input的对应name
        String username = req.getParameter("name");
        String password = req.getParameter("pwd");

        //验证用户名和密码是否正确
        if(username.equals("root")==true&&password.equals("123456")==true){
            //合法，跳转到欢迎界面
            res.sendRedirect("Welcome");
        }
        else{
            //不合法，跳转到登录界面
            //参数是写上登录界面的url地址,url地址也就是注解名。
            res.sendRedirect("Login");
            System.out.println("请重新登录！");
        }
    }
    public void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        this.doGet(req,res);
    }
}
```



* 欢迎界面Welcome.java

```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/Welcome")
public class WelCome extends HttpServlet {
    public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        //将页面内容改成gbk编码模式，防止中文乱码。
        res.setContentType("text/html;charset=gbk");
        PrintWriter printWriter = res.getWriter();
        printWriter.println("欢迎你，登录成功!");
    }
    public void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        this.doGet(req,res);
    }
}
```

**res.sendRedirect("Welcome")实现重定向(页面跳转)**

![在这里插入图片描述](https://img-blog.csdnimg.cn/19b261149707401d9f952cdb7a57876b.png)

		在图 1 中，当客户端访问 Servlet1 时，由于在 Servlet1 中调用了 sendRedirect() 方法将请求重定向到 Servlet2，因此，浏览器收到 Servlet1 的响应消息后，立刻向 Servlet2 发送请求，Servlet2 对请求处理完毕后，再将响应消息回送给客户端浏览器并显示。





## request和response的作用执行流程

1. 浏览器发送请求

2. 服务器接收请求,创建两个对象(request和response),将请求的信息封装request对象

3. 找到对应的servlet,将这两个对象传递给servlet

4. Servlet收到请求,执行service方法,处理自己的业务逻辑,生成动态的内容,将内容返回给服务器

5. 服务器收到内容之后,进行拆分,生成响应信息,返回给浏览器



## 同一用户不同页面共享数据

1. cookie技术
2. sendRedirect()重定向

3. 隐藏表单
4. session技术





## Cookie

### 什么是Cookie?

1. 服务器在用户端保存用户的信息，比如用户名，密码等，这些信息就是cookie。

2. 通过下面这张图片可以更好地理解，计算机A向服务器发出一个请求，请求中包含

用户的登录信息，服务器又将这些登录信息回写到计算机A的某个文件目录中，等

到服务器需要这些登录信息的时候，服务器会从计算机A中提取这些登录信息。这

些登录信息也就做cookie。

![在这里插入图片描述](https://img-blog.csdnimg.cn/edb2e22492c34ed4bebd56e4827fab14.png)



### Cookie可以用来做什么?

1. 保存用户名，密码，在一定时间内不用重新登录。
2. 记录用户访问网站的喜好，比如有无背景音乐、网站的背景颜色是什么?
3. 网站的个性化，比如定制网站的服务，内容。



### Cookie的使用

1. Cookie有点像一张表，分成两列，一列是名字，一列是值，数据类型都是String。

2. 如何创建一个Cookie?(在服务端创建的)

   Cookie cookie = new Cookie(String name,String val);

3. 如何将Cookie添加到一个客户端?

   	response.add(cookie);

4. 如何读取Cookie?(从客户端读取服务端的Cookie)

   request.getCookies();



### Cookie的其他说明

![在这里插入图片描述](https://img-blog.csdnimg.cn/cd97280b06764c6284bacf0b0a8775e3.png)



### Cookie实例

往Cookie中放入属性，又往Cookie取出属性。

* CookieTest.java

```java
import com.sun.net.httpserver.HttpServer;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

//创建一个Cookie，往Cookie中存入属性。
@WebServlet("/Demo01")
public class CookieTest extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html;charset=gbk");

        PrintWriter printWriter = resp.getWriter();
        //当用户访问servlet时，就将信息放到该用户的Cookie中

        //服务器端创建一个Cookie
        Cookie cookie = new Cookie("color","red");

        //设置Cookie的生存时间
        cookie.setMaxAge(10);
        //如果不设计Cookie的生存时间,当程序运行完，cookie就准备消失。

        //将cookie回写到客户端
        resp.addCookie(cookie);

        printWriter.write("已经创建了一个cookie");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```



* CookieTest2.java

  ```
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.Cookie;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  import java.io.PrintWriter;
  
  //往Cookie中取出相关的属性。
  @WebServlet("/Demo02")
  public class CookieTest2 extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          resp.setContentType("text/html;charset=gbk");
          PrintWriter printWriter = resp.getWriter();
          Cookie a[] = req.getCookies();
          if(a.length==0){
             printWriter.println("不存在Cookie");
          }
         else{
             int flag=0;
             for(int i=0;i<a.length;i++){
                 Cookie temp = a[i];
                 if(temp.getName().equals("color")){
                     String value = temp.getValue();
                     printWriter.println("color:"+value);
                     flag=1;
                     break;
                 }
             }
             if(flag==0){
                 printWriter.println("有cookie，但是没含有name为color的cookie");
             }
         }
      }
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          this.doGet(req,resp);
      }
  }
  ```

* CookieTest3.java

  ```java
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.Cookie;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  import java.io.PrintWriter;
  //删除Cookie
  @WebServlet("/Demo3")
  public class CookieTest3 extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          //解决中文乱码
          resp.setContentType("text/html;charset=gbk");
          PrintWriter printWriter = resp.getWriter();
          //获取客户端的cookies
          Cookie a[] = req.getCookies();
  
          if(a.length==0){
              System.out.println("没有cookie，不用删除。");
          }
          else{
              int flag=0;
              for(int i=0;i<a.length;i++){
                  Cookie temp = a[i];
                  if(temp.getName().equals("color")){
                      temp.setMaxAge(0);
                      printWriter.println("删除含有name为color的cookie");
                      flag=1;
                      break;
                  }
              }
              if(flag==0){
                  printWriter.println("存在cookie，但是没有含有name为color的cookie");
              }
          }
      }
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          this.doGet(req,resp);
      }
  }
  ```

  



## sendRedirect()

		通过sendRedirect()方法，我们可以将一个页面的信息传递到另一个页面中，比如可以将用户验证页面的用户名和密码信息传递到Welcome页面中。
	
		比如可以在Welcome页面中显示用户的用户名和密码。

![在这里插入图片描述](https://img-blog.csdnimg.cn/598dd5635bda478d83f63a3cfd051e2f.png)



### 应用实例

在welcome页面中显示用户的用户名和密码

* 登录页面Login.java

```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/demo01")
public class Login extends HttpServlet {

    public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        //将页面内容改成gbk编码模式，防止中文乱码。
        res.setContentType("text/html;charset=gbk");

        PrintWriter printWriter = res.getWriter();
        printWriter.println("<!DOCTYPE html>\n" +
                "<html>\n" +
                "<head>\n" +
                "    <title></title>\n" +
                "</head>\n" +
                "<body>\n" +
                "<center>\n" +
                "    <form action=demo02 method=\"post\">\n" +
                "        <p>name:<input type=\"text\" name=\"name\"/></p>\n" +
                "        <p>password:<input type=\"password\" name=\"pwd\"/></p>\n" +
                "        <input type=\"submit\" value=\"submit\">\n" +
                "\n" +
                "    </form>\n" +
                "</center>\n" +
                "</body>\n" +
                "</html>");
    }
    public void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        this.doGet(req,res);
    }
}
```

* 用户验证页面LoginCheck.java

```
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

//用户验证Servlet
@WebServlet("/demo02")
public class LoginCheck extends HttpServlet {
    public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {

        //接收登录界面的账号和密码
        //参数是Login.java的对应input的对应name
        String username = req.getParameter("name");
        String password = req.getParameter("pwd");

        //验证用户名和密码是否正确
        if(username.equals("root") &&password.equals("123456")==true){
            //合法，跳转到欢迎界面
            res.sendRedirect("demo03?username="+username+"&password="+password);
        }
        else{
            //不合法，跳转到登录界面
            //参数是写上登录界面的url地址
            res.sendRedirect("Login");
            System.out.println("请重新登录！");
        }
    }
    public void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        this.doGet(req,res);
    }
}
```

* 欢迎页面Welcome.java

```
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/demo03")
public class WelCome extends HttpServlet {
    public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        //将页面内容改成gbk编码模式，防止中文乱码。
        res.setContentType("text/html;charset=gbk");
        //得到从注解名为LoginCheck的servlet的username传递过来的username
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        PrintWriter printWriter = res.getWriter();
        printWriter.println("欢迎你，登录成功!"+" "+username+" "+password);

    }
    public void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        this.doGet(req,res);
    }
}
```



## session

### 什么是session

		当用户打开浏览器，访问某个网站时，服务器就会在服务器内存中，为该浏览器分配一个内存空间，该空间会被该浏览器所独占。这块内存空间就叫做session。该空间数据默认存在时间是30min，你也可以想修改。

### session可以用来做什么?

1. 网上商城的购物车。
2. 保存登录用户信息。
3. 将数据放到session中，供同一用户的各个页面使用。

4. 防止用户非法跳转到某个页面。

### 如何理解session

可以将session看作成一张表，表里面存放了一些属性名和对应的值。

![在这里插入图片描述](https://img-blog.csdnimg.cn/8b2765ba413d43b38512a15641abf8af.png)



### 如何使用session

![在这里插入图片描述](https://img-blog.csdnimg.cn/8f9369e47ce144608f98f6f0b5d6268d.png)



### session的注意事项

1. session的生存时间默认是30min，如果想要对生存时间进行修改，可以对web.xml文件进行修改。

```xml
<session-config>
    <session-timeout>40</session-timeout>
</session-config>
```



2. 上面所说的30min不是指的是累计时间，而指的是用户的发呆时间。

3. 当客户端访问网站的时候，服务器会给浏览器分配唯一的session id，通过id来区分不同的浏览器，

   即为客户端。

4. 因为session中的属性名和值要占用服务器的内存，因此软件公司在迫不得已地情况下才会使用session。



### session应用实例

1. 实现保存登录用户的信息。

2. 防止用户非法进入到某个页面。

* 登录页面Login.java

```
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/Login")
public class Login extends HttpServlet {

    public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        res.setContentType("text/html;charset=gbk");
        PrintWriter printWriter = res.getWriter();
        printWriter.println("<!DOCTYPE html>\n" +
                "<html>\n" +
                "<head>\n" +
                "    <title></title>\n" +
                "</head>\n" +
                "<body>\n" +
                "<center>\n" +
                "    <form action=LoginCheck method=\"post\">\n" +
                "        <p>name:<input type=\"text\" name=\"name\"/></p>\n" +
                "        <p>password:<input type=\"password\" name=\"pwd\"/></p>\n" +
                "        <input type=\"submit\" value=\"submit\">\n" +
                "\n" +
                "    </form>\n" +
                "</center>\n" +
                "</body>\n" +
                "</html>");
    }
    public void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        this.doGet(req,res);
    }
}
```

* 用户验证页面LoginCheck.java

```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
import java.io.PrintWriter;

//用户验证Servlet
@WebServlet("/LoginCheck")
public class LoginCheck extends HttpServlet {
    public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        String username = req.getParameter("name");
        String password = req.getParameter("pwd");
        if(password.equals("123456")==true){
            //获取session，并将pass ok存入到session中。
            HttpSession session = req.getSession(true);
            session.setAttribute("pass","ok");
            res.sendRedirect("Welcome?username="+username+"&password="+password);
        }
        else{
            res.sendRedirect("Login");
            System.out.println("请重新登录！");
        }
    }
    public void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        this.doGet(req,res);
    }
}
```

* 欢迎页面(Welcome.java)

```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/Welcome")
public class WelCome extends HttpServlet {
    public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        //将页面内容改成gbk编码模式，防止中文乱码。
        res.setContentType("text/html;charset=gbk");
        //获取session,并从session中获取pass属性对应的值。
        HttpSession session = req.getSession(true);
        String s = (String) session.getAttribute("pass");
        //获取没有获取到值，则跳转到登录页面，如果获取了值，则输出这个值是什么。
        if(s==null){
            res.sendRedirect("Login");
        }
        else{
            System.out.println(s);
        }
        //得到从注解名为LoginCheck的servlet的username传递过来的username
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        PrintWriter printWriter = res.getWriter();
        printWriter.println("欢迎你，登录成功!"+" "+username+" "+password);
    }
    public void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        this.doGet(req,res);
    }
}
```



## servlet中操作数据库

		在登录页面中输入用户的用户名和密码来进行用户验证，如果输入的用户名和密码匹配数据库中的某一条数据，则进入欢迎页面。

### 架构图

![在这里插入图片描述](https://img-blog.csdnimg.cn/5d9eb8c98f204079b75089ea56b14490.png)

* mysql依赖包

  ```java
  <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.30</version>
  </dependency>
  ```

* 用户登录页面Login.java

```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/Login")
public class Login extends HttpServlet {

    public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        res.setContentType("text/html;charset=gbk");
        PrintWriter printWriter = res.getWriter();
        printWriter.println("<!DOCTYPE html>\n" +
                "<html>\n" +
                "<head>\n" +
                "    <title></title>\n" +
                "</head>\n" +
                "<body>\n" +
                "<center>\n" +
                "    <form action=LoginCheck method=\"post\">\n" +
                "        <p>name:<input type=\"text\" name=\"name\"/></p>\n" +
                "        <p>password:<input type=\"password\" name=\"pwd\"/></p>\n" +
                "        <input type=\"submit\" value=\"submit\">\n" +
                "\n" +
                "    </form>\n" +
                "</center>\n" +
                "</body>\n" +
                "</html>");
    }
    public void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        this.doGet(req,res);
    }
}
```



* 用户验证页面LoginCheck.java

```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
import java.io.PrintWriter;
import java.sql.*;

//用户验证Servlet
@WebServlet("/LoginCheck")
public class LoginCheck extends HttpServlet{
    ResultSet resultSet = null;
    Statement statement = null;
    Connection connection = null;
    public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        String username = req.getParameter("name");
        String password = req.getParameter("pwd");

        //连接数据库
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");

            //得到连接
            connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/company", "root", "123456");

            //创建statement
            statement = connection.createStatement();

            String sql = "select *from student where username=username and password=password";
            resultSet = statement.executeQuery(sql);
            if(resultSet.next()){
                String username1 = resultSet.getString("username");

                if(username1==username){
                    //表示真的合法
                    HttpSession session = req.getSession(true);
                    session.setAttribute("pass", "ok");
                    res.sendRedirect("Welcome?username=" + username + "&password=" + password);
                }
                else{
                    res.sendRedirect("Login");
                    System.out.println("请重新登录！");
                }

            }
            else {
                res.sendRedirect("Login");
                System.out.println("请重新登录！");
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
    public void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        this.doGet(req,res);
    }
}
```



* 欢迎页面Welcome.java

```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/Welcome")
public class WelCome extends HttpServlet {
    public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        //将页面内容改成gbk编码模式，防止中文乱码。
        res.setContentType("text/html;charset=gbk");
        HttpSession session = req.getSession(true);
        String s = (String) session.getAttribute("pass");
        if(s==null){
            res.sendRedirect("Login");
            System.out.println("返回到登录页面");
        }
        else{
            System.out.println(s);
        }
        //得到从注解名为LoginCheck的servlet的username传递过来的username
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        PrintWriter printWriter = res.getWriter();
        printWriter.println("欢迎你，登录成功!"+" "+username+" "+password);
    }
    public void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        this.doGet(req,res);
    }
}
```



## 在servlet中显示图片

* 在webapp文件夹目录下创建一个img目录，把图片放到这个位置。

* 显示图片的写法

```java
printWriter.write("<img src=\"img/ldh.png\"><br>");
```



## ServletContext详解

### 什么是ServletContext？

要理解ServletContext,就必须和cookie、session做一个对比。

你可以把ServletContext想象成一个共同的空间、可以被所有的用户所访问。也就是说，A用户可以访问D，B用户可以访问D，C用户可以访问D。

![在这里插入图片描述](https://img-blog.csdnimg.cn/1b1bfb92bd2c4d3da70e7480302053ab.png)



### 怎么使用ServletContext?

1. 怎么得到ServletContext实例?

   this.getServletContext()

2. 可以把ServletContext想象成一张表，这张表和session的表非常相似。

![在这里插入图片描述](https://img-blog.csdnimg.cn/4ceffb2a5cc843908bd7d4e134a598fb.png)



3. 添加属性

   setAttribute(String name,Object value);

4. 得到属性的值

   getAttribute(String name);

5. 删除属性

   removeAttribute(String name);

6. 生命周期

   从创建ServletConext对象开始，到关闭服务器结束。



### 注意事项

存入servletContext的数据会长时间的保存在服务器，会占用内存，因此我们不要往servletContext中添加过大的数据。



### servletContext实例

给servletContext实例中添加一个属性，然后又从里面取出属性。

* ServletContextTest类

```java
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

//给ServletContext添加属性
@WebServlet("/servlet01")
public class ServletContextTest extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html;charset=gbk");
        PrintWriter printWriter = resp.getWriter();

        ServletContext servletContext = this.getServletContext();

        //添加属性
        servletContext.setAttribute("name","chris");
        printWriter.println("给servletContext添加一个属性，属性名是name,属性值为chris");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req,resp);
    }
}
```



* ServletContextTest2类

```java
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/servlet02")
//得到servletContext中的值
public class ServletContextTest2 extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html;charset=gbk");
        PrintWriter printWriter = resp.getWriter();
        ServletContext servletContext = this.getServletContext();

        //得到属性和它对应的值
        String name = (String) servletContext.getAttribute("name");
        printWriter.println("从servletContext中得到属性名为name,值为"+name);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req,resp);
    }
}
```



## 访问路径设置

* Servlet需要被访问，必须要设置访问路径(urlpattern)。

  一个Servlet，可以设置多个访问路径。

  ```java
  @WebServlet(urlPatterns = {"/demo01","/demo02"})
  ```

  **相关代码**

  ```java
  @WebServlet(urlPatterns = {"/demo01","/demo02"})
  ```

  

* urlpattern的配置规则

1. **精确查找**

```java
//完整的显示路径
@WebServlet(urlPatterns = {"/user/Demo01"})

```



2. **目录匹配**

```java
@WebServlet(urlPatterns = {"/user/*"})
```



## 综合实例

对上面的cookie,session,一些知识点的总结，通过一个简易的登录系统来实现。

* index.html(登录页面)

  ```java
  <!DOCTYPE html>
  <html>
  <head>
    <title></title>
    <meta charset="UTF-8">
  </head>
  <body>
  <center>
    <h1>登录界面</h1>
    <form action="LoginCheck" method="post">
      <p>name:<input type="text" name="name"/></p>
      <p>password:<input type="password" name="pwd"/></p>
      <p><input type="checkbox" name="keep" value="2">两周内不再重新登录</p>
      <input type="submit" value="提交">
    </form>
  </center>
  </body>
  </html>
  ```

* Conn.java(连接类)

```
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Conn{

    public static Connection getConnection () throws ClassNotFoundException, SQLException {
        Class.forName("com.mysql.cj.jdbc.Driver");
        Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/company", "root", "123456");
        return connection;
    }
}
```

* StudentBeanCheck.java(连接数据库，执行sql语句的类)

  ```java
  import java.sql.Connection;
  import java.sql.ResultSet;
  import java.sql.SQLException;
  import java.sql.Statement;
  import java.util.List;
  
  //这是一个处理Student表的类
  public class StudentBeanCheck {
  
      public Connection connection=null;
      public Statement statement=null;
      public ResultSet resultSet=null;
  
      //连接数据库，并且执行sql语句
      public ResultSet check(String username,String password) throws SQLException, ClassNotFoundException {
          //得到连接
          connection = Conn.getConnection();
          statement = connection.createStatement();
          String sql = "select *from student where username=username and password=password";
          resultSet = statement.executeQuery(sql);
          return resultSet;
      }
  }
  ```

  

* LoginCheck.java(用户验证页面)

```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.*;
import java.io.IOException;
import java.io.PrintWriter;
import java.sql.*;
import java.util.List;

//用户验证Servlet
@WebServlet("/LoginCheck")
public class LoginCheck extends HttpServlet{
    public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        String username = req.getParameter("name");
        String password = req.getParameter("pwd");
        //连接数据库
        try {
            ResultSet resultSet = new StudentBeanCheck().check(username,password);
            if(resultSet.next()){
                String username1 = resultSet.getString("username");
                String password1 = resultSet.getString("password");
                if(username1.equals(username)&&password1.equals(password)){
                    //判断是否勾选了两周内不用重新登录
                    String keep = req.getParameter("keep");
                    //如果勾选了，则我们创建Cookie
                    if(keep!=null) {
                        //创建Cookie
                        Cookie name = new Cookie("myname", username1);
                        Cookie pwd = new Cookie("mypwd", password);

                        //设置cookie的生存时间
                        name.setMaxAge(14 * 24 * 3600);
                        pwd.setMaxAge(14 * 24 * 3600);
                        System.out.println("调用了。。");
                        //回写到客户端
                        res.addCookie(name);
                        res.addCookie(pwd);
                    }

                    //表示真的合法
                    HttpSession session = req.getSession(true);
                    session.setAttribute("username",username1);
                    session.setAttribute("password",password);
                    res.sendRedirect("Welcome?username=" + username + "&password=" + password);
                }
                else{
                    res.sendRedirect("index.html");
                    System.out.println("请重新登录！");
                }
            }
            else {
                res.sendRedirect("index.html");
                System.out.println("请重新登录！");
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
    public void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        this.doGet(req,res);
    }
}
```

* Welcome.java(欢迎页面)

```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.*;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/Welcome")
public class WelCome extends HttpServlet {
    public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        String n = null;
        String p = null;
        //将页面内容改成gbk编码模式，防止中文乱码。
        res.setContentType("text/html;charset=gbk");
        HttpSession session = req.getSession(true);
        String username = (String) session.getAttribute("username");
        String password = (String) session.getAttribute("password");
        if (username == null) {
            //如果session中没有用户信息，我们再看一下cookie中有没有
            PrintWriter printWriter = res.getWriter();
            Cookie a[] = req.getCookies();
            if(a==null){
                res.sendRedirect("index.html");
                System.out.println("返回到登录页面");
            }
            for (int i = 0; i < a.length; i++) {
                Cookie temp = a[i];
                if (temp.getName().equals("myname")) {
                    n = temp.getValue();
                }
                else if (temp.getName().equals("mypwd")) {
                    p = temp.getValue();
                }
            }
            if (n != null && p != null) {
                //到LoginCheck页面进行验证
                res.sendRedirect("LoginCheck?username=" + n + "&password=" + p);
                System.out.println("调用了cookie这");
            }
        }
        else {
            Cookie a[] = req.getCookies();
            if(a.length==0){
                System.out.println(username+" "+password);
                PrintWriter printWriter = res.getWriter();
                printWriter.write("<img src=\"img/ldh.png\"><br>");
                printWriter.println("欢迎你，登录成功!" + " " + username + " " + password);
            }
            else{
                int flag=0;
                System.out.println(username+" "+password);
                PrintWriter printWriter = res.getWriter();
                printWriter.write("<img src=\"img/ldh.png\"><br>");
                for(int i=0;i<a.length;i++){
                    String name = a[i].getName();
                    String pwd = a[i].getValue();
                    System.out.println();
                    if(name.equals(username)&&pwd.equals(password)){
                        printWriter.println("欢迎你，登录成功!" + " " + name + " " + pwd);
                        flag=1;
                        break;
                    }
                }
                if(flag==0){
                    printWriter.println("欢迎你，登录成功!" + " " + username + " " + password);
                }
            }
        }

}
    public void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        this.doGet(req,res);
    }
}
```
