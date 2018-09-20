
# 连接

大多数使用CURL的协议都讲TCP.使用TCP,客户机(如curl)必须首先找出要与之通信的主机的IP地址,然后连接到它.""连接到它"意味着执行TCP协议握手.

For ordinary command line usage, operating on a URL, these are details which are taken care of under the hood, and which you can mostly ignore. 但有时你可能会发现自己想要调整细节…

## 名字解析技巧

### 编辑主机文件

也许你需要命令`curl http://example.com`连接到本地服务器而不是实际服务器.

你可以通过编辑你的`hosts`文件(文件)`/etc/hosts`在Linux和UNIX系统上,例如添加`127.0.0.1 example.com`将主机重定向到本地主机.然而,此编辑需要管理访问,并且它具有影响其他所有应用程序的缺点.

### 更改主机:标题

这个`Host:` header is the normal way an HTTP client tells the HTTP server which server it speaks to, as typically an HTTP server serves many different names using the same software instance.

因此,通过自定义修改`Host:`标头您可以让服务器响应站点的内容,即使您实际上没有连接到该主机名.

例如,运行主站点的测试实例.`www.example.com`在你的本地机器上,你想要cURL,要求索引HTML:

```
curl -H "Host: www.example.com" http://localhost/
```

设置习惯时`Host:`标头和使用Cookie,CURL将提取自定义名称,并在匹配cookie发送时使用它作为主机.

这个`Host:`当与HTTPS服务器通信时,页眉不够.对于HTTPS,在TLS协议中有一个单独的扩展字段称为SNI(Server Name Indication),它允许客户机告诉服务器它想要与之通信的服务器的名称.cURL将只提取从给定URL发送的SNI名称.

### 为名称提供自定义IP地址

你知道比卷发器应该去哪里更好吗?然后你可以给自己的IP地址cURL!如果要重定向端口80,请访问`example.com`相反,到达你的本地主机:

```
curl --resolve example.com:80:127.0.0.1 http://example.com/
```

您甚至可以指定多个`--resolve`切换以提供这种类型的多个重定向,如果使用的URL使用HTTP重定向,或者仅希望命令行使用多个URL时,可以使用这些重定向.

`--resolve`将地址插入到curl的DNS缓存中,因此它将有效地使curl相信这是解析名称时获得的地址.

当谈论HTTPS时,它将发送URL中的名称的SNI,curl将验证服务器的响应以确保它服务于URL中的名称.

### 提供替换名称

作为一个近亲`--resolve`选择权`--connect-to`选项提供了微小的变化.当使用特定的名称和端口号连接时,它允许您指定用于在引擎盖下使用的curl的替换名称和端口号.

