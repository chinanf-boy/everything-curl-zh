
### 多接口驱动

"多"的名称是多个,就像多个并行传输一样,都是在同一个线程中完成的.多API是非阻塞的,因此对于单个传输也可以使用它.

转会仍然以"容易"的方式进行.`CURL *`句柄如所述[在上面](libcurl-easyhandle.md)但是,多接口也需要多个接口.`CURLM *`创建并使用它来驱动所有单独的传输.多手柄可以"握住"一个或多个容易把手:

```
CURLM *multi_handle = curl_multi_init();
```

多个句柄也可以得到某些选项集,你可以这样做.`curl_multi_setopt()`但是,在最简单的情况下,你可能没有任何东西可以设置.

要驱动多接口传输,首先需要添加应该传输到多句柄的所有单个简单句柄.您可以在任何时候将它们添加到多句柄,并且可以随时删除它们.当然,从多个句柄中删除一个简单的句柄将删除关联,并且该特定的传输将立即停止.

在多句柄中添加一个简单的句柄非常容易:

```
curl_multi_add_handle( multi_handle, easy_handle );
```

去除一个同样容易做到:

```
curl_multi_remove_handle( multi_handle, easy_handle );
```

添加了表示要执行的传输的简单句柄,可以编写传输循环.使用多接口,可以进行循环,这样您就可以向LbCURL请求一组文件描述符和一个超时值,并执行`select()`打电话给自己,或者你可以使用稍微简化的版本,这对我们来说.`curl_multi_wait`. 最简单的循环基本上是这样的:*请注意,实际应用程序将检查返回代码.*)

```
int transfers_running;
do {
   curl_multi_wait ( multi_handle, NULL, 0, 1000, NULL);
   curl_multi_perform ( multi_handle, &transfers_running );
} while (transfers_running);
```

第四个论点`curl_multi_wait`在上面的示例中设置为1000,是毫秒中的超时.在函数返回之前,函数将等待最长的时间.你不想在呼叫之前锁定太长时间`curl_multi_perform`同样,有超时,进度回调,更多的,如果你这样做可能会降低精度.

相反,我们自己选择SO(),我们从libcurl中提取文件描述符和超时值.*请注意,实际应用程序将检查返回代码.*):

```
int transfers_running;
do {
  fd_set fdread;
  fd_set fdwrite;
  fd_set fdexcep;
  int maxfd = -1;
  long timeout;

  /* extract timeout value */
  curl_multi_timeout(multi_handle, &timeout);
  if (timeout < 0)
    timeout = 1000;

  /* convert to struct usable by select */
  timeout.tv_sec = timeout / 1000;
  timeout.tv_usec = (timeout % 1000) * 1000;

  FD_ZERO(&fdread);
  FD_ZERO(&fdwrite);
  FD_ZERO(&fdexcep);

  /* get file descriptors from the transfers */
  mc = curl_multi_fdset(multi_handle, &fdread, &fdwrite, &fdexcep, &maxfd);

  if (maxfd == -1) {
    SHORT_SLEEP;
  }
  else
   select(maxfd+1, &fdread, &fdwrite, &fdexcep, &timeout);

  /* timeout or readable/writable sockets */
  curl_multi_perform(multi_handle, &transfers_running);
} while ( transfers_running );
```

这两个循环都允许您使用一个或多个您自己的文件描述符等待,就像您从自己的套接字或管道或类似的文件中读取一样.

再次,您可以在循环期间的任意点添加和移除多句柄的简单句柄.删除一个手柄中间传输,当然会中止传输.

## 什么时候完成单次传送?

正如上面的例子所示,一个程序可以检测到一个单独的传输何时完成.`transfers_running`变量减少.

也可以打电话`curl_multi_info_read()`如果传输已经结束,那么它将返回指向结构(消息)的指针,然后您可以使用该结构找出该传输的结果.

当您执行多个并行传输时,一个以上的传输当然可以在同一个传输中完成.`curl_multi_perform`调用,然后可能需要多个调用`curl_multi_info_read`获取每个完成传输的信息.
