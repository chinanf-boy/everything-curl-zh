
# 阅读电子邮件

在因特网上有两个主要的协议用于从服务器读取/下载电子邮件(至少如果我们不计算基于Web的阅读),它们是IMAP和POP3.前者是稍有现代性的替代品.cURL支撑两者.

## POP3

列出消息号码和大小:

```
curl pop3://mail.example.com/
```

下载消息1:

```
curl pop3://mail.example.com/1
```

删除消息1:

```
curl --request DELE pop3://mail.example.com/1
```

## IMAP

TBD
