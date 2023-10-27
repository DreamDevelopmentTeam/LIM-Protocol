# 消息控制协议
> Message Console Protocol

---

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

---

### 用户注册 C->S

Type = 1

```json
{
  "name": "张三",
  "pwd": "8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92"
}
```

| 字段名 | 描述 |
| --- | --- |
| name | 用户昵称 |
| pwd | SHA256加密后的用户密码数据 |

### 用户注册 S->C

Type = 1

> 成功
```json
{
  "code": "200",
  "msg": "注册成功！请登录！",
  "id": 10001
}
```

> 失败
```json
{
  "code": "403",
  "msg": "您的电脑在黑名单中！",
  "id": 0
}
```

| 字段名 | 描述 |
| --- | --- |
| code | 发送给客户端的请求状态码 |
| msg | 发送给客户端的提示信息 |
| id | 用户注册所获取的用户ID |


### 用户登录 C->S

Type = 2

```json
{
  "id": 10001,
  "pwd": "8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92"
}
```

| 字段名 | 描述 |
| --- | --- |
| id | 用户注册所获取的用户ID |
| pwd | SHA256加密后的用户密码数据 |

### 用户登录 S->C

Type = 2

> 成功
```json
{
  "code": "200",
  "msg": "欢迎登录！张三！",
  "token": "6ea7fb05fa7a62bce5d0551bb0e6d26b56264c4e290443b215acc37565f1432c"
}
```

> 失败
```json
{
  "code": 403,
  "msg": "错误的密码！",
  "token": ""
}
```

| 字段名 | 描述 |
| --- | --- |
| code | 发送给客户端的请求状态码 |
| msg | 发送给客户端的提示信息 |
| token | 用户登录请求获得的Token临时密钥 |



### 消息 C->S

Type = 3

```json
{
  "token": "6ea7fb05fa7a62bce5d0551bb0e6d26b56264c4e290443b215acc37565f1432c",
  "to": 10002,
  "msg": "你好啊！李四！",
  "type": 1,
}
```

| 字段名 | 描述 |
| --- | --- |
| token | 用户登录请求获得的Token临时密钥 |
| to | 消息接收目标 |
| msg | 字符串消息数据 |
| type | TLIM协议中规定的消息类型 |

### 消息 S->C

Type = 3

```json
{
  "from": 10002,
  "to": 10001,
  "msg": "你好啊！张三！",
  "type": 1,
}
```

| 字段名 | 描述 |
| --- | --- |
| from | 消息发送方 |
| to | 消息接收目标 |
| msg | 字符串消息数据 |
| type | TLIM协议中规定的消息类型 |










