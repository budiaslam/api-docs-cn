---
description: 此 API 允许商户生成用于交易处理的结账页面
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

# 结账发票

<mark style="color:green;">`POST`</mark> `/api/checkout`

**Request**

<table><thead><tr><th width="212.65625">类型</th><th width="82.26953125">类型</th><th width="100.20703125" data-type="checkbox">必填</th><th>输入校验</th><th width="223.890625">描述</th></tr></thead><tbody><tr><td>merchant_transaction_id</td><td>string</td><td>true</td><td></td><td>商户交易的唯一标识符。</td></tr><tr><td>amount</td><td>string</td><td>true</td><td>数字型, 1-12 位数字</td><td>以数字格式表示的交易金额。</td></tr><tr><td>currency</td><td>string</td><td>true</td><td>允许的取值<code>"IDR"</code></td><td>交易货币（仅限印尼盾）。</td></tr><tr><td>customer_name</td><td>string</td><td>true</td><td>允许 5–25 个字符:  <code>a-z A-Z 0-9 - (space)</code></td><td>与交易关联的客户名称。</td></tr><tr><td>product_name</td><td>string</td><td>true</td><td>允许 5–25 个字符:  <code>a-z A-Z 0-9 - (space)</code></td><td>与交易关联的商品名称。</td></tr><tr><td>description</td><td>string</td><td>false</td><td></td><td>交易的附加详情。</td></tr></tbody></table>

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

<table><thead><tr><th width="329.98828125">类型</th><th>描述</th></tr></thead><tbody><tr><td>transaction_id</td><td>商户提供的交易唯一标识符。</td></tr><tr><td>client_id</td><td>请求方客户端标识符</td></tr><tr><td>amount</td><td>交易金额的数字值。</td></tr><tr><td>currency</td><td>交易使用的货币。仅支持印尼盾（IDR）。</td></tr><tr><td>description</td><td>描述交易的说明。</td></tr><tr><td>customer_details</td><td>交易相关的客户信息。</td></tr><tr><td>customer_details.customer_name</td><td>交易相关的客户名称</td></tr><tr><td>customer_details.product_name</td><td>交易相关的商品名称</td></tr><tr><td>checkout_url</td><td>用于将用户重定向至网页完成支付的 URL</td></tr></tbody></table>



{% tabs %}
{% tab title="Success" %}
```json
{
    "transaction_id": "9e536777-2bfa-4161-9931-35f5c4b23faf",
    "client_id": "9dbf86be-f9e9-4000-8b39-fdd5a841fce1",
    "amount": "100000",
    "currency": "IDR",
    "description": "Order Product",
    "customer_details": "{\"customer_name\":\"John Doe\",\"product_name\":\"Product Test\"}",
    "checkout_url": "{host}/checkout/9e536777-2bfa-4161-9931-35f5c4b23faf"
}
```
{% endtab %}

{% tab title="Failed" %}
```
{
    "status":false,
    "message": string
}
```
{% endtab %}
{% endtabs %}

***

**Callback**

{% hint style="warning" %}
要接收回调，您必须先注册您的域名 URL

回调仅在用户完成支付后触发
{% endhint %}

<mark style="color:green;">`POST`</mark> `{your_callback_url}`

<table><thead><tr><th width="269.14453125">Name</th><th>Description</th></tr></thead><tbody><tr><td>transaction_id</td><td>交易的唯一识别码</td></tr><tr><td>merchant_transaction_id</td><td>客户为商户指定的唯一识别码</td></tr><tr><td>created_at</td><td>交易创建时的时间戳</td></tr><tr><td>amount</td><td>交易的总金额</td></tr><tr><td>currency</td><td><strong>交易所使用的货币</strong> (e.g., IDR, USD).</td></tr><tr><td>channel</td><td>本次交易所使用的支付渠道 (e.g., BCA, QRIS).</td></tr><tr><td>service_fee_rate</td><td>本次交易所适用的服务费比例 (e.g., 2.5, 3.0).</td></tr><tr><td>service_fee_amount</td><td>本次交易所扣除的服务费总金额 (e.g., 2000, 4000).</td></tr><tr><td>nett_amount</td><td>本次交易在扣除服务费后实际到账的最终金额</td></tr><tr><td>rrn</td><td>关联支付交易的 RRN（检索参考号）</td></tr><tr><td>customer_info</td><td>本次交易所提供的额外客户资料</td></tr><tr><td>customer_info.customer_name</td><td>本次交易所提供的客户姓名</td></tr><tr><td>customer_info.product_name</td><td>与本次交易相关的产品名称</td></tr></tbody></table>

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
  "rrn": "987654321",
  "customer_info": {
       "customer_name": "John Doe",
       "product_name": "Deposit Now"
  }
}

```
{% endtab %}
{% endtabs %}
