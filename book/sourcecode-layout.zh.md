
## 代码布局

cURL源代码树既不庞大也不复杂.要记住的关键一点是,libcurl可能是库,而库是curl命令行工具的最大组件.

### 根

我们试图将源树根中的文件数量保持在最低限度.如果检查发布归档文件,与git存储库中存储的文件相比,会发现文件稍有不同,因为发布脚本生成了几个文件.

其中一些更值得注意的包括:

-   `buildconf`用于在从Git存储库中创建源卷时构建配置和更多.
-   `buildconf.bat`Windows版本的BudidCONF.在从Git签出完整的源代码之后运行此操作.
-   `CHANGES`:在发布时生成并放入发布存档.它包含源库的1000个最新更改.
-   `configure`生成的脚本,用于在构建cURL时在UNIX类系统上生成安装.
-   `COPYING`许可证详述了使用代码的规则.
-   `GIT-INFO`只存在于Git中,包含关于如何从Git签出代码后如何构建cURL的信息.
-   `maketgz`用于生成发布档案和每日快照的脚本.
-   `README`一个简短的总结什么cURL和利比曲.
-   `RELEASE-NOTES`包含为最新版本所做的更改;当在git中发现时,它包含自上个版本以来所做的更改,这些更改注定要在下一个版本中结束.

### 国际清算银行

此目录包含LICORL的完整源代码.它是一百个C源文件和一些私有头文件的所有平台的相同源代码.当应用程序不支持LIcCURL时使用的头文件不存储在该目录中;请参阅包含/cURL.

根据您自己的构建中启用了哪些特性以及平台提供了哪些功能,一些源文件或部分源文件可能包含特定构建中不使用的代码.

### LIB/VTLS

LBCURL中的VTLS子段是所有TLS后端的家,可以构建LBCURL来支持."虚拟"TLS内部API是libcurl内用于访问TLS和密码函数的公共API,而不需要主代码确切知道使用哪个TLS库.这允许构建LBCURL的人从各种各样的TLS库中选择来构建.

