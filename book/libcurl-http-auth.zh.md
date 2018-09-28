
# HTTP认证

libcurl支持广泛的HTTP认证方案.

注意,这种身份验证方式不同于现今web上非常广泛使用的方案,其中身份验证由HTTP POST执行,然后将状态保存在cookie中.见[曲奇饼](libcurl-http-cookies.md)有关如何做到这一点的细节.

## 用户名和密码

如果没有给定的用户名,libcurl将不尝试任何HTTP身份验证.设置如下:

```
 curl_easy_setopt(curl, CURLOPT_USERNAME, "joe");
```

当然,大多数认证也需要设置一个单独设置的密码:

```
 curl_easy_setopt(curl, CURLOPT_PASSWORD, "secret");
```

这就是你所需要的.这将使libcurl在其传输的默认身份验证方法上切换:*基本HTTP协议*.

## 要求认证

客户端本身并没有决定要发送一个经过验证的请求.这是服务器需要的东西.当服务器具有受保护且需要身份验证的资源时,它将以401 HTTP响应和`WWW-Authenticate:`标题.该报头将包含关于该资源接受何种特定身份验证方法的详细信息.

## 基本的

BASIC是默认的HTTP身份验证方法,顾名思义,它确实是基本的.它接受名称和密码,用冒号分隔它们,并在将整个内容放入`Authorization:`请求中的HTTP报头.

如果名称和密码设置为上面所示的示例,则确切的输出头看起来像这样:

```
Authorization: Basic am9lOnNlY3JldA==
```

这种认证方法在HTTP上完全不安全,因为凭据将通过纯文本在网络上发送.

您可以明确地告诉libcurl使用基本方法来进行这样的特定传输:

```
curl_easy_setopt(curl, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
```

## 文摘

另一种身份验证方法称为摘要.这种方法有一个优点

## NTLM

TBD

## 谈判

TBD

## 持票人

TBD

## 先试一试

TBD
