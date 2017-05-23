---
title: github没有记录contributions之迷
date: 2017-05-23 23:59:46
tags:
---
#### 起因
---
由于重装了系统，导致git的一些配置丢失。项目提交到github上后，在profile contributions graph中对应的contributions也没有显示出来。找了下原因，将git重新配置了一番，问题迎刃而解。<!-- more -->

#### username和email的配置
---
```
git config user.name xxx
git config user.email xxx@xxx.xxx
// 查看的话 去掉值就好 如
git config user.name
```
#### SSH Key的配置
---
在Git Bash上输入如下命令:

```
ssh-keygen -t rsa -C "xxx@xxx.xxx"
//然后三下回车(提示输入密码可以不用管)
```
完成之后，会提示你id_rsa和id_rsa.pub两个文件生成成功
1. 在**C:\Users\Administrator\\.ssh** 下找到**id_rsa.pub**
2. 登录github，打开[Github Settings](https://github.com/settings/keys)，新建**SSH Key**
3. 将**id_rsa.pub**中的密钥复制到新建的**SSH key**中

#### 还是无法显示contributions

看看[官方解释](https://help.github.com/articles/why-are-my-contributions-not-showing-up-on-my-profile/)的解释，这种情况一般是由于username和email配置不正确导致，仔细看下[github的个人信息](https://github.com/settings/profile)核实一下就好了。