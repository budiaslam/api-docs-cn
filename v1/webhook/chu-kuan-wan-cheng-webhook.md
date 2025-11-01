---
icon: anchor-circle-check
---

# 出款完成 Webhook

{% hint style="warning" %}
要接收回调，您必须先注册您的域名网址。\
只有在用户完成支付后，回调才会被触发。
{% endhint %}

<mark style="color:green;">POST</mark> `disbursement.completed`

<table><thead><tr><th width="250.2578125">Name</th><th width="120.9375">Type</th><th>Description</th></tr></thead><tbody><tr><td>event</td><td>string</td><td>事件名称：<strong>"disbursement.completed"</strong></td></tr><tr><td>data</td><td>object</td><td><strong>付款明细</strong> (Disbursement Detail)</td></tr><tr><td>data.id</td><td>string</td><td>交易的唯一标识符</td></tr><tr><td>data.merchant_transaction_id</td><td>string</td><td>客户为商户提供的唯一标识符</td></tr><tr><td>data.amount</td><td>number</td><td>交易的总金额</td></tr><tr><td>data.bank_code</td><td>string</td><td>银行代码</td></tr><tr><td>data.bank_name</td><td>string</td><td>处理该交易的银行名称。</td></tr><tr><td>data.account_name</td><td>string</td><td>收款账户持有者的姓名。</td></tr><tr><td>data.account_number</td><td>string</td><td>收款人的账户号码。</td></tr><tr><td>data.status</td><td>string</td><td>状态: <code>"success"</code> 或 <code>"fail"</code></td></tr><tr><td>data.created_at</td><td>string</td><td>付款发放创建的时间戳（ISO 8601 格式）</td></tr><tr><td>data.updated_at</td><td>string</td><td>付款发放最后更新的时间戳（ISO 8601 格式）。</td></tr></tbody></table>

```
{
  "event": "disbursement.completed",
  "data": {
    "id": "01jmmtbfcthj8y0eg3w1he4q3t",
    "merchant_transaction_id": "RLG00000000",
    "amount": 25000,
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

\
