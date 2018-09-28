
### 读回调

读取回调设置为`CURLOPT_READFUNCTION`:

```
curl_easy_setopt(handle, CURLOPT_READFUNCTION, read_callback);
```

这个`read_callback`函数必须与原型匹配:

```
size_t read_callback(char *buffer, size_t size, size_t nitems, void *stream);
```

此回调函数在向数据服务器发送数据时由libcurl调用.这是一个传输,您已经设置上传数据或以其他方式发送到服务器.此回调将被反复调用,直到所有数据已被传递或传输失败.

这个**流动**指向私有数据集的指针`CURLOPT_READDATA`:

```
curl_easy_setopt(handle, CURLOPT_READDATA, custom_pointer);
```

如果未设置此回调,则默认情况下,libcurl将使用"Frad".

指针指向的数据区域**缓冲区**最多应该填满**大小**乘以**硝酸甘油酯**函数的字节数.然后,回调应该返回存储在该内存区域中的字节数,或者返回0(如果我们已经到达数据的末尾).回调还可以返回一些"神奇的"返回代码,以使libcurl立即返回失败或暂停特定的传输.见[CuropoptRead功能手册](https://curl.haxx.se/libcurl/c/CURLOPT_READFUNCTION.html)详情.
