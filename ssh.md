# ssh

ssh 的基本使用形式：

```
ssh 用户@主机
```

用户是你登录服务器用的用户名，@ 符号右边是服务器的主机名或者 IP 地址。

比如我要连接之前创建的 CentOS 系统的虚拟机，这台机器有个 IP 地址是 192.168.33.20，连接到它可以这样：

```
ssh vagrant@192.168.33.20
```

vagrant 是我连接服务器的时候使用的用户，这个用户应该提前在服务器上创建好。服务器一开始一般只有一个超级管理员叫 root，第一次登录到服务器，通常你就需要使用 root 身份登录。

第一次连接到某台服务器，可能会出现这样的提示：

```
The authenticity of host '192.168.33.20 (192.168.33.20)' can't be established.
ECDSA key fingerprint is SHA256:YdSnCe4b5efT7xy5pOX4lG/dJBPVVBNHaj7RR1cAm5o.
Are you sure you want to continue connecting (yes/no)?
```

输入 yes ，然后回车。可能会提示你输入跟 vagrant 用户对应的密码，也可能会提示：

```
Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
```

出现上面的 Permission denied 提示是因为服务器验证用户身份用的是密钥的方式。但服务器期待的密钥跟我们提供给它的不一样，所以就会出现被拒绝的这个提示。下面再了解一下两种验证用户身份的方法。

## 验证

用 SSH 远程登录到 Linux 服务器主要有两种验证用户身份的方法，一种是用户名 + 密码，另一种是使用密钥。在服务器端，可以通过配置 SSH 服务，让用户只能使用某种方法验证身份。

### 用户名 + 密码

SSH 连接的时候你需要输入跟连接时使用的那个用户对应的密码才能登录到服务器。比如你连接时用的是 root 用户，确定连接以后，会提示你输入 root 用户的密码，验证成功，才让你登录到服务器。要注意的是，服务端有可能会禁用这种用户名 + 密码的验证身份的方式。更安全的做法是使用密钥的方式验证用户身份。

### 密钥

你在本地电脑上用 ssh-keygen 生成一对密钥，然后配置服务器，把你生成的密钥里的公钥（public key）告诉服务器。这样再用 SSH 连接到服务器的时候，只要验证密钥成功就让你登录，不再需要输入跟用户对应的密码了。

在《[工作流](https://workflow.ninghao.net/)》里我已经解释了什么是 [ssh 密钥](https://workflow.ninghao.net/ssh-key.html)，怎么生成。现在你只需要用一下生成的这对密钥。先输出在自己电脑上生成的公钥：

```
cat ~/.ssh/id_rsa.pub
```

输出的东西类似下面这样：

```
ssh-rsa AAAAB3NzaC1yc2EAAAAOAQABLAABAQCVDSJo3/9aATdCWph8Utst6G/0ngYKoNVxdf35CU07wly8Doz+VnEIn1SpRQcPgr6LoXg9Cih69CGNIFXcc3dOqIyYwwCeSHFwh/wFV9NSN8XBeZkjkd1OAFIybGLxjLElKEOjKVGwfP2hYh4pZAXUpo2pdBOfPZlaGm4vI4t9EsQogrKgXPv+g90JXoVxFngYGMHUsatY0s3+nRsz6RzfAWqFyvv7+xaZ67sFRkLF2s2b0XAW7UBZyk8uRcP3jc2Fiw/iGdbt9Dp/60LESfeZC25iO7lKcfrXZD4IDp5coZhO1nx1lRNL/5SovwMb+tWNt4xwUJjG62/F+Y5drFTn wanghao@wanghaodeMacBook.local
```

复制一下输出的内容，然后再登录到 Linux 服务器。登录以后，编辑一下当前登录的用户的主目录下面的 .ssh/authorized\_keys ，如果没这个文件，你可以创建一个。然后把输出的在自己电脑上生成的公钥内容插入到这个 authorized\_keys 文件里面。这样下次你再使用服务器上的这个用户登录服务器的时候，就不再需要输入密码了。

如果你想对服务器上的其它用户也使用这种密钥的方式登录，你再做同样的配置，也就是把公钥放到这个用户主目录下的 .ssh/authorized\_keys 文件里面。

比如 wanghao 用户的 authorized\_keys 文件就应该是在：

```
/home/wanghao/.ssh/authorized_keys
```



