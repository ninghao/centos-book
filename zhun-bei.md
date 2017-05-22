# 准备

学习 CentOS，你需要一台安装了这种操作系统的机器，可以是一台真正的服务器，也可以是在本地电脑上创建的一台 CentOS 的虚拟机。《[Vagrant](https://vagrant.ninghao.net/)》这本书里介绍了在本地管理虚拟机的方法。

### 虚拟机

创建一台 CentOS 系统的虚拟机。

```
cd ~/desktop
mkdir ninghao-centos
cd ninghao-centos
vagrant init centos/7
vagrant up
vagrant ssh
```

上面这些步骤的详细说明，你可以参考 《[Vagrant](https://vagrant.ninghao.net)》这本书。

