---
title: IntelliJ Idea 创建第一个 Servlet 项目
date: 2018-07-02 10:17:10
tags: Servlet
categories: [Java, Servlet]
---

&emsp;&emsp;本文主要根据[Intellij Idea 创建Web项目并部署servlet](https://blog.csdn.net/antony9118/article/details/51800404)，[JavaWeb开发 - 使用IDEA创建Servlet程序](https://www.jianshu.com/p/82446a31f0b9)两篇文章。

&emsp;&emsp;一开始入手  JavaWeb、Tomcat 还是有一点点迷茫的，因为自己还没有太多的接触过有关的网络编程，不过学习编程早已习惯了一开始的无从下手，不用怕，找路就是了。本文记录的是使用 ``IntelliJ Idea 2017.3.2 ``编写第一个 JavaWeb 项目，并部署Servlet，用到了 Tomcat。（一开始学习还不知道这个具体来做什么的）

## 1. 创建一个web项目

![创建项目](http://cmhblog.cfzhao.com/blog/180702/FlHI8ljLE4.png)

<!--more-->我们创建好后文件工程结构目标如下：（图片来源于网络）![mark](http://cmhblog.cfzhao.com/blog/180702/C2i412I6I2.png)

&emsp;&emsp;但我使用``IntelliJ Idea 2017.3.2``创建好后，却没有 WEB-INF 文件夹和 web.xml 文件，这时就要自己配置添加``web.xml``。

步骤：

1. 打开 Project Structure， ( File -> Project Structure, 快捷键 Ctrl+Shift+Alt+S. 也可以右击项目，有个 Open Module Settings, 快捷键 F4. )

2. 在 Facets 中选中次级的 Web 或者在 Modules 中选中Web，在 Deployment Descriptors 面板里，点击   + 号选择 web.xml 以及版本号。然后在弹出的对话框中修改 xml 默认的目录，加上 web 就可以了。 ![mark](http://cmhblog.cfzhao.com/blog/180702/Cf0hgKcKAD.png)

3. 完成。![mark](http://cmhblog.cfzhao.com/blog/180702/didEidD7E5.png)
(classes和lib是后边设置的)

## 2. Web工程设置

### 创建``classes``、``lib``目录

1. 在WEB-INF 目录下点击右键，New --> Directory，创建 classes 和 lib 两个目录 。classes目录用于存放编译后的class文件，lib用于存放依赖的jar包 
2. ``classes``目录配置<br>File --> Project Structure，进入 Project Structure窗口，点击 Modules --> 选中项目“JavaWeb” --> 切换到 Paths 选项卡 --> 勾选 “Use module compile output path”，将 “Output path” 和 “Test output path” 都改为之前创建的 classes 目录。![mark](http://cmhblog.cfzhao.com/blog/180702/miJm6Ij30i.png)即将后面编译的class文件默认生成到classes目录下 
3. ``lib``目录配置<br>还是在这个`Project Structure`这个窗口。点击 Modules --> 选中项目“JavaWeb” --> 切换到 Dependencies 选项卡 --> 点击右边的“+”，选择 “JARs or directories...”，选择创建的lib目录。 

### 配置打包方式Artifacts

&emsp;&emsp;点击 Artifacts选项卡，IDEA会为该项目自动创建一个名为“JavaWeb:war exploded”的打包方式，表示 打包成war包，并且是文件展开性的，输出路径为当前项目下的 out 文件夹，保持默认即可。~~另外勾选下“Build on make”~~"Include in project build"(虽然不知道是干啥用的，但按照他的意思勾上吧)，表示编译的时候就打包部署，勾选“Show content of elements”，表示显示详细的内容列表。![mark](http://cmhblog.cfzhao.com/blog/180702/l5BbLilfm0.png)

## 3. Tomcat 配置

### 创建 Tomcat 容器

1. 打开菜单Run -> 选择Edit Configuration  ![mark](http://cmhblog.cfzhao.com/blog/180702/6fKA0BD1l5.png)
2. 点击“+”号 -> 选择“Tomcat Server” -> 选择“Local”  ![mark](http://cmhblog.cfzhao.com/blog/180702/md4Hg8mfKl.png)
3. 点击 "Application server" 后面的 "Configure...", 弹出Tomcat Servers 窗口，选择本地安装的 Tomcat 目录，  比如我的是"G:\Tomcat\apache-tomcat-9.0.10", 即 bin 文件夹所在的目录。 点击OK。
4. 在”Run/Debug Configurations” 窗口的”Server” 选项板中，取消勾选”After launch”，设置”HTTP port” 和”JMX port”（默认值即可），点击 Apply -> OK， 至此 Tomcat 配置完成。  
5. 点击Deployment -> 选择刚刚建立的 Tomcat 容器 -> 选择 Deployment -> 点击右边的 “+” 号 -> 选择 Artifact-> 选择 web 项目 -> Application context 可以填 “/HelloWorld”(其实也可以不填的~) -> OK .![mark](http://cmhblog.cfzhao.com/blog/180702/J2LgCk9b5L.png)

## 4. 编辑 ``index.jsp``文件

![mark](http://cmhblog.cfzhao.com/blog/180702/44DBe7bldL.png)

## 5. 运行 Tomcat， 在浏览器中查看结果

&emsp;&emsp;点击绿色按钮，在浏览器地址栏软入 localhost:8080/HelloWorld (HelloWorld为刚刚设置的地址)，查看结果。![mark](http://cmhblog.cfzhao.com/blog/180702/CAICkfi5bH.png)

## 6. Servlet简单实现

### 编写Servlet源文件

&emsp;&emsp;在 src 目录下新建 HelloWorld.java，并编写一下代码并进行编译： 

```java
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class HelloWorld extends HttpServlet {
    @Override
    public void init() throws ServletException {

    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html");

        PrintWriter out = resp.getWriter();
        out.println("<h1>Hello,world!</h1>");
    }

    @Override
    public void destroy() {
        super.destroy();
    }
}
```

&emsp;&emsp;因为是第一次编写 Servlet 代码，没有过经验，所以可能要导入下 J2EE 库， Idea 蛮智能的，跟着它的引导走就好了。

&emsp;&emsp;编译后会发现在 classes 目录下生成了 HelloWorld.class 文件。![mark](http://cmhblog.cfzhao.com/blog/180702/3j5EKBFjh9.png)



## 7. 部署 Servlet

方法一：在 WEB-INF 目录下 web.xml 文件的 `<web-app> `标签中添加如下内容：

```xml
<servlet>
        <servlet-name>HelloWorld</servlet-name>
        <servlet-class>HelloWorld</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>HelloWorld</servlet-name>
        <url-pattern>/HelloWorld</url-pattern>
    </servlet-mapping>
```

 其中 `<url-pattern>`为访问网址的地址。

方法二：在 HelloWorld 文件的类前面加上：

```java
@WebServlet(name = "HelloWorld", urlPatterns = "/my")
```

### 运行 Servlet

![mark](http://cmhblog.cfzhao.com/blog/180702/HaalhdHFJA.png)

&emsp;&emsp;点击运行， 控制台出现 successfully 则 tomcat 服务启动成功！打开浏览器输入：localhost:8080/HelloWorld/HelloWorld 即可查看 servlet 运行状态了。

![mark](http://cmhblog.cfzhao.com/blog/180702/I6Hldc71C5.png)

### 更新后点击运行四个按钮

![mark](http://cmhblog.cfzhao.com/blog/180702/galelEm2FG.png)四个按钮的区别待更新。


<p align="right">2018年7月2日</p>