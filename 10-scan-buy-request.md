# 商户扫码购买下单 (scan.buy.request)

**应用场景**：商户系统已创建的扫码购买订单，向平台发起扫码购买下单的请求，由平台撮合商户的扫码购买订单

***

### 请求报文（业务参数定义）

| 节点名 | 字段名称 | 可空 | 类型 | 备注 |
| --- | --- | -- | --- | --- |
| merchUid | 商户UID | 必填 | N | 商户UID |
| merchOrderNo | 商户单号 | 必填 | C(1,64) | 商户系统唯一订单号 |
| currency | 交易币种 | 必填 | C(1,16) | 交易币种 |
| payType | 支付方式 | 必填 | C(1,32) | 支付方式 |
| orderAmt | 下单金额 | 必填 | N | 下单金额 |
| userNo | 用户编号 | 必填 | C(1,32) | 发起支付的用户唯一标识 |
| userIp | 用户IP地址 | 必填 | C(1,64) | 用户真实IP |
| userName | 用户真实姓名 | 必填 | C(1,32) | 用户真实姓名 |
| userLevel | 用户等级 | 可空 | N | 用户等级,如：0、1、2、3、4、5、6、7、8、9等级 |
| sourceType | 终端来源 | 必填 | C(1,16) | 10: Android 20: IOS 30: PC 40: H5 |
| notifyUrl | 交易结果通知地址 | 必填 | C(1,128) | 交易结果通知地址 |
| acctNoHistory | 历史账号后缀 | 可空 | C(1,128) | 历史账号后缀(后4位)，多个后缀以英文逗号拼接，例如：1111,2222,3333 |
| ext | 扩展字段 | 可空 | C(1,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256 |

#### 请求报文示例

```json
{
  "data":"{\"merchUid\": \"48351\",\"merchOrderNo\": \"MB5975096332\",\"currency\": \"CNY\",\"payType\": \"OsAlipay\",\"orderAmt\": \"350.21\",\"userNo\": \"xiangcai001\",\"userIp\": \"用户真实IP\",\"userName\": \"张三\",\"userLevel\": \"5\",\"sourceType\": \"20\",\"notifyUrl\": \"[http://127.0.0.1:8080/youarepig](http://127.0.0.1:8080/youarepig)\",\"acctNoHistory\": \"1122,2233,3344,4455\"}",
  "cusUid": "48351",
  "transCode": "scan.buy.request",
  "signMsg":"ym+j9ScT8ITairvmTjwyVVrGyTrwzH2RN64F35ddc7/Xfqc/4XhFD8sTLNRh/+0ef6h3hdzMlHXcZ5L1eBmU+IkIOkj14JM8LGmZ+zJhLdhTAyrutNDHUfA5df8RsZL/NW+8bGXIFG1K8cDUEkD8O7UPoTH/tm8o8rLlman0iwo7E8pOvwzqQEYBOUxfyWiKpsnWywGlfgYkxIW2cY3aEydGY7yw9jRHRdMRvZEQrMG2R6QzwTkb2x/An3u+qKVHRvKxO7JTc82rbbDdojEwPoHCkHBwwiSBFR521+EtqV2WzjHmt3N5tofGR/1HroFpbCjY4QAMbs0Jxs8XlgqOWQ==",
  "version": "01",
  "reqSn": "424001698768455182"
}

```

响应报文(业务参数定义)

| 节点名 | 字段名称 | 可空 | 类型 | 备注 |
| --- | --- | -- | --- | --- |
| merchUid | 商户UID | 必填 | N | 商户UID |
| merchOrderNo | 商户单号 | 必填 | C(1,64) | 商户系统唯一订单号 |
| orderAmt | 下单金额 | 必填 | N | 下单金额 |
| orderNo | 平台单号 | 可空 | C(1,64) | 平台唯一订单号； 只有转代客下单成功时，平台单号为空，其他情况必返回平台单号 |
| payType | 支付方式 | 必填 | C(1,32) | 支付方式 |
| payUrl | 支付链接 | 可空 | C(1,1024) | 支付链接 |
| tradeDate | 平台下单日期 | 必填 | C(8) | 平台下单日期，格式:yyyyMMdd，订单结果查询时需要上送该时间 |
| status | 业务状态码 | 必填 | N | 10:下单成功； -100:下单失败； 20:转代客下单 |
| remark | 业务返回描述 | 必填 | C(1,64) | 业务返回描述 |
| ext | 扩展字段 | 可空 | C(1,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256 |


注：解析说明

1、retCode 为000000，并且data中的status为10，代表购买下单受理成功(不代表交易成功)

2、retCode 为 000000，并且data中的status为-100，代表购买下单失败(代表交易失败)

3、retCode 为其它情况，代表购买下单失败


响应报文示例

```json
{
  "transCode": "scan.buy.request",
  "version": "01",
  "cusUid": "48351",
  "reqSn": "483511705853189027",
  "retCode": "000000",
  "retMsg": "请求成功",
  "data": "{\"merchOrderNo\":\"MB5975096332\",\"merchUid\":48351,\"orderAmt\":350.21,\"orderNo\":\"2024012200062999310\",\"payType\":\" OsAlipay\",\"payUrl\":\"[http://127.0.0.1:19010/home?param=eyJuYW1lIjoi6LWb5bCU5Y+3Iiwib3JkZXJBbXQiOiIzNTAuMjEiLCJrZXkiOiJlMzZkNzM3NDg1ZjJjYzFkYmI1YmU4ZjA5NzkxNGYzMCJ9](http://127.0.0.1:19010/home?param=eyJuYW1lIjoi6LWb5bCU5Y+3Iiwib3JkZXJBbXQiOiIzNTAuMjEiLCJrZXkiOiJlMzZkNzM3NDg1ZjJjYzFkYmI1YmU4ZjA5NzkxNGYzMCJ9)\",\"remark\":\"请求成功\",\"status\":10,\"tradeDate\":\"20240122\"}",
  "signMsg": "UVc+wkYPGkf11gRu/EEeKALjR4RahLl5nDj71Qq9iBOwmw8OmiBXukcrAKKZ6XC5Okz4+7PMCOa77bo0nEf4MOVYzIQXDwom+15XFogUnqfh46Y+bo2vGcK5CrQT14Ld2xjMR0PYVUTZoH2CniblsheZYKVTTdC49zyI7b7Q7Vd593b3pkzm/1+B66mLkOCHcr6goER1CYvDnImaTf29ucAqJi5vhjClq2oArqqsC3V5HR1D/EatL6rA4xe8tenas+i/wwUQb/mRyVZzblKyTCrr1lnZ9A0QrXC9fjoJiWU/QXFscp9Q1RGBJeHdRtlo3JXtfWsKCG5290JNodpH6Q=="
}
```
