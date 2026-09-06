# 商户充币查询 (deposits.query)

**应用场景**：商户系统向平台发起对商户充币查询，平台返回当前商户充币下单的交易状态。

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
  "data": "{\"merchUid\":48351,\"merchOrderNo\":\"3972002317\",\"tradeDate\":\"20231121\"}",
  "cusUid": "48351",
  "transCode": "deposits.query",
  "signMsg": "oUYuivXudSFFjQQwB5XkZH6FOJcDHONodZ47u8jLSWBrPYh5VDE/xx96nFX3wFauNjKzHt/U4hjLDW3hHYn8QnLT66P6fLzuRH+fZqRtubO236UrbksSPB8Xm3SVFXf7fBvjOUIEnkYsdOmsUehLUZEZWinYi3sy9b1ecodBTGYldW4OiKvjgOtkoMAI/WKay02vx3ZP0tSQAwk7J+ScIT2zUUrcfm76b1e64nPjnzT3TqwwPZM9CUO7Ihf+azscgHyhf3rfix1ctVkR5W4xog7zd/7t+zc8vtpu5fJZAuM1yPeQggh9Zm7HzULQUeto34iV7KYuIF6yFK1GFOzFqA==",
  "version": "01",
  "reqSn": "483511700644801603"
}
```

***

### 2. 响应报文

#### 业务参数定义

| 节点名             | 字段名称   | 可空 | 类型        | 备注                              |
| --------------- | ------ | -- | --------- | ------------------------------- |
| `merchUid`      | 商户UID  | 必填 | N         | 商户UID                           |
| `merchOrderNo`  | 商户单号   | 必填 | C(1,64)   | 商户系统唯一订单号                       |
| `orderNo`       | 平台单号   | 必填 | C(19)     | 平台唯一订单号                         |
| `tradeDate`     | 交易日期   | 必填 | C(8)      | 平台下单日期，格式:yyyyMMdd              |
| `userNo`        | 用户编号   | 必填 | C (1,32)  | 发起支付的用户唯一标识                     |
| `userName`      | 用户真实姓名 | 必填 | C (1,32)  | 用户付款账户的真实姓名                     |
| `orderAmt`      | 下单数量   | 必填 | N         | 下单数量                            |
| `payAmt`        | 实际交易数量 | 可空 | N         | 实际交易数量，交易成功时返回                  |
| `status`        | 订单状态   | 必填 | N         | 10:处理中; 100:交易成功；-100:交易失败      |
| `payTime`       | 交易完成时间 | 可空 | C(0,21)   | 交易成功时返回                         |
| `extrasContent` | 扩展内容   | 可空 | C (0,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256 |
| `remark`        | 业务返回描述 | 必填 | C(1,64)   | 业务返回描述                          |

> **解析说明**：
>
> 1. `retCode` 为 `000000`，代表商户充币查询成功，以 `data` 中 `status` 为判定交易是否成功（`status = 100`，代表商户充币交易成功）。
> 2. `retCode` 为 `000010`，代表未查询到订单信息，可以判定交易失败。
> 3. `retCode` 为其它情况，代表充币查询失败，请重新查询。

#### 响应报文示例

```json
{
  "transCode": "deposits.query",
  "version": "01",
  "cusUid": "48351",
  "reqSn": "483511700644932460",
  "retCode": "000000",
  "retMsg": "请求成功",
  "data": "{\"extrasContent\":\"{\\\"a\\\":1111,\\\"b\\\":2222}\",\"merchOrderNo\":\"2582887329\",\"merchUid\":48351,\"orderAmt\":182.37,\"orderNo\":\"2023112217215290368\",\"payAmt\":0,\"remark\":\"请求成功\",\"status\":10,\"tradeDate\":\"20231122\",\"userName\":\"gsd1120\",\"userNo\":\"gsd1120\"}",
  "signMsg": "aDOtZRtZeib33GjOEOn4GfGJ6NOJCIIe4+7XqEaTRKLjhFv/UObdgCvNLNpHrRVW3S3+PBcAU1uSa8fwxAj0/gBhiFTyXbrdMi9y4zfXTX5fwYYSpx/p9Z2e/Cwf2YfuqFFD+oqFcoDv1ujdHO/neIBfk2NiZp327pvxd23layryNUz97/grYH18qzV+M+npRLDrRxwfNrdWIC/sadFDtT7az8fpvZq8XWTpRLygDN6XHDMeWvOe3q3NnzXVl7nbyjVZNH3bNGlvjYtYpXl8lFukICZnJQeun3g5QpVfAHrw6JGz8NnBiRBFapHsdqAnuZTU2ATALX/QqXg7GHOCnw=="
}
```
