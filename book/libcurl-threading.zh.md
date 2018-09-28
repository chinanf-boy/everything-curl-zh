
## LabCURL多线程

libcurl是线程安全的,但没有内部线程同步.您可能必须提供您自己的锁定或更改选项,以正确使用libcurl螺纹.确切地说,需要什么取决于如何构建libcurl.请参阅[螺纹安全性](https://curl.haxx.se/libcurl/c/threadsafe.html)网页,包含最新信息.
