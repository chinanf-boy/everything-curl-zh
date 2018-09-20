
# 用BurnSSL构建cURL

## 建造博林SSL

$home /Src是我在这个例子中放入代码的地方.你可以随心所欲地挑选.

```
$ cd $HOME/src
$ git clone https://boringssl.googlesource.com/boringssl
$ cd boringssl
$ mkdir build
$ cd build
$ cmake ..
$ make
```

## 设置生成树以通过CURL的配置来检测

在BurnStl源树根中,确保有一个`lib`和一个`include`迪尔这个`lib`DIR应该包含两个LIBs(我让它们成为构建DIR的链接).这个`include`默认情况下,DIR已经存在.制作填充`lib`像这样(源树根中发出的命令,而不是在`build/`子目录).

```
$ mkdir lib
$ cd lib
$ ln -s ../build/ssl/libssl.a
$ ln -s ../build/crypoto/libcrypto.a
```

## 配置cURL

`LIBS=-lpthread ./configure --with-ssl=$HOME/src/boringssl`(在这里我指出了BurnStl树的根)

验证在配置结束时,它表示检测到使用的BurnStl.

## cURL

跑`make`在cURL源树中.

现在你可以正常安装cURL.`make install`等.
