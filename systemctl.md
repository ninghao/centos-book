# systemctl

systemctl 是管理服务用的工具。

## 启动服务

启动服务可以使用 start 命令，像这样：

```
sudo systemctl start 服务
```

注意启动服务需要使用管理员权限，所以如果当前登录的用户不是管理员，需要在命令前面加上 sudo 。

## 停止服务

停止服务用的是 stop 命令：

```
sudo systemctl stop 服务
```

## 服务状态

确定服务的运行状态用的是 status ：

```
sudo systemctl status 服务
```

## 重启与重载

重启就是关闭服务再打开服务，重载是重新加载服务。一般对服务做了配置以后，可以选择重载，这样不需要重启服务，就可以让配置生效。

重启：

```
sudo systemctl restart 服务
```

重载：

```
sudo systemctl reload 服务
```

## 启用与禁用

让服务开机自启用可以用 enable 启用一下这个服务，这样每次重启服务器以后，这个服务会自动启动。也就是你不需要手工再用 start 命令去启动它了。禁用服务就是让服务不开机自启动。

启用：

```
sudo systemctl enable 服务
```

禁用：

```
sudo systemctl disable 服务
```

## 练习

安装一个 NGINX Web 服务器，然后练习一下 systemctl 相关的命令。

1，先安装一下 NGINX，使用 Yum 包管理工具，执行：

```
sudo yum install nginx -y
```

完成以后，先查看一下 NGINX 服务的状态：

```
sudo systemctl status nginx
```

返回的是：

```
● nginx.service - nginx - high performance web server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
     Docs: http://nginx.org/en/docs/
```

注意 Active 那里，显示的是 inactive（dead），这就表示这个服务当前还没有运行。

2，启动 NGINX 服务，执行：

```
sudo systemctl start nginx
```

然后再次查看 NGINX 的运行状态，这次会返回：

```
● nginx.service - nginx - high performance web server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2017-05-22 06:06:16 UTC; 5s ago
     Docs: http://nginx.org/en/docs/
  Process: 4896 ExecStart=/usr/sbin/nginx -c /etc/nginx/nginx.conf (code=exited, status=0/SUCCESS)
  Process: 4895 ExecStartPre=/usr/sbin/nginx -t -c /etc/nginx/nginx.conf (code=exited, status=0/SUCCESS)
 Main PID: 4900 (nginx)
   CGroup: /system.slice/nginx.service
           ├─4900 nginx: master process /usr/sbin/nginx -c /etc/nginx/nginx.conf
           └─4901 nginx: worker process

May 22 06:06:16 localhost.localdomain systemd[1]: Starting nginx - high performance web server...
May 22 06:06:16 localhost.localdomain nginx[4895]: nginx: the configuration file /etc/nginx/nginx.conf sy... ok
May 22 06:06:16 localhost.localdomain nginx[4895]: nginx: configuration file /etc/nginx/nginx.conf test i...ful
May 22 06:06:16 localhost.localdomain systemd[1]: Failed to read PID from file /run/nginx.pid: Invalid argument
May 22 06:06:16 localhost.localdomain systemd[1]: Started nginx - high performance web server.
Hint: Some lines were ellipsized, use -l to show in full.
```

这次 Active 就变成了 active（running），表示服务正在运行。

3，启用 NGINX 服务，让它可以开机自启动，执行：

```
sudo systemctl enable nginx
```

上面这个动作只需要做一次。执行以后会返回：

```
Created symlink from /etc/systemd/system/multi-user.target.wants/nginx.service to /usr/lib/systemd/system/nginx.service.
```

4，重新启动一下服务器，然后再查看 NGINX 的运行状态。没什么问题的话，它的状态应该是 active（running）。

5，查看系统进程，看一下运行 NGINX 工作进程的用户是谁。执行：

```
ps aux | grep nginx
```

返回类似的内容：

```
nginx     4901  0.0  0.3  46184  1888 ?        S    06:06   0:00 nginx: worker process
```

一开始的 nginx 就是 nginx 的 worker process 进程的用户。

6，修改 NGINX 的配置文件，重新设置运行 nginx 的用户。先编辑文件：

```
sudo vi /etc/nginx/nginx.conf
```

把：

```
user  nginx;
```

改成：

```
user  www-data;
```

保存 NGINX 的配置文件。创建一个用户：

```
sudo useradd www-data
```

然后重载一下 NGINX 服务：

```
sudo systemctl reload nginx
```

再次查看系统进程，看一下运行 nginx 的用户是谁。你会发现，这次会是 www-data 这个用户。

