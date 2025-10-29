---
icon: signature
layout:
  width: default
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: false
  pagination:
    visible: false
  metadata:
    visible: true
---

# 签名生成

要生成签名，您需要一对 **RSA 私钥和公钥**。\
您可以自行生成这些密钥，或使用第三方工具（例如 [cryptotools.net](https://cryptotools.net/rsagen).）来创建。

**必填参数**\
要生成有效的签名，请确保包含以下参数:

<table><thead><tr><th width="170.92578125">Parameter</th><th width="575.16796875">Description</th></tr></thead><tbody><tr><td>METHOD</td><td>HTTP 请求方法 (e.g., <code>POST</code>, <code>GET</code>).</td></tr><tr><td>URL</td><td>API 端点网址 (e.g., <code>/api/checkout</code>).</td></tr><tr><td>PAYLOAD</td><td>将 <strong>JSON 请求正文（request body）</strong> 进行 <strong>SHA-256 哈希运算</strong>，并将结果转换为 <strong>小写十六进制字符串</strong>。对于 <strong>GET</strong> 请求，无需发送请求正文（payload）。</td></tr><tr><td>TIMESTAMP</td><td>当前时间戳，采用 <strong>ISO8601</strong> 格式。</td></tr><tr><td>SECRET_KEY</td><td>由商户提供的唯一共享密钥</td></tr></tbody></table>

#### 参数拼接

#### 签名字符串（**stringToSign**）通过以下格式将参数依次拼接而成：

```
stringToSign = {METHOD}:{URL}:{PAYLOAD}:{TIMESTAMP}
```

将占位符替换为实际值：

* `{METHOD}` → HTTP 请求方法。
* `{URL}` → API 接口地址。 (e.g., `/api/checkout`).
* `{PAYLOAD}` → JSON 请求正文的 **SHA-256 哈希值**（对于 **GET** 请求，无需发送请求正文）
* `{TIMESTAMP}` → 当前时间戳，采用 **ISO8601** 格式

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const httpMethod = 'POST';
const endpointUrl = '/api/create/va';
const timestamp = '2024-12-16T12:11:14+07:00';
const payload = `
{
    "merchant_transaction_id": "ICZ10000001",
    "amount": "100000",
    "currency": "IDR",
    "bank_code": "nobu",
    "description": "Lorem Ipsum",
    "customer_name": "John Doe",
    "product_name": "Product Test"
}`;
const body = JSON.parse(payload);
const payload = crypto.createHash('sha256').update(JSON.stringify(body, null, 0)).digest('hex').toLowerCase();
const stringToSign = [
    httpMethod,
    endpointUrl,
    payload,
    timestamp
];
let signature = '';
const stringToSign = stringToSign.join(':');
const privateKey = fs.readFileSync('./path-to-private-key-file.pem');
const sign = crypto.createSign('RSA-SHA256');
sign.update(stringToSign);
signature = sign.sign(privateKey, 'base64');
console.log('Generated Signature:', signature);
```
{% endtab %}
{% endtabs %}

> * 请确保私钥被安全存储，且不要在公共代码库中公开。
> * `SECRET_KEY` 必须妥善保密，仅用于身份验证目的。
> * `TIMESTAMP` 必须采用 **ISO8601** 格式生成，并且时间不得超过几分钟，以防止重放攻击。
