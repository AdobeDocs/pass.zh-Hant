---
title: TempPass功能
description: TempPass功能
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '2203'
ht-degree: 0%

---

# TempPass功能 {#temp-pass-feature}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

TempPass是多功能的功能，可讓程式設計師為沒有有效MVPD帳戶憑證的使用者提供對其受保護內容的暫時存取權。 不論是透過基本存取情境或目標式促銷活動，它都是吸引觀眾的有效工具。

TempPass是程式設計師的強大解決方案，可以：

* **與檢視者互動：**&#x200B;提供優質內容的味道，以吸引新的訂閱者。
* **推動促銷活動：**&#x200B;執行目標式行銷活動，以增加內容曝光率，並建立品牌忠誠度。
* **保留控制權：**&#x200B;管理存取期間、強制限制，以及視需要重設存取權，以符合業務目標。

TempPass功能的提供方式是在Adobe Pass驗證伺服器設定中匯入偽MVPD （進一步命名為「Temp Pass」），作為與參與程式設計人員的整合。 TempPass功能提供兩種設定：

* [基本TempPass](#basic-temp-pass)以時間為基礎的存取。
* [促銷活動暫時傳遞](#promotional-temp-pass)，用於彈性促銷活動導向的存取。

>[!IMPORTANT]
>
> TempPass功能是進階功能，目前需要Adobe的授權。

下表提供基本與促銷暫時傳遞功能的簡短比較：

| **功能** | **基本TempPass** | **促銷臨時通票** |
|-------------------------------|------------------------------|-------------------------------------------------------------------------------------------------|
| **存取內容** | <ul><li>基於時間</li></ul> | <ul><li>基於時間</li><li>資源數量上限</li></ul> |
| **存取安全性依據** | <ul><li>裝置ID</li></ul> | <ul><li>裝置ID</li><li>提供的使用者識別碼資訊的雜湊（例如電子郵件）</li></ul> |
| **增強錯誤碼** | 可用 | 可用 |
| **TempPass重設功能** | 可用 | 可用 |

>[!IMPORTANT]
> 
> Adobe Pass驗證不包括內建機制，可在分配時間（X分鐘）過後自動停止進行中的資料流。 程式設計師有責任在TempPass在持續串流期間過期後強制實施存取限制。

無論您是要提供內容庫的窺視或促銷圈選事件，TempPass都能提供工具，讓您擴充您的對象，同時保持對存取權的控制。

## 基本暫時傳遞 {#basic-temp-pass}

基本的TempPass功能可讓程式設計師針對各種情境提供有時間限制的內容存取：

* **簡短預覽：**&#x200B;提供簡短預覽，例如10分鐘的每日存取期，以吸引潛在訂閱者。
* **事件型存取：**&#x200B;啟用主要事件的較長存取權，例如4小時工作階段。
* **組合存取：**&#x200B;混合和比對持續時間，例如初始的延長檢視期間，之後是幾天內的每日較短預覽。

某些事件可能需要分階段免費存取內容，例如初始的延長免費存取時段（例如，4小時），接著較短的每日免費存取間隔（例如，每天10分鐘）。 為了實作此情境，程式設計師必須和他們的Adobe代表協調，根據他們的需求量身打造兩個TempPass MVPD。

例如，若要提供起始的4小時免費工作階段以及隨後的10分鐘每日免費工作階段，Adobe可以為程式設計師設定：

* **TempPass1**：已設定4小時的存留時間(TTL)，以涵蓋初始的免費存取期間。
* **TempPass2**：設定為後續每日自由存取間隔的10分鐘存留時間(TTL)。

為確保日常存取的正常運作，所有裝置的TempPass2必須在每天00:00小時重設。

### 功能詳細資料 {#basic-temp-pass-feature-details}

**組態引數：**

* **TTL （存留時間）：**&#x200B;程式設計師可以指定存取期間。 無論實際檢視時間為何，這個以時鐘為基礎的TTL都會過期。

**使用者識別：**

基本的TempPass功能使用裝置識別碼作為使用者識別引數。

下表可協助您瞭解使用者識別引數如何影響使用者試用體驗：

| 裝置識別碼 | 結果 |
|-------------------|----------------|
| 新增 | 新試用版 |
| 現有 | 現有試用版 |

**檢視時間計算：**

TTL代表從初始授權請求時間到到期時間的持續時間，與檢視內容所花費的實際時間無關。 每個未來的請求都會根據儲存的到期時間檢查目前的伺服器時間，以授權存取。

**驗證：**

Basic TempPass不需要驗證，允許您直接進入授權步驟。

**授權：**

由於無法與實際MVPD互動，因此，鑑於TempPass有效，基本「Temp Pass」MVPD將授權任何資源。 如果授權成功，媒體權杖驗證器程式庫仍適用於驗證媒體權杖，以及在起始內容播放之前確保資源驗證。

授權決定是根據使用者識別引數和設定的TTL。 若要成功取得資源的授權，有效的請求必須符合下列條件：

* **未使用的持續時間：**&#x200B;到期時間是透過將初始授權要求時間（儲存在我們的資料庫中）新增至設定的TTL來計算的。 系統會比較目前的伺服器時間與此到期時間，以判斷TempPass是否仍然有效。

如果使用者超過設定的TTL，除非重設其TempPass，否則他們將無法再在相同的裝置上檢視內容。

**預先授權：**

當對基本「暫時通過」MVPD發出預先授權請求時，回應將傳回請求中的完整資源清單，因為已成功預先授權。 鑑於授權條件是根據時間限制，而不是特定資源，此行為會反映授權邏輯。 只要時間限制有效，請求的資源就會獲得授權。

**登出：**

Basic TempPass不需要登出，允許您使用實際使用者MVPD直接切換到驗證步驟。

**追蹤資料和分析：**

在基本TempPass流程中，追蹤資料會使用雜湊版本的裝置識別碼，並將MVPD識別碼設為「Temp Pass」。 程式設計師應在其Analytics實作中將TempPass量度與標準MVPD量度區分開來。

## 促銷暫時傳遞 {#promotional-temp-pass}

促銷TempPass功能擴充了基本TempPass的功能，專為執行促銷活動而設計。 此功能可讓程式設計師在收集有效的使用者身分識別（例如電子郵件地址）後，在指定期間記憶體取預先定義數量的VOD標題，與使用者互動。

促銷TempPass包含基本TempPass的所有功能，並增加下列專案的彈性：

* 定義促銷期間可存取的VOD標題數量上限。
* 設定促銷存取的有效期間。

一旦使用者超過預先定義的存取限制(VOD標題數量或持續時間)，除非重設其TempPass，否則他們將無法再使用相同的使用者識別碼或相同的裝置上檢視內容。

### 功能詳細資料 {#promotional-temp-pass-feature-details}

**組態引數：**

* **使用者資訊金鑰：**&#x200B;用來傳達使用者提供的識別碼的金鑰，例如電子郵件地址（亦即，金鑰是電子郵件）。
* **資源數目：**&#x200B;定義使用者可以存取的VOD標題數目。
* **TTL （存留時間）：**&#x200B;使用者可以使用允許資源的期間。

**使用者識別：**

促銷暫時傳遞功能會使用使用者提供的識別碼雜湊，在裝置識別碼上方，做為使用者識別碼引數。

>[!IMPORTANT]
>
> 使用者提供之識別碼的驗證和雜湊是由程式設計師而非Adobe所管理。 Adobe不會儲存任何個人識別資訊(PII)。 因此，程式設計師在與Adobe Pass驗證API互動時，負責產生並提交使用者唯一識別碼的雜湊。

Adobe建議在傳送至Adobe之前，先對資料使用&#x200B;**SHA-2**&#x200B;系列或其特定的&#x200B;**SHA-256**、**SHA-512**&#x200B;功能。 例如，超過&#x200B;**&quot;user@domain.com&quot;**&#x200B;的&#x200B;**SHA-256**&#x200B;是&#x200B;**&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&quot;**。

下表可協助您瞭解使用者識別引數如何影響使用者試用體驗：

| 使用者提供的識別碼雜湊 | 裝置識別碼 | 結果 |
|-------------------------------|-------------------|---------------------------------------------------------|
| 新增 | 新增 | 新試用版 |
| 現有 | 新增 | 現有試用版（根據使用者提供的識別碼雜湊） |
| 新增 | 現有 | 現有試用版（根據裝置識別碼） |
| 現有 | 現有 | 現有試用版 |

**檢視時間計算：**

TTL代表從初始授權請求時間到到期時間的持續時間，與檢視內容所花費的實際時間無關。 每個未來的請求都會根據儲存的到期時間檢查目前的伺服器時間，以授權存取。

**驗證：**

Promotional TempPass不需要驗證，您可以直接進入授權步驟。

為了支援程式設計師應用程式的實作，Promotional TempPass會公開下列使用者中繼資料資訊，可透過對應的索引鍵存取：

* **`remaining_resources`**：指出使用者仍有權使用的資源數目。
* **`used_assets`**：提供使用者已使用的資源清單。
* **`expiration_date`**：顯示使用者促銷臨時傳遞的到期日。

**授權：**

由於無法與實際MVPD互動，因此促銷「暫時通過」MVPD將授權任何資源，前提是TempPass有效。 如果授權成功，媒體權杖驗證器程式庫仍適用於驗證媒體權杖，以及在起始內容播放之前確保資源驗證。

授權決定是根據使用者識別引數，以及設定的資源數和TTL。 若要成功取得資源的授權，有效的請求必須符合下列條件：

* **未使用的持續時間：**&#x200B;到期時間是透過將初始授權要求時間（儲存在我們的資料庫中）新增至設定的TTL來計算的。 系統會比較目前的伺服器時間與此到期時間，以判斷TempPass是否仍然有效。
* **未使用的資源：**&#x200B;已追蹤使用的資源數目（儲存在我們的資料庫中）。 耗用的資源數會與設定的資源數比較，以判斷TempPass是否仍然有效。

如果使用者超過設定的TTL或資源數，除非重設其TempPass，否則他們將無法再於相同裝置或使用相同使用者提供的識別碼檢視內容。

**預先授權：**

當對促銷「暫時通過」MVPD提出預先授權請求時，回應會將請求的整個資源清單傳回為已成功預先授權。 此行為會反映授權邏輯，因為授權條件是根據時間限制和存取的資源總數，而不是特定資源。 只要時間限制有效且未超過資源限制，就會授權請求的資源。

**登出：**

Promotional TempPass不需要登出，因此您可以使用實際的使用者MVPD直接切換到驗證步驟。

**追蹤資料和分析：**

在促銷TempPass流程期間，追蹤資料會使用雜湊版本的裝置識別碼，並將MVPD識別碼設為「Temp Pass」。 程式設計師應在其Analytics實作中將TempPass量度與標準MVPD量度區分開來。

## 重設TempPass API存取 {#reset-tempass-api-access}

在存取Reset TempPass API之前，您必須完成動態使用者端註冊(DCR)程式中的必要步驟。 此強製程式可確保您擁有與Reset TempPass API互動所需的存取Token。

如需完整的指示，請參閱[Dynamic Client註冊概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)檔案。

## 重設TempPass API - DELETE /reset-tempass/v3/reset {#reset-tempass-v3-reset}

為了重設裝置或所有裝置的特定TempPass，Adobe Pass驗證為程式設計師提供同時適用於基本和促銷TempPass的API。

### 請求 {#reset-tempass-v3-reset-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">主機</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">路徑</td>
      <td>/reset-tempass/v3/reset</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">方法</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">查詢參數</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>在上線流程中與服務提供者相關聯的內部唯一識別碼。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>上線流程期間與TempPass相關聯的內部唯一識別碼。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">device_id</td>
      <td>
            此重設作業有效的裝置識別碼。
            <br/><br/>
            如果未提供值，則重設操作會套用至所有裝置。
      </td>
      <td>可選</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">標頭</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Authorization</td>
      <td><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">擷取存取Token</a>檔案中描述了持有人權杖承載的產生。</td>
      <td><i>必填</i></td>
   </tr>
</table>

### 回應 {#reset-tempass-v3-reset-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">程式碼</th>
      <th style="background-color: #EFF2F7;">文字</th>
      <th style="background-color: #EFF2F7;">說明</th>
   </tr>
   <tr>
      <td>204</td>
      <td>無內容</td>
      <td>
        重設成功。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>錯誤請求</td>
      <td>
        請求無效，使用者端需要修正請求，然後再試一次。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未獲授權</td>
      <td>
        存取權杖無效，使用者端需要取得新的存取權杖並重試。 如需詳細資訊，請參閱<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">動態使用者端註冊概觀</a>檔案。
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>已禁止</td>
      <td>
        存取權杖無效，使用者端需要取得新的使用者端憑證和新的存取權杖，然後再試一次。 如需詳細資訊，請參閱<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">動態使用者端註冊概觀</a>檔案。
      </td>
   </tr>
</table>

### 範例 {#reset-tempass-v3-reset-samples}

#### 重設特定裝置的TempPass {#reset-tempass-v3-reset-specific-device}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=ba23d141-d715-561c-94f4-e9e4c966b1eb"
```

#### 重設所有裝置的TempPass {#reset-tempass-v3-reset-all-devices}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=all"
```

## 重設TempPass API - DELETE /reset-tempass/v3/reset/generic {#reset-tempass-v3-reset-generic}

為了重設一般金鑰（使用者提供的識別碼雜湊）或所有金鑰的特定TempPass，Adobe Pass驗證為程式設計師提供可用於促銷TempPass的API。

### 請求 {#reset-tempass-v3-reset-generic-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">主機</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">路徑</td>
      <td>/reset-tempass/v3/reset/generic</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">方法</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">查詢參數</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>在上線流程中與服務提供者相關聯的內部唯一識別碼。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>上線流程期間與TempPass相關聯的內部唯一識別碼。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">key</td>
      <td>
            使用者提供的識別碼雜湊，此重設作業對其有效。
            <br/><br/>
            如果未提供值，則重設操作會套用至所有使用者。
      </td>
      <td>可選</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">標頭</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Authorization</td>
      <td><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">擷取存取Token</a>檔案中描述了持有人權杖承載的產生。</td>
      <td><i>必填</i></td>
   </tr>
</table>

### 回應 {#reset-tempass-v3-reset-generic-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">程式碼</th>
      <th style="background-color: #EFF2F7;">文字</th>
      <th style="background-color: #EFF2F7;">說明</th>
   </tr>
   <tr>
      <td>204</td>
      <td>無內容</td>
      <td>
        重設成功。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>錯誤請求</td>
      <td>
        請求無效，使用者端需要修正請求，然後再試一次。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未獲授權</td>
      <td>
        存取權杖無效，使用者端需要取得新的存取權杖並重試。 如需詳細資訊，請參閱<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">動態使用者端註冊概觀</a>檔案。
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>已禁止</td>
      <td>
        存取權杖無效，使用者端需要取得新的使用者端憑證和新的存取權杖，然後再試一次。 如需詳細資訊，請參閱<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">動態使用者端註冊概觀</a>檔案。
      </td>
   </tr>
</table>

### 範例 {#reset-tempass-v3-reset-generic-samples}

#### 重設特定金鑰的TempPass {#reset-tempass-v3-reset-specific-key}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7"
```

#### 重設所有金鑰的TempPass {#reset-tempass-v3-reset-all-keys}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=all"
```

## REST API V2 {#rest-api-v2}

運用TempPass功能需要實作程式碼更新，以修改您的隨處電視(TVE)應用程式與Adobe Pass驗證[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)互動的方式。

如需這些更新和相關工作流程的完整指南，請參閱[暫時存取流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)檔案。
