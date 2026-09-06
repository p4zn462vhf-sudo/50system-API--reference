# 充币订单通知（deposits.notify）

**应用场景**：当充币订单状态发生变更（如交易成功、失败或取消）时，平台向商户提交订单下单时传入的 `notifyUrl` 发送异步结果通知。

***

### 1. 通知报文

#### 业务参数定义

| 节点名             | 字段名称     | 可空 | 类型        | 备注                                                                                 |
| --------------- | -------- | -- | --------- | ---------------------------------------------------------------------------------- |
| `merchUid`      | 商户UID    | 必填 | N         | 商户UID                                                                              |
| `merchOrderNo`  | 商户单号     | 必填 | C (1,64)  | 商户系统唯一订单号                                                                          |
| `currency`      | 交易币种（代币） | 必填 | C (1,16)  | 参考交易币种字典                                                                           |
| `orderNo`       | 平台单号     | 必填 | C (19)    | 平台唯一订单号                                                                            |
| `tradeDate`     | 交易日期     | 必填 | C (8)     | 平台下单日期，格式:yyyyMMdd                                                                 |
| `userNo`        | 用户编号     | 必填 | C (1,32)  | 发起支付的用户唯一标识                                                                        |
| `userName`      | 用户真实姓名   | 必填 | C (1,32)  | 用户付款账户的真实姓名                                                                        |
| `payAmt`        | 实际交易数量   | 必填 | N         | 实际交易数量                                                                             |
| `status`        | 订单状态     | 必填 | N         | <p><code>100</code>: 交易成功<br><code>-100</code>: 交易失败<br><code>-90</code>: 订单取消</p> |
| `payTime`       | 交易完成时间   | 必填 | C (19)    | 交易成功时返回，格式：`yyyy-MM-dd HH:mm:ss`                                                   |
| `extrasContent` | 扩展内容     | 可空 | C (0,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256                                                    |
| `remark`        | 业务返回描述   | 可空 | C (0,64)  | 业务返回描述                                                                             |

> **解析说明**： 以 `data` 中的 `status` 作为判定交易是否成功的最终依据（`status = 100` 代表交易成功）。

#### 通知报文示例

```json
{
  "cusUid": "54453",
  "data": "{\"merchUid\":54453,\"merchOrderNo\":\"MB5580987391\",\"currency\":\"CNY\",\"orderNo\":\"2023102812361082032\",\"tradeDate\":\"20231028\",\"userNo\":\"BUY014\",\"userName\":\"张三\",\"payAmt\":\"100\",\"status\":100,\"payTime\":\"2023-10-28 12:36:10\",\"remark\":\"交易成功\"}",
  "reqSn": "1698467906732",
  "signMsg": "nb5HuEzPuuBMYKEs9smPMznJ1uBYkLiVC+Jlhrh4EWVy65RKRr+x2e/UEbaMDpaRWFcZgS/i74JwcjkFdsfsCfkpmKb7w4YnIoACk1EqaZOdW0zdatAEwFPz4RE+ScQNp4Tu+22VHUF2F4mxhhPm1Oh6r8hvHSKJenmdEGHXAhDNFfqfFI+5MHp/oyYAt1blsr/QNaZQRyfQc/XAiONGpwvA8c5H0xLyQFdoE9BCIatIsI73qZQ8Sucq23jty7GVEQp2b2O9MuW3OQLkefSuO9bKp8/bPH5xs459hFBUAObQYAVKxTEEaNdhuBl4bxNb8gqUxaael97nqbLS1fRdfg==",
  "transCode": "deposits.notify",
  "version": "1.0"
}
```
