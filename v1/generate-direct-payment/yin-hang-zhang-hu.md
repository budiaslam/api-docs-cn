---
description: 此 API 允许商户生成用于交易处理的结账页面
icon: building-columns
---

# 银行账户

`POST` `/api/create/bank`

**Request**

| Name                      |   Type | Required | Validation                                  | Description       |
| ------------------------- | -----: | :------: | ------------------------------------------- | ----------------- |
| merchant\_transaction\_id | string |   true   |                                             | 客户为商户提供的唯一标识符     |
| amount                    | string |   true   | 数字类型，1–12 位                                 | 交易的总金额。           |
| currency                  | string |   true   | **可接受的数值:** `"IDR"`                         | 交易所使用的货币（仅限印尼卢比）。 |
| bank\_code                | string |   true   | **可接受的数值**: BCA, MDR, BNI, BRI              | 交易所使用的银行代码        |
| customer\_name            | string |   true   | 长度在 5–25 个字符之间，允许字符：a–z、A–Z、0–9、-（短横线）、（空格） | 与本次交易相关的客户姓名      |
| product\_name             | string |   true   | 长度在 5–25 个字符之间，允许字符：a–z、A–Z、0–9、-（短横线）、（空格） | 与本次交易相关的产品名称      |
| description               | string |   false  |                                             | 与本次交易相关的额外信息      |

{% tabs %}
{% tab title="Example Payload" %}
```json
{
    "merchant_transaction_id": "ICZ10000001",
    "amount": "100000",
    "currency": "IDR",
    "bank_code": "BCA",
    "description": "Lorem Ipsum",
    "customer_name": "John Doe",
    "product_name": "Product Test"
}
```
{% endtab %}
{% endtabs %}

***

**Response**

| Name                 | Description                              |
| -------------------- | ---------------------------------------- |
| status               | 表示请求的状态。成功时为 true，失败时为 false             |
| message              | 描述请求结果的消息。通常在请求成功时为 “success”            |
| data.account\_number | 生成的银行账号。客户将使用此账号进行付款。                    |
| data.account\_name   | 账户持有者的姓名。                                |
| data.amount          | 用户需要汇款的总金额                               |
| data.channel         | 与银行账户支付相关的银行或支付渠道。                       |
| data.redirect\_url   | 用户将被重定向到的链接（如适用）。如果未提供重定向链接，此字段可以为 null。 |

{% tabs %}
{% tab title="Success" %}
```json
{
    "status": true,
    "message": "success",
    "data": {
        "account_number": "4871002066",
        "account_name": "John Doe",
        "amount": 100089,
        "channel": "BANK_BCA",
        "redirect_url": "{host}/checkout/9e536777-2bfa-4161-9931-35f5c4b23faf/bank/BCA"
    }
}
```
{% endtab %}

{% tab title="Failed" %}
```json
{
    "status": false,
    "message": string
}
```
{% endtab %}
{% endtabs %}

***

**Callback**

{% hint style="warning" %}
要接收回调，您必须先注册您的域名网址。\
只有在用户完成支付后，回调才会被触发。
{% endhint %}

`POST` `{your_callback_url}`

| Name                          | Description                  |
| ----------------------------- | ---------------------------- |
| transaction\_id               | 交易的唯一标识符                     |
| merchant\_transaction\_id     | 客户为商户提供的唯一标识符                |
| created\_at                   | 交易创建的时间戳                     |
| amount                        | 交易的总金额                       |
| currency                      | 交易所使用的货币（例如：IDR、USD）         |
| channel                       | 使用的支付渠道 (e.g., BCA, QRIS).   |
| service\_fee\_rate            | 适用的服务费百分比 (e.g., 2.5, 3.0).  |
| service\_fee\_amount          | 扣除的服务费总额 (e.g., 2000, 4000). |
| nett\_amount                  | 扣除服务费后的最终到账金额                |
| rrn                           | 关联支付交易的 RRN（检索参考号）           |
| customer\_info                | 客户的附加详情                      |
| customer\_info.customer\_name | 客户姓名                         |
| customer\_info.product\_name  | 与交易关联的产品名称                   |

{% tabs %}
{% tab title="Example Payload" %}
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
  "rrn": null,
  "customer_info": {
       "customer_name": "John Doe",
       "product_name": "Deposit Now"
  }
}
```
{% endtab %}
{% endtabs %}
