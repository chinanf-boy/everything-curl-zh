
## 代理

代理是一种机器或软件,它代表客户,为客户做一些事情.

您还可以将其看作一个位于您和想要与之一起工作的服务器之间的中间人,一个您连接的中间人,而不是实际的远程服务器.您要求代理为您执行所需的操作,然后它将运行并执行该操作,然后将数据返回给您.

有几种不同类型的代理,我们将在本节中进一步列出和讨论它们.

### 发现你的代理

有些网络设置成需要代理,以便您访问Internet或者您感兴趣的特定网络.由于策略或技术原因,运行网络的人员和管理层在您的网络上介绍了代理的使用.

在网络空间中,有几种自动检测代理的方法以及如何连接到代理,但这些方法都不是真正通用的,curl也不支持这些方法.此外,当您通过代理与外部世界通信时,这通常意味着您必须对代理信任很多,因为代理将能够查看和修改您发送或通过的所有非安全网络流量.这种信任是不容易自动承担的.

如果检查浏览器的网络设置,有时在高级设置选项卡下,可以了解浏览器配置为使用什么代理或代理.当你使用cURL时,你应该使用同一个或多个.

TBD:如何在Firefox和Chrome中找到代理设置的截图?

### 派克靴

一些网络环境提供了几种不同的代理,这些代理应该在不同的情况下使用,以及浏览器支持的非常可定制的处理方式.这被称为"代理自动配置"或PAC.

PAC文件包含一个JavaScript函数,该函数决定一个给定的网络连接(URL)应该使用哪个代理,即使它根本不应该使用代理.浏览器通常从本地网络上的URL读取PAC文件.

由于CURL没有JavaScript功能,所以CURL不支持PAC文件.如果您的浏览器和网络使用PAC文件,则最容易的前进路径通常是手动读取PAC文件,并找出成功运行curl所需的代理.

### 囚禁门户

(这些不是代理,但在路上)

TBD

### 代理类型

CURL支持几种不同类型的代理.

默认的代理类型是HTTP,因此如果指定代理主机名(或IP地址)而没有方案部分(通常写成"[http://](http://")CURL假设它是HTTP代理.

