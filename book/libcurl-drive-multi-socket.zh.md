## 用"多插座"接口驱动

多套接字是常规多接口的额外辛辣版本,它是为事件驱动的应用而设计的.请务必阅读[多接口驱动](libcurl-drive-multi.md)第一节.

multi_socket 支持多个并行传输(全部在同一个线程中完成),并且用于在单个应用程序中运行数万个传输.通常情况下,如果执行大量并行传输(100 次左右),API 就变得最有意义.

在这种情况下,事件驱动意味着您的应用程序使用一个系统级库或设置,该系统级库或设置可以"订阅"多个套接字,并且它让应用程序知道这些套接字中的一个何时是可读或可写的,并且它确切地告诉您哪一个.

这种设置允许客户端扩展比其他系统高得多的同时传输的数量,并且仍然保持良好的性能."常规"API 会浪费太多时间扫描所有的套接字列表.

## 挑一

有许多基于事件的系统可以从中选择,而 LBURCL 完全不可知.libevent、libev 是 libuv 三个流行的解决方案,但是您也可以直接访问操作系统的本机解决方案,如 epoll、kqueue、/dev/poll、pollset、Event Completion 或 I/O Completion Ports.

## 许多简单的把手

就像普通的多接口一样,你可以为多个句柄添加简单的句柄.`curl_multi_add_handle()`. 一个简单的句柄为每个转移您要执行.

在传输过程中,您可以在任何时候添加它们,也可以在任何时候使用类似的方法移除容易的句柄.`curl_multi_remove_handle`打电话.通常,只在传输完成后删除句柄.

## 多套接字回调

如上所述,这种基于事件的机制依赖于应用程序知道 libcurl 使用哪些套接字以及 libcurl 在这些套接字上等待什么:如果它等待套接字变成可读、可写或两者兼而有之!

它还需要告诉 libcurl 超时时间已经过期,因为它可以控制 libcurl 本身无法执行的所有操作.因此,libcurl 也必须告诉应用程序一个更新的超时值.

### 套接字回调

LICORL 通知应用程序套接字活动等待调用回调[cURL 功能](https://curl.haxx.se/libcurl/c/CURLMOPT_SOCKETFUNCTION.html). 您的应用程序需要实现这样的功能:

```
int socket_callback(CURL *easy,      /* easy handle */
                    curl_socket_t s, /* socket */
                    int what,        /* what to wait for */
                    void *userp,     /* private callback pointer */
                    void *socketp)   /* private socket pointer */
{
   /* told about the socket 's' */
}

/* set the callback in the multi handle */
curl_multi_setopt(multi_handle, CURLMOPT_SOCKETFUNCTION, socket_callback);
```

使用此,libcurl 将设置和移除应用程序应该监视的套接字.您的应用程序告诉基础的基于事件的系统等待套接字.如果有多个套接字要等待,那么这个回调将被多次调用,并且当状态改变时,它将再次调用,也许您应该从等待可写套接字切换到等待它变得可读.

当应用程序在 libcurl 的代表上监视的一个套接字根据请求注册它变得可读或可写时,通过调用`curl_multi_socket_action()`并在受影响的套接字中传递相关联的位掩码,指定已注册的套接字活动:

```
int running_handles;
ret = curl_multi_socket_action(multi_handle,
                               sockfd, /* the socket with activity */
                               ev_bitmask, /* the specific activity */
                               &running_handles);
```

### 定时器回调

应用程序处于控制状态,将等待套接字活动.但即使没有套接字的活动,也有一些事情需要做.超时事件、调用进度回调、重试开始或传输失败花费了太长时间等等.要使其工作,应用程序还必须确保处理 libcurl 设置的单次超时.

libcurl 用 TimeReLead 回调设置超时[cURL 时间函数](https://curl.haxx.se/libcurl/c/CURLMOPT_TIMERFUNCTION.html):

```
int timer_callback(multi_handle,   /* multi handle */
                   timeout_ms,     /* milliseconds to wait */
                   userp)          /* private callback pointer */
{
  /* the new time-out value to wait for is in 'timeout_ms' */
}

/* set the callback in the multi handle */
curl_multi_setopt(multi_handle, CURLMOPT_TIMERFUNCTION, timer_callback);
```

对于整个多句柄,无论添加了多少单独的简单句柄或正在进行中的传输,应用程序都只能处理一个超时.定时器回调将以当前最接近的时间更新.如果由于套接字活动,libcurl 在超时过期时间之前被调用,那么它很有可能在过期之前再次更新超时值.

当你选择的事件系统最终告诉你计时器已经过期时,你需要告诉它:

```
curl_multi_socket_action(multi, CURL_SOCKET_TIMEOUT, 0, &running);
```

在许多情况下,这将使 libcurl 再次调用 TimeReCalCube 并为下一个期满期设置一个新的超时.

### 如何开始一切

当您向多句柄添加一个或多个简单句柄并在多句柄中设置套接字和定时器回调时,就可以开始传输了.

首先,您告诉 libcurl 它超时(因为所有简单的句柄都以一个非常非常非常短的超时开始),这将使 libcurl 调用回调来设置设置,然后您可以让事件系统驱动:

```
/* all easy handles and callbacks are setup */

curl_multi_socket_action(multi, CURL_SOCKET_TIMEOUT, 0, &running);

/* now the callbacks should have been called and we have sockets to wait for
   and possibly a timeout, too. Make the event system do its magic */

event_base_dispatch(event_base); /* libevent2 has this API */

/* at this point we have exited the event loop */
```

### 什么时候完成?

返回的"RunnIn 句柄"计数器`curl_multi_socket_action`持有未完成的当前传输数量.当这个数字达到零时,我们知道没有转移.

每次"运行句柄"计数器改变时,`curl_multi_info_read()`将返回关于完成的特定传输的信息.
