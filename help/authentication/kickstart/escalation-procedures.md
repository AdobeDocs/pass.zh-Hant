---
title: 向上呈報程式
description: 向上呈報程式
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '879'
ht-degree: 0%

---

# 向上呈報程式 {#escalation-procedures}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
> 
>請致電熱線： **+1-205-693-9813**，並傳送電子郵件至&#x200B;**tve-support@adobe.com**，主旨列中包含&#x200B;**緊急 — 事件**。

## 簡介 {#introduction}

本檔案說明影響Adobe Pass驗證、Adobe Pass並行監視及其合作夥伴之重大事件（**嚴重性1**&#x200B;層級）的支援程式。


## 嚴重程度1級事件的定義 {#definition-of-a-severity-1-level-incident}

**嚴重性1**&#x200B;層級事件是生產環境中&#x200B;**發生的**&#x200B;即時&#x200B;**狀況，**&#x200B;不允許完成一個管道和一個MVPD的驗證和/或授權流程，這會影響執行流程的MVPD的大量訂閱者。


## 嚴重程度1的事件範例 {#examples-of-severity-1-incidentcs}

* 在`https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js` （或`https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`）託管的生產存取啟用程式無法使用。

* 針對特定MVPD，當使用者選取MVPD （在任何支援的瀏覽器中）後，Adobe不再重新導向/顯示登入頁面。

* 合作夥伴會收到大量報告，指出使用者無法透過特定MVPD進行驗證/授權。

* 在驗證過程中，使用者卡在Adobe錯誤頁面上，無法重新啟動驗證/授權流程。


| **NOT** Severity 1事件的範例 |
|---|
| 對於這些型別的問題，Adobe將提供調查支援，但不是嚴重程度1的事件：<ul><li>由於Flash版本問題(缺少Flash、Flash封鎖程式、Flash版本錯誤)，一或數個訂閱者無法執行流程。</li><li>一或數個訂閱者無法驗證並停留在MVPD登入頁面上。</li><li>一或數個訂閱者已驗證，但無法播放視訊。</li><li>一個/少數/所有訂閱者在程式設計師網站上遇到JavaScript錯誤</li></ul> |

## 嚴重程度1提升流程 {#severity-1-escalation-flows}

嚴重程度1的事件可能會由Adobe或Adobe Pass驗證合作夥伴發起。 各步驟如下所示。

### 合作夥伴起始的流程 {#partner-initiated-flow}

1. 合作夥伴會識別需要Adobe立即關注的Severity 1事件（如上所述）。
1. 合作夥伴傳送電子郵件至&#x200B;**tve-support@adobe.com**，主旨列包含&#x200B;**URGENT - INCIDENT**，並新增下列資訊：
   * 標題
   * 說明和要再現的步驟
   * 作業系統/瀏覽器
   * SDK與版本
   * 受影響的裝置
   * %個受影響的使用者
   * 說明問題的HTTP追蹤或裝置記錄
   * （選用）任何可用的熒幕擷取畫面或影片擷取畫面，示範問題
1. 如果Adobe在30分鐘內沒有回應票證，合作夥伴會呼叫以下號碼：
   **1-205-693-9813**
   >[!IMPORTANT]
   >如果您的票證標題中未包含「URGENT-INCIDENT」（緊急事件），我們的通知系統將不會擷取該事件**。

### Adobe起始的流量 {#adobe-initiated-flow}

#### ...針對Adobe Pass驗證問題 {#adobe-initiated-flow-authn-issue}

1. Adobe會識別內部問題，並使用我們的追蹤系統開啟票證。

1. Adobe會通知合作夥伴的計畫經理和技術聯絡人，指定票證號碼和問題的估計影響。

1. Adobe致力於解決事件，並隨時向所有受影響的合作夥伴通報。

#### ...針對合作夥伴問題(程式設計師/MVPD) {#adobe-initiated-flow-partner-issue}

1. Adobe會識別與MVPD或程式設計師網站上整合相關的問題。

1. Adobe會依照該合作夥伴</u>的支援程式通知受影響的合作夥伴<u>，並開啟合作夥伴支援組織的票證。

1. 在影響分析期間，如果Adobe發現問題屬於事件情境的預先同意決定之一，請參閱&#x200B;**事件情境的預先同意決定**，它將採取適當的行動，而不需要等待合作夥伴的輸入。

1. 當服務還原時，Adobe將等待合作夥伴的更新和合作夥伴的通知。

## 事件情境的預先同意決策 {#pre-agreed-descn}

在某些情況下，將會在該情境的發生情況下執行預設動作：

|   | 情境 | 說明 | 動作 |
|---|---|---|---|
| S1 | Adobe會識別正常生產作業期間MVPD整合的問題。 | 在一般生產作業期間，Adobe會識別其中一個MVPD的問題，導致無法執行驗證/授權流程（例如過期的憑證、過期的SAML回應、已關閉的連線埠、已變更的引數等） | -Adobe會通知受影響的MVPD和程式設計人員。 </br> </br> -Adobe將會為所有受影響的程式設計師停用此MVPD。</br> </br> -Adobe將依照與該MVPD協定的支援程式，透過該MVPD開啟票證 |
| S2 | Adobe為程式設計師啟用新的MVPD，而程式設計師允許在啟動日期之前使用MVPD。 | Adobe正在為程式設計師網站啟用新的MVPD，而且網站已在選擇器中顯示新的MVPD，即使不應該顯示。 | -Adobe會在排程日期之前，將新MVPD出現在選擇器中的情況通知程式設計師。</br> </br> — 如有必要，程式設計師會採取動作將它從選擇器中移除。 |
| S3 | Adobe為程式設計師啟用新的MVPD，即使MVPD尚未準備好投入生產 | Adobe正在為程式設計師啟用新的MVPD，但MVPD尚未部署整合的支援，因此無法執行驗證/授權流程 |  — 只有程式設計師</br>要求，Adobe才會進行部署 </br> — 一旦執行所有測試，程式設計師將負責確保MVPD的許可。 |

## 嚴重程度1事件的回應期望 {#response-expectations-for-severity-one-incidents}

* 初始回應：30分鐘(24/7)
* 行動計畫：1小時(24/7)
* 解析度：ASAP (24/7)
