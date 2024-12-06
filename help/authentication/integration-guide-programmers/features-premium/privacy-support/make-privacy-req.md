---
title: 如何提出隱私權請求
description: 如何提出隱私權請求
exl-id: abb21306-98d6-4899-914a-bdfa85cbd204
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 0%

---

# 如何提出隱私權請求 {#howto-make-privacy-request}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 識別碼和名稱空間 {#identifier-namespace}

傳送存取或刪除隱私權請求時，客戶應用程式必須包含下列識別碼：

* **mvpdID** - MVPD的唯一識別碼。
* **userID** — 唯一識別程式設計師應用程式的使用者，但源自MVPD。 請參閱程式設計師概觀中的瞭解使用者ID 。
* **IMSOrgID** - Adobe Experience Cloud Identity Management服務組織ID，可在Adobe Experience Cloud中唯一識別客戶


請檢查下列範例：

```JSON
"userIDs": [{
     "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",  -----> "Dish" is the id of the MVPD
     "type":"unregistered",
     "value":"1234-5678-8765-4321" ----> "1234-5678-8765-4321" is the userId associated with the MVPD account
}]
```

>[!IMPORTANT]
>
>使用者必須經過驗證，才能產生Adobe Pass驗證的隱私權請求。 否則，程式設計師必須找到其他方法來擷取MVPD userID。

## 要求型別 {#req-type}

Adobe Pass驗證支援存取和刪除請求。

### 存取 {#access-req}

針對存取要求：

我們會提供JSON檔案，其中包含為該資料主體建立的驗證和授權要求總數摘要。
所有這些事件都是按照客戶篩選的。


**要求樣本**

您必須上傳含有Adobe Pass驗證識別碼的JSON，才能提交資料存取請求。 若要檢視格式正確的JSON是什麼樣子，請參閱此範例：

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["access"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**回應範例**

```JSON
{
    "jobId": "d9a6b417-f619-4420-82a3-09f61fa8eff3",
    "requestId": "15765127177927284RX-739",
    "userKey": "John Dow",
    "action": "access",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/16/2019 04:11 PM GMT",
    "lastModifiedDate": "12/16/2019 04:15 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/16/2019 04:15 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-16T16:15:23.4Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "6",
                        "numberOfAuthorizationDecisions": "11"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/d9a6b417-f619-4420-82a3-09f61fa8eff3/d9a6b417-f619-4420-82a3-09f61fa8eff3.zip",
    "regulation": "ccpa"
}
```

### 刪除 {#delete-req}

您必須上傳含有Adobe Pass驗證識別碼的JSON，才能提交資料刪除請求。 若要檢視格式正確的JSON是什麼樣子，請參閱此範例：

**要求樣本**

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["delete"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**回應範例**

對於刪除請求：

* 我們只分享已移除資料的回條，而非包含所有已移除資料的彙總檔案。
* 回應中包含的回條包含該資料主體找到之驗證和授權權杖總數的摘要。

```JSON
{
    "jobId": "aab380d1-a0cd-4a0d-ba95-2649ee90c063",
    "requestId": "15759883098453100RX-074",
    "userKey": "John Dow",
    "action": "delete",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/10/2019 02:31 PM GMT",
    "lastModifiedDate": "12/10/2019 02:34 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/10/2019 02:34 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-10T14:34:55.274Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "2",
                        "numberOfAuthorizationDecisions": "3"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/aab380d1-a0cd-4a0d-ba95-2649ee90c063/aab380d1-a0cd-4a0d-ba95-2649ee90c063.zip",
    "regulation": "ccpa"
}
```

## 如何觸發請求 {#trigger-req}

客戶若要傳送隱私權請求給Adobe，有2個選項可以選擇：

* **手動** — 使用[Privacy Service使用者介面](#privacy-service-ui)
* **自動** — 使用[Privacy ServiceAPI](#privacy-service-api)

### 使用Privacy ServiceUI {#privacy-service-ui}

已透過Adobe I/O服務線上提供如何存取及使用Privacy Service使用者介面的[完整教學課程](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)。 此外，客戶可以使用此連結來存取隱私權法規的影片和文章庫。 按一下Adobe Experience Cloud和GDPR功能表。 以下影片將說明其使用方式：「GDPR UI使用說明」。

在UI中，客戶需要載入自己的IMSOrgID以及包含每個產品之GDPR要求詳細資料的JSON。

### 藉由使用Privacy Service API {#privacy-service-api}

Adobe Experience Platform Privacy Service可針對私人資料的存取/刪除請求和選擇退出銷售請求，提供通用、集中的簡化操作。

**Privacy Service API檔案**&#x200B;深入涵蓋Adobe客戶如何與AdobeAPI整合。

**使用Postman （免費的協力廠商軟體）視覺化API呼叫：**

* 在GitHub上[Privacy ServiceAPI Postman集合](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Privacy%20Service%20API.postman_collection.json)
* 建立Postman環境的[影片指南](https://video.tv.adobe.com/v/28832)
* [在Postman中匯入環境和集合的步驟](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)


**API路徑：**

* 平台閘道URL： `https://platform.adobe.io/`
* 此API的基礎路徑： `/data/core/privacy/jobs`
* 完整路徑的範例： `https://platform.adobe.io/data/core/privacy/jobs/ping`


**必要的標頭：**

* 所有呼叫都需要標題`Authorization`、`x-gw-ims-org-id`和`x-api-key`。 如需如何取得這些值的詳細資訊，請參閱&#x200B;**驗證教學課程**。
* 要求內文中具有裝載的所有要求(例如POST、PUT和PATCH呼叫)都必須包含值為`application/json`的標頭`Content-Type`。

<!--

>[!RELATEDINFORMATION]
>
>* [Privacy Services Overview](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)
>* Privacy Service API documentation

-->
