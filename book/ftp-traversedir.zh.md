
## 如何遍历目录

当执行FTP命令遍历远程文件系统时,curl有几种不同的方式可以继续进行, 以到达即用户想要传输的目标文件.

### multicwd

curl可以在文件树层次结构中的每个单独目录中，都运行一个更改目录(CWD)命令. 如完整路径是`one/two/three/file.txt`，那方法意味着在请求`file.txt`文件传输之前, 做了三次`CWD`请求的命令. 因此,如果路径是多层次的,那么该方法会产生相当多的命令. 这种方法是由早期规范(RFC 1738)规定的. cURL默认行为:

```
curl --ftp-method multicwd ftp://example.com/one/two/three/file.txt
```

这等于这个FTP 命令/响应 序列(简化):

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

与CWD相反的是根本不改变目录.此方法要求服务器同时使用整个路径,因此非常快.但有时服务器会有问题,因为它不符合纯粹的标准:

```
curl --ftp-method nocwd ftp://example.com/one/two/three/file.txt
```

这等于这个FTP命令/响应序列(简化):

```
> RETR one/two/three/file.txt
```

### singlecwd

在以上两种FTP方法之间的是 singlecwd .这使得单一`CWD`命令到目标目录,然后请求给定的文件:

```
curl --ftp-method singlecwd ftp://example.com/one/two/three/file.txt
```

这等于这个FTP命令/响应序列(简化):

```
> CWD one/two/three
< 250 OK. Current directory is /one/two/three
> RETR file.txt
```
