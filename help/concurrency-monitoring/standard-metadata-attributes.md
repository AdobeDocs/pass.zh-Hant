---
title: 標準中繼資料屬性
description: 標準中繼資料屬性
exl-id: 99ffa98c-213f-47a5-a6e7-fbacb77875d0
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '1053'
ht-degree: 0%

---

# 標準中繼資料屬性 {#std-metadata-attributes}

此頁面旨在提供可供「並行監視」服務處理，且可作為可實作之原則基礎的中繼資料屬性完整清單。 標準中繼資料屬性可依下列方式分類：

* 設計包含的屬性（在每次工作階段初始化呼叫時傳送，因為URL路徑中需要這些屬性）。 沒有這些值就無法執行有效的呼叫。
* 中繼資料屬性：在工作階段初始化呼叫期間，需要以表單資料的形式傳遞的值（如果後端原則需要其值的話）。

## 設計所需的屬性 {#attr-req-by-design}

Concurrency Monitoring API強制使用者端傳送下列值做為任何有效初始化呼叫的一部分： [工作階段起始呼叫](/help/concurrency-monitoring/restrict-concurr-usage-mult-apps.md#api-calls-descr)。

| 欄位名稱 | 範例值 | 使用位置 | 取得自 |
|---------------|-----------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationId | 75b4-431b-adb2-eb6b9e546013 | 授權標頭 | 整合時的Zendesk票證 |
| mvpdName | Sample_MVPD | URI路徑 | 使用者選取MVPD時，從設定端點進行Adobe Pass驗證 |
| accountId | 12345 | URI路徑 | 使用者登入後的Adobe Pass Authentication upstreamUserID中繼資料[使用者Metadata upstreamUserID - Adobe Pass驗證](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) |


## 中繼資料屬性 {#metadata-attr}

程式設計師和MVPD可以使用下表中的欄位來建立將在並行監視中實作的原則。

透過[API v2.0](http://docs.adobeptime.io/cm-api-v2/)，如果定義的原則需要這些屬性中的任何一個，則沒有該屬性的工作階段初始化嘗試將導致400錯誤請求。


| 實體 | 屬性名稱 | 資料型別 | 說明 | 外部參考（例如：EIDR、OATC） | 範例值 | 驗證規則 |
|---------------|-------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 媒體公司 | 程式設計師名稱 | 字串 | 程式設計師的名稱 |                                                   | 程式設計師X |                                                                                   |
| 資源 | 頻道 | 字串 | 電視頻道 |                                                   | ChannelY |                                                                                   |
|                 | assetId | 字串 | 針對此內容顯示的「易記」或消費者可讀取的標題 | [EIDR 2.0資料欄位參考](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/EIDR_2_0_Data_Fields.pdf){target=_blank} | 賓漢 |                                                                                   |
|                 | type | 分項清單 | 說明TveItem所表示之內容的一般型別的值。 列舉值包括：電影broadcastEpisode nonBroadcastEpisode musicVideo awardsShow clip concert conference newsEvent sportingEvent預告片 | [OATC中繼資料摘要建議作法](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | broadcastEpisode | 欄位必須對應到列舉中的其中一個專案 |
|                 | contenttype | 字串 | 此欄位會決定要求的內容為即時或VOD | 不適用 | 即時， vod | live或vod |
|                 | 型別 | 字串 | 串流處理內容的型別。 說明一般程式設計型別 | [OATC中繼資料摘要建議使用](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank}實務 | 喜劇 | 有效的型別型別 |
|                 | 期間 | 數字 | 媒體專案持續時間（秒數） | [OATC中繼資料摘要建議作法](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | 1800 | 編號規則 |
| 裝置/瀏覽器 | deviceId | 字串 | 唯一裝置識別碼。 | [Device Atlas屬性](https://deviceatlas.com/device-data/properties){target=_blank} | 2b6f0cc904d137be2e1730235f5664094b831186 |                                                                                   |
|                 | 裝置名稱 | 字串 | 此裝置的易記名稱。 |                                                   | Joe的iPad |                                                                                   |
|                 | marketingName | 字串 | 裝置的行銷名稱（或客戶易記名稱） | [Device Atlas屬性](https://deviceatlas.com/device-data/properties){target=_blank} | iPhone 6s | 有效的行銷名稱 |
|                 | 行動裝置 | 布林值 | 如果裝置用於移動用途，則為True | [Device Atlas屬性](https://deviceatlas.com/device-data/properties){target=_blank} | true， false | true， false |
|                 | 裝置型號 | 字串 | 裝置、瀏覽器或其他元件的型號名稱 | [Device Atlas屬性](https://deviceatlas.com/device-data/properties){target=_blank} | 平板電腦、手機、xbox。 機上盒 | 有效的裝置型號名稱 |
|                 | osName | 字串 | 裝置正在執行的作業系統 | [Device Atlas — 作業系統預先定義的屬性值](https://deviceatlas.com/device-data/explorer/#defined_property_values/877430/4121272){target=_blank} | Android、Windows 10、OS X、Linux、其他附註：您必須使用Device Atlas中的使用者名稱和密碼登入，才能檢視屬性值 | 預期值是Device Atlas預先定義屬性中的值之一 |
|                 | browserName | 字串 | 裝置上瀏覽器的名稱或型別 | [Device Atlas — 瀏覽器預先定義的屬性值](https://deviceatlas.com/device-data/explorer/#defined_property_values/7/2705619){target=_blank} | 裝置上瀏覽器的名稱或型別。  注意：您必須使用Device Atlas中的使用者名稱和密碼登入，才能檢視屬性值 | 預期值是Device Atlas預先定義屬性中的值之一 |
|                 | browserVersion | 字串 | 裝置上的瀏覽器版本 | [Device Atlas屬性](https://deviceatlas.com/device-data/properties){target=_blank} | 裝置上的瀏覽器版本 |                                                                                   |
| 應用 | applicationName | 字串 | 應用程式的「使用者易記」或消費者可讀取的名稱 | 不適用 | 範例_應用程式 |                                                                                   |
|                 | applicationId | 字串 | 唯一識別使用者端應用程式的應用程式ID。 | 不適用 | de305d54-75b4-431b-adb2-eb6b9e546013 |                                                                                   |
|                 | applicationPlatform | 字串 | 應用程式的原生平台 | 不適用 | ios， android |                                                                                   |
|                 | applicationversion | 字串 | 此值可用於分析目的 | 不適用 | 1.0， 2.0 |                                                                                   |
| 主旨 | accountId | 字串 | 並行監視主體的帳戶ID (在MVPD的範圍內) | 不適用 | test-account |                                                                                   |
|                 | contracttype | 字串 | 進階、基本。 客戶可以自由地將此專案新增為自訂中繼資料，並在其自己的領域中使用它 | 不適用 | premium，基本 |                                                                                   |
| 使用者 | 名稱 | 字串 | 有些MVPD會提供與播放內容之特定使用者相關的資訊。 | 不適用 |                                                                                                                                                         |                                                                                   |
|                 | hba | 布林值 | 識別使用者是否嘗試從他的住家位置起始資料流 | 不適用 | true， false | true或false |
| 位置 | 大陸 | 字串 | 傳送播放請求的deviceID所在的大陸 | 不適用 | 北美 | 有效的大陸名稱 |
|                 | 國家/地區 | 字串 | 傳送播放請求的deviceID來源國家/地區 | 不適用 | 美國 | 有效的國家/地區名稱 |
|                 | state | 字串 | 傳送播放請求的deviceID來源狀態 | 不適用 | CA | 有效的狀態名稱 |
|                 | 城市 | 字串 | 傳送播放請求的deviceID來源城市 | 不適用 | 庫比蒂諾 | 有效的城市名稱 |
|                 | 郵遞區號 | 數字 | 傳送播放請求的deviceID來源的Zip碼 | 不適用 | 95014 | 有效的郵遞區號 |
| 串流 | streamId | 字串 | 由CM服務產生，不受客戶控制。 定義了maxstreams型別的規則時隱含使用。 | 不適用 | 不適用 | 不適用 |
|                 | streamCDN | 字串 | 表示從中擷取資料流的CDN | 不適用 | 不適用 | 不適用 |

## 使用中繼資料屬性建立原則的範例 {#examples-metadata-attr}

標準中繼資料欄位可用於根據其欄位值定義伺服器端原則：

* 您可以將原則設定為僅套用至特定欄位值(例如，專用的iOS原則： `osType`為`iOS`)
* 您可以限制給定欄位的不同值數量。 部分範例如下：
   * 不多於X個相異裝置： `HAVING DISTINCT COUNT(deviceId) <= 2`
   * 不多於X個不同的壓縮碼： `HAVING DISTINCT COUNT(zipcode) <= 3`
* 您可以限制每個欄位值的作用中串流數目。 部分範例如下：
   * 單一裝置型別不超過X個使用中串流： `GROUP BY deviceType HAVING COUNT(streamId) <= 3`
   * 即時內容資料流的有效資料流不超過X個： `SELECT COUNT(streamId) AS streamCount WHERE contentType='live' HAVING streamCount <= 3`

請在Zendesk[&#128279;](mailto:tve-support@adobe.com)中建立票證，連絡並行監視小組，並指示您要實作哪些原則。

您可在下列中找到更多原則和整合Cookbook範例：

* [原則決定點](/help/concurrency-monitoring/cm-policy-decision-point.md)
* [API主控台 — Adobe並行監視](http://docs.adobeptime.io/cm-api-v2/)
