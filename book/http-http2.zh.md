
## HTTP/2

CURL支持http\://http\://http\://url 2,假设卷曲是用适当的先决条件构建的.当给定HTTPS URL时,它甚至默认使用HTTP/2,因为这样做意味着没有惩罚,并且当curl与不支持HTTP/2的站点一起使用时,请求将改为协商HTTP/1.1.

然而,使用http\://url,对HTTP/2的升级是用`Upgrade:`当看到这种报头时,可能会导致额外的往返,甚至可能更麻烦、很大一部分旧服务器将返回400个响应.

还应该注意到一些(大多数?)支持HTTP/HTTP 2的服务器://(它本身并不是所有服务器)将不承认`Upgrade:`例如,头上的标题.

要求服务器使用HTTP/2,只需:

```
curl --http2 http://example.com/
```

如果卷曲不支持HTTP/2,那么命令行会返回这样的错误.运行`curl -V`将显示你的卷曲版本支持它.

如果您碰巧已经知道服务器使用HTTP/2(例如,在您自己的受控环境中,您确切地知道机器中运行的是什么),则可以缩短HTTP/2"协商"的时间`--http2-prior-knowledge`.

### 多路复用

HTTP/2协议的主要特性之一是在同一物理连接上复用多个逻辑流的能力.当使用curl命令行工具时,您不能利用这个很酷的特性,因为curl以严格的串行方式执行其所有网络请求,一个接一个,而第二个仅在前一个结束之后才启动.

希望未来的卷发版本将得到加强,以允许使用此功能.