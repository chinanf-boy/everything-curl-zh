
## 如何遍历目录

当执行FTP命令遍历远程文件系统时,有几种不同的方式可以继续进行curl以到达目标文件,即用户想要传输的文件.

### 多小波分析

CURL可以为文件树层次结构中的每个单独目录更改一个目录(CWD)命令.如果完整路径是`one/two/three/file.txt`那个方法意味着做三`CWD`请求之前的命令`file.txt`文件传输.因此,如果路径是多层次的,那么该方法会产生相当多的命令.这种方法是由早期规范(RFC 1738)规定的,它是如何cURL默认行为:

```
curl --ftp-method multicwd ftp://example.com/one/two/three/file.txt
```

这等于这个FTP命令/响应序列(简化):

```
> CWD one
< 250 OK. Current directory is /one
> CWD two
< 250 OK. Current directory is /one/two
> CWD three
< 250 OK. Current directory is /one/two/three
> RETR file.txt
```

### nocwd

与每个目录部分做一个CWD相反的是根本不改变目录.此方法要求服务器同时使用整个路径,因此非常快.有时服务器有问题,它不是纯粹符合标准的:

```
curl --ftp-method nocwd ftp://example.com/one/two/three/file.txt
```

这等于这个FTP命令/响应序列(简化):

```
> RETR one/two/three/file.txt
```

### 单选

这是其他两种FTP方法之间的中间点.这使得单一`CWD`命令到目标目录,然后请求给定的文件:

```
curl --ftp-method singlecwd ftp://example.com/one/two/three/file.txt
```

这等于这个FTP命令/响应序列(简化):

```
> CWD one/two/three
< 250 OK. Current directory is /one/two/three
> RETR file.txt
```
