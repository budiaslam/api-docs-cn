---
icon: anchor-circle-check
---

# 付款完成 Webhook

{% hint style="warning" %}
要接收回调，您必须先注册您的域名网址。\
只有在用户完成支付后，回调才会被触发。
{% endhint %}

<mark style="color:green;">POST</mark> `payment.completed`

负载结构

| Name                      | Type   | Description              |
| ------------------------- | ------ | ------------------------ |
| transaction\_id           | string | 交易的唯一标识符。                |
| merchant\_transaction\_id | string | 客户为商户提供的唯一标识符。           |
| created\_at               | string | 付款发放创建的时间戳（ISO 8601 格式）。 |
| amount                    | number | 交易的总金额                   |
| currency                  | string | 交易所使用的货币（仅限印尼卢比）。        |
| channel                   | string | 本次交易所使用的支付渠道             |
| service\_fee\_rate        | string | 本次交易所适用的服务费比例            |
| service\_fee\_amount      | string | **服务费金额**                |
| nett\_amount              | number | 净额                       |
| rrn                       | string | 关联支付交易的 RRN（检索参考号）       |
| customer\_info            | object | 客户的附加详情                  |

**Example Payload**

{% tabs %}
{% tab title="Success" %}
```json
{
  "transaction_id": "9dc1614d-ed70-426c-80da-5304ad258c9c",
  "merchant_transaction_id": "RLP0000000000",
  "created_at": "2024-12-18T16:21:07.000000Z",
  "amount": "10000.00",
  "currency": "IDR",
  "channel": "BCA",
  "service_fee_rate": 0,
  "service_fee_amount": 0,
  "nett_amount": 10000,
  "rrn": "0392813042",
  "customer_info": {
       "customer_name": "John Doe",
       "product_name": "Deposit Now"
  }
}
```
{% endtab %}
{% endtabs %}
