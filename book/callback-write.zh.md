
### 写回调

写入回调设置为`CURLOPT_WRITEFUNCTION`:

```
curl_easy_setopt(handle, CURLOPT_WRITEFUNCTION, write_callback);
```

这个`write_callback`函数必须与原型匹配:

```
size_t write_callback(char *ptr, size_t size, size_t nmemb, void *userdata);
```

一旦收到需要保存的数据,这个回调函数就被libcurl调用.*ptr*指向所传递的数据,并且该数据的大小为*大小*乘以*nmemb*.

如果未设置此回调,则默认情况下,libcurl将使用"fWrad".

在所有调用中,写回调将尽可能多地传递数据,但是它不能做出任何假设.它可能是一个字节,可能是数千个.将传递给写回调的最大体数据量在CURL.H头文件中定义:`CURL_MAX_WRITE_SIZE`(通常默认为16KB).如果`CURLOPT_HEADER`启用此传输,这使得头数据传递到写回调,您可以起床.`CURL_MAX_HTTP_HEADER`头数据传递到它的字节.这通常意味着100KB.

如果传输的文件是空的,这个函数可以用零字节数据调用.

传递给这个函数的数据不会被零终止!例如,不能使用Prtff的"%s"运算符来显示内容,也不能使用SrcPy复制它.

这个回调应该返回实际被处理的字节数.如果该数字与传递给回调函数的数字不同,它将向库发出一个错误条件.这将导致传输中止,并且使用的libcurl函数将返回.`CURLE_WRITE_ERROR`.

用户指针传入*用户数据*参数设置为`CURLOPT_WRITEDATA`:

```
curl_easy_setopt(handle, CURLOPT_WRITEDATA, custom_pointer);
```
