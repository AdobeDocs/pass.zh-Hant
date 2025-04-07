---
title: REST API V2檢查清單
description: REST API V2檢查清單
exl-id: 9095d1dd-a90c-4431-9c58-9a900bfba1cf
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '2545'
ht-degree: 0%

---

# REST API V2檢查清單 {#rest-api-v2-checklist}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

本檔案針對實作使用Adobe Pass驗證[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)的使用者端應用程式的程式設計人員，彙總其強制需求與建議作法。

在實作REST API V2時，遵循本檔案必須視為驗收標準的一部分，且必須用作檢查清單，以確保已採取所有必要步驟來取得成功的整合。

## 強制需求 {#mandatory-requirements}

### 1.註冊階段 {#mandatory-requirements-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">需求</th>
      <th style="background-color: #EFF2F7;">風險</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>註冊的應用程式範圍</i></td>
      <td>使用具有REST API v2範圍的已註冊應用程式。</td>
      <td>觸發HTTP 401「未獲授權」錯誤回應、系統資源超載及延遲增加的風險。</td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;"><i>使用者端憑證快取</i></td>
      <td>將使用者端憑證儲存在永久儲存體中，並重複用於每個存取權杖請求。</td>
      <td>重新產生使用者端認證時，可能會遺失驗證。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>存取權杖快取</i></td>
      <td>將存取權杖儲存在永久儲存體中，並重複使用直到過期為止。<br/><br/>請勿為每個REST API v2呼叫要求新的權杖，只有在存取權杖過期時才重新整理存取權杖。</td>
      <td>可能會造成系統資源超載、延遲增加，以及可能觸發HTTP 429「太多請求」錯誤回應。</td>
   </tr>
</table>

### 2.設定階段 {#mandatory-requirements-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">需求</th>
      <th style="background-color: #EFF2F7;">風險</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>設定擷取</i></td>
      <td>只有在需要提示使用者在驗證階段之前選取其MVPD （電視提供者）時，才會擷取設定回應。<br/><br/>下列情況不需要擷取設定回應：<ul><li>使用者已經過驗證。</li><li>授予使用者暫時存取權。</li><li>使用者驗證已過期，但系統會提示使用者，確認他們仍是先前所選MVPD的訂閱者。</li></ul></td>
      <td>可能會造成系統資源超載及延遲增加。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>TV提供者選擇快取</i></td>
      <td>將使用者的付費電視提供者(MVPD)選擇專案儲存在永久儲存空間中，以便用於所有後續階段：<ul><li>從設定回應存放區，使用者選取MVPD「id」。</li><li>從設定回應存放區，使用者選取MVPD "displayName"。</li><li>從設定回應存放區，使用者選取MVPD "logoUrl"。</li></ul></td>
      <td>可能會造成系統資源超載及延遲增加。</td>
   </tr>
</table>

