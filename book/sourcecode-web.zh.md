
## 网站源代码

大多数curl网站也可以在公共git存储库中使用,尽管它与源代码存储库分开,因为对于相同的人来说,它通常不感兴趣,而且我们可以维护具有不同的推送权限的人员列表等等.

网站Git存储库在GITHUB上可以在这个URL上使用:[HTTPS://GITHUBCOM/CURL/CURL WWW](https://github.com/curl/curl-www)你可以克隆一个这样的网页代码:

```
git clone https://github.com/curl/curl-www.git
```

### 构建网络

该网站是一个老定制的设置,主要从一组源文件构建静态HTML文件.源文件用基本上是一个增强的C预处理器进行预处理.[fcpp](https://daniel.haxx.se/projects/fcpp/)以及一组Perl脚本.MAN页面被转换为HTML[罗菲特](https://daniel.haxx.se/projects/roffit/). 确保FCPP、Perl、RoFIT、Bug和CURL都在您的$PATH中.

一旦您首次克隆了Git存储库,调用`sh
bootstrap.sh`一次获取一个Syrink和一些初始本地文件设置,然后可以通过调用本地构建网站.`make`在源根树中.

请注意,这并不能使您成为一个完整的网站镜像,因为有些脚本和文件只在真实的网站上可用,但是应该能够让您在本地查看大多数HTML页面.
