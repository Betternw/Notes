1. HTTP与HTTPS有什么区别？
* HTTPS经由HTTP进行通信，但利用SSL/TLS来加密数据包
* HTTPS即HTTP下加入SSL层，超文本传输协议 (HTTP-Hypertext transfer protocol) 是一种详细规定了浏览器和万维网服务器之间互相通信的规则，通过因特网传送万维网文档的数据传送协议
* 如果URL的协议是HTTP，则客户端会打开一条到服务端端口80（默认）的连接，并向其发送老的HTTP请求。 如果URL的协议是HTTPS，则客户端会打开一条到服务端端口443（默认）的连接，然后与服务器握手，以二进制格式与服务器交换一些SSL的安全参数，附上加密的 HTTP请求。
* 在写法上的区别也是前缀的不同
2. Https 请求慢的解决办法
* 不通过DNS解析，直接访问IP
* 解决head of line blocking——它的原因是队列的第一个数据包（队头）受阻而导致整列数据包受阻，使用http pipelining，确保几乎在同一时间把request发向了服务器
3. Http的request和response的协议组成
* Request组成 请求行（request line）、请求头部（header）、空行和请求数据四个部分组成。


