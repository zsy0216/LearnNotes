# Tomcat的安装与配置

## 下载

下载地址：[](https://tomcat.apache.org/)

选择对应系统位数的压缩文件。这里选择的版本是8.5.49,64-bit Window zip

## 安装配置

tomcat的运行需要基于jdk，安装tomcat之前要保证系统中存在完整地java环境。

直接解压到你想保存的位置，如：`D:\devtools\apache-tomcat-8.5.49`即可使用。

## 目录结构

![image-20191201221653639](https://zsy0216.github.io/image/notes/javaweb/tomcat/image-20191201221653639.png)

Tomcat的目录结构

- bin：存放一些启动运行Tomcat的可执行程序和相关内容；
- conf：存放一些关于Tomcat服务器的全局配置；　　  　  
- lib：目录存放Tomcat运行或者站点运行所需的jar包，所有在此Tomcat上的站点共享这些jar包。　　　　  
- logs： 存放日志文件
- temp:  存放临时文件
- wabapps：目录是默认的站点根目录，可以更改。　　　　  
- work：目录用于在服务器运行时过度资源，简单来说，就是存储jsp、servlet翻译、编译后的结果。　

## 环境变量

根据网上众说纷纭的环境变量配置方法，一般情况都要求加入以下三个系统变量
- TOMCAT_HOME ：`D:\devtools\apache-tomcat-8.5.49`
- CATALINA_BASE ： `%TOMCAT_HOME%`
- CATALINA_HOME ：`%TOMCAT_HOME%`

根据网上的解释，这样设置的好处是之后修改tomcat的版本时只需要修改TOMCAT_HOME的路径即可，而在不确定未来是否要这么做之前，可以只设置一个CATALINA_HOME变量，值为Tomcat的安装路径。

## 运行

此时，在bin目录下找到`startup.bat` 双击执行即可打开Tomcat服务。此时打开浏览器在地址栏输入127.0.0.1:8080，看到tomcat的管理界面即可证明安装完成。

![image-20191201224613303](https://zsy0216.github.io/image/notes/javaweb/tomcat/image-20191201224613303.png)

想要关闭tomcat服务，关闭弹出的窗口，或者在bin目录下找到`shutdown.bat` 双击即可。