---
description: 此 API 允许您生成用于银行转账的结账页面
icon: file-invoice-dollar
---

# 结账银行发票

`POST` `/api/checkout/bank`

**Request**

<table><thead><tr><th>Name</th><th align="right">Type</th><th data-type="checkbox">Required</th><th>Validation</th><th>Description</th></tr></thead><tbody><tr><td>merchant_transaction_id</td><td align="right">string</td><td>true</td><td></td><td>用于标识商户交易的唯一识别码</td></tr><tr><td>amount</td><td align="right">string</td><td>true</td><td>数字型, 1-12 数字</td><td>本次交易的金额，以数字形式表示。</td></tr><tr><td>currency</td><td align="right">string</td><td>true</td><td><p>允许的取值：</p><p><code>"IDR"</code></p></td><td>本次交易使用的货币，仅限印尼卢比。</td></tr><tr><td>customer_name</td><td align="right">string</td><td>true</td><td>长度为 5 至 25 个字符，允许的取值： <code>a-z A-Z 0-9 - (space)</code></td><td><strong>与本次交易相关的客户姓名</strong><br></td></tr><tr><td>product_name</td><td align="right">string</td><td>true</td><td>长度为 5 至 25 个字符，允许的取值：<code>a-z A-Z 0-9 - (space)</code></td><td>与本次交易相关的产品名称</td></tr><tr><td>description</td><td align="right">string</td><td>true</td><td></td><td>与本次交易相关的额外信息</td></tr></tbody></table>

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

| Name                             | Description             |
| -------------------------------- | ----------------------- |
| transaction\_id                  | 商户提供的交易唯一标识符            |
| client\_id                       | 请求方客户端标识符               |
| amount                           | 交易金额的数值                 |
| currency                         | 交易所使用的货币，仅支持印尼卢比（IDR）   |
| description                      | 描述交易内容的说明信息             |
| customer\_details                | **涉及交易的客户详情**           |
| customer\_details.customer\_name | **涉及交易的客户姓名**           |
| customer\_details.product\_name  | **涉及交易的产品名称**           |
| checkout\_url                    | **用于通过网页浏览器完成支付的重定向网址** |

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
{% endtabs %}

***

**Callback**

{% hint style="warning" %}
要接收回调，您必须先注册您的域名网址

只有在用户完成支付后，回调才会被触发
{% endhint %}

`POST` `{your_callback_url}`

| Name                          | Description                  |
| ----------------------------- | ---------------------------- |
| transaction\_id               | 交易的唯一标识符                     |
| merchant\_transaction\_id     | 客户为商户提供的唯一标识符                |
| created\_at                   | 交易创建的时间戳                     |
| amount                        | 交易的总金额                       |
| currency                      | 交易所使用的货币 (e.g., IDR, USD).   |
| channel                       | 使用的支付渠道 (e.g., BCA, QRIS).   |
| service\_fee\_rate            | 适用的服务费百分比 (e.g., 2.5, 3.0).  |
| service\_fee\_amount          | 扣除的服务费总额 (e.g., 2000, 4000). |
| nett\_amount                  | 扣除服务费后的最终到账金额                |
| rrn                           | 关联支付交易的 RRN（检索参考号）           |
| customer\_info                | 客户的附加详情                      |
| customer\_info.customer\_name | 涉及交易的客户姓名                    |
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
