
# 构建使用TLS库

为了让curl支持基于TLS的协议,比如HTTPS、FTPS、SMTPS、POP3S、IMAPS等等,您需要使用第三方TLS库来构建,因为curl并不实现TLS协议本身.

cURL是用大量的TLS库编写的:

-   博林SSL
-   GSKit(OS/400专用)
-   侏儒
-   NSS
-   OpenSSL
-   安全传输(本地MACOS)
-   沃尔夫斯尔
-   斧头
-   利布雷斯勒
-   麦德尔斯
-   施奈尔(原生视窗)

在构建curl和libcurl以使用这些库之一时,在构建机器上安装库及其包含头非常重要.

## 配置

下面,您将学习如何告诉配置使用不同的库.请注意,除了OpenSSL及其兄弟姐妹以外的所有图书馆,您必须*使残废*使用OpenSSL的检查`--without-ssl`.

### OpenSSL,BoringSSL,利布雷斯尔

```
./configure
```

默认情况下,配置将在其默认路径中检测OpenSSL.您可以选择将点配置到自定义安装路径前缀,在那里可以找到OpenSSL:

```
./configure --with-ssl=/home/user/installed/openssl
```

替代方案[博林SSL](building-boringssl.md)libressl看起来非常相似,因此configure将以与OpenSSL相同的方式检测它们,但是它将使用一些额外的措施来找出它正在使用的特定口味.

### 侏儒

```
./configure --with-gnutls --without-ssl
```

默认情况下,配置将检测其默认路径中的GNUTL.您可以选择将点配置到自定义安装路径前缀,在那里可以找到GNUTLS:

```
./configure --with-gnutls=/home/user/installed/gnutls --without-ssl
```

### NSS

```
./configure --with-nss --without-ssl
```

默认情况下,配置将检测其默认路径中的NSS.您可以选择将点配置到自定义安装路径前缀,在那里可以找到NSS:

```
./configure --with-nss=/home/user/installed/nss --without-ssl
```

### 沃尔夫斯尔

```
./configure --with-cyassl --without-ssl
```

(Cysl是库的前一个名称)配置将默认地检测其默认路径中的WOLFSSL.您可以选择将点配置到自定义安装路径前缀,在那里可以找到WolfSSL:

```
./configure --with-cyassl=/home/user/installed/nss --without-ssl
```

### 斧头

```
./configure --with-axtls --without-ssl
```

默认情况下,配置将在其默认路径中检测AXTLS.您可以选择将点配置到自定义安装路径前缀,在那里可以找到AXTLS:

```
./configure --with-axtls=/home/user/installed/axtls --without-ssl
```

### 麦德尔斯

```
./configure --with-mbedtls --without-ssl
```

默认情况下,配置将检测其默认路径中的MbEdTLS.您可以选择将点配置到自定义安装路径前缀,在那里可以找到MbEdTLS:

```
./configure --with-mbedtls=/home/user/installed/mbedtls --without-ssl
```

### 安全传输

```
./configure --with-darwinssl --without-ssl
```

(DarwinSSL是安全传输的替代名称)配置将默认情况下在其默认路径中检测安全传输.您可以选择将点配置到自定义安装路径前缀,在那里可以找到DarwinSSL:

```
./configure --with-darwinssl=/home/user/installed/darwinssl --without-ssl
```

### 安全通道

```
./configure --with-winssl --without-ssl
```

(WinSSL是SnNeNE的替代名称)配置将默认情况下检测ShannEL默认路径.您可以选择将点配置到自定义安装路径前缀,在那里可以找到WinSSL:

```
./configure --with-winssl=/home/user/installed/winssl --without-ssl
```
