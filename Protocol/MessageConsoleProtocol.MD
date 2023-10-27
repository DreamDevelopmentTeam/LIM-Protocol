# 消息控制协议
> Message Console Protocol

---

消息控制协议用于控制TLIM客户端与TLIM服务端之间的通信过程。

协议流程如下：

```text
C->S : Client Hello
S->C : Server Hello
C->S : Client Key Test
S->C : Server Key Test
---------
C->S : ...
S->C : ...
.... : ...
```
