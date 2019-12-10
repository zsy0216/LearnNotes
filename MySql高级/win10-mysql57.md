# Win10下MySql5.7的安装与配置

## 下载

[官网下载地址](https://dev.mysql.com/downloads/mysql/)

选择免安装版即可，

![](https://zsy0216.github.io/image/notes/20191210102827.png)

## 解压

将下载的压缩包解压到你想要放置MySQL的目录，避免中文空格。

示例：`D:\devtools\mysql-5.7.28-winx64`

## 配置

### 安装mysql服务

- 在安装目录中的bin目录下使用管理员权限打开cmd窗口，安装mysql服务。

  命令：`mysqld --install`

  ![](https://zsy0216.github.io/image/notes/20191210105425.png)

### 初始化mysql

- 初始化会生成一个随机密码，这里需要记住这个密码，方便之后登录mysql进行修改。

  命令：`mysqld --initialize --console`

  ![](https://zsy0216.github.io/image/notes/20191210110058.png)

### 启动mysql服务

- 登录mysql之前，需要启动之前安装的服务。

  命令：`net start MySql`

  ![](https://zsy0216.github.io/image/notes/20191210110404.png)

### 登录

- 登录mysql可以验证是否安装成功，这里登录时的用户名为root，密码为刚才初始化生成的随机密码，刚才应该已经记录。

  命令：`mysql -uroot -p`

  ![](https://zsy0216.github.io/image/notes/20191210110835.png)

这个时候发现登录不上去。。

报错信息：

```shell
Enter password: ************
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)
```

**解决方案：**

1. 首先停止MySQL服务，使用管理员权限打开cmd窗口，输入`net stop mysql`
2. 停止成功后，找到mysql的安装目录，删除data文件夹
3. 在mysql安装路径下的bin目录下，执行命令：`mysqld --initialize --console`,这一步会在最后生成一个随机初始密码，先记住下一步登录时用
4. 现在启动MySQL服务：`net start mysql`
5. 登录mysql： `mysql -uroot -p********`，*代表第3步生成的随机密码
6. 这时发现登录成功，之后修改密码，使用初始密码登录的mysql没有对数据库的任何权限

**修改密码**

```shell
mysql> alter user 'root'@'localhost' identified by '新密码';
mysql> quit
```

重新登录，即可正常使用mysql数据库

至此，MySQL5.7的安装与配置已经完成。

## 设置全局环境变量

右键此电脑👉点击左侧高级系统设置👉点击最下面的环境变量👉下面系统变量中找到path👉点击下面的编辑按钮👉点击右侧新建👉将mysql的bin目录地址复制进去（`D:\devtools\mysql-5.7.28-winx64\bin`）

![](https://zsy0216.github.io/image/notes/20191210114152.png)

