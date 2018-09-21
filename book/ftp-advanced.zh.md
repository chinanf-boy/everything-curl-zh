
# 更先进的FTP

## FTP目录列表

通过确保URL以尾随斜线结束,可以列出具有cURL的远程FTP目录.如果URL以斜杠结尾,cURL将假定它是要列出的目录.如果它实际上不是一个目录,那么你很可能会得到一个错误.

```
curl ftp://ftp.example.com/directory/
```

对于FTP,对于使用标准FTP命令的这类命令返回的目录输出没有标准语法`LIST`. 列表通常是可读的,并且完全可以理解,但是您将看到不同的服务器将以稍微不同的方式返回列表.

获得目录中所有名称的列表,从而避免常规目录列表的特殊格式的一种方法是告诉curl`--list-only`(或者只是`-l`)cURL然后发出`NLST`相反,FTP命令:

```
curl --list-only ftp://ftp.example.com/directory/
```

NLST有它自己的怪癖,因为一些FTP服务器只列出实际.*文件夹*在对NLST的响应中,它们不包括目录和符号链接!

## 用FTP上传

要上传到FTP服务器,需要在URL中指定整个目标文件路径和名称,并且指定要上传的本地文件名`-T,
--upload-file`. 可选地,使用斜线结束目标URL,然后本地路径中的文件组件将由curl附加并用作远程文件名.

像:

```
curl -T localfile ftp://ftp.example.com/dir/path/remote-file
```

或使用本地文件名作为远程名称:

```
curl -T localfile ftp://ftp.example.com/dir/path/
```

cURL也支持[通配符](cmdline-globbing.md)在-t参数中,您可以选择轻松地上传一系列或一系列文件:

```
curl -T image[1-99].jpg ftp://ftp.example.com/upload/
```

或

```
curl -T '{Huey,Dewey,Louie}.jpg' ftp://ftp.example.com/nephews/
```

## 自定义FTP命令

TBD

## FTPS

TBD

## 常见的FTP问题

TBD
