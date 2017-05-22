# Yum

Yum 是用在 CentOS 系统上的包管理工具。

## 搜索包

安装包之前可以先用关键词搜索一下，看看仓库里能不能找到你想要安装的软件包。

```
yum search 关键词
```

比如我要找一下有没有 mariadb 这个数据库：

```
yum search mariadb
```

会返回一些结果（截取）：

```
mariadb.x86_64 : A community developed branch of MySQL
mariadb100u.x86_64 : A community developed branch of MySQL
mariadb101u.x86_64 : A community developed branch of MySQL
```

注意返回的结果里，有些软件包有数字 + u 这个后缀，这是因为我们安装了第三方仓库 IUS，它里的软件包比较新，为了跟系统自带的软件包区分开，包的结尾就用了数字 + u 后缀。数字部分表示包的版本，u 应该指的是 IUS 。

## 包的详情

确定已经找到了你想安装的包，你可以再查看一下这个包的详细信息，比如它的版本，介绍，网站等等，使用这些信息，你可以进一步确认这个包是不是自己需要的。

```
yum info 包的名字
```

比如查看一下 mariadb 这个包的详细信息：

```
yum info mariadb
```

返回类似的内容：

```
Available Packages
Name        : mariadb
Arch        : x86_64
Epoch       : 1
Version     : 5.5.52
Release     : 1.el7
Size        : 8.7 M
Repo        : base/7/x86_64
Summary     : A community developed branch of MySQL
URL         : http://mariadb.org
License     : GPLv2 with exceptions and LGPLv2 and BSD
Description : MariaDB is a community developed branch of MySQL.
            : MariaDB is a multi-user, multi-threaded SQL database server.
            : It is a client/server implementation consisting of a server daemon (mysqld)
            : and many different client programs and libraries. The base package
            : contains the standard MariaDB/MySQL client programs and generic MySQL files.
```

再查看一下另外的一个软件包的详细信息：

```
yum info mariadb101u
```

返回：

```
Available Packages
Name        : mariadb101u
Arch        : x86_64
Epoch       : 1
Version     : 10.1.22
Release     : 1.ius.centos7
Size        : 6.3 M
Repo        : ius/x86_64
Summary     : A community developed branch of MySQL
URL         : http://mariadb.org
License     : GPLv2 with exceptions and LGPLv2 and BSD
Description : MariaDB is a community developed branch of MySQL.
            : MariaDB is a multi-user, multi-threaded SQL database server.
            : It is a client/server implementation consisting of a server daemon (mysqld)
            : and many different client programs and libraries. The base package
            : contains the standard MariaDB/MySQL client programs and generic MySQL files.
```

注意返回的信息，Arch 是这个软件包适用的架构，Version 是这个软件包的版本号，Repo 指的是这个软件包来自哪个仓库。

## 安装包

使用 Yum 安装包：

```
yum install 包的名字
```

注意在使用普通用户安装包的时候，你要在命令前面加上 sudo 获取到管理员的权限，因为只有管理员才能在系统上安装软件包。

比如我要安装 mariadb101u 这个包：

```
sudo yum install mariadb101u -y
```

加上 `-y` 表示确认安装，不然会提示你，是否要安装指定的软件包。

## 解决冲突

你要安装的软件包，可能跟系统上已经存在软件包发生冲突。比如上面执行安装 maraidb101u 这个软件包的时候，就提示：

```
Error: mariadb101u-config conflicts with 1:mariadb-libs-5.5.52-1.el7.x86_64
```

意思就是要安装的软件包或者它依赖的某个包，跟系统上的另外一包（mariadb-libs-5.5...） 有冲突。这可能是因为要安装的东西同样依赖 mariadb-libs ，但可能需要的版本不同。解决的方法是删除掉系统上已经存在的这个软件包：

```
sudo yum remove mariadb-libs -y
```

完成以后再次执行安装，这样应该就不会遇到冲突问题了。

