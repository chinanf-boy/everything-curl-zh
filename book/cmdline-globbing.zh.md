
## URL-glob

有时你想要获取一系列的URL,它们大部分是相同的,只有在请求的一小部分变化.也许它是一个数字范围,或者是一组名称.curl提供了"glob/globbing",可以很容易地指定许多URL.

`globbing`通过使用`[]`和`{}`做到这点,这些符号通常不能作为合法URL的一部分(除了数字IPv6地址,但是curl无论如何都能很好地处理它们).如果 glob 挡住了你的路,就禁用它`-g, --globoff`.

虽然大多数在curl中的相关功能传输是由libcurl库提供的,但是URL  glob功能则不是!

### 数值范围

你可以要求一个数字范围[N-M]语法,其中 **N** 是起始索引,它上升到,并包括 **M**.

```
curl -O http://example.com/[1-100].png
```

它甚至可以做 0前缀 的范围,比如数字一直是三位:

```
curl -O http://example.com/[001-100].png
```

或者你只需要偶数的图像,所以你也告诉curl,跳几步的增量.这个示例范围从 0增加到100,增量为2:

```
curl -O http://example.com/[0-100:2].png
```

### 字母范围

cURL也可以按字母顺序排列,就像一个站点目录,有a到z的部分:

```
curl -O http://example.com/section[a-z].html
```

### 一个列表

有时,这些部分并不遵循这样的简单模式,然后您可以自己给出完整的列表,但要在花括号`{}`内,而不是用于范围的`[]`内:

```
curl -O http://example.com/{one,two,three,alpha,beta}.html
```

### 组合

您可以在同一URL中使用多个glob,这也会使curl逐个获取这些URL.为了下载 本、爱丽丝和弗兰克的图像,范围在 100x100 和 1000x1000 的分辨率,命令行看起来像:

```
curl -O http://example.com/{Ben,Alice,Frank}-{100x100,1000x1000}.jpg
```

或者下载国际象棋棋盘的所有图像,由0到7的两个坐标索引:

```
curl -O http://example.com/chess-[0-7]x[0-7].jpg
```

当然,你可以混合范围和列表.为Web服务器和邮件服务器获取一个星期的日志值:

```
curl -O http://example.com/{web,mail}-log[0-6].txt
```

### glob的输出变量

在本章前面的所有全局例子中,我们选择了使用`-O / --remote-name`选项,这使curlL保存了已使用URL的文件名部分,作为目标文件.

有时候,这还不够.您正在下载多个文件,可能希望将它们保存在不同的子目录中,或者以不同的方式创建保存的文件名.不急,curl当然有解决这些问题的方法: 输出文件名变量.

在URL中使用的每个"GLUB"都有一个单独的变量.它们被称为'#[num]' - 表示为单个字母'#',后面跟着glob的编号,首个glob从 1 开始,以最后一个glob结束.

保存两个不同站点的主页:

```
curl http://{one,two}.example.com -o "file_#1.txt"
```

在子目录中保存具有两个glob的命令行的输出;

```
curl http://{site,host}.host[1-5].example.com -o "subdir/#1_#2"
```
