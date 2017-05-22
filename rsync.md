# rsync

rsync（Remote Sync）。你有两个目录想保持同步，可以使用 rsync 。要同步的目录可以是本地之间的目录，也可以是本地与远程之间的目录。

```
rsync 选项 源 目标
```

## 选项

* r（recursive），递归复制，复制同步的文件不保留文件的权限，创建与修改时间。
* a（archive），存档模式，可以递归复制，保留文件替身，复制同步的文件会保留文件的拥有者，用户名，时间，权限。
* z（compress），压缩传输，传输文件时会压缩文件。
* n（dry-run），假装同步，看看都有什么东西可以同步的，不会真正执行同步。
* h（human-readable），用人类都看懂的方式显示数字。
* P（progress），进度。

## 递归同步

r（recursive），用递归模式同步，可以把源目录下包含的所有内容同步到目标目录。递归模式同步不保留文件属性。

```
rsync -r 源 目标
```

## 存档模式

a（archive），存档模式同步可以递归同步，也可以保留文件属性，比如文件的拥有者，用户组，修改时间等等。

```
rsync -a 源 目标
```

## 显示进度

P，同步时可以显示进度。

```
rsync -a -P 源 目标
```

这样写也行：

```
rsync -aP 源 目标
```

## 删除

delete ，执行同步之后在目标目录上会删除掉在源上被删除掉的内容。

```
rsync -a -P --delete 源 目标
```

## 远程

```
rsync -a -P 用户@主机:源 目标
```

## 练习

**1**，创建两个目录，在其中一个目录里添加几个文件。

```
mkdir app1 app2
touch app2/f{1..3}
```

查看 app2，里面有三个空白的文件：

```
$ ls -l app2

total 0
-rw-r--r--. 1 wanghao ninghao 0 May 22 13:27 file1
-rw-r--r--. 1 wanghao ninghao 0 May 22 13:27 file2
-rw-r--r--. 1 wanghao ninghao 0 May 22 13:27 file3
```

**2**，同步本地目录。下面把 app2 目录下的文件同步到 app1 里面，用一个 -r 选项可以递归同步。执行：

```
rsync -r app2/ app1
```

注意 app2 后面有个 / ，表示要同步的源是 app2 这个目录下面的东西，并不是 app2 目录本身。

同步完成以后，查看一下 app1 下面的内容：

```
$ ls -l app1

total 0
-rw-r--r--. 1 wanghao ninghao 0 May 22 13:38 file1
-rw-r--r--. 1 wanghao ninghao 0 May 22 13:38 file2
-rw-r--r--. 1 wanghao ninghao 0 May 22 13:38 file3
```

app1 里面已经包含了在 app2 下面的所有内容。观察文件的修改时间，你会发现跟 app2 下面的源文件的修改时间是有变化的。app1 下面的内容的修改时间是同步完成以后的时间。

删除 app1 下面的所有内容，执行：

```
rm -rf app1/*
```

用 root 用户的身份再执行一下同步：

```
sudo rsync -r app2/ app1
```

然后查看 app1 下的内容：

```
$ ls -l app1

total 0
-rw-r--r--. 1 root root 0 May 22 13:41 file1
-rw-r--r--. 1 root root 0 May 22 13:41 file2
-rw-r--r--. 1 root root 0 May 22 13:41 file3
```

这次 app1 下的内容跟 app2 下的源内容相比，拥有者，所属用户组，修改时间，这些东西都不一样了。

**3**，用存档模式同步。存档模式可以保留文件属性，比如拥有者，修改时间等等，需要用一个 -a 选项。先把 app1 里的东西删除掉，执行：

```
rm -rf app1/*
```

然后用存档模式同步：

```
sudo rsync -a app2/ app1
```

查看同步之后的 app1 里的内容：

```
$ ls -l app1

total 0
-rw-r--r--. 1 wanghao ninghao 0 May 22 13:27 file1
-rw-r--r--. 1 wanghao ninghao 0 May 22 13:27 file2
-rw-r--r--. 1 wanghao ninghao 0 May 22 13:27 file3
```

观察文件属性。虽然同步时我们用了 root 用户身份，但同步之后的文件跟源文件的属性是一样的，一样的拥有者，用户组，一样的修改时间。

**4**，显示同步进度。先删除 app1 下的内容：

```
rm -rf app1/*
```

显示同步进度需要用一个 P 选项，执行：

```
rsync -a -P app2/ app1
```

返回类似的东西：

```
sending incremental file list
./
file1
           0 100%    0.00kB/s    0:00:00 (xfer#1, to-check=2/4)
file2
           0 100%    0.00kB/s    0:00:00 (xfer#2, to-check=1/4)
file3
           0 100%    0.00kB/s    0:00:00 (xfer#3, to-check=0/4)

sent 193 bytes  received 72 bytes  530.00 bytes/sec
total size is 0  speedup is 0.00
```

**5**，同步有变化的文件。rsync 只会同步有变化的文件，先执行一下同步：

```
rsync -a -P app2/ app1
```

这次没同步任何文件，因为 app2 与 app1 里的内容完全是一样的。再修改一下 app2 里的 file1 文件里的内容：

```
echo 'hello' >> app2/file1
```

再执行一下同步，这次就会把 app2 下面发生变化的 file1 同步到 app1 下面了。

**6**，同步时删除不存在的文件。使用 `--delete` 选项，先删除 app2 下面的 file2 这个文件：

```
rm -rf app2/file2
```

然后执行同步，使用一个 delete 选项：

```
rsync -a -P --delete app2/ app1
```

返回：

```
sending incremental file list
./
deleting file2

sent 77 bytes  received 15 bytes  184.00 bytes/sec
total size is 6  speedup is 0.07
```

同步完成以后，查看 app1 下的内容列表，你会发现 file2 也不见了。

7，把远程目录同步到本地目录。



