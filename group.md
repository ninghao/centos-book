# 群组

把用户放到用户群组里。你可以设置文件与目录的所属群组的权限。

## 查看用户群组

用 groups 命令可以查看用户所属的群组。

```
groups 用户
```

比如查看之前创建的 wanghao 所属的用户组：

```
groups wanghao
```

会返回：

```
wanghao : wanghao wheel
```

表示 wanghao 属于两个用户组，wanghao 还有 wheel 。

## 创建群组

groupadd 可以添加新的群组。

```
groupadd 群组名
```

比如添加一个叫 ninghao 的群组：

```
sudo groupadd ninghao
```

群组相关的信息会保存在一个文件里：

```
/etc/group
```

试一下：

```
cut -d: -f1 /etc/group
```

## 把用户放到群组里

usermod 可以修改用户，使用这个命令可以把用户添加到某个群组里。

```
usermod -a -G 群组 用户
```

**练习**

比如把 wanghao 这个用户放到 ninghao 这个群组里，执行：

```
 sudo usermod -a -G ninghao wanghao
```

再查看一下 wanghao 所属的群组，你会看到里面已经有 ninghao 这个用户组了。

```
wanghao : wanghao wheel ninghao
```

## 修改用户主群组

修改了用户的主群组以后，这个用户创建的目录与文件所属的用户组就是修改之后的这个主群组的名字。用的命令是 usermod ，使用的参数是 -g，像这样：

```
usermod -g 主群组 用户
```

**练习**

把 wanghao 用户的主群组修改成 ninghao，执行：

```
sudo usermod -g ninghao wanghao
```

再查看一下这个用户的群组：

```
groups wanghao
```

返回：

```
wanghao : ninghao wheel
```



