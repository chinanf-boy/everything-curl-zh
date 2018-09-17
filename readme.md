# Everything-Curl [![translate-svg]][translate-list]

[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list


「 关于curl , 的 一切 」

[中文](./readme.md) | [english](https://github.com/bagder/everything-curl)


---

## 校对 🀄️

<!-- doc-templite START generated -->
<!-- repo = 'bagder/everything-curl' -->
<!-- commit = '1e4f4b8fd19b95138ad7c068835a908df7d56366' -->
<!-- time = '2018 9.4' -->
翻译的原文 | 与日期 | 最新更新 | 更多
---|---|---|---
[commit] | ⏰ 2018 9.4 | ![last] | [中文翻译][translate-list]

[last]: https://img.shields.io/github/last-commit/bagder/everything-curl.svg
[commit]: https://github.com/bagder/everything-curl/tree/1e4f4b8fd19b95138ad7c068835a908df7d56366

<!-- doc-templite END generated -->


- [ ]  [如何阅读本书](how.md)
- [ ]  [cURL项目](curl.md)
    - [ ]  [它是如何开始的](curl-started.md)
    - [ ]  [名字](curl-name.md)
    - [ ]  [curl做什么?](curl-does.md)
    - [ ]  [项目沟通](curl-comm.md)
    - [ ]  [邮件列表礼仪](curl-etiquette.md)
    - [ ]  [邮件列表](curl-maillists.md)
    - [ ]  [报告错误](curl-bugs.md)
    - [ ]  [发布](curl-releases.md)
    - [ ]  [安全](curl-security.md)
    - [ ]  [相信](curl-trust.md)
    - [ ]  [开发团队](curl-devteam.md)
    - [ ]  [卷曲的用户](curl-users.md)
    - [ ]  [未来](curl-future.md)
- [ ]  [开源](opensource.md)
    - [ ]  [执照](opensource-license.md)
    - [ ]  [版权和法律](opensource-copyright.md)
    - [ ]  [行为守则](opensource-coc.md)
    - [ ]  [发展](opensource-devel.md)
- [ ]  [源代码](sourcecode.md)
    - [ ]  [代码布局](sourcecode-layout.md)
    - [ ]  [处理构建选项](sourcecode-options.md)
    - [ ]  [代码风格](sourcecode-style.md)
    - [ ]  [特约](sourcecode-contributing.md)
    - [ ]  [报告漏洞](sourcecode-reportvuln.md)
    - [ ]  [网站](sourcecode-web.md)
- [ ]  [网络和协议](protocols.md)
    - [ ]  [网络简化](protocols-network.md)
    - [ ]  [协议](protocols-protocols.md)
    - [ ]  [卷曲协议](protocols-curl.md)
- [ ]  [命令行基础知识](cmdline.md)
    - [ ]  [命令行选项](cmdline-options.md)
    - [ ]  [选项取决于版本](cmdline-versions.md)
    - [ ]  [网址](cmdline-urls.md)
    - [ ]  [URL通配](cmdline-globbing.md)
    - [ ]  [列表选项](cmdline-listopts.md)
    - [ ]  [配置文件](cmdline-configfile.md)
    - [ ]  [密码](cmdline-passwords.md)
    - [ ]  [进度表](cmdline-progressmeter.md)
- [ ]  [使用卷曲](usingcurl.md)
    - [ ]  [详细](usingcurl-verbose.md)
    - [ ]  [持久的联系](usingcurl-persist.md)
    - [ ]  [下载](usingcurl-downloads.md)
    - [ ]  [上传](usingcurl-uploads.md)
    - [ ]  [连接](usingcurl-connections.md)
    - [ ]  [超时](usingcurl-timeouts.md)
    - [ ]  [的.netrc](usingcurl-netrc.md)
    - [ ]  [代理](usingcurl-proxies.md)
    - [ ]  [退出状态](usingcurl-returns.md)
    - [ ]  [FTP](usingcurl-ftp.md)
        - [ ]  [两个连接](ftp-twoconnections.md)
        - [ ]  [目录遍历](ftp-traversedir.md)
        - [ ]  [高级FTP使用](ftp-advanced.md)
    - [ ]  [SCP和SFTP](usingcurl-scpsftp.md)
    - [ ]  [IMAP和POP3](usingcurl-reademail.md)
    - [ ]  [SMTP](usingcurl-smtp.md)
    - [ ]  [TELNET](usingcurl-telnet.md)
    - [ ]  [TLS](usingcurl-tls.md)
        - [ ]  [SSLKEYLOGFILE](tls-sslkeylogfile.md)
    - [ ]  [调试](usingcurl-debug.md)
    - [ ]  [复制为卷曲](usingcurl-copyas.md)
    - [ ]  [卷曲的例子](curlexamples.md)
- [ ]  [如何使用curl进行HTTP](http.md)
    - [ ]  [协议基础知识](http-basics.md)
    - [ ]  [回应](http-response.md)
    - [ ]  [认证](http-auth.md)
    - [ ]  [范围](http-ranges.md)
    - [ ]  [HTTP版本](http-versions.md)
    - [ ]  [HTTPS](http-https.md)
    - [ ]  [HTTP POST](http-post.md)
    - [ ]  [Multipart formposts](http-multipart.md)
    - [ ]  [-d vs -F](http-postvspost.md)
    - [ ]  [重定向](http-redirects.md)
    - [ ]  [修改HTTP请求](http-requests.md)
    - [ ]  [HTTP PUT](http-put.md)
    - [ ]  [饼干](http-cookies.md)
    - [ ]  [HTTP / 2](http-http2.md)
    - [ ]  [HTTP备忘单](http-cheatsheet.md)
- [ ]  [建立和安装](building.md)
    - [ ]  [安装预建的二进制文件](building-binary.md)
    - [ ]  [从源头构建](building-source.md)
    - [ ]  [依赖](building-deps.md)
    - [ ]  [TLS库](building-tls.md)
        - [ ]  [BoringSSL](building-boringssl.md)
- [ ]  [libcurl基础知识](libcurl.md)
    - [ ]  [易于操作](libcurl-easyhandle.md)
    - [ ]  [驱动转移](libcurl-drive.md)
        - [ ]  [轻松驾驶](libcurl-drive-easy.md)
        - [ ]  [用多驱动器](libcurl-drive-multi.md)
        - [ ]  [用multi_socket驱动](libcurl-drive-multi-socket.md)
    - [ ]  [连接重用](libcurl-connectionreuse.md)
    - [ ]  [回调](libcurl-callbacks.md)
        - [ ]  [写数据](callback-write.md)
        - [ ]  [读取数据](callback-read.md)
        - [ ]  [进度信息](callback-progress.md)
        - [ ]  [标题数据](callback-header.md)
        - [ ]  [调试](callback-debug.md)
        - [ ]  [sockopt的](callback-sockopt.md)
        - [ ]  [SSL上下文](callback-sslcontext.md)
        - [ ]  [寻求和ioctl](callback-seek.md)
        - [ ]  [网络数据转换](callback-conversions.md)
        - [ ]  [Opensocket和closesocket](callback-openclosesocket.md)
        - [ ]  [SSH密钥](callback-sshkey.md)
        - [ ]  [RTSP交错数据](callback-rtsp.md)
        - [ ]  [FTP匹配](callback-ftpmatch.md)
    - [ ]  [清理](libcurl-cleanup.md)
    - [ ]  [名称解析](libcurl-names.md)
    - [ ]  [代理](libcurl-proxies.md)
    - [ ]  [转移后信息](libcurl-getinfo.md)
    - [ ]  [在句柄之间共享数据](libcurl-sharing.md)
    - [ ]  [API兼容性](libcurl-api.md)
    - [ ]  [--libcurl](libcurl--libcurl.md)
    - [ ]  [头文件](libcurl-headers.md)
    - [ ]  [全局初始化](libcurl-globalinit.md)
    - [ ]  [多线程](libcurl-threading.md)
    - [ ]  [卷曲简单的选择](libcurl-options.md)
    - [ ]  [CURLcode返回码](libcurl-curlcode.md)
    - [ ]  [详细操作](libcurl-verbose.md)
    - [ ]  [libcurl示例](libcurlexamples.md)
    - [ ]  [对于C ++程序员](libcurl-cplusplus.md)
- [ ]  [带有libcurl的HTTP](libcurl-http.md)
    - [ ]  [HTTP响应](libcurl-http-responses.md)
    - [ ]  [HTTP请求](libcurl-http-requests.md)
    - [ ]  [HTTP版本](libcurl-http-versions.md)
    - [ ]  [HTTP范围](libcurl-http-ranges.md)
    - [ ]  [HTTP身份验证](libcurl-http-auth.md)
    - [ ]  [与libcurl的Cookie](libcurl-http-cookies.md)
    - [ ]  [下载](libcurl-http-download.md)
    - [ ]  [上传](libcurl-http-upload.md)
- [ ]  [绑定](bindings.md)
- [ ]  [libcurl内部](internals.md)
- [ ]  [指数](bookindex.md)


### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[help me live , live need money 💰](https://github.com/chinanf-boy/live-need-money)

---

### 目录

<!-- START doctoc -->
<!-- END doctoc -->


# 介绍

*一切卷曲*它是关于curl、项目、命令行工具、库、一切如何开始以及它是如何变成今天的内容的广泛指南.我们如何进一步开发它,使用它需要什么,如何对代码和bug报告做出贡献,以及为什么数百万现有用户都使用它.

这本书对休闲读者和稍微有经验的开发人员来说都是有意思和有用的,并且提供了一些供大家选择和选择的东西.不要从头到脚读.阅读你感兴趣的章节,按照你认为合适的顺序来回走动.

我希望运行这本书项目,因为我做所有其他项目,我的工作:在开放,完全免费下载和阅读,任何人免费评论,提供给大家的贡献和帮助.发送错误报告,向我提出请求或批评,我将相应地改进这本书.

这本书永远也完不完.我打算继续工作,虽然有时我会认为它相当完整,涵盖了项目的大部分方面(即使这看起来是一个不可逾越的目标),但是curl项目将继续移动,所以书中总是有需要更新的内容.

本书于2015年9月底启动.

## 图书网站

**[HTTPS://BooCurl HAXX.SE](https://bookcurl.haxx.se)**是这本书的家.它的特点是使用PDF、ePUB和MOBI等多种不同版本中的一个,可以在线阅读网络版本的书籍,或者下载用于脱机阅读的副本.

**<https://ec.haxx.se>**是这本书的HTML版本的快捷方式.

**[HTTPS://Github. COM/Babd/任意卷曲](https://github.com/bagder/everything-curl)**主持所有的书籍内容.

## 作者

希望成为这篇文章的合著者,我是Daniel Stenberg.我创立了卷曲项目.我是一个内心深处的开发者,为了乐趣和利润.我在瑞典的斯德哥尔摩生活和工作.

所有关于我的知识都可以找到[我的网站](https://daniel.haxx.se/).

## 救命!

如果在本文件中发现错误、遗漏、错误或明显的谎言,请将受影响的段落的更新版本发给我,我将修改版本.我会给每个有帮助的人提供适当的学分!我希望随着时间的推移,这份文件更好.

最好是你提交[错误](https://github.com/bagder/everything-curl/issues)或[拉动请求](https://github.com/bagder/everything-curl/pulls)在这本书的GITHUB页面上.

## 许可证

这份文件是在[创作共享署名4许可](https://creativecommons.org/licenses/by/4.0/).
