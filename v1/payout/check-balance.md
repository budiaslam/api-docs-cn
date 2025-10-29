---
description: 此 API 可用于获取或检查可用余额。
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

# 检查余额

<mark style="color:green;">`GET`</mark> `/api/balance`

**Request**

{% tabs %}
{% tab title="Example Payload" %}
无需传递请求体。
{% endtab %}
{% endtabs %}

***

**Response**

| Name                    | Description                       |
| ----------------------- | --------------------------------- |
| status                  | 表示操作是否成功（_true_ 为成功，_false_ 为失败）。 |
| data                    | 包含交易的详细信息。                        |
| data.balance            | 客户的总账户余额。                         |
| data.reserved\_funds    | 客户的总预留资金。                         |
| data.outstanding\_funds | 客户的总未结资金。                         |
| message                 | 提供与错误消息相关的详细信息。                   |

{% tabs %}
{% tab title="Success" %}
```json
{
    "status": true,
    "data": {
        "balance": 528534207.52,
        "reserved_funds": 210000,
        "outstanding_funds": 0
    }
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
