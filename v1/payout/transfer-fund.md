---
description: 此 API 可用于使用指定的查询 ID 来转账资金。
icon: file-lines
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

# 转账

{% hint style="warning" %}
请注意，响应状态为 “true” 并不一定表示付款已成功。
{% endhint %}

<mark style="color:green;">`POST`</mark> `/api/transfer`

**Request**

<table><thead><tr><th width="116.26171875">Name</th><th width="86.8671875">Type</th><th width="86.453125" data-type="checkbox">Required</th><th>Description</th></tr></thead><tbody><tr><td>inquiry_id</td><td>string</td><td>true</td><td>要处理的查询请求的唯一标识符。</td></tr><tr><td>force</td><td>bool</td><td>false</td><td>如果设置为 true，即使账户无效也会继续进行转账。<br>默认值：false。</td></tr></tbody></table>

{% tabs %}
{% tab title="Example Payload" %}
```json
{
    "inquiry_id": "01jj1g751vqw68ng5546xnda5c",
    "force": true
}
```
{% endtab %}
{% endtabs %}

***

**Response**

| Name    | Description                       |
| ------- | --------------------------------- |
| status  | 表示操作是否成功（_true_ 为成功，_false_ 为失败）。 |
| message | 提供与错误消息相关的详细信息（如适用）。              |

{% tabs %}
{% tab title="Success" %}
```json
{
    "status": true
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
要接收回调，您必须先注册您的域名 URL。

只有在付款发放流程完成后，回调才会被触发。
{% endhint %}

_A request will be sent to the path:_

<mark style="color:green;">`POST`</mark> `{your_callback_url}`

<table><thead><tr><th width="264.64453125">Name</th><th>Description</th></tr></thead><tbody><tr><td>event</td><td>触发此 Webhook 的事件名称（例如：<code>disbursement.completed</code>，<code>payment.completed</code>）。</td></tr><tr><td>data</td><td>包含付款发放详情的数据对象。</td></tr><tr><td>data.id</td><td>查询请求的唯一标识符（Inquiry ID）。</td></tr><tr><td>data.merchant_transaction_id</td><td>商户为此付款发放提供的交易 ID。</td></tr><tr><td>data.amount</td><td>付款发放金额。</td></tr><tr><td>data.bank_code</td><td>支付处理方的银行代码（例如：014、002）。</td></tr><tr><td>data.account_name</td><td>收款账户持有者的姓名。</td></tr><tr><td>data.account_number</td><td>收款人的账户号码。</td></tr><tr><td>data.status</td><td>付款发放的当前状态(e.g., success, pending, fail).</td></tr><tr><td>data.created_at</td><td>付款发放创建的时间戳（ISO 8601 格式）。</td></tr><tr><td>data.updated_at</td><td>付款发放最后更新的时间戳（ISO 8601 格式）。</td></tr></tbody></table>

{% tabs %}
{% tab title="Example Payload" %}
```json
{
  "event": "disbursement.completed",
  "data": {
    "id": "01jmmtbfcthj8y0eg3w1he4q3t",
    "merchant_transaction_id": "RLG00000000",
    "amount": "25000.00",
    "bank_code": "002",
    "bank_name": "Bank Rakyat Indonesia",
    "account_name": "John Doe",
    "account_number": "0000000000",
    "status": "success",
    "created_at": "2025-01-10T00:54:42+07:00",
    "updated_at": "2025-01-10T00:54:48+07:00"
  }
}
```
{% endtab %}
{% endtabs %}
