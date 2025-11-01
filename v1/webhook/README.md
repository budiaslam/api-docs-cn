---
description: 系统在发生重要事件时会向您注册的回调 URL 发送 Webhook 通知。您必须在客户接入过程中或通过管理面板配置您的 Webhook 端点。
icon: anchor
---

# Webhook

### [**Webhook 注册**](#user-content-fn-1)[^1]

Webhook 端点在客户接入过程中注册，或可稍后通过管理面板进行配置。每种事件类型都需要单独的 Webhook 配置，包括：

* **基础 URL**（例如：`https://yourdomain.com`）
* **路径**（例如：`/api/webhooks/payment`）

完整的 Webhook URL 构造方式为：`{base_url}{path}`

### **响应要求**

Webhook 端点必须返回标准 HTTP 状态码：

* **200–299：** 成功 — Webhook 已成功处理
* **4xx：** 客户端错误 — 不会重试
* **5xx：** 服务器错误 — 系统会重试

⚠️ **响应时间必须在 2 秒以内**，Webhook 系统有 2 秒的超时限制。

### **安全最佳实践**

为确保 Webhook 请求的安全性与真实性，请遵循以下建议：

* 验证 **`X-SIGNATURE`** 请求头以确认请求来源。
* 检查 **`X-PARTNER-ID`** 是否与您的客户端 ID 一致。
* 验证 **`X-TIMESTAMP`**，防止重放攻击。
* 使用 **HTTPS** 协议的 Webhook 端点。
* 通过 **事务 ID（transaction ID）** 实现幂等性检查，防止重复处理。

[^1]: 
