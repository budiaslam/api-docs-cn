---
description: 此 API 可用于检查交易状态。如果提供了 RRN 参数，系统将在搜索过程中优先使用该参数。
icon: hourglass-clock
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

# 检查交易状态

<mark style="color:green;">`POST`</mark> `/api/check/transaction`

**Request**

<table><thead><tr><th width="212.08984375">Name</th><th width="69.63671875">Type</th><th width="94.1953125" data-type="checkbox">Required</th><th width="373.05078125">Description</th></tr></thead><tbody><tr><td>merchant_transaction_id</td><td>string</td><td>true</td><td>商户交易的唯一标识符</td></tr><tr><td>rrn</td><td>string</td><td>false</td><td>与支付交易相关的 RRN（检索参考号）。</td></tr></tbody></table>

{% tabs %}
{% tab title="Example Payload" %}
```json
{
  "merchant_transaction_id": "ICZ10000001",
  "rrn": "015221245517"
}
```
{% endtab %}
{% endtabs %}

***

**Response**

<table><thead><tr><th width="259.9453125">Name</th><th>Description</th></tr></thead><tbody><tr><td>status</td><td>表示请求是否成功。如果成功则返回 <em>true</em>，否则返回 <em>false</em>。</td></tr><tr><td>data.status</td><td>交易的当前状态 (e.g., <mark style="color:green;">Success</mark>, <mark style="color:yellow;">Pending</mark>, <mark style="color:red;">Fail</mark>)</td></tr><tr><td>data.transaction_id</td><td>商户提供的交易唯一标识符。</td></tr><tr><td>data.merchant_transaction_id</td><td>商户提供的交易唯一标识符。</td></tr><tr><td>data.amount</td><td>交易金额的数值。</td></tr><tr><td>data.channel</td><td>本次交易所使用的支付渠道。</td></tr><tr><td>data.created_at</td><td>交易创建时的时间戳。</td></tr><tr><td>data.updated_at</td><td>交易上次更新或修改的时间戳。</td></tr><tr><td>data.service_fee_rate</td><td>适用的服务费百分比。</td></tr><tr><td>data.service_fee_amount</td><td>从交易中扣除的固定服务费金额。</td></tr><tr><td>data.service_fee_final</td><td>应用于该交易的最终服务费金额。</td></tr><tr><td>data.nett_amount</td><td>扣除所有费用后存入商户账户的最终金额。</td></tr><tr><td>data.rrn</td><td>关联支付交易的 RRN（检索参考号）</td></tr></tbody></table>

{% tabs %}
{% tab title="Success" %}
```json
{
    "status": true,
    "data": {
        "status": "Success",
        "transaction_id": "0aa59d5a-2a10-45be-a174-f7b954113d12",
        "created_at": "2025-01-01T00:00:00.000000Z",
        "updated_at": "2025-01-01T12:00:00.000000Z",
        "merchant_transaction_id": "INQ12300456",
        "amount": "100000.00",
        "channel": "QRIS",
        "service_fee_rate": "4",
        "service_fee_amount": "0",
        "service_fee_final": "2000",
        "nett_amount": "96000",
        "rrn": "987654321"
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
