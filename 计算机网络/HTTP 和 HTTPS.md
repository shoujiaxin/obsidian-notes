## HTTP

无状态的（协议对于事物处理没有记忆能力）、面向连接的协议。

### 报文结构

#### 请求报文

- 请求行：包含[请求方法](#请求方法)、URL、协议版本等（由空格分隔）
- 请求头部：Host、User-Agent、Connection、Accept-Encoding、Accept-Language、Accept-Charset 等
- 空行：表示请求头部结束
- 请求正文（可选）

#### 响应报文

- 状态行：协议版本、[状态码](#状态码)、状态码描述等（由空格分隔）
- 响应头部：Server、Content-Type、Content-Length、Content-Charset、Content-Encoding、Content-Language 等
- 空行：表示响应头部结束
- 响应正文

### 请求方法

- `GET`：请求指定的页面信息，并返回实体主体
- `HEAD`：类似于 `GET` 请求，但返回的响应中没有具体的内容，用于获取报头
- `POST`：向指定资源提交数据进行处理请求（例如提交表单或者上传文件），数据被包含在请求体中。`POST` 请求可能会导致新的资源的建立和/或已有资源的修改
- `PUT`：从客户端向服务器传送的数据取代指定的文档的内容
- `DELETE`：请求服务器删除指定的页面
- `CONNECT`：HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器
- `OPTIONS`：允许客户端查看服务器的性能
- `TRACE`：回显服务器收到的请求，主要用于测试或诊断
- `PATCH`：是对 `PUT` 方法的补充，用来对已知资源进行局部更新

#### 性质

- 安全 (Safe)：如果一个方法的语义在本质上是只读的，那么这个方法就是安全的
- 幂等 (Idempotent)：同一个请求方法执行多次和仅执行一次的效果完全相同
- 可缓存 (Cacheable)：一个方法是否可以被缓存

#### `GET` 和 `POST` 的区别

- `GET` 的语义是获取制定的资源，是安全、幂等、可缓存的；`POST` 的语义是请求负荷（报文主体）对制定的资源作出处理，是不安全、不幂等、不可缓存的
- `GET` 的请求参数在 URL 中；`POST` 的参数在请求体中
- `GET` 的编码类型为 `application/x-www-form-url`；`POST` 的编码类型为 `encodedapplication/x-www-form-urlencoded` 或 `multipart/form-data`
- `GET` 只允许 ASCII 字符；`POST` 没有限制，也允许二进制
- `GET` 书签可收藏；`POST` 书签不可收藏

### [状态码](https://zh.wikipedia.org/zh-cn/HTTP状态码)

#### 1xx 消息

表示请求已被接受，需要继续处理。此类响应是临时响应，只包含状态行和某些可选的响应头信息，并以空行结束。

#### 2xx 成功

表示请求已成功被服务器接收、理解、并接受。

#### 3xx 重定向

表示需要客户端采取进一步的操作才能完成请求。这些状态码通常用来重定向，后续的请求地址（重定向目标）在本次响应的 Location 域中指明。

#### 4xx 客户端错误

表示客户端可能发生了错误，妨碍了服务器的处理。

#### 5xx 服务器错误

表示服务器无法完成明显有效的请求。此类状态码表示服务器在处理请求的过程中有错误或者异常状态发生，也有可能是服务器意识到以当前的软硬件资源无法完成对请求的处理。

### 长短连接

本质上是 [TCP](TCP.md#TCP) 协议的长连接和短连接。HTTP/1.0 默认使用短连接，HTTP/1.1 起默认使用长连接。

- 短连接：客户端和服务器每进行一次 HTTP 操作，就建立一次连接，任务结束就中断连接
- 长连接：响应头中加入 `Connection:keep-alive`，当一个网页打开完成后，客户端和服务器之间用于传输 HTTP 数据的 TCP 连接会保持一定时间，客户端再次访问这个服务器时，会继续使用这一条已经建立的连接

## HTTPS

HTTPS = HTTP + SSL/TLS

### SSL/TLS

SSL (Secure Sockets Layer) 协议最初是网景公司为了保障网上交易安全而开发的，该协议通过加密来保护客户个人资料，通过认证和完整性检查来确保交易安全，在 TCP 的上一层被实现。

IEFT 在标准化 SSL 时将其改名为 TLS (Transport Layer Security)。SSL 和 TLS 指代的协议版本不同。

### 连接过程

```mermaid
sequenceDiagram
	Client ->> Server: Client Hello
	Server ->> Client: Server Hello
	Server ->> Client: 公钥证书
	Server ->> Client: Server Hello Done
	Note over Client, Server: 第一次 SSL 握手结束
	Client -->> Client: 校验证书
	Client ->> Server: Client Key Exchange
	Client ->> Server: Change Cipher Spec
	Client ->> Server: Finished
	Server ->> Client: Change Cipher Spec
	Server ->> Client: Finished
	Note over Client, Server: SSL 连接建立
	Client ->> Server: HTTP Request
	Server ->> Client: HTTP Response
```

1. 客户端发送 Client Hello 报文开始 SSL 通信。报文中包含客户端支持的 SSL 版本、加密组件列表（加密算法和密钥长度等）
2. 服务端可进行 SSL 通信时，发送 Server Hello 作为应答。报文中包含服务端支持的 SSL 版本以及筛选后的、客户端发来的加密组件列表
3. 服务端发送公钥证书报文
4. 服务端发送 Server Hello Done 报文，表示第一次 SSL 握手结束
5. 客户端校验服务端发来证书的有效性：查找并比对操作系统中受信任的证书发布机构 (CA) 证书
6. 客户端发送 Client Key Exchange 报文。报文包含一个使用证书中公钥加密的随机数 (pre-master secret)，用于生成之后对称加密的密钥
   > 客户端和服务端根据 pre-master secret 生成 master secret，再通过 master secret 生成对称加密的密钥
7. 客户端发送 Change Cipher Spec 报文，提示服务端之后的通信采用对称加密
8. 客户端发送 Finished 报文。该报文包含连接至今全部报文的整体校验值
9. 服务端发送 Change Cipher Spec 报文
10. 服务端发送 Finished 报文。SSL 连接建立
11. 客户端发送 HTTP 请求，服务端响应
12. 客户端断开连接，发送 Close Notify 报文，然后断开 TCP 连接

## 浏览器输入 URL 后发生了什么

1. 检查浏览器和本机是否有该 URL 对应 IP 地址的缓存
2. 如果没有缓存，则向本地 DNS 服务器发起请求
   - 迭代查询
   - 递归查询
3. 建立 TCP 连接
4. 建立 SSL/TLS 连接
5. 浏览器发送 HTTP 请求，服务端返回 HTTP 响应
6. 浏览器解析 & 渲染
7. 如果开启了[长连接](#长短连接)，则维持连接
8. 断开连接
