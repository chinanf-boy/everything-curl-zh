
## HTTP POST

POST是HTTP方法,它被发明用来，将数据发送到接收Web应用程序, 对应的就是最常见的HTML表单的web工作方式.它通常发送一小块相对少量的数据给接收端.

当数据填写完表单后,由浏览器发送数据时,它将发送"URL编码",用符号('&')分隔的序列化`name=value`对.cURL用`-d`或`--data`发送这样的数据:

```
curl -d 'name=admin&shoesize=12' http://example.com/
```

当指定多个`-d`选项,curl将把它们连接起来,并在它们之间插入符号,所以上面的示例也可以这样进行:

```
curl -d name=admin -d shoesize=12 http://example.com/
```

如果要发送的数据量，并不适合在命令行上堆成一个字符串,那么您也可以用标准cURL样式，选择从文件名中读取它:

```
curl -d @filename http://example.com
```

### Content-Type，内容类型

curl的`-d`选项的POST方式, 它的默认头部将包括一个像`Content-Type: application/x-www-form-urlencoded`. 这就是浏览器一个典型的简单POST.

许多POST数据接收者，并不关心或检查内容类型报头.

如果那个报头，你觉得不够好,你应该替换它,提供正确的.例如,如果将JSON发布到服务器,并希望更准确地告诉服务器内容是什么:

```
curl -d '{json}' -H 'Content-Type: application/json' https://example.com
```

### POST出二进制

若是文件读取时,`-d`会剥离回车和换行. 如果您希望cURL读取和使用给定文件的二进制完全一样，你可以使用`--data-binary`:

```
curl --data-binary @filename http://example.com/
```

### URL编码

百分号`%`编码,也称为URL编码,在技术上是一种编码数据的机制,以便它可以出现在URL中.此编码通常用于带有`application/x-www-form-urlencoded`内容类型的POST方式,正如cURL带有`--data`和`--data-binary`的发送.

上面提到的命令行选项，都要求您提供正确编码的数据,即需要确保数据已经以正确格式存在.虽然这给了你很多自由,但有时也有点不方便.

为了帮助您发送尚未编码的数据,curl提供了`--data-urlencode`选项.这个选项提供了几种不同的URL编码方法.

你可以像`--data-urlencode data`这样，与另一个`--data`选项相同的方式使用它.要符合CGI,**data**部分应该从名称开始,然后是分隔符和特定内容.这个**data**部分使用以下语法之一，就可以传递给cURL:

-   "content":这将使curl URL编码内容，并传递该内容. 要小心,content不包含任何=或@符号, 因为这样会使语法与下面的其他情况对上了!

-   "=content":这将使curl URL编码内容并传递该内容.初始的'=’符号不包含在数据中.

-   "name=content":这将使curl URL编码内容部分并传递该内容.请注意,name部分已经被URL编码了.

-   "@filename":这将使cURL从给定文件(包括任何换行)加载数据,URL编码该数据并在POST中传递.

-   "name@filename":这将使cURL从给定文件(包括任何换行)加载数据, URL编码该数据并在POST中传递. name部分得到等号附加,导致name=urlencoded-文件内容.注意,名称已经被URL编码了.

举个例子,你可以POST一个名字，让它用curl编码:

```
curl --data-urlencode "name=John Doe (Junior)" http://example.com
```

这将在实际请求正文中，发送以下数据:

```
name=John%20Doe%20%28Junior%29
```

如果存储字符串`John Doe (Junior)`在一个名为`contents.txt`的文件中，您可以使用字段名"用户"告诉curl编码发送的内容URL:

```
curl --data-urlencode user@contents.txt http://example.com
```

在上述两个示例中,字段名称不是URL编码,而是按原样传递.如果您还想URL编码字段名,就像您想传递一个名为"user name"的字段名一样,您可以要求curl编码整个字符串，通过添加前缀=符号(这符号不会发送):

```
curl --data-urlencode "=user name=John Doe (Junior)" http://example.com
```

### 将其转换为GET

一个小的便利功能,可以适合在这方面提到(即使不是为POST准备的),就是`-G`或`--get`选项,它用不同的方法将你指定`-d`的所有数据变量,以"?"附加数据在URL的右端，然后让curl发送一个GET.

