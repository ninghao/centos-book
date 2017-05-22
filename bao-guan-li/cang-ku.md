# 仓库

仓库（Repository），这里指的是可以使用包管理工具安装的软件包的列表。系统自带一些仓库，如果你发现要安装的包在这些仓库里不存在，你可能需要在系统上安装额外的仓库。

## 仓库列表

先查看一下安装在系统上的仓库列表，执行：

```
yum repolist
```

返回类似的东西：

```
repo id                                            repo name                                             status
base/7/x86_64                                      CentOS-7 - Base                                       9,363
extras/7/x86_64                                    CentOS-7 - Extras                                       337
updates/7/x86_64                                   CentOS-7 - Updates                                    1,598
repolist: 11,298
```

## 第三方仓库

有时候你要安装的东西在系统自带的仓库列表里没找到，或者版本比较低。这时你可以选择一些第三方的仓库，比如 IUS ，它里面包含了很多比较新的软件包。

安装 IUS 仓库：

```
sudo yum install https://centos7.iuscommunity.org/ius-release.rpm -y
```

完成以后再查看一下仓库列表，你会发现多了两个仓库：

```
epel/x86_64                      Extra Packages for Enterprise Linux 7 - x86_64                          11,661
ius/x86_64                       IUS Community Packages for Enterprise Linux 7 - x86_64                     378
```

epel 是 ius 仓库依赖的一个仓库，所以安装 ius 的时候包管理工具也把 epel 安装好了。

## 仓库文件

系统上安装的这些仓库，其实都是用一些仓库文件定义的，这些文件的位置是在：

```
/etc/yum.repos.d/
```

查看上面这个目录，你会看到一些 .repo 文件，再查看一下某个 .repo 文件里的内容，观察定义一个仓库都用了什么东西。

有时候我们可能需要手工去创建一些仓库文件，比如下面是一个 NGINX 稳定版的仓库，在 /etc/yum.repos.d 目录下面创建一个叫 nginx.repo 的文件，文件内容如下：

```
[nginx]
name=NGINX Stable Version
baseurl=http://nginx.org/packages/centos/7/$basearch/
gpgcheck=0
enabled=1
```

再查看一下仓库列表，你会发现 NGINX Stable Version 这个仓库。





