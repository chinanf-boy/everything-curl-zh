
## 放

介词和词尾之间的差别是微妙的.它们实际上是相同的传输,除了不同的方法字符串.当POST意欲将数据传递到远程资源时,PUT应该是该资源的新版本.

在这方面,PUT类似于其他协议中发现的旧标准文件上传.您可以用PoT上传资源的新版本.URL标识资源,并指出要放置的本地文件:

```
curl -T localfile http://example.com/new/resource/file
```

…--t将暗示一个放和告诉CURL哪个文件要发送.但是POST和PUT之间的相似性还允许您使用常规cURLPOST机制,使用`-d`但要求它使用一个代替:

```
curl -d "data to PUT" -X PUT http://example.com/new/resource/file
```
