# 接口接入说明

## 1.1 接口接入技术

接口接入技术使用标准的 https 协议（单校验）通讯和具有安全性的 ssl 加密传输，建立信息安全通道，来保证数据传输的安全。业务数据按照接入规范中定义的格式进行传输，编码格式为 UTF-8，请重点关注接入规范部分的 JSON 报文格式说明。

## 1.2 签名算法

采用签名验证的方式对传输报文进行安全处理。

### 1.2.1 签名

根据所有报文的请求体或者响应报文体的参数,按照参数名称进行自然排序(根据参数名称中的首字母进行 A-Za-z 的自然排序，如果首字母一样，则按照第二个字母进行排序，以此类推)，所得的排序集合按照标准的 URI Query String 格式以 & 符号链接在一起(注意：参数为空串的不需要加入链接, 请求参数非 Java 语言中的八种基本数据类型也不需要加入链接)生成了明文串。

签名工具类：

```java
import java.util.Map;
import cn.hutool.core.bean.BeanUtil;
import cn.hutool.core.codec.Base64;
import cn.hutool.core.map.MapUtil;
import cn.hutool.crypto.SecureUtil;
import cn.hutool.crypto.asymmetric.Sign;
import cn.hutool.crypto.asymmetric.SignAlgorithm;
import lombok.extern.slf4j.Slf4j;

/**
 * <p>Description: 签名工具类</p>
 * <p>Copyright: Copyright (c)2021</p>
 * <P>Created Date: 2021年09月16日</P>
 * @version 1.0
 */
@Slf4j
public class SignUtil {
   
   private SignUtil() {}
   
   private static final String SIGN_KEY = "signMsg";
   private static final String EXCLUDEPROPERTY = "serialVersionUID";
   private static final String CONNECT = "&";
   private static final String CONNECT_EQUAL = "=";
   
   /**
    * 根据对象生成签名
    * @param obj
    * @param privateKey
    * @return
    */
   public static String genSign(Object obj, String privateKey) {
      Sign sign = SecureUtil.sign(SignAlgorithm.SHA256withRSA, privateKey, null);
      return Base64.encode(sign.sign(genSignStr(obj)));
   }
   
   /**
    * 根据待签名字符串生成签名
    * @param signStr
    * @param privateKey
    * @return
    */
   public static String genSign(String signStr, String privateKey) {
      Sign sign = SecureUtil.sign(SignAlgorithm.SHA256withRSA, privateKey, null);
      return Base64.encode(sign.sign(signStr));
   }
   
   /**
    * 生成签名字符串
    * @param obj
    * @return
    */
   @SuppressWarnings("unchecked")
   public static String genSignStr(Object obj) {
      Map<String, Object> map;
      if (obj instanceof Map) {
         map = (Map<String, Object>) obj;
      } else {
         map = BeanUtil.beanToMap(obj, Boolean.FALSE, Boolean.TRUE);
      }
      map.remove(SIGN_KEY);
      map.remove(EXCLUDEPROPERTY);
      String signStr = MapUtil.sortJoin(map, CONNECT, CONNECT_EQUAL, Boolean.TRUE);
      log.info("生成签名字符串：{}",signStr);
      return signStr;
   }
   
   /**
    * 验证签名
    * @param obj
    * @param signMsg
    * @param publicKey
    * @return
    */
   public static boolean verifySign(Object obj, String signMsg, String publicKey) {
      Sign sign = SecureUtil.sign(SignAlgorithm.SHA256withRSA, null, publicKey);
      return sign.verify(genSignStr(obj).getBytes(), Base64.decode(signMsg));
   }
   
}

//【1】报文内容
String data = "{\"userName\":\"陆开花\",\"userNo\":\"1229008\"}";

//【2】请求报文封装
BaseMessageDTO baseMessageDTO = new BaseMessageDTO();
baseMessageDTO.setTransCode("MB1001");
baseMessageDTO.setVersion("1.0");
baseMessageDTO.setCusUid("123321");
baseMessageDTO.setReqSn("12332189223424324");
baseMessageDTO.setData(data);

//【3】调用进行签名
String jsonStr = SignUtil.genSignStr(baseMessageDTO);
String signMsg = SignUtil.genSign(jsonStr,"商户私钥");
baseMessageDTO.setSignMsg(signMsg);

// 请求报文结构：
// {"data":"","cusUid":"","transCode":"","retCode":"","signMsg":"","retMsg":"","version":"","reqSn":""}

//【4】接口返回验证签名
// 1）例如返回报文如下：
String result = "{\"cusUid\":\"10117\",\"data\":\"{\\\"extrasContent\\\":{},\\\"merchOrderNo\\\":\\\"20211234567890104018\\\",\\\"merchUid\\\":10117,\\\"orderAmt\\\":300000,\\\"payType\\\":\\\"AliPay2\\\",\\\"payUrl\\\":\\\"[https://www.baidu.com/?tn=62095104_19_oem_dg](https://www.baidu.com/?tn=62095104_19_oem_dg)\\\",\\\"recAcctName\\\":\\\"收款人姓名\\\",\\\"recAcctNo\\\":\\\"2453730156653278\\\",\\\"recAcctType\\\":10,\\\"recBankCode\\\":\\\"CISO\\\",\\\"recBankName\\\":\\\"汇丰银行\\\",\\\"recSubBankName\\\":\\\"汇丰银行华盛顿中环大道支行\\\",\\\"returnCode\\\":\\\"000000\\\",\\\"remark\\\":\\\"请求成功\\\",\\\"status\\\":10,\\\"orderNo\\\":\\\"2022010422361700024\\\",\\\"tradeDate\\\":\\\"20220104\\\"}\",\"reqSn\":\"7302273727707065375575\",\"retCode\":\"000000\",\"retMsg\":\"OK\",\"signMsg\":\"QvZ8OmTGlgb6JKplReW2PH5w+ddt3DhOPz9zyJnefRErKdaWiCaAc4ILB11a9F+TPwtYUH2X3Z1UJADJYRUc7NxCfAZ2AciPODs8DNTwq/sp16YxRkeNu4xImVqns6JWKpbBMaNDoKfWbILwoWxNoYOTcY1q2UEVAJr0obSLgaW84TAm7natPjfjrzvKK5jFfiHTiD28XA5Tq38PFbgBjhjEF9Gs+y3btcHW+7VmkellGlaZ4qdys2P7O290pvRv1SUwOaABBfcHXhoHd52ct/iDg6ICNHMtE2ukyF7/pSPvOlqxLCv+w+pdEU360w831mYBhxJo7xfo9VbjDgLE5g==\",\"transCode\":\"MB1001\",\"version\":\"1.0\"}";

BaseMessageDTO retDto = JSONObject.parseObject(result, BaseMessageDTO.class);
String signMsg = dto.getSignMsg();
dto.setSignMsg("");
if(!verifySign(retDto, signMsg, "平台公钥")){
     log.info("签名验证不通过");
}
```
2. 接口接入规范
2.1 报文结构
为了方便阅读及理解所有接口传输报文均为JSON结构；报文分为报文公共部分与报文内容两部分；报文头决定业务走向与接口版本等公共元素；报文体则为各个报文定制化参数；

