
# 邮政转帐信息

记住LyCURL传输如何与"简单句柄"关联!每个传输都具有这样的句柄,并且当传输完成时,在句柄被清理或重用于另一个传输之前,可以使用它从先前的操作中提取信息.

你这样做的朋友叫`curl_easy_getinfo()`你告诉它你感兴趣的具体信息,如果可以的话,它会还给你.

当您使用这个函数时,您将传递简单的句柄,您想要哪些信息,以及一个指向变量的指针来保存答案.你必须传递一个指向正确类型变量的指针,否则你会冒着事情发生的风险.这些信息值被设计为提供.*之后*转让完成.

你收到的数据可以是一个长的"字符".*一个"结构"列表*一个双人或一个插座.

这就是你如何提取`Content-Type:`来自先前HTTP传输的值:

```
CURLcode res;
char *content_type;
res = curl_easy_getinfo(curl, CURLINFO_CONTENT_TYPE, &content_type);
```

但是如果要提取在该连接中使用的本地端口号:

```
CURLcode res;
long port_number;
res = curl_easy_getinfo(curl, CURLINFO_LOCAL_PORT, &port_number);
```

## 可用信息

| GETFILE选项                      | 类型   | 描述                                                  |
| ------------------------------ | ---- | --------------------------------------------------- |
| CurnFiFi激活盒                    | 姜黄   | 会话的活动套接字                                            |
| CurnFixAppTeleNoTimeTimeTimes  | 双重的  | 从开始到SSL/SSH握手完成的时间.                                 |
| CurnFier-CurtFielf             | 结构列表 | 证书链                                                 |
| 科林福德条件                         | 长的   | 是否满足时间条件                                            |
| CurnIfFixCelpTimeTimeTimes     | 双重的  | 从开始到远程主机或代理完成的时间                                    |
| CurnFiFi内容                     | 双重的  | 内容长度标题的长度                                           |
| CurnFiFi内容                     | 双重的  | 上传大小                                                |
| Curnnfl Con Type型              | 烧焦\* | 内容类型头的内容类型                                          |
| 库林福克厨师                         | 结构列表 | 所有已知曲奇列表                                            |
| CurrnFixEffelyURL              | 烧焦\* | 最后使用的URL                                            |
| CurnFiFi文件时间                   | 长的   | 检索文档的远程时间                                           |
| CurnFifftpNexyYyType           | 烧焦\* | 登录到FTP服务器后的入口路径                                     |
| 卷叶蛾大小                          | 长的   | 接收到的所有报头的字节数                                        |
| CurnFifHutpAuthoAvess          | 长的   | 可用的HTTP身份验证方法(位掩码)                                  |
| CurnFixHtp1连接码                 | 长的   | 最后代理连接响应代码                                          |
| CurnFiffHtppI版本                | 长的   | 连接中使用的HTTP版本                                        |
| 柯林福斯插座                         | 长的   | 最后一个套接字                                             |
| 黄连                             | 烧焦\* | 最后连接的本地端IP地址                                        |
| CurnFiff-LoalalPoT端口           | 长的   | 最后连接的本地端端口                                          |
| CurrnFoNayeloOkuPuthiTimeTimes | 双重的  | 从开始到完成解析的时间                                         |
| CurnFiNoNuxLink                | 长的   | 用于以前传输的新成功连接的数目                                     |
| 科林弗罗塞尔诺                        | 长的   | 从上次连接失败的错误                                          |
| CurnFe前转移时间                    | 双重的  | 从开始到转移之前的时间                                         |
| 黄连                             | 烧焦\* | 最后连接的IP地址                                           |
| CurrnFib原端口                    | 长的   | 最后连接端口                                              |
| 科林福克私人                         | 烧焦\* | 用户专用数据指针                                            |
| 科林福协议                          | 长的   | 用于连接的协议                                             |
| 黄连                             | 长的   | 可用的HTTP代理认证方法                                       |
| CurrnFoPro                     | 长的   | 代理证书验证结果                                            |
| CurnFib重定向计数                   | 长的   | 遵循的重定向的总数                                           |
| CurnLoFi重定向时间                  | 双重的  | 最后转移前所有重定向步骤所花费的时间                                  |
| CurrIfFueRealdTr.URL           | 烧焦\* | 如果启用重定向,URL A重定向将带您到达                               |
| CurnLoFi请求尺寸                   | 长的   | 在发出的HTTP请求中发送的字节数                                   |
| CurnFIO响应码                     | 长的   | 最后接收响应代码                                            |
| CurnFixRTSPEclipse CSEQ        | 长的   | 下一个将使用的RTSP CSEQ                                    |
| 黄连                             | 长的   | 上次接收到的RTSP                                          |
| CurnFixTrpServer               | 长的   | RTSP CSEQ,这将是下一个预期                                  |
| CurnFuffTrasScript             | 烧焦\* | 会话标识                                                |
| 柯林福方案                          | 烧焦\* | 用于连接的方案                                             |
| CurnFixsiz下载                   | 双重的  | 下载的字节数                                              |
| CurnFixsiz上传                   | 双重的  | 上载字节数                                               |
| CulnFixSuffySype下载             | 双重的  | 平均下载速度                                              |
| CulnFixSuffely上传               | 双重的  | 平均上传速度                                              |
| 科林福斯发动机                        | 结构列表 | OpenSSL加密引擎列表                                       |
| CurnFiff-SLSVIEVIVE结果          | 长的   | 证书验证结果                                              |
| CurnFixSistTrimeTimeTimeTimes  | 双重的  | 从开始到接收第一个字节的时间                                      |
| CurnFulfTLS-SI会话               | 结构列表 | 可用于进一步处理的TLS会话信息.(**弃权选项,使用CulnFixTLSsSLSYPTR代替!**) |
| Curnnfl TLS-SLSYPTR            | 结构列表 | 可用于进一步处理的TLS会话信息                                    |
| CurrnFo.TooTimeTimeTimes       | 双重的  | 以前转移的总时间                                            |
