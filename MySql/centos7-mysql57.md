# centos7下MySQL5.7的安装与配置

## 下载

[下载地址](https://dev.mysql.com/downloads/mysql/)

![](https://zsy0216.github.io/image/notes/20191210145518.png)

![](https://zsy0216.github.io///image/notes/20191210145627.png)

根据系统和版本选择红框中的四个RPM包下载即可，然后放到centos7系统中的/opt目录下，等待稍后安装。

![](https://zsy0216.github.io/image/notes/20191210152329.png)

## 安装前的准备

### 1. 检查系统中是否已经安装mariadb

当系统中存在`mariadb`时，安装`mysql5.7`会出现冲突，所以检查是否存在。

命令：

```shell
rpm -qa | grep mariadb
```

![](https://zsy0216.github.io/image/notes/20191210150638.png)

如果出现上图所示`mariadb-libs-5.5.60-1.el7_5.x86_64`输出，说明已经安装。我们在安装mysql5.7之前要卸载它。

命令：

```shell
rpm -e --nodeps mariadb-libs
```

卸载并检查：

![](https://zsy0216.github.io/image/notes/20191210150759.png)

什么都不输出，表示已经卸载完成。

### 2. 检查系统中是否缺少libaio和net-tools

安装`mysql`需要依赖`libaio`和`net-tools`，所以安装之前需要进行缺省检查

命令：

```shell
rpm -qa | grep libaio
```

```shell
rpm -qa | grep net-tools
```

![](https://zsy0216.github.io/image/notes/20191210151301.png)

如图所示，这两个包都已经存在，所以不需要其他额外的操作，若缺少，应自行安装。

### 3. 检查/tmp目录权限

命令：

```shell
ll / | grep tmp
```

![](https://zsy0216.github.io/image/notes/20191210151906.png)

可以看到此目录权限为所有权限。

## 安装

下载阶段，我们已经把从mysql官网下载的4个rpm包放置于`/opt`目录，现在依次执行下面的命令进行安装。（按照顺序）

i 表示安装，v表示展现信息，h带进度条

```shell
rpm -ivh mysql-community-common-5.7.28-1.el7.x86_64.rpm
rpm -ivh mysql-community-libs-5.7.28-1.el7.x86_64.rpm
rpm -ivh mysql-community-client-5.7.28-1.el7.x86_64.rpm
rpm -ivh mysql-community-server-5.7.28-1.el7.x86_64.rpm
```

![](https://zsy0216.github.io//image/notes/20191210152846.png)

查看版本检验是否安装成功：

```shell
[root@localhost opt]# mysqladmin --version
mysqladmin  Ver 8.42 Distrib 5.7.28, for Linux on x86_64
# 至此，表示mysql已经成功安装。
```

## 配置

### 1. 服务初始化

```shell
mysqld --initialize --user=mysql
```

执行命令后，没有任何提示消息，因为信息位于日志文件中；

初始化命令执行后，在日志信息最后可以看到生成的mysql的初始密码，我们应该记住这个密码。方便接下来进行登录和修改密码。

我们使用以下命令来查看初始化的日志信息，以判断是否初始化成功以及成功之后的初始密码。

```shell
cat /var/log/mysqld.log
```

![](https://zsy0216.github.io///image/notes/20191210153752.png)

### 2. 启动mysql服务

只有启动了mysql服务，才可以进行登录操作。

```shell
systemctl start mysqld

# 查看服务状态
systemctl status mysqld
```

![](https://zsy0216.github.io//image/notes/20191210154334.png)

## 登录

执行下面的登录命令，密码为服务初始化时生成的随机密码

```shell
mysql -uroot -p
```

![](https://zsy0216.github.io//image/notes/20191210154532.png)

登录成功！

## 修改密码

使用初始密码登录的mysql没有任何权限，所以我们需要修改为我们自己设定的密码才可以进行数据库的操作。

```shell
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
```

![](https://zsy0216.github.io//image/notes/20191210154813.png)

修改密码后退出mysql然后重新登录即可进行所有想做的数据库操作

```shell
quit
```

---

查看mysql服务是否时开机自启

```shell
systemctl list-unit-files | grep mysqld
```

## 字符编码问题

```shell
vim /etc/my.cnf
```

按 i 进入编辑模式，在文件最后一行加上`character_set_server=utf8`

按 esc 输入`:wq`，保存退出。

重启服务：

```shell
systemctl restart mysqld
```

