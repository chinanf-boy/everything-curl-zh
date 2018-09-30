
### 进度回调

进程回调是在传输的整个生命周期内对每个传输定期和重复调用的过程.旧的回调是用`CURLOPT_PROGRESSFUNCTION`但是,现代和首选的回调设置为`CURLOPT_XFERINFOFUNCTION`:

```
curl_easy_setopt(handle, CURLOPT_XFERINFOFUNCTION, xfer_callback);
```

这个`xfer_callback`函数必须与原型匹配:

```
int xfer_callback(void *clientp, curl_off_t dltotal, curl_off_t dlnow,
                  curl_off_t ultotal, curl_off_t ulnow);
```

如果设置了这个选项`CURLOPT_NOPROGRESS`设置为0(零),这个回调函数以频繁的间隔被libcurl调用.当数据被传输时,它将被非常频繁地调用,并且在慢速期间,比如当什么都没有被传输时,它可以减慢到大约每秒一个调用.

这个**客户群**指向私有数据集的指针`CURLOPT_XFERINFODATA`:

```
curl_easy_setopt(handle, CURLOPT_XFERINFODATA, custom_pointer);
```

回调告诉多少数据libcurl将传输和传输,以字节为单位:

-   **德尔泰**LITCURL预期在此传输中下载的总字节数.
-   **DLNE**到目前为止下载的字节数.
-   **总体的**LIPURL预期在这个传输中上传的总字节数.
-   **乌尔诺**到目前为止上传的字节数.

传递给回调的未知/未使用的参数值将被设置为0(比如,如果仅下载数据,则上传大小将保持0).很多时候,在知道数据大小之前,回调将首先被调用一次或多次,因此必须编写一个程序来处理这个问题.

从这个回调中返回一个非零值会导致libcurl中止传输和返回.`CURLE_ABORTED_BY_CALLBACK`.

如果使用多接口传输数据,则除非调用执行传输的适当libcurl函数,否则在闲置期间不会调用此函数.

(被贬低的回调)`CURLOPT_PROGRESSFUNCTION`工作相同,而不是进行类型的论证`curl_off_t`,它使用`double`)
