
# 曲奇饼

默认情况下,通过设计,libcurl使传输尽可能基本,并且需要启用功能.一个这样的特性是HTTP cookies,更常见的是简单而简单的"cookies".

Cookie是由服务器发送的名称/值对(使用`Set-Cookie:`header)存储在客户端中,然后应该在匹配主机和路径要求的请求中再次发送回来,这些请求与cookie从服务器传出时与cookie一起指定的路径要求相匹配(使用`Cookie:`标题).在当今的现代网络中,网站有时会使用大量的cookies.

## 饼干引擎

当为特定的简单句柄启用"cookie引擎"时,这意味着它将记录传入的cookie,将它们存储在与该简单句柄相关联的内存中的"cookie存储"中,并且如果发出匹配的HTTP请求,则随后发送正确的cookie返回.

切换Cookie引擎有两种方法:

### 启用读取cookie引擎

询问libcurl将cookie从给定文件名导入到轻松句柄中`CURLOPT_COOKIEFILE`选项:

```
curl_easy_setopt(easy, CURLOPT_COOKIEFILE, "cookies.txt");
```

一个常见的技巧是只指定一个不存在的文件名或普通的"",让它只用一个空cookie存储来激活cookie引擎.

可以多次设置此选项,然后读取每个给定文件.

### 启用Cookie引擎

请求接收的cookie与文件一起存储在文件中`CURLOPT_COOKIEJAR`选项:

```
curl_easy_setopt(easy, CURLOPT_COOKIEJAR, "cookies.txt");
```

当容易的手柄稍后关闭时`curl_easy_cleanup()`所有已知的Cookie将被写入给定的文件.文件格式是浏览器也曾经使用过的著名的"Netscape Cookie文件"格式.

## 设置自定义Cookie

在请求中不向cookie存储添加任何cookie,甚至不激活cookie引擎的情况下,传递一组特定cookie的更简单和更直接的方法是使用"CURLOPT_COOKIE:":

```
curl_easy_setopt(easy, CURLOPT_COOKIE, "name=daniel; present=yes;");
```

您在那里设置的字符串是在HTTP请求中发送的原始字符串,并且应该采用以下重复序列的格式`NAME=VALUE;`-包括分号分离器.

## 进出口

内存中的cookie存储库可以容纳许多cookie,libcurl为应用程序提供了非常强大的方法来使用它们.您可以设置新的cookie,可以替换现有的Cookie,并且可以提取现有的Cookie.

### 向cookie商店添加cookie

将cookie添加到cookie中,将新cookie添加到cookie中.`CURLOPT_COOKIELIST`用一个新的饼干.输入的格式是Cookie文件格式中的单行,或者格式化为`Set-Cookie:`响应头,但是我们推荐Cookie文件样式:

```
#define SEP  "\\t"  /* Tab separates the fields */

char *my_cookie =
  "example.com"    /* Hostname */
  SEP "FALSE"      /* Include subdomains */
  SEP "/"          /* Path */
  SEP "FALSE"      /* Secure */
  SEP "0"          /* Expiry in epoch time format. 0 == Session */
  SEP "foo"        /* Name */
  SEP "bar";       /* Value */

curl_easy_setopt(curl, CURLOPT_COOKIELIST, my_cookie);
```

如果给定的cookie与已经存在的cookie(具有相同的域和路径等)匹配,它将用新的内容覆盖旧的cookie.

### 从cookie商店获取所有cookies

有时,当您关闭句柄时写入cookie文件是不够的,然后应用程序可以选择从存储中提取所有当前已知的cookie,如下所示:

```
struct curl_slist *cookies
curl_easy_getinfo(easy, CURLINFO_COOKIELIST, &cookies);
```

这将返回指向链接的cookie列表的指针,并且每个cookie都被(再次)指定为cookie文件格式的单一行.该列表是为您分配的,所以不要忘记调用`curl_slist_free_all`当应用程序与信息一起完成时.

### cookie存储命令

如果设置和提取Cookie是不够的,还可以以更多的方式干扰Cookie存储:

用内存擦除内存中的所有内容:

```
curl_easy_setopt(curl, CURLOPT_COOKIELIST, "ALL");
```

从内存中删除所有会话cookie(没有到期日期的Cookie):

```
curl_easy_setopt(curl, CURLOPT_COOKIELIST, "SESS");
```

强制将所有cookie写入先前指定的文件名`CURLOPT_COOKIEJAR`:

```
curl_easy_setopt(curl, CURLOPT_COOKIELIST, "FLUSH");
```

强制从先前指定的文件名中重新加载Cookie.`CURLOPT_COOKIEFILE`:

```
curl_easy_setopt(curl, CURLOPT_COOKIELIST, "RELOAD");
```

## Cookie文件格式

Cookie文件格式是基于文本的,每行存储一个Cookie.开始的线条`#`被当作评论.

每个指定单个cookie的每行由七个与选项卡字符分隔的文本字段组成.

| 场   | 例子                 | 意义             |
| --- | ------------------ | -------------- |
| 零   | 范例网站               | 域名             |
| 一   | 错误的                | 包含布尔子域         |
| 二   | /福布尔/              | 路径             |
| 三   | 错误的                | 设置安全传输         |
| 四   | 十四亿六千二百二十九万九千二百一十七 | 自1月1日1970或0日到期 |
| 五   | 人                  | 饼干名称           |
| 六   | 丹尼尔                | 饼干的价值          |
