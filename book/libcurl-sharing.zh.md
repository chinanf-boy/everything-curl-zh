
# 在手柄之间共享数据

有时应用程序需要在传输之间共享数据.所有添加到同一个多句柄的简单句柄都会自动在同一个多句柄中的句柄之间进行大量共享,但有时这并不是您想要的.

## 多手柄

所有轻松句柄添加到同一个多句柄自动共享[曲奇饼](libcurl-http-cookies.md),[连接缓存](libcurl-connectionreuse),[DNS缓存](libcurl-names.md)和SSL会话ID缓存.

## 易于处理之间的共享

libcurl具有一个通用的"共享接口",其中应用程序创建一个"共享对象",该对象保存可以被任意数量的简单句柄共享的数据.然后将数据从共享对象中存储和读取,而不是保存在共享它的句柄中.

```
CURLSH *share = curl_share_init();
```

共享对象可以被设置为共享cookie、连接缓存、DNS缓存和SSL会话ID缓存的全部或任何一个.

例如,设置共享以保存Cookie和DNS缓存:

```
curl_share_setopt(share, CURLSHOPT_SHARE, CURL_LOCK_DATA_COOKIE);
curl_share_setopt(share, CURLSHOPT_SHARE, CURL_LOCK_DATA_DNS);
```

…然后设置相应的传输来使用这个共享对象:

```
curl_easy_setopt(curl, CURLOPT_SHARE, share);
```

用此完成的传输`curl`因此,句柄将使用并存储其cookie和DNS信息.`share`把手.可以设置几个简单的句柄来共享同一个共享对象.

## 分享什么

`CURL_LOCK_DATA_COOKIE`-设置此位以共享Cookie jar.请注意,每个简单的句柄仍然需要得到它的cookie"引擎"正确启动开始使用cookies.

`CURL_LOCK_DATA_DNS`DNS缓存是libcurl存储解析主机名称的地址一段时间以使后续查找更快的地方.

`CURL_LOCK_DATA_SSL_SESSION`-SSL会话ID缓存是libcurl存储用于SSL连接的恢复信息的地方,以便能够更快地恢复先前的连接.

`CURL_LOCK_DATA_CONNECT`-当设置时,该句柄将使用共享连接缓存,因此可能更可能找到要重用的现有连接等,这在以串行方式向同一主机进行多次传输时可能导致更快的性能.

## 锁定

如果希望在多线程环境中共享传输的共享对象.也许您有一个具有许多内核的CPU,并且希望每个内核运行自己的线程并传输数据,但是您仍然希望不同的传输共享数据.然后需要设置互斥回调.

如果你不使用线程和你*知道*您可以以串行方式一次性访问共享对象,而不需要设置任何锁.但是如果一次有多个传输访问共享对象,则需要设置互斥回调以防止数据破坏甚至崩溃.

由于libcurl本身不知道如何锁定东西,甚至不知道您正在使用什么线程模型,所以必须确保执行一次只允许一个访问的互斥锁.使用应用程序的pTrk的锁定回调可以类似于:

```
static void lock_cb(CURL *handle, curl_lock_data data,
                    curl_lock_access access, void *userptr)
{
  pthread_mutex_lock(&lock[data]); /* uses a global lock array */
}
curl_share_setopt(share, CURLSHOPT_LOCKFUNC, lock_cb);
```

与相应的解锁回调可以类似:

```
static void unlock_cb(CURL *handle, curl_lock_data data,
                      void *userptr)
{
  pthread_mutex_unlock(&lock[data]); /* uses a global lock array */
}
curl_share_setopt(share, CURLSHOPT_UNLOCKFUNC, unlock_cb);
```

## 不分享

TBD
