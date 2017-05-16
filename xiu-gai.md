# 编辑

mv（move）命令可以移动或重命名目录与文件。

把之前创建的 README.md 放到 awesome-project 目录的下面，可以执行：

```
mv README.md awesome-project/
```

再重命名一下 awesome-project  这个目录：

```
mv awesome-project ninghao-project
```

**编辑文件**

vi 可以在命令行下编辑文件。

```
vi ninghao-project/README.md
```

这样会打开要编辑的文件，使用方向键可以移动光标。

想要编辑按一下小 i ，这样会进入到 vi 编辑器的编辑模式，修改完成以后，按 esc 可以退出编辑模式。搜索文件里的内容可以按 / ，然后输入要搜索的关键词，n 继续查找，N 往前查找。输入 :wq ，可以保存对文件的修改。



