
## LIPURL HTTP下载

get方法是在请求HTTP URL时不使用LbCURL的默认方法,并且不需要特定的其他方法.它向服务器请求特定的资源——标准的HTTP下载请求:

```
easy = curl_easy_init();
curl_easy_setopt(easy, CURLOPT_URL, "http://example.com/");
curl_easy_perform(easy);
```

由于在easy句柄中设置的选项很粘,并且一直保留到更改,所以有时您要求使用GET以外的其他请求方法,然后希望再次切换回GET以获得后续请求.为了这个目的,有`CURLOPT_HTTPGET`选项:

```
curl_easy_setopt(easy, CURLOPT_HTTPGET, 1L);
```

### 下载标题

HTTP传输还包括一组响应标头.响应头是与实际有效载荷相关联的元数据,称为响应体.所有的下载都会得到一组标题,但是当使用libcurl时,您可以选择是否要下载(查看).

你可以让libcurl把标题和规则体一样传递给同一个"流",通过使用`CURLOPT_HEADER`:

```
easy = curl_easy_init();
curl_easy_setopt(easy, CURLOPT_HEADER, 1L);
curl_easy_setopt(easy, CURLOPT_URL, "http://example.com/");
curl_easy_perform(easy);
```

或者,您可以选择依赖于默认行为来将标题存储在单独的下载文件中.[写](callback-write.md)和[报头回调](callback-header.md):

```
easy = curl_easy_init();
FILE *file = fopen("headers", "wb");
curl_easy_setopt(easy, CURLOPT_HEADERDATA, file);
curl_easy_setopt(easy, CURLOPT_URL, "http://example.com/");
curl_easy_perform(easy);
fclose(file);
```

如果您只想随意浏览标题,那么在开发时只要设置详细模式就足够了,因为这将显示发送到stderr的传出和传入标题:

```
curl_easy_setopt(easy, CURLOPT_VERBOSE, 1L);
```
