---
title: cookie_session_token
date: 2020-01-13 15:24:58
categories: 浏览器
---
# 基本作用
三者的作用大同小异，基本上是为了管理会话，使同域下的网页能够共享数据，例如用户身份验证，购物车管理等。
出现原因是因为http是无状态的。

无状态是指这次请求与上一次请求之间完全没有联系。

# cookie
是一段存储在web端的数据，以key-value形式存在。一般由后端通过setCookie设置，每次请求都会带上cookie。

# session
是一段存储在服务端上的数据，占用服务器内存，有状态。
当浏览器第一次发送请求时，由服务端生成sessionid，可以通过cookie或附加在url上返回，等下次请求时会被带上，在服务端根据sessionid做校验。
- ❌ 容易造成CSRF攻击。
- ❌ 只适用于单个服务器，不能实现负载均衡，即服务器集群不能共享session
- ❌ 随用户变多开销变大

# token
一个流行的方案：Json Web Token
是一段存储在客户端上的数据，不占用服务器内存，无状态
