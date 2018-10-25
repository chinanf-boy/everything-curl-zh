
# TLS

TLS代表传输层安全性,是以前称为SSL的技术.但是SSL这个术语并没有真正消失,所以现在TLS和SSL这两个术语经常可互换地用于描述相同的事情.

TLS是TCP"之上"的密码安全层,它基于强公钥密码和数字签名,使数据防篡改,并保证服务器的真实性.

## 加密

当cURL连接到TLS服务器时,它协商协议,并且涉及讨论双方需要同意的几个参数和变量.其中一个参数是使用哪些加密算法,即所谓的加密.随着时间的推移,安全研究人员发现了现有加密的缺陷和弱点,随着时间的推移,它们逐渐被淘汰.

使用日志的选项`-v`，您可以获得关于密码和TLS版本协商的信息. 通过使用`--ciphers`选项,您可以更改在协商中更喜欢什么加密方式,但请注意,这是一个功能强大的特性,它需要具备如何使用的知识,而不使情况变得更糟.

## 启用TLS

curl支持许多协议的TLS版本.HTTP有HTTPS,FTP有FTPS,LDAP有LDAP,POP3有POP3S,IMAP有IMAPS和SMTP有SMTPS.

如果服务器端支持它,您可以使用cURL使用这些协议的TLS版本.

有两种通用协议来处理TLS.其一是已经在第一次连接握手时，使用TLS；而另一个是使用协议特定的指令从纯文本"升级"到TLS的连接.

使用curl,如果显式地在URL中指定协议的TLS版本(名称以"S"字符结尾的那个),curl一开始就尝试TLS连接,而如果要在URL中指定非TLS版本,则可以使用`--ssl`选项*通用*升级为TLS的连接.

支持表如下:

| 干净版本  | TLS 版本 | --ssl   |
|--------|-------------|---------|
| HTTP   | HTTPS       | no      |
| LDAP   | LDAPS       | no      |
| FTP    | FTPS        | **yes** |
| POP3   | POP3S       | **yes** |
| IMAP   | IMAPS       | **yes** |
| SMTP   | SMTPS       | **yes** |

协议*可以*`--ssl`都赞成那种方法. 使用`--ssl`意味着cURL会*尝试*升级为TLS的连接,但如果失败了,它将继续使用协议的纯文本版本的传输. 在失败情况下，要让`--ssl`选项**必须**继续TLS连接,可用`--ssl-reqd`选项，而这若curl不能成功地协商TLS,将使传输失败.

FTP传输必须要TLS安全性:

```
curl --ssl-reqd ftp://ftp.example.com/file.txt
```

建议TLS用于您的FTP传输:

```
curl --ssl ftp://ftp.example.com/file.txt
```

直接使用TLS(如`HTTPS://, LDAPS://, FTPS://`等)意味着TLS是强制性的,如果没有成功协商TLS,curl将返回错误.

通过HTTPS获取文件:

```
curl https://www.example.com/
```

## SSL和TLS版本

SSL是在90年代中期发明的,此后就发展起来了.SSL版本2是互联网上使用的第一个广泛版本,但在很长一段时间以前,它被认为是不安全的.SSL版本3从此接管,并且它也被认为不安全,但足以使用.

TLS版本1是第一个"标准"。RFC 2246发表于1999。TLS 1.1在2006出炉,进一步提高了安全性,其次是TLS 1.2在2008。TLS 1.2成为TLS的黄金标准，也十年了.

TLS 1.3(RFC 8446)于2018年8月由IETF确定并发布为标准。这是迄今为止最安全和最快的TLS版本。然而,因为太新，软件、工具和库并不支持它。

curl默认使用SSL/TLS的"安全版本". 这意味着除非特别告知,否则它不会协商SSLv2或SSLv3,事实上,几个TLS库不再提供对这些协议的支持,因此在很多情况下,除非您认真审对,否则curl甚至不会说出那些协议版本.

| 选项    | 使用                |
|-----------|--------------------|
| --sslv2   | SSL version 2      |
| --sslv3   | SSL version 3      |
| --tlsv1   | TLS >= version 1.0 |
| --tlsv1.0 | TLS >= version 1.0 |
| --tlsv1.1 | TLS >= version 1.1 |
| --tlsv1.2 | TLS >= version 1.2 |
| --tlsv1.3 | TLS >= version 1.3 |

