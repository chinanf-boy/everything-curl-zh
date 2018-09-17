
## 曲奇饼

HTTP Cookie是客户端代表服务器存储的密钥/值对.它们在随后的请求中被返回,以允许客户端在请求之间保持状态.Remember that the HTTP protocol itself has no state but instead the client has to resend all data in subsequent requests that it wants the server to be aware of.

Cookie由服务器设置为`Set-Cookie:`header and with each cookie the server sends a bunch of extra properties that need to match for the client to send the cookie back. 像域名和路径,也许最重要的cookie应该生存多久.

Cookie的到期日要么设置为未来的固定时间(或者生存数秒),要么根本没有到期.A cookie without an expire time is called a "session cookie" and is meant to live for the duration of the "session" but not longer. 在这方面的会话通常被认为是浏览站点的浏览器的生命期.关闭浏览器后,结束会话.使用支持Cookie的命令行客户端执行HTTP操作会回避会话何时真正结束的问题.

### 饼干引擎

The general concept of curl only doing the bare minimum unless you tell it differently makes it not acknowledge cookies by default. You need to switch on "the cookie engine" to make curl keep track of cookies it receives and then subsequently send them out on requests that have matching cookies.

通过请求CURL读取或写入Cookie,可以启用Cookie引擎.If you tell curl to read cookies from a non-existing file, you will only switch on the engine but start off with an empty internal cookie store:

```
curl -b non-existing http://example.com
```

但是仅仅打开cookie引擎,获取单个资源然后退出是没有意义的,因为curl实际上没有机会发送它收到的任何cookie.假设本示例中的站点将设置Cookie,然后进行重定向,我们将这样做:

```
curl -L -b non-existing http://example.com
```

### 从文件中读取Cookie

从一个空白的cookie商店开始可能不是可取的.为什么不从你以前存储的cookies或者你获得的cookies开始呢?curl用于cookie的文件格式称为Netscape cookie格式,因为它曾经是浏览器使用的文件格式,然后您可以很容易地告诉curl使用浏览器的cookie!

为方便起见,CURL还支持Cookie文件,它是一组设置Cookie的HTTP头.这是一种劣质的格式,但可能是你唯一拥有的东西.

告诉CURL从哪个文件读取初始cookie:

```
curl -L -b cookies.txt http://example.com
```

记住这只是*读数*从文件中.如果服务器将更新其响应中的cookie,curl将更新其内存存储中的cookie,但是最终当它退出时抛弃它们,并且随后调用相同的输入文件将再次使用原始cookie内容.

### 将Cookie写入文件

Cookie被存储的地方有时被称为"cookie jar".当您以curl启用cookie引擎并且它已经接收到cookie时,您可以指示curl在它存在之前将其所有已知的cookie写到一个文件,即cookie jar.重要的是要记住,curl只在退出时更新输出cookie jar,而不在其生命周期内更新,无论处理给定输入需要多长时间.

你指出cookie jar输出`-c`:

```
curl -c cookie-jar.txt http://example.com
```

`-c`是指示*写*文件的cookie,`-b`是指示*阅读*来自文件的Cookie.通常你想要两者兼而有之.

当curl向该文件写入cookie时,它将保存所有已知的cookie,包括会话cookie(没有给定的生存期).curl本身没有会话的概念,它不知道会话何时结束,所以除非您告诉它,否则它不会刷新会话cookie.

### 新cookie会话

为了避免在会话结束时告诉curl,为了刷新会话cookie,并且向服务器发出我们正在开始新会话的基本信号,curl具有一个选项,该选项允许用户决定新会话何时开始.

一个新的cookie会话意味着所有的会话cookie都会被丢弃.这相当于关闭浏览器并重新启动它.

告诉CURL一个新的cookie会话开始使用`-j, --junk-session-cookies`:

```
curl -j -b cookies.txt http://example.com/
```
