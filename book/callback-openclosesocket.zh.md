
# OpenSKET和CeleSoCKET回调

有时,您会希望应用程序更精确地控制套接字libcurl将用于其操作的内容.libcurl提供了一对回调,它取代了libcurl自己的调用.`socket()`随后`close()`相同的文件描述符.

## 提供文件描述符

通过设置`CURLOPT_OPENSOCKETFUNCTION`回调,您可以提供一个自定义函数返回一个文件描述符用于libcurl以使用:

```
curl_easy_setopt(handle, CURLOPT_OPENSOCKETFUNCTION, opensocket_callback);
```

这个`opensocket_callback`函数必须与原型匹配:

```
curl_socket_t opensocket_callback(void *clientp,
                                  curlsocktype purpose,
                                  struct curl_sockaddr *address);
```

回调得到*客户群*作为第一个参数,它只是一个不透明的指针.`CURLOPT_OPENSOCKETDATA`.

其他两个参数传递的数据标识*目的*和*地址*插座是要使用的.这个*目的*是具有值的Type`CURLSOCKTYPE_IPCXN`或`CURLSOCKTYPE_ACCEPT`基本上识别在何种情况下创建了套接字."."情况是当使用libcurl来接受FTP活动模式时的传入FTP连接时,libcurl为其自己的传出连接创建套接字的所有其他情况*IPCXN*传递值.

这个*地址*指针指向`struct curl_sockaddr`描述创建该套接字的网络目的地的IP地址.例如,回调可以使用这些信息来白名单或黑名单特定的地址或地址范围.

如果希望提供某种网络过滤器或转换层,则还显式地允许套接字打开回调修改该结构中的目标地址.

回调应该返回文件描述符或`CURL_SOCKET_BAD`这将在libcurl中造成不可恢复的错误,并且最终返回.`CURLE_COULDNT_CONNECT`从它的执行功能来看.

如果要返回文件描述符,则为*已经连接*对于服务器,则还必须设置[Sokopt回调](callback-sockopt.md)并确保返回正确的返回值.

这个`curl_sockaddress`结构看起来像这样:

```
struct curl_sockaddr {
  int family;
  int socktype;
  int protocol;
  unsigned int addrlen;
  struct sockaddr addr;
};
```

## 套接字关闭回调

对应于打开的套接字的回调当然是关闭套接字.通常,当您提供自定义的方法来提供文件描述符时,您也要提供自己的清理版本:

```
curl_easy_setopt(handle, CURLOPT_CLOSEOCKETFUNCTION, closesocket_callback);
```

这个`closesocket_callback`函数必须与原型匹配:

```
int closesocket_callback(void *clientp, curl_socket_t item);
```
