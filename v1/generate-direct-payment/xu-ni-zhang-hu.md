---
description: 通过此 API，您可以生成用于银行转账支付的结账页面。
icon: vr-cardboard
---

# 虚拟账户

为特定交易或客户生成的临时唯一银行账号，用于简化付款流程。

`POST` `/api/create/va`

## Request

<table><thead><tr><th>Name</th><th width="83.38671875" align="right">Type</th><th width="91.625" data-type="checkbox">Required</th><th>Validation</th><th>Description</th></tr></thead><tbody><tr><td>merchant_transaction_id</td><td align="right">string</td><td>true</td><td></td><td>商户提供的交易唯一标识符</td></tr><tr><td>amount</td><td align="right">string</td><td>true</td><td>数字类型，1–12 位</td><td>交易金额的数值</td></tr><tr><td>currency</td><td align="right">string</td><td>true</td><td><p><strong>可接受的数值：</strong></p><p><code>"IDR"</code></p></td><td>交易所使用的货币，仅支持印尼卢比（IDR）</td></tr><tr><td>bank_code</td><td align="right">string</td><td>true</td><td><strong>可接受的数值：</strong>PRMT, CIMB, MDR, BNI, BRI</td><td>交易所使用的银行代码。</td></tr><tr><td>customer_name</td><td align="right">string</td><td>true</td><td>长度在 5–25 个字符之间，允许字符：a–z、A–Z、0–9、-（短横线）、（空格）</td><td>与本次交易相关的客户姓名</td></tr><tr><td>product_name</td><td align="right">string</td><td>true</td><td>长度在 5–25 个字符之间，允许字符：a–z、A–Z、0–9、-（短横线）、（空格）</td><td>与本次交易相关的产品名称</td></tr><tr><td>description</td><td align="right">string</td><td>false</td><td></td><td>与本次交易相关的额外信息</td></tr></tbody></table>

{% tabs %}
{% tab title="Example Payload" %}
```json
{
    "merchant_transaction_id": "ICZ10000001",
    "amount": "100000",
    "currency": "IDR",
    "bank_code": "BRI",
    "description": "Lorem Ipsum",
    "customer_name": "John Doe",
    "product_name": "Product Test"
}
```
{% endtab %}
{% endtabs %}

***

## Response

| Name               | Description                                     |
| ------------------ | ----------------------------------------------- |
| status             | 表示请求的状态。成功时为 true，失败时为 false。                   |
| message            | 描述请求结果的消息。通常在请求成功时为 “success”。                  |
| data.va\_number    | 生成的虚拟账户（VA）号码，客户将使用此号码完成付款。如果未生成虚拟账户号码，此字段可能为空。 |
| data.channel       | 与虚拟账户关联的银行或支付渠道。                                |
| data.redirect\_url | 用户将被重定向到的链接（如适用）。如果未提供重定向链接，此字段可以为 null。        |

{% tabs %}
{% tab title="Success" %}
```json
{
    "status": true,
    "message": "success",
    "data": {
        "va_number": "8999817535253527",
        "channel": "MDR",
        "redirect_url": "{host}/checkout/9e536777-2bfa-4161-9931-35f5c4b23faf/va/MDR"
    }
}
```
{% endtab %}

{% tab title="Failed" %}
```json
{
    "status": false,
    "message": "string"
}
```
{% endtab %}
{% endtabs %}

***

## Callback

{% hint style="warning" %}
要接收回调，您必须先注册您的域名网址。\
只有在用户完成支付后，回调才会被触发。
{% endhint %}

`POST` `{your_callback_url}`

| Name                          | Description                        |
| ----------------------------- | ---------------------------------- |
| transaction\_id               | 交易的唯一识别码。                          |
| merchant\_transaction\_id     | 客户为商户指定的唯一识别码。                     |
| created\_at                   | 交易创建时的时间戳。                         |
| amount                        | 交易的总金额。                            |
| currency                      | 交易所使用的货币（例如：IDR、USD）               |
| channel                       | 使用的支付渠道 (e.g., BCA, QRIS).         |
| service\_fee\_rate            | 适用的服务费百分比 (e.g., 2.5, 3.0).        |
| service\_fee\_amount          | 本次交易所扣除的服务费总金额 (e.g., 2000, 4000). |
| nett\_amount                  | 本次交易在扣除服务费后实际到账的最终金额               |
| rrn                           | 关联支付交易的 RRN（检索参考号）                 |
| customer\_info                | 客户的附加详情                            |
| customer\_info.customer\_name | 本次交易所提供的客户姓名                       |
| customer\_info.product\_name  | 与本次交易相关的产品名称                       |

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
