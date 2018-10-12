
# 阅读电子邮件

在因特网上有两个主要的协议用于，从服务器读取/下载电子邮件(如果我们不计算基于Web的阅读的话),它们是IMAP和POP3.前者是更现代的替代品.cURL两者都支持.

## POP3

列出邮件号码和大小:

```
curl pop3://mail.example.com/
```

下载邮件1:

```
curl pop3://mail.example.com/1
```

删除邮件1:

```
curl --request DELE pop3://mail.example.com/1
```

## IMAP

TBD
