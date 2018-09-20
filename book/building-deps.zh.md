
# 依赖关系

制作好软件的关键是建立在其他伟大软件之上.通过使用许多其他人使用的库,我们重新发明相同的东西的时间更少,我们得到更可靠的软件,因为有更多的人使用相同的代码.

CURL提供的一系列完整的特性要求它被构建为使用一个或多个外部库.它们是cURL的依赖关系.他们中没有一个是*必修的*但是大多数用户都希望至少使用其中的一些.

## zlib

[HTTPS://ZLIB NET/](https://zlib.net/)

如果使用ZLIB构建,CURL可以对HTTP传输的数据进行自动解压缩.通过有线获得压缩数据将使用更少的带宽.

## C-阿瑞斯

[HTSPS://C-AES.HAXX.SE//](https://c-ares.haxx.se/)

CURL可以用C-ARES来构建,以便能够进行异步名称解析.启用异步名称解析的另一个选项是使用线程化名称解析器后端构建curl,而后端将为每个名称解析创建一个单独的帮助线程.C-ARES都在同一线程中.

## LIPSHS2

[HTTPS://LIbsHS2.Org/](https://libssh2.org/)

当使用LIPSH2构建CURL时,它支持SCP和SFTP协议.

## nghttp2

<https://nghttp2.org/>

这是一个用于处理HTTP/2帧的库,是CURL支持HTTP版本2的先决条件.

## openldap

[HTTPS://www. OpenLDAP.Org/](https://www.openldap.org/)

此库是允许CURL获得LDAP和LDAP URL方案的支持的一种选择.在Windows上,还可以选择使用CURL来构建WINSSL库.

## 溴化锂

[HTTPS://RTMPDUM.MPLAGER HQ.HU/](https://rtmpdump.mplayerhq.hu/)

为了支持curl对RTMP URL方案的支持,必须使用来自RTMPDump项目的librtmp库构建curl.

## 锂离子电池

[HTTPS://ActuxPAD.NET/LIBMETALLink](https://launchpad.net/libmetalink)

用LyMealLink构建cURL以支持它[金属链](https://www.metalinker.org/)格式,它允许CURL从多个地方下载相同的文件.它包括校验和等等.见cURL[--金属线](https://curl.haxx.se/docs/manpage.html#--metalink)选择权.

## LIPPSL

[HTTPS://ROCKDABOT.GITHUBIO/LIBPSL/](https://rockdaboot.github.io/libpsl/)

在构建支持libpsl的curl时,cookie解析器将了解Public Suffix List,从而适当地处理这些cookie.

## 利比登2

[HTTPS://www. GNUGOR/软件/LIDBIN/LIDIDN2/MUROLAL/LIDBYN2.HTML](https://www.gnu.org/software/libidn/libidn2/manual/libidn2.html)

CURL借助LIDBIN2库处理国际域名(IDN).

## TLS文库

有许多不同的TLS库可供选择,因此它们被覆盖在一个[分部](building-tls.md).
