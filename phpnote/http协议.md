##http协议
HTTP协议的主要特点：

- 支持客户端/服务器模式，BS架构，计算量大多在服务器端，客户端主要负责页面渲染。
- 简单快速，客户端向服务器端请求服务时，只需要传送请求方法和路劲。请求方法常有GET/HEAD/POST。
- 灵活。HTTP允许传输任意类型的数据对象，通过Content-Type标识传输类型。
- 无连接。每次连接只处理一个请求，服务器处理完客户端请求并受到客户端的应答后，就断开连接。
- 无状态。无状态是指协议对于对事务处理没有记忆能力。缺少状态意味着如果后续助理需要前面的信息，则它必须重传。

HTTP请求由三部分组成，请求行、消息包头、请求正文。

请求行：以一个方法符号开头，以空格分开，后面跟着请求的URI和协议的版本。格式如下：Method Request-URI HTTP-Verison CRLF。  
其中Method表示请求方法。  Request-URI是一个统一资源标识符。HTTP-Version表示请求的HTTP协议版本。CRLF表示回车和换行。

请求方法：  
GET 请求获取Request-URI所标识的资源。  
POST 在Request-URI所表示的资源后，附加新的数据。
HEAD 请求获取有Request-URI所标识的资源的响应消息报头。

状态代码：  
1xx：指示信息--标识请求已接受，继续处理。  
2xx：成功--表示请求已被成功接受、理解、接受。  
3xx：重定向--要完成请求必须进行更进一步的操作。  
4xx：客户端错误--请求有语法错误或请求无法实现。
5xx：服务器端错误--服务器未能实现合法请求。

100 => "HTTP/1.1 100 Continue",   
101 => "HTTP/1.1 101 Switching Protocols",  
200 => "HTTP/1.1 200 OK",  
201 => "HTTP/1.1 201 Created",   
202 => "HTTP/1.1 202 Accepted",   
203 => "HTTP/1.1 203 Non-Authoritative Information",   
204 => "HTTP/1.1 204 No Content",   
205 => "HTTP/1.1 205 Reset Content",   
206 => "HTTP/1.1 206 Partial Content",   
300 => "HTTP/1.1 300 Multiple Choices", 
301 => "HTTP/1.1 301 Moved Permanently",   
302 => "HTTP/1.1 302 Found",   
303 => "HTTP/1.1 303 See Other",   
304 => "HTTP/1.1 304 Not Modified",   
305 => "HTTP/1.1 305 Use Proxy",   
307 => "HTTP/1.1 307 Temporary Redirect",   
400 => "HTTP/1.1 400 Bad Request",   
401 => "HTTP/1.1 401 Unauthorized",   
402 => "HTTP/1.1 402 Payment Required",   
403 => "HTTP/1.1 403 Forbidden",   
404 => "HTTP/1.1 404 Not Found",   
405 => "HTTP/1.1 405 Method Not Allowed",   
406 => "HTTP/1.1 406 Not Acceptable",   
407 => "HTTP/1.1 407 Proxy Authentication Required",   
408 => "HTTP/1.1 408 Request Time-out",   
409 => "HTTP/1.1 409 Conflict",   
410 => "HTTP/1.1 410 Gone",   
411 => "HTTP/1.1 411 Length Required",   
412 => "HTTP/1.1 412 Precondition Failed",   
413 => "HTTP/1.1 413 Request Entity Too Large",   
414 => "HTTP/1.1 414 Request-URI Too Large",   
415 => "HTTP/1.1 415 Unsupported Media Type",   
416 => "HTTP/1.1 416 Requested range not satisfiable",   
417 => "HTTP/1.1 417 Expectation Failed",   
500 => "HTTP/1.1 500 Internal Server Error",   
501 => "HTTP/1.1 501 Not Implemented",   
502 => "HTTP/1.1 502 Bad Gateway",   
503 => "HTTP/1.1 503 Service Unavailable",   
504 => "HTTP/1.1 504 Gateway Time-out"    