例如,这个选项可以轻松地在 POST 和GET 表单之间切换.

### 期待100继续

HTTP没有适当的方式阻止正在进行的传输(在任何方向),且仍保持连接.因此,如果我们发现传输最好在传输开始之后停止,那么只有两种方式可以继续进行:切断连接,并为下一个请求付出重新建立连接的代价,或者继续传输并浪费更多的带宽,但下次能够重用连接.

这种情况发生的一个例子是，当您通过HTTP发送一个大文件时,发现服务器需要身份验证，并立即返回401响应代码.

使这种情况不那么频繁的缓解，是进行cURL传递加多个额外的报头,`Expect: 100-continue`。这给服务器在大量数据发送之前，拒绝请求的机会.curl发送此期望:默认情况下，如果已知POST或怀疑,它仅仅小.cURL的PUT请求也是同样的.

当服务器收到具有100-continue的请求，并认为该请求正常时,它将100个响应给客户端继续.如果服务器不喜欢这个请求,它会为它认为的错误发回响应代码.

不幸的是,世界上很多服务器都不能正确地支持Expect:header,或者不能正确地处理它,所以curl将只等待1000毫秒，期待第一个响应,然后才会继续执行.

这是1000个浪费的毫秒.然后可以取消使用: 避免等待请求`-H`:

```
curl -H Expect: -d "payload to send" http://example.com
```

在某些情况下,如果将要发送的内容非常小(比如低于1千字节),则curl将禁止使用Expect头部,因为必须"浪费"这么一小块数据，并不算什么问题.

### 成块编码POSTs

当与HTTP 1.1服务器交谈时,您可以告诉curl发送不代`Content-Length:`标题的请求主体，这样就不知前面POST确切有多大。通过坚持使用分块传输编码(chun.Transfer-Encoding)的curl,curl将以一种特殊的样式逐个发送POST"块",还随着块的进行而发送每个块的大小.

你可以发送一个这样的cURL POST:

```
curl -H "Transfer-Encoding: chunked" -d "payload to send" http://example.com
```

### 隐藏表单字段

本章说明了如何发送带`-d`的POST。相当于HTML表单被填写和提交时，浏览器所做的事情.

提交这样的表格是一种非常常见的cURL操作,以自动有效的方式，感觉就在网页.

如果您想提交一个带有curl的表单,并使它看起来像是用浏览器完成的,那么提供表单中的所有输入字段是很重要的.对于网页,一种非常常见的方法是设置一些隐藏的输入字段到表单中,并在HTML中直接为它们分配值.当然,成功的表单提交还包括这些字段,并且为了自动做到这样,您可能被迫，先下载保存表单的HTML页面,解析它,并提取隐藏的字段值,以便您可以用curl发送它们.

### 了解浏览器发送的内容

一个非常常见的快捷方式是简单地用浏览器填写表单,然后提交，并检查浏览器的网络开发工具到底发送了什么.

一种稍微不同的方法是保存包含表单的HTML页面,然后编辑该HTML页面以将表单的"action="部分，重定向到您自己的服务器或测试服务器,该测试服务器只输出表单所获得的内容.完成表单提交之后,将准确地显示浏览器如何发送它.

当然,第三种选择是使用，诸如Wireshark这样的网络捕获工具来准确地检查，通过'导线'发送的内容.如果您正在使用HTTPS,则无法在'导线'上以明文形式看到表单提交, 而是需要确保Wireshark能够从浏览器中提取TLS私钥.有关Wireshark文档的详细信息,请参阅Wireshark文档.

### JavaScript与表单

对于使用curl的自动化"代理"或脚本,一种非常常见的’缓解‘方法是，让具有HTML表单的页面使用JavaScript来设置一些输入字段的值,通常是隐藏字段之一.通常,有一些JavaScript代码在页面加载或按下提交按钮时，执行,这些代码设置了一个魔术值,服务器可以在认为提交有效之前，验证该值.

通常可以通过读取JavaScript代码和重新编写脚本中的逻辑来解决这一问题.使用上面提到的技巧来准确地查看浏览器发送的内容，也是一个很好的帮助.
