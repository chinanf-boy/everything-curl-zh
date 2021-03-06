
## FTP使用两个连接

FTP使用两个TCP连接! 第一个连接是由客户端在连接到FTP服务器时建立的,称为*控制连接*. 作为初始连接,它处理身份验证并去到远程服务器上的正确目录等.当客户端准备传输文件时,建立第二个TCP连接,并通过该连接传输数据.

设置第二个连接会引起麻烦和头痛,原因有几个.

### 主动连接

客户端可以选择设置，当请求服务器时, 要求服务器要连接到它本身的位置, 也就是所谓的"主动"连接.这是用PORT 或 EPRT命令完成的. 允许远程主机连接到客户端打开的端口上, 这要求, 在这它倆之间没有禁止通过的防火墙或其他网络设备,当然可能的情况远不止这些. 好了，您可以使用`curl -P [arg]`(长命令也称为`--ftp-port`)，要求一个`主动`传输使用的地址, 虽然该选项允许您精确地指定地址, 但是设置成与您的地址一样的地址几乎就是正确的选择, 像`-P -`这样的, 来要求一个文件:

```
curl -P - ftp://example.com/foobar.txt
```

还可以显式地告诉curl不使用EPRT(这是比端口稍有更新的命令)，通过`--no-eprt`命令行选项.

### 被动连接

Curl缺省设置为请求一个"被动"连接,这意味着它向服务器发送一个PASV或EPSV命令,然后服务器为第二个连接打开一个新端口,然后curl连接到该端口.对于终端用户和客户端来说, 对新端口的传出连接通常更容易,限制也更少, 但是它要求服务器端的网络允许.

默认情况下启用了被动连接,但如果您以前已打开了主动连接,则可以使用`--ftp-pasv`切换为被动连接.

您还可以明确地请求curl不使用EPSV(这是比PASV稍微稍微更新的命令). 通过`--no-epsv`命令行选项.

有时服务器正在运行一个奇怪的设置,当curl发出PASV命令时,服务器会用一个IP地址作为响应, 而该地址是错误的, 之后curl就无法设置数据连接.对于这个(希望是罕见的)情况,您可以请求curl忽略PASV响应中提到的IP地址(`--ftp-skip-pasv-ip`)。而将,把用在控制连接中的IP地址, 甚至可以用在第二连接.

### 防火墙问题

使用主动或被动传输,网络路径中的任何现有防火墙都必须有状态地检查FTP流量,以找出新端口以打开该端口,并接受该端口用于第二连接.
