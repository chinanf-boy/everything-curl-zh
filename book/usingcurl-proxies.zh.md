## 代理

代理是一种代表客户,为客户做一些事情的机器或软件.

您还可以将其看作一个位于, 您和想要与之一起工作的服务器之间的中间人,一个您连接的中间人,而不是实际的远程服务器.您要求代理为您执行所需的操作,然后它将运行并执行该操作,然后将数据返回给您.

有几种不同类型的代理,我们将在本节中进一步列出和讨论它们.

### 发现你的代理

有些网络设置成需要代理,以便您访问 Internet 或者您感兴趣的特定网络.由于策略或技术原因,运行网络的人员和管理层在您的网络上介绍了代理的使用.

在网络空间中,有几种自动检测代理,以及如何连接到代理的方法,但这些方法都不是真正通用的,curl 也不支持这些方法.此外,当您通过代理与外部世界通信时,这通常意味着您必须对代理很信任,因为代理将能够查看和修改您发送或通过的所有非安全网络流量. 这种信任是不轻易自动承担的.

如果检查浏览器的网络设置,有时在高级设置选项卡下,可以了解浏览器配置为使用什么代理.当你使用 cURL 时,很大机会你可以使用同一个或多个.

TBD:如何在 Firefox 和 Chrome 中找到代理设置的截图?

### PAC

一些网络环境提供了几种不同的代理,这些代理应该在不同的情况下使用,以及浏览器支持的灵活自定义的处理方式.这被称为"代理自动配置"或 PAC.

PAC 文件包含一个 JavaScript 函数,该函数决定一个给定的网络连接(URL)应该使用哪个代理,即使它根本不应该使用代理.浏览器通常从本地网络上的 URL 读取 PAC 文件.

由于 curl 没有 JavaScript 功能,所以 curl 不支持 PAC 文件.如果您的浏览器和网络使用 PAC 文件,最容易的前进路径通常是手动读取 PAC 文件,并找出成功运行 curl 所需的代理.

### Captive portals 门户

(这些不是代理,但在路上)

TBD

### 代理类型

curl 支持几种不同类型的代理.

默认的代理类型是 HTTP,因此如果指定代理主机名(或 IP 地址)而没有协议部分(通常写成"[http://](http://"), 那 curl 假设它是 HTTP 代理.