### 3.驗證階段 {#mandatory-requirements-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">需求</th>
      <th style="background-color: #EFF2F7;">風險</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>輪詢機制啟動</i></td>
      <td>在下列條件下啟動輪詢機制：<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">在主要（熒幕）應用程式內執行的驗證</a></b><ul><li>瀏覽器元件載入在「工作階段」端點要求中為「redirectUrl」引數指定的URL後，當使用者到達最終目的地頁面時，主要（串流）應用程式應該開始輪詢。</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">在次要（熒幕）應用程式內執行的驗證</a></b><ul><li>主要（串流）應用程式應在使用者起始驗證程式後立即開始輪詢 — 在收到「工作階段」端點回應並向使用者顯示驗證代碼之後。</li></ul></td>
      <td>可能會造成系統資源超載、延遲增加，以及可能觸發HTTP 429「太多請求」錯誤回應。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>正在停止輪詢機制</i></td>
      <td>在下列條件下停止輪詢機制： <br/><br/><b>成功的驗證</b><ul><li>已成功擷取使用者的設定檔資訊，確認其驗證狀態，因此不再需要輪詢。</li></ul><br/><b>驗證工作階段與程式碼到期</b><ul><li>驗證工作階段和程式碼到期，使用者必須重新啟動驗證程式，使用先前驗證程式碼的輪詢應立即停止。</li></ul><br/><b>已產生新的驗證碼</b><ul><li>如果使用者要求新的驗證代碼，則現有工作階段會失效，並且使用先前驗證代碼的輪詢應立即停止。</li></ul></td>
      <td>可能會造成系統資源超載、延遲增加，以及可能觸發HTTP 429「太多請求」錯誤回應。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>輪詢機制設定</i></td>
      <td>在下列條件下設定輪詢機制頻率：<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">在主要（熒幕）應用程式內執行的驗證</a></b><ul><li>主要（串流）應用程式應每3-5秒或更長時間輪詢一次。</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">在次要（熒幕）應用程式內執行的驗證</a></b><ul><li>主要（串流）應用程式應每3-5秒輪詢一次。</li></ul></td>
      <td>可能會造成系統資源超載、延遲增加，以及可能觸發HTTP 429「太多請求」錯誤回應。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>設定檔快取</i></td>
      <td>將部分使用者設定檔資訊儲存在永久儲存體中，以提高效能並將不必要的REST API v2呼叫降至最低。<br/><br/>快取應該集中在下列設定檔回應欄位：<br/><br/><b>mvpd</b><ul><li>使用者端應用程式可使用此項來追蹤使用者選取的電視提供者，並在預先授權或授權階段中繼續使用它。</li><li>當目前的使用者設定檔過期時，使用者端應用程式可以使用記憶中的MVPD選項，並直接要求使用者確認。</li></ul><br/><b>屬性</b><ul><li>用於根據不同的使用者中繼資料索引鍵（例如zip、maxRating等）個人化使用者體驗。</li><li>使用者中繼資料在驗證流程完成後即可使用，因此使用者端應用程式不需要查詢個別端點來擷取使用者中繼資料資訊，因為它已包含在設定檔資訊中。</li><li>某些中繼資料屬性可能會在「授權階段」期間更新，具體取決於MVPD （例如Charter）和特定的中繼資料屬性（例如householdID）。 因此，使用者端應用程式可能需要在授權後再次查詢設定檔API，以擷取最新的使用者中繼資料。</li></ul></td>
      <td>可能會造成系統資源超載、延遲增加，以及可能觸發HTTP 429「太多請求」錯誤回應。</td>
   </tr>
</table>

### 4. （選擇性）預先授權階段 {#mandatory-requirements-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">需求</th>
      <th style="background-color: #EFF2F7;">風險</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>預先授權決定擷取</i></td>
      <td>使用預先授權決定進行內容篩選，永不用於播放決定。</td>
      <td>違反程式設計師、MVPD和Adobe之間合約協定的風險。<br/><br/>略過我們的監視和警示系統的風險。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>預先授權決定擷取重試</i></td>
      <td>適當處理<a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">增強型錯誤碼</a>，並利用動作欄位來決定必要的修正步驟。<br/><br/>只有有限數目的增強型錯誤碼才應重試，而大多數需要其他解決方案，如動作欄位中所指定。<br/><br/>請確定任何針對擷取預先授權決定所實作的重試機制都不會導致無止境的回圈，而且它會將重試限製為合理的數字（即2-3）。</td>
      <td>可能會造成系統資源超載、延遲增加，以及可能觸發HTTP 429「太多請求」錯誤回應。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>預先授權決定快取</i></td>
      <td>快取成功允許記憶體中的決定改善效能，並將不必要的REST API v2呼叫減至最少，因為應用程式執行時的訂閱更新並不頻繁。</td>
      <td>可能會造成系統資源超載、延遲增加，以及可能觸發HTTP 429「太多請求」錯誤回應。</td>
   </tr>
</table>

### 5.授權階段 {#mandatory-requirements-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">需求</th>
      <th style="background-color: #EFF2F7;">風險</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>授權決定擷取</i></td>
      <td>在播放之前取得授權決定 — 無論預先授權決定是否存在。<br/><br/>即使媒體權杖在播放期間到期，並允許資料流繼續不受中斷，並在使用者提出下一個播放請求時請求包含（新增）媒體權杖的新授權決定，無論該媒體權杖是否用於相同或不同的資源。<br/><br/>長時間執行的即時資料流可選擇在視訊作業後要求新的授權決定，例如暫停內容、啟動商業插播，或在MRSS發生變更時修改資產層級設定。</td>
      <td>違反程式設計師、MVPD和Adobe之間合約協定的風險。<br/><br/>略過我們的監視和警示系統的風險。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>授權決定擷取重試</i></td>
      <td>適當處理<a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">增強型錯誤碼</a>，並利用動作欄位來決定必要的修正步驟。<br/><br/>只有有限數目的增強型錯誤碼才應重試，而大多數需要其他解決方案，如動作欄位中所指定。<br/><br/>請確定為擷取授權決定所實作的重試機制不會產生無止盡的回圈，而且它會將重試限製為合理的數字（即2-3）。</td>
      <td>可能會造成系統資源超載、延遲增加，以及可能觸發HTTP 429「太多請求」錯誤回應。</td>
   </tr>
