@[TOC]()
参考：[开启SSH服务](https://www.itcoder.tech/posts/how-to-enable-ssh-on-ubuntu-20-04/)；[密钥登陆](https://linuxize.com/post/how-to-setup-passwordless-ssh-login/?spm=a2c6h.12873639.0.0.7539785d7rHjKu#setup-ssh-passwordless-login) 

layout: post
title:  "Ubuntu20.04 开启SSH服务并启用密钥登陆)"
date:   

categories: jekyll update
---

# 启用SSH服务

Ubuntu自带有openssh-client，仅需安装服务端

## 安装openssh-server软件包


```
sudo apt update
sudo apt install openssh-server

```

## 验证 SSH 是否正在运行
安装完成，SSH 服务将会被自动启动。
```
sudo systemctl status ssh
```

## 打开 SSH 端口

```
sudo ufw allow 22
```

# 局域网内连接到SSH服务器


```
ssh username@ip_address
```
ip_address`修改成你安装了 SSH 的 Ubuntu 机器的 IP 地址
# 开启SSH密钥登陆
## 检查现有的SSH密钥对

```
ls -al ~/.ssh/id_*.pub
```
如果看到，No such file or directory或者no matches found这意味着您没有SSH密钥，则可以继续下一步并生成一个新密钥。
如果存在现有密钥，则可以使用这些密钥并跳过下一步，或者备份旧密钥并生成一个新密钥。
## 生成一个新的SSH密钥对

```
ssh-keygen -t rsa -b 4096 -C "your_email@domain.com"
```
按下Enter以接受默认文件位置和文件名：

```
Enter file in which to save the key (/home/yourusername/.ssh/id_rsa):
```
ssh-keygen工具将要求您输入安全密码。是否要使用密码取决于您自己，如果您选择使用密码，您将获得额外的安全保护。在大多数情况下，开发人员和系统管理员在不使用密码的情况下使用SSH，因为它们对于全自动流程很有用。如果您不想使用密码短语，请按Enter。

```
Enter passphrase (empty for no passphrase):
```
验证生成了SSH密钥

```
ls ~/.ssh/id_*
```
结果应如下

```
/home/yourusername/.ssh/id_rsa /home/yourusername/.ssh/id_rsa.pub
```
## 复制公钥
将公钥复制到服务器的最简单方法是使用名为的命令ssh-copy-id

```
ssh-copy-id remote_username@server_ip_address
```
系统将提示您输入remote_username密码：

```
remote_username@server_ip_address's password:
```
##验证使用SSH密钥登录到服务器

```
ssh remote_username@server_ip_address
```
# 禁用SSH密码登陆
要为服务器添加额外的安全性，防止遭到暴力破解，您可以禁用SSH的密码身份验证。在禁用SSH密码身份验证之前，请确保您可以不使用密码登录服务器，并且使用sudo特权登录的用户。
## 使用具有sudo特权或root用户的SSH密钥登录到远程服务器

```
ssh sudo_user@server_ip_address
```
## 打开SSH配置文件/etc/ssh/sshd_config，搜索以下指令并进行如下修改：

```
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM no
```
## 保存文件并重启SSH服务
在Ubuntu或Debian服务器上，运行以下命令

```
sudo systemctl restart ssh
```

