
## 上载

上传是将数据发送到远程服务器的术语. 每个协议的上传是不同的, 其中几个协议甚至可以允许用不同方式上传数据.

### 允许上传的协议

可以使用以下协议之一上传数据:FILE、FTP、FTPS、HTTP、HTTPS、IMAP、IMAPS、SCP、SFTP、SMB、SMBS、SMTP、SMTPS和TFTP.

### HTTP提供了几个"上传"

HTTP(及其大哥HTTPS)提供了几种不同方法, 将数据上传到服务器, curl为此提供了简单的命令行选项,可以采用下面描述的三种最常用的方法进行上传.

HTTP的一个有趣的细节是,在同一个操作中,上传也可以是下载,实际上许多下载都是通过HTTP POST发起的.

#### POST

POST是HTTP方法,它被发明用于将数据发送到接收Web应用程序,并且它是web上最常见的HTML形式. 常常发送一小块少量的数据给接收端.

上传主要通常`-d`或`--data`选项完成,但有一些额外的更改.

可阅读有关如何在cURL中做到这一点的[cURL的HTTP POST 方法](http-post.zh.md)章的详细说明.

#### POST:多数据-form

> multipart formpost , 指得是使用 `<form>`的POST方式, 下面我会仅说 **这种方式**

在Web站点, HTML form 的标签使用的 **这种方式**; 常常在涉及到文件上传时. 这种类型的上传也是一个HTTP POST, 但它根据一些特殊的规则发送格式化的数据,这就是"multipart formpost"名称的意思.

因为它发送完全不同类型的格式数据, 所以不能随意选择使用哪种类型的POST, 但是其实主要还是看, 接收服务器端的期望和可以处理的内容.

这种方式通过`-F`选项完成. 请参阅[HTTP multipart formpost](http-multipart.zh.md)章.

#### PUT

HTTP PUT是一种上传,它被设计为发送一个完整的资源,该资源原封不动地放在远程站点上,或者甚至替换那里的现有资源.也可以说,这也是目前网络上使用最少的HTTP上传方法,很多Web服务器甚至没有启用PUT.

使用`-T`选项发送HTTP上传,并上传文件:

```
curl -T uploadthis http://example.com/
```

### FTP上传

使用FTP,您可以看到您将访问的远程文件系统.您可以告诉服务器您希望上传的目录和要使用的文件名.如果你指定上传URL使用了后缀斜杠`/`,curl将把本地使用的文件名附加到URL,然后它将是当远程存储时使用的文件名:

```
curl -T uploadthis ftp://example.com/this/directory/
```

因此,如果您要选择远程端与本地使用的文件名不同的文件名,则在URL中指定它.

```
curl -T uploadthis ftp://example.com/this/directory/remotename
```

了解更多关于 FTP在curl的使用,在[curl/FTP](usingcurl-ftp.zh.md)部分.

### SMTP上传

你可能不会考虑发送电子邮件是通过"上传"完成的,但cURL它是. 您将邮件体上传到SMTP服务器. 使用SMTP , 您还需要在邮件主体中包括所需的所有电子邮件头(To:、.:、Date:,等等),因为curl根本不会添加任何电子邮件头.

```
curl -T mail smtp://mail.example.com/ --mail-from user@example.com
```

了解更多curl关于使用SMTP, [使用 curl/SMTP](usingcurl-smtp.zh.md)部分.

### 上传的进度表

一般, cURL提供进度表(参见[进度表](cmdline-progressmeter.zh.md)部分)也适用于上传. 需要记住的是,当您将输出发送到stdout时,进度表将自动禁用,并且大多数curl支持的协议甚至可以在上传时输出一些内容.

因此,您可能需要显式重定向下载的数据,到文件(使用shell重定向`>`,`-o`或类似的)以获得用于上传的进度表.

### 速率限制

速率限制对于上传和下载和curl的工作完全相同,实际上,只有一个限制可以限制两个方向的速度.

参见[下载速率限制部分](usingcurl-downloads.zh.md#速率限制).
