---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass 驗證
user-guide-description: Adobe Pass 驗證是 TV Everywhere 的權益解決方案，它提供模組化架構用來確定要求存取資源的人是否有權限存取該資源。
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '1262'
ht-degree: 2%

---


# Adobe Pass驗證說明 {#authentication}

+ [Adobe Pass 驗證](home.md)
+ [產品公告](product-announcements.md)
+ 產品發行{#product-releases}
   + 2024 {#2024}
      + [Adobe Pass Authentication 3.0.3發行說明](notes-releases/auth-rn-303.md)
      + [Adobe Pass Authentication 3.0發行說明](notes-releases/auth-rn-300.md)
      + [Adobe Pass Authentication 2.70發行說明](notes-releases/auth-rn-270.md)
      + [Adobe Pass Authentication 2.69發行說明](notes-releases/auth-rn-269.md)
      + [Adobe Pass Authentication JavaScript 4.7.0發行說明](notes-releases/authn-rn-javascript-470.md)
      + [Adobe Pass Authentication iOS / tvOS 3.9.2發行說明](notes-releases/authn-rn-ios-tvos-392.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.4發行說明](notes-releases/authn-rn-ios-tvos-384.md)
   + 2023 {#2023}
      + [Adobe Pass Authentication 2.68發行說明](notes-releases/auth-rn-268.md)
      + [Adobe Pass Authentication 2.67發行說明](notes-releases/auth-rn-267.md)
      + [Adobe Pass Authentication 2.66發行說明](notes-releases/auth-rn-266.md)
      + [Adobe Pass Authentication 2.65.1發行說明](notes-releases/auth-rn-2651.md)
      + [Adobe Pass Authentication 2.65發行說明](notes-releases/auth-rn-265.md)
      + [Adobe Pass Authentication 2.64.1發行說明](notes-releases/auth-rn-2641.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.3發行說明](notes-releases/authn-rn-ios-tvos-383.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.2發行說明](notes-releases/authn-rn-ios-tvos-382.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.1發行說明](notes-releases/authn-rn-ios-tvos-381.md)
      + [Adobe Pass Authentication Android 3.7.3發行說明](notes-releases/authn-rn-android-373.md)
   + 2022 {#2022}
      + [Adobe Pass Authentication 2.64發行說明](notes-releases/auth-rn-264.md)
      + [Adobe Pass Authentication 2.63發行說明](notes-releases/auth-rn-263.md)
      + [Adobe Pass Authentication 2.62.1發行說明](notes-releases/auth-rn-2621.md)
      + [Adobe Pass Authentication JavaScript 4.6.0發行說明](notes-releases/authn-rn-javascript-460.md)
   + 2021 {#2021}
      + [Adobe Pass Authentication JavaScript 4.4.0發行說明](notes-releases/authn-rn-javascript-440.md)
      + [Adobe Pass Authentication iOS / tvOS 3.7.0發行說明](notes-releases/authn-rn-ios-tvos-370.md)
+ Kickstart {#kickstart}
   + [技術檔案](kickstart/technical-paper.md)
   + [程式設計師概觀](kickstart/programmer-overview.md)
   + [MVPD概觀](kickstart/mvpd-overview.md)
   + [程式設計師快速入門手冊](kickstart/programmer-kickstart-guide.md)
   + [MVPD快速入門手冊](kickstart/mvpd-kickstart-guide.md)
   + [向上呈報程式](kickstart/escalation-procedures.md)
+ 程式設計師的整合指南{#integration-guide-programmers}
   + REST API {#rest-apis}
      + REST API DCR {#rest-api-dcr}
         + [Dynamic Client註冊概述](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
         + [動態使用者端註冊字彙表](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)
         + [Dynamic Client註冊常見問題集](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)
         + API {#rest-api-dcr-apis}
            + [擷取使用者端認證](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
            + [擷取存取權杖](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
         + 流程{#rest-api-dcr-flows}
            + [動態使用者端註冊流程](integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)
      + REST API V2 {#rest-api-v2}
         + [REST API V2概觀](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
         + [rest API V2字彙表](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)
         + [REST API V2常見問題集](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)
         + API {#rest-api-v2-apis}
            + [REST API V2 API概述](integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
            + 設定{#rest-api-v2-configuration-apis}
               + [擷取特定服務提供者的設定](integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
            + 工作階段{#rest-api-v2-sessions-apis}
               + [建立驗證工作階段](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
               + [繼續驗證工作階段](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
               + [擷取驗證工作階段](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
               + [在使用者代理程式中執行驗證](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
            + 設定檔{#rest-api-v2-profiles-apis}
               + [擷取設定檔](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
               + [擷取特定mvpd的設定檔](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
               + [擷取特定程式碼的設定檔](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
            + 決定{#rest-api-v2-decisions-apis}
               + [使用特定mvpd擷取授權決策](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
               + [使用特定mvpd擷取預先授權決定](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
            + 登出{#rest-api-v2-logout-apis}
               + [啟動特定mvpd的登出](integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
            + 合作夥伴單一登入{#rest-api-v2-partner-single-sign-on-apis}
               + [擷取合作夥伴驗證請求](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
               + [使用合作夥伴驗證回應擷取設定檔](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
         + 流程{#rest-api-v2-flows}
            + [REST API V2流程概述](integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
            + 基本存取流程{#rest-api-v2-basic-access-flows}
               + [主要應用程式內執行的基本設定檔流程](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
               + [在次要應用程式內執行的基本設定檔流程](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
               + [主要應用程式內執行的基本驗證流程](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
               + [在次要應用程式內執行的基本驗證流程](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
               + [主要應用程式內執行的基本授權流程](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
               + [主要應用程式內執行的基本預先授權流程](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
               + [主要應用程式內執行的基本登出流程](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
            + 降級存取流程{#rest-api-v2-degraded-access-flows}
               + [存取流程效能降低](integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
            + 暫時存取流程{#rest-api-v2-temporary-access-flows}
               + [暫時存取流程](integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
            + 單一登入存取流程{#rest-api-v2-single-sign-on-access-flows}
               + [使用合作夥伴流程的單一登入](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
               + [使用平台身分流程的單一登入](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
               + [使用服務權杖流程的單一登入](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
               + [單一登出流程](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
         + 逐步指南{#rest-api-v2-cookbooks}
            + [REST API V2逐步指南（使用者端對伺服器）](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
            + [REST API V2逐步指南（伺服器對伺服器）](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)
         + 附錄{#rest-api-v2-appendix}
            + 標頭{#rest-api-v2-appendix-headers}
               + [頁首 — 授權](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
               + [頁首 — AP-Device-Identifier](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
               + [頁首 — X-Device-Info](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
               + [標頭 — AD-Service-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
               + [標題 — Adobe-Subject-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
               + [頁首 — AP-Partner-Framework-Status](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
               + [頁首 — AP-TempPass-Identity](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + 標準功能{#standard-features}
      + 權利{#entitlements}
         + [識別受保護的資源](integration-guide-programmers/features-standard/entitlements/identify-protected-resources.md)
         + [預檢授權](integration-guide-programmers/features-standard/entitlements/preflight-authz.md)
         + [如何整合媒體權杖驗證器](integration-guide-programmers/features-standard/entitlements/media-token-verifier-int.md)
         + [使用者中繼資料](integration-guide-programmers/features-standard/entitlements/user-metadata.md)
      + 報告{#error-reporting}時發生錯誤
         + [增強的錯誤碼](integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)
      + 單一登入存取{#sso-access}
         + 合作夥伴單一登入{#partner-sso}
            + Apple單一登入{#apple-sso}
               + [Apple SSO概觀](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md)
               + [Apple SSO逐步指南(REST API V2)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)
         + 平台單一登入{#platform-sso}
            + Amazon單一登入{#amazon-sso}
               + [Amazon SSO逐步指南(REST API V2)](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
            + Roku單一登入{#roku-sso}
               + [Roku SSO概觀](integration-guide-programmers/features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-overview.md)
      + 以住家為基礎的驗證存取{#hba-access}
         + [適用於所有地方電視的家庭式驗證](integration-guide-programmers/features-standard/hba-access/home-based-authn-tve.md)
         + [MVPD的HBA狀態](integration-guide-programmers/features-standard/hba-access/hba-status-mvpds.md)
      + 隱私權支援{#privacy-support}
         + [隱私權支援概述](integration-guide-programmers/features-premium/privacy-support/privacy-supp-overview.md)
         + [如何提出隱私權請求](integration-guide-programmers/features-premium/privacy-support/make-privacy-req.md)
   + 進階功能{#features-premium}
      + 暫時存取{#temporary-access}
         + [暫時通過](integration-guide-programmers/features-premium/temporary-access/temp-pass.md)
         + [促銷臨時傳遞](integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md)
         + [重設暫時通過](integration-guide-programmers/features-premium/temporary-access/reset-temp-pass.md)
      + 存取許可權降級{#degraded-access}
         + [降級API概觀](integration-guide-programmers/features-premium/degraded-access/degradation-api-overview.md)
      + ESM {#esm}
         + [軟體權利檔案服務監視概觀](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)
         + [權益服務監視API](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)
         + [伺服器端量度](integration-guide-programmers/features-premium/esm/understanding-serverside-metrics.md)
      + 分析{#analytics}
         + [將Adobe Pass驗證伺服器端資料整合至Adobe Analytics](integration-guide-programmers/features-premium/analytics/integrate-authn-servr-data-analytics.md)
         + [在Adobe Pass驗證中使用Experience CloudID](integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md)
   + 舊版{#legacy}
      + （舊版） REST API V1 {#rest-api-v1}
         + [（舊版） REST API V1概覽](integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)
         + [（舊版） REST API V1參考](integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
         + （舊版） API {#rest-api-v1-apis}
            + [（舊版）註冊代碼要求](integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)
            + [（舊版）退貨註冊記錄](integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md)
            + [（舊版）刪除註冊記錄](integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md)
            + [（舊版）提供MVPD清單](integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)
            + [（舊版）起始驗證](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)
            + [（舊版）檢查驗證Token](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)
            + [（舊版）擷取驗證Token](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)
            + [（舊版）啟動授權](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md)
            + [（舊版）擷取授權Token](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md)
            + [（舊版）取得短媒體Token](integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md)
            + [（舊版）依第二熒幕Web應用程式檢查驗證流程](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md)
            + [（舊版）擷取預先授權資源清單](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md)
            + [（舊版）依第二熒幕Web應用程式擷取預先授權資源清單](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
            + [（舊版）啟動登出](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md)
            + [（舊版）使用者中繼資料](integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)
            + [（舊版）擷取設定檔請求](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md)
            + [（舊版） Token Exchange](integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md)
            + [（舊版）臨時通關和促銷臨時通關的免費預覽](integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md)
         + （舊版）逐步指南{#rest-api-v1-cookbooks}
            + [（舊版） REST API V1逐步指南（使用者端對伺服器）](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-clienttoserver.md)
            + [（舊版） REST API V1逐步指南（伺服器對伺服器）](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md)
      + （舊版） SDK {#sdks}
         + （舊版） JavaScript SDK {#javascript-sdk}
            + [（舊版） JavaScript SDK概觀](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-overview.md)
            + [（舊版） JavaScript SDK逐步指南](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-cookbook.md)
            + [（舊版） JavaScript SDK API參考](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
            + [（舊版） JavaScript SDK API預先授權](integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
            + （舊版）指南{#javascript-sdk-guidelines}
               + [（舊版）不需重新整理的登入和登出](integration-guide-programmers/legacy/sdks/javascript-sdk/refreshless-login-and-logout.md)
         + （舊版） iOS/tvOS SDK {#ios-tvos-sdk}
            + [（舊版） iOS/tvOS SDK概觀](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-overview.md)
            + [（舊版） iOS/tvOS SDK逐步指南](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md)
            + [（舊版） iOS/tvOS SDK API參考](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
            + [（舊版） iOS/tvOS SDK API預先授權](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
            + （舊版）指南{#ios-tvos-sdk-guidelines}
               + [（舊版） iOS/tvOS應用程式註冊](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)
               + [（舊版） iOS/tvOS v3.x移轉指南](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-v3x-migration-guide.md)
               + [（舊版） iOS/tvOS儲存完整性檢查](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-storage-integrity-checks.md)
         + （舊版） Android SDK {#android-sdk}
            + [（舊版） Android SDK概觀](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-overview.md)
            + [（舊版） Android SDK逐步指南](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-cookbook.md)
            + [（舊版） Android SDK API參考](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md)
            + [（舊版） Android SDK API預先授權](integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)
            + （舊版）指南{#android-sdk-guidelines}
               + [（舊版） Android應用程式註冊](integration-guide-programmers/legacy/sdks/android-sdk/android-application-registration.md)
               + [（舊版）透過動態使用者端註冊的Android SDK](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-with-dynamic-client-registration.md)
         + （舊版） FireOS SDK {#fireos-sdk}
            + [（舊版） Amazon FireOS技術概覽](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-technical-overview.md)
            + [（舊版） Amazon FireOS整合逐步指南](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-integration-cookbook.md)
            + [（舊版） Amazon FireOS API參考資料](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)
            + [（舊版） Amazon FireOS應用程式註冊](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-application-registration.md)
            + [（舊版）具有動態使用者端註冊的FireOS SDK](integration-guide-programmers/legacy/sdks/fireos-sdk/fireos-sdk-with-dynamic-client-registration.md)
            + [（舊版） Amazon FireOS SSO — 程式設計師啟動指南](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-firetv-sso-programmer-kickoff-guide.md)
      + （舊版）使用者端資訊{#client-information}
         + [（舊版）傳遞使用者端資訊（裝置、連線和應用程式）](integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)
      + （舊版）錯誤報告{#error-reporting}
         + [（舊版）錯誤報告](integration-guide-programmers/legacy/error-reporting/error-reporting.md)
      + （舊版）單一登入存取{#sso-access}
         + [（舊版）單一登入支援](integration-guide-programmers/legacy/sso-access/sso-support.md)
         + [（舊版）透過被動驗證的SSO](integration-guide-programmers/legacy/sso-access/sso-passive-authn.md)
         + [（舊版） Amazon SSO逐步指南(REST API V1)](integration-guide-programmers/legacy/sso-access/amazon-sso-cookbook-rest-api-v1.md)
         + [（舊版） Apple SSO逐步指南(REST API V1)](integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md)
         + [（舊版） Apple SSO逐步指南(iOS/tvOS SDK)](integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-iostvos-sdk.md)
      + （舊版） TVE儀表板{#tve-dashboard}
         + [（舊版） TVE控制面板使用手冊](integration-guide-programmers/legacy/tve-dashboard/tve-dashboard-user-guide.md)
      + （舊版）技術說明{#tech-notes}
         + （舊版） REST API V1 {#rest-api-v1}
            + [（舊版）無使用者端API實作 — 錯誤代碼/包含可能原因/原因的訊息](integration-guide-programmers/legacy/notes-technical/clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
            + [（舊版）缺少裝置ID時的無使用者端API流程](integration-guide-programmers/legacy/notes-technical/clientless-api-flow-in-the-absence-of-device-id.md)
            + [（舊版）無使用者端：避免在/authenticate請求中使用&#39;&amp;&#39;reg_code](integration-guide-programmers/legacy/notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)
            + [（舊版）啟用Xbox 360和XboxOne無使用者端程式設計師的Adobe Pass軟體權利檔案服務](integration-guide-programmers/legacy/notes-technical/enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
            + [（舊版）無使用者端裝置型別和量度](integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
         + （舊版） SDK {#sdks}
            + [（舊版）憑證常見問答](integration-guide-programmers/legacy/notes-technical/certificates-qa.md)
            + [（舊版）瞭解使用者ID](integration-guide-programmers/legacy/notes-technical/understanding-user-ids.md)
            + （舊版） JavaScript SDK {#javascript-sdk}
               + [（舊版）預防追蹤評估 — Apple Safari](integration-guide-programmers/legacy/notes-technical/tracking-prevention-assessment-apple-safari.md)
               + [（舊版）預防追蹤評估 — Google Chrome](integration-guide-programmers/legacy/notes-technical/tracking-prevention-assessment-google-chrome.md)
               + [（舊版） Cookie更新 — SameSite和Secure標幟](integration-guide-programmers/legacy/notes-technical/cookies-updates-samesite-and-secure-flags.md)
               + [（舊版）偵錯技巧](integration-guide-programmers/legacy/notes-technical/appendix-b-debugging-tips.md)
            + （舊版） Android SDK {#android-sdk}
               + [（舊版） Access Enabler Android 10應用程式上的Android SDK單一登入(SSO)](integration-guide-programmers/legacy/notes-technical/access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
               + [（舊版） Adobe Pass驗證和Android 6「Marshmallow」新許可權模型](integration-guide-programmers/legacy/notes-technical/adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
            + （舊版） iOS/tvOS SDK {#ios-tvos-sdk}
               + [（舊版） iOS SDK 3.1+上的WKWebView支援](integration-guide-programmers/legacy/notes-technical/wkwebview-support-on-ios-sdk-31.md)
               + [（舊版） iOS SDK 3.2+上的SFSafariViewController支援](integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md)
               + [（舊版） iOS在使用Adobe Pass Authentication Access Enabler時的SSO](integration-guide-programmers/legacy/notes-technical/sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
               + [（舊版） iOS驗證錯誤 — 找不到adobepass.ios.app](integration-guide-programmers/legacy/notes-technical/ios-authentication-error-adobepassiosapp-cannot-be-found.md)
               + [（舊版） iOS上的重設暫時傳遞](integration-guide-programmers/legacy/notes-technical/reset-temp-pass-on-ios.md)
               + [（舊版）使用主控台應用程式記錄檔對AccessEnabler iOS/tvOS SDK進行除錯](integration-guide-programmers/legacy/notes-technical/debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
               + [（舊版） AccessEnabler iOS/tvOS 3.7.0升級路徑](integration-guide-programmers/legacy/notes-technical/accessenabler-iostvos-370-upgrade-path.md)
         + （舊版）使用者體驗{#user-experience}
            + [（舊版）如何將MVPD登入頁面從iFrame移轉至快顯視窗](integration-guide-programmers/legacy/notes-technical/migr-mvpd-login-iframe-popup.md)
            + [（舊版）預檢功能：如何啟用、疑難排解或解決問題](integration-guide-programmers/legacy/notes-technical/preflight-feature.md)
            + [（舊版）在選取對話方塊中允許MVPD](integration-guide-programmers/legacy/notes-technical/allow-mvpd-selectn-dialog.md)
            + [（舊版）防止MVPD出現在選取範圍對話方塊](integration-guide-programmers/legacy/notes-technical/prevent-mvpd-selectn-dialog.md)
         + （舊版）疑難排解{#troubleshooting}
            + [（舊版）使用Charles Proxy](integration-guide-programmers/legacy/notes-technical/using-charles-proxy.md)
            + [（舊版）監控Adobe PassAdobePayTV Pass](integration-guide-programmers/legacy/notes-technical/monitoring-adobe-pay-tv-pass.md)
            + [（舊版）如何使用Adobe API測試網站測試驗證和授權流程](integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md)
   + [程式設計師整合指南概觀](integration-guide-programmers/programmer-integration-guide-overview.md)
   + [程式設計師權益流程](integration-guide-programmers/entitlement-flow.md)
   + [程式設計師使用案例](integration-guide-programmers/programmer-use-cases.md)
   + [節流機制](integration-guide-programmers/throttling-mechanism.md)
   + [最低系統需求](integration-guide-programmers/minimum-system-requirements.md)
+ MVPD {#integration-guide-mvpds}的整合指南
   + [整合功能](integration-guide-mvpds/mvpd-integr-features.md)
   + [驗證](integration-guide-mvpds/authn-usecase.md)
   + [使用OAuth 2.0通訊協定進行驗證](integration-guide-mvpds/authn-oauth2-protocol.md)
   + [Authorization](integration-guide-mvpds/authz-usecase.md)
   + [預檢授權](integration-guide-mvpds/mvpd-preflight-authz.md)
   + [MVPD登出](integration-guide-mvpds/usecase-mvpd-logout.md)
   + [內容中繼資料交換](integration-guide-mvpds/mvpd-content-metadata-exchange.md)
   + [使用者中繼資料交換](integration-guide-mvpds/mvpd-user-metadata-exchng.md)
   + [Proxy MVPD Web服務](integration-guide-mvpds/proxy-mvpd-webserv.md)
   + [Proxy MVPD SAML整合](integration-guide-mvpds/proxy-mvpd-saml-int.md)
   + [服務提供者範圍](integration-guide-mvpds/serv-provider-scoping.md)
   + [MVPD允許IP位址](integration-guide-mvpds/mvpd-listing-ip-addres.md)
+ TVE儀表板{#user-guide-tve-dashboard}的使用者指南
   + [TVE儀表板概觀](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
   + [環境](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md)
   + [檢閱和推送變更](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)
   + [儀表板](/help/authentication/user-guide-tve-dashboard/tve-dashboard-home.md)
   + [頻道](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md)
   + [程式設計師](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md)
   + [MVPDs](/help/authentication/user-guide-tve-dashboard/tve-dashboard-mvpds.md)
   + [整合](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md)
   + [報表](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md)
   + [變更記錄](/help/authentication/user-guide-tve-dashboard/tve-dashboard-changes-log.md)
+ 技術說明{#tech-notes}
   + 環境{#environments}
      + [瞭解Adobe環境](notes-technical/environments/understanding-the-adobe-environments.md)
      + [在預備中設定您的環境及測試](notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md)
