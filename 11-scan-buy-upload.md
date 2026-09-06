# 商户购买凭证上传 (buy.upload)

**应用场景**：商户用户支付完成后，将支付凭证链接通过此接口通知平台

***

### 请求报文（业务参数定义）

| 节点名 | 字段名称 | 可空 | 类型 | 备注 |
| --- | --- | -- | --- | --- |
| merchUid | 商户UID | 必填 | N | 商户UID |
| orderNo | 平台单号 | 可空 | C(1,64) | 平台单号 |
| merchOrderNo | 商户单号 | 必填 | C(1,64) | 商户系统唯一订单号 |
| tradeDate | 交易日期 | 必填 | C(8) | 下单日期，格式: yyyyMMdd |
| certUrl | 凭证链接 | 必填 | C(1,1024) | 支付凭证图片URL数组JSON字符串 |
| notifyUrl | 异步通知地址 | 必填 | C(1,128) | 交易结果通知地址 |
| ext | 扩展字段 | 可空 | C(1,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256 |

#### 请求报文示例

```json
{
  "data": "{\"merchUid\":\"42400\",\"orderNo\":\"\",\"merchOrderNo\":\"QC8939891749\",\"tradeDate\":\"20230914\",\"certUrl\":\"[\\\"[https://www.yangzeye.cn/tu/alipaypay04.jpg](https://www.yangzeye.cn/tu/alipaypay04.jpg)\\\",\\\"[https://newsn.net/usr/img/water/11/11e99feb454566d3.png](https://newsn.net/usr/img/water/11/11e99feb454566d3.png)\\\",\\\"[https://img95.699pic.com/photo/50626/8481.jpg_wh300.jpg](https://img95.699pic.com/photo/50626/8481.jpg_wh300.jpg)\\\",\\\"[https://img95.699pic.com/photo/50388/3904.jpg_wh300.jpg](https://img95.699pic.com/photo/50388/3904.jpg_wh300.jpg)\\\"]\",\"notifyUrl\":\"[http://127.0.0.1:8080/youaredog](http://127.0.0.1:8080/youaredog)\"}",
  "cusUid": "42400",
  "transCode": "buy.upload",
  "signMsg": "yfKjszLjXIHtpW2rBABTJDtUc/brrMZQkGFOZdR2U95Vg77vJMRMq/V8Yq2itYj+8tgLSAdhumzzGeSjS2/PrkYfuHODTWpHmZVhKNUxPrTh6ANlt2IepiXL1ySCrFLXlNd+7tqmc6/DyAdVKpzer1o0TE+NvIXsDpXLtOTGe1cHYsl08bhGNas8Xb6Z3ESFupqLqzhRsvRd9EulH64pLycVhH9KMOy6VvPnxxgwo20gLSSoDlh+6DICFMgfJPMqIvcVwAFiwBMiTpU98ulMK2w7wPdjlyXRG4TM81uKsJ9/e9Yh+aSMdF4qsfeBDNvQ7ylKcNwN8vv8NcNssDdNWQ==",
  "version": "01",
  "reqSn": "424001698770619812"
}

```

响应报文(业务参数定义)

| 节点名 | 字段名称 | 可空 | 类型 | 备注 |
| --- | --- | -- | --- | --- |
| merchUid | 商户UID | 必填 | N | 商户UID |
| merchOrderNo | 商户单号 | 必填 | C(1,64) | 商户系统唯一订单号 |
| tradeDate | 交易日期 | 必填 | C(8) | 平台下单日期，格式: yyyyMMdd |
| certId | 凭证ID | 必填 | N | 凭证唯一标识ID |


注：解析说明

1、retCode 为 000000，代表购买凭证上传成功

2、retCode 为其它情况，代表购买凭证上传失败


响应报文示例

```json
{
  "transCode": "buy.upload",
  "version": "01",
  "cusUid": "42400",
  "reqSn": "424001698771057940",
  "retCode": "000000",
  "retMsg": "请求成功",
  "data": "{\"merchUid\":42400,\"merchOrderNo\":\"QC8673288573\",\"tradeDate\":\"20231101\",\"certId\":1719396558810669057}",
  "signMsg": "XKoRL7of5N+4fsPAmaKVrKhxIl6pmkLs32aYIOWCbZUWrL/RXfxnuVmtoJQxykIWCqjao0UkE8AyBdAhAvmPhd60n+J2biVjVKFlgl84h0DPbSRqAnJ3TsFYAzWcUlr2d4vUi/cgQbBu/lXeP6AukoCdIcToNnXxohMp53YeXehPMIQbvL8RJzedgDvrjRVve0MQe0VG/rd6XpH3AX7+5GtgeDJnVhBV/mdr4sZ7exdF5W9/NSe7rFQUA4TQWa+BkACm5X4mHx+CDvUKT8nTu8SOym5uVihA6Mz9InIXeTyOU1Db3Wo7LvSFObhI/4ubU7t4YK9MNnCyC0mFaUGM7A=="
}
```
