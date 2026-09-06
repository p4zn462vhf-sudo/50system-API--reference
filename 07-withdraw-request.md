# 商户提币下单(withdraw.request)

**应用场景**：钱包用户在商户系统创建的提币订单，向平台发起商户提币下单请求，审核通过后对应币将从商户的账户提取到钱包用户账户中。

***

### 1. 请求报文

#### 业务参数定义

| 节点名             | 字段名称     | 可空  | 类型        | 备注                              |
| --------------- | -------- | --- | --------- | ------------------------------- |
| `merchUid`      | 商户UID    | 必填  | N         | 商户UID                           |
| `merchOrderNo`  | 商户单号     | 必填  | C (1,64)  | 商户系统唯一订单号                       |
| `currency`      | 交易币种（代币） | 必填  | C (1,16)  | 参考交易币种字典                        |
| `orderAmt`      | 下单数量     | 必填  | N         | 下单数量                            |
| `userNo`        | 用户编号     | 必填  | C (1,32)  | 发起支付的用户唯一标识                     |
| `userIp`        | 用户IP地址   | 必填  | C (1,64)  | 用户发起支付时的IP地址                    |
| `userName`      | 用户真实姓名   | 必填  | C (1,32)  | 用户付款账户的真实姓名                     |
| `userLevel`     | 用户等级     | 非必填 | N         | 用户等级，如：0、1、2、3、4、5、6、7、8、9等级    |
| `recAddress`    | 提币地址     | 可空  | C (0,128) | 提币地址，获取路径为：进入钱包链接 > 我的 > 提币地址   |
| `notifyUrl`     | 交易结果通知地址 | 必填  | C (1,128) | 交易结果终态时根据该地址通知商户系统              |
| `extrasContent` | 扩展内容     | 可空  | C (0,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256 |

#### 请求报文示例

```json
{
  "data": "{\"merchUid\":\"42400\",\"merchOrderNo\":\"MB1862504941\",\"currency\":\"CNY\",\"orderAmt\":\"5000\",\"userNo\":\"9527\",\"userIp\":\"用户真实IP\",\"userName\":\"张三\",\"userLevel\":0,\"recAddress\":\"424001000372XCDDFFF80559CDBC51\",\"notifyUrl\":\"127.0.0.1\"}",
  "cusUid": "42400",
  "transCode": "withdraw.request",
  "signMsg": "TWIPO+M23GMvLUd4xQY12AEn9puxq1hOe7mtJoy1N0YkQEaVbFsXiSD6MCQZfijS6xHTBH9jEIQtsY9i7tpzK57e0PRJKJZCXM41FnjQILoWwrA3LaJg/OCQEa510/dQKJcmSnxXIIWD1v+2stnHTwICXUq4moNHBYjTc97t/EzVjuv+n5Lb2uxxXBh8764CfnadqsHBNT3u84u5QFO9X8S4NrInHRtb+JTekBLsUAkpuveRVrwwWkP2DO1/4ILQRjP69b1XnhWp/2x9GZNAl/1ddKhMVLZVkxnZHivLKMloFbRsdhotcFFdqpFtmYrUmomq7vjyQvMBnIOGhpkwyQ==",
  "version": "01",
  "reqSn": "424001698775903612"
}
```

***

### 2. 响应报文

#### 业务参数定义

| 节点名             | 字段名称   | 可空 | 类型        | 备注                                                      |
| --------------- | ------ | -- | --------- | ------------------------------------------------------- |
| `merchUid`      | 商户UID  | 必填 | N         | 商户UID                                                   |
| `merchOrderNo`  | 商户单号   | 必填 | C(1,64)   | 商户系统唯一订单号                                               |
| `userUid`       | 用户UID  | 必填 | N         | 用户UID                                                   |
| `orderNo`       | 平台单号   | 必填 | C(19)     | 平台唯一订单号                                                 |
| `tradeDate`     | 交易日期   | 必填 | C(8)      | 平台下单日期，格式:yyyyMMdd                                      |
| `orderAmt`      | 下单数量   | 必填 | N         | 下单数量                                                    |
| `recAddress`    | 提币地址   | 必填 | C (0,128) | 提币地址                                                    |
| `status`        | 下单状态   | 必填 | N         | <p><code>10</code>: 下单成功<br><code>-100</code>: 下单失败</p> |
| `remark`        | 业务返回描述 | 必填 | C(1,64)   | 业务返回描述                                                  |
| `extrasContent` | 扩展内容   | 可空 | C (0,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256                         |

> **解析说明**：
>
> 1. `retCode` 为 `000000`，并且 `data` 中的 `status` 为 `10`，代表提币下单受理成功（不代表交易成功）。
> 2. `retCode` 为 `000000`，并且 `data` 中的 `status` 为 `-100`，代表提币下单失败（代表交易失败）。
> 3. `retCode` 为其它情况，请以“商户提币订单查询”中的结果来判定交易是否成功。

#### 响应报文示例

```json
{
  "cusUid": "42400",
  "data": "{\"merchUid\":42400,\"merchOrderNo\":\"MB2025958404\",\"userUid\":1004157,\"orderNo\":\"2023110102145782317\",\"tradeDate\":\"20231101\",\"orderAmt\":5000,\"recAddress\":\"424001000372XCDDFFF80559CDBC51\",\"status\":10,\"remark\":\"请求成功\"}",
  "reqSn": "424001698776095079",
  "retCode": "000000",
  "retMsg": "请求成功",
  "signMsg": "kj3NUvpEoFpWhi6VyNEWVVLVb69LpCHv7OwpbsXUEO0Icw2jOwi3LJUFeSG7KtMze6bi/UaHh0b/h2s3Em1UT+5HdV6++wGWaAE6AtF3Sx7ylp9i6GNeoAz4WTv3JmF/E9dD9ezCQiZGfDgnUEyAclKD8UEq0egYKTKGtEwGb7intqLJM7xf25pr3z/3ugoTd+kcBueeoIDmjbxfWIliExqsJG6tT3JA2vDx820ImC1kim86go7dqWRIoqg89Zcns7HyF9NlDTR7cMWXXWtAiFIj1EwWDf44AVRWvNFxRp0ZsXdD40ZZ7pV2GneaVz+UG2YbuNRD+K3mSYfvX+xyWw==",
  "transCode": "withdraw.request",
  "version": "01"
}
```
