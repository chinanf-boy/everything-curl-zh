
### HTTP协议基础

(假设您已经阅读了[网络与协议](protocols.zh.md)或已熟悉协议.

HTTP是一种易于学习的协议.客户端连接到服务器,它总是主动地发送请求,并接收响应的客户端.请求和响应都由标题和正文组成. 在两个方向上可以有很少或大量的信息.

客户端发送的HTTP请求从请求行开始,接着是报头,然后可选地是一个正文.最常见的HTTP请求可能是请求服务器返回特定资源的GET请求,并且该请求不包含主体.

当客户端连接到"example.com",并请求"/"资源时,它发送一个没有请求主体的GET:

```
GET / HTTP/1.1
User-agent: curl/2000
Host: example.com
```

服务器可以用下面的内容做出响应,使用响应头和响应体(hello).响应中的第一行还包含响应代码, 和服务器支持的特定版本:

```
HTTP/1.1 200 OK
Server: example-server/1.1
Content-Length: 5
Content-Type: plain/text

hello
```

如果客户端，用一个小请求体发送一个请求(hello),它可以是这样的:

```
POST / HTTP/1.1
Host: example.com
User-agent: curl/2000
Content-Length: 5

hello
```

服务器总是响应HTTP请求,除非有问题.

### 转换为请求的URL

因此,当给HTTP客户端给予一个URL操作时,该URL随后被使用、分拣,并且在向服务器发出的请求中，这部分被不同地使用.让我们举一个URL例子:

```
https://www.example.com/path/to/file
```

-   **https** : 意味着curl将使用TLS到远程端口443(这是在URL中没有指定使用时的默认端口号).

-   **www.example.com** : curl将解析为连接到的一个或多个IP地址的主机名. 在HTTP请求中,这个主机名使用`Host:`标题.

-   **/path/to/file** : 在HTTP请求中使用,以告诉服务器curl想要获取的确切文档/资源.

### --path-as-is

URL的路径部分是, 从主机名后面的第一个斜杠开始的部分,并在URL的末尾或在"?"或者'#'(大致).

如果包含子串包括`/../`或`/./`在路径中,curl会在路径被发送到服务器之前自动压缩它们, 这是由标准以及这些字符串在本地文件系统中如何工作决定的. 我们来分析一下这个`/../`序列，删除前一路径节,如`/hello/sir/../`变成了`/hello/`和`/./`。又如`/hello/./sir/`会变成`/hello/sir/`.

发送到服务器之前，*阻止*cURL将这些魔法序列粉碎,从而允许它们通过.可使用`--path-as-is`选项.
