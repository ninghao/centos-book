# 虚拟机

学习 CentOS 操作系统，你得有一台安装了这种操作系统的机器。一般服务器会用到这种操作系统，比如你购买了一台阿里云的 ECS 服务器，可以为服务器选择使用 CentOS 这种操作系统。然后在本地用命令行连接到服务器以后，你就可以在上面去做练习了。

还有一种方法，你可以在本地创建一台 CentOS 系统的虚拟机，然后在这台虚拟机上去练习使用 CentOS。练习以后，你可以把这台虚拟机销毁掉，在本地虚拟机上学习 CentOS，是最省钱，最简单，最安全的方法。

> 创建虚拟机，参考宁皓网的《Vagrant》这本书，或者相关的视频课程。

打开命令行，先在桌面上创建一个目录：

```
cd ~/desktop
mkdir ninghao-centos
cd ninghao-centos
```

初始化一台虚拟机，指定虚拟机使用 centos/7 这个镜像：

```
vagrant init centos/7
```

编辑目录下的 Vagrantfile，配置一上虚拟机的私有网络，文件里的内容如下：

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.network "private_network", ip: "192.168.33.20"
end
```

启动虚拟机：

```
vagrant up
```

连接到虚拟机：

```
vagrant ssh
```



