
## 日志操作

好,我们只是演示了如何将错误作为人类可读的文本来处理,因为这对于找出在特定传输中发生了什么错误是一个极好的帮助,并且经常解释为什么可以这样处理或者目前存在什么问题.

在编写每个人都需要了解并需要广泛使用的libcurl应用程序时(至少在开发libcurl应用程序或调试libcurl本身时),下一个救生圈是启用`CURLOPT_VERBOSE`:

```
CURLcode ret = curl_easy_setopt(handle, CURLOPT_VERBOSE, 1L);
```

当libcurl被告知详细时,它将在传输过程中向stderr提及与传输相关的细节和信息.这是可怕的,找出为什么事情失败,并准确地了解什么样的cURL当你问不同的事情.可以通过改变STDRR来重定向其他地方的输出.`CURLOPT_STDERR`或者你可以用调试回调的方式获得更多的信息(在后面的部分中进一步解释).

### 追踪一切

日志无疑是好的,但有时你需要更多.libcurl还提供了一个跟踪回调,除了向您展示日志模式所需的所有内容外,它还通过*全部的*数据发送和接收,以便您的应用程序获得所有的全部踪迹.

传递给跟踪回调的发送和接收数据以其未加密形式提供给回调,当使用基于TLS或SSH的协议工作时,当从网络上捕获数据以进行调试时,回调非常方便.

当你设置`CURLOPT_DEBUGFUNCTION`选项,你仍然需要`CURLOPT_VERBOSE`启用,但跟踪回调集libcurl将使用回调而不是内部处理.

跟踪回调应该匹配这样的原型:

```
int my_trace(CURL *handle, curl_infotype type, char *ptr, size_t size,
             void *userp);
```

**手柄**是它容易处理的问题,**类型**描述传递给回调的特定数据(数据输入/输出、报头导入/输出、TLS数据输入/输出和"文本").**ptr**指向数据**大小**字节数.**美国地质调查局**您设置的自定义指针`CURLOPT_DEBUGDATA`.

指向的数据**ptr** *不会*为零终止,但将完全按照**大小**争论.

回调必须返回0或libcurl将认为它是一个错误,并中止传输.

在cURL网站上,我们举了一个例子[调试C](https://curl.haxx.se/libcurl/c/debug.html)这包括一个简单的跟踪函数来获取灵感.

还有更多的细节在[CurLopt\*Debug函数页面](https://curl.haxx.se/libcurl/c/CURLOPT_DEBUGFUNCTION.html).
