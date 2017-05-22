# chmod

chmod（change mode）命令可以修改文件与目录的权限。

```
chmod 权限 目录/文件
```

## 数字

修改的权限可以使用数字表示，用三个数字分别表示拥有者，所属用户组还有其它人的权限。比如把一个目录的权限修改成 755：

```
chmod 755 目录
```

把一个文件的权限修改成 644：

```
chmod 644 文件
```

## 字母

修改权限的时候也可以使用字母。r 是读取，w 是写入，x 是执行。u 表示拥有者，g 表示所属用户组，o 表示其它人，a 表示所有人。另外可以配合使用 + 与 - 号，去添加或去掉权限。

比如给文件的拥有者添加一个执行权限，可以这样：

```
chmod u+x 文件
```

让某个目录的所属用户组拥有写入权限，可以：

```
chmod g+w 目录
```

## 练习

先用除了 root 用户以外的身份登录到系统，比如在介绍用户的时候我创建了一个叫 wanghao 的用户，我现在把身份切换成 wanghao 这个用户。

**1**，进入到用户主目录下面，创建一个文件，查看文件的权限：

```
cd ~
touch hello.txt
ls -l
```

返回：

```
-rw-rw-r--. 1 wanghao ninghao 0 May 22 06:56 hello.txt
```

这个 hello.txt 文件的权限如果用数字表示的话，应该是 664 ，读取（r） + 写入（w） = 6 ，读取权限用  4 表示。文件的拥有者是 wanghao，所属用户组是 ninghao 。因为之前我把 wanghao 用户的主群组修改成了 ninghao，所以使用这个用户创建的东西所属的用户组就会是这个 ninghao 用户组。

**2**，理解文件的写入权限。试着写入内容到 hello.txt 文件里：

```
echo 'echo "hello"' >> hello.txt
```

执行上面这行命令的用户是 wanghao，他是 hello.txt 文件的拥有者，这个文件的拥有者有可以写入的权限，所以会成功写入内容。

**3**，理解文件的执行权限。执行一下：

```
./hello.txt
```

意思是执行一下 hello.txt 这个文件。会返回：

```
bash: ./hello.txt: Permission denied
```

提示 Permission denied ，没有权限。因为 hello.txt 对于任何人来说都没有可执行权限（x）。我们可以为文件的拥有者添加一个可执行权限：

```
chmod u+x hello.txt
```

再执行一下 hello.txt，这次就会返回一个 ”hello“，说明拥有者现在已经可以执行这个文件了。

**4**，在用户主目录下创建一个叫 app 的目录：

```
cd ~
mkdir app
```

成功创建了 app 目录，因为用户主目录的拥有者就是当前登录的用户，他对于用户主目录拥有执行与写入的权限。查看主目录下的东西：

```
drwxr-xr-x. 2 wanghao ninghao  6 May 22 08:38 app
-rwxr--r--. 1 wanghao ninghao 13 May 22 08:31 hello.txt
```

创建的 app 目录的拥有者是 wanghao，所属用户组是 ninghao。

**5**，理解目录的执行权限。先把 hello.txt 移动到 app 目录下：

```
mv hello.txt app/
```

当前用户可以成功移动文件到 app 目录。再把 hello.txt 移出 app ：

```
mv app/hello.txt ./
```

也可以完成。这们我们去掉 app 目录的拥有者的可执行权限：

```
chmod u-x app
```

再试着把 hello.txt 放到 app 里面，这次会提示：

```
mv: cannot stat ‘app/hello.txt’: Permission denied
```

提示没有权限，我们再试着用 wanghao 的身份进入到 app 目录：

```
cd app
```

提示：

```
bash: cd: app: Permission denied
```

也没有权限。也就是用户如果对目录没有执行权限，他不能进入这个目录，即使有写入权限，他也不能写入内容到这个目录。

**6**，理解目录的写入权限。先把执行权限交给目录拥有者：

```
chmod u+x app
```

然后去掉拥有者对这个目录的写入权限：

```
chmod u-w app
```

再试一下进入这个目录：

```
cd app
```

可以进入。再返回上一组目录：

```
cd ../
```

用户对目录没有写入权限，但拥有执行权限，是可以进入到这个目录的。

试一下把 hello.txt 移动到 app ：

```
mv hello.txt app/
```

返回：

```
mv: cannot move ‘hello.txt’ to ‘app/hello.txt’: Permission denied
```

没权限，因为 wanghao 这个用户现在不能写入内容到 app 目录的下面，因为他对这个目录没有写入的权限。重新把写入权限交给他，他就可以对这个目录做写入操作了。

```
chmod u+w app
```

**7**，理解用户组的权限。创建一个用户，设置登录密码，把身份切换到这个用户：

```
sudo useradd xiaoxue
sudo passwd xiaoxue
su xiaoxue
```

现在用户的身份是 xiaoxue ，我们试一下把 hello.txt 放到 app 下面，会提示没有权限。对于 app 这个目录来说，xiaoxue 的权限属于其它人，因为 xiaoxue 即不是目录的拥有者，她的群组里面也不包含 app 所属的用户组。

退出 xiaoxue，把她的主群组设置成 ninghao：

```
sudo usermod -g ninghao xiaoxue
```

再修改一下 app 的权限，为所属用户组添加写入权限：

```
sudo chmod g+w app
```

再把身份切换到 xiaoxue 这个用户，然后再试一下把 hello.txt 移动到 app 的下面，这次就会移动成功，因为 xiaoxue 所属的用户组 ninghao，对 app 这个目录拥有执行与写入的权限。

