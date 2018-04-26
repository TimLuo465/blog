---
title: yarn日常指南
date: 2017-08-23 20:30:47
tags:
    - node
    - 日常指南
---
#### 01. 安装

##### 1. 利用Chocolatey安装

- ``cmd.exe``安装``Chocolatey``，执行以下命令:

    ```
    @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
    ```
- 执行``choco install yarn`` 安装 ``yarn``
- [Chocolatey官方下载地址](https://chocolatey.org/install)

##### 2. 利用Scoop安装

- ``Powershell 3+`` 安装``Scoop``, 执行以下命令:

    ``iex (new-object net.webclient).downloadstring('https://get.scoop.sh')``

- [Scoop官方下载地址](http://scoop.sh/)

##### 3. ``.msi``安装包的方式

- [下载地址](https://github-production-release-asset-2e65be.s3.amazonaws.com/49970642/52aa59e0-1ae7-11e8-96d7-90881daa5309?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20180412%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20180412T034642Z&X-Amz-Expires=300&X-Amz-Signature=824354f8d347d362e49d83d02bf13169784acc24b47d9d1d313263d5c108a7a5&X-Amz-SignedHeaders=host&actor_id=11442713&response-content-disposition=attachment%3B%20filename%3Dyarn-1.5.1.msi&response-content-type=application%2Foctet-stream)

>安装完成后在``cmd``中运行``yarn --version``，如果出现无法找到``yarn``命令之类的，那可能是环境变量未配置导致的，将``yarn``的bin目录配置到系统变量中即可。

#### 02. 国内镜像

```
// 获取当前的源
yarn config get registry
// 设置淘宝镜像源
yarn config set registry https://registry.npm.taobao.org
```

#### 03. 使用

- 查看版本

    ```yarn --version```
- 初始化新项目
    
    ```yarn init```
- 添加依赖包

    ```
    yarn add [package]
    yarn add [package]@[version]
    yarn add [package]@[tag]
    ```
- 分别将依赖添加到 ``devDependencies``、``peerDependencies`` 和 ``optionalDependencies``：

    ``` 
    yarn add [package] --dev
    yarn add [package] --peer
    yarn add [package] --optional
    ```
- 升级依赖包

    ```
    yarn upgrade [package]
    yarn upgrade [package]@[version]
    yarn upgrade [package]@[tag]
    ```
- 移除依赖包

    ```yarn remove [package]```
- 安装项目的全部依赖
    ```
    yarn 或 yarn install
    ```
- 全局操作

    ```
    yarn global <add/bin/list/remove/upgrade> [--prefix]
    ```
#### 参考地址

- [yarn中文](https://yarnpkg.com/zh-Hans/)
- [npm与yarn的区别](http://web.jobbole.com/88459/)