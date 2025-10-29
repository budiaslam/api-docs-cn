---
description: 此 API 允许商户生成用于交易处理的结账页面
icon: qrcode
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

# QRIS

<mark style="color:green;">`POST`</mark> `/api/create/qris`

**Request**

<table><thead><tr><th width="217.06640625">Name</th><th width="76.33984375">Type</th><th width="86.2890625" data-type="checkbox">Required</th><th width="194.36328125">Validation</th><th>Description</th></tr></thead><tbody><tr><td>merchant_transaction_id</td><td>string</td><td>true</td><td></td><td>商户交易的唯一标识符</td></tr><tr><td>amount</td><td>string</td><td>true</td><td>数字类型，1–12 位</td><td>以数字格式表示的交易金额</td></tr><tr><td>currency</td><td>string</td><td>true</td><td>允许的值："IDR"</td><td>交易所使用的货币（仅限印尼卢比）</td></tr><tr><td>customer_name</td><td>string</td><td>true</td><td>长度在 5–25 个字符之间，允许字符：a–z、A–Z、0–9、-（短横线）、（空格）</td><td>与交易关联的客户姓名</td></tr><tr><td>product_name</td><td>string</td><td>true</td><td>长度在 5–25 个字符之间，允许字符：a–z、A–Z、0–9、-（短横线）、（空格）</td><td><strong>与交易关联的产品名称</strong></td></tr><tr><td>description</td><td>string</td><td>false</td><td></td><td><strong>交易的附加详情</strong></td></tr></tbody></table>

{% tabs %}
{% tab title="Example Payload" %}
```json
{
  "merchant_transaction_id": "ICZ10000001",
  "amount": "100000",
  "currency": "IDR",
  "description": "Lorem Ipsum",
  "customer_name": "John Doe",
  "product_name": "Product Test"
}
```
{% endtab %}
{% endtabs %}

***

**Response**

<table><thead><tr><th width="170.515625">Name</th><th>Description</th></tr></thead><tbody><tr><td>status</td><td>表示请求的状态。成功时为 <strong>true</strong>，失败时为 <strong>false</strong></td></tr><tr><td>message</td><td>描述请求结果的消息。通常在请求成功时为 “<strong>success</strong>”</td></tr><tr><td>data.transaction_id</td><td>商户提供的交易唯一标识符</td></tr><tr><td>data.qr_url</td><td>生成的二维码链接（如适用）。如果没有提供二维码链接，此字段可能为 null</td></tr><tr><td>data.qr_content</td><td>二维码的原始内容。通常是特定格式的字符串（例如用于支付系统的 QRIS 格式）</td></tr><tr><td>data.redirect_url</td><td>用户需要跳转的链接（如适用）。如果没有提供跳转链接，此字段可能为 null</td></tr></tbody></table>

{% tabs %}
{% tab title="Success" %}
```json
{
    "status": true,
    "message": "success",
    "data": {
        "transaction_id": "8366a675-f1cc-418c-b3a8-358410671ca0",
        "qr_url": "{host}/images/payment/qris/BP1f44d20caefb8b0ef181797bffed088c.png",
        "qr_content": null,
        "redirect_url": "{host}/checkout/8366a675-f1cc-418c-b3a8-358410671ca0/qris"
    }
}
```
{% endtab %}

{% tab title="Failed" %}
<pre class="language-json"><code class="lang-json"><strong>{
</strong>    "status": false,
    "message": string,
}
</code></pre>
{% endtab %}
{% endtabs %}

***

**Callback**

{% hint style="warning" %}
要接收回调，您必须先注册您的域名 URL\
只有在用户完成付款后，回调才会被触发
{% endhint %}

<mark style="color:green;">`POST`</mark> `{your_callback_url}`

<table><thead><tr><th width="269.14453125">Name</th><th>Description</th></tr></thead><tbody><tr><td>transaction_id</td><td>交易的唯一标识符。</td></tr><tr><td>merchant_transaction_id</td><td>客户为商户提供的唯一标识符。</td></tr><tr><td>created_at</td><td>交易创建的时间戳。</td></tr><tr><td>amount</td><td>交易总金额。</td></tr><tr><td>currency</td><td>交易使用的货币（例如：IDR，USD）。</td></tr><tr><td>channel</td><td>使用的支付渠道（例如：BCA，QRIS）。</td></tr><tr><td>service_fee_rate</td><td>应用的服务费百分比（例如：2.5，3.0）。</td></tr><tr><td>service_fee_amount</td><td>扣除的服务费总额（例如：2000，4000）。</td></tr><tr><td>nett_amount</td><td>扣除费用后的最终到账金额。</td></tr><tr><td>rrn</td><td>相关支付交易的 RRN（检索参考号）。</td></tr><tr><td>customer_info</td><td>额外的客户信息。</td></tr><tr><td>customer_info.customer_name</td><td>客户姓名。</td></tr><tr><td>customer_info.product_name</td><td>与交易关联的产品名称。</td></tr></tbody></table>

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
  "rrn": "0392813042",
  "customer_info": {
       "customer_name": "John Doe",
       "product_name": "Deposit Now"
  }
}

```
{% endtab %}
{% endtabs %}
