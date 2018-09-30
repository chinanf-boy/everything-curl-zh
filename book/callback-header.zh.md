
### 报头回调

标题回调设置为`CURLOPT_HEADERFUNCTION`:

```
curl_easy_setopt(handle, CURLOPT_HEADERFUNCTION, header_callback);
```

这个`header_callback`函数必须与原型匹配:

```
size_t header_callback(char *ptr, size_t size, size_t nmemb, void *userdata);
```

一旦接收到一个报头,这个回调函数就被libcurl调用.*ptr*指向所传递的数据,并且该数据的大小为*大小*乘以*nmemb*. libcurl缓冲区标题,并仅将"完整"标题逐一传递给该回调.

传递给这个函数的数据不会被零终止!例如,不能使用Prtff的"%s"运算符来显示内容,也不能使用SrcPy复制它.

这个回调应该返回实际被处理的字节数.如果该数字与传递给回调函数的数字不同,它会向库发出一个错误条件.这将导致传输中止,并且使用的libcurl函数将返回.`CURLE_WRITE_ERROR`.

用户指针传入*用户数据*参数设置为`CURLOPT_HEADERDATA`:

```
curl_easy_setopt(handle, CURLOPT_HEADERDATA, custom_pointer);
```
