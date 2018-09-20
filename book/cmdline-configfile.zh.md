
## 配置文件

您可以轻松地结束使用大量命令行选项的curl命令行,这使它们很难使用.有时要输入的命令行的长度甚至命中命令行系统允许的最大长度.微软Windows命令提示符是一个具有相当小的最大行长度的示例.

为了帮助这些情况,CURL提供了一个我们称为"配置文件"的特性.它基本上允许您在文本文件中写入命令行选项,然后告诉curl除了命令行之外还要从该文件读取选项.

您告诉CURL用-K/-CONFIG选项从特定的文件读取更多的命令行选项,如下所示:

```
curl -K cmdline.txt http://example.com
```

……而且在`cmdline.txt`文件(当然,可以使用任何文件名,请您)每行输入每个命令行:

```
# this is a comment, we ask to follow redirects
--location
# ask to do a HEAD request
--head
```

配置文件接受短选项和长选项,就像在命令行上写入它们一样.作为一个特殊的额外特性,它还允许您编写选项的长格式,而不用前两个破折号,以便于阅读.使用该样式,上面所示的配置文件可以另选为:

```
# this is a comment, we ask to follow redirects
location
# ask to do a HEAD request
head
```

带参数的命令行选项必须在与选项相同的行上提供其参数.例如,可以更改用户代理HTTP报头.

```
user-agent "Everything-is-an-agent"
```

为了让配置文件看起来更像真正的配置文件,它还允许在选项及其参数之间使用"="或":".正如你所看到的,这是不必要的,但有些像它提供的清晰度.再次设置用户代理选项:

```
user-agent = "Everything-is-an-agent"
```

可以不带双引号指定选项的参数,然后curl将把下一个空格或换行作为参数的结尾.因此,如果要提供嵌入空间的参数,则必须使用双引号.

上面我们使用的用户代理字符串示例没有空格,因此也可以在没有引号的情况下提供,如:

```
user-agent = Everything-is-an-agent
```

最后,如果要在配置文件中提供URL,则必须这样做.`--url`方式,或者只是`url`而不是在命令行上,基本上所有不是选项的东西都被假定为URL.所以你提供了一个cURL的URL,像这样:

```
url = "http://example.com"
```

### 默认配置文件

当调用cURL时,它总是(除非)`-q`用于检查默认配置文件并在找到时使用它.它检查的文件名是`.curlrc`关于类UNIX系统`_curlrc`在Windows上.

默认配置文件按以下顺序在以下位置检查:

1.  CURL试图找到"home目录":它首先检查CurLyHome,然后检查家庭环境变量.失败了,它使用`getpwuid()`在UNIX类系统上(返回系统中当前用户的主目录).在Windows上,它检查AppDATA变量,或者作为"%USER配置文件%\\Apple Debug"的最后手段.

2.  在Windows上,如果主目录中没有\_curlrc文件,则检查是否放置了curl可执行文件.在UNIX类系统中,它将简单地尝试从确定的主目录加载.CURLRC.
