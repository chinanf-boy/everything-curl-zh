
# FTP高级用法

## FTP目录列表

通过确保URL以`/`尾随斜线结束,可以列出远程FTP目录.如果URL以斜杠结尾,cURL将假定它是要列出目录. 若它实际上不是一个目录,那么你很可能会得到一个错误.

```
curl ftp://ftp.example.com/directory/
```

使用FTP时，对于使用标准FTP命令`LIST`的此类命令，返回的目录输出没有标准语法. 列表通常是可读的,并且完全可以理解,但是不同的服务器会用稍微不同的方式返回列表.

获得目录中所有的名称的列表, 从而避免常规目录列表的特殊格式的一种方法是告诉curl `--list-only`(或者只是`-l`), 然后cURL会变成发出`NLST`FTP命令:

```
curl --list-only ftp://ftp.example.com/directory/
```

NLST有它自己的怪癖,因为一些FTP服务器在NLST的响应中只列出实际*文件*,它们不包括目录和符号链接!

## 用FTP上传

要上传到FTP服务器,需要在URL中指定整个目标文件路径和名称,并且用`-T,
--upload-file`指定要上传的本地文件名. 可选地,使用斜线结束目标URL,然后本地路径中的文件组件将由curl附加并用作远程文件名.

像:

```
curl -T localfile ftp://ftp.example.com/dir/path/remote-file
```

或使用本地文件名作为远程名称:

```
curl -T localfile ftp://ftp.example.com/dir/path/
```

cURL`-T`参数也支持[通配符-globbing](cmdline-globbing.zh.md), 您可以选择轻松地上传一系列或一系列文件:

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