**注:**TLS 1.3版本只在选定的最近的开发版本的TLS库支持,需要cURL7.52.0或以上.

## 验证服务器证书

如果你不能确定你正在与一个**当前**服务器进行通信,那么连接到服务器是不值得的. 如果我们不知道这些,我们也可能和一个冒名顶替者谈话. *作为*我们认为的那个人.

为了检查它是否与正确的TLS服务器通信, curl使用一组本地存储的CA证书来验证服务器证书的签名.作为TLS握手的一部分,所有服务器都向客户端提供证书,并且所有使用TLS的公共服务器，都从已建立的证书颁发机构获得证书.

在应用了一些密码魔法之后,curl知道服务器实际上，正是curl用来连接到它的主机名且证书是正确的服务器.未能验证服务器的证书是TLS握手失败,curl会错误退出.

在极少数情况下,即使证书验证失败,您也可能决定仍然希望与TLS服务器通信.那你需要接受这样一个事实:你的连接可能会受到中间人的攻击.你放下警卫`-k`或`--insecure`选项.

## CA存储

curl需要一个"CA存储",一个CA证书的集合,来验证它所讨论的TLS服务器.

如果构建curl时，使用对您的平台来说是"原生"的TLS库, 那么该库也可能使用本地CA存储.如果没有, 则必须构建curl才能知道本地CA存储库在哪里,或者用户需要在调用curl时提供到CA存储库的路径.

您可以用`--cacert`命令行选项，在TLS握手中指出一个特定的CA包. 这个包需要用PEM格式. 还可以设置环境变量`CURL_CA_BUNDLE`通向完整路径.

### Windows上的CA存储

若在Windows系统，不使用本机TLS库(Schannel)的构建的curl，那你要在 如何找到和使用CA存储上 多几步了

curl将在这些目录中搜索一个名为"curl-ca-bundle.crt"的CA证书文件,并按此顺序:

1.  应用程序目录
2.  当前工作目录
3.  Windows系统目录(例如`C:\windows\system32`)
4.  Windows目录(例如`C:\windows`)
5.  所有沿着`%PATH%`的目录

## 证书钉扎

TLS证书钉扎是验证用于签署服务器证书的公钥没有改变的一种方法.它被"钉住"了.

在协商TLS或SSL连接时,服务器会发送一个表示其身份的证书.从这个证书中提取一个公钥,如果它不完全匹配提供给这个选项的公钥,curl将在发送或接收任何数据之前中止连接.

您告诉curl能读取到sha256值的文件名,或者直接在命令行中用"sha256//"前缀指定base64编码的散列.可以指定这样的一个或多个散列,用分号(;)分隔.

```
curl --pinnedpubkey "sha256//83d34tasd3rt..." https://example.com/
```

此特性不是所有TLS后端都支持.

## OCSP装订

这使用名为`Certificate Status Request`的TLS扩展，来请求服务器在握手时从CA提供新的"proof", 证明它返回的`proof`仍然有效.这是一种确保服务器证书没有被撤销的方法.

如果服务器不支持这个扩展,测试将失败,并且cURL返回错误.服务器不支持这一点还是很常见了.

握手期间使用Status Request像这样:

```
curl --cert-status https://example.com/
```

此特性仅被OpenSSL, GnuTLS and NSS后端支持.

## 客户证书

TLS客户端证书是客户端加密地向服务器，证明它们确实是正确的对等体的一种方式. 命令行若使用客户端证书，需指定证书和相应的密钥,然后将它们传递给与服务器的TLS握手.

当这样做时,您需要将客户端证书存储在文件中,并且您以前应该通过不同方式的正确实例中获得过它.

密钥通常由需要提供密码，或交互提示的密码键入.

curl提供`--cert`选项,让您指定一个文件,该文件既是客户端证书,又是使用的私钥.或者可以独立地指定密钥文件`--key`:

```
curl --cert mycert:mypassword https://example.com
curl --cert mycert:mypassword --key mykey https://example.com
```

对于一些TLS后端,您还可以使用不同的类型,传递密钥和证书:

```
curl --cert mycert:mypassword --cert-type PEM \
     --key mykey --key-type PEM https://example.com
```

## TLS-AUTH

TBD

## 不同的TLS后端

TBD

## 多个TLS后端

TBD
