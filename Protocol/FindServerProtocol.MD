# Find Server Protocol
> 服务器发现协议

---

## 协议规范

该协议用于在局域网中查找和发现暴露在网络中的TLIM服务器。

### 客户端报文（发现报文）
> #TLIM@FindServer%Find

### 服务端报文（相应报文）
> #TLIM@FindServer%Return

**服务端监听端口：65000/UDP**

## 通信方式

当新客户端上线时，通过向局域网广播发现报文的方式来检索服务端信息。