curl 还允许许多不同的选项来设置代理类型,而不是使用协议前缀.见[socks](#socks)下面部分.

### 超文本传输协议

HTTP 代理是客户端用 HTTP 进行传输的代理.默认情况下,cURL 将假定您指向的主机`-x`或`--proxy`是一个 HTTP 代理,除非您还指定了端口号,否则它将默认为端口 3128(特定端口号的原因纯粹是历史的).

如果您想使用 192.168.0.1 端口 8080 上的代理,请求 example.com 网页,命令行可以像:

```
curl -x 192.168.0.1:8080 http:/example.com/
```

回想一下,代理接收您的请求,将其转发到真正的服务器,然后从服务器读取响应,然后将响应传回客户端.

如果启用日志模式`-v`当与代理对话时,您将看到 curl 连接到代理而不是远程服务器,并且您将看到它使用稍微不同的请求行.

### HTTPS 与代理

HTTPS 被设计为允许, 并提供从客户端到服务器的安全和保护端到端隐私(并且返回).为了在和 HTTP 代理对话时提供这一点,HTTP 协议有一个特殊的请求,curl 使用该请求通过代理建立隧道,然后可以对其进行加密和验证.这种 HTTP 方法称为`CONNECT`.

当代理在 CONNECT 方法建立之后, 将加密数据隧道传输到远程服务器时,代理不能 _在不破坏加密的情况下_ 查看或修改通信量:

```
curl -x proxy.example.com:80 https://example.com/
```

### MITM 代理

MITM 指的是中间人.MITM 代理通常由公司部署在"企业环境"和其他地方,网络的所有者甚至希望调查 TLS 加密的流量.

为此,它们要求用户在客户端安装自定义的"信任根"(CA 证书),然后代理阻拦来自客户端的所有 TLS 通信量,并模拟远程服务器充当代理.然后,代理返回由自定义 CA 签名的生成的证书.这种代理设置通常清晰透明地捕获, 客户端到远程机器上的 TCP 端口 443 的所有通信量. 在这样的网络中运行的 cURL 也会被获得 HTTPS 流量.

自然,这种做法是允许中间人对所有 TLS 流量进行解密和窥探.

### HTTP 代理上的非 HTTP 协议

一个"HTTP 代理"意味着代理本身讲 HTTP. HTTP 代理主要用于代理 HTTP,但它们也支持其他协议也是很常见的.特别是,FTP 是相当普遍的支持.

当谈论 FTP"运行在"HTTP 代理时,通常是通过或多或少假装其他协议能像 HTTP 一样工作,并请求代理"获取此 URL", 即使 URL 不使用 HTTP 协议. 记住这种区别很重要,因为它意味着当通过像这样的 HTTP 代理发送时, 即例如给定一个 FTP URL,curl 也不会真正说 FTP; 因此,特定于 FTP 的特性将无法工作:

```
curl -x http://proxy.example.com:80 ftp://ftp.example.com/file.txt
```

那,你能解决它,你可以做的是"隧道通过"HTTP 代理!

### HTTP 代理隧道

大多数 HTTP 代理允许客户端"隧道"它到另一侧的服务器.这就是每次使用 HTTPS 协议,通过 HTTP 代理时所做的事情.

通过使用 cURL 的`-p`或`--proxytunnel`,使 HTTP 代理进行隧道传输.

当通过代理执行 HTTPS 时,通常要连接到默认的 HTTPS 远程 TCP 端口号 443,因此您会发现大多数 HTTP 代理白名单,只允许连接到该端口号上的主机,也许还有其他主机.大多数代理服务器会拒绝客户端连接到自身的任意随机端口(因为只有代理管理员知道).

尽管如此,假设 HTTP 代理允许它,您可以要求它以任何端口号通过隧道传送到远程服务器,这样即进行了隧道传送,可以"正常"执行其他协议.你可以这样做 FTP 隧道:

```
curl -p -x http://proxy.example.com:80 ftp://ftp.example.com/file.txt
```

您可以告诉 curl 在发给 HTTP 代理的`CONNECT`请求中使用 HTTP/1.0, 通过`--proxy1.0 [proxy]`而不是`-x`.

### SOCKS 类型

SOCKS 是用于代理的协议,curl 支持它.curl 既支持 SOCKS 版本 4,也支持版本 5,两种版本都有两种口味.

您可以通过使用`-x`给定代理主机的正确协议部分来选择特定的 SOCKS 版本,或者您可以用单独的选项来指定它,而不是`-x`.

SOCKS4 用于版本 4,SOCKS4A 用于版本 4,不在本地解析主机名称:

```
curl -x socks4://proxy.example.com http://www.example.com/

curl --socks4 proxy.example.com http://www.example.com/
```

SOCSK4A 版本:

```
curl -x socks4a://proxy.example.com http://www.example.com/

curl --socks4a proxy.example.com http://www.example.com/
```

SOCKS5 用于版本 5,SoCKS5 主机名用于版本 5,不在本地解析主机名:

```
curl -x socks5://proxy.example.com http://www.example.com/

curl --socks5 proxy.example.com http://www.example.com/
```

SOCSK5 主机名版本.这将主机名发送到服务器,因此在本地没有名字解析:

```
curl -x socks5h://proxy.example.com http://www.example.com/

curl --socks5-hostname proxy.example.com http://www.example.com/
```

### 代理认证

HTTP 代理可能需要身份验证,因此 curl 需要向代理提供允许使用它的适当凭据,而如果不这样做,则只会使代理使用代码 407 返回 HTTP 响应.

代理的身份验证非常类似于"基本"HTTP 身份验证,但与服务器身份验证分开,这允许客户端可以独立使用基本主机身份验证和代理身份验证.

使用 curl,您可以为代理身份验证设置用户名和密码`-U user:password`或`--proxy-user user:password`选项:

```
curl -U daniel:secr3t -x myproxy:80 http://example.com
```

此示例将默认使用基本身份验证协议.一些代理将需要另一个身份验证协议(当您获得 407 响应时返回的头将告诉您哪个),然后您可以要求使用以下方法进行特定的身份验证`--proxy-digest`,`--proxy-negotiate`,`--proxy-ntlm`. 上面的示例再次被命令,但是用 NTLM auth 请求代理:

```
curl -U daniel:secr3t -x myproxy:80 http://example.com --proxy-ntlm
```

还有一个选项, 可提问 curl 确定代理想要和支持哪个方法,(可能需要额外的往返成本)就是使用`--proxy-anyauth`.提问 curl 使用代理所需要的任何方法是这样的:

```
curl -U daniel:secr3t -x myproxy:80 http://example.com --proxy-anyauth
```

### HTTPS 到代理服务器

前面提到的所有与代理对话的协议都是明文协议、HTTP 和 SOCKS 版本.使用这些方法可以允许某人窃听您或代理所在的本地网络的流量.

一种解决协议是使用 HTTPS 到代理,然后代理建立安全且加密的连接,该连接不受简单监视.

### 代理环境变量

cURL 检查是否存在特殊命名的环境变量,然后运行它来查看是否请求的代理被使用.

通过设置变量名来指定代理`[scheme]_proxy`保存代理主机名(与您`-x`指定主机的方式相同)因此,如果您想在访问 HTTP 服务器时,告诉 curl 使用代理,那么您就设置了"http_proxy"环境变量.这样:

```
http_proxy=http://proxy.example.com:80
curl -v www.example.com
```

虽然上面的例子显示了 HTTP,当然,您也可以设置 ftp_proxy、https_proxy 等.除了 http_proxy 之外,所有这些代理环境变量名称也可以用大写形式指定,例如 HTTPS_PROXY.

设置单个变量可控*全部的*协议,ALL_PROXY 是存在.如果一个特定的协议变量存在,这一个将优先.

当使用环境变量来设置代理时,您可能很容易陷入一种情况,即应该排除一个或多个主机名通过代理.然后用 NO_PROXY 变量完成此操作.将其设置为逗号分隔的主机名列表,该列表在访问时,不应使用的代理.可以将 NO_PROXY 设置为单个星号(`*`)匹配所有主机.

作为 NO_PROXY 变量的替代,还有一个`--noproxy`命令行选项,相同的目的,工作.

### 代理报头

代理头

TBD