例如,假设有一个站点被调用`www.example.com`这实际上是由三个不同的个人HTTP服务器:Load 1、Load 2和Load 3提供的,用于负载均衡目的.在典型的正常过程中,curl解析主站点,并访问负载均衡服务器之一(因为它返回一个列表,只选择其中的一个),一切都很好.如果您想将测试请求发送到负载平衡集之外的一个特定服务器(`load1.example.com`例如,你可以命令Curl做这件事.

你*可以*仍然使用`--resolve`如果您知道Loop1的特定IP地址,就可以做到这一点.但是,如果不必首先解析和修复IP地址,就可以告诉CURL:

```
curl --connect-to www.example.com:80:load1.example.com:80 http://www.example.com
```

它从源名称+源端口重定向到目的地名称+目的地端口.卷发将解决`load1.example.com`名称和连接,但在所有其他方式仍然假设它正在交谈`www.example.com`.

### 用C-ARES解析解决方案

如本书中其他地方所详述的那样,cURL可以用几种不同的名称解析后端来构建.这些后端之一由c-ares库提供动力,当构建curl以使用c-ares时,它获得一些额外的超级能力,而构建curl以使用其他名称解析后端则不能.也就是说,它获得了更具体地指示DNS服务器使用的能力以及DNS流量如何使用网络的能力.

用`--dns-servers`您可以精确地指定哪些DNS服务器cURL应该使用,而不是默认的.这允许您运行自己的实验服务器,该服务器的回答不同,或者如果您的常规服务器不可靠或已死,则使用备份服务器.

用`--dns-ipv4-addr`和`--dns-ipv6-addr`您要求CURL将DNS通信的本地端绑定到一个特定的IP地址`--dns-interface`您可以指示CURL使用特定的网络接口来使用DNS请求.

这些`--dns-*`期权是非常先进的,只适用于那些知道自己在做什么的人,并理解这些选择.但是它们提供非常可定制的DNS名称解析操作.

## 连接超时

CURL通常将TCP连接作为其网络传输的初始部分与主机连接.如果存在不稳定的网络条件或有故障的远程服务器,TCP连接可能会失败或非常缓慢.

为了减少对脚本或其他使用的影响,可以设置允许连接尝试的curl的最大时间(以秒为单位).用`--connect-timeout`你告诉CURL允许连接的最大时间,如果在那个时候卷发没有连接,它会返回一个故障.

连接超时仅限制了curl在连接时所花的时间,因此一旦建立了TCP连接,它可能需要更长的时间.见[超时](usingcurl-timeouts.md)更多关于通用cURL超时的章节.

如果指定了低超时,则有效地禁用了curl连接到通过不可靠网络访问的远程服务器、慢速服务器或服务器的能力.

连接超时可以指定为次精度的十进制值.例如,允许2781毫秒连接:

```
curl --connect-timeout 2.781 https://example.com/
```

## 网络接口

在具有连接到多个网络的多个网络接口的机器上,存在这样的情况,您可以决定希望传出网络流量使用哪个网络接口.或者在通信中使用哪个源IP地址(来自多个IP地址).

告诉curl您希望将通信的本地端"绑定"到哪个网络接口、哪个IP地址或甚至主机名,以及`--interface`选项:

```
curl --interface eth1 https://www.example.com/

curl --interface 192.168.0.2 https://www.example.com/

curl --interface machine2 https://www.example.com/
```

## 本地端口号

在本地端的IP地址和端口号与远程端的IP地址和端口号之间创建TCP连接.远程端口号可以在URL中指定,通常有助于标识您正在瞄准的服务.

本地端口号通常由网络堆栈随机分配给TCP连接,您通常不必再考虑它.但是,在某些情况下,您会发现自己处于网络设备、防火墙或类似的设置后面,这些设置限制了哪些源端口号可以允许设置传出连接.

对于这样的情况,您可以指定CURL应将哪些本地端口绑定到连接.您可以指定要使用的单个端口号,或者指定端口的范围.我们建议使用一个范围,因为端口是稀缺资源,而您想要的确切资源可能已经在使用.如果您要求本地端口号(或范围)无法获得cURL,它将以失败退出.

而且,在大多数操作系统中,如果不具有更高的特权级别(root),则无法绑定到1024以下的端口号,并且如果可以避免,我们通常建议不要将curl作为root运行.

在获得此HTTPS页面时,请要求CURL使用4000到4200之间的本地端口号:

```
curl --local-port 4000-4200 https://example.com/
```

## 保持活力

在不使用TCP连接的情况下,无论在哪个方向上,TCP连接都是完全没有流量的.因此,不能将完全空闲的连接与由于网络或服务器问题而完全过时的连接清楚地分开.

同时,现在许多网络设备,如防火墙或NAT,都在跟踪TCP连接,以便它们能够转换地址,阻塞"错误的"传入数据包等.冰到设备,但有时短到10分钟甚至更少.

避免真正缓慢的连接(或空闲的连接)被当作死连接和错误死连接的一种方法是确保使用TCP...TCP keepalive是TCP协议中的一个特性,它使得TCP在完全空闲时来回发送"ping帧".它帮助空闲连接检测中断,甚至当没有通信量移动到其上时,并且帮助中间系统不考虑连接死亡.

CURL默认使用TCP KeePovior作为这里提到的原因.但有时你想*使残废*保持活力,或者你想改变TCP"Ping"之间的间隔(cURL默认值为60秒).你可以关闭:

```
curl --no-keepalive https://example.com/
```

或将间隔改为5分钟(300秒):

```
curl --keepalive-time 300 https://example.com/
```
