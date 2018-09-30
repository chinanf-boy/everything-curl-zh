
# HTTP请求

HTTP请求是当它告诉服务器要做什么时,CURL向服务器发送的请求.当它想要获取数据或发送数据时.所有涉及HTTP的传输都是从HTTP请求开始的.

HTTP请求包含方法、路径、HTTP版本和一组请求标头.当然,使用应用程序的LIcCURL可以调整所有这些字段.

## 请求方法

每一个HTTP请求都包含一个"方法",有时称为"动词".它通常类似于GET、Head、PoST或PoT,但也有一些类似删除、补丁和选项等深奥的内容.

通常,当使用libcurl设置和执行传输时,特定的请求方法是由您使用的选项所暗示的.如果你只需要一个URL,就意味着该方法将是`GET`如果你设置为例`CURLOPT_POSTFIELDS`这将使libcurl使用`POST`方法.如果你设置`CURLOPT_UPLOAD`如果是真的,libcurl将发送一个`PUT`方法在其HTTP请求等方面.请求`CURLOPT_NOBODY`将使用cURL`HEAD`.

但是,有时这些默认HTTP方法不够好,或者根本不是您希望传输使用的方法.然后,您可以指示libcurl使用您喜欢的特定方法.`CURLOPT_CUSTOMREQUEST`. 例如,您想发送一个`DELETE`方法到您选择的URL:

```
curl_easy_setopt(curl, CURLOPT_CUSTOMREQUEST, "DELETE");
curl_easy_setopt(curl, CURLOPT_URL, "https://example.com/file.txt");
```

CurLopttCuestRevices设置应该仅是HTTP请求行中作为方法使用的单个关键字.如果您想更改或添加附加的HTTP请求头,请参阅下面的章节.

## 自定义HTTP请求报头

当libcurl发出HTTP请求作为执行您所请求的数据传输的一部分时,它当然会用一组适合于完成交给它的任务的HTTP头将它们发送出去.

如果只给了URL[http://LoalHoal/Fiel1.txt](http://localhost/file1.txt"),libcurl 7.51.0将向服务器发送以下请求:

```
GET /file1.txt HTTP/1.1
Host: localhost
Accept: */*
```

如果你要指示你的应用程序也设置`CURLOPT_POSTFIELDS`对于字符串"FooBar"(6个字母,这里仅用于视觉分隔符的引号),它将发送以下标题:

```
POST /file1.txt HTTP/1.1
Host: localhost
Accept: */*
Content-Length: 6
Content-Type: application/x-www-form-urlencoded
```

如果您对libcurl发送的默认头部集不满意,则应用程序有权在HTTP请求中添加、更改或删除头部.

### 添加页眉

若要添加在请求中不存在的标题,请将其添加到`CURLOPT_HTTPHEADER`. 假设您需要一个标题`Name:`包含`Mr. Smith`:

```
struct curl_slist *list = NULL;
list = curl_slist_append(list, "Name: Mr Smith");
curl_easy_setopt(curl, CURLOPT_HTTPHEADER, list);
curl_easy_perform(curl);
curl_slist_free_all(list); /* free the list again */
```

### 更改页眉

如果其中一个默认标题不符合您的满意,您可以更改它.就像你认为违约`Host:`标头是错误的(即使它是从您给出的libcurl的URL派生的),您可以告诉您自己的libcurl:

```
struct curl_slist *list = NULL;
list = curl_slist_append(list, "Host: Alternative");
curl_easy_setopt(curl, CURLOPT_HTTPHEADER, list);
curl_easy_perform(curl);
curl_slist_free_all(list); /* free the list again */
```

### 删除页眉

当您认为libcurl在请求中使用了确实认为不应该使用的头时,您可以很容易地告诉它从请求中删除它.就像你想带走`Accept:`标题.只需提供标题名称就不需要冒冒失失地看到冒号:

```
struct curl_slist *list = NULL;
list = curl_slist_append(list, "Accept:");
curl_easy_setopt(curl, CURLOPT_HTTPHEADER, list);
curl_easy_perform(curl);
curl_slist_free_all(list); /* free the list again */
```

### 提供没有内容的页眉

正如您在上面的章节中可能已经注意到的,如果您试图在冒号的右侧添加一个没有内容的标头,那么它将被当作一个删除指令,相反,它将完全禁止发送该标头.如果你代替*真正地*想在右边发送一个零内容的头,需要使用一个特殊的标记.必须用分号提供标题,而不是正确的冒号.喜欢`Header;`. 因此,如果您想向刚刚发出的HTTP请求添加头`Moo:`没有什么跟随冒号,你可以这样写:

```
struct curl_slist *list = NULL;
list = curl_slist_append(list, "Moo;");
curl_easy_setopt(curl, CURLOPT_HTTPHEADER, list);
curl_easy_perform(curl);
curl_slist_free_all(list); /* free the list again */
```

## 推荐者

这个`Referer:`header(是的,拼写错误)是一个标准的HTTP header,它告诉服务器用户代理从哪个URL指向它现在请求的URL.它是一个普通的标题,这样你就可以自己设置`CURLOPT_HEADER`方法如上所示,也可以使用快捷方式称为`CURLOPT_REFERER`. 这样地:

```
curl_easy_setopt(curl, CURLOPT_REFERER, "https://example.com/fromhere/");
curl_easy_perform(curl);
```

### 自动引用器

当LIGURL被要求跟踪时,用`CURLOPT_FOLLOWLOCATION`选项,您仍然希望拥有`Referer:`标题设置为正确的前URL从哪里做重定向,您可以要求libcurl自己设置:

```
curl_easy_setopt(curl, CURLOPT_FOLLOWLOCATION, 1L);
curl_easy_setopt(curl, CURLOPT_AUTOREFERER, 1L);
curl_easy_setopt(curl, CURLOPT_URL, "https://example.com/redirected.cgi");
curl_easy_perform(curl);
```