CURL还允许许多不同的选项来设置代理类型,而不是使用方案前缀.见[袜子](#socks)下面部分.

### 超文本传输协议

HTTP代理是客户端用HTTP进行传输的代理.默认情况下,cURL将假定您指向的主机.`-x`或`--proxy`是一个HTTP代理,除非您还指定了端口号,否则它将默认为端口3128(特定端口号的原因纯粹是历史的).

如果您想使用192.1680.1端口8080上的代理请求示例.com网页,命令行可以像:

```
curl -x 192.168.0.1:8080 http:/example.com/
```

回想一下,代理接收您的请求,将其转发到真正的服务器,然后从服务器读取响应,然后将响应传回客户端.

如果启用日志模式`-v`当与代理对话时,您将看到curl连接到代理而不是远程服务器,并且您将看到它使用稍微不同的请求行.

### HTTPS与代理

HTTPS被设计为允许并提供从客户端到服务器的安全和安全的端到端隐私(并且返回).为了在和HTTP代理对话时提供这一点,HTTP协议有一个特殊的请求,curl使用该请求通过代理建立隧道,然后可以对其进行加密和验证.这种HTTP方法称为`CONNECT`.

当代理在CONNECT方法建立之后将加密数据隧道传输到远程服务器时,代理不能在不破坏加密的情况下查看或修改通信量:

```
curl -x proxy.example.com:80 https://example.com/
```

### MITM代理

米特指的是中间人.MITM代理通常由公司部署在"企业环境"和其他地方,网络的所有者甚至希望调查TLS加密的流量.

为此,它们要求用户在客户端安装自定义的"信任根"(CA证书),然后代理终止来自客户端的所有TLS通信量,模拟远程服务器并充当代理.然后,代理返回由自定义CA签名的生成的证书.这种代理设置通常透明地捕获从客户端到远程机器上的TCP端口443的所有通信量.在这样的网络中运行cURL也会获得它的HTTPS流量.

当然,这种做法允许中间人对所有TLS流量进行解密和窥探.

### HTTP代理上的非HTTP协议

一个"HTTP代理"意味着代理本身讲HTTP.HTTP代理主要用于代理HTTP,但它们也支持其他协议也是很常见的.特别是,FTP是相当普遍的支持.

当谈论FTP"over"HTTP代理时,通常是通过或多或少假装其他协议像HTTP一样工作,并请求代理"获取此URL",即使URL不使用HTTP.这种区别很重要,因为它意味着当通过像这样的HTTP代理发送时,即使给定FTP URL,curl也不会真正说FTP;因此,特定于FTP的特性将无法工作:

```
curl -x http://proxy.example.com:80 ftp://ftp.example.com/file.txt
```

然后,你可以做的是"隧道通过"HTTP代理!

### HTTP代理隧道

大多数HTTP代理允许客户端"隧道"它到另一侧的服务器.这就是每次使用HTTP代理通过HTTP代理时所做的事情.

通过使用cURL的HTTP代理进行隧道传输.`-p`或`--proxytunnel`.

当通过代理执行HTTPS时,通常要连接到默认的HTTPS远程TCP端口号443,因此您会发现大多数HTTP代理都是白名单,并且只允许连接到该端口号上的主机,也许还有其他主机.大多数代理将拒绝客户端连接到任意随机端口(因为只有代理管理员知道).

尽管如此,假设HTTP代理允许它,您可以要求它以任何端口号通过隧道传送到远程服务器,这样即使进行隧道传送,也可以"正常"执行其他协议.你可以这样做FTP隧道:

```
curl -p -x http://proxy.example.com:80 ftp://ftp.example.com/file.txt
```

您可以告诉CURL在使用HTTP代理发出的连接请求中使用HTTP/1.`--proxy1.0 [proxy]`而不是`-x`.

### 袜子类型

SOCKS是用于代理的协议,CURL支持它.CURL既支持SOCKS版本4,也支持版本5,两种版本都有两种口味.

您可以通过使用给定代理主机的正确方案部分来选择特定的SOCKS版本.`-x`,或者您可以用单独的选项来指定它,而不是`-x`.

SOCKS4用于版本4,SOCKS4A用于版本4,而不在本地解析主机名称:

```
curl -x socks4://proxy.example.com http://www.example.com/

curl --socks4 proxy.example.com http://www.example.com/
```

SOCSK4A版本:

```
curl -x socks4a://proxy.example.com http://www.example.com/

curl --socks4a proxy.example.com http://www.example.com/
```

SOCKS5用于版本5,SoCKS5主机名用于版本5,而不在本地解析主机名:

```
curl -x socks5://proxy.example.com http://www.example.com/

curl --socks5 proxy.example.com http://www.example.com/
```

SOCSK5主机名版本.这将主机名发送到服务器,因此在本地没有名字解析:

```
curl -x socks5h://proxy.example.com http://www.example.com/

curl --socks5-hostname proxy.example.com http://www.example.com/
```

### 代理认证

HTTP代理可能需要身份验证,因此curl需要向允许使用它的代理提供适当的凭据,而如果不这样做,则只会使代理使用代码407返回HTTP响应.

代理的身份验证非常类似于"普通"HTTP身份验证,但是与服务器身份验证是分开的,以允许客户端独立使用普通主机身份验证和代理身份验证.

使用CURL,您可以为代理身份验证设置用户名和密码.`-U user:password`或`--proxy-user user:password`选项:

```
curl -U daniel:secr3t -x myproxy:80 http://example.com
```

此示例将默认使用基本身份验证方案.一些代理将需要另一个身份验证方案(当您获得407响应时返回的头将告诉您哪个),然后您可以要求使用以下方法进行特定的身份验证`--proxy-digest`,`--proxy-negotiate`,`--proxy-ntlm`. 上面的示例再次被命令,但是用代理请求NTLM AUTH:

```
curl -U daniel:secr3t -x myproxy:80 http://example.com --proxy-ntlm
```

还有一个选项要求curl确定代理想要和支持哪个方法,然后使用`--proxy-anyauth`. 请求CURL使用代理所需要的任何方法是这样的:

```
curl -U daniel:secr3t -x myproxy:80 http://example.com --proxy-anyauth
```

### HTTPS到代理服务器

前面提到的所有与代理对话的协议都是明文协议、HTTP和SOCKS版本.使用这些方法可以让人窃听您的流量,在本地网络中,您或代理驻留.

一种解决方案是使用HTTPS到代理,然后代理建立安全且加密的连接,该连接不受简单监视.

### 代理环境变量

cURL检查是否存在特殊命名的环境变量,然后运行它来查看是否请求代理被使用.

通过设置变量名来指定代理`[scheme]_proxy`保存代理主机名(与您指定主机的方式相同)`-x`)因此,如果您想在访问HTTP服务器时告诉CURL使用代理,那么您就设置了"httpjyPosivServer"环境变量.这样地:

```
http_proxy=http://proxy.example.com:80
curl -v www.example.com
```

虽然上面的例子显示了HTTP,当然,您也可以设置FTPJOpAgent、HTTPSY代理等.除了HTTPJPro代理之外,所有这些代理环境变量名称也可以用大写形式指定,例如HTTPSY-代理.

设置控件的单个变量*全部的*协议,ALLYPROVER存在.如果一个特定的协议变量存在,这样的一个将优先.

当使用环境变量来设置代理时,您可能很容易陷入一种情况,即应该排除一个或多个主机名通过代理.然后用NOTY代理变量完成此操作.将其设置为逗号分隔的主机名列表,该列表在访问时不应使用代理.可以将NOIY代理设置为单个星号(’\*)匹配所有主机.

作为NONY代理变量的替代,还有一个`--noproxy`命令行选项,用于相同的目的并以相同的方式工作.

### 代理报头

代理头

TBD
