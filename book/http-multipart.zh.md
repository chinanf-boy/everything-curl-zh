
## HTTP多部分模板

多部分FasPoST是HTTP客户端在提交HTML表单时发送的内容.*编码方式*设置为"多部分/形式数据".

它是一个HTTP POST请求,请求正文被特别地格式化为一系列"部分",用MIME边界分隔.

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

用户可以在"名称"字段中填写文本,并且通过按"浏览"按钮,可以选择一个本地文件,当按下"提交"时,该文件将被上传.

### 用卷发方式发送这样的表单

使用cURL,将每个单独的多个部分加上一个`-F`(或)`--form`然后,标志和您继续为您要发送的表单中的每个输入字段添加一个-f.

上面的小示例表单有两个部分,一个名为"人",一个是纯文本字段,一个是"秘密",这是一个文件.

把你的数据发送到这样的表格:

```
curl -F person=anonymous -F secret=@file.txt http://example.com/submit.cgi
```

### 生成的HTTP

这个**行动**指定发送邮件的位置.**方法**说这是一个帖子**编码方式**告诉我们它是一个多部分的模板.

通过上面填写的字段,CURL生成并发送这些HTTP请求头到主机示例:

```
POST /submit.cgi HTTP/1.1
Host: example.com
User-Agent: curl/7.46.0
Accept: */*
Content-Length: 313
Expect: 100-continue
Content-Type: multipart/form-data; boundary=------------------------d74496d66958873e
```

**内容长度**当然,告诉服务器需要多少数据.这个例子的313个字节非常小.

这个**期待**标题在[HTTP邮局](http-post.md)章.

这个**内容类型**页眉有点特殊.它表示这是一个多部分的FasPoST,然后设置"边界"字符串.边界字符串是一行字符,其中有些地方有一组随机数字,用作将提交的表单的不同部分之间的分隔符.在这个示例中,您看到的特定边界具有随机部分.`d74496d66958873e`但是,当你运行cURL(或者当你用浏览器提交这样的表单)时,你会得到不同的东西.

因此,最初的一组标题跟随请求正文.

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

这里你清楚地看到两个部分被发送,与边界字符串分开.每一部分都从一个或多个标题开始,每个标题用它的名字来描述,可能还有一些细节.然后在部件的头出现在部件的实际数据之后,没有任何类型的编码.

最后一个边界字符串有两个额外的破折号.`--`附加信号结束.

### 内容类型

用CURL -F选项发布将使其在请求中包含默认内容类型头,如上面的示例所示.这样说`multipart/form-data`然后指定MIME边界字符串.该内容类型是多部分表单post的默认类型,但是您当然仍然可以对自己的命令进行修改,如果进行了修改,那么curl足够聪明,仍然可以将边界魔术附加到替换的标题中.您不能真正更改边界字符串,因为CURL需要用于生成后流.

若要替换页眉,请使用`-H`这样地:

```
curl -F 'name=Dan' -H 'Content-Type: multipart/magic' https://example.com
```

### 转换HTML表单

TBD
