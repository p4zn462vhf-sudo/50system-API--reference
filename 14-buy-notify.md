# 购买结果通知 (buy.notify)

**应用场景**：购买订单产生终态（交易成功或交易失败）时，平台系统主动将交易结果通过回调地址异步通知给商户系统

***

### 通知报文（业务参数定义）

| 节点名 | 字段名称 | 可空 | 类型 | 备注 |
| --- | --- | -- | --- | --- |
| merchUid | 商户UID | 必填 | N | 商户UID |
| merchOrderNo | 商户单号 | 必填 | C(1,64) | 商户系统唯一订单号 |
| orderNo | 平台单号 | 必填 | C(1,64) | 平台唯一订单号 |
| tradeDate | 交易日期 | 必填 | C(8) | 平台下单日期，格式: yyyyMMdd |
| payType | 支付方式 | 必填 | C(1,32) | 支付渠道类型，如：OsBank |
| orderAmt | 下单金额 | 必填 | N | 订单创建金额 |
| payAmt | 实际支付金额 | 必填 | N | 实际到账/支付金额 |
| status | 业务状态码 | 必填 | N | 100: 交易成功；-100: 交易失败 |
| payTime | 支付完成时间 | 必填 | C(19) | 交易成功时返回，格式: yyyy-MM-dd HH:mm:ss |
| remark | 业务返回描述 | 必填 | C(1,128) | 结果描述（如“取消”或失败原因） |
| ext | 扩展字段 | 可空 | C(1,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256 |

注：解析说明

1、以data中status为判定交易是否成功（status = 100, 代表交易成功）

2、商户收到通知并校验签名无误后，需按照通用响应规范返回应答

通知报文示例

```json
{
  "data": "{\"merchUid\":42400,\"merchOrderNo\":\"QC0070424495\",\"orderNo\":\"2023101211323816431\",\"tradeDate\":\"20231012\",\"payType\":\"OsBank\",\"orderAmt\":689,\"payAmt\":0,\"status\":-100,\"payTime\":\"2023-11-01 00:32:26\",\"remark\":\"取消\"}",
  "version": "1.0",
  "reqSn": "1698769946033",
  "cusUid": "42400",
  "transCode": "buy.notify",
  "signMsg": "naZlLTaOEgcC/H23y4MsDMBZA7rx6WJRojaj/znTRfyO1ERRK8MnG/4ROXNGec2SZ3qQS4syT5le+wGXI2D5qCZ29QoTWdDtTITdZe2XlwbYLNdKrFKGjtSfyBYQQQR9c9KnEZ+HEYMKtWdKJnmpkbto86zUOZZU6xgKIlr71ZwX4joRdwqbwEGFHZ43ufriim6CnUmLQaKOxF08wyQUR6PWrosI40gDPB0cu7vgN9/Q3lJoEad7Mb5v/tcWGBYBBNU+/LC63VN/Y/E7xFL0Wg2abFV3vL3EtTIPXPLCJmkJywV9y2obgxnxi89Idl3FFPPNwsLTsZFdw7Ep9EOkbg=="
}
```
通知报文(业务参数定义)

| 节点名 | 字段名称 | 可空 | 类型 | 备注 |
| --- | --- | -- | --- | --- |
| merchUid | 商户UID | 必填 | N | 商户UID |
| merchOrderNo | 商户单号 | 必填 | C(1,64) | 商户系统唯一订单号 |
| orderNo | 平台单号 | 必填 | C(1,64) | 平台唯一订单号 |
| tradeDate | 交易日期 | 必填 | C(8) | 平台下单日期，格式: yyyyMMdd |
| payType | 支付方式 | 必填 | C(1,32) | 支付渠道类型，如：OsBank |
| orderAmt | 下单金额 | 必填 | N | 订单创建金额 |
| payAmt | 实际支付金额 | 必填 | N | 实际到账/支付金额 |
| status | 业务状态码 | 必填 | N | 100: 交易成功；-100: 交易失败 |
| payTime | 支付完成时间 | 必填 | C(19) | 交易成功时返回，格式: yyyy-MM-dd HH:mm:ss |
| remark | 业务返回描述 | 必填 | C(1,128) | 结果描述（如“取消”或失败原因） |
| ext | 扩展字段 | 可空 | C(1,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256 |
