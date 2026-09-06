# 出售结果通知 (sell.notify)

**应用场景**：出售订单产生终态（交易成功或交易失败）时，平台系统主动将交易结果通过回调地址异步通知给商户系统

***

### 通知报文（业务参数定义）

| 节点名 | 字段名称 | 可空 | 类型 | 备注 |
| --- | --- | -- | --- | --- |
| merchUid | 商户UID | 必填 | N | 商户UID |
| merchOrderNo | 商户单号 | 必填 | C(1,64) | 商户系统唯一订单号 |
| orderNo | 平台单号 | 必填 | C(1,64) | 平台唯一订单号 |
| tradeDate | 交易日期 | 必填 | C(8) | 平台下单日期，格式: yyyyMMdd |
| orderAmt | 下单金额 | 必填 | N | 订单创建金额 |
| payAmt | 实际支付金额 | 必填 | N | 实际支付金额 |
| status | 业务状态码 | 必填 | N | 100: 交易成功；-100: 交易失败 |
| payType | 支付方式 | 必填 | C(1,32) | 支付渠道类型，如：OsBank |
| recAcctNo | 收款人账号 | 必填 | C(1,64) | 收款人银行账号(银行卡、支付宝、云闪付、数字人民币时，必填) |
| recAcctName | 收款人姓名 | 必填 | C(1,32) | 收款人账户姓名 |
| recBankName | 收款银行名称 | 可空 | C(1,64) | 收款银行名称 |
| recBankCode | 收款银行编码 | 可空 | C(1,32) | 收款银行机构代码 |
| payTime | 支付完成时间 | 可空 | C(19) | 交易成功时返回 格式yyyy-MM-dd HH:mm:ss |
| remark | 业务返回描述 | 必填 | C(1,128) | 交易结果描述（如“交易失败”） |
| ext | 扩展字段 | 可空 | C(1,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256 |

注：解析说明

1、以data中status为判定交易是否成功（status = 100,代表交易成功）

2、商户收到通知并校验签名无误后，需按照通用响应规范返回应答

通知报文示例

```json
{
  "data": "{\"merchUid\":11619,\"merchOrderNo\":\"MB3224351972\",\"orderNo\":\"2023103015323282209\",\"tradeDate\":\"20231030\",\"orderAmt\":1224,\"payAmt\":0,\"status\":-100,\"payType\":\"OsBank\",\"recAcctNo\":\"356840416373168010\",\"recAcctName\":\"鲁智深\",\"recBankName\":\"啵啵啵银行\",\"recBankCode\":\"GGBONG\",\"payTime\":\"2023-10-30 15:38:02\",\"remark\":\"交易失败\"}",
  "version": "1.0",
  "reqSn": "1698651482428",
  "cusUid": "11619",
  "transCode": "sell.notify",
  "signMsg": "br5UJ5uj4DSe9WH0HxcGbi3OSBUBsF77eN5QMyDmKPsYZBNdTcg2kGp0PY61EvEanS5iqZL2L1GPb841HeiGjFEHudSDwatTJFIuiioWUJAR+5MX/weP6YKEZ0GVdkTHvlfonQHlQlH1RE70VD5DKQsSlwJrMgnoWkLvjoFpfPeaakxt7bikDTBgDUU6MIgGqEKORgZ/qijPBeGrgqbPFIwZ0iaG/otJt0NmZBfN18PcA+9pGoTZ5qFntCK7bQXOR7Nws4Ark8Xhi9BVuvREk/OE8yqdxAWu+tsvmWyffEcKAy9wm4NtON1PG4MUf6ZeVkV7sp/Z1F5N2wgLgNbziw=="
}
```

