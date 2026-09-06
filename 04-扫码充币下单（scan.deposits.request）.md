
商户扫码充币下单(scan.deposits.request)

应用场景：钱包用户在商户系统创建的扫码充币订单，向平台发起商户扫码充币下单请求，平台受理返回扫码充币链接，该接口适用于轻度对接的商户。

请求报文(业务参数定义)

节点名

字段名称

可空

类型

备注

merchUid

商户UID

必填

N

商户UID

merchOrderNo

商户单号

必填

C (1,64)

商户系统唯一订单号

currency

交易币种（代币）

必填

C (1,16)

参考交易币种字典

orderAmt

下单数量

必填

N

下单数量

userNo

用户编号

必填

C (1,32)

发起支付的用户唯一标识

userIp

用户IP地址

必填

C (1,64)

用户发起支付时的IP地址

userName

用户真实姓名

必填

C (1,32)

用户付款账户的真实姓名

userLevel

用户等级

可空

N

用户等级，如：0、1、2、3、4、5、6、7、8、9等级，不传默认为98

notifyUrl

交易结果通知地址

必填

C (1,128)

交易结果终态时根据该地址通知商户系统

请求报文示例

{ 

"data":"{"merchUid":48351, "userLevel":99,"userNo":"1120","merchOrderNo":"4813805310","userIp":"127.0.0.1","notifyUrl":"http://127.0.0.1","currency":"TTT","orderAmt":54.47,"userName":"1120"}", 

"cusUid":"48351", 

"transCode":"scan.deposits.request", 

"signMsg":"HauzduENQiV7WVotNpGTGjrGN3m4bx/oYDFgXpTvEqYY4IfgTs4LgulvyeqPHG2ccEGyJFZKSZ80TbupfWSfQ/5NasQ+BhEAlGgf4SiYCepxSuFqiU60Dz7bpSTPtFB3STe/iQNogsV0alsfGpPUtCCSPuRun9plB5bk8MZgm0g+c1iXOcdEH6xd4A+yo4VkkFrGnVz3P1zfmg8XPQCsuOVQgz1VbcHVHdjEoh+jal8/Be6ZXQcH6QLSKbgLZKZ76wV9CQ/FNIsYlqRWujRfGQqL2WpVdoPqrFznyr3ERlEdhLUkV6HrWquyoR/dW50L5FWTcKDnVGhMj5OduGaq7A==", 

"version":"01", 

"reqSn":"483511700644932447" 

}

响应报文(业务参数定义)

节点名

字段名称

可空

类型

备注

merchUid

商户UID

必填

N

商户UID

merchOrderNo

商户单号

必填

C(1,64)

商户系统唯一订单号

userNo

用户编号

必填

C (1,32)

发起支付的用户唯一标识

userName

用户真实姓名

必填

C (1,32)

用户付款账户的真实姓名

orderAmt

下单数量

必填

N

下单数量

url

确认充币链接

必填

C(1,1024)

确认充币链接

remark

业务返回描述

必填

C(1,64)

业务返回描述

注:  解析说明
1、retCode 为000000，代表商户扫码充币下单受理成功

2、retCode 为其它情况，代表商户扫码充币下单失败

响应报文示例

{ 

"transCode": "scan.deposits.request", 

"version": "01", 

"cusUid": "48351", 

"reqSn": "483511728958665037", 

"retCode": "000000", 

"retMsg": "请求成功", 

"data": "{"merchOrderNo":"7984755206","merchUid":48351,"orderAmt":63.18,"remark":"请求成功","url":"http://127.0.0.1:19010/home?param=eyJuYW1lIjoi5Zyw5ZyoIiwib3JkZXJBbXQiOiI2My4xOCIsInR5cGUiOiIzMSIsImtleSI6IjNjOTFiNjIyOWE4NDRhMDgyYTM4NmFlNDYxYzBiOGE2In0=","userName":"地在","userNo":"20241010"}", 

"signMsg": "eH/M3vkQz4z9E71yWcQQPhPdgSlsSTfgkvdXLLF5K4boBelLArDRbJO126relXZ28cHLYSzJ40nz1VWwuLE6nvSNwYpLBVvDewrb3oik+4g4E+EKYD71L34JURV8hEY4l6d7X7t/EUSXWvympn9Qrwxd8fSEgiPl8gBChT06kRVabsK4yqa1WAe2A/VB5J57cyTjxsgNDZH0k5ZquK8o1t8vlvGYVjkcbjANGbn2cIJGLukVxt/9nDMJItgX28aiBdyVyUSIrLamg3uWYMXXosjg1TFe99r5ui4Sjrr6dn2YzEXPiArKJEn6DVf3pf0Ga0zW5krTQYQYHI0LixYctw==" 

}