2.2 详细接口接入规范
报文不要格式化。

所有异步通知接口均有支付推送系统完成，推送系统负责保证推送必达效果，所以需要有握手动作，故约定约定如下：

客户方正常处理完通知后必须返回“success”(不包含引号且小写)

如果客户方处理通知且没有返回“success”,则服务器会在24小时内不断按时间间隔递增重复通知，重试时间间隔例如：2m,10m,10m,1h

在解析响应报文时，应先判断响应报文头是否含有错误信息，如有则不会带有报体信息。

2.3 请求地址
接口采用统一网关报文解析，通过报文编码解析不同流程处理。

注：商户购买查询、商户出售查询、商户账户查询使用查询类请求地址，其它接口使用API类请求地址

查询类请求地址：访问域名+/merch/query

API类请求地址：访问域名+/merch/api

2.4 通知地址
1） 交易下单时上送notify_url;交易完成后，将会根据此地址进行结果通知

### 请求报文定义

| 节点名 | 字段名称 | 类型 | 可空 | 取值 |
| :--- | :--- | :--- | :--- | :--- |
| transCode | 交易代码 | 必填 | C(1,20) | 参考交易编码枚举3.3.1 |
| cusUid | 商户UID | 必填 | C(1,20) | 对接商户端为商户UID |
| version | 接口版本 | 必填 | C(2) | 1.0 |
| reqSn | 交易请求号 | 必填 | C(1,36) | 数据格式：商户UID+不重复序号 |
| data | 请求内容 | 必填 | C(2048) | JSON串（参考具体接口请求参数） |
| signMsg | 签名串 | 必填 | C(32) | SHA256WithRSA签名算法对json串进行签名 |

