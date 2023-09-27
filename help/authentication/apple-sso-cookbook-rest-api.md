---
title: Apple SSO逐步指南(REST API)
description: Apple SSO逐步指南(REST API)
exl-id: cb27c4b7-bdb4-44a3-8f84-c522a953426f
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1435'
ht-degree: 0%

---

# Apple SSO逐步指南(REST API) {#apple-sso-cookbook-rest-api}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 簡介 {#Introduction}

Adobe Pass Authentication REST API可透過我們所說的Apple SSO工作流程，支援在iOS、iPadOS或tvOS上執行之使用者端應用程式的一般使用者平台單一登入(SSO)驗證。

請注意，本檔案可用作現有REST API檔案的擴充功能，您可在其中找到 [此處](/help/authentication/rest-api-reference.md).

## 逐步指南 {#Cookbooks}

若要從Apple SSO使用者體驗中獲益，一個應用程式必須整合 [視訊訂閱者帳戶](https://developer.apple.com/documentation/videosubscriberaccount) 由Apple開發的框架，雖然關於Adobe Pass驗證REST API通訊，但必須遵循下面提供的提示順序。

### 驗證 {#Authentication}

- [是否有有效的Adobe驗證Token？](#Is_there_a_valid_Adobe_authentication_token)
- [使用者是否透過Platform SSO登入？](#Is_the_user_logged_in_via_Platform_SSO)
- [擷取Adobe設定](#Fetch_Adobe_configuration)
- [使用Adobe設定起始平台SSO工作流程](#Initiate_Platform_SSO_workflow_with_Adobe_config)
- [使用者登入是否成功？](#Is_user_login_successful)
- [從Adobe取得所選MVPD的設定檔要求](#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD)
- [將Adobe請求轉寄給Platform SSO以取得設定檔](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile)
- [交換Platform SSO設定檔以取得Adobe驗證權杖](#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token)
- [Adobe權杖是否已成功產生？](#Is_Adobe_token_generated_successfully)
- [啟動第二個熒幕驗證工作流程](#Initiate_second_screen_authentication_workflow)
- [繼續授權流程](#Proceed_with_authorization_flows)


![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/qu/platform-sso.jpeg)


#### 步驟：「是否有有效的Adobe驗證Token？」 {#Is_there_a_valid_Adobe_authentication_token}

>[!TIP]
>
> **<u>秘訣：</u>** 透過以下媒體實施此作業： [Adobe Pass驗證](/help/authentication/check-authentication-token.md) 服務。


#### 步驟：「使用者是否透過Platform SSO登入？」 {#Is_the_user_logged_in_via_Platform_SSO}

>[!TIP]
>
> **<u>秘訣：</u>** 透過以下媒體實施此作業： [視訊訂閱者帳戶](https://developer.apple.com/documentation/videosubscriberaccount) 框架。

- 應用程式必須檢查 [存取許可權](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) 使用者的訂閱資訊，並僅在使用者允許時繼續。
- 應用程式必須提交 [請求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) 以取得訂閱者帳戶資訊。
- 應用程式必須等候並處理 [中繼資料](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 資訊。



>[!TIP]
>
> **<u>專業秘訣：</u>** 請依照程式碼片段操作，並特別留意相關註解。

```swift
...
let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();

videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
            switch (accessStatus) {
            // The user allows the application to access subscription information.
            case VSAccountAccessStatus.granted:
                    // Construct the request for subscriber account information.
                    let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();

                    // This is actually the SAML Issuer not the channel ID.
                    vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAccountProviderIdentifier = true;
                    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                    
                    // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
                    vsaMetadataRequest.isInterruptionAllowed = false;
                    
                    // Submit the request for subscriber account information - accountProviderIdentifier.
                    videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in        
                        if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                            // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                            // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                            ...
                            // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                            // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                            ...
                            // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                            // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                            ...
                            // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                            ...
                            // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Fallback to regular authentication workflow.
                ...
            }
}
...  
```

#### 步驟：「擷取Adobe設定」 {#Fetch_Adobe_configuration}

>[!TIP]
>
> **<u>秘訣：</u>** 透過以下媒體實施此作業： [Adobe Pass驗證](/help/authentication/provide-mvpd-list.md) 服務。


>[!TIP]
>
> **<u>專業秘訣：</u>** 請留意MVPD屬性： *`enablePlatformServices`*， *`boardingStatus`*， *`displayInPlatformPicker`*， *`platformMappingId`*， *`requiredMetadataFields`* 另外請格外留意其他步驟程式碼片段中的註解。

#### 步驟「使用Adobe設定啟動平台SSO工作流程」 {#Initiate_Platform_SSO_workflow_with_Adobe_config}

>[!TIP]
>
> **<u>秘訣：</u>** 透過以下媒體實施此作業： [視訊訂閱者帳戶](https://developer.apple.com/documentation/videosubscriberaccount) 框架。

- 應用程式必須檢查 [存取許可權](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) 使用者的訂閱資訊，並僅在使用者允許時繼續。
- 應用程式必須提供 [委派](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) 用於VSAccountManager。
- 應用程式必須提交 [請求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) 以取得訂閱者帳戶資訊。
- 應用程式必須等候並處理 [中繼資料](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 資訊。



>[!TIP]
>
> **<u>專業秘訣：</u>** 請依照程式碼片段操作，並特別留意相關註解。


```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    // This must be a class implementing the VSAccountManagerDelegate protocol.
    let videoSubscriberAccountManagerDelegate: VideoSubscriberAccountManagerDelegate = VideoSubscriberAccountManagerDelegate();
    
    videoSubscriberAccountManager.delegate = videoSubscriberAccountManagerDelegate;
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account framework to prompt the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = true;
                        
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to filter the TV providers from the Apple picker.
                        vsaMetadataRequest.supportedAccountProviderIdentifiers = supportedAccountProviderIdentifiers;
    
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to sort the TV providers from the Apple picker.
                        if #available(iOS 11.0, tvOS 11, *) {
                            vsaMetadataRequest.featuredAccountProviderIdentifiers = featuredAccountProviderIdentifiers;
                        }
                        
                        // Submit the request for subscriber account information - accountProviderIdentifier.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in                        
                            // This represents the checks for the "Is user login successful?" step.
                            if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                                // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                                // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                                ...
                                // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                                // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                                ...
                                // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                                // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                                ...
                                // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                                ...
                                // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
                                ...
                            } else {
                                // The user is not authenticated at platform level.
                                if (vsaError != nil) {
                                    // The application can check to see if the user selected a provider which is present in Apple picker, but the provider is not onboarded in platform SSO.
                                    if let error: NSError = (vsaError! as NSError), error.code == 1, let appleMsoId = error.userInfo["VSErrorInfoKeyUnsupportedProviderIdentifier"] as! String? {
                                        var mvpd: Mvpd? = nil;
    
                                        // The requestor.mvpds must be computed during the "Fetch Adobe configuration" step. 
                                        for provider in requestor.mvpds {
                                            if provider.platformMappingId == appleMsoId {
                                                mvpd = provider;
                                                break;
                                            }
                                        }
                                        
                                        if mvpd != nil {
                                            // Continue with the "Initiate second screen authentcation workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate second screen authentcation workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate second screen authentcation workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate second screen authentcation workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### 步驟：「使用者登入是否成功？」 {#Is_user_login_successful}

>[!TIP]
>
> **<u>專業秘訣：</u>** 請留意中的程式碼片段 [「使用Adobe設定起始平台SSO工作流程」](#Initiate_Platform_SSO_workflow_with_Adobe_config) 步驟。 萬一發生下列情況，使用者登入成功： *`vsaMetadata!.accountProviderIdentifier`* 包含有效值，且目前日期尚未超過 *`vsaMetadata!.authenticationExpirationDate`* 值。

#### 步驟「從Adobe取得所選MVPD的設定檔要求」 {#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD}

>[!TIP]
>
> **<u>秘訣：</u>** 透過Adobe Pass驗證媒體實作此專案 [設定檔請求](/help/authentication/retrieve-profilerequest.md) 服務。

>[!TIP]
>
> **<u>專業秘訣：</u>** 請注意，從視訊訂閱者帳戶架構取得的提供者識別碼代表 *`platformMappingId`* 就Adobe Pass驗證設定而言。 因此，應用程式必須使用 *`platformMappingId`* 值，透過Adobe Pass驗證 [提供MVPD清單](/help/authentication/provide-mvpd-list.md) 服務。

#### 步驟：「將Adobe請求轉寄給Platform SSO以取得設定檔」 {#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile}

>[!TIP]
>
> **<u>秘訣：</u>** 透過以下媒體實施此作業： [視訊訂閱者帳戶](https://developer.apple.com/documentation/videosubscriberaccount) 框架。


- 應用程式必須檢查 [存取許可權](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) 使用者的訂閱資訊，並僅在使用者允許時繼續。
- 應用程式必須提交 [請求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) 以取得訂閱者帳戶資訊。
- 應用程式必須等候並處理 [中繼資料](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 資訊。



>[!TIP]
>
> **<u>專業秘訣：</u>** 請依照程式碼片段操作，並特別留意相關註解。

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = false;
    
                        // This are the user metadata fields expected to be available on a successful login and are determined from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service. Look for the requiredMetadataFields associated with the provider determined in a previous step.
                        vsaMetadataRequest.attributeNames = requiredMetadataFields;
    
                        // This is the payload from [Adobe Pass Authentication](/help/authentication/retrieve-profilerequest.md) service.
                        vsaMetadataRequest.verificationToken = profileRequestPayload;
                        
                        // Submit the request for subscriber account information.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in
                            if (vsaMetadata != nil && vsaMetadata!.samlAttributeQueryResponse != nil) {
                                var samlResponse: String? = vsaMetadata!.samlAttributeQueryResponse!;
                                
                                // Remove new lines, new tabs and spaces.
                                samlResponse = samlResponse?.replacingOccurrences(of: "[ \\t]+", with: " ", options: String.CompareOptions.regularExpression);
                                samlResponse = samlResponse?.components(separatedBy: CharacterSet.newlines).joined(separator: "");
                                samlResponse = samlResponse?.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines);
                                
                                // Base64 encode.
                                samlResponse = samlResponse?.data(using: .utf8)?.base64EncodedString(options: []);
                                
                                // URL encode. Please be aware not to double URL encode it further.
                                samlResponse = samlResponse?.addingPercentEncoding(withAllowedCharacters: CharacterSet.init(charactersIn: "!*'();:@&=+$,/?%#[]").inverted);
                                
                                // Continue with the "Exchange the Platform SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Fallback to regular authentication workflow.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### 步驟：「將Platform SSO設定檔交換為Adobe驗證Token」 {#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token}

>[!TIP]
>
> **<u>秘訣：</u>** 透過Adobe Pass驗證媒體實作此專案 [Token Exchange](/help/authentication/token-exchange.md) 服務。


>[!TIP]
>
> **<u>專業秘訣：</u>** 請留意中的程式碼片段 [「將Adobe請求轉寄給Platform SSO以取得設定檔」](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile) 步驟。 這個 *`vsaMetadata!.samlAttributeQueryResponse!`* 代表 *`SAMLResponse`*，需要傳遞 [Token Exchange](/help/authentication/token-exchange.md) 而且需要字串操控和編碼(*Base64* 編碼和 *URL* 之後編碼)。

</br>

#### 步驟：「是否成功產生Adobe代號？」 {#Is_Adobe_token_generated_successfully}

>[!TIP]
>
> **<u>秘訣：</u>** 透過媒體Adobe Pass驗證實作此專案 [Token Exchange](/help/authentication/token-exchange.md) 成功的回應，將是 *`204 No Content`*，表示已成功建立權杖，並已準備好用於授權流程。

</br>

#### 步驟：「啟動第二個畫面驗證工作流程」 {#Initiate_second_screen_authentication_workflow}

**重要：** 「第二熒幕驗證工作流程」術語適用於AppleTV，而「第一熒幕驗證工作流程」/「一般驗證工作流程」術語則更適用於iPhone和iPad。


>[!TIP]
>
> **<u>秘訣：</u>** 透過Adobe Pass驗證媒體實作此專案

[註冊代碼要求](/help/authentication/registration-code-request.md)， [啟動驗證](/help/authentication/initiate-authentication.md) 和 [REST API擷取驗證Token](/help/authentication/retrieve-authentication-token.md) 或 [檢查驗證Token](/help/authentication/check-authentication-token.md) 服務。


>[!TIP]
>
> **<u>專業秘訣：</u>** 請依照下列步驟進行tvOS實作。

- 應用程式必須 [取得註冊代碼](/help/authentication/registration-code-request.md) 並在第一部裝置（熒幕）上向一般使用者展示。
- 應用程式必須啟動 [輪詢以確認驗證狀態](/help/authentication/retrieve-authentication-token.md) 取得註冊代碼後，在第一個裝置（熒幕）上執行。
- 另一個應用程式必須 [啟動驗證](/help/authentication/initiate-authentication.md) 註冊碼時，於第2部裝置（熒幕）上傳送。
- 應用程式必須停止 [輪詢](/help/authentication/retrieve-authentication-token.md) 驗證權杖產生時於第一個裝置（畫面）上。



>[!TIP]
>
> **<u>專業秘訣：</u>** 請依照下列步驟實作iOS/iPadOS。

- 應用程式必須 [取得註冊代碼](/help/authentication/registration-code-request.md) 不應在第一部裝置（熒幕）上呈現給一般使用者。
- 應用程式必須 [啟動驗證](/help/authentication/initiate-authentication.md) 在第一部裝置（熒幕），使用註冊碼和 [Wkwebview](https://developer.apple.com/documentation/webkit/wkwebview) 或 [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) 元件。
- 應用程式必須啟動 [輪詢以瞭解驗證狀態](/help/authentication/retrieve-authentication-token.md) 在之後的第一個裝置（畫面） [Wkwebview](https://developer.apple.com/documentation/webkit/wkwebview) 或 [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) 元件關閉。
- 應用程式必須停止 [輪詢](/help/authentication/retrieve-authentication-token.md) 驗證權杖產生時於第一個裝置（畫面）上。

</br>

#### 步驟：「繼續授權流程」 {#Proceed_with_authorization_flows}

>[!TIP]
>
> **<u>秘訣：</u>** 透過Adobe Pass驗證媒體實作此專案 [啟動授權](/help/authentication/initiate-authorization.md) 和 [取得短媒體Token](/help/authentication/obtain-short-media-token.md) 服務。

</br>

### 登出 {#Logout}

此 [視訊訂閱者帳戶](https://developer.apple.com/documentation/videosubscriberaccount) 架構未提供API以程式設計方式將已在裝置系統層級登入其電視提供者帳戶的人登出。 因此，登出若要完全生效，使用者必須明確從登出 *`Settings -> TV Provider`* 在iOS/iPadOS上或 *`Settings -> Accounts -> TV Provider`* 在tvOS上。 使用者可以選擇從特定應用程式設定區段（電視提供者存取）撤銷存取使用者訂閱資訊的許可權。

>[!TIP]
>
> **<u>秘訣：</u>** 透過Adobe Pass驗證媒體實作此專案 [使用者中繼資料呼叫](/help/authentication/user-metadata.md) 和 [登出](/help/authentication/initiate-logout.md) 服務。


>[!TIP]
>
> **<u>專業秘訣：</u>** 請依照下列步驟進行tvOS實作。


- 應用程式必須使用&#39;&#39;判斷是否由於透過平台SSO登入而發生驗證&#x200B;*tokenSource&quot;* [使用者中繼資料](/help/authentication/user-metadata.md) 來自Adobe Pass驗證服務。
- 應用程式必須指示/提示使用者明確從登出 *`Settings -> Accounts -> TV Provider`* 在tvOS上 **僅限** 萬一 *&quot;tokenSource&quot;* 值等於&quot;*Apple」。*
- 應用程式必須 [啟動登出](/help/authentication/initiate-logout.md) 使用直接HTTP呼叫從Adobe Pass驗證服務取得。 這不會促進MVPD端的工作階段清理。



>[!TIP]
>
> **<u>專業秘訣：</u>** 請依照下列步驟實作iOS/iPadOS。

- 應用程式必須使用&#39;&#39;判斷是否由於透過平台SSO登入而發生驗證&#x200B;*tokenSource&quot;* [使用者中繼資料](/help/authentication/user-metadata.md) 來自Adobe Pass驗證服務。
- 應用程式必須指示/提示使用者明確從登出 *`Settings -> TV Provider`* 在iOS/iPadOS上 **僅限** 萬一 *&quot;tokenSource&quot;* 值等於 *&quot;Apple&quot;*.
- 應用程式必須 [啟動登出](/help/authentication/initiate-logout.md) 從Adobe Pass Authentication服務，使用 [Wkwebview](https://developer.apple.com/documentation/webkit/wkwebview) 或 [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) 元件。 這有助於MVPD端的工作階段清理。

<!--

## Resources

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [REST API Overview](/help/authentication/rest-api-overview.md)
- [REST API Cookbook (Server-to-Server)](/help/authentication/rest-api-cookbook-servertoserver.md)
- [REST API Cookbook (Client-to-Server)](/help/authentication/rest-api-cookbook-clienttoserver.md)
- [REST API Reference](/help/authentication/rest-api-reference.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
