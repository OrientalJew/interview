#### HttpClient与HttpUrlConnection

都支持Https协议，都是以流形式进行数据传输，还有ipv6和连接池等功能；

HttpClient拥有非常多的API，在不破坏它的兼容性的情况下，很难进行扩展，在5.0以上已经被废弃，6.0已经删除；

HttpUrlConnection：多用途，轻量级的Http客户端，适用于大多数的应用程序。API简单，容易使用和扩展，在4.4以上，底层采用OKHttp实现；

