---
title: 重要概念
description: 瞭解並行監視的基本概念，包括工作階段、原則、中繼資料等
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---


# 重要概念 {#key-concepts}

瞭解並行監視的核心概念，是成功實作的關鍵。 本指南說明基本組成要素，以及它們如何共同運作。

## 核心概念 {#core-concepts}

### 工作階段

**工作階段**&#x200B;代表使用中的視訊串流執行個體。 將其視為允許使用者觀看內容的「票證」。

#### 工作階段生命週期

1. **建立** — 使用者開始觀看內容時
2. **使用中** — 使用者正在觀看時（由心率維護）
3. **終止** — 當使用者停止觀看或工作階段過期時

#### 工作階段屬性

```json
{
  "sessionId": "unique-session-identifier",
  "idp": "identity-provider",
  "subject": "user-identifier",
  "startTime": "2024-01-15T10:30:00Z",
  "expiresAt": "2024-01-15T10:40:00Z",
  "metadata": {
    "deviceId": "device-123",
    "channel": "Channel1",
    "contentType": "live"
  }
}
```

### 原則

**原則**&#x200B;是定義並行串流限制的商業規則。 它們決定使用者何時可以開始新串流。

#### 原則元件

- **規則** — 特定限制和條件
- **中繼資料需求** — 需要哪些資料
- **評估邏輯** — 如何套用原則

#### 原則範例

```json
{
  "name": "basic_stream_limit",
  "description": "Maximum 3 concurrent streams per user",
  "rules": [
    {
      "type": "maxstreams",
      "limit": 3,
      "message": "You have reached your maximum of 3 concurrent streams"
    }
  ]
}
```

### 中繼資料

**中繼資料**&#x200B;提供有關每個工作階段的內容。 其中包含有關裝置、內容、使用者和應用程式的資訊。

#### 中繼資料類別

| 類別 | 範例 | 用途 |
|-----------------|------------------------------------------|---------------------------------|
| **裝置** | `deviceId`，`deviceType`，`osName` | 識別並分類裝置 |
| **內容** | `channel`，`contentType`，`assetId` | 描述正在觀看的內容 |
| **使用者** | `subject`，`subscriptionTier` | 使用者特定資訊 |
| **應用程式** | `applicationName`，`applicationPlatform` | 應用程式專屬詳細資訊 |
| **位置** | `country`，`hba` | 地理和網路資訊 |

#### 必要與選擇性中繼資料

- **必要的中繼資料** — 必須提供以建立工作階段
- **選擇性中繼資料** — 可提供增強式原則
- **標準中繼資料** — 所有應用程式都可使用的預先定義欄位
- **自訂中繼資料** — 您定義的應用程式特定欄位

## 身分和驗證 {#identity-and-authentication}

### 身分提供者(IdP)

**身分提供者**&#x200B;是驗證使用者並提供其身分資訊的服務。 在Adobe Pass整合中，這通常是MVPD。

#### IdP範例

- 有線公司（康卡斯特公司、查特公司等）
- 衛星電視供應商(DirecTV、Dish)
- 串流服務(Netflix、Hulu)
- 行動電信業者(Verizon、AT&amp;T)

### 主旨

**subject**&#x200B;是身分提供者內使用者的唯一識別碼。 這通常是使用者的帳戶ID或訂閱者ID。

## 工作階段管理 {#session-management}

### 心率

**心率**&#x200B;是定期的API呼叫，會保持工作階段作用中。 他們會告訴系統「此使用者仍在觀看」。

#### 心率需求

- **頻率** — 每60秒（建議）
- **用途** — 保持工作階段作用中，更新中繼資料
- **終止** — 工作階段會在心率停止時過期


### 工作階段結束

工作階段可以幾種方式終止：

1. **使用者停止觀看** — 應用程式呼叫DELETE端點
2. **工作階段過期** — 逾時內未收到任何心率
3. **原則違規** — 新工作階段會終止舊的工作階段(FIFO)
4. **遠端終止** — 另一個裝置終止工作階段

## 原則評估 {#policy-evaluation}

### 原則如何運作

1. **使用者要求新資料流**
2. **系統會根據目前使用狀況評估原則**
3. **已作出決定** — 允許或拒絕
4. **已傳送回應** — 成功或衝突資訊

## 衝突解決 {#conflict-resolution}

### 什麼是衝突？

當使用者嘗試啟動新資料流但超出其限制時，會發生&#x200B;**衝突**。

### 衝突回應

發生衝突時，系統會傳回409衝突回應，其中包含詳細資訊：

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
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "max_streams",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {...}
            }
          ]
        }
      }
    ]
  }
}
```

### 解決策略

#### 後進先出

- **舊工作階段已受保護**
- **新工作階段已封鎖**
- **較簡單的UI**
- **更適合延伸檢視**


#### FIFO （先進先出）

- **新工作階段會終止現有的工作階段**
- **使用者選擇要停止的工作階段**
- **需要更複雜的UI**
- **更適合切換內容**

## 應用程式與租使用者 {#applications-and-tenants}

### 應用

**應用程式**&#x200B;是與並行監視整合的軟體程式。 每個應用程式都有唯一的ID，而且可以有自己的原則。

#### 應用程式範例

- 行動應用程式(iOS/Android)
- 網頁應用程式
- 智慧型電視應用程式
- 機上盒應用程式

### 租使用者

**租使用者**&#x200B;是擁有一或多個應用程式的組織。 租使用者可以在多個應用程式間共用原則。

#### 租使用者結構

```
Tenant: "Streaming Company"
├── Application: "Mobile App"
├── Application: "Web App"
└── Application: "TV App"
```

## 環境 {#environments}

### 中繼環境

- **用途** — 開發和測試
- **URL** - `https://streams-stage.adobeprimetime.com/v2/`
- **認證** — 測試應用程式識別碼
- **資料** — 僅測試資料

### 生產環境

- **用途** — 即時使用者流量
- **URL** - `https://streams.adobeprimetime.com/v2/`
- **認證** — 生產應用程式識別碼
- **資料** — 真實使用者資料

## 整合流程 {#integration-flow}

### 基本流量

1. **使用者驗證** — 使用Adobe Pass進行驗證
2. **工作階段建立** — 使用使用者中繼資料建立CM工作階段
3. **心率管理** — 傳送定期心率
4. **工作階段終止** — 當使用者停止觀看時終止

### 錯誤處理

1. **404找不到** - CM服務未產生工作階段識別碼的處理要求
2. **409衝突** — 處理原則違規
3. **410已過去** — 處理工作階段終止
4. **網路錯誤** — 處理連線問題
5. **驗證錯誤** — 處理認證問題


## 常用術語 {#common-terminology}

| 詞語 | 定義 |
|-----------------|--------------------------------------------|
| **工作階段** | 使用中的視訊串流執行個體 |
| **原則** | 定義使用量限制的商業規則 |
| **中繼資料** | 有關工作階段的內容相關資訊 |
| **IDP** | 身分提供者（驗證服務） |
| **主旨** | IDP中的使用者識別碼 |
| **心率** | 定期呼叫以保持工作階段作用中 |
| **衝突** | 啟動新資料流時違反原則 |
| **後進先出** | 最後進、先出衝突解決 |
| **FIFO** | 先入先出衝突解決 |
| **租使用者** | 擁有應用程式的組織 |
| **應用程式** | 使用CM的軟體程式 |

