# 商户站内信通知(wap.sell.confirmNotify)

**应用场景**：平台向商户发送站内信通知。商户收到通知后，由商户系统向用户展示站内信消息，并引导用户进入钱包，在钱包系统中确认出售挂单是否到账。

***

### 1. 通知报文

#### 业务参数定义

| 节点名           | 字段名称   | 可空 | 类型       | 备注                              |
| ------------- | ------ | -- | -------- | ------------------------------- |
| `merchUid`    | 商户UID  | 必填 | N        | 商户UID                           |
| `advertNo`    | 挂单单号   | 必填 | C (19)   | 挂单单号                            |
| `userNo`      | 用户编号   | 必填 | C (1,32) | 用户唯一标识                          |
| `confirmTime` | 确认截止时间 | 必填 | C (19)   | 确认截止时间，格式：`yyyy-MM-dd HH:mm:ss` |
| `msg`         | 站内信消息  | 必填 | C (64)   | 站内信消息                           |

> **解析说明**： 商户收到站内信通知后，由商户系统向用户展示站内信消息，并引导用户进入钱包，在钱包系统中确认出售挂单是否到账。

#### 通知报文示例

```json
{
  "cusUid": "19438",
  "data": "{\"confirmTime\":\"2024-04-15 12:10:22\",\"merchOrderNo\":\"2024041510483000952\",\"merchUid\":19438,\"msg\":\"DDB钱包有一笔到账，请您确认查收\",\"userNo\":\"sell002\"}",
  "reqSn": "1713153922478",
  "signMsg": "0LkLJ4faThh3JD5O6lA9RGI0o0AOuR0GouxLW00tiA/SnG7wHeL3W6GfPs1Ra7HXXaEZtyxD2FjhVjJvky8C4N+93+LmLzZeZT2eWXmuuIBLdoh6cQ6bGh6Rd/5K3a4XOtsOT5tJ7AyV4xtLCZy1RIh76NsQOjfhQ631zmLjmmGA096svUGGKsOINNlZcIyK8UJlPTombhEicvdsS/JDuzub+vQwNAxUPuMOigaHHQ3u+gE3GArP8AKsEOp1ny4lY2vOYCMjrEWVkQh7ocA8aCuP0VlQ6Ql8m8nyudkGGtrYm1AL1YIk1TCFNfzmWrJhWGHsVtbRmHwShf4XS1f6jw==",
  "transCode": "wap.sell.confirmNotify",
  "version": "1.0"
}
```
