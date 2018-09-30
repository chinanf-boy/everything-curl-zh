## 代码布局

curl 源代码树既不庞大也不复杂.要记住的关键一点是,libcurl 是个库且是 curl 命令行工具的最大组件.

### 根

我们试图将源树根中的文件数量保持在最低限度.如果检查发布归档文件,与 git 存储库中存储的文件相比,会发现文件稍有不同,因为发布脚本生成了几个文件.

[github source](https://github.com/curl/curl)

其中一些更值得注意的包括:

- `buildconf`: 构建配置和更多用在从源 Git 存储库中构建 curl 时.
- `buildconf.bat`: Windows 版本的 buildconf.在从 Git checkout 完整的源代码之后,运行此操作.
- `CHANGES`: 在发布时生成并放入发布存档.它包含源库的 1000 个最新更改.
- `configure`: 生成脚本,用于在构建 curl 时在 UNIX 类系统上生成安装.
- `COPYING`: 许可证详述了使用代码的规则.
- `GIT-INFO`: 只存在于 Git 中,包含关于如何从 Git checkout 代码后如何构建 curl 的信息.
- `maketgz`: 用于生成,发布档案和每日快照的脚本.
- `README`: 一个简短的总结 curl 和 libcurl 是什么.
- `RELEASE-NOTES`: 包含为最新版本所做的更改; 当在 git 中发现时,它包含自上个版本以来所做的更改,这些更改都在下一个版本标为结束.

### lib

此目录包含`libcurl`的完整源代码.它具有超一百个 C 源文件和一些私有头文件的相同源代码,提供所有平台.当应用程序不支持 libcurl 时, 使用的头文件不会存储在该目录中;请参阅 include/curl .

根据您自己的构建中启用了哪些特性以及平台提供了哪些功能,一些源文件或部分源文件可能包含在特定构建中不使用的代码.

### lib/vtls

libcurl 中的 VTLS 子是所有 TLS 后端的家,可以为构建 libcurl 提供支持. 这"虚拟"TLS 内部 API 是 libcurl 内,用于访问 TLS 和密码函数的公共 API,而不需要主代码确切知道使用哪个 TLS 库. 这允许构建 libcurl 的人可从各种各样的 TLS 库中选择构建.

我们还维持一个[SSL 比较表](https://curl.haxx.se/docs/ssl-compared.html)在网站上帮助用户.

- `OpenSSL`: (目前为止)最流行的 TLS 库.
- `BoringSSL`: 由谷歌维护的 OpenSSL 分支.它使 libcurl 缺少库中的某些功能进而禁用一些特征.
- `LibreSSL`: OpenSSD 团队维护的 OpenSSL 分支.
- `NSS`: 一个完整的 TLS 库,它最可能被 Firefox Web 浏览器使用.这是默认的 TLS 后端,用于 Fedora 和 ReHAT 系统的 curl.
- `GnuTLS`: 一个完整的 TLS 库,Debian 默认情况下使用的封装 curl.
- `mbedTLS`: (以前称为 PolarSSL)是一个针对嵌入式市场的 TLS 库.
- `WolfSSL`: (以前称为 cyaSSL)是一个面向嵌入式市场的 TLS 库.
- `axTLS`: 一个微小的 TLS 库,侧重于一个小的占地面积.
- `Schannel`: 在 Windows 上的本地 TLS 库.
- `SecureTransport`: Mac OS X 上的本地 TLS 库.
- `GSKit`: OS/400 上的本地 TLS 库.

### src

该目录保存 curl 命令行工具的源代码.它是所有平台运行该工具的的相同源代码.

命令行工具所做的大部分工作,就是将给定的命令行选项转换为对应的 libcurl 选项或选项集合,然后确保正确地运用这些选项,以便根据用户的意愿驱动网络传输.

此代码使用 libcurl,就像任何其他应用程序一样.

### include/curl

下面是为应用程序提供 libcurl 的公共头文件.其中一些是在配置或发布时生成的,因此它们在 git 存储库中的与发布文件中的看起来不同.

要使用现代的 libcurl, 所有的应用程序都要`#include <curl/curl.h>`包含在它的 C 源代码中

### docs

主要文档位置.此目录中的文本文件,通常是纯文本文件.我们已经慢慢地开始转向 Markdown 格式,所以少数(但希望数量不断增加)文件使用`.md` 后缀来表示这一点.

大多数这些文件也显示在 curl 网站上,从文本自动转换为 Web 友好格式/外观.

- `BINDINGS`: 列出所有已知的 libcurl 语言实现绑定库,以及在哪里找到它们
- `BUGS`: 如何和在哪里报告错误
- `CODE_OF_CONDUCT.md`: 我们希望人们在这个项目中如何去表现
- `CONTRIBUTE`: 当想对项目做出贡献时该考虑些什么
- `curl.1`: curl 命令行工具 man 页面, nroff 格式
- `curl-config.1`:  nroff 格式的 curl 配置页面
- `FAQ`: 常见的与 curl 相关的问题
- `FEATURES`: curl 特性的不完整列表
- `HISTORY`: 描述项目是如何启动,并经过多年发展的
- `HTTP2.md`: 如何使用 curl 和 libcurl 使用 HTTP/2
- `HTTP-COOKIES`: curl 如何支持和使用 HTTP Cookie
- `index.html`: 文档 index HTML
- `INSTALL`: 如何建立和安装 curl 和 libcurl
- `INSTALL.cmake`: 如何用 CMake 构建 curl 和 curl
- `INSTALL.devcpp`: 如何用 devcpp 构建 curl 和 curl
- `INTERNALS`: curl 和 curl 内部结构细节
- `KNOWN_BUGS`: 已知错误和问题列表
- `LICENSE-MIXING`: 描述如何组合不同的第三方模块及其各自的许可证.
- `MAIL-ETIQUETTE`: 这是如何在邮件列表中进行通信的
- `MANUAL`: 关于如何使用 curl 的教程指南
- `mk-ca-bundle.1`: mk-ca-bundle 工具手册,以 nroff 格式
- `README.cmake`: 特定CMake细节
- `README.netware`: 特定Netware细节
- `README.win32`: 特定win32细节
- `RELEASE-PROCEDURE`: 如何发布 curl 和 curl 版本
- `RESOURCES`: 进一步阅读,进一步研究 curl 的 what、why和how
- `ROADMAP.md`: 我们将来想做什么
- `SECURITY`: 我们如何处理安全漏洞
- `SSLCERTS`: TLS 证书处理记录
- `SSL-PROBLEMS`: 常见 SSL 问题及其原因
- `THANKS`: 幸亏有这么多友好的人,今天 curl 就存在了!
- `TheArtOfHttpScripting`: 使用 curl 编写 HTTP 脚本教程
- `TODO`: 我们或你能做的事情
- `VERSIONS`: libcurl 作品的版本编号

### docs/libcurl

所有的 libcurl 函数都有自己的man页,每个文件中有 3 个扩展名,使用 nroff 格式,在这个目录中.这也是下面描述的一些其他文件.

- `ABI`
- `index.html`
- `libcurl.3`
- `libcurl-easy.3`
- `libcurl-errors.3`
- `libcurl.m4`
- `libcurl-multi.3`
- `libcurl-share.3`
- `libcurl-thread.3`
- `libcurl-tutorial.3`
- `symbols-in-versions`

### docs/libcurl/opts

该目录包含用于三个不同的 libcurl 函数的单选项的 man 页面.

- `curl_easy_setopt()`选项开始于`curlOPT_`,
- `curl_multi_setopt()`选项开始于`curlMOPT_`和
- `curl_easy_getinfo()`选项开始于`curlINFO_`.

### docs/examples

包含 100 个独立的示例,以帮助读者理解如何使用 libcurl.

也见[libcurl的例子](libcurl-examples.zh.md),这本书的章节.

### scripts

手提脚本.

- `contributors.sh`: 从给定的 hash/tag 中提取 Git存储库中的所有贡献者. 其目的是为 RELEASE-NOTES文件 生成一个列表,并允许手动添加的名称即使在更新时也保留在该列表中.脚本使用"THANKS-filter"文件重写一些名称.

- `contrithanks.sh`: 从给定的 hash/tag 提取 Git 存储库中的贡献者, 过滤掉在`THANKS`所有已经提到的名称.然后输出到`THANKS`,在最后附上新贡献者的列表; 这意味着允许更容易地更新感谢文档.脚本使用"THANKS-filter"文件重写一些名称.

- `log2changes.pl`: 生成发布时`CHANGES`文件的脚本,作为发布脚本使用. 它简单地转换 Git 日志输出.

- `zsh.pl`: 帮助 ZSH shell 的用户,提供 curl 命令行tab补充.
