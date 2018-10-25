
# HTTP版本

和其他任何互联网协议一样,HTTP协议多年来一直在发展,现在有分布在世界各地的客户端和服务器,随着时间的推移,这些客户端和服务器使用不同版本并获得不同程度的成功.因此,为了让libcurl与传递的URL一起工作,libcurl为您提供了指定请求和传输应该使用哪个HTTP版本的方法.libcurl的设计方式是,如果需要,它尝试使用最常见、最合理的默认值,但有时这还不够,然后您可能需要指示libcurl做什么.

自2016年年中以来,libcurl默认使用HTTP/HTTPS服务器2,如果您有内置HTTP/2能力的libcurl,则libcurl将尝试使用HTTP/2,或者在协商失败的情况下,将其降至1.1.默认情况下,非HTTP/2可配置的libcurlL通过HTTPS获得1.1.普通HTTP请求仍然默认为HTTP/1.1.

如果默认行为对于传输来说不够好,则`CURLOPT_HTTP_VERSION`选择权在你手中.

| 选择权                      | 描述                                    |
| ------------------------ | ------------------------------------- |
| Curl HyppVeluxon         | 重置为默认行为                               |
| CURLH-HTPIPVISORNO1L0    | 强制使用传统HTTP/1协议版本                      |
| Curl HyppVeluxOn1L1      | 使用HTTP/1.1协议版本进行请求                    |
| CURLH-HTPIPVISORN 2Y0    | 尝试使用HTTP/2                            |
| Curl HyppVeluon 2TLS     | 尝试仅在HTTPS连接上使用HTTP/2,否则执行HTTP / 1.1   |
| CURLIHHTPIPVRONSON2L先验知识 | 直接使用HTTP/2,不需要从1.1升级.它要求你知道这个服务器是可以的. |
