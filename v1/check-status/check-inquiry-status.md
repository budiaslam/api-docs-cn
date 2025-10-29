---
description: 用于查看或验证付款查询的当前状态。
icon: hourglass-clock
---

# 检查查询状态

<mark style="color:green;">`POST`</mark> `/api/check/inquiry`

**Request**

<table><thead><tr><th width="212.08984375">Name</th><th width="76.2421875">Type</th><th width="88.7265625" data-type="checkbox">Required</th><th width="363.32421875">Description</th></tr></thead><tbody><tr><td>merchant_transaction_id</td><td>string</td><td>true</td><td>分配给商户交易的唯一标识符。</td></tr></tbody></table>

{% tabs %}
{% tab title="Example Payload" %}
```json
{
  "merchant_transaction_id": "ICZ10000001"
}
```
{% endtab %}
{% endtabs %}

***

**Response**

<table><thead><tr><th width="259.9453125">Name</th><th>Description</th></tr></thead><tbody><tr><td>status</td><td>表示请求的状态。成功时为 true，失败时为 false。</td></tr><tr><td>data.status</td><td>交易的当前状态 (e.g., <mark style="color:green;">Success</mark>, <mark style="color:yellow;">Pending</mark>, <mark style="color:purple;">Processing</mark>, <mark style="color:red;">Fail</mark>)</td></tr><tr><td>data.merchant_transaction_id</td><td>用于标识商户交易的唯一识别码</td></tr><tr><td>data.amount</td><td>交易金额的数值</td></tr><tr><td>data.bank_code</td><td>标识与该交易关联银行的代码。</td></tr><tr><td>data.account_number</td><td>与交易关联的银行账号。</td></tr><tr><td>data.reason</td><td>说明交易失败或状态更新原因的信息（可为 null）。</td></tr><tr><td>data.service_fee_final</td><td>应用的交易手续费。</td></tr></tbody></table>

{% tabs %}
{% tab title="Success" %}
```json
{
    "status": true,
    "data": {
        "status": "success",
        "created_at": "2025-01-01T00:00:00.000000Z",
        "updated_at": "2025-01-01T12:00:00.000000Z",
        "merchant_transaction_id": "INQ12300456",
        "amount": "100000.00",
        "bank_code": "009",
        "account_number": "00000000",
        "reason": "",
        "service_fee_final": "5500.00"
    }
}
```
{% endtab %}

{% tab title="Failed" %}
```json
{
    "status": false,
    "data": null
    "message": string
}
```
{% endtab %}
{% endtabs %}
