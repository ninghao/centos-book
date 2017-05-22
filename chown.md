# chown

chown（change owner），这个命令可以修改文件与目录的所有权，也就是拥有者还有所属的群组。

```
chown 用户:群组 文件/目录
```

如果想修改某个目录以及它里面包含的所有的子目录的所有权，可以加上一个 -R 选项：

```
chown -R 用户:群组 目录
```

## 练习

1，先切换到一个普通用户的身份，比如切换成我之前创建的 wanghao 这个用户。

```
su wanghao
```

2，进入到 /mnt 这个目录的下面，试着去创建一个新的目录。

```
cd /mnt
mkdir app
```

会提示：

```
mkdir: cannot create directory ‘app’: Permission denied
```

因为 wanghao 没有权限在 /mnt 下面创建新的目录，所以我们在命令前面加上一个 sudo 再试一下：

```
sudo mkdir app
```

这次成功创建了一个叫 app 的目录，查看一下 /mnt 里面的内容：

```
ls -l
```

返回：

```
drwxr-xr-x. 2 root root 6 May 22 08:12 app
```

观察 app 目录的拥有者还有所属的用户组，现在它的拥有者是 root ，所属的用户组是 root。

3，把 /mnt 下的 app  目录的拥有者修改成 wanghao，执行：

```
sudo chown wanghao app
```

查看 /mnt 下的内容，你会发现 app 目录的拥有者现在变成了 wanghao。

```
drwxr-xr-x. 2 wanghao root 6 May 22 08:12 app
```

再把 app 目录的所属用户组修改成 ninghao：

```
sudo chown wanghao:ninghao app
```

查看 /mnt 目录下的内容列表，观察 app 目录的拥有者，还有所属用户组。

