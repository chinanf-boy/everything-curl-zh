
## URL全球化

有时你想要得到一系列URL,它们大部分是相同的,只有一小部分在请求之间变化.也许它是一个数字范围,或者是一组名称.CURL提供了"球形",可以很容易地指定许多URL.

globbing为此使用保留的符号\[]和{},这些符号通常不能作为合法URL的一部分(除了数字IPv6地址,但是curl无论如何都能很好地处理它们).如果球体挡住了你的路,就禁用它.`-g, --globoff`.

虽然大多数卷积相关的功能在CURL中是由libcurl库提供的,但是URL GuangBin功能不是!

### 数值范围

你可以要求一个数字范围[N-M]语法,其中N是起始索引,它上升到并包括M.

```
curl -O http://example.com/[1-100].png
```

它甚至可以做零前缀的范围,比如数字一直是三位:

```
curl -O http://example.com/[001-100].png
```

或者你只需要偶数的图像,所以你也告诉卷轴一步计数器.这个示例范围从0增加到100,增量为2:

```
curl -O http://example.com/[0-100:2].png
```

### 字母范围

cURL也可以按字母顺序排列,就像一个站点有一个名为A到Z的部分:

```
curl -O http://example.com/section[a-z].html
```

### 一览表

有时,这些部分并不遵循这样的简单模式,然后您可以自己给出完整的列表,但是可以在花括号内而不是用于范围的括号内:

```
curl -O http://example.com/{one,two,three,alpha,beta}.html
```

### 组合

您可以在同一URL中使用多个GLUs,这也会使CURL在这些URL上迭代.为了下载本、爱丽丝和弗兰克的图像,在100x100和1000 x1000的分辨率中,命令行看起来像:

```
curl -O http://example.com/{Ben,Alice,Frank}-{100x100,1000x1000}.jpg
```

或者下载国际象棋棋盘的所有图像,由0到7的两个坐标索引:

```
curl -O http://example.com/chess-[0-7]x[0-7].jpg
```

当然,你可以混合范围和系列.为Web服务器和邮件服务器获取一个星期的日志值:

```
curl -O http://example.com/{web,mail}-log[0-6].txt
```

### 全球化的输出变量

在本章前面的所有全局例子中,我们选择了使用`-O / --remote-name`选项,这使得CURL使用已使用URL的文件名部分保存目标文件.

有时候,这还不够.您正在下载多个文件,可能希望将它们保存在不同的子目录中,或者以不同的方式创建保存的文件名.当然,CURL也有解决这些问题的方法:输出文件名变量.

在URL中使用的每个"GLUB"都有一个单独的变量.它们被称为"γ".[号码]'-表示单个字母'#',后面跟着glob编号,第一个glob从1开始,最后一个glob结束.

保存两个不同站点的主页:

```
curl http://{one,two}.example.com -o "file_#1.txt"
```

在子目录中保存具有两个GULL的命令行的输出;

```
curl http://{site,host}.host[1-5].example.com -o "subdir/#1_#2"
```
