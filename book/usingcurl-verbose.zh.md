
## 日志模式

如果您的curl命令没有执行,或返回您所期望的内容,那么您的第一个直觉反应应该总是使用`-v /
--verbose`选项获取更多信息.

当启用日志模式时,curl变得更加健谈,并将解释和显示更多的行为.它将添加信息测试并用`*`作前缀.例如,让我们看看在尝试一个简单的HTTP示例(将下载的数据保存在名为"saved"的文件中)时,curl可能会说什么:

```
$ curl -v http://example.com -o saved
* Rebuilt URL to: http://example.com/
```

好的,我们用一个cURL调用URL,它认为不完整,所以它帮助我们,在移动之前添加了一个尾部斜杠.

```
*   Trying 93.184.216.34...
```

这告诉我们,curl现在尝试连接到这个IP地址.这意味着名称"example.com"已经解析为一个或多个地址,这是第一个(可能也是唯一的地址)curl将尝试连接到的地址.

```
* Connected to example.com (93.184.216.34) port 80 (#0)
```

它奏效了! curl连接到了站点, 这里解释了名称如何映射到IP地址以及它连接到哪个端口.`(#0)`部分是cURL内部赋予这种连接的数字.如果在同一命令行中尝试多个URL,可以看到它使用更多的连接或重用连接,因此连接计数根据curl决定需要做的，可能增加或不增加.

如果我们使用 `HTTPS:` URL而不是`HTTP:` URL, 那么还将有一大堆行解释curl如何使用CA证书来验证服务器的证书,以及服务器证书中的一些细节,等等,包括选择了哪些密码和更多的TLS细节.

除了从curl内部提供的附加信息之外,`-v`日志模式还将使curl显示它发送和接收的所有头部.对于没有报头的协议(如FTP、SMTP、POP3等),我们可以将命令和响应视为报头,因此`-v`也将显示它们.

如果我们继续从上面的命令中看到的输出(但是忽略实际的HTML响应),curl将显示:

```
> GET / HTTP/1.1
> Host: example.com
> User-Agent: curl/7.45.0
> Accept: */*
>
```

这是对站点的完整HTTP请求.这个请求是默认`curl 7.45.0`使用中的外观,当然,在不同的版本之间可能会略有不同,特别是如果添加命令行选项,它会发生更改.

HTTP请求标头的最后一行看起来是空的,它是.它表明头部和body之间的分离,在这个请求中没有"body"发送.

继续进行,并假设一切按计划进行,发送的请求将从服务器获得相应的响应,并且HTTP响应,会有一组头部开始在响应主体之前:

```
< HTTP/1.1 200 OK
< Accept-Ranges: bytes
< Cache-Control: max-age=604800
< Content-Type: text/html
< Date: Sat, 19 Dec 2015 22:01:03 GMT
< Etag: "359670651"
< Expires: Sat, 26 Dec 2015 22:01:03 GMT
< Last-Modified: Fri, 09 Aug 2013 23:54:35 GMT
< Server: ECS (ewr/15BD)
< Vary: Accept-Encoding
< X-Cache: HIT
< x-ec-custom-error: 1
< Content-Length: 1270
<
```

这可能在你看来像巨无霸,但这是一组关于响应的HTTP头元数据.第一行的"200"可能是其中最重要的一条信息,意思是"一切都很好".

正如您所看到的,所接收的报头的最后一行是空的,这是HTTP协议用来通知报头结束的标记.

在报头到达实际响应体后,会有数据的有效载荷.这`-v`日志模式不显示数据内容,只显示

```
{ [1270 bytes data]
```

那1270个字节应该在`saved`文件中.您还可以看到,在响应中有一个名为`Content-Length:`的头部,它包含准确的文件长度(它不会总是出现在响应中).

### --trace 和 --trace-ascii

有时`-v`是不够的.特别是,当您想要存储包括实际传输的数据的完整流时.

对于curl使用的`HTTPS、FTPS或SFTP`等协议进行加密文件传输的情况,其他网络监视工具(如Wireshark或tcpdump)将无法为您轻松地完成这项工作.

为此,curl提供了另外两个选项,而不是使用`-v`.

`--trace [filename]`将在给定的文件名中保存完整的跟踪.您也可以使用'-'(减号),来将其传递到stdout，而不是文件名.你会这样使用它:

