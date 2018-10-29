
## 多部分表单POST

多部分表单POST是HTTP客户端在提交HTML表单时,发送的内容.*编码方式*设置为"multipart/form-data".

它是一个HTTP POST请求,正文被特别地格式化为一系列"部分",用MIME边界分隔.

HTML的一个例子是这样的:

```
<form action="submit.cgi" method="post" enctype="multipart/form-data">
   Name: <input type="text" name="person"><br>
   File: <input type="file" name="secret"><br>
   <input type="submit" value="Submit">
</form> 
```

它可以在Web浏览器中看起来像这样:

![a multipart form](multipart-form.png)

用户可以在"name"字段中填写文本，并且通过按"浏览"按钮,可以选择一个本地文件,当按下"Submit"时,该文件将被上传.

### 用cURL方式发送这样的表单

使用cURL，将每个单独的多部分加上一个`-F`(或)`--form`选项，然后继续，为您要发送的表单中的每个输入字段添加一个`-f`.

上面的表单小示例有两个部分,一个名为"person",一个是纯文本字段,一个是"secret",这是一个文件.

把你这样的表格数据，发送:

```
curl -F person=anonymous -F secret=@file.txt http://example.com/submit.cgi
```

### 生成的HTTP

这个**action**指定发送邮件的位置.**method**说这是一个POST和**enctype**告诉我们它是一个多部分的表单post.

通过上面填写的字段,curl生成并发送这些HTTP请求头到主机example.com:

```
POST /submit.cgi HTTP/1.1
Host: example.com
User-Agent: curl/7.46.0
Accept: */*
Content-Length: 313
Expect: 100-continue
Content-Type: multipart/form-data; boundary=------------------------d74496d66958873e
```

**Content-Length**，当然,告诉服务器需要多少数据.这个例子的313个字节非常小.

这个**Expect**标题在[HTTP邮局](http-post.zh.md)章解释了.

这个**Content-Type**有点特殊.它表示这是一个multipart formpost，然后设置"boundary-边界"字符串。边界字符串是一行字符,其中有些地方有一组随机数字，用作提交表单的不同部分之间的分隔符。在这个示例中，您看到的特定边界具有的随机部分`d74496d66958873e`但,当你运行cURL(或者当你用浏览器提交这样的表单)时,你会得到不同的东西.

因此,请求的正文，带着的最初的一组标题.

```
--------------------------d74496d66958873e
Content-Disposition: form-data; name="person"

anonymous
--------------------------d74496d66958873e
Content-Disposition: form-data; name="secret"; filename="file.txt"
Content-Type: text/plain

contents of the file
--------------------------d74496d66958873e--
```

这里你清楚地看到两个部分被发送,被边界字符串分隔.每一部分都从一个或多个标题开始，每个标题用它的名字来描述,可能还有一些细节.然后这部分出现没有任何类型的编码的实际数据,.

最后一个边界字符串有两个附加额外的破折号`--`信号，结束.

### Content-Type

curl用`-F`选项POST，会在请求中包含默认Content-Type,如上面的示例所示。会有`multipart/form-data`，然后指定MIME边界字符串。该Content-Type是多部分表单post的默认类型，但是您仍可以对自己的命令进行修改，curl足够聪明知道你修改了，就会将边界魔术附加到替换的标题中。您不是真的更改了边界字符串, 而是curl之后需要用于POST流.

若要替换header,请使用`-H`，这样:

```
curl -F 'name=Dan' -H 'Content-Type: multipart/magic' https://example.com
```

### 转换HTML表单

TBD
