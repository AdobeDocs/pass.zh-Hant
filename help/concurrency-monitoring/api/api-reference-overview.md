---
title: API參考概述
description: 並行監控API的完整參考，包括端點、驗證和回應格式
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 2%

---


# API參考概述 {#api-reference-overview}

「並行監控API」提供RESTful介面，用於管理串流工作階段及強制實施並行使用原則。 此參考提供所有端點、驗證方法、請求/回應格式及錯誤處理的完整檔案。

## API基本URL

### 生產環境

```
https://streams.adobeprimetime.com/v2/
```

### 中繼環境

```
https://streams-stage.adobeprimetime.com/v2/
```

**注意：**&#x200B;請一律使用中繼環境進行開發和測試。 生產認證僅在成功中繼整合後提供。

## 驗證

所有API呼叫都需要使用您的應用程式認證進行HTTP基本驗證：

- **使用者名稱：**&#x200B;您的應用程式識別碼(由Adobe提供)
- **密碼：**&#x200B;空字串

### 驗證標頭範例

```bash
curl -u "<your-app-id>:" https://streams-stage.adobeprimetime.com/v2/sessions

For an application with id "demo-app" the authentication header would be exactly as shown below, including the quotes and colon:
curl -u "demo-app:" https://streams-stage.adobeprimetime.com/v2/sessions
```

## 回應格式標準

### 成功回應

所有成功的回應都遵循此結構：

```json
{
  "status": "success",
  "data": {
    // Response-specific data
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### 錯誤回應

所有錯誤回應都遵循此結構：

```json
{
  "associatedAdvice": [
    {
      "policyName": "string",
      "ruleName": "string",
      "scope": {},
      "attribute": "string",
      "threshold": 0,
      "conflicts": [
        {}
      ]
    }
  ],
  "obligations": [
    {
      "namespace": "string",
      "action": "string",
      "arguments": [
        "string"
      ]
    }
  ]
}
```

### 評估結果格式

評估原則時（尤其是針對409衝突），回應會包含評估結果：

```json
{
  "evaluationResult": {
    "decision": "DENY",
    "obligations": [
      {
        "id": "obligation-id",
        "fulfillOn": "DENY",
        "attributes": {
          "attribute1": "value1"
        }
      }
    ],
    "associatedAdvice": [
      {
        "id": "advice-id",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "rule-name",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## 常見HTTP狀態代碼

| 程式碼 | 說明 | 當傳回時 |
|------|----------------------|------------------------------------------------|
| 200 | 確定 | 成功的GET要求 |
| 202 | 建立/接受 | 成功建立工作階段/記錄心率 |
| 400 | 錯誤請求 | 引數無效或缺少必要欄位 |
| 401 | 未獲授權 | 驗證無效或遺失 |
| 403 | 已禁止 | 許可權不足 |
| 404 | 找不到工作階段識別碼 | CM服務未產生工作階段識別碼 |
| 409 | 衝突 | 原則違規（已達到並行限制） |
| 410 | 已過期 | 工作階段已過期或已終止 |
| 429 | 太多請求 | 超出速率限制 |

## 引數傳遞方法

### 路徑引數

屬於URL路徑一部分的必要引數：

1. `{idp}` — 身分提供者識別碼
2. `{subject}` — 使用者識別碼(通常來自Adobe Pass)
3. `{sessionId}` — 工作階段識別碼（在Location標題中傳回）

### 其他引數

選用引數會在URL中傳遞：

```bash
GET /sessions/{idp}/{subject}?platform=test
```

### 表單資料(POST/PUT)

請求內文中的中繼資料和工作階段資料：

```bash
POST /sessions/{idp}/{subject}
Content-Type: application/x-www-form-urlencoded

channel=Channel1&deviceId=device-123&contentType=live
```

### 標頭

在HTTP標頭中傳遞的特殊引數：

```bash
X-Terminate: termination-code-123
X-Client-Version: 1.0.0
```

## 錯誤處理最佳實務

### 409衝突處理

當您收到409衝突回應時：

1. **剖析評估結果**&#x200B;以瞭解原則違規
2. **從**&#x200B;擷取衝突資訊`associatedAdvice`
3. 根據您的LIFO/FIFO策略&#x200B;**向使用者呈現選項**
4. 如果實作LIFO行為，**使用終止代碼**

### 410已停止處理

收到「410過去」回應時：

1. **檢查回應是否有內文** — 表示遠端終止
2. **剖析建議**&#x200B;以瞭解工作階段終止的原因
3. **更新UI**&#x200B;以反映工作階段結束
4. **正常處理** — 工作階段可能已自然逾時
5. **開始新的工作階段** — 如果認為適當，請啟動新的工作階段

### 速率限制

當您收到429太多請求時：

1. **將呼叫頻率**&#x200B;限製為每分鐘最多200個要求，這是CM接受的最大層級
2. **以所需的間隔** （每分鐘一次）傳送心率。

## 測試工具

### 互動式API總管

使用我們的[Swagger UI](https://streams-stage.adobeprimetime.com/swagger-ui/index.html)進行互動式測試：

1. 在右上角輸入您的應用程式ID
2. 按一下「瀏覽」以設定驗證
3. 使用真實引數測試端點
4. 檢視要求/回應範例