我们还维持一个[SSL比较表](https://curl.haxx.se/docs/ssl-compared.html)在网站上帮助用户.

-   OpenSSL:(目前为止)最流行的TLS库.
-   BoringSSL:由谷歌维护的OpenSSL叉子.它将使LIbCURL由于缺少库中的某些功能而禁用一些特征.
-   LibreSSL:OpenSSD团队维护的OpenSSL叉子.
-   NSS:一个完整的TLS库,它最可能被Firefox Web浏览器所使用.这是默认的TLS后端,用于FEDORA和ReHAT系统的cURL.
-   GnuTLS:一个完整的TLS库,默认情况下使用Debian封装cURL.
-   MeBTLS:(以前称为PulsSSL)是一个针对嵌入式市场的TLS库.
-   WOLFSSL(以前称为CysAL)是一个面向嵌入式市场的TLS库.
-   AXTLS:一个微小的TLS库,侧重于一个小的占地面积.
-   SneNe:在Windows上的本地TLS库.
-   安全传输:Mac OS X上的本地TLS库.
-   GSKit:OS/ 400上的本地TLS库.

### src

该目录保存cURL命令行工具的源代码.它是运行该工具的所有平台的相同源代码.

命令行工具所做的大部分工作就是将给定的命令行选项转换为对应的libcurl选项或选项集,然后确保正确地发布这些选项,以便根据用户的意愿驱动网络传输.

此代码使用LBCURL,就像任何其他应用程序一样.

### 包含/cURL

下面是为应用程序提供LIGBURL的公共头文件.其中一些是在配置或发布时生成的,因此它们在git存储库中的外观与发布归档中的外观不同.

使用现代的LIcCURL,所有的应用程序都希望包含在它的C源代码中`#include <curl/curl.h>`

### 文档

主要文档位置.此目录中的文本文件通常是纯文本文件.我们已经慢慢地开始转向Markdown格式,所以少数(但希望数量不断增加)文件使用.md扩展来表示这一点.

大多数这些文件也显示在cURL网站上,从文本自动转换为Web友好格式/外观.

-   `BINDINGS`列出所有已知的LIbCURL语言绑定,以及在哪里找到它们
-   `BUGS`如何报告错误和在哪里
-   `CODE_OF_CONDUCT.md`我们希望人们在这个项目中表现如何
-   `CONTRIBUTE`当对项目做出贡献时该考虑些什么
-   `curl.1`CURL命令行工具MAN页面,以NROFF格式
-   `curl-config.1`在NROFF格式的cURL配置页面
-   `FAQ`常见的与cURL相关的问题
-   `FEATURES`cURL特征的不完整列表
-   `HISTORY`描述项目是如何启动并经过多年发展的
-   `HTTP2.md`如何使用cURL和LyCURL使用HTTP/2
-   `HTTP-COOKIES`CURL如何支持和使用HTTP Cookie
-   `index.html`作为文档索引页的基本HTML页面
-   `INSTALL`如何建立和安装CURL和LICORL
-   `INSTALL.cmake`如何用CMake构建cURL和cURL
-   `INSTALL.devcpp`如何用DEVCPP构建cURL和cURL
-   `INTERNALS`细节:cURL和cURL内部结构
-   `KNOWN_BUGS`已知错误和问题列表
-   `LICENSE-MIXING`描述如何组合不同的第三方模块及其各自的许可证.
-   `MAIL-ETIQUETTE`这是如何在邮件列表中进行通信的
-   `MANUAL`关于如何使用cURL的教程指南
-   `mk-ca-bundle.1`MK-CA捆绑工具手册,以NROFF格式
-   `README.cmake`具体细节
-   `README.netware`NETWORD具体细节
-   `README.win32`Win32的具体细节
-   `RELEASE-PROCEDURE`如何做cURL和cURL的释放
-   `RESOURCES`进一步阅读进一步研究cURL的原因、原因和方法
-   `ROADMAP.md`我们将来想做什么
-   `SECURITY`我们如何处理安全漏洞
-   `SSLCERTS`TLS证书处理记录
-   `SSL-PROBLEMS`常见SSL问题及其原因
-   `THANKS`幸亏有这么多友好的人,今天卷发就存在了!
-   `TheArtOfHttpScripting`使用HTL编写HTTP脚本教程
-   `TODO`我们或你能做的事情
-   `VERSIONS`LIPCURL作品的版本编号

### 文件/文件表

所有的LILCURL函数都有自己的人页,每个文件中有3个扩展名,使用NROFF格式,在这个目录中.这也是下面描述的一些其他文件.

-   `ABI`
-   `index.html`
-   `libcurl.3`
-   `libcurl-easy.3`
-   `libcurl-errors.3`
-   `libcurl.m4`
-   `libcurl-multi.3`
-   `libcurl-share.3`
-   `libcurl-thread.3`
-   `libcurl-tutorial.3`
-   `symbols-in-versions`

### DOC/LBCURL/OPTS

该目录包含用于三个不同的LILCURL函数的单个选项的MAN页面.

`curl_easy_setopt()`选项开始`CURLOPT_`,`curl_multi_setopt()`选项开始`CURLMOPT_`和`curl_easy_getinfo()`选项开始`CURLINFO_`.

### 文档/实例

包含100个独立的示例,以帮助读者理解如何使用LyCURL.

也见[诽谤的例子](libcurl-examples.md)这本书的章节.

### 脚本

手提脚本.

-   `contributors.sh`:从给定的散列/标签中提取GIT存储库中的所有贡献者.其目的是为RELEASE-NOTES文件生成一个列表,并允许手动添加的名称即使在更新时也保留在该列表中.脚本使用"感谢筛选器"文件重写一些名称.
-   `contrithanks.sh`:从给定的散列/标签提取Git存储库中的贡献者,过滤掉所有已经提到的名称.`THANKS`然后输出`THANKS`在最后附上新贡献者的列表;这意味着允许更容易地更新感谢文档.脚本使用"感谢筛选器"文件重写一些名称.
-   `log2changes.pl`生成`CHANGES`释放脚本,如发布脚本所使用的.它简单地转换Git日志输出.
-   `zsh.pl`帮助脚本向ZSH shell的用户提供cURL命令行完成.
