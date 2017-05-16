# sudo

让普通用户可以使用 root 身份去执行任务，你得让用户拥有可以使用 sudo 的权限。先把身份切换到之前新创建的 wanghao 这个用户：

```
su wanghao
```

然后先试一下让他使用 sudo 去执行一些任务：

```
sudo systemctl start firewalld
```

会提示：

```
[sudo] password for wanghao: 
wanghao is not in the sudoers file.  This incident will be reported.
```

提示 wanghao 这个用户不在 sudoers 这个文件里。也就是他还不能使用 sudo 获得 root 权限。

先执行 `exit` 退出 wanghao 用户身份，然后用有 root 权限的用户，把 wanghao 加到 wheel 这个用户群组里，在这个群组里的用户都可以使用 sudo。执行：

```
sudo gpasswd -a wanghao wheel
```

返回：

```
Adding user wanghao to group wheel
```

查看用户所属群组：

```
groups wanghao
```

返回：

```
wanghao : wanghao wheel
```

再切换到 wanghao，然后用一下 sudo：

```
su wanghao
sudo systemctl start firewalld
```

这次就成功的完成了需要 root 身份才能完成的任务（启动了 firewalld 防火墙）。权力太大忍不住再试一次：

```
sudo systemctl stop firewalld
```

这回又把 firewalld 防火墙给关了。查看服务状态可以执行：

```
sudo systemctl status firewalld
```