示例：
{
    "cusUid":"10117",
    "data":"{\"allianceUid\":223,\"currency\":\"CNY\",\"merchOrderNo\":\"2021123456789010317\",\"merchUid\":10117,\"notifyUrl\":\"[http://104.168.215.114/mock-web/merchNotify/doMS3001](http://104.168.215.114/mock-web/merchNotify/doMS3001)\",\"orderAmt\":10000,\"payType\":\"BankTransfer2\",\"recAcctName\":\"人间富贵花\",\"recAcctNo\":\"65473847563728374\",\"recAcctType\":\"10\",\"recBankCode\":\"ICBC\",\"recBankName\":\"中国工商银行\",\"recSubBankName\":\"柳州分行\",\"userIp\":\"172.32.56.8\",\"userLevel\":2,\"userName\":\"陆开花\",\"userNo\":\"1229008\"}",
    "reqSn":"8386332444758488080545",
    "signMsg":"ouSmfo8bUyxblN+M7T0s6jgduw9a2o+YIcST3zVirr9sCHUQqGFKLcgYT2TtWjMWKcBT75pCYkkek3ackUWgIz/Kt4A8o4dzzgQhY3hJI5+oZ0FUtrgrcOgK45b0z3HJtRgB4boUCbcBFCdOF2RxaKrFS+G/zBS96w0cb3KKtFVU2KPsdpxkP9tF10F/HYnsk0TQvfp3TYOMGTMMtUTxA9OHEXnulYRPa57sJlX6VigOi2QziqbM/x3fg2i+/VgR71J1+rEUX9+p4rRJqrfViqPxMRPTcCvTDsNwc5dAWlzFvbsBr0W3NYk9Q7kekGlGZjdes0uNbznUG7TDVUBMTA==",
    "transCode":"MS1001",
    "version":"1.0"
}

### 返回报文定义

| 节点名 | 字段名称 | 类型 | 可空 | 取值 |
| :--- | :--- | :--- | :--- | :--- |
| transCode | 交易代码 | 必填 | C(1,20) | 按请求时填写 |
| cusUid | 商户UID | 必填 | C(1,20) | 按请求时填写 |
| version | 接口版本 | 必填 | C(2) | 按请求时填写 |
| reqSn | 交易请求号 | 必填 | C(1,36) | 按请求时填写 |
| data | 请求内容 | 必填 | C(2048) | JSON串（参考具体接口返回参数） |
| signMsg | 签名串 | 必填 | C(32) | SHA256WithRSA签名算法对json串进行签名 |
| retCode | 接口请求应答码 | 可空 | C(6) | 接口响应时由服务端填写000000为请求成功，其他均为请求失败 |
| retMsg | 接口请求应答描述 | 可空 | C(0,128) | 接口响应时由服务端填写 |