```
$ curl --trace dump http://example.com
```

完成后,有一个`dump`文件,它可以相当大.在这种情况下,'dump'文件的最先的15行看起来如下:

```
== Info: Rebuilt URL to: http://example.com/
== Info:   Trying 93.184.216.34...
== Info: Connected to example.com (93.184.216.34) port 80 (#0)
=> Send header, 75 bytes (0x4b)
0000: 47 45 54 20 2f 20 48 54 54 50 2f 31 2e 31 0d 0a GET / HTTP/1.1..
0010: 48 6f 73 74 3a 20 65 78 61 6d 70 6c 65 2e 63 6f Host: example.co
0020: 6d 0d 0a 55 73 65 72 2d 41 67 65 6e 74 3a 20 63 m..User-Agent: c
0030: 75 72 6c 2f 37 2e 34 35 2e 30 0d 0a 41 63 63 65 url/7.45.0..Acce
0040: 70 74 3a 20 2a 2f 2a 0d 0a 0d 0a                pt: */*....
<= Recv header, 17 bytes (0x11)
0000: 48 54 54 50 2f 31 2e 31 20 32 30 30 20 4f 4b 0d HTTP/1.1 200 OK.
0010: 0a                                              .
<= Recv header, 22 bytes (0x16)
0000: 41 63 63 65 70 74 2d 52 61 6e 67 65 73 3a 20 62 Accept-Ranges: b
0010: 79 74 65 73 0d 0a                               ytes..
```

每一个发送和接收的字节,都以十六进制数字单独显示.

如果你认为十六进制不起作用,你可以试试.`--trace-ascii
[filename]`取而代之,当然也接受stdout的"-",这使得跟踪的最先15行看起来像:

```
== Info: Rebuilt URL to: http://example.com/
== Info:   Trying 93.184.216.34...
== Info: Connected to example.com (93.184.216.34) port 80 (#0)
=> Send header, 75 bytes (0x4b)
0000: GET / HTTP/1.1
0010: Host: example.com
0023: User-Agent: curl/7.45.0
003c: Accept: */*
0049:
<= Recv header, 17 bytes (0x11)
0000: HTTP/1.1 200 OK
<= Recv header, 22 bytes (0x16)
0000: Accept-Ranges: bytes
<= Recv header, 31 bytes (0x1f)
0000: Cache-Control: max-age=604800
```

### --trace-time

该选项以高清晰的时间显示所有 verbose/trace 输出到打印行.它与`-v / --verbose`选项以及`--trace`和`--trace-ascii`规则一致.

一个例子可以是这样的:

```
$ curl -v --trace-time http://example.com
23:38:56.837164 * Rebuilt URL to: http://example.com/
23:38:56.841456 *   Trying 93.184.216.34...
23:38:56.935155 * Connected to example.com (93.184.216.34) port 80 (#0)
23:38:56.935296 > GET / HTTP/1.1
23:38:56.935296 > Host: example.com
23:38:56.935296 > User-Agent: curl/7.45.0
23:38:56.935296 > Accept: */*
23:38:56.935296 >
23:38:57.029570 < HTTP/1.1 200 OK
23:38:57.029699 < Accept-Ranges: bytes
23:38:57.029803 < Cache-Control: max-age=604800
23:38:57.029903 < Content-Type: text/html
---- snip ----
```

每一行都是以小时为单位:分钟:秒,然后在秒后的是微秒数.

<!-- 校对到这里 -->

### HTTP/2

当使用HTTP协议的第二个版本,HTTP/2进行文件传输时,curl发送和接收 **压缩的** 报头.因此,为了以可读和可理解的方式显示传出和传入的HTTP/2头,curl实际以与HTTP/1.1相似的样式显示未压缩版本.

### --write-out

这是一个在cURL兵工厂经常被遗忘的小宝石命令行选项.`--write-out`或者只是`-w`简写,在传输完成之后写出信息,并且它具有可以在输出中,包含大量变量、用值设置的变量以及来自传输的信息.

通过将字符串传递给该选项,您可以告诉curl编写一个字符串:

```
curl -w "formatted string" http://example.com/
```

而且,如果用`@`字符串前缀,也可以从给定文件中读取该字符串.

```
curl -w @filename http://example.com/
```

