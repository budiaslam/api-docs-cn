---
description: 此 API 可用于创建与付款相关的查询请求。
icon: message-dollar
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: false
  pagination:
    visible: false
  metadata:
    visible: true
---

# 查询

{% hint style="warning" %}
所有查询请求将在 30 分钟后自动失效，在此期间相应的资金将被暂时锁定。\
被锁定的余额可在 **余额记录 > 预留余额 中查看**。
{% endhint %}

<mark style="color:green;">`POST`</mark> `/api/inquiry`

**Request**

<table><thead><tr><th width="191.73046875">Name</th><th width="84.48046875">Type</th><th width="92.765625" data-type="checkbox">Required</th><th width="176.0234375">Validation</th><th width="188.2265625">Description</th></tr></thead><tbody><tr><td>merchant_transaction_id</td><td>string</td><td>true</td><td></td><td>客户为商户提供的唯一标识符</td></tr><tr><td>bank_code</td><td>string</td><td>true</td><td></td><td>与该交易关联的银行代码。</td></tr><tr><td>amount</td><td>number</td><td>true</td><td>数字类型，1–12 位</td><td>以数字格式表示的交易金额</td></tr><tr><td>account_number</td><td>string</td><td>true</td><td><p>数字类型，1–12 位</p><p><br></p></td><td>客户用于该交易的银行账号。</td></tr><tr><td>customer_name</td><td>string</td><td>true</td><td>长度在 5–25 个字符之间，允许字符：a–z、A–Z、0–9、-（短横线）、（空格）</td><td>与本次交易相关的客户姓名</td></tr></tbody></table>

{% tabs %}
{% tab title="Example Payload" %}
```json
{
    "merchant_transaction_id": "RP123456789",
    "amount": 100000,
    "bank_code": "014",
    "account_number": "0123456789",
    "customer_name": "John Doe"
}
```
{% endtab %}
{% endtabs %}

***

**Response**

<table><thead><tr><th width="231.0546875">Name</th><th>Description</th></tr></thead><tbody><tr><td>status</td><td>表示请求是否成功。如果成功则返回 <em>true</em>，否则返回 <em>false</em>。</td></tr><tr><td>data</td><td>包含与交易相关的详细信息。</td></tr><tr><td>data.merchant_transaction_id</td><td>客户为商户指定的唯一识别码。</td></tr><tr><td>data.account_number</td><td>与交易关联的银行账号。</td></tr><tr><td>data.account_name</td><td>账户持有者的姓名（如果不可用，则可能为 null）。</td></tr><tr><td>data.bank_code</td><td>与交易相关的银行代码。</td></tr><tr><td>data.bank_name</td><td>处理该交易的银行名称。</td></tr><tr><td>data.amount</td><td>以数字格式表示的交易金额。</td></tr><tr><td>data.fee</td><td>应用的交易手续费。</td></tr><tr><td>data.inquiry_id</td><td>交易查询的唯一标识符。</td></tr><tr><td>message</td><td>包含错误消息的相关信息。</td></tr></tbody></table>

{% tabs %}
{% tab title="Success" %}
```json
{
    "status": true,
    "data": {
        "merchant_transaction_id": "RP123333333",
        "account_number": "0123456789",
        "account_name": "John Doe",
        "bank_code": "014",
        "bank_name": "PT. BANK CENTRAL ASIA, TBK.",
        "amount": 100000,
        "fee": 5500,
        "inquiry_id": "01jj1g751vqw68ng5546xnda5c"
    }
}
```
{% endtab %}

{% tab title="Failed" %}
```json
{
    "status":false,
    "data": null,
    "message":string,
}
```
{% endtab %}
{% endtabs %}
