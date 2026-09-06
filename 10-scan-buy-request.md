# 商户代币空投下单(airdrop.request)

**应用场景**：商户系统创建代币空投订单，向平台发起商户代币空投下单请求。

***

### 1. 请求报文

#### 业务参数定义

| 节点名             | 字段名称     | 可空 | 类型        | 备注                              |
| --------------- | -------- | -- | --------- | ------------------------------- |
| `merchUid`      | 商户UID    | 必填 | N         | 商户UID                           |
| `merchOrderNo`  | 商户单号     | 必填 | C (1,64)  | 商户系统唯一订单号                       |
| `currency`      | 交易币种（代币） | 必填 | C (1,16)  | 参考交易币种字典                        |
| `orderAmt`      | 代币空投数量   | 必填 | N         | 代币空投数量                          |
| `userNo`        | 用户编号     | 必填 | C (1,32)  | 用户唯一标识                          |
| `userIp`        | 用户IP地址   | 必填 | C (1,64)  | 用户发起时的IP地址                      |
| `userName`      | 用户真实姓名   | 必填 | C (1,32)  | 用户真实姓名                          |
| `notifyUrl`     | 交易结果通知地址 | 必填 | C (1,128) | 交易结果终态时根据该地址通知商户系统              |
| `extrasContent` | 扩展内容     | 可空 | C (0,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256 |

#### 请求报文示例

```json
{
  "data": "{\"merchUid\":48351,\"userNo\":\"xxx0408\",\"extrasContent\":\"{\\\"a\\\":1111,\\\"b\\\":2222}\",\"merchOrderNo\":\"MB6208745649\",\"userIp\":\"127.0.1.2\",\"notifyUrl\":\"[http://127.0.0.1](http://127.0.0.1)\",\"currency\":\"TTT\",\"orderAmt\":135.1,\"userName\":\"地在\"}",
  "cusUid": "48351",
  "transCode": "airdrop.request",
  "signMsg": "ELvrtkYf90ReZgrZJSbkDkoUQbLcl03FBh/t80+SbxWmF/5Ggt2uSOmB/E99t+OPRbooNZ/D3QZt8QWWYGs8TH/2IH6zaK3jvJZ57ZQzbeZe7/F844kj3LlDOwr8Q5N0TBHQjbMtMnhR6laVP6HO08nCIF94W98aJNO2pdeBXuBe8Tg+707HyDSLfE2j2bHqzBRSwJBFQ/pSE7ODK6S/LkOk9CHwSEWYtJrB3U+QJYKDH8m4IFzhf18XxOZQ7v7Z05iIvt1D+sPNgAuv/XYfTbvE449xDLKp0akpkhAgvpHWS8OlH6Jg8YsmzWtmP8F9ySx1saw6jOykXAaFHueWuQ==",
  "version": "01",
  "reqSn": "483511715782404830"
}
```

***

### 2. 响应报文

#### 业务参数定义

| 节点名             | 字段名称   | 可空 | 类型        | 备注                                                      |
| --------------- | ------ | -- | --------- | ------------------------------------------------------- |
| `merchUid`      | 商户UID  | 必填 | N         | 商户UID                                                   |
| `merchOrderNo`  | 商户单号   | 必填 | C(1,64)   | 商户系统唯一订单号                                               |
| `orderNo`       | 平台单号   | 必填 | C(19)     | 平台唯一订单号                                                 |
| `tradeDate`     | 交易日期   | 必填 | C(8)      | 平台下单日期，格式:yyyyMMdd                                      |
| `orderAmt`      | 代币空投数量 | 必填 | N         | 代币空投数量                                                  |
| `status`        | 下单状态   | 必填 | N         | <p><code>10</code>: 下单成功<br><code>-100</code>: 下单失败</p> |
| `remark`        | 业务返回描述 | 必填 | C(1,64)   | 业务返回描述                                                  |
| `extrasContent` | 扩展内容   | 可空 | C (0,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256                         |

> **解析说明**：
>
> 1. `retCode` 为 `000000`，并且 `data` 中的 `status` 为 `10`，代表代币空投下单受理成功（不代表交易成功）。
> 2. `retCode` 为 `000000`，并且 `data` 中的 `status` 为 `-100`，代表代币空投下单失败（代表交易失败）。
> 3. `retCode` 为其它情况，请以“商户代币空投查询”中的结果来判定交易是否成功。

#### 响应报文示例

```json
{
  "transCode": "airdrop.request",
  "version": "01",
  "cusUid": "48351",
  "reqSn": "483511715782657945",
  "retCode": "000000",
  "retMsg": "请求成功",
  "data": "{\"extrasContent\":\"{\\\"a\\\":1111,\\\"b\\\":2222}\",\"merchOrderNo\":\"MB0280342697\",\"merchUid\":48351,\"orderAmt\":301.1,\"orderNo\":\"2024051522173803077\",\"remark\":\"请求成功\",\"status\":10,\"tradeDate\":\"20240515\"}",
  "signMsg": "Od1MxyYZhHWTLdV5LN/3bhmT43RINh1N+HPMhqzpOjmaqd8kt2acH9wHpfparlExaMKEYXWv1nFz2yyod1voohYmyjceVizrMWmqjKbnJTOVyoIyr+vlMlpV62DpLIMQ876K1dlJO4bf69CtknENyOFdeWr+jY+ix0ZRLVWoqaw0daZlvNABFraYIEhL2X1OotRkxgO/r7pCXlpWoUcrl4h07jbzSPTZtjuHkbDWu4Ia5BUoCByiYNw+52UaGI0OdU1Qwv3vYt7kYs+Fnxd6bqRH8PjWiJfYYX09/CeYOuR9uKvgyWokTg8guStsXg340pdf8IUT4TBNEbOwVxbMCw=="
}
```