</table>

### 6.登出階段 {#mandatory-requirements-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">需求</th>
      <th style="background-color: #EFF2F7;">風險</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>登出支援</i></td>
      <td>實作登出API以允許使用者手動登出、終止其已驗證的設定檔，並遵循為每個已移除設定檔指定的REST API v2動作名稱：<ul><li>對於支援登出端點的MVPD，使用者端應用程式需要導覽至使用者代理程式中提供的「url」。</li><li>針對「appleSSO」型別設定檔，使用者端應用程式需要引導使用者也從合作夥伴層級(Apple的系統設定)登出。</li></ul></td>
      <td>由於缺少使用者端應用程式端的支援，導致使用者端應用程式故障的風險。</td>
   </tr>
</table>

### 7.引數和標題 {#mandatory-requirements-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">需求</th>
      <th style="background-color: #EFF2F7;">風險</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>傳送授權標頭</i></td>
      <td>針對每個REST API v2請求傳送<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md">Authorization</a>標頭。</td>
      <td>觸發HTTP 401「未獲授權」錯誤回應、系統資源超載及延遲增加的風險。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>傳送AP-Device-Identifier標頭</i></td>
      <td>針對每個REST API v2要求傳送<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>標頭。<br/><br/>即使請求來自代表裝置的伺服器，AP-Device-Identifier標頭值也必須反映實際的串流裝置識別碼。</td>
      <td>觸發HTTP 400「錯誤請求」錯誤回應、系統資源超載及延遲增加的風險。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>傳送X-Device-Info標頭</i></td>
      <td>針對每個REST API v2請求傳送<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a>標頭。<br/><br/>即使請求來自代表裝置的伺服器，X-Device-Info標頭值也必須反映實際的串流裝置資訊。</td>
      <td>歸類為源自未知平台且視為不安全的風險，會受限於較嚴格的規則，例如較短的驗證TTL。<br/><br/>此外，某些欄位（例如串流裝置connectionIp和connectionPort）是諸如Spectrum的Home Base Authentication之類功能的必要欄位。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>穩定裝置識別碼</i></td>
      <td>計算並儲存<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>標頭的穩定裝置識別碼，此識別碼不會因更新或重新啟動而變更。<br/><br/>對於沒有硬體識別碼的平台，請從應用程式屬性產生唯一識別碼，並加以儲存。</td>
      <td>裝置識別碼變更時，可能會遺失驗證。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>遵循API參考</i></td>
      <td>確保您只傳送REST API v2預期的引數和標頭。</td>
      <td>觸發HTTP 400「錯誤請求」錯誤回應、系統資源超載及延遲增加的風險。</td>
   </tr>
</table>

### 8.錯誤處理 {#mandatory-requirements-error-handling}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">需求</th>
      <th style="background-color: #EFF2F7;">風險</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>增強的錯誤碼處理支援</i></td>
      <td>適當處理<a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">增強型錯誤碼</a>，並利用動作欄位來決定必要的修正步驟。<br/><br/>只有有限數目的增強型錯誤碼才應重試，而大多數需要其他解決方案，如動作欄位中所指定。<br/><br/> <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2">增強錯誤碼 — REST API V2</a>檔案中所列的大多數增強錯誤碼，如果能在啟動應用程式之前的開發階段中正確處理，就可以完全避免。</td>
      <td>可能會造成系統資源超載、延遲增加，以及可能觸發HTTP 429「太多請求」錯誤回應。<br/><br/>由於遺漏處理增強型錯誤碼，導致錯誤訊息不清楚、使用者指引不正確或遞補行為不正確，因此使用者端應用程式可能會發生故障。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>HTTP錯誤處理支援</i></td>
      <td>區別處理HTTP錯誤回應（例如400、401、403、404、405、500）和包含增強型錯誤代碼裝載的成功回應（例如200、201），如上所述。<br/><br/>只有有限數量的HTTP錯誤碼需要重試，而大多數需要替代解決方案。<br/><br/>如果在啟動應用程式之前，在開發階段處理正確，大部分的HTTP錯誤回應都可以完全避免。</td>
      <td>可能會造成系統資源超載、延遲增加，以及可能觸發HTTP 429「太多請求」錯誤回應。<br/><br/>由於遺漏處理增強型錯誤碼，導致錯誤訊息不清楚、使用者指引不正確或遞補行為不正確，因此使用者端應用程式可能會發生故障。</td>
   </tr>
