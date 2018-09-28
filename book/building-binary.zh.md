
## 安装预编译二进制文件

有许多友好的人和组织将curl和libcurl的二进制包组合在一起,并使它们可供下载.

许多操作系统提供了一个"包储存库",其中包含软件包供您安装.从[cURL下载页面](https://curl.haxx.se/download.html)您还可以遵循普通操作系统的普通二进制包的链接.

## 从包库中安装

curl和libcurl已经存在很长时间了,大多数Linux发行版、BSD版本和其他\*nix版本都有允许您安装curl包的包存储库.

最常见的描述如下.

### 恰当的

`apt`是在Debian Linux和Ubuntu Linux发行版和衍生品上安装预构建包的工具.

要安装cURL命令行工具,通常只需要

```
apt install curl
```

然后,确保安装了依赖项,通常LICORL也被安装为单独的包.

如果希望根据libcurl构建应用程序,则需要安装一个开发包来获得include头和一些附加文档等.然后您可以选择具有您喜欢的TLS后端的libcurl:

```
apt install libcurl4-openssl-dev
```

或

```
apt install libcurl4-nss-dev
```

或

```
apt install libcurl4-gnutls-dev
```

### 百胜

使用ReHAT Linux和CENTOS Linux衍生工具,您使用`yum`安装软件包.用以下命令安装命令行工具:

```
yum install curl
```

您可以安装libcurl开发包(包括包含文件和一些文档等):

```
yum install curl-devel
```

(Linux系统的Redhat家族通常使用CRS来构建使用NSS作为TLS后端).

### 尼克斯

[尼克斯](https://nixos.org/nix/)包管理器默认为NIXOS发行版,但它也可以用于任何Linux发行版.

为了安装命令行工具:

```
nix-env -i curl
```

### 自酿啤酒

[自酿啤酒](https://brew.sh/)是一个OSX包管理器.它不是默认的,但是可以很容易地安装它.

安装命令行工具:

```
brew install curl
```

### 赛文

TBD

## Windows的二进制包

TBD
