# 网络协议 & 示例

![network](../.gitbook/assets/network.png)

## 前置知识

### URI、URL、URN

![url](../.gitbook/assets/uri.png)

完整格式如下： `[协议类型]://[访问资源需要的凭证信息]@[服务器地址]:[端口号]/[资源层级UNIX文件路径][文件名]?[查询]#[片段ID]`

[List of URI schemes](https://en.wikipedia.org/wiki/List_of_URI_schemes)

各协议根据使用规范拥有独立格式

其中常用协议有：

* `http/https` - `标准格式`
* `data` - `data:<mediatype>[;base64],<data>`
* `urn` - `urn:<namespace>:<specificpart>` 为了确保 URN 命名空间的全局唯一性，URN 的标识（NID）必须在 IANA 注册。

### UUID

通用唯一识别码（英语：Universally Unique Identifier，缩写：UUID）是用于计算机体系中以识别信息数目的一个 128 位标识符。根据标准方法生成，不依赖中央机构的注册和分配，UUID 具有唯一性，这与其他大多数编号方案不同。重复 UUID 码概率接近零，可以忽略不计。

### TCP/UDP

![tcp](../.gitbook/assets/tcp.svg)

* tcp \| udp：面向连接、可靠、大量数据、速度慢 \| 反之

### TLS/SSL

![ssl](../.gitbook/assets/ssl.svg)

## HTTP、HTTPS

请求报文

```text
GET /search?q=question HTTP/1.1
Host: cn.bing.com
User-Agent: insomnia/6.6.2
Accept: */*

（CONTENT）
```

响应报文

```text
HTTP/1.1 200 OK
Cache-Control: private, max-age=0
Transfer-Encoding: chunked
Content-Type: text/html; charset=utf-8
Expires: Wed, 06 May 2020 03:51:23 GMT
P3P: CP="NON UNI COM NAV STA LOC CURa DEVa PSAa PSDa OUR IND"
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
X-MSEdge-Ref: Ref A: 5922E57BF24C4F229A65C71D4EBAFC2A Ref B: BJ1EDGE0317 Ref C: 2020-05-06T03:52:23Z
Date: Wed, 06 May 2020 03:52:23 GMT

（CONTENT）
```

### 发展与区别

* **http/0.9**
  * 只有 GET 请求，没有 Header
  * 只有 HTML 文件可以传送，无法传输其他类型的文件；也没有状态码或错误代码
* **http/1.1**
  * 引入状态码会在响应开始时发送
  * 引入了 HTTP 头，用 Content-Type 头确定文件类型
* **http/1.1（标准化的协议）**
  * 连接可以复用
  * 增加管线化技术，允许在第一个应答被完全发送之前就发送第二个请求，以降低通信延迟。
  * 支持响应分块。
  * 引入额外的缓存控制机制。
  * 引入内容协商机制，包括语言，编码，类型等，并允许客户端和服务器之间约定以最合适的内容进行交换。
  * Host 头，能够使不同域名配置在同一个 IP 地址的服务器上。
* **http/2**
  * HTTP/2 是二进制协议而不是文本协议。不再可读，也不可无障碍的手动创建，改善的优化技术现在可被实施。
  * 这是一个复用协议。并行的请求能在同一个链接中处理，移除了 HTTP/1.x 中顺序和阻塞的约束。
  * 压缩了 headers。因为 headers 在一系列请求中常常是相似的，其移除了重复和传输重复数据的成本。
  * 其允许服务器在客户端缓存中填充数据，通过一个叫服务器推送的机制来提前请求。
* **http/3**
  * [https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Alt-Svc](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Alt-Svc)
  * 参考文档 wiki [https://en.wikipedia.org/wiki/HTTP/3](https://en.wikipedia.org/wiki/HTTP/3)
  * usage [https://blog.cloudflare.com/http3-the-past-present-and-future/](https://blog.cloudflare.com/http3-the-past-present-and-future/)

### [请求方式](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods)

1. `GET`方法请求一个指定资源的表示形式. 使用 GET 的请求应该只被用于获取数据.
2. `HEAD`方法请求一个与 GET 请求的响应相同的响应，但没有响应体.
3. `POST`方法用于将实体提交到指定的资源，通常导致在服务器上的状态变化或副作用.
4. `PUT`方法用请求有效载荷替换目标资源的所有当前表示。
5. `DELETE`方法删除指定的资源。
6. `CONNECT`方法建立一个到由目标资源标识的服务器的隧道。
7. `OPTIONS`方法用于描述目标资源的通信选项。
8. `TRACE`方法沿着到目标资源的路径执行一个消息环回测试。
9. `PATCH`方法用于对资源应用部分修改。

### [返回码](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status)

信息响应\(100–199\)，成功响应\(200–299\)，重定向\(300–399\)，客户端错误\(400–499\)和服务器错误 \(500–599\)

100 Continue 200 Ok 201 Create —— 对应 PUT 请求，，目标资源不存在，并且 PUT 方法成功创建了一份 204 No Content —— 对应 PUT 请求，更新，但不刷新页面 205 Reset Content —— 对应 PUT 请求，更新，刷新页面 206 Partial Content —— 部分响应

e.g.

```text
HTTP/1.1 206 Partial Content
Date: Wed, 15 Nov 2015 06:25:24 GMT
Last-Modified: Wed, 15 Nov 2015 04:58:08 GMT
Content-Range: bytes 21010-47021/47022
Content-Length: 26012
Content-Type: image/gif

... 26012 bytes of partial image data ...
```

```text
HTTP/1.1 206 Partial Content
Date: Wed, 15 Nov 2015 06:25:24 GMT
Last-Modified: Wed, 15 Nov 2015 04:58:08 GMT
Content-Length: 1741
Content-Type: multipart/byteranges; boundary=String_separator

--String_separator
Content-Type: application/pdf
Content-Range: bytes 234-639/8000

...the first range...
--String_separator
Content-Type: application/pdf
Content-Range: bytes 4590-7999/8000

...the second range
--String_separator--
```

301 Moved Permanently 302 Moved Temporarily 304 Not Modified 400 Bad Request 401 Unauthorized 403 Forbidden 404 Not Found 405 Method Not Allowed 408 Request Timeout 500 Internal Server Error 502 Bad Gateway 504 Gateway Timeout

### [Header](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers)

头包含连接信息，控制内容协商，缓存，跨域，也可以自定义特殊需求

#### 认证

| 端 | header | 解释 | e.g. |
| :--- | :--- | :--- | :--- |
| Server | `Authorization` | 发送 Authorization 认证 | `Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l` |
| Server | `WWW-Authenticate` | Authorization 不通过时，WWW-Authenticate header 通常会和一个 401 Unauthorized 的响应一同被发送 | `Accept: text/html, */*;q=0.9` |

鉴权

1. Cookie Session

登录-服务器存session-注入cookie

1. JWT

`Authorization: Bearer JWT`

1. Basic Auth

`Authorization: Basic base64encode(username+":"+password)`

1. OAuth \(2.0\)

#### 缓存

| 端 | header | 解释 | e.g. |
| :--- | :--- | :--- | :--- |
| Client | `Age` | 值则是表示代理服务器当前的系统时间与此应答中的通用头 Date 的值之差 | `Age: <delta-seconds>` |
| Server | `Date` | 报文创建的日期和时间 | `Date: Wed, 21 Oct 2015 07:28:00 GMT` |
|  |  |  |  |
| Client | `Expires` | http/1.0 在此时候之后，响应过期。 | `Expires: Wed, 21 Oct 2015 07:28:00 GMT` |
| Server | `Cache-control` | http/1.1 缓存控制 no-store,no-cache,private,public max-age=31536000,设置后 Expires 失效，响应 200 | `Cache-Control: no-store` |
|  |  |  |  |
| Client | `If-Modified-Since` | 上次收到的修改时间 | `If-Modified-Since: Wed, 21 Oct 2015 07:28:00 GMT` |
| Server | `Last-Modified` | 最近修改时间 ，响应 304 | `Last-Modified: Wed, 21 Oct 2015 07:28:00 GMT` |
|  |  |  |  |
| Client | `If-None-Match` | 上次 Etag 值 ，响应 304 | `If-None-Match: "bfc13a64729c4290ef5b2c2730249c88ca92d82d"` |
| Server | `Etag` | 一般为文件 MD5 值 | `ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"` |
|  |  |  |  |
| Server | `Vary` | 其他响应头是否加入`If-None-Match`的校验 | `Vary: User-Agent` |

#### 内容协商

| 端 | header | 解释 | e.g. |
| :--- | :--- | :--- | :--- |
| Server | `Connection` | 决定当前的事务完成后，是否会关闭网络连接 | `Connection: keep-alive` |
| Server | `Keep-Alive` | 允许消息发送者暗示连接的状态，还可以用来设置超时时长和最大请求数 | `Keep-Alive: timeout=5, max=1000` |
|  |  |  |  |
| Client | `Accept` | 告知（服务器）客户端可以处理的内容类型 | `Accept: text/html, */*;q=0.9` |
| Client | `Accept-Charset` | 告知（服务器）客户端可以处理的字符集类型 | `Accept-Charset: utf-8, iso-8859-1;q=0.5` |
| Server | `Content-Type` | 资源的 MIME 类型 | `Content-Type: text/html; charset=utf-8` |
|  |  |  |  |
| Client | `Accept-Encoding` | 可处理的内容编码方式 | `Accept-Encoding: deflate, gzip;q=1.0, *;q=0.5` |
| Server | `Content-Encoding` | 内容编码 | `Content-Encoding: deflate` |
|  |  |  |  |
| Client | `Accept-Language` | 可处理语言 | `Accept-Language: fr-CH, fr;q=0.9, en;q=0.8, de;q=0.7, *;q=0.5` |
| Server | `Content-Language` | 内容语言 | `Content-Encoding: zh-cn` |

[Accept-Encoding](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Encoding)

* gzip：采用 Lempel-Ziv coding \(LZ77\) 压缩算法
* deflate：采用 zlib 结构和 deflate 压缩算法
* br：采用 Brotli 算法的编码
* compress：采用 Lempel-Ziv-Welch \(LZW\) 压缩算法
* identity：不压缩
* \*

NODE 端处理压缩 \(zlib\)

* zlib.Inflate, zlib.gzip 两种压缩方式
* zlib.Unzip 可自动检测并解压
* express，koa 提供中间件 require\('compression'\)等，不要手写

e.g.

```javascript
const fs = require('fs');
const zlib = require('zlib');

const gzip = zlib.createGzip();
const readableStream = fs.createReadStream('./hello.txt');
const writeableStream = fs.createWriteStream('./hello.txt.gz');

readableStream.pipe(gzip).pipe(writeableStream);
```

#### 跨域

| 端 | header | 解释 | e.g. |  |
| :--- | :--- | :--- | :--- | :--- |
| Client | `Origin` | 源网站 | `Origin: http://foo.example` |  |
| Client | `Access-Control-Request-Methods` | Option 请求所允许使用的 HTTP 方法 | `Access-Control-Request-Methods: <method>[, <method>]*` |  |
| Client | `Access-Control-Request-Headers` | Option 预检需要的 Headers | `Access-Control-Request-Headers: <field-name>[, <field-name>]*` |  |
| Server | `Access-Control-Allow-Methods` | 实际请求所允许使用的 HTTP 方法 | `Access-Control-Allow-Methods: <method>[, <method>]*` |  |
| Server | `Access-Control-Allow-Headers` | 实际请求所允许使用的 Header | `Access-Control-Allow-Headers: <field-name>[, <field-name>]*` |  |
| Server | `Access-Control-Allow-Origin` | 允许访问该资源的外域 URI。对于不需要携带身份凭证的请求，服务器可以指定该字段的值为通配符 | \`Access-Control-Allow-Origin:  | \*\` |
| Server | `Access-Control-Expose-Headers` | 跨域访问时，XMLHttpRequest 对象的 getResponseHeader\(\)方法只能拿到一些最基本的响应头 Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma 如果要访问其他头，则需要服务器设置本响应头。 | `Access-Control-Expose-Headers: X-My-Custom-Header, X-Another-Custom-Header` |  |
| Server | `Access-Control-Max-Age` | 在有效时间内，浏览器无须为同一请求再次发起预检请求。 | `Access-Control-Max-Age: 86400` |  |
| Server | `Access-Control-Allow-Credentials` | withCredentials 标志设置为 true，从而向服务器发送 Cookies | `Access-Control-Allow-Credentials: true` |  |

#### 缓存和其他

| 端 | header | 解释 | e.g. |
| :--- | :--- | :--- | :--- |
| Client | `Cookie` | 缓存列表 | `Cookie: name=value; name2=value2; name3=value3` |
| Server | `Set-Cookie` | 设置[Cookie](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Set-Cookie) | `Set-Cookie: <cookie-name>=<cookie-value>` |
|  |  |  |  |
| Server | `Location` | 301,302 跳转 | `Location: https://www.taobao.com/?q=question?q=question` |
| Client | `Host` | 请求的主机名 | `Host: www.taobao.com` |
| Client | `User-Agent` | 客户端信息 | `User-Agent: insomnia/6.6.2` |
|  |  |  |  |
| Client | `Range` | 告知服务器返回文件的哪一部分 | `Range: bytes=200-1000, 2000-6576, 19000-` |
| Server | `Accept-Ranges` | 自身支持范围请求\(partial requests\) | `Accept-Ranges: bytes` |
| Server | `Content-Range` | 文件部分 | `Content-Range: <unit> <range-start>-<range-end>/<size>` |

### 传参方式

application/x-www-form-urlencoded

```text
POST http://www.example.com HTTP/1.1
Content-Type: application/x-www-form-urlencoded;charset=utf-8
title=test&sub%5B%5D=1&sub%5B%5D=2&sub%5B%5D=3
```

multipart/form-data

```text
POST /test HTTP/1.1 
Host: foo.example
Content-Type: multipart/form-data;boundary="boundary" 

--boundary 
Content-Disposition: form-data; name="field1" 

value1 
--boundary 
Content-Disposition: form-data; name="field2"; filename="example. txt" 

value2
--boundary--
```

application/json

```text
POST http://www.example.com HTTP/1.1 
Content-Type: application/json;charset=utf-8

{"title":"test","sub":[1,2,3]}
```

text/xml

```text
POST http://www.example.com HTTP/1.1 
Content-Type: text/xml

<?xml version="1.0"?>
<methodCall>
    <methodName>examples.getStateName</methodName>
    <params>
        <param>
            <value><i4>41</i4></value>
        </param>
    </params>
</methodCall>
```

### WebSocket 的实现和应用

WebSocket

> WebSocket 是 HTML5 中的协议，支持持久连续，http 协议不支持持久性连接。Http1.0 和 HTTP1.1 都不支持持久性的链接，HTTP1.1 中的 keep-alive，将多个 http 请求合并为 1 个

WebSocket 是什么样的协议，具体有什么优点

> \(1\) HTTP 的生命周期通过 Request 来界定，也就是 Request 一个 Response，那么在 Http1.0 协议中，这次 Http 请求就结束了。在 Http1.1 中进行了改进，是的有一个 connection：Keep-alive，也就是说，在一个 Http 连接中，可以发送多个 Request，接收多个 Response。但是必须记住，在 Http 中一个 Request 只能对应有一个 Response，而且这个 Response 是被动的，不能主动发起。 \(2\) WebSocket 是基于 Http 协议的，或者说借用了 Http 协议来完成一部分握手，在握手阶段与 Http 是相同的。我们来看一个 websocket 握手协议的实现，基本是 2 个属性，upgrade，connection。

基本请求如下：

```text
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
Origin: http://example.com
```

多了下面 2 个属性：

```text
Upgrade:webSocket
Connection:Upgrade
告诉服务器发送的是websocket
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
```

