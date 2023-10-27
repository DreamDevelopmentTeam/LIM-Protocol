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

### Client Hello

客户端握手数据报，包含客户端版本/MAC地址/公钥等信息：
```json
{
  "ver": 10000,
  "mac": "DB-9A-80-30-F6-3E",
  "key": "-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhkiG9w0BAQEFA ...... ywIDAQAB\n-----END PUBLIC KEY-----"
}
```

| 字段名 | 描述 |
| --- | --- |
| ver | 客户端协议版本号 |
| mac | 客户端机器码/网卡MAC地址 |
| key | 客户端非对称加密公钥 |