示例：
{
    "cusUid":"10117",
    "data":"{\"extrasContent\":{},\"merchOrderNo\":\"20211234567890104018\",\"merchUid\":10117,\"orderAmt\":300000,\"payType\":\"AliPay2\",\"payUrl\":\"[https://www.baidu.com/?tn=62095104_19_oem_dg](https://www.baidu.com/?tn=62095104_19_oem_dg)\",\"recAcctName\":\"收款人姓名\",\"recAcctNo\":\"2453730156653278\",\"recAcctType\":10,\"recBankCode\":\"CISO\",\"recBankName\":\"汇丰银行\",\"recSubBankName\":\"汇丰银行华盛顿中环大道支行\" ,\"remark\":\"请求成功\",\"status\":10,\"orderNo\":\"2022010422361700024\",\"tradeDate\":\"20220104\"}",
    "reqSn":"7302273727707065375575",
    "retCode":"000000",
    "retMsg":"OK",
    "signMsg":"QvZ8OmTGlgb6JKplReW2PH5w+ddt3DhOPz9zyJnefRErKdaWiCaAc4ILB11a9F+TPwtYUH2X3Z1UJADJYRUc7NxCfAZ2AciPODs8DNTwq/sp16YxRkeNu4xImVqns6JWKpbBMaNDoKfWbILwoWxNoYOTcY1q2UEVAJr0obSLgaW84TAm7natPjfjrzvKK5jFfiHTiD28XA5Tq38PFbgBjhjEF9Gs+y3btcHW+7VmkellGlaZ4qdys2P7O290pvRv1SUwOaABBfcHXhoHd52ct/iDg6ICNHMtE2ukyF7/pSPvOlqxLCv+w+pdEU360w831mYBhxJo7xfo9VbjDgLE5g==",
    "transCode":"MB1001",
    "version":"1.0"
}

### 通知报文定义

| 节点名 | 字段名称 | 类型 | 可空 | 取值 |
| :--- | :--- | :--- | :--- | :--- |
| transCode | 交易代码 | 必填 | C(1,20) | 参考交易编码枚举3.3.1 |
| cusUid | 商户UID | 必填 | C(1,20) | 按请求时填写 |
| version | 接口版本 | 必填 | C(2) | 1.0 |
| reqSn | 交易请求号 | 必填 | C(1,36) | 数据格式：商户UID+不重复序号 |
| data | 请求内容 | 必填 | C(2048) | JSON串（参考具体接口请求参数） |
| signMsg | 签名串 | 必填 | C(32) | SHA256WithRSA签名算法对json串进行签名 |

示例：
{
    "cusUid":"10117",
    "data":"{\"extrasContent\":\"{}\",\"merchOrderNo\":\"5869193249\",\"merchUid\":65863,\"orderAmt\":21.78,\"orderNo\":\"2023072218473800374\",\"payAmt\":21.78,\"payTime\":\"2023-07-22 18:47:39\",\"payType\":\"BankTransfer\",\"remark\":\"交易成功\" ,\"status\":100}",
    "version":"1.0",
    "reqSn":"1690022860549",
    "transCode":"buy.notify",
    "signMsg":"ypsqgaOUIue3SIgGIYW43SlH+QG/KuCM5iY3nbsWbxQcN0czMywGfzCDQncZSM00mlmtCfGQEarb3CajFdXMqrdZ/m0uiFPpZiWxGhqO/UupG0yb+GdjoMlqfdBt1xnPirgM4vrDc92JORaJpoAijZjYanr420KvCsUnIlageCGDjoVyuWDPpJILhu+2lyE3qfhKx3X2kJRGSOKb10sJ5kgV4l6bSbo/AxXV+CqO3p4f1XDspysfVohPyo+esG97L/QXCogH2J3arkzGnVcjJfWxHf15FbNS5dF4jMJnk8/KT73Z9UGw4BHpiWJhn1+Exp7sbUqwNNjI3CM401nm+g=="
}
