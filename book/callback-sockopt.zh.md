
### Sokopt回调

Sokopt回调设置为`CURLOPT_SOCKOPTFUNCTION`:

```
curl_easy_setopt(handle, CURLOPT_SOCKOPTFUNCTION, sockopt_callback);
```

这个`sockopt_callback`函数必须与原型匹配:

```
int sockopt_callback(void *clientp,
                     curl_socket_t curlfd,
                     curlsocktype purpose);
```

libcurl在创建新套接字时但在连接调用之前调用此回调函数,以允许应用程序更改特定的套接字选项.

这个**客户群**指向私有数据集的指针`CURLOPT_SOCKOPTDATA`:

```
curl_easy_setopt(handle, CURLOPT_SOCKOPTDATA, custom_pointer);
```

此回调应该返回:

-   成功之路
-   CurLySokoptTyError将不可恢复的错误信号发送到libcurl
-   CURLIOSockoptLaReRelyyx连接到信号成功,但实际上套接字实际上已经连接到目的地.
