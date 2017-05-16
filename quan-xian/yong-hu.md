# 用户

你会使用某个用户的身份连接到 CentOS 服务器。

## root

root 这个用户是系统的超级管理员，拥有最高级别的权限。它可以在系统里面做任何想做的事情，比如去管理系统里的服务，去为系统添加新的用户，设置用户的密码等等。拿到一台服务器，除了服务器的 IP 地址以外，一般还会给你一个 root 用户的密码。你可以先用它登录到服务器，去做一些初始化的配置。

因为 root 用户的能力太强大了，同时也很危险。所以一般的工作，我们不直接使用它，可以创建一个普通的用户，当需要用到 root 权限去做事情的话，我们可以再切换到 root 用户的身份去执行任务。你可以配置普通用户，让它可以拥有使用 sudo 的权限，这样需要 root 身份执行的任务，就在任务最前面添加一个 sudo 。

## 添加用户

useradd 可以在系统里添加新用户。比如我要添加一个叫 wanghao 的用户，执行：

```
useradd wanghao
```

提示：

```
-bash: /usr/sbin/useradd: Permission denied
```

因为我当前登录的用户并不是超级管理员（root），它没有直接的权限去做添加用户，但是这个用户是可以使用 sudo 获得 root 用户的权限。再这样试一下：

```
sudo useradd wanghao
```

这样就会成功添加一个叫 wanghao 的用户。

## 设置密码

passwd 可以设置用户的密码。我们为之前添加的 wanghao 用户设置一下登录用的密码。

```
sudo passwd wanghao
```

提示：

```
Changing password for user wanghao.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.
```

New password，输入新的密码，Retype new password，重新再输入一次。

服务器上的用户密码要尽量复杂，可以使用工具生成一些随机的密码：

```
openssl rand -base64 32
```

返回类型的内容：

```
wZ9asIXK5/GF3KIqOpRugK8Zkb6ARMkrBjy/+3t3Zu8=
```

## 切换用户

使用 su （swich user）命令切换用户的身份，命令后面不加具体的用户，就是要切换到 root 这个用户。

```
sudo su
```

提示你输入密码，这个密码应该是 root 用户的密码。不知道或者忘了密码可以用 passwd 去设置一下。

切换成功以后，在命令提示符上会显示你当前的身份：

```
[root@localhost vagrant]#
```

@ 左边的 root 就是当前用户的身份。或者也可以执行：

```
echo $USER
```

上面的命令会告诉你当前用户是谁。再切换到 wanghao 这个用户：

```
su wanghao
```

退出可以执行：

```
exit
```

```
[wanghao@localhost vagrant]$ exit
exit
[root@localhost vagrant]# exit
exit
[vagrant@localhost ~]$ 
```