</table>

### 9.測試 {#mandatory-requirements-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">需求</th>
      <th style="background-color: #EFF2F7;">風險</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>生命週期測試</i></td>
      <td>使用官方的Adobe Pass驗證非生產環境開發和測試應用程式：<ul><li>前期生產</li><li>Release-Staging</li></ul><br/>在啟動至發行生產之前，先在這些環境中執行徹底品質保證(QA)。<br/><br/>使用者端應用程式必須先在非生產環境中完成端對端驗證，才能進行發行生產。</td>
      <td>發生嚴重和重大缺陷的風險。<br/><br/>缺少簡短且有效的偵錯路徑，可能會妨礙Adobe支援和工程部門快速介入。</td>
   </tr>
</table>

## 建議作法 {#recommended-practices}

### 1.註冊階段 {#recommended-practices-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">實務</th>
      <th style="background-color: #EFF2F7;">風險</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>存取Token驗證</i></td>
      <td>主動檢查存取權杖是否有效，以在過期時重新整理。<br/><br/>在重試原始請求之前，請確定用於處理HTTP 401「未獲授權」錯誤的任何重試機制都會先重新整理存取權杖。</td>
      <td>觸發HTTP 401「未獲授權」錯誤回應、系統資源超載及延遲增加的風險。</td>
   </tr>
</table>

### 2.設定階段 {#recommended-practices-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">實務</th>
      <th style="background-color: #EFF2F7;">風險</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>設定快取</i></td>
      <td>將設定回應儲存在記憶體或持續性儲存體一段時間（例如3-5分鐘）以提高效能並將不必要的REST API v2呼叫減至最少。</td>
      <td>可能會造成系統資源超載及延遲增加。</td>
   </tr>
</table>

### 3.驗證階段 {#recommended-practices-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">實務</th>
      <th style="background-color: #EFF2F7;">風險</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>驗證代碼驗證（第2個畫面驗證）</i></td>
      <td>在呼叫/api/v2/authenticate API之前，驗證透過次要（第2個）應用程式（熒幕）上的使用者輸入所提交的驗證代碼，符合以下條件：<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-with-preselected-mvpd">在次要（熒幕）應用程式內使用預先選取的mvpd執行的驗證</a></b><ul><li>使用<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md">繼續驗證工作階段</a> - POST /api/v2/{serviceProvider}/sessions/{code}</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-without-preselected-mvpd">在次要（熒幕）應用程式內執行的驗證，但未預先選取mvpd</a></b><ul><li>利用<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md">擷取驗證工作階段</a> - GET /api/v2/{serviceProvider}/sessions/{code}</li></ul><br/>如果輸入的驗證代碼錯誤或驗證工作階段過期，使用者端應用程式會收到錯誤。</td>
      <td>在驗證期間可能會出現各種錯誤回應和工作流程問題。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>多個設定檔支援</i></td>
      <td>透過提示使用者選取設定檔或套用自訂邏輯（例如自動選取有效期最長的設定檔），確保使用者端應用程式可處理多個設定檔。</td>
      <td>由於缺少使用者端應用程式端的支援，導致使用者端應用程式故障的風險。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>（選用）非基本流量支援</i></td>
      <td>支援非基本流程，以備客戶應用程式業務需要：<ul><li>存取流程效能降低（進階功能）</li><li>暫時存取流程（進階功能）</li><li>單一登入存取流程（標準功能）</li></ul></td>
      <td>可能造成不理想的使用者體驗。</td>
   </tr>
</table>

### 4. （選擇性）預先授權階段 {#recommended-practices-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">實務</th>
      <th style="background-color: #EFF2F7;">風險</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>使用者體驗</i></td>
      <td>使用MVPD或Adobe透過增強型錯誤代碼提供的訊息，在預先授權決定被拒絕時顯示清除的使用者意見回應。</td>
      <td>可能造成不理想的使用者體驗。</td>
   </tr>
</table>

