# tar

在 Linux 系统上，打包与解压可以使用 tar。

## 选项

tar 有很多选项，记住它们不容易，先只需要记住常用的操作需要的选项。

* c（create） 创建
* z（gzip）一种压缩格式
* f（file） 表示文件
* x（extract） 表示解压

## 打包

需要 create file 这两个选项，打包生成的文件一般用 .tar 后缀。

```
tar --create --file 生成的打包文件.tar 要打包的文件/目录
```

使用的选项也可以简化一下：

```
tar -cf 生成的打包文件.tar 要打包的文件/目录
```

## 打包并压缩

需要 create gzip file 这三个选项，生成的压缩包文件一般用 .tar.gz 后缀。

```
tar -czf 生成的打包文件.tar.gz 要打包的文件/目录
```

## 解包

需要用 extract file 这两个选项。

```
tar -xf 要解包的文件.tar
```

## 解压

解开用 gzip 格式压缩的包，用 extract gzip file 这三个选项。

```
tar -xzf 要解包的文件.tar.gz
```

## 练习

**1**，先随便找一个文本文件，把它放在用户主目录的下面。

```
cd ~
curl -O https://raw.githubusercontent.com/airbnb/javascript/master/README.md
```

查看一下这个文件的大小：

```
ls -lh
```

返回：

```
-rw-r--r--. 1 wanghao ninghao 107K May 22 11:55 README.md
```

显示下载的 README.md 这个文件的大小是 107K。

**2**，创建一个打包文件。执行：

```
tar -cf airbnb-js.tar README.md
```

airbnb-js.tar 这个打包文件里面的内容就是 README.md。查看一下文件大小：

```
[wanghao@localhost ~]$ ls -lh
total 220K
-rw-r--r--. 1 wanghao ninghao 110K May 22 12:00 airbnb-js.tar
-rw-r--r--. 1 wanghao ninghao 107K May 22 11:55 README.md
```

你会发现打包文件 airbnb-js.tar 的体积比 README.md 大一点。

**3**，解包。先把 README.md 删除掉：

```
rm -rf README.md
```

再去解包 airbnb-js.tar，执行：

```
tar -xf airbnb-js.tar
```

查看一下目录下的列表，README.md 又出现了。

**4**，压缩包。先删除掉 airbnb-js.tar：

```
rm -rf airbnb-js.tar
```

把 README.md 放到一个目录的下面：

```
mkdir airbnb-js
mv README.md airbnb-js/
```

创建解压包：

```
tar -czf airbnb-js.tar.gz airbnb-js
```

然后查看生成的解压包的体积：

```
ls -lh
```

返回：

```
drwxr-xr-x. 2 wanghao ninghao  23 May 22 12:07 airbnb-js
-rw-r--r--. 1 wanghao ninghao 28K May 22 12:07 airbnb-js.tar.gz
```

观察 airbnb-js.tar.gz 这个文件的大小。现在它是 28K，包里包含的 README.md 文件的大小是 107K，这种生成包的时候我们用了 z 选项，也就是会用 gzip 压缩文件，所以生成的包的体积会小一些。

**5**，解压包。先删除目录 airbnb-js，执行：

```
rm -rf airbnb-js
```

再解压 airbnb-js.tar.gz，执行：

```
tar -xzf airbnb-js.tar.gz
```

查看目录下的列表，airbnb-js 这个目录又会出现了，它是解压 airbnb-js.tar.gz 以后生成的目录。