或者如果你使用`-`作为文件名,curl会从stdin中读取字符串:

```
curl -w @- http://example.com/
```

可用的变量可以通过在字符串中写入`%{variable_name}`来访问. 该变量会被正确的值替换.若要输出正常的`%`,只需将其写入`%%`.还可以使用`\n`来输出换行符,使用`\r`的回车符和带有`\t`的制表符空间.

(%-符号在Windows命令行上是特殊的,在使用此选项时,所有出现的%都必须加倍).

例如,我们可以从HTTP传输中输出Content-Type和响应代码,用换行符和一些额外的文本分隔,如下所示:

```
curl -w "Type: %{content_type}\nCode: %{response_code}\n" http://example.com
```

该特性将输出写入stdout,因此您可能希望确保,不会将下载的内容发送到stdout,因为此时可能很难分离数据和此特性输出.

#### 可用--write-out 的变量

这些变量中的一些在旧的cURL版本中是不可用的.

- `%{content_type}` : 显示请求文档的内容类型,如果有任何类型的话.

- `%{filename_effective}` : 显示了curl写入的最终文件名.这是唯一有意义的,如果cURL被告知要写入文件是`--remote-name`或`--output`选项. 最有用的`--remote-header-name`组合选项.

- `%{ftp_entry_path}` : 显示在登录到远程FTP服务器时结束的初始路径cURL.

- `%{response_code}` : 显示在最后一次传输中发现的数字响应代码.

- `%{http_connect}` : 显示了在最后一个响应(从代理)到cURL连接请求中找到的数字代码.

- `%{local_ip}` : 显示最近完成的连接的本地端的IP地址可以是IPv4或IPv6.

- `%{local_port}` : 显示最近连接的本地端口号.

- `%{num_connects}` : 显示最近传输中新连接的数目.

- `%{num_redirects}` : 显示请求中所遵循的重定向次数.

- `%{redirect_url}` : 显示实际URL *将*带您到一个重定向,若HTTP请求没有`-L`跟随重定向时.

- `%{remote_ip}` : 显示最近建立的连接的远程IP地址可以是IPv4或IPv6.

- `%{remote_port}` : 显示最近创建的连接的远程端口号.

- `%{size_download}` : 显示下载的字节总数.

- `%{size_header}` : 显示下载的页眉的总字节数.

- `%{size_request}` : 显示HTTP请求中发送的字节总数.

- `%{size_upload}` : 显示了上传的字节总数.

- `%{speed_download}` : 显示以每秒钟字节为单位测量的完整下载的cURL的平均下载速度.

- `%{speed_upload}` : 显示了平均上传速度,即以字节为单位每秒测量完整上传的cURL速度.

- `%{ssl_verify_result}` : 显示了请求的SSL对等证书验证的结果.0表示验证成功.

- `%{time_appconnect}` : 以秒为单位显示从开始到完成到远程主机的SSL/SSH/etc连接/握手所需的时间.

- `%{time_connect}` : 以秒为单位显示从开始到完成到远程主机(或代理)的TCP连接所花费的时间.

- `%{time_namelookup}` : 显示从开始到开始,直到名称解析完成的时间,以秒为单位.

- `%{time_pretransfer}` : 显示从一开始就花费了几秒钟的时间,直到文件传输即将开始.这包括所有预传输命令和特定于特定协议的协商.

- `%{time_redirect}` : 显示在开始事务之前所有重定向步骤(包括名称查找、连接、预传输和传输)所花费的时间,以秒为单位. 也显示了多个重定向的完整执行时间.

- `%{time_starttransfer}` : 显示从一开始就花费的时间,直到第一个字节即将被传输.这包括time_pretransfer以及服务器计算结果所需的时间.

- `%{time_total}` : 显示整个操作持续的总时间,以秒为单位.时间将以毫秒分辨率显示.

- `%{url_effective}` : 显示上次获取的URL.这是特别有意义的,如果您已经告诉curl跟着位置: headers (用`-L`)

### 安静

当然,日志的反义词是使cURL变得更安静.与`-s`(或)`--silent`选项使cURL关闭进度表,不输出任何错误消息,当出现错误时.它变得哑巴了.它仍然会输出你要求下载的数据.

通过激活安静,您可以要求它通过添加`-S`或`--show-error`仍输出错误消息.
