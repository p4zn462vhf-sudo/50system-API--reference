# 商户账户查询 (acct.query)

**应用场景**：商户系统向平台发起商户的账户查询，平台返回被查询商户的所有账户余额数据。

***

### 请求报文（业务参数定义）

| 节点名      | 字段名称      | 可空 | 类型      | 备注    |
| -------- | --------- | -- | ------- | ----- |
| merchUid | 商户UID     | 必填 | N       | 商户UID |
| currency | 币种（法币、代币） | 必填 | C(1,16) | 币种    |

#### 请求报文示例

```json
{
  "data": "{\"merchUid\":\"42400\",\"currency\":\"CNY\"}",
  "cusUid": "42400",
  "transCode": "acct.query",
  "signMsg": "qA2kK+YGu6iHiWCWlWxy9f4XwbYTPGLWfKN7ktVqcjCoLYaL2DJqXjLRKZburvWcRpKmt9zeMEc4KakGf1+t9TORtoDsVmwjqsOToQyeMs3nX/zlPor+dOMmZjx8OsjhTD6P17x6biwFOR6//AigfPDTa/RZHLRFqAj0CFp7hBIYAiN41Ezp/RlPcFY1BPwP2AlV/mGxnsJWxd235SdjZ+U5pkDMzusNUEDRwHzg/TR1YCadCukAQl2ojco2hPQs9VdUQt48XELmJ1XL+5DEeofXdjjA94FDH8Dr/I39KEAxvkSvR/r3uQvrkq03MLzy9dLYqGWzqiGZ6m8lBMRjBw==",
  "version": "01",
  "reqSn": "424001698772391558"
}

```

响应报文(业务参数定义)

| 节点名           | 字段名称      | 可空 | 类型      | 备注                       |
| ------------- | --------- | -- | ------- | ------------------------ |
| merchUid      | 商户UID     | 必填 | N       | 商户UID                    |
| currency      | 币种（法币、代币） | 必填 | C(1,16) | 币种                       |
| availableAmt  | 可用余额      | 必填 | N       | 可用余额                     |
| frozenAmt     | 冻结余额      | 必填 | N       | 冻结余额                     |
| riskFrozenAmt | 风险冻结余额    | 必填 | N       | 风险冻结余额                   |
| updateTime    | 查询时间      | 必填 | C(19)   | 查询时间，格式: yyyy-MM-dd HHss |

注：解析说明

retCode 为 000000，代表账户查询成功。 \
retCode 为其它情况，代表账户查询失败。<br>

响应报文示

```json
{
  "cusUid": "42400",
  "data": "{\"merchUid\":42400,\"currency\":\"CNY\",\"availableAmt\":-111959.43,\"frozenAmt\":160520.57,\"riskFrozenAmt\":0.0,\"updateTime\":\"2023-11-01 01:13:12\"}",
  "reqSn": "424001698772391558",
  "retCode": "000000",
  "retMsg": "请求成功",
  "signMsg": "Di1rcfkWayK1vnMNZ+t3PBEMJbcpf5KhleYYVrYzws3ytkLJvUoZsIdXWZAkl6BWX5WrPHPOpXK9ouzsamWZsVoKYXfYAeFahSXQ6EHhg2wwBmlq0DmqRC6knfhiDv/qJlOQNzzHfasqVbe1v9v0IqBviDpi2bxGlPsjdgTJ0l5PmWXpGDRLtdimlbN8BXgPKnLbU8RL0e4QUlE/MNeY2A46xuABWOvGOG+zIx+IwQI1Tpae45k9r9eGWTux5QCWVRTtdLCiGlnblJwmxynL3OYqFEVVtfGLcKSdCfxjQTWvDwKJz+lZnJVNLP9hKl5Pwqp2PCb9kb3JqJT1l14XYg==",
  "transCode": "acct.query",
  "version": "01"
}
```
