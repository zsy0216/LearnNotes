# Servlet简介

## 概念

运行在服务器端的小程序

  * Servlet就是一个接口，定义了Java类被浏览器访问到(tomcat识别)的规则。
    * 将来我们自定义一个类，实现Servlet接口或Servlet的子类，覆写方法。

## 运行原理

```xml
 <!-- web.xml配置Servlet -->
<servlet>
    <servlet-name>demo</servlet-name>
    <servlet-class>cn.web.servlet.ServletDemo</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>demo</servlet-name>
    <url-pattern>/demo</url-pattern>
</servlet-mapping>
```

1. 当服务器接受到客户端浏览器的请求后，会解析请求URL路径，获取访问的Servlet的资源路径;
2. 查找web.xml文件，是否有对应的`<url-pattern>`标签体内容;
3. 如果有，则在找到对应的`<servlet-class>`全类名;
4. tomcat会将字节码文件加载进内存，并且创建其对象;
5. 调用其方法;

## 生命周期

![](https://zsy0216.github.io/image/notes/20200310182350.png)

### 1.初始化阶段

当客户端向Servlet容器发出HTTP请求要求访问Servlet时，Servlet容器首先会解析请求，检查内存中是否已经有了该Servlet对象，如果有，则直接使用该Servlet对象，如果没有，则创建Servlet实例对象，然后通过调用init()方法实现Servlet的初始化工作。需要注意的是，在Servlet的整个生命周期内，它的init()方法只能被调用一次。

- 第一次请求时，创建Servlet实例对象；
- 调用init()方法初始化。（不是第一次时直接使用Servlet对象）

### 2.运行阶段

这是Servlet声明周期中最重要的阶段，在这个阶段中，Servlet容器会为这个请求创建代表HTTP请求的ServletRequest对象和代表HTTP响应的ServletResponse对象，然后将它们作为参数传递给Servlet的service()方法。

service()方法从ServletRequest对象中获得客户请求信息并处理该请求，通过ServletResponse对象生成响应结果。

在Servlet的整个生命周期内，对于Servlet的每一次访问请求，Servlet容器都会调用一次Servlet的service() 方法，并且创建新的ServletRequest 和 ServletResponse 对象，也就是说，service() 方法在Servlet 的整个生命周期中会被调用多次。

- 一次请求就调用一次service()方法，并创建新的请求和响应对象作为参数传入；

### 3.销毁阶段

当服务器关闭或Web应用被移除出容器时，Servlet随着Web应用的关闭而销毁。在销毁Servlet之前，Servlet容器会调用 Servlet 的 destory() 方法，以便让 Servlet 对象释放它所占用的资源。在Servlet的整个生命周期中，destory() 方法也只会被调用一次。

需要注意的是，Servlet 对象一旦创建就会驻留在内存中等待客户端的访问，直到服务器关闭或 Web 应用被移除出容器时，Servlet 对象才会销毁。

- 服务器关闭或应用销毁时，调用 destory() 方法销毁；

## 体系结构

Servlet是一个接口，下面有两个抽象子类。

其中GenericServlet抽象类直接实现Servlet接口，HttpServlet继承GenericServlet抽象类；

## request和response

- request和response对象是由服务器创建的。
- request对象是来获取请求消息，response对象是来设置响应消息

- request和response对象是ServletRequest接口的两个子接口HttpServletRequest和HttpServletResponse声明的两个对象，用来处理请求和响应进而与客户端进行交互；

- ServletRequest接口 的子接口 HttpServlet比较常用，一般使用HttpServleRequestt来声明request对象；response同理；

---

- 通过request对象可以获得**请求行**数据中的请求方式，URI，请求参数，HTTP协议等；还可以获得**请求头**数据和**请求体**数据；
- 通过response对象可以设置响应消息；**响应行，响应头，响应体；**
- HTTP传输数据类型：ContentType

### request域

**域对象**：一个有作用范围的对象，可以在范围内共享数据；

作用范围：一次请求。一般用于请求转发的多个资源中共享数据；

**方法**:

- `void setAttribute(String name, Object obj)`：存储数据；
- `Object getAttribute(String name)`：根据键获取数据；
- `void removeAttibute(String name)`：根据键移除键值对数据；

## Servlet请求响应流程

1. 浏览器发送请求到服务器（请求）
2. 服务器接收浏览器的请求进行解析，创建request对象存储请求数据
3. 服务器调用对应的Servlet进行请求处理，并将request对象作为实参传递给Servlet的service方法
4. Servlet的service方法执行请求处理：
   1. 设置请求的编码格式 `request.setCharacterEncoding("utf-8");`
   2. 设置响应的编码格式 `response.setContentType("text/html;charset=utf-8");`
   3. 获取请求信息
   4. 处理请求信息
      1. 创建业务层对象
      2. 调用业务层对象的方法
   5. 响应请求处理
      1. 直接响应
      2. 请求转发
      3. 重定向

### 请求转发

服务器内部资源跳转的一种方式。

```java
request.getRequestDispatcher("servletname").forward(request,response);
```

* 请求转发的两个Servlet属于一次请求，可以使用request对象共享数据；
* 浏览器地址栏信息不改变，
* 只能转发到当前服务器的资源；
* 刷新时可能导致表单重复提交。
* / :表示项目根目录

### 重定向

资源跳转的方式。

```java
response.sendRedirect("url");
```

* 使用时机：解决当前Servlet无法处理请求的问题以及表单重复提交的问题。
* 重定向的两个Servlet属于两次请求，拥有两个request对象；
* 浏览器地址会改变，Servlet之间数据不能共享。
* 可以重定向到其他站点的资源；
* 两次请求的数据共享需要使用session，session依赖于cookie。
* / : 表示服务器根目录，绝对路径需要加上虚拟项目名

## 会话技术

- 会话：一次会话中包含多次请求和响应；一次会话是指浏览器第一次向服务器发起请求，会话建立，直到有一方断开为止；

- 功能：再一次会话的范围内的多次请求间共享数据；
- 方式：
  - Cookie：客户端（浏览器）会话技术；
  - Session：服务器端会话技术；

### Cookie

概念：客户端会话技术，将数据保存在客户端。

实现原理：基于响应头set-cookie（发送） 和 请求头cookie实现（接收）；

#### 使用步骤：

1. 创建cookie对象，绑定数据

```java
Cookie cookie = new Cookie(String name, String value);
```

2. 发送Cookie对象，在响应头中的set-cookie字段中；

```java
response.addCookie(cookie);
```

3. 获取Cookie，拿到Cookie，在下一次请求请求头的cookie字段中；

```java
Cookie[] cookies = request.getCookies();//数组
```

#### 细节：

1. 一次可不可以发送多个cookie？

   - 可以，创建多个cookie对象，多次调用addCookie方法；

2. cookie在浏览器中保存多长时间？

   - 默认情况下，当浏览器关闭后，cookie数据被销毁；

   - 持久化存储,设置有效期：`cookie.setMaxAge(int seconds);`

     参数为正数表示持久化到硬盘并设置cookie存活时间，单位秒；负数为默认值；0代表删除cookie信息；

3. cookie能不能存中文？

   - tomcat8 之前不能直接存储中文数据，tomcat8之后支持中文数据；

   - tomcat8之前对中文要进行URL编码存储，URL解码解析；
   - tomcat8对特殊字符如空格等也不支持，要进行URL编码存储；

4. cookie共享问题？

   - 在同一个tomcat服务器下的多个web项目不可以共享cookie；
   - 可以通过设置有效路径：`cookie.setPath(String uri);`设置cookie的获取范围。默认情况下为当前项目的虚拟目录；
   - 如果要在同一服务器内多个web项目共享，可以把path设置为"/",表示服务器内部共享；

   - 如果要在不同服务器间进行cookie共享，可以设置域名setDomain(String path)：如果设置一级域名相同，那么多个服务器之间的cookie可以共享；
     - setDomain(".baidu.com")，那么tieba.baidu.com和news.baidu.com之间的cookie可以共享；

#### 特点：

1. cookie存储在客户端浏览器；
2. 浏览器对于单个cookie的大小有限制，以及对同一个域名下的总数量也有限制；

#### 作用：

- cookie一般用于存储少量的不太敏感的数据；
- 在不登录的情况下，完成服务器对客户端的身份识别；

### Session

**概念**：服务器会话技术，在一次会话的多次请求间共享数据，将数据保存在服务器端的对象中，属于HttpSession类型；

**实现原理**：session的实现是依赖于cookie的；

Session也是一个**域对象**，与Request、ServletContext相似，也有域对象同样的方法：

- `Object getAttribute(String name);`
- `void setAttribute(String name, Object value);`
- `void removeAttribute(String name);`

#### 使用步骤：

1. 创建/获取session对象;
   - 如果请求中拥有session的标识符也就是JSSESIONID，则返回其对应的session对象
   - 如果请求中没有JSSESIONID，则创建新的session对象，并将其JSSESIONID作为cookie对象存储到浏览器的运行内存中
   - 如果session对象失效了，也会创建一个session对象，并将其JSESSION对象存储到浏览器的运行内存中

```java
HttpSession session = request.getSession();
```

2. 存储数据

```java
session.setAttribute(String name, Object value);
```

3. 获取数据

```java
Object value = session.getAtrribute(String name);
```

注意：存储数据的动作和获取数据的动作发生在不同的请求中，但是存储要先于取出执行；

#### 细节：

1. 客户端关闭后，服务器不关闭，两次获取的session是否为同一个？

   默认情况下，不是。默认情况下，浏览器关闭后，cookie即失效，session也就失效了，除非提前创建cookie并设置最大存活时间，让cookie持久化保存，在设置时间范围内，获取的session为同一个。

   ```java
   Cookie c = new Cookie("JSESSIONID", session.getId());
   c.setMaxAge(60*60);
   response.addCookie(c);//此时1小时内，获取的session为同一个；
   ```

2. 客户端不关闭，服务器关闭后，两次获取的session是否为同一个？

   不是。但是要确保数据不丢失。Tomcat服务器自动实现钝化和活化；

   - session的钝化：在服务器正常关闭之前，将session对象序列化到硬盘上；
   - session的活化：在服务器启动后，将session文件转化为内存中的session对象；

3. session什么时候被销毁？

   - 服务器关闭；

   - session对象调用invalidate()  `session.invalidate();`；

   - 默认失效时间为30分钟；

     可以在web.xml中配置session-config设置默认失效时间。

     也可以设置：`hs.setMaxInactiveInterval(int seconds);`

#### 特点：

- session用于存储一次会话的多次请求的数据，存在服务器端
- session可以存储任意类型，任意大小的数据
- 依赖cookie技术，生命周期为一次回话，默认存储时间是30分钟
- session对象的唯一标识JSESSIONID同过cookie技术作为一个cookie数据存储到浏览器的运行内存中

与cookie的区别：

- session存储数据在服务器端，cookie在客户端；
- session没有数据大小限制，cookie有；
- session数据安全(在服务器端)，cookie相对不安全；

#### 总结：

4. 总结：session解决了一个用户的不同请求的数据共享问题，只要在JSESSIONID不失效和session对象不失效的情况下，用户的任意请求在处理时都能获取到同一个session对象

5. 注意：JSESSIONID存储在了cookie的临时存储空间中，浏览器关闭即失效；

## ServletContext对象

> request解决了一次请求内的数据共享问题，session解决了一个用户不同请求的数据共享问题，而不同用户之间的数据共享问题则需要使用ServletContext对象。

### 特点

   	1. 由服务器创建
      	2. 用户共享
         	3. 一个项目只有一个

### 生命周期

​	服务器启动到服务器关闭

### 使用：

```java
//1.获取ServletContext对象（三种方式，等价）
ServletContext sc = this.getServletContext();
ServletContext sc2 = this.getServletConfig().getServletContext();
ServletContext sc3 = request.getServletContext();
//ServletContext sc3 = req.getSession().getServletContext();
//2.使用ServletContext对象完成数据共享
sc.setAttribute(String name,Object value);
sc.getAttribute(String name);//返回Object对象类型数据
//3.获取项目中web.xml中的全局配置数据
sc.getInitParameter(String name);//返回String类型数据
//4.获取项目根的根路径信息
sc.getRealPath();
```

### 功能：

1. 获取MIME类型：在互联网通信过程中定义的一种文件数据类型；

   格式：大类型/小类型	text/html;

   方法：String getMinmeType(String file);

2. **域对象**：共享数据；所有用户共享数据；

   - setAttribute(String name, Object value);
   - getAttribute(String name);
   - removeAttribute(String name);

3. 获取文件的真实（服务器）路径；

   - String getRealPath(String path);

### 文件下载

设置请求头信息：

- setHeader("content-disposition", "attachment;filename=xxx");
- setHeader("content-type", this.getServletContext().getMinmeType(filename));
  - HTTP传输数据类型：ContentType；
  - 这里要在响应头中设置HTTP传输数据的类型MIME为文件对应的类型；

注意：文件名的中文问题要根据浏览器版本信息设置对应的文件编码方式；

## ServletConfig

​	ServletConfig对象是Servlet的专属对象（域对象）.

### 问题：

​	如何获取在web.xml中给每个Servlet单独配置的数据？

### 解决：

​	使用ServletConfig对象

### 使用：

 1. 获取ServletConfig对象

    ```java
    ServletConfig sc = this.getServletConfig();
    ```

 2. 获取web.xml中的配置数据

    ```java
    String value = sc.getInitParameter("name")
    ```

## Web.xml文件使用总结

### 作用：

​	存储项目相关的配置信息，保护Servlet。解耦一些数据对程序的依赖。

### 使用位置：

​	每个Web 项目中；

​	Tomcat服务器中（在服务器conf目录下）

### 区别：

​	Web项目下的web.xml文件为局部配置，针对本项目的配置；

​	Tomcat下的web.xml文件为全局配置，配置公共信息；

### 内容（核心组件）

​	全局上下文配置（全局配置参数）

​	Servlet配置

​	过滤器配置

​	监听器配置

### 加载顺序：

​	Web容器会按ServletContext -> context-param -> listener -> filter -> servlet这个顺序加载组件，这些元素可配置在web.xml文件中的任意位置。

​	在服务器启动时完成加载。

## server.xml

### 问题：

​	浏览器发起请求后，服务器根据请求在webapps目录下调用对应的的Servlet进行请求处理。那么为什么是webapps目录，难道不能是其他的文件吗？

### 解决：

​	了解server.xml文件的配置信息

server.xml文件的核心组件

```xml
<Server>
	<Service>
        <Connector/>
        <Connector/>
        <Engine>
            <Host>
                <Context/>
            </Host>
        </Engine>
    </Service>
</Server>
```

### 热部署

```xml
<Context path="/Pet" reloadable="true" docBase="F:/PetWebroot"/>
```

