# 商户二维码购买取消 (scan.buy.cancel)

**应用场景**：商户系统向平台发起二维码购买取消交易，平台关闭当前二维码购买订单的交易状态

***

### 请求报文（业务参数定义）

| 节点名 | 字段名称 | 可空 | 类型 | 备注 |
| --- | --- | -- | --- | --- |
| merchUid | 商户UID | 必填 | N | 商户UID |
| merchOrderNo | 商户单号 | 必填 | C(1,64) | 商户系统唯一订单号 |
| tradeDate | 交易日期 | 必填 | C(8) | 下单时返回的交易日期，格式:yyyyMMdd，只使用商户单号查询时，交易日期为必填 |
| operator | 操作人 | 必填 | C(1,32) | 操作人姓名或标识 |
| remark | 取消原因 | 必填 | C(1,128) | 业务取消原因说明 |

#### 请求报文示例

```json
{
  "data": "{\"merchUid\":48351,\"merchOrderNo\":\"xx1858058697\",\"remark\":\"我要取消 \",\"tradeDate\":\"20230901\",\"operator\":\"操作人A\"}",
  "cusUid": "48351",
  "transCode": "scan.buy.cancel",
  "signMsg": "BHxDoDlubDzWVkje9Fs0rhtDZKKogpCAHLDkHIDuy2z6UbCZzZuWmJ98lZLsLw7afUlKdy0e+Pb8tiztylG7MVHydbopnzO8/i7FeF+7lGOhj8gtaV8uPUcdJnSzgONv0yY47kFYnMEg2YWfEoKuTOJDO42C6XoZO1h+RzN4ApskXWUlvhpmM0brqkUFAi76PYzzy+NTYWG01ybNMD2TO5zVE8e9gbqpAY4sOeZj3/10i+gbs1K5UHjjYBDGAUukHgRk03TjG97X2+D6lOyfhXrEIoKX/nSaqAgUR9fzCMvLfaGZn8D50TLEUcXmQkABOnIEtixWocG7ynk1xD3Ubg==",
  "version": "01",
  "reqSn": "483511705906984816"
}
```


响应报文(业务参数定义)
```json
| 节点名 | 字段名称 | 可空 | 类型 | 备注 |
| --- | --- | -- | --- | --- |
| merchUid | 商户UID | 必填 | N | 商户UID |
| merchOrderNo | 商户单号 | 必填 | C(1,64) | 商户系统唯一订单号 |
| tradeDate | 交易日期 | 必填 | C(8) | 平台下单日期，格式:yyyyMMdd |
| status | 业务状态码 | 必填 | N | 业务状态，-100 代表交易失败/已取消 |

```

注：解析说明

1、retCode 为000000，并且data中的status为-100 代表扫码购买取消成功(代表交易失败)

2、retCode 为其它情况，代表扫码购买取消失败


响应报文示例
```json
{
  "transCode": "scan.buy.cancel",
  "version": "01",
  "cusUid": "48351",
  "reqSn": "483511705906984816",
  "retCode": "000000",
  "retMsg": "请求成功",
  "data": "{\"merchOrderNo\":\"xx1858058697\",\"merchUid\":48351,\"status\":-100,\"tradeDate\":\"20230901\"}",
  "signMsg": "ASBl2spuEGxoDEvEdqNaNjMc1lJ8gkN3ORTF8xth/uBOi9FOMka4G7h94YuJwp4NnsqbjKU6xyMdSqA8nS5ulNhoruuNyOJQ1jyO5+af2V05l1ilBqtP2bvB6VEzeFWIc2t1a7RovnjkJJl0BXLsd/Pr/7qkUBfwhnexv1Y4L9lDEskYC11Vr1fDe0O9zMmAXZdXDNHxZQCBgcYJL7Ynw+vYb609UwJ6T2s0zG+wEYxYELBp8OPtmirQkjFr2tFbRo3HRadR95+ae+WYka9VurxoS3gERSO7Yy/7hhvMX1X+oRmfTuPAo3SXercgFnfK6SOIwqU023LmSY3TcpKhrw=="
}
```
