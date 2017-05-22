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

会返回一些结果：

```
mariadb-bench.x86_64 : MariaDB benchmark scripts and data
mariadb-devel.i686 : Files for development of MariaDB/MySQL applications
mariadb-devel.x86_64 : Files for development of MariaDB/MySQL applications
mariadb-embedded.i686 : MariaDB as an embeddable library
mariadb-embedded.x86_64 : MariaDB as an embeddable library
mariadb-embedded-devel.i686 : Development files for MariaDB as an embeddable library
mariadb-embedded-devel.x86_64 : Development files for MariaDB as an embeddable library
mariadb-libs.i686 : The shared libraries required for MariaDB/MySQL clients
mariadb-libs.x86_64 : The shared libraries required for MariaDB/MySQL clients
mariadb-server.x86_64 : The MariaDB server and related files
mariadb.x86_64 : A community developed branch of MySQL
mariadb-test.x86_64 : The test suite distributed with MariaD
```

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



