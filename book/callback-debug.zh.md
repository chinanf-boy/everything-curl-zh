
### 调试回调

调试回调设置为`CURLOPT_DEBUGFUNCTION`:

```
curl_easy_setopt(handle, CURLOPT_DEBUGFUNCTION, debug_callback);
```

这个`debug_callback`函数必须与原型匹配:

```
int debug_callback(CURL *handle,
                   curl_infotype type,
                   char *data,
                   size_t size,
                   void *userdata);
```

此回调函数替换库中的默认详细输出函数,并将调用所有调试和跟踪消息,以帮助应用程序理解正在发生的事情.这个*类型*参数解释了提供的数据类型:页眉、数据或SSL数据以及它流向哪个方向.

此回调的一个常用用法是获取LIcCURL发送和接收的所有数据的完整跟踪.发送给此回调的数据总是未加密版本,即使使用例如HTTPS或其他加密协议时也是如此.

此回调必须返回零或导致传输以错误代码停止.

用户指针传入*用户数据*参数设置为`CURLOPT_DEBUGDATA`:

```
curl_easy_setopt(handle, CURLOPT_DEBUGDATA, custom_pointer);
```
