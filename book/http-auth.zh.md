
# HTTP认证

每个HTTP请求都能认证. 如果服务器或代理希望用户提供他们具有访问URL或执行操作的正确凭据的证明,那么它可以发送回HTTP响应代码,该代码通知客户端,它需要在允许的请求中，提供正确的HTTP身份验证header.

需要身份验证的服务器，返回401响应代码和相关联的`WWW-Authenticate:`，列出服务器支持的所有身份验证方法.

需要身份验证的HTTP代理，返回407响应代码和相关联的`Proxy-Authenticate:`，列出代理支持的所有身份验证方法.

值得注意的是,今天的大多数网站不需要HTTP身份验证来登录等 ,而是要求用户在网页上登录,然后浏览器将用户和密码等发出POST,然后为会话维护cookie.

若要告诉curl进行身份验证的HTTP请求,请使用`-u, --user`选项提供用户名和密码(用冒号分隔).这样:

```
curl --user daniel:secret http://example.com/
```

这将使curl使用默认的"基本"HTTP身份验证方法.是的,它实际上被称为BASIC,它是非常基础的.明确要求基本方法,使用`--basic`.

基本身份验证方法，通过网络(base64编码)以明文形式发送用户名和密码,所以HTTP传输应避免使用该验证.

当要求使用单个(指定或隐含的)身份验证方法进行HTTP传输时,curl将把已经位于连接上的第一个请求中的身份验证header插入.

如果你宁愿先cURL*测试*是否确实需要身份验证,您可以要求curl搞一搞,也就是使用`--anyauth`自动使用它知道的最安全的方法. 这使得curl尝试未经身份验证的请求,然后在必要时切换到身份验证:

```
curl --anyauth --user daniel:secret http://example.com/
```

同样的概念适用于可能需要认证的HTTP操作:

```
curl --proxy-anyauth --proxy-user daniel:secret http://example.com/ \
     --proxy http://proxy.example.com:80/
```

curl通常还使用其他几种身份验证方法,包括Digest、Negotiate和 NTLM.你也可以指明使用这些方法:

```
curl --digest --user daniel:secret http://example.com/
curl --negotiate --user daniel:secret http://example.com/
curl --ntlm --user daniel:secret http://example.com/
```
