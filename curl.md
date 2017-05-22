# curl

curl 可以通过各种形式的地址传输数据。

## 选项

curl 命令有很多选项，最小化原则，暂时你只需要认识两个选项。

* o（--output），写入到文件
* O（--remote-name），写入到文件（用远程文件的名字）

## 输出

输出地址内容：

```
curl 地址
```

## 下载

把地址内容下载到某个文件里：

```
curl -o 文件名 地址
```

如果想用远程地址是文件，把文件下载下来，文件名使用远程文件的名字，可以这样：

```
curl -O 文件地址
```

## 练习

**1**，输出一个远程页面内容，执行：

```
curl https://ninghao.net
```

会返回宁皓网的首页内容。

**2**，把页面内容下载到某个文件，执行：

```
curl -o index.html https://ninghao.net
```

会把宁皓网的首页代码保存到一个叫 index.html 的文档里。

**3**，下载远程文件，执行：

```
curl -O https://raw.githubusercontent.com/airbnb/javascript/master/README.md
```

会把远程文件 README.md，下载下来，文件名仍然是 README.md。

