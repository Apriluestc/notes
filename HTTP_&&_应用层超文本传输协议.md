* [基础概念](#基础概念)
	* [URI](#uri)
	* [请求和响应报文](#请求和响应报文)
		* [请求报文](#请求报文)
		* [响应报文](#响应报文)
* [HTTP 方法](#http-方法)
	* [GET](#get)
	* [POST](#post)
	* [HEAD](#head)
	* [PUT](#put)
	* [DELETE](#delete)
	* [OPTIONS](#options)
	* [CONNECT](#connect)
	* [TRACE](#trace)
	* [LINK](#link)
	* [UNLINK](#unlink)
	* [PATCH](#patch)
* [HTTP 状态码](#http-状态码)
	* [1XX 信息性状态码](#1xx-信息性状态码)
	* [2XX 成功状态吗](#2xx-成功状态吗)
	* [3XX 重定向错误码](#3xx-重定向错误码)
	* [4XX 客户端错误状态码](#4xx-客户端错误状态码)
	* [5XX 服务器错误状态码](#5xx-服务器错误状态码)
	* [HTTP 常见状态码](#http-常见状态码)
* [HTTP 首部](#http-首部)
	* [HTTP 常见 Header](#http-常见-header)
	* [参考资料（供查阅）](#参考资料供查阅)
		* [通用首部字段](#通用首部字段)
		* [请求首部字段](#请求首部字段)
		* [响应首部字段](#响应首部字段)
		* [实体首部字段](#实体首部字段)
* [HTTP 具体应用](#http-具体应用)
	* [连接管理](#连接管理)
		* [短链接和长连接](#短链接和长连接)
		* [流水线](#流水线)
	* [Cookie](#cookie)
	* [缓存](#缓存)
* [HTTPS](#https)
* [HTTP /1.1 新特性](#http-11-新特性)
* [HTTP 1.0 和 HTTP 1.1 的区别](#http-10-和-http-11-的区别)
* [长连接（PersistentConnection）](#长连接persistentconnection)
	* [HTTP 1.1支持长连接（PersistentConnection）](#http-11支持长连接persistentconnection)
	* [流水线（Pipelining）](#流水线pipelining)
	* [host字段](#host字段)
	* [100(Continue) Status](#100continue-status)
	* [Chunked transfer-coding](#chunked-transfer-coding)
	* [cache](#cache)
* [HTTP 2.0](#http-20)
* [GET 与 POST 比较](#get-与-post-比较)
	* [作用](#作用)
	* [参数](#参数)
	* [安全](#安全)
	* [幂等性](#幂等性)
	* [可缓存](#可缓存)
	* [XMLHttpRequest（了解）](#xmlhttprequest了解)

# 基础概念

## URI

URI：用于表示某一互联网资源名称的字符串，该种标识允许用户对任何的资源通过特定的协议进行交互操作

URL：俗称为网址

URI：统一资源标识符、URL：统一资源定位符、URN：统一资源名称

URI 包含 URL 和 URN

## 请求和响应报文

### 请求报文

- 内容包括：首行、Header、Body

- 首行：[方法] + [URL] + [协议版本号]

- Header：请求的属性，以冒号分隔键值对即（键与值之间用冒号分割）；每组属性之间用 \n 分隔；遇到空行 Header 部分结束

- Body：空行后面的是 Body，允许 Body 为空字符串；如果 Body 存在，则在 Header 中有一个 Content-Length 来标识 Body 的长度

### 响应报文

- 内容包括：首行、Header、Body
- 首行：[协议版本号] + [状态码] + [状态码解释]
- Header：同请求报文 Header
- Body：同请求报文 Body + 如果服务器返回了一个 html 页面，那么 html 页面内容就在 Body 中

# HTTP 方法

​	客户端发送的**请求报文**中第一行为请求行，包含了方法字段

## GET

- 获取资源
- 当前网络中绝大多数用的是 GET 方法

## POST

- 传输实体主体
- POST 主要用来传输数据，而 GET 主要用于获取资源

## HEAD

- 获取报文首部
- 和 GET 方法类似，但是不返回实体主体部分
- 主要用于确认 URL 的有效性及资源更新的日期时间等

## PUT

- 上传文件

- 由于自身不带验证机制，任何人都可以上传文件，因此存在安全性问题，一般不用该种方法

- ```html
  PUT /new.html HTTP/1.1
  Host:example.com
  Content-type:text/html
  Content-Length:16
  <p>
      New File
  </p>
  ```

  

## DELETE

- 删除文件

- 与 PUT 功能相反，并且同样不带验证机制

- ```html
  DELETE /file.html HTTP/1.1
  ```

  

## OPTIONS

- 查询支持的方法

- 查询指定的 URL 能够支持的方法，会返回 

  ```html
  Allow:GET, POST, HEAD, OPTIONS等
  ```

  

## CONNECT

- 要求在与代理服务器通信时建立连接隧道

- 使用 SSL 和 TLS 协议把通信内容加密后经过网络隧道传输

  ```HTML
  CONNECT:www.example.com:443 HTTP/1.1
  ```

  

## TRACE

- 追踪路径
- 服务器会将通信路径返回客户端
- 发送请求时，在 Max-Forwards 首部字段中填入数值，每经过经过一个服务器就会 减 1，当数值为 0 时就会停止传输
- 但是通常不使用 TRACE，并且它容易受到 XST 攻击（跨站追踪）

## LINK

- 建立和资源之间的联系

## UNLINK

- 断开和资源之间的联系

## PATCH

- 对资源进行部分修改

- PUT 也可用于对资源进行修改，但是只能完全替代原始资源，PATCH 允许部分修改

- ```html
  PATCH /file.txt HTTP/1.1
  Host: www.example.com
  Content-Type: application/example
  If-Match: "e0023aa4e"
  Content-Length: 100
  
  [description of changes]
  ```

  

# HTTP 状态码

​	服务器返回的**响应报文**中第一行为状态行，包含了状态码及原因短语，用来告知客户端请求的结果

## 1XX 信息性状态码

## 2XX 成功状态吗

## 3XX 重定向错误码

## 4XX 客户端错误状态码

## 5XX 服务器错误状态码

## HTTP 常见状态码

200 OK 服务器正确处理了请求

301/302 重定向，请求的 URL 已移走，响应中应该包含一个 Location URL，说明资源现在所处位置

304 未修改，客户端缓存资源是最新的，要客户端使用缓存

404 未找到资源

501 服务器出现错误，使其无法对请求提供服务

200 OK：客户端请求成功。

400 Bad Request：客户端请求有语法错误，不能被服务器所理解。

401 Unauthorized：请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用。

403 Forbidden：服务器收到请求，但是拒绝提供服务。

404 Not Found：请求资源不存在，举个例子：输入了错误的URL。

500 Internal Server Error：服务器发生不可预期的错误。

503 Server Unavailable：服务器当前不能处理客户端的请求，一段时间后可能恢复正常，举个例子：HTTP/1.1 200 OK（CRLF）

# HTTP 首部

## HTTP 常见 Header

- Content-Type：数据类型（text/html 等）
- Content-Length：Body 长度
- Host：客户端告知服务器，所请求的资源在哪个主机的端口上；
- User-Agent：声明用户的操作系统和浏览器版本信息
- referer：当前页面是从哪个页面跳转过来的
- location：搭配 3XX 状态码使用，告诉客户端接下来要去访问哪里
- Cookie：用于在客户端存储少量信息，通常用于实现会话 session 的功能

## 参考资料（供查阅）

### 通用首部字段

- Cache-Control：控制缓存行为
- Connection：控制不在转发给代理的首部字段，管理持久转接
- Date：创建报文的日期时间
- Pragma：报文指令
- Trailer：报文末端的首部一览
- Transfer-Encoding：指定报文主体的传输编码方式
- Upgrade：升级为其他协议
- Via：代理服务器相关信息

- Warning：错误通知

### 请求首部字段

- Accept：用户代理可处理的媒体类型
- Accept-Charset：优先的字符集
- Accept-Encoding：优先的内容编码
- Accept-Language：优先的语言
- Authorization：Web 认证信息
- Expert：期待服务器的特定行为
- From：用户的电子邮箱地址
- Host：请求资源所在服务器
- if-Match：比较实体标记
- if-Modified-Since：比较资源的更新时间
- if-None-Match：比较实体标记（与if-Match 相反）
- if-Range：资源未更新时发送实体 Byte 的范围请求
- If-Unmodified-Since：比较资源的更新时间（与 If-Modified-Since 相反）
- Max-Forwards：最大传输逐跳数
- Proxy-Authorization：代理服务器要求客户端的认证信息
- Range：实体的字节范围请求
- Referer：对请求中 URI 的原始获取方
- TE：传输编码的优先级
- User-Agent：HTTP 客户端程序的信息

### 响应首部字段

- Accept-Ranges：是否接受字节范围请求
- Age：推算资源创建经过时间
- ETag：资源的匹配信息
- Location：令客户端重定向至指定 URI
- Proxy-Authenticate：代理服务器对客户端的认证信息
- Retry-After：对再次发起请求的时机要求
- Server：HTTP 服务器的安装信息
- Vary：代理服务器缓存的管理信息
- WWW-Authenticate：服务器对客户端的认证信息

### 实体首部字段

- Allow：资源可支持的 HTTP 方法
- Content-Encoding：实体主体适用的编码方式
- Content-Language：实体主体的自然语言
- Content-Length：实体主体的大小
- Content-Location：替代对应资源的 URI
- Content-MD5：实体主体的报文摘要
- Content-Range：实体主体的位置范围
- Content-Type：实体主体的媒体类型
- Expires：实体主体过期的日期时间
- Last-Modified：资源的最后修改日期时间

# HTTP 具体应用

## 连接管理

### 短链接和长连接

​	当浏览器访问一个包含多张图片的 HTML 页面时，除了请求访问 HTML 页面资源，还会请求图片资源。如果每进行一次 HTTP 通信就要新建一个 TCP 连接，那么开销会很大

长连接只需要建立一次 TCP 连接就能进行多次 HTTP 通信

- 从 HTTP/1.1 开始默认是长连接的，如果要断开连接，需要由客户端或者服务器端提出断开，使用 `Connection : close`
- 在 HTTP/1.1 之前默认是短连接的，如果需要使用长连接，则使用 `Connection : Keep-Alive`

### 流水线

​	默认情况下，HTTP 请求是按顺序发出的，下一个请求只有在当前请求收到响应之后才会被发出。由于会受到网络延迟和带宽的限制，在下一个请求被发送到服务器之前，可能需要等待很长时间

流水线是在同一条长连接上发出连续的请求，而不用等待响应返回，这样可以避免连接延迟

## Cookie

## 缓存

# HTTPS

# HTTP /1.1 新特性

- HTTP/1.1 的首部带有大量信息，而且每次都要重复发送

- 默认是长连接
- 支持流水线
- 支持同时打开多个 TCP 连接
- 支持虚拟主机
- 新增状态码 100
- 支持分块传输编码
- 新增缓存处理指令 max-age

# HTTP 1.0 和 HTTP 1.1 的区别

# 长连接（PersistentConnection）

## HTTP 1.1支持长连接（PersistentConnection）

​	HTTP 1.0 规定浏览器与服务器只保持短暂的连接，浏览器的每次请求都需要与服务器建立一个 TCP 连接，服务器完成请求处理后立即断开 TCP 连接，服务器不跟踪每个客户也不记录过去的请求。

​	HTTP 1.1 则支持持久连接 Persistent Connection, 并且默认使用 persistent  connection. 在同一个 tcp 的连接中可以传送多个 HTTP 请求和响应. 多个请求和响应可以重叠，多个请求和响应可以同时进行. 更加多的请求头和响应头(比如 HTTP1.0 没有 host 的字段).

- 在 1.0 时的会话方式：
- 建立连接
- 发出请求信息
- 回送响应信息
- 关掉连接

​	HTTP 1.1的持续连接，也需要增加新的请求头来帮助实现，例如，Connection请求头的值为Keep-Alive时，客户端通知服务器返回本次请求结果后保持连接；Connection请求头的值为close时，客户端通知服务器返回本次请求结果后关闭连接。HTTP 1.1还提供了与身份认证、状态管理和Cache缓存等机制相关的请求头和响应头。

## 流水线（Pipelining）

​	请求的流水线（Pipelining）处理，在一个TCP连接上可以传送多个HTTP请求和响应，减少了建立和关闭连接的消耗和延迟。例如：一个包含有许多图像的网页文件的多个请求和应答可以在一个连接中传输，但每个单独的网页文件的请求和应答仍然需要使用各自的连接。  HTTP 1.1还允许客户端不用等待上一次请求结果返回，就可以发出下一次请求，但服务器端必须按照接收到客户端请求的先后顺序依次回送响应结果，以保证客户端能够区分出每次请求的响应内容。

## host字段

​	在HTTP1.0中认为每台服务器都绑定一个唯一的IP地址，因此，请求消息中的URL并没有传递主机名（hostname）。但随着虚拟主机技术的发展，在一台物理服务器上可以存在多个虚拟主机（Multi-homed Web Servers），并且它们共享一个IP地址。

​	HTTP1.1的请求消息和响应消息都应支持 Host 头域，且请求消息中如果没有Host头域会报告一个错误（400 Bad Request）。此外，服务器应该接受以绝对路径标记的资源请求。

## 100(Continue) Status

​	HTTP/1.1 加入了一个新的状态码 100（Continue）。客户端事先发送一个只带头域的请求，如果服务器因为权限拒绝了请求，就回送响应码 401（Unauthorized）；如果服务器接收此请求就回送响应码100，客户端就可以继续发送带实体的完整请求了。100 (Continue) 状态代码的使用，允许客户端在发 request 消息 body 之前先用request header 试探一下 server，看 server 要不要接收 request body，再决定要不要发 request body。

## Chunked transfer-coding

​	HTTP/1.1中引入了 Chunked transfer-coding 来解决上面这个问题，发送方将消息分割成若干个任意大小的数据块，每个数据块在发送时都会附上块的长度，最后用一个零长度的块作为消息结束的标志。这种方法允许发送方只缓冲消息的一个片段，避免缓冲整个消息带来的过载。

## cache

​	HTTP/1.1 在 1.0 的基础上加入了一些 cache 的新特性，当缓存对象的 Age 超过 Expire 时变为 stale 对象，cache不需要直接抛弃 stale 对象，而是与源服务器进行重新激活（revalidation）

# HTTP 2.0

# GET 与 POST 比较

## 作用

​	GET 用于获取资源而 POST 用于传输实体

## 参数

​	GET 和 POST 都能使用额外的参数，但是 GET 的参数是以查询字符串出现在 URL 中，而 POST 参数存储在实体中。不能因为 POST 参数存储在实体中就认为它的安全性高，因为照样就可以通过一些抓包工具（Fiddler）

​	因为 URL  只支持 ASSIC 编码，因此 GET 参数中如果存在中文等字符就需要先进行编码，例如：“中文”会转换成“%E4%B8%AD%E6%96%87”，而空格会转换成 %20，POST 参数支持标准字符集

```html
GET /test/demo_form.asp?name1=value1&name2=value2 HTTP/1.1
```

```html
POST /test/demo_form.asp HTTP/1.1
Host: w3schools.com
name1=value1&name2=value2
```



## 安全

​	安全的 HTTP 方法不会改变服务器状态，也就是说它仅仅是只读的

GET 方法是安全的而 POST 方法却不是，因为 POST 的目的是传输实体的内容，这个内容可能是用户上传的表单数据，上传成功之后，服务器可能把这些数据存储在数据库中，因此状态也就发生了改变

​	安全的方法除了 GET 之外还有：HEAD，OPTIONS

​	不安全的方法除了 POST 之外还有：PUT ,DELETE

如何理解服务器的状态发生了改变

## 幂等性

​	幂等的 HTTP 方法，同样的请求被执行一次或者多次的效果是一样的，服务器的状态也是一样的，所有的安全方法都是幂等的

​	在正确实现的条件下，GET、HEAD、PUT 和 DELETE 都是幂等的，而 POST 方法不是

GET /pa'geX HTTP/1.1 是幂等的，连续调用多次，客户端接收到的结果都是一样的

```html
GET /pageX HTTP/1.1
GET /pageX HTTP/1.1
GET /pageX HTTP/1.1
GET /pageX HTTP/1.1
```

POST /add_row HTTP/1.1 不是幂等的，如果调用多次，就会增加多行记录

```html
POST /add_row HTTP/1.1   -> Adds a 1nd row
POST /add_row HTTP/1.1   -> Adds a 2nd row
POST /add_row HTTP/1.1   -> Adds a 3rd row
```

DELETE /idX/delete HTTP/1.1 是幂等的，即便不同的请求接收到的状态码不一样：

```html
DELETE /idX/delete HTTP/1.1   -> Returns 200 if idX exists
DELETE /idX/delete HTTP/1.1   -> Returns 404 as it just got deleted
DELETE /idX/delete HTTP/1.1   -> Returns 404
```

## 可缓存

如果要对响应进行缓存就需要满足以下条件：

- 请求报文的 HTTP 方法本身是可缓存的，包括 GET 和 HEAD，但是 PUT 和 DELETE 是不可缓存的，POST 在多数情况下不可缓存
- 响应报文的状态码是可缓存的，包括：200，203，204，206，300，301，404，414 & 501
- 响应报文的 Cache-Control 首部字段没有指定不进行缓存

## XMLHttpRequest（了解）

为了阐述 POST 和 GET 的另一个区别，需要先了解 XMLHttpRequest：

​	XMLHttpRequest 是一个 API，它为客户端提供了在客户端和服务器之间传输数据的功能。它提供了一个通过 URL 来获取数据的简单方式，并且不会使整个页面刷新。这使得网页只更新一部分页面而不会打扰到用户。XMLHttpRequest 在 AJAX 中被大量使用。

- 在使用 XMLHttpRequest 的 POST 方法时，浏览器会先发送 Header 再发送 Data。但并不是所有浏览器会这么做，例如火狐就不会。
- 而 GET 方法 Header 和 Data 会一起发送。
