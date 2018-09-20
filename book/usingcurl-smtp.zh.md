
# SMTP

SMTP代表[简单邮件传输协议](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol).

curl支持将数据发送到SMTP服务器,该服务器与正确的一组命令行选项相结合,使电子邮件发送到您选择的一组接收器.

当用SURL发送SMTP时,有两个必要的命令行选项**必须**被使用.

-   您需要告诉服务器至少一个收件人`--mail-rcpt`. 你可以多次使用这个选项,然后CURL会告诉服务器所有的电子邮件地址都应该收到电子邮件.

-   您需要告诉服务器哪个电子邮件地址是电子邮件的发送者.`--mail-from`. 重要的是要认识到,这个电子邮件地址不一定是相同的,如在`From:`电子邮件文本行.

然后,您需要提供实际的电子邮件数据.这是一个根据文本格式化的(文本)文件[RFC 5322](https://tools.ietf.org/html/rfc5322.html). 它是一组头部和身体.头部和身体都需要被正确编码.标题通常包括`To:`,`From:`,`Subject:`,`Date:`等.

发送电子邮件的基本命令:

```
curl smtp://mail.example.com --mail-from myself@example.com --mail-rcpt
receiver@example.com --upload-file email.txt
```

## 实例EMAL.TXT

```
From: John Smith <john@example.com>
To: Joe Smith <smith@example.com>
Subject: an example.com example email
Date: Mon, 7 Nov 2016 08:45:16

Dear Joe,
Welcome to this example email. What a lovely day.
```

## 安全邮件传输

一些邮件提供者允许或需要使用SMTP的SSL.它们可以为SSL使用专用端口,或者允许在纯文本连接上进行SSL升级.

如果您的邮件提供程序有一个专用的SSL端口,您可以使用smtps://而不是smtp://,默认情况下,它使用465的SMTP SSL端口,并且要求整个连接是SSL.例如,SMTP://SMTP.gmail.

但是,如果您的提供商允许从明文升级到安全传输,您可以使用以下选项之一:

```
--ssl           Try SSL/TLS (FTP, IMAP, POP3, SMTP)
--ssl-reqd      Require SSL/TLS (FTP, IMAP, POP3, SMTP)
```

你可以告诉科尔*尝试*但不需要通过添加来升级到安全传输.`--ssl`命令:

```
curl --ssl smtp://mail.example.com --mail-from myself@example.com
     --mail-rcpt receiver@example.com --upload-file email.txt
```

你可以告诉科尔*要求*通过添加使用安全传输升级`--ssl-reqd`命令:

```
curl --ssl-reqd smtp://mail.example.com --mail-from myself@example.com
     --mail-rcpt receiver@example.com --upload-file email.txt
```

## SMTP URL

SMTP请求的路径部分指定在与邮件服务器通信时呈现的主机名.如果路径被省略,那么CURL将试图找出本地计算机的主机名并使用它.但是,这可能不会返回一些邮件服务器所需的完全限定的域名,并且指定此路径允许您设置备选名称,例如您的机器的完全限定的域名,您可能从外部函数(如gethostname或getad)获得该名称.DrnField.

连接到邮件服务器`mail.example.com`并在Helo/EHLO命令中发送本地计算机的主机名:

```
curl smtp://mail.example.com
```

你当然可以一如既往地使用`-v`选项以查看客户端-服务器通信.

取而代之的是卷发`client.example.com`在`HELO` / `EHLO`命令到邮件服务器`mail.example.com`,用途:

```
curl smtp://mail.example.com/client.example.com
```

## 没有MX查找!

当您使用普通邮件客户端发送电子邮件时,它将首先检查要向其发送电子邮件的特定域的MX记录.如果您发送电子邮件到Joe@ ExpLo.com,客户端将获得MX记录`example.com`了解发送电子邮件给ExpLo.com用户时要使用的邮件服务器.

cURL本身没有MX查找.如果您想确定为特定域向哪个服务器发送电子邮件,我们建议您首先确定这一点,然后调用curl来使用这些服务器.有用的命令行工具获得MX记录,包括"挖掘"和"NSLoopUp".
