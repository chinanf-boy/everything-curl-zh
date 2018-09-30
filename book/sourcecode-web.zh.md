
## 网站源代码

虽然网站与curl源代码存储库分开,但大多数curl网站也可以在公共git存储库中找到. 而对于相同的人来说,感兴趣的部分可能有所不同,这样我们也可以维护具有不同的推送权限的人员列表等等.

网站Git存储库在Github上. 可以在[https://github.com/curl/curl-www](https://github.com/curl/curl-www)这个URL上使用: 你可以克隆一个这样的网页代码:

```
git clone https://github.com/curl/curl-www.git
```

### 构建网络

该网站是一个老定制的设置,主要从一组源文件构建静态HTML文件.源文件用基本上是一个增强的C预处理器.[fcpp](https://daniel.haxx.se/projects/fcpp/)进行预处理以及一组Perl脚本.[roffit](https://daniel.haxx.se/projects/roffit/)将man页面转换为HTML. 确保fcpp, perl,
roffit, make 和 curl都在您的$PATH中.

一旦您首次克隆了Git存储库,调用一次`sh
bootstrap.sh`获取一个链接和一些初始本地文件设置,然后可以通过在源根树中的`make`调用,本地构建网站.

请注意,这并不能使您成为一个完整的网站镜像,因为有些脚本和文件只在真实的网站上可用,但是应该能够让您在本地查看大多数HTML页面.
