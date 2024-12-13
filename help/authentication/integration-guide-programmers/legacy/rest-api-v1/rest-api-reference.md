---
title: REST API參考
description: Rest api參考
exl-id: 67e4639e-db0b-4400-bb81-e214263e8395
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 2%

---

# （舊版） REST API參考 {#rest-api-reference}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 節流機制

Adobe Pass驗證REST API受[節流機制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)所控管。

## 回應格式 {#response-formats}


>[!NOTE]
>
> 這些服務中提供的API可以傳回XML或JSON格式的回應（適用於傳回回應的API）。 有3種不同的方式可在請求中指定回應格式：
>
>* 將HTTP Accept標頭設定為`application/xml`或`application/json`。
>* 在要求承載中，指定引數`format=xml`或`format=json`。
>* 呼叫副檔名為`.xml`或`.json`的Web服務端點。 例如，`/regcode.xml`或`/regcode.json`
>
>您可以指定上述任一種方法。 使用衝突的格式指定多個方法可能會導致錯誤或不適當的輸出。

## REST API端點 {#clientless-endpoints}

&lt;REGGIE_FQDN>：

* 生產 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 正在暫存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>：

* 生產 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 正在暫存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## 網站服務摘要 {#web_srvs_summary}

下表列出無使用者端方法的可用網路服務。 按一下Web服務端點以取得詳細資訊（範例要求與回應、輸入引數、HTTP方法等）


| Sr | Web服務端點 | 說明 | <!--[Diag.  </br>Ref](http://tve.helpdocsonline.com/api-reference-v2-test#illustration)-->。 | 託管位置 | 呼叫者 |
|-----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|-----------------------------------------------------------|-----------------------------|
| 1. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | 傳回隨機產生的註冊代碼和登入頁面URI | 2 | Adobe</br>登入代碼服務 | 智慧型裝置 |
| 2. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode/ </br> {registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | 傳回包含註冊代碼UUID、註冊代碼和雜湊裝置ID的註冊代碼記錄 | 8 | Adobe</br>登入代碼服務 | Adobe Pass 驗證 |
| 3. | [&lt;SP_FQDN>/api/v1/config/ </br> {requestorId}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | 傳回要求者的已設定MVPD清單 | 5 | Adobe</br>Adobe Pass </br>驗證</br>服務 | 登入</br>網頁</br>應用程式 |
| 4. | [&lt;SP_FQDN>/api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | 透過通知MVPD選取事件來起始AuthN程式。 在驗證資料庫上建立記錄，在從MVPD收到成功回應時進行調節（步驟13） | 7 | Adobe</br>Adobe Pass </br>驗證</br>服務 | 登入</br>網頁</br>應用程式 |
| 5. | SAML判斷提示取用者 | Adobe Pass驗證和MVPD之間的現有SAML工作流程 | 13 | Adobe Pass </br>驗證</br>服務 | Adobe Pass 驗證 |
| 6. | [&lt;SP_FQDN>/api/v1/checkauthn/ </br> {registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | 登入網頁應用程式可檢查嘗試的登入流程是否成功 |                                                                                             | Adobe Pass </br>驗證   </br>服務 | 登入   </br>網頁   </br>應用程式 |
| 7. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | 取得AuthN權杖相關的中繼資料 | 15 | Adobe Pass </br>驗證</br>服務 | 智慧型裝置 |
| 8. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode/ </br> {registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md) | 刪除登入程式碼記錄並釋出登入程式碼以供重複使用 | 16 | Adobe</br>登入代碼服務 | Adobe Pass 驗證 |
| 9. | [&lt;SP_FQDN>/api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | 取得授權回應。 | 17 | Adobe Pass </br>驗證</br>服務 | 智慧型裝置 |
| 10. | [&lt;SP_FQDN>/api/v1/checkauthn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) | 指出裝置是否具有未過期的AuthN權杖。 |                                                                                             | Adobe Pass </br>驗證</br>服務 | 智慧型裝置 |
| 11. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | 如果找到，則傳回AuthN權杖。 |                                                                                             | Adobe Pass </br>驗證</br>服務 | 智慧型裝置 |
| 12. | [&lt;SP_FQDN>/api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | 如果找到，則傳回AuthZ權杖。 |                                                                                             | Adobe Pass </br>驗證</br>服務 | 智慧型裝置 |
| 13. | [&lt;SP_FQDN>/api/v1/tokens/media](/help/authentication/integration-guide-programmer/rest-apis/rest-api-v1/apis/obtain-short-media-token.md | 如果找到，則傳回短媒體權杖 — 與/api/v1/mediatoken相同 |                                                                                             | Adobe Pass </br>驗證</br>服務 | 智慧型裝置 |
| 14. | [&lt;SP_FQDN>/api/v1/mediatoken](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | 取得短媒體Token |                                                                                             | Adobe Pass </br>驗證</br>服務 | 智慧型裝置 |
| 15. | [&lt;SP_FQDN>/api/v1/preauthorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) | 擷取預先授權的資源清單 |                                                                                             | Adobe Pass </br>驗證</br>服務 | 智慧型裝置 |
| 16. | [&lt;SP_FQDN>/api/v1/preauthorize/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | 擷取預先授權資源的清單 |                                                                                             | Adobe Pass </br>驗證</br>服務 | 登入網頁應用程式 |
| 17. | [&lt;SP_FQDN>/api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | 從儲存空間中移除AuthN和AuthZ權杖 |                                                                                             | Adobe Pass </br>驗證   </br>服務 | 智慧型裝置 |
| 18. | [&lt;SP_FQDN>/api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | 驗證流程完成後取得使用者中繼資料 | 不適用 | 不適用 | 智慧型裝置 |
| 19. | [&lt;SP_FQDN>/api/v1/authenticate/freepreview](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md) | 為Temp Pass或Promotivation Temp Pass建立驗證Token | 不適用 | Adobe Pass </br>驗證</br>服務 | 智慧型裝置 |


## REST API安全性 {#security}

必須使用HTTPS通訊協定呼叫所有Adobe Pass驗證REST API，以進行安全通訊。 此外，呼叫的大多數API應該包含已取得的存取權杖，如[擷取存取權杖](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) API檔案中所述。
