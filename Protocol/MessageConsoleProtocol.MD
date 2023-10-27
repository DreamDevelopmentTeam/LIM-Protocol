# 消息控制协议
> Message Console Protocol

---

---

## 握手阶段

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


### Server Hello

服务端握手数据报，包含服务端版本/服务端名称/服务端描述/公钥等信息：
```json
{
  "ver": 10000,
  "name": "信息课官方服务器",
  "desc": "本机房唯一官方通讯服务器！",
  "key": "-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhkiG9w0BAQEFA ...... ywIDAQAB\n-----END PUBLIC KEY-----"
}
```

| 字段名 | 描述 |
| --- | --- |
| ver | 服务端协议版本号 |
| name | 服务端名称 |
| desc | 服务端简介/描述 |
| key | 服务端非对称加密公钥 |



### Client Key Test

客户端用于检测与服务端之间的加密通信是否成功建立的消息：
```json
{
  "code": 114514
}
```

| 字段名 | 描述 |
| --- | --- |
| code | 客户端随机生成的整形数据 |

### Server Key Test

服务端回复客户端的用于检测与服务端之间的加密通信是否成功建立的消息：
```json
{
  "code": 114514
}
```

| 字段名 | 描述 |
| --- | --- |
| code | 客户端发来的随机生成的整形数据 |


---

## 消息传输阶段

完成握手阶段后，进入消息传输阶段。该阶段发送的所有数据报文应遵循以下格式：
```json
{
  "type": 0,
  "time": 1696241166000,
  "data": {}
}
```

| 字段名 | 描述 |
| --- | --- |
| type | 消息类型 |
| time | 时间戳（毫秒级13位） |
| data | 消息附带的数据 |

### ---