### 5.授權階段 {#recommended-practices-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">實務</th>
      <th style="background-color: #EFF2F7;">風險</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>媒體權杖驗證</i></td>
      <td>使用<a href="/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier">媒體權杖驗證器</a>資料庫驗證媒體權杖。</td>
      <td>冒著詐騙計畫的風險，例如串流擷取。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>使用者體驗</i></td>
      <td>使用MVPD或Adobe透過增強型錯誤代碼提供的訊息，在授權決定被拒絕時顯示清除的使用者回饋。</td>
      <td>可能造成不理想的使用者體驗。</td>
   </tr>
</table>

### 6.登出階段 {#recommended-practices-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">實務</th>
      <th style="background-color: #EFF2F7;">風險</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>使用者體驗</i></td>
      <td>在拒絕預先授權或授權的情況下，避免自動（以程式設計方式）呼叫登出API，因為登出API只應該在回應直接使用者請求時叫用。</td>
      <td>風險會讓使用者混淆驗證失敗。</td>
   </tr>
</table>

### 7.引數和標題 {#recommended-practices-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">實務</th>
      <th style="background-color: #EFF2F7;">風險</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>重複使用程式碼</i></td>
      <td>重複使用REST API v1的程式碼，以透過微幅調整計算裝置識別碼和裝置資訊，但請確保您僅傳送REST API v2預期的引數和標題。<br/><br/>重複使用REST API v1的程式碼來呼叫DCR API以擷取存取權杖。</td>
      <td>-</td>
   </tr>
</table>

### 8.測試 {#recommended-practices-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">實務</th>
      <th style="background-color: #EFF2F7;">風險</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>測試涵蓋範圍</i></td>
      <td>確定已跨裝置和平台測試下列基本流程：<br/><br/><b>驗證流程</b><ul><li>主要應用程式（熒幕）驗證案例</li><li>次要應用程式（熒幕）驗證案例</li></ul><br/><b>（選擇性）預先授權流程</b><ul><li>測試允許決定案例</li><li>測試拒絕決定案例</li></ul><br/><b>授權流程</b><ul><li>測試允許決定案例</li><li>測試拒絕決定案例</li></ul><br/><b>登出流程</b><br/><br/>此外，測試其他存取流程（如果適用）：<br/><br/><ul><li>存取流程效能降低（進階功能）</li><li>暫時存取流程（進階功能）</li><li>單一登入存取流程（標準功能）</li></ul><br/>涵蓋MVPD最上層的整合（涵蓋使用最廣泛的提供者）。</td>
      <td>在生產環境中發生無法預見的失敗的風險，尤其是在測試頻率較低的平台或極少數流程（如負面情況）中。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>測試工具</i></td>
      <td>使用<a href="https://developer.adobe.com/adobe-pass/">Adobe Developer</a>網站。</td>
      <td>-</td>
   </tr>
</table>

## 摘要 {#summary}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">階段</th>
      <th style="background-color: #EFF2F7;">強制</th>
      <th style="background-color: #EFF2F7;">（強烈）建議</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>註冊</i></td>
      <td>快取使用者端認證<br/><br/>快取存取權杖</td>
      <td>驗證和重新整理存取權杖</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>設定</i></td>
      <td>將設定回應擷取最小化</td>
      <td>快取設定回應</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>驗證</i></td>
      <td>輪詢機制微調<br/><br/>設定檔的快取部分</td>
      <td>支援多個設定檔<br/><br/>支援降級功能（若業務需求）<br/><br/>支援TempPass功能（若業務需求）<br/><br/>支援單一登入功能（若業務需求）</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>預先授權</i></td>
      <td>快取允許預先授權決定<br/><br/>重試機制微調</td>
      <td>增強使用者體驗，針對拒絕的預先授權決定使用錯誤代碼</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Authorization</i></td>
      <td>使用者要求播放時擷取授權決定<br/><br/>重試機制微調</td>
      <td>針對拒絕的授權決定<br/><br/>媒體權杖驗證，使用錯誤碼來增強使用者體驗</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>登出</i></td>
      <td>實作登出API以允許使用者手動登出</td>
      <td>避免自動呼叫登出API</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">強制</th>
      <th style="background-color: #EFF2F7;">（強烈）建議</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>引數和標頭</i></td>
      <td>遵循必要的標頭規格</td>
      <td>重複使用REST API v1的程式碼</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>錯誤處理</i></td>
      <td>實作增強式錯誤處理<br/><br/>實作HTTP錯誤處理</td>
      <td>-</td>
   </tr>
</table>
