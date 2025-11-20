---
title: 處理409衝突錯誤
description: 瞭解如何在達到並行使用量限制時處理409衝突錯誤
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---


# 處理409衝突錯誤 {#handling-409-errors}

當使用者嘗試啟動新串流並點選並行使用量限制時，並行監視傳回&#x200B;**409衝突**&#x200B;回應。 瞭解如何處理此錯誤對於提供良好的使用者體驗至關重要。

## 什麼是409衝突？ {#what-is-a-409-conflict}

409衝突發生於：

1. **使用者嘗試啟動新資料流**
2. **原則評估決定**&#x200B;要求將超過限制
3. **系統傳回409**，其中包含詳細的衝突資訊
4. **應用程式必須決定**&#x200B;如何處理衝突

### 409回應結構

```json
{
  "status": "error",
  "error": {
    "code": "POLICY_VIOLATION",
    "message": "Concurrent usage limit exceeded"
  },
  "evaluationResult": {
    "decision": "DENY",
    "associatedAdvice": [
      {
        "id": "advice-1",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "max_streams",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1",
                "contentType": "live"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## 瞭解回應 {#understanding-the-response}

### 關鍵欄位

- **`decision`**：409衝突一律為「拒絕」
- **`associatedAdvice`**：說明違規的建議物件陣列
- **`conflicts`**：造成衝突的使用中工作階段清單
- **`terminationCode`**：終止特定工作階段的唯一代碼

### 建議型別

#### 規則違規建議

```json
{
  "adviceType": "rule-violation",
  "attributes": {
    "rule": "max_streams",
    "threshold": 3,
    "current": 4,
    "conflicts": [...]
  }
}
```

#### 遠端終止建議

```json
{
  "adviceType": "remote-termination",
  "attributes": {
    "terminatedBy": "session-456",
    "reason": "New session requested with X-Terminate header"
  }
}
```

## 處理策略 {#handling-strategies}

- 在FIFO模式中，CM允許新工作階段透過終止現有工作階段來啟動。
- 在後進位模式中，CM會封鎖新的工作階段並通知使用者


## 最佳實務 {#best-practices}

### 1.清除使用者通訊

- **說明限制** — 使用者應該瞭解他們被封鎖的原因
- **顯示可用的選項** — 它們可以做些什麼來解決衝突
- **提供內容** — 顯示哪些工作階段作用中

### 2.正常錯誤處理

- **不要當機** — 正常處理409錯誤
- **提供替代方案** — 提供解決衝突的方法
- **儲存使用者狀態** — 不要失去其內容選擇

### 3.使用者體驗考量事項

- **快速解決** — 輕鬆解決衝突
- **清除選擇** — 使用者應該瞭解他們的選項
- **一致的行為** — 每次處理衝突的方式都相同

### 4.技術考量

- **仔細剖析回應** — 擷取所有相關資訊
- **處理邊緣案例** — 若未傳回衝突怎麼辦？
- **記錄衝突** — 追蹤原則違規以進行分析


