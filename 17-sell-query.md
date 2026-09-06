# 商户出售查询 (sell.query)

**应用场景**：商户系统向平台发起出售订单交易查询，平台返回当前出售订单的交易状态

***

### 请求报文（业务参数定义）

| 节点名 | 字段名称 | 可空 | 类型 | 备注 |
| --- | --- | -- | --- | --- |
| merchUid | 商户UID | 必填 | N | 商户UID |
| merchOrderNo | 商户单号 | 可空 | C(1,64) | 商户系统唯一订单号 |
| orderNo | 平台单号 | 可空 | C(1,64) | 平台唯一订单号 |
| tradeDate | 交易日期 | 条件选填 | C(8) | 下单时返回的交易日期，格式:yyyyMMdd，只使用商户单号查询时，交易日期为必填 |

#### 请求报文示例

```json
{
  "data": "{\"merchUid\":\"42400\",\"merchOrderNo\":\"MB3405103774\",\"tradeDate\":\"20231101\"}",
  "cusUid": "42400",
  "transCode": "sell.query",
  "signMsg": "HMRfaWorVruVe3W4Q06ggbRmmKf0ozj12NvewlN0Pvk982vBQW0mc2cxzb7dmcYBDrCn+Pg9GpqikqTj3Bn0ISSlMBuHxGS8Raocwy/YzmxGFDNAl8P4rpllVqvTvCiUpVK2NhqPikSwJ774ef9c9/4tQegnM9a9G66RdqZ09nU43NXhHLKRtrLLYKQHvCl1IuO36RGhdP6d0qSli7aDw7jtp5aMef2ipF8v0iWEMwfvi0Lq0QdXvT3+9c5HFuXR6A7+OG+TR98W0mMk95PKR8wuRavHT+DSENt9SAjK/KjmbDzG+PvIew5U1dTijzjquQ4bsq/ak0yBSj6QrmX3Tg==",
  "version": "01",
  "reqSn": "424001698772044972"
}
```

响应报文(业务参数定义)

| 节点名 | 字段名称 | 可空 | 类型 | 备注 |
| --- | --- | -- | --- | --- |
| merchUid | 商户UID | 必填 | N | 商户UID |
| merchOrderNo | 商户单号 | 必填 | C(1,64) | 商户系统唯一订单号 |
| orderNo | 平台单号 | 必填 | C(1,64) | 平台唯一订单号 |
| tradeDate | 交易日期 | 必填 | C(8) | 平台下单日期，格式: yyyyMMdd |
| orderAmt | 下单金额 | 必填 | N | 订单创建金额 |
| payAmt | 实际支付金额 | 必填 | N | 实际支付金额 |
| status | 业务状态码 | 必填 | N | 10:处理中; 100:交易成功；-100:交易失败 |
| payType | 支付方式 | 必填 | C(1,32) | 支付渠道类型，如：OsBank |
| recAcctNo | 收款人账号 | 必填 | C(1,64) | 收款人银行账号(银行卡、支付宝、云闪付、数字人民币时，必填) |
| recAcctName | 收款人姓名 | 必填 | C(1,32) | 收款人账户姓名 |
| recBankName | 收款银行名称 | 可空 | C(1,64) | 收款银行名称 |
| recBankCode | 收款银行编码 | 可空 | C(1,32) | 收款银行机构代码 |
| remark | 业务返回描述 | 必填 | C(1,64) | 业务返回描述 |
| extrasContent | 扩展字段 | 可空 | C(1,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256 |

注：解析说明

1、retCode 为000000，代表出售查询成功，以data中status为判定交易是否成功（status = 100,代表交易成功）

2、retCode 为000010，代表未查询到订单信息，可以判定交易失败

3、retCode 为其它情况，代表出售查询失败，请重新查询

响应报文示例
```json
{
  "cusUid": "42400",
  "data": "{\"merchUid\":42400,\"merchOrderNo\":\"MB3405103774\",\"orderNo\":\"2023110101021382315\",\"tradeDate\":\"20231101\",\"orderAmt\":960,\"payAmt\":0,\"status\":10,\"payType\":\"OsBank\",\"recAcctNo\":\"6225889852422142345\",\"recAcctName\":\"哈哈哈\",\"recBankName\":\"哪都通银行\",\"recBankCode\":\"ZSYY\",\"remark\":\"请求成功\"}",
  "reqSn": "424001698772044972",
  "retCode": "000000",
  "retMsg": "请求成功",
  "signMsg": "ScalNUdsjBTX7Xc9M8u5nCC7ZkQ9OXAZus3YG1kRJECNR7W+cC9e+UnHmE1bK0Tnl7MybmhEE4yVkO5FCIhWYRcMrBO4AKbljUfvp/+8664x8+SviUH3v2hZ7srGLsSYjxr0j0xTfJDqxTGVGTqg08zxuyGtH3nGNgvnHyZwp+FXqNDB/vU+++JEO5is+OsZhVwjWLMjzGs08uxL4VjzbfNAJzBKRBjGIh54ORYLkK9BtTpoNi+crhyztfvUVKAZ4dkViU2iGnPDIvl78wepROQNlsrT0sr/n2GsHLmyBnkmYREHjp2vHSkgsLSstRkjYwDI4y/fgPMtfhokPpIr+w==",
  "transCode": "sell.query",
  "version": "01"
}
```
