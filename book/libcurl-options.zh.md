
## 设置句柄选项

您可以在easy句柄中设置选项,以控制如何进行传输,或者在某些情况下,您可以实际设置选项,并在传输进行中修改传输行为.您设置选项`curl_easy_setopt()`并且提供句柄、要设置的选项和选项的参数.所有选项只需一个参数,并且必须始终将三个参数传递给CURLYASEYSETOPTE()调用.

由于curl\_.\_setopt()调用接受数百个不同的选项,并且各种选项接受各种不同类型的参数,因此了解细节并精确地提供特定选项支持和期望的参数类型非常重要.误入歧途会导致意想不到的副作用或难以理解的打嗝.

每个传输需要的最重要的选项是URL.libcurl不能执行传输,而不知道它关注哪个URL,所以必须告诉它.URL选项名称为`CURLOPT_URL`因为所有选项都是前缀`CURLOPT_`然后描述性名称都使用大写字母.一个示例行设置URL以获得"[HTTP://Excel](http://example.com)"HTTP内容看起来像:

```
CURLcode ret = curl_easy_setopt(easy, CURLOPT_URL, "http://example.com");
```

再次:这只设置了句柄中的选项.它不会做实际的转移或任何事情.它基本上只告诉LICORL复制字符串,如果这样,它返回OK.

当然,检查返回代码以确保没有出错,这是一个好的形式.

### 设置数值选项

因为CURLIASEASYSETopt()是一个VARARG函数,其中第三个参数可以根据情况使用不同的类型,所以不能进行正常的C语言类型转换.所以你**必须**如果文档告诉你的话,确保你真的通过了一个"长"而不是"int".在它们大小相同的架构上,您可能不会遇到任何问题,但不是所有的工作都是这样的.类似地,对于接受"CurLyOffyt"类型的选项,它是**关键的**在一个参数中传递,而不是其他类型.

长期执行:

```
curl_easy_setopt(handle, CURLOPT_TIMEOUT, 5L); /* 5 seconds timeout */
```

强制执行命令:

```
curl_off_t no_larger_than = 0x50000;
curl_easy_setopt(handle, CURLOPT_MAXFILE_LARGE, no_larger_than);
```

### 获取句柄选项

不,没有通用方法来提取以前设置的相同信息.`curl_easy_setopt()`!如果您需要能够更早地提取您先前设置的信息,那么我们鼓励您自己在应用程序中跟踪这些数据.
