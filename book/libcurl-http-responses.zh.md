
# HTTP响应

每一个HTTP请求都包含一个HTTP响应.HTTP响应是一组元数据和响应体,其中主体偶尔可以是零字节,因此不存在.然而,HTTP响应总是具有响应标头.

响应体将传递给[写回调](callback-write.md)和响应头到[报头回调](callback-header.md)但是,有时应用程序只想知道数据的大小.

响应的大小*服务器头所告知的*可以用`curl_easy_getinfo()`这样地:

```
double size;
curl_easy_getinfo(curl, CURLINFO_CONTENT_LENGTH_DOWNLOAD, &size);
```

但是如果您可以等到传输已经完成之后再进行传输,这也是一种更可靠的方式,因为并非所有URL都预先提供大小(例如,对于按需生成内容的服务器),那么您可以在最近的传输中请求下载的数据量.

```
double size;
curl_easy_getinfo(curl, CURLINFO_SIZE_DOWNLOAD, &size);
```

## HTTP响应代码

每个HTTP响应都从包含HTTP响应代码的单行开始.它是一个三位数字,包含服务器对请求的状态的想法.这些数字在HTTP标准规范中详细说明,但它们被划分成基本上像这样工作的范围:

| 代码  | 意义          |
| --- | ----------- |
| 1xx | 瞬态代码,一个新的代码 |
| 2xx | 一切都好        |
| 3xx | 内容在别的地方     |
| 4xx | 由于客户端问题而失败  |
| 5xx | 由于服务器问题而失败  |

在这样的传输之后,您可以提取响应代码.

```
long code;
curl_easy_getinfo(curl, CURLINFO_RESPONSE_CODE, &code);
```

## 关于HTTP响应代码"错误"

虽然响应代码编号可以包括服务器用来发出处理请求有错误的信号的编号(在4xx和5xx范围内),但是重要的是要认识到这不会导致libcurl返回错误.

当LIPURL被要求执行HTTP传输时,如果HTTP传输失败,它将返回一个错误.然而,获得HTTP 404或类似的返回对于libcurl不是问题.它不是HTTP传输错误.用户很可能正在编写一个客户端来测试服务器的HTTP响应.

如果您坚持将HTTP响应代码从400和上处理为cURL,libcurl提供`CURLOPT_FAILONERROR`选项,如果集合指示cURL返回`CURLE_HTTP_RETURNED_ERROR`在这种情况下.然后,它将尽快返回错误,而不传递响应体.
