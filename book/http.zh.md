
# 如何用cURL进行HTTP

在所有用户调查中,以及在curl的整个生命周期中,HTTP一直是curl支持的最重要和最常用的协议.本章将介绍如何进行有效的HTTP传输和一般的cURL处理.

这对于HTTPS来说,基本上是相同的工作方式,因为它们实际上是同一回事,因为HTTPS是具有额外安全性TLS层的HTTP.具体见[HTTPS](#https)下面部分.

## HTTP方法

在每一个HTTP请求中,都有一个方法.有时称为动词.最常用的是GET、POST、HEAD和PUT.

通常,您并不需要在命令行中指定方法, 取而代之的是由使用确切方法，来决定您选的选项. GET是默认值, 使用`-d`或`-F`使它成为一个POST,`-I`生成一个HEAD和`-T`发送一个PUT.

更多关于[修改HTTP请求](http-requests.zh.md)部分.

## 脚本类浏览器任务

TBD
