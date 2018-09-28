
## 卷积码返回码

许多libcurl函数返回一个CURLCODE.这是一个特殊的libcurl类型的错误代码变量.它返回`CURLE_OK`(如果值为零)如果一切都很好,那么如果检测到问题,它会返回一个非零值.几乎有一百`CURLcode`使用中的错误,您可以在`curl/curl.h`头文件,并在LIcCURL错误手册页中记录.

可以将CURLCODE转换为一个人类可读的字符串.`curl_easy_strerror()`功能-但要注意,这些错误很少以适合任何人在UI中或向最终用户公开的方式表达:

```
const char *str = curl_easy_strerror( error );
printf("libcurl said %s\n", str);
```

在错误的情况下获得稍微好一些的错误文本的另一种方法是设置`CURLOPT_ERRORBUFFER`选项在程序中指出一个缓冲区,然后libcurl将在返回错误之前在那里存储相关的错误消息:

```
curl error[CURL_ERROR_SIZE]; /* needs to be at least this big */
CURLcode ret = curl_easy_setopt(handle, CURLOPT_ERRORBUFFER, error);
```
