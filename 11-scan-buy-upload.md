# 商户代币空投查询 (airdrop.query)

**应用场景**：商户系统向平台发起对商户代币空投交易查询，平台返回当前商户代币空投订单的交易状态。

***

### 1. 请求报文

#### 业务参数定义

| 节点名            | 字段名称  | 可空 | 类型      | 备注                                        |
| -------------- | ----- | -- | ------- | ----------------------------------------- |
| `merchUid`     | 商户UID | 必填 | N       | 商户UID                                     |
| `merchOrderNo` | 商户单号  | 可空 | C(0,64) | 与 `orderNo` 二选一                           |
| `orderNo`      | 平台单号  | 可空 | C(0,19) | 与 `merchOrderNo` 二选一                      |
| `tradeDate`    | 交易日期  | 可空 | C(0,8)  | 下单时返回的交易日期，格式:yyyyMMdd，只使用商户单号查询时，交易日期为必填 |

#### 请求报文示例

```json
{
  "data": "{\"merchUid\":48351,\"orderNo\":\"2023120423350195017\",\"tradeDate\":\"20231204\"}",
  "cusUid": "48351",
  "transCode": "airdrop.query",
  "signMsg": "k3N0mfdOjKmgefcZKauRkMow4nmcb2f0Zg+dubqiv4XFja3HFuSzkUlFb0Hs8v1vWMOmaWY9Y2OXa12mxtBU42cdE6t//+MoJkSRnN9LxCt6IKqb6iD1F05awGqWVOLTMMsLmeTTyXrfhr0VUp0wbXahJcwkLVYCw/ovMJ9ZwThH8OmQqeVv/WUNF/WAV7rZKaN2CWaEkzMoeOTl69vCJfLE2lo+t8tfUP2o83EKjnz8rGpGcS0BXLHU7ayx/zpbpNxJ/g20EFW7MWP2vGT0rJvtodwWnuchmZUafurK9gLxJwM2K/v8um+VXpTl7VK6mmnsWiaxLMycU0bb3k8Z6g==",
  "version": "01",
  "reqSn": "483511715782498083"
}
```

***

### 2. 响应报文

#### 业务参数定义

| 节点名             | 字段名称   | 可空 | 类型        | 备注                                                                               |
| --------------- | ------ | -- | --------- | -------------------------------------------------------------------------------- |
| `merchUid`      | 商户UID  | 必填 | N         | 商户UID                                                                            |
| `merchOrderNo`  | 商户单号   | 必填 | C(1,64)   | 商户系统唯一订单号                                                                        |
| `orderNo`       | 平台单号   | 必填 | C(19)     | 平台唯一订单号                                                                          |
| `tradeDate`     | 交易日期   | 必填 | C(8)      | 平台下单日期，格式:yyyyMMdd                                                               |
| `userNo`        | 用户编号   | 必填 | C (1,32)  | 用户唯一标识                                                                           |
| `userName`      | 用户真实姓名 | 必填 | C (1,32)  | 用户真实姓名                                                                           |
| `orderAmt`      | 代币空投数量 | 必填 | N         | 代币空投数量                                                                           |
| `status`        | 订单状态   | 必填 | N         | <p><code>10</code>: 处理中<br><code>100</code>: 交易成功<br><code>-100</code>: 交易失败</p> |
| `payTime`       | 空投完成时间 | 可空 | C(0,21)   | 交易成功时返回                                                                          |
| `extrasContent` | 扩展内容   | 可空 | C (0,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256                                                  |
| `remark`        | 业务返回描述 | 必填 | C(1,64)   | 业务返回描述                                                                           |

> **解析说明**：
>
> 1. `retCode` 为 `000000`，代表代币空投查询成功，以 `data` 中 `status` 为判定空投交易是否成功（`status = 100` 代表空投交易成功）。
> 2. `retCode` 为 `000010`，代表未查询到空投订单信息，可以判定交易失败。
> 3. `retCode` 为其它情况，代表空投订单查询失败，请重新查询。

#### 响应报文示例

```json
{
  "transCode": "airdrop.query",
  "version": "01",
  "cusUid": "48351",
  "reqSn": "483511715782657962",
  "retCode": "000000",
  "retMsg": "请求成功",
  "data": "{\"extrasContent\":\"{\\\"a\\\":1111,\\\"b\\\":2222}\",\"merchOrderNo\":\"MB9016145871\",\"merchUid\":48351,\"orderAmt\":226.1,\"orderNo\":\"2024051522145803073\",\"payTime\":\"2024-05-15 22:14:58\",\"remark\":\"交易成功\",\"status\":100,\"tradeDate\":\"20240515\",\"userName\":\"地在\",\"userNo\":\"xxxx0408\"}",
  "signMsg": "UGU1cOtKXBlyqXibEXrh7FsIt1J2GFHyAdxQecfxbzOeDyLl/HGDHoSRE/nMyzcEL3gfllF6wYePUeCdMnSPJRyHW79t9cnaYtJ3O33RknaI2bLtzCAE/A/pi+QFWDzO+ZlKorUnsxEqJWQ0fWWjtUk/Lgm+NoZNvY6G54ICAYjhgFnyIijpz18e231XNqNFmhwmOMY+2sdU0dsmHngI0MkUJhx1n2FleltXqfjWOp10tFfPxsScQIDk7GyIye5qwQozfgObe3XrhKLukmMqyGGyaT+gSLICAVSTNQGh9vFxT056JpWHomb702csAG5dRTTKr2WRtc5x3g21x2IsdA=="
}
```
