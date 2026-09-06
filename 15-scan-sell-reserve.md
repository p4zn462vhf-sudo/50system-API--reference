# 商户二维码出售预约下单 (scan.sell.reserve.request)

**应用场景**：商户系统已创建的二维码预约出售订单，由平台撮合商户向平台发起二维码出售撮合的请求，预约出售的订单在运营平台有预约的展示，方便运营精准调配出售订单

***

### 请求报文（业务参数定义）

| 节点名 | 字段名称 | 可空 | 类型 | 备注 |
| --- | --- | -- | --- | --- |
| merchUid | 商户UID | 必填 | N | 商户UID |
| merchOrderNo | 商户单号 | 必填 | C(1,64) | 商户系统唯一订单号 |
| currency | 交易币种 | 必填 | C(1,16) | 币种代码，如：UDK |
| payType | 支付方式 | 必填 | C(1,32) | 支付渠道类型，如：wxpay |
| orderAmt | 下单金额 | 必填 | N | 下单金额 |
| recAcctType | 收款账号类型 | 必填 | N | 收款账号类型代码，如：40 |
| recAcctName | 收款人姓名 | 必填 | C(1,32) | 收款账户真实姓名 |
| recUrl | 收款二维码链接 | 必填 | C(1,512) | 收款二维码图片URL |
| userNo | 用户编号 | 必填 | C(1,32) | 发起出售的用户唯一标识 |
| userName | 用户真实姓名 | 必填 | C(1,32) | 用户真实姓名 |
| userIp | 用户IP地址 | 必填 | C(1,64) | 用户真实IP |
| userLevel | 用户等级 | 可空 | N | 用户等级,如：0、1、2、3、4、5、6、7、8、9等级 |
| notifyUrl | 异步通知地址 | 必填 | C(1,128) | 交易结果通知地址 |
| extrasContent | 扩展字段 | 可空 | C(1,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256 |

#### 请求报文示例

```json
{
  "data": "{\"userNo\":\"xxx1016002\",\"extrasContent\":\"{\\\"a\\\":1111,\\\"b\\\":2222}\",\"recAcctType\":40,\"orderAmt\":2438.44,\"userName\":\"许嘉\",\"merchUid\":48351,\"payType\":\"wxpay\",\"userLevel\":1,\"merchOrderNo\":\"MB8362856228\",\"recUrl\":\"[http://127.0.0.1/qrcode.jpg](http://127.0.0.1/qrcode.jpg)\",\"userIp\":\"127.0.1.1\",\"notifyUrl\":\"[http://127.0.0.1](http://127.0.0.1)\",\"currency\":\"UDK\",\"recAcctName\":\"许嘉浩\"}",
  "cusUid": "48351",
  "transCode": "scan.sell.request",
  "signMsg": "W1dgZzZSXeG/MS/B12P8KDCAgIIRZHYlOjOTSKadoNsaKQbLHC6MrbxpBPtoanlTzEYWkvxPr2vF/iUnimXCVf+4bbY9yEfafp6mkzHJO4C9+Ba8SaARt2vSf0yjnQyywnW3ts5D46K0eSppupAED5FANwwOJOf+Wxz436x9ADIBbrGtXcpocfLYwT89SmFEIU8xQGZDp/7QVAzVs6SxfQGrFu2dQjINUtH9FVvkdkkhSOSqFMmtRka9A4Jmc+gfbW7hOM5j/+DIgmGCRBbCyV9ZvbnNaWQuhbvgLQ4K6X0qmQde/RZWSOu6CnoPL7AeHDWIaY2dwaBsu7s6yLEzaQ==",
  "version": "01",
  "reqSn": "483511705906984827"
}
```

响应报文(业务参数定义)

| 节点名 | 字段名称 | 可空 | 类型 | 备注 |
| --- | --- | -- | --- | --- |
| merchUid | 商户UID | 必填 | N | 商户UID |
| merchOrderNo | 商户单号 | 必填 | C(1,64) | 商户系统唯一订单号 |
| orderNo | 平台单号 | 必填 | C(1,64) | 平台唯一订单号 |
| orderAmt | 下单金额 | 必填 | N | 下单金额 |
| tradeDate | 平台下单日期 | 必填 | C(8) | 平台下单日期，格式: yyyyMMdd |
| status | 业务状态码 | 必填 | N | 10:下单成功； -100:下单失败 |
| remark | 业务返回描述 | 必填 | C(1,64) | 业务返回描述 |
| extrasContent | 扩展字段 | 可空 | C(1,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256 |

注：解析说明

1、retCode 为000000，并且data中的status为10，代表扫码出售下单受理成功(不代表交易成功)

2、retCode 为000000，并且data中的status为-100，代表扫码出售下单失败(代表交易失败)

3、retCode 为其它情况，请以”商户出售查询”中的结果来判定交易是否成功

响应报文示例
```json
{
  "transCode": "scan.sell.request",
  "version": "01",
  "cusUid": "48351",
  "reqSn": "483511705906984827",
  "retCode": "000000",
  "retMsg": "请求成功",
  "data": "{\"extrasContent\":\"{\\\"a\\\":1111,\\\"b\\\":2222}\",\"merchOrderNo\":\"MB8362856228\",\"merchUid\":48351,\"orderAmt\":2438.44,\"orderNo\":\"2024012215030599349\",\"remark\":\"请求成功\",\"status\":10,\"tradeDate\":\"20240122\"}",
  "signMsg": "ltlaVEmR+gCiMr1gUdQIIweSJsFkruIr9TnzfR1a+g0PljT5SM+PTZBYNSDz/TYJX8ZqkvX1LWNxU25PdILkMEmiiB6pHEn4ZyEXbeIatj1PMzUOK9HuXzo/NwX/85f+0iVZO6wqOu2wW+sYpvEUIMtUkluJSQ2fHHnGo7BwwnqoVYPTA4niVXRcahZBbni4cR7p4WMyp5JYLc4ZYY5wZLjnuvv5dExBDbUOk/fewfEX7D/SH3VVtQfvjyhiyvKdWNOU3gkAgbeEx7JhDqVGRwbtO5ATrT1rkUIg2Eba0e9bxB29J5DK2SeTp4LyxKpQAkZnzVmPRc5f4Jv7zEpy0A=="
}
```